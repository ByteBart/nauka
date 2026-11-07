# AWS Systems Manager Automation — notatki do egzaminu SAP-C02

> **Zakres:** SSM Automation (runbooki, akcje, wyzwalacze, IAM, multi-account, approvals), a także różnice względem Run Command, Step Functions, Lambda, CloudFormation i Config remediation.
> **Stan wiedzy:** listopad 2026.
> **Najważniejsze:** Automation to **orkiestrator operacyjny oparty na runbookach** — wykonuje wieloetapowe zadania na zasobach AWS (i managed nodes), z logiką warunkową, oczekiwaniem na stan zasobu, zatwierdzeniami i kontrolą współbieżności. To nie jest „Run Command na sterydach” ani ogólny silnik workflow aplikacji.

---

## 0. Najkrótsza możliwa odpowiedź

- **AWS Systems Manager Automation** to usługa Systems Manager do **automatyzacji zadań operacyjnych** (maintenance, deployment, remediation) przy użyciu **runbooków**.
- **Runbook** to dokument SSM typu `Automation` (YAML/JSON, schema `0.3`), złożony z **parametrów** i **kroków (`mainSteps`)**. Każdy krok to jedna **akcja** (`aws:*`).
- Automation może:
  - wywoływać **dowolne API AWS** (`aws:executeAwsApi`),
  - uruchamiać skrypty **Python/PowerShell** (`aws:executeScript`),
  - wykonywać polecenia na instancjach przez SSM Agent (`aws:runCommand`),
  - czekać na stan zasobu (`aws:waitForAwsResourceProperty`) i warunkowo rozgałęziać (`aws:branch`),
  - wymagać **ręcznego zatwierdzenia** (`aws:approve`),
  - uruchamiać inne runbooki (`aws:executeAutomation`) i state machines (`aws:executeStateMachine`).
- Wyzwalacze: ręcznie (konsola/CLI/SDK/API), **EventBridge** (event lub harmonogram), **Maintenance Window**, **State Manager association**, **Config remediation**, **Incident Manager**, **Change Manager** (istniejący klienci).
- Uruchamiane jest z **IAM role** (`AutomationAssumeRole`); uprawnienia tej roli (oraz SCP) decydują, co runbook może zrobić.
- Limity: **100 równoczesnych automations na konto** (plus kolejka 5000), **25 rate-control automations** (kolejka 1000); kroki można ograniczać `MaxConcurrency`/`MaxErrors`.
- Multi-account/multi-region: role `AWS-SystemsManager-AutomationAdministrationRole` (centralne konto) i `AWS-SystemsManager-AutomationExecutionRole` (konta docelowe); runbook musi być udostępniony kontom docelowym.

**Zdanie-klucz na egzamin:** *Run Command wykonuje polecenie; Automation orkiestruje wieloetapowy proces operacyjny. Config wykrywa niezgodność i może wywołać runbook Automation, ale to Automation wykonuje remediację.*

---

## 1. Czym jest Automation — komponenty

| Pojęcie | Znaczenie |
|---|---|
| **Automation** | wykonanie wszystkich kroków zdefiniowanych w runbooku przez usługę Automation |
| **Runbook** | dokument SSM typu `Automation`; YAML/JSON; schema `0.3`; zawiera parametry i `mainSteps` |
| **Krok (step)** | pojedyncza akcja `aws:*` z wejściami, wyjściami, timeoutem, retry i obsługą błędów |
| **Akcja** | typ operacji (np. `aws:executeAwsApi`, `aws:runCommand`, `aws:approve`); Automation ma **20 typów akcji** |
| **Parametry** | wejścia runbooka, typowane (`String`, `StringList`, `Integer`, `Boolean`, `StringMap`), z ograniczeniami (`allowedValues`, `allowedPattern`) |
| **Wyjścia** | wyniki kroków, np. `{{ StepName.Output }}`; można ich używać w kolejnych krokach |
| **AutomationAssumeRole** | rola IAM, której używa Automation do wykonywania akcji; bez niej użyte są uprawnienia wywołującego |
| **Execution** | pojedyncze uruchomienie runbooka; ma status i historię kroków |

### Runbook a inne dokumenty SSM

| Typ dokumentu | Schema | Do czego |
|---|---|---|
| **Automation** | `0.3` | runbooki Automation (wieloetapowe) |
| **Command** | `1.2`, `2.0`, `2.2` | polecenia dla Run Command / State Manager |
| **Session** | — | ustawienia Session Manager |
| **Policy** | `2.0+` | polityki (np. dokumenty polityk) |

To częsta pułapka: **runbook Automation to nie Command document** — nie uruchomisz go jako „polecenia” w Run Command.

---

## 2. Struktura runbooka

Minimalny przykład (YAML):

```yaml
schemaVersion: '0.3'
description: Reboot an instance and wait until it is running
assumeRole: '{{ AutomationAssumeRole }}'
parameters:
  InstanceId:
    type: String
    description: EC2 instance to reboot
  AutomationAssumeRole:
    type: String
    default: ''
mainSteps:
  - name: rebootInstance
    action: aws:executeAwsApi
    inputs:
      Service: ec2
      Api: RebootInstances
      InstanceIds:
        - '{{ InstanceId }}'

  - name: waitForRunning
    action: aws:waitForAwsResourceProperty
    timeoutSeconds: 600
    inputs:
      Service: ec2
      Api: DescribeInstances
      InstanceIds:
        - '{{ InstanceId }}'
      PropertySelector: '$.Reservations[0].Instances[0].State.Name'
      DesiredValues:
        - running
```

Elementy wspólne wszystkich kroków:
- `name` — unikalna nazwa kroku,
- `action` — typ akcji,
- `inputs` — parametry akcji,
- `timeoutSeconds`, `maxAttempts` — timeout i retry,
- `onFailure`: `Abort` / `Continue` / `step:nazwa` — co dalej przy błędzie,
- `onCancel` — co przy anulowaniu,
- `nextStep`, `isEnd`, `isCritical` — przepływ i krytyczność.

### Parametry i walidacja

- `type`: `String`, `StringList`, `Integer`, `Boolean`, `StringMap`.
- `allowedValues`, `allowedPattern`, `default`, `description`.
- Parametry można przekazywać z CLI/API/EventBridge (input transformer).

### Wyjścia i referencje

- Wynik kroku: `{{ StepName.Output }}` (np. `{{ getInstanceDetails.instanceId }}`).
- W `Output` runbooka można wystawić wybrane wartości.
- `aws:executeScript` zwraca `$.Payload`; `aws:executeAwsApi` zwraca odpowiedź API; `aws:runInstances` zwraca ID instancji.

---

## 3. Akcje Automation — co potrafi runbook

| Akcja | Co robi | Uwaga egzaminacyjna |
|---|---|---|
| `aws:executeAwsApi` | wywołuje **dowolne API AWS** | najbardziej uniwersalna; wymaga uprawnień w roli |
| `aws:executeScript` | uruchamia **Python/PowerShell** wewnątrz Automation | nie na instancji; bez SSM Agent; output można logować do CloudWatch Logs |
| `aws:runCommand` | uruchamia polecenie na **managed instance** | wymaga SSM Agent; to nie to samo co „skrypt w Automation” |
| `aws:runInstances` | tworzy instancje EC2 | może tworzyć zasoby, ale to nie CloudFormation |
| `aws:changeInstanceState` | zmienia/asygnalizuje stan instancji | start/stop/terminate/restart |
| `aws:createImage` / `aws:copyImage` / `aws:deleteImage` | AMI: utwórz/kopiuj/usuń | golden AMI, kopie między regionami |
| `aws:createStack` / `aws:deleteStack` | tworzy/usuwa stack CloudFormation | orkiestracja wokół CFN |
| `aws:createTags` | nadaje tagi zasobom | compliance/tagging |
| `aws:executeAutomation` | uruchamia **inny runbook** (nested) | reużywalne runbooki; child automation |
| `aws:executeStateMachine` | uruchamia **Step Functions** | połączenie z workflow aplikacyjnym |
| `aws:invokeLambdaFunction` | wywołuje Lambda | integracja z custom logiką |
| `aws:invokeWebhook` | wywołuje webhook | integracje zewnętrzne |
| `aws:branch` | warunkowe rozgałęzienie | logika if/else |
| `aws:loop` | pętla po krokach | iteracje |
| `aws:pause` | pauza automacji | wstrzymanie |
| `aws:sleep` | opóźnienie | czekanie na coś spoza AWS |
| `aws:waitForAwsResourceProperty` | czeka, aż zasób osiągnie stan | np. `running`, `available` |
| `aws:assertAwsResourceProperty` | sprawdza stan i **failuje**, jeśli niezgodny | walidacja przed dalszymi krokami |
| `aws:approve` | czeka na **ręczne zatwierdzenie** | SNS, approvers, timeout |
| `aws:updateVariable` | aktualizuje zmienną runbooka | logika pomocnicza |

**Egzaminowo:** jeśli potrzebujesz „uruchomić skrypt na instancji” → `aws:runCommand`; jeśli „wykonać logikę w Pythonie bez wchodzenia na instancję” → `aws:executeScript`; jeśli „wywołać dowolne API AWS” → `aws:executeAwsApi`.

---

## 4. Jak uruchomić automację — wyzwalacze

| Sposób | Jak | Typowe użycie |
|---|---|---|
| **Ręcznie** | konsola, CLI `start-automation-execution`, SDK, API | jednorazowa operacja |
| **EventBridge — event** | reguła z targetem **Systems Manager Automation** | reakcja na zdarzenie (np. EC2 state change, GuardDuty finding) |
| **EventBridge — schedule** | reguła scheduled/cron lub Scheduler | cykliczne zadania (AMI, cleanup) |
| **Maintenance Window** | zarejestrowany task typu **Automation** | patching/maintenance w oknie |
| **State Manager association** | association z dokumentem Automation | utrzymywanie stanu/scheduled automation |
| **Config remediation** | akcja remediacji Config wskazuje runbook | naprawa niezgodności po wykryciu |
| **Incident Manager** | runbook = dokument Automation | automatyczna reakcja na incydent |
| **Change Manager** | change request → runbook | governance/approvals (tylko istniejący klienci) |

### CLI — podstawy

```bash
# uruchomienie
aws ssm start-automation-execution \
  --document-name "AWS-RestartEC2Instance" \
  --parameters InstanceId=i-0123456789abcdef0

# status i kroki
aws ssm get-automation-execution --automation-execution-id <id>
aws ssm describe-automation-step-executions --automation-execution-id <id>

# zatwierdzenie / odrzucenie
aws ssm send-automation-signal \
  --automation-execution-id <id> \
  --signal-type Approve
```

### Statusy

`Pending` → `InProgress` → (`Waiting` przy approval) → `Success` / `Failed` / `TimedOut` / `Cancelled`.

Kroki mają własne statusy (`Pending`, `InProgress`, `Waiting`, `Success`, `Failed`, `TimedOut`, `Cancelled`, `Exited` dla `aws:loop`).

---

## 5. IAM i bezpieczeństwo

- **AutomationAssumeRole** — rola, którą Automation przyjmuje do wykonywania akcji. Trust policy musi pozwalać `ssm.amazonaws.com`.
- **Bez roli** Automation działa z uprawnieniami użytkownika wywołującego (simple execution). Dla runbooków `AWS-*` i akcji międzyusługowych **zalecana jest dedykowana rola**.
- Rola musi mieć uprawnienia do **wszystkich** usług, których dotykają akcje (`ec2:*`, `s3:*`, `iam:PassRole` itd.) oraz `iam:PassRole` dla ról przekazywanych do zasobów.
- **SCP** mogą zablokować `ssm:StartAutomationExecution` lub akcje wykonywane przez rolę.
- **CloudTrail** rejestruje wywołania API (`StartAutomationExecution`, `SendAutomationSignal` itd.).
- **CloudWatch Logs** — output `aws:executeScript` można wysłać do log group (przydatne do debugowania).
- **Dokumenty** można udostępniać kontom lub publicznie; runbooki `AWS-*` są zarządzane przez AWS.
- **Approval** — SNS topic musi mieć nazwę zaczynającą się od `Automation`; approvers to użytkownicy/role IAM (max 10); domyślny timeout 7 dni, maks. 30 dni.

### Przykład approval step

```yaml
- name: approveChange
  action: aws:approve
  timeoutSeconds: 604800
  inputs:
    Message: 'Approve patching of production instances'
    Approvers:
      - arn:aws:iam::111122223333:role/ChangeApprover
    MinRequiredApprovals: 1
    NotificationArn: arn:aws:sns:eu-central-1:111122223333:AutomationApprovals
```

**Pułapka:** `aws:approve` **nie obsługuje** automacji multi-account/multi-region. Do takich scenariuszy użyj Change Manager (jeśli istniejący klient) albo zewnętrznego procesu approvals.

---

## 6. Multi-account i multi-region

Automation może działać na wiele kont/regionów z **centralnego konta**:

- Centralne konto: **delegated administrator** Systems Manager (lub management account) — potrzebne do listowania kont w OU.
- Role:
  - `AWS-SystemsManager-AutomationAdministrationRole` — w koncie centralnym (uruchamianie),
  - `AWS-SystemsManager-AutomationExecutionRole` — w **każdym** koncie docelowym; najlepiej przez **CloudFormation StackSets**.
- Cele: konta, **OU** (z child OUs), regiony, tagi, resource groups.
- **Custom runbook musi być udostępniony** kontom docelowym (shared document).
- Kontrola: `TargetLocationMaxConcurrency`, `TargetLocationMaxErrors`, `TargetsMaxConcurrency`, `TargetsMaxErrors`.
- Uruchomienie: `start-automation-execution` z `--target-locations` lub `--target-locations-url` (JSON z S3 presigned URL).
- Wynik: child automations w kontach docelowych; ich `AutomationExecutionId` jest outputem w koncie centralnym.
- **`aws:approve` nie działa** w multi-account/multi-region.

**Egzaminowo:** „centralnie patchować 100 kont” → Automation multi-account + role + StackSets + shared runbook; ewentualnie Maintenance Window per konto.

---

## 7. Limity i kontrola współbieżności

| Element | Limit |
|---|---|
| Równoczesne automacje na konto | **100** (w tym child automations i rate-control) |
| Kolejka automacji | **5000** |
| Równoczesne rate-control automations | **25** |
| Kolejka rate-control automations | **1000** |
| Approvers w `aws:approve` | max **10** |
| Timeout approval | domyślnie **7 dni**, max **30 dni** |
| Akcje w Automation | **20 typów** |

- **Rate controls** (`MaxConcurrency`, `MaxErrors`) ograniczają liczbę zasobów/kont-regionów przetwarzanych jednocześnie i liczbę błędów przed zatrzymaniem.
- Po przekroczeniu 100 automacje trafiają do kolejki ze statusem `Pending`.
- Można włączyć **adaptive concurrency**.
- CloudWatch alarm może anulować automację (i uruchomić `onCancel`).

---

## 8. Sąsiedzi w Systems Manager — co jest czym

| Narzędzie | Rola | Relacja do Automation |
|---|---|---|
| **Run Command** | jednorazowe polecenia/dokumenty na managed nodes | Automation może wywołać `aws:runCommand`; Run Command nie ma approvals i logiki warunkowej jak Automation |
| **Maintenance Windows** | harmonogram okien maintenance i zadań | może uruchamiać task typu Automation |
| **State Manager** | utrzymywanie zdefiniowanego stanu (associations) | association może uruchamiać runbook Automation (scheduled) |
| **Patch Manager** | patchowanie (baselines, patch groups) | często orkiestrowane przez Maintenance Window/Automation |
| **Change Manager** | governance zmian, approvals, change templates | uruchamia runbooki Automation; **zamknięty dla nowych klientów od 7.11.2025** |
| **Incident Manager** | zarządzanie incydentami, escalation | runbooki to dokumenty Automation |
| **OpsCenter** | OpsItems, centrum operacyjne | do OpsItem można dołączyć runbook |
| **Change Calendar** | kalendarz zmian/okresów freeze | używany przez Change Manager |
| **Compliance** | statusy zgodności patchy/asocjacji | Automation może remediować |
| **Session Manager** | sesje shell bez bastionu | nie zastępuje Automation |

### Change Manager — ważny status

Change Manager to framework change management (change templates, change requests, approvals, Change Calendar, audyt) uruchamiający runbooki Automation. **Od 7 listopada 2025 nie jest otwarty dla nowych klientów**; istniejący klienci mogą go używać. Na egzaminie może pojawić się jako pojęcie, ale w nowych projektach wybieraj raczej `aws:approve`, EventBridge, Maintenance Windows i zewnętrzny proces zmian.

---

## 9. Automation vs Run Command vs Step Functions vs Lambda vs CloudFormation vs Config

| Usługa | Do czego | Kiedy wybrać | Kiedy nie |
|---|---|---|---|
| **SSM Automation** | wieloetapowe zadania operacyjne na zasobach AWS, runbooki, approvals, remediacja | „zautomatyzuj operację na flocie”, „remediuj niezgodność”, „golden AMI”, „restart z warunkami” | ogólny workflow aplikacji (Step Functions), proste polecenie (Run Command) |
| **Run Command** | jedno polecenie/dokument na instancje | szybka operacja na instancjach, patch, skrypt | wieloetapowa logika, approvals, API AWS |
| **Step Functions** | ogólny workflow/orkiestracja aplikacji, złożone stany, integracje | aplikacyjne procesy, saga, human approval (callback) | proste zadania operacyjne „AWS-owe” |
| **Lambda** | obliczenia event-driven | custom logika, krótkie funkcje, integracje | orkiestracja wieloetapowa, operacje na flocie |
| **CloudFormation** | deklaratywne provisionowanie infrastruktury | tworzenie/zarządzanie zasobami jako kod | operacje na istniejących zasobach, remediacja |
| **Config remediation** | wykrycie + wywołanie remediacji | „napraw po wykryciu niezgodności” | samo wykrywanie (Config rule) |

**Dwie osie:**
- **Imperatywne vs deklaratywne:** Automation = imperatywne kroki operacyjne; CloudFormation = deklaratywny stan.
- **Operacje vs aplikacja:** Automation = runbooki AWS-owe; Step Functions = workflow aplikacyjny.

**Współpraca:**
- Config rule → remediation → **SSM Automation runbook** (`AWSConfigRemediation-*`).
- EventBridge event (np. GuardDuty finding) → **Automation**.
- Automation → `aws:runCommand` (instancje), `aws:invokeLambdaFunction` (custom), `aws:executeStateMachine` (Step Functions), `aws:createStack` (CFN).
- Step Functions → `ssm:startAutomationExecution` (przez AWS SDK integration).

---

## 10. Typowe wzorce użycia

### 10.1 Golden AMI

Runbook: `aws:createImage` → `aws:copyImage` do regionów → `aws:createTags` → `aws:deleteImage` (stare) → ewentualnie update ASG/launch template. Wyzwalane przez EventBridge schedule lub Maintenance Window.

### 10.2 Remediacja niezgodności (Config)

Config rule wykrywa publiczny bucket / brak szyfrowania → **automatic remediation** uruchamia runbook Automation (np. `AWS-DisableS3BucketPublicReadWrite` lub runbook `AWSConfigRemediation-*`, np. `AWSConfigRemediation-ConfigureS3BucketPublicAccessBlock`) → po naprawie status compliance się zmienia.

### 10.3 Reakcja na incydent

GuardDuty finding → EventBridge → Automation runbook:
- zmiana security group (izolacja),
- snapshot EBS do forensyki,
- stop/terminate instancji,
- rotacja kluczy,
- tagowanie zasobu/OpsItem.

### 10.4 Patching floty

Maintenance Window (targets po tagach) → task Automation lub Run Command (`AWS-RunPatchBaseline`) → pre/post snapshot, reboot, weryfikacja, raport compliance.

### 10.5 Zatrzymywanie idle resources

EventBridge schedule (np. codziennie 20:00) → Automation `aws:changeInstanceState` (stop) lub `aws:executeAwsApi` (StopInstances) → oszczędności; z wyjątkami przez `aws:branch`.

### 10.6 Multi-account remediation

Centralne konto (delegated admin) → Automation z `TargetLocations` (OU/konta/regiony) → child automations w kontach docelowych; role Administration/Execution; shared runbook.

### 10.7 Approval workflow

Runbook z `aws:approve` → SNS do approverów → `send-automation-signal Approve/Reject` → dalsze kroki. Dla wielu poziomów zatwierdzeń: Change Manager (istniejący klienci) lub własny proces.

---

## 11. Typowe pułapki egzaminacyjne

1. **Automation ≠ Run Command** — Run Command wykonuje polecenie/dokument na instancjach; Automation orkiestruje wieloetapowy proces (warunki, approvals, czekanie na stan).
2. **Runbook to dokument typu `Automation`** (schema 0.3), nie `Command` (1.2/2.0/2.2).
3. **`aws:executeScript` nie działa na instancji** — wykonuje się w Automation; do instancji użyj `aws:runCommand`.
4. **`aws:runCommand` wymaga SSM Agent**; `aws:executeAwsApi`/`executeScript` nie.
5. **Automation potrzebuje uprawnień** — bez `AutomationAssumeRole` działa z uprawnieniami wywołującego; dla `AWS-*` runbooków skonfiguruj rolę.
6. **`iam:PassRole`** — często brakuje go, gdy runbook przekazuje rolę do zasobu.
7. **100 równoczesnych automations** — reszta w kolejce; rate control osobno (25).
8. **Multi-account wymaga ról** Administration/Execution i **shared runbook**; nie wystarczy samo `--target-locations`.
9. **Custom runbook musi być udostępniony** kontom docelowym.
10. **`aws:approve` nie obsługuje multi-account/multi-region**.
11. **SNS topic do approval musi zaczynać się od `Automation`**.
12. **Approval timeout** domyślnie 7 dni, maks. 30 dni; max 10 approvers.
13. **Config remediation wywołuje runbook** — to nie Config sam wykonuje akcje; nie myl wykrywania z wykonaniem.
14. **Change Manager zamknięty dla nowych klientów** (7.11.2025); istniejący mogą używać.
15. **Incident Manager runbooks = Automation documents**; to nie osobny format.
16. **Maintenance Window uruchamia task Automation** — to nie to samo co Automation.
17. **State Manager association** może uruchamiać Automation, ale służy do utrzymywania stanu; nie myl z jednorazowym runbookiem.
18. **Step Functions to nie Automation** — ogólny workflow aplikacyjny vs runbooki operacyjne.
19. **Automation nie jest CloudFormation** — może tworzyć zasoby, ale nie jest deklaratywnym provisioningiem.
20. **`aws:waitForAwsResourceProperty` czeka; `aws:assertAwsResourceProperty` failuje** — nie myl.
21. **`onFailure` / `onCancel` / `nextStep` / `isEnd`** — przepływ sterujesz w runbooku; domyślnie błąd abortuje.
22. **Automation jest regionalne** — multi-region przez target locations; runbooki/dokumenty są regionalne.
23. **Historia wykonań** — do długoterminowego audytu używaj CloudTrail/CloudWatch Logs; konsola/API pokazuje bieżące/niedawne wykonania.
24. **Child automations** liczą się do limitu 100; `aws:executeAutomation` może kaskadować.

---

## 12. Scenariusze egzaminacyjne

**1. Co miesiąc utwórz golden AMI, skopiuj do 3 regionów i usuń starą.**
→ **SSM Automation** + EventBridge schedule (lub Maintenance Window); akcje `aws:createImage`, `aws:copyImage`, `aws:deleteImage`.

**2. Wykryto publiczny bucket S3 — napraw automatycznie.**
→ **Config rule** + **automatic remediation** → runbook Automation (np. `AWS-DisableS3BucketPublicReadWrite` lub `AWSConfigRemediation-ConfigureS3BucketPublicAccessBlock`).

**3. 200 kont ma dostać poprawkę security group.**
→ **Automation multi-account/multi-region**: centralne konto delegated admin, `AWS-SystemsManager-AutomationAdministrationRole`, `AWS-SystemsManager-AutomationExecutionRole` w kontach docelowych, shared runbook, `TargetLocations` (OU/konta/regiony).

**4. Produkcyjny restart wymaga zatwierdzenia przez change approvera.**
→ `aws:approve` w runbooku (single account) + SNS; dla wielu poziomów/Change Calendar → Change Manager (existing customers).

**5. GuardDuty wykrył kompromitację instancji — izoluj ją.**
→ EventBridge rule na finding → **Automation runbook**: `aws:executeAwsApi` (modify security group), snapshot EBS, stop instance, tag.

**6. Codziennie o 20:00 zatrzymaj instancje dev.**
→ EventBridge schedule → Automation `aws:changeInstanceState`/`StopInstances`; wyjątki przez `aws:branch`/tagi.

**7. Po patchowaniu zrestartuj instancje i poczekaj, aż będą zdrowe.**
→ Automation: `aws:runCommand` (patch), `aws:changeInstanceState` (reboot), `aws:waitForAwsResourceProperty`, `aws:assertAwsResourceProperty` dla health check.

**8. Uruchom skrypt Python, który wywołuje AWS API, bez SSH.**
→ `aws:executeScript` (Python) — działa w Automation, nie na instancji; output do CloudWatch Logs.

**9. Runbook ma czekać, aż EBS będzie `available`.**
→ `aws:waitForAwsResourceProperty` (`Service: ec2`, `Api: DescribeVolumes`, `DesiredValues: [available]`).

**10. W zależności od stanu zasobu wykonaj inne kroki.**
→ `aws:branch` + `aws:assertAwsResourceProperty`/`aws:waitForAwsResourceProperty`.

**11. Jeden runbook ma wywołać inny runbook (reuse).**
→ `aws:executeAutomation` (nested/child automation).

**12. Uruchom Step Functions z runbooka.**
→ `aws:executeStateMachine`.

**13. Wywołaj Lambda z runbooka.**
→ `aws:invokeLambdaFunction`.

**14. Utrzymuj reguły security group w pożądanym stanie.**
→ **State Manager association** z Automation/Command document (scheduled); Automation do driftu.

**15. Reakcja na incydent według planu eskalacji.**
→ **Incident Manager** + runbook Automation.

**16. Wymuszenie zatwierdzenia i pełny audyt zmian.**
→ Change Manager (existing customers) lub `aws:approve` + CloudTrail/CloudWatch Logs.

**17. Migracja/aktualizacja AMI w Auto Scaling Group.**
→ `AWS-UpdateLinuxAmi`/`AWS-UpdateWindowsAmi` (Automation) + `aws:createImage`/`copyImage` + update launch template/ASG.

---

## 13. Pytania testowe (z odpowiedziami)

**1.** Czym jest SSM Automation?
A) kolejką wiadomości
B) orkiestratorem wieloetapowych zadań operacyjnych opartym na runbookach
C) usługą monitoringu
D) zamiennikiem CloudFormation
**Odp: B.**

**2.** Jakiego typu dokumentem SSM jest runbook Automation?
A) Command (schema 1.2)
B) Automation (schema 0.3)
C) Session
D) Policy
**Odp: B.**

**3.** Chcesz wykonać polecenie na instancji EC2 z runbooka. Której akcji użyjesz?
A) `aws:executeScript`
B) `aws:runCommand`
C) `aws:executeAwsApi`
D) `aws:invokeLambdaFunction`
**Odp: B.** `aws:runCommand` wymaga SSM Agent; `executeScript` działa w Automation.

**4.** Chcesz uruchomić dowolne API AWS z runbooka. Co wybierzesz?
A) `aws:executeAwsApi`
B) `aws:sleep`
C) `aws:branch`
D) `aws:approve`
**Odp: A.**

**5.** Ile automations może działać równocześnie na konto (domyślnie)?
A) 10
B) 25
C) 100
D) 1000
**Odp: C.** Nadmiar trafia do kolejki (5000).

**6.** Które role są potrzebne do multi-account Automation?
A) tylko `OrganizationAccountAccessRole`
B) `AWS-SystemsManager-AutomationAdministrationRole` i `AWS-SystemsManager-AutomationExecutionRole`
C) tylko `AWSServiceRoleForAmazonSSM`
D) `AWSServiceRoleForOrganizations`
**Odp: B.** Administration w centralnym, Execution w każdym docelowym.

**7.** Co jest wymagane przez `aws:approve`?
A) tylko temat SNS o dowolnej nazwie
B) lista approverów, SNS topic z prefiksem `Automation`, opcjonalnie min. liczba zatwierdzeń
C) tylko rola IAM
D) CloudFormation stack
**Odp: B.**

**8.** Czy `aws:approve` działa w automacji multi-account/multi-region?
A) tak, zawsze
B) nie
C) tylko z SNS
D) tylko w Change Manager
**Odp: B.** Nie obsługuje multi-account/region.

**9.** Jak wyzwolić Automation z EventBridge?
A) target typu Systems Manager Automation
B) target SQS
C) alarm CloudWatch
D) nie da się
**Odp: A.**

**10.** Co robi Config remediation?
A) sam wykonuje zmiany bez runbooka
B) wywołuje runbook Automation (lub inną akcję remediacji), który wykonuje zmianę
C) blokuje API
D) wykrywa ataki
**Odp: B.** Config wykrywa; Automation wykonuje.

**11.** Który mechanizm jest najlepszy do ogólnego workflow aplikacyjnego?
A) SSM Automation
B) Step Functions
C) Run Command
D) Config
**Odp: B.** Automation jest operacyjny/AWS-owy.

**12.** Co robi `aws:waitForAwsResourceProperty`?
A) failuje, jeśli stan jest niezgodny
B) czeka, aż zasób osiągnie określony stan
C) uruchamia Lambda
D) tworzy stack
**Odp: B.** `assert...` failuje przy niezgodności.

**13.** Co robi `aws:executeAutomation`?
A) uruchamia inny runbook (child automation)
B) usuwa stack
C) tworzy AMI
D) wysyła SNS
**Odp: A.**

**14.** Czy Change Manager jest dostępny dla nowych klientów?
A) tak, od 2025
B) nie — od 7.11.2025 zamknięty dla nowych; istniejący mogą używać
C) tylko w Organizations
D) tylko z Automation
**Odp: B.**

**15.** Który mechanizm utrzymuje stan (association) i może uruchamiać Automation?
A) State Manager
B) Run Command
C) Config
D) CloudTrail
**Odp: A.**

---

## 14. Ściąga — „jeśli w pytaniu..., to...”

| Sygnał | Odpowiedź |
|---|---|
| „wieloetapowa automatyzacja operacji AWS” | SSM Automation |
| „runbook” | dokument SSM typu Automation (schema 0.3) |
| „wykonaj polecenie na instancji” | `aws:runCommand` / Run Command |
| „skrypt Python/PowerShell bez wchodzenia na instancję” | `aws:executeScript` |
| „wywołaj dowolne API AWS” | `aws:executeAwsApi` |
| „czekaj na stan zasobu” | `aws:waitForAwsResourceProperty` |
| „fail, jeśli stan zły” | `aws:assertAwsResourceProperty` |
| „warunkowe kroki” | `aws:branch` |
| „zatwierdzenie przed zmianą” | `aws:approve` (single account) / Change Manager (existing) |
| „harmonogram” | EventBridge rule (schedule) / Maintenance Window / State Manager |
| „reakcja na event” | EventBridge target „Systems Manager Automation” |
| „remediacja po Config” | Config remediation → Automation runbook |
| „wiele kont/regionów” | Automation `TargetLocations` + Administration/Execution roles + shared runbook |
| „utrzymuj stan” | State Manager association |
| „patchowanie” | Patch Manager + Maintenance Window/Automation |
| „incydent” | Incident Manager runbook (Automation doc) |
| „golden AMI” | `aws:createImage`/`copyImage`/`deleteImage` + EventBridge |
| „nested runbook” | `aws:executeAutomation` |
| „wywołaj Step Functions” | `aws:executeStateMachine` |
| „wywołaj Lambda” | `aws:invokeLambdaFunction` |
| „twórz stack” | `aws:createStack` |
| „limit współbieżności” | 100 automations/konto, kolejka 5000 |
| „multi-account approval” | `aws:approve` NIE; Change Manager lub własny proces |
| „Change Manager” | zamknięty dla nowych od 7.11.2025 |

---

## 15. Mapowanie na SAP-C02

- **Domain 1 (Organizational complexity):** multi-account/multi-region Automation, delegated admin, role Administration/Execution, shared runbooks, Organizations OUs.
- **Domain 2 (New solutions):** event-driven remediation (EventBridge/GuardDuty/Config → Automation), golden AMI pipelines, approval workflows.
- **Domain 3 (Continuous improvement):** patching, drift remediation, operational runbooks, rate controls, audit (CloudTrail/CloudWatch Logs).
- **Domain 4 (Migration/modernization):** automatyzacja migracji, standaryzacja operacji, zastępowanie ręcznych runbooków.

---

## 16. Źródła (docs)

- SSM Automation — what is: https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html
- Runbooki: https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-documents.html
- Akcje Automation: https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-actions.html
- `aws:approve`: https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-approve.html
- Multi-account/multi-region: https://docs.aws.amazon.com/systems-manager/latest/userguide/running-automations-multiple-accounts-regions.html
- EventBridge trigger: https://docs.aws.amazon.com/systems-manager/latest/userguide/running-automations-event-bridge.html
- Maintenance Windows: https://docs.aws.amazon.com/systems-manager/latest/userguide/scheduling-automations-maintenance-windows.html
- State Manager: https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html
- Change Manager: https://docs.aws.amazon.com/systems-manager/latest/userguide/change-manager.html
- Statusy: https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-statuses.html

---

*Najczęstszy błąd: wybieranie Run Command na pytanie o wieloetapową remediację z warunkami/approvals (to Automation) oraz wybieranie Automation na pytanie o prosty, jednorazowy skrypt na instancji (to Run Command).*
