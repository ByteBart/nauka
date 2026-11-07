# AWS Organizations i OU — notatki do egzaminu SAP-C02

> **Zakres:** AWS Certified Solutions Architect – Professional (SAP-C02), domena 1 „Design Solutions for Organizational Complexity”.
> **Stan wiedzy:** listopad 2026. Egzamin SAP-C02 nie musi zawierać najnowszych funkcji (RCP, polityki deklaratywne, billing transfer), ale pytania o nie pojawiają się już w materiałach przygotowawczych — dlatego są opisane osobno na końcu.
> **Jak korzystać:** najpierw sekcje 0–3 (fundament), potem 5–6 (SCP/RCP — najczęściej pytane), 8–9 (co Organizations odblokowuje i porównanie), a na koniec 12–15 (pułapki, scenariusze, pytania, ściąga).

---

## 0. Najkrótsza możliwa odpowiedź

- **AWS Organizations** to darmowa, globalna usługa do **centralnego zarządzania wieloma kontami AWS**: wspólny rachunek, hierarchia kont (OU), polityki strażnicze (SCP/RCP), tworzenie i zamykanie kont oraz integracje z innymi usługami (CloudTrail, Config, GuardDuty, Security Hub, RAM, IAM Identity Center, Control Tower itd.).
- **OU (Organizational Unit)** to **kontener na konta** wewnątrz organizacji, który można zagnieżdżać. Służy do **grupowania kont według polityk**, a nie według struktury firmy. Polityki przypisane do OU **dziedziczą się w dół** na konta.
- **Bez Organizations** każde konto jest samodzielne: własny rachunek, własny płatnik, brak SCP, brak centralnego audytu, brak współdzielenia zasobów „po organizacji”.
- **Z włączonym Organizations** (feature set `ALL`) dostajesz: jeden rachunek, SCP/RCP, konta tworzone przez API, centralny CloudTrail/Config/GuardDuty/Security Hub, RAM bez zaproszeń, IAM Identity Center dla całej organizacji, Control Tower, StackSets na OU.
- **Feature set `CONSOLIDATED_BILLING`** (tylko wspólny rachunek) to nie to samo co `ALL` — nie ma SCP, RCP, polityk, integracji usług (trusted access) ani delegated administratorów.

**Zdanie-klucz na egzamin:** *SCP i RCP niczego nie przyznają — tylko ograniczają maksymalne uprawnienia. IAM nadal musi przyznać dostęp. SCP nie działa na konto zarządzające.*

---

## 1. Pojęcia i elementy organizacji

| Pojęcie | Znaczenie | Ważne na egzaminie |
|---|---|---|
| **Organization** | Zbiór kont zarządzanych centralnie, w drzewie z rootem na czele | Jedno konto należy do **jednej** organizacji w danym momencie |
| **Root (administrative root)** | Najwyższy kontener organizacji, tworzony automatycznie; jest w koncie zarządzającym | Tylko **jeden** root. Polityki na root działają na wszystkie konta członkowskie (SCP/RCP) lub na wszystkie konta łącznie z zarządzającym (polityki deklaratywne) |
| **OU** | Jednostka organizacyjna — kontener kont, może zawierać inne OU | Zagnieżdżanie do **5 poziomów** pod rootem; każdą politykę można przypiąć do root/OU/konta |
| **Management account** | Konto, które utworzyło organizację; płatnik (payer) i „właściciel” organizacji | **Nie można go zmienić**; **SCP go nie obejmują**; nie powinno się w nim trzymać workloadów |
| **Member account** | Każde inne konto w organizacji | SCP/RCP działają na konta członkowskie, **także na delegated administratorów** |
| **Delegated administrator** | Konto członkowskie wyznaczone do zarządzania daną usługą lub politykami organizacji w imieniu management account | Rejestracja przez `RegisterDelegatedAdministrator`; nie może tworzyć/zapraszać kont |
| **Trusted access** | Zgoda, by usługa AWS (np. CloudTrail, RAM, StackSets) działała w całej organizacji | Włącza się w management account; często tworzy service-linked role `AWSServiceRoleForOrganizations` |
| **Handshake** | Bezpieczna wymiana informacji przy zaproszeniu konta / włączaniu all features | Zaproszenie wygasa po **15 dniach**, enable-all-features po **90 dniach** |
| **OrganizationAccountAccessRole** | Rola tworzona w nowych kontach zakładanych przez Organizations, pozwalająca management account zarządzać kontem | Domyślna nazwa; można ją usunąć/zmienić — wtedy tracisz dostęp administracyjny z management account |

### Konto zarządzające — najważniejsze konsekwencje

- Płaci za **wszystkie** konta członkowskie.
- Tworzy/zaprasza konta, przenosi je między OU, przypina polityki, wyznacza delegated administratorów.
- **SCP i RCP go nie ograniczają** — dlatego nie trzymaj tam danych ani workloadów. Jeśli ktoś w management account ma `AdministratorAccess`, może wyłączyć CloudTrail, mimo SCP na root.
- Nie można go zamknąć, dopóki ma konta członkowskie — najpierw trzeba je usunąć lub zamknąć (potem zamknięcie konta zarządzającego standardową procedurą root).
- Aby „zmienić” konto zarządzające, w praktyce trzeba zbudować nową organizację i przenieść konta.

---

## 2. Struktura, dziedziczenie i limity

### Drzewo

```
Root (konto zarządzające)
├── OU: Security
│   ├── konto: log-archive
│   ├── konto: security-tooling
│   └── OU: Security/Detective
├── OU: Infrastructure
│   ├── konto: network
│   └── konto: shared-services
├── OU: Workloads
│   ├── OU: Prod
│   │   ├── konto: app1-prod
│   │   └── konto: app2-prod
│   └── OU: NonProd
│       └── konto: app1-dev
└── OU: Sandbox
```

- Konto może być **bezpośrednio w root** albo w **dokładnie jednym** OU.
- Konto tworzone przez Organizations powstaje **w root** i dopiero potem przenosisz je do OU (albo od razu przez Account Factory/Control Tower).
- **Głębokość OU: maks. 5 poziomów** pod rootem (bez roota i bez kont).
- **Liczba OU w organizacji: 2000** (domyślnie).
- **Liczba kont:** limit jest zwiększalny przez Service Quotas; aktualna dokumentacja podaje niski default i możliwość zwiększenia **do 50 000**. Starsze materiały podają 10 000 — na egzaminie liczby kont raczej nie są pytane, ale nie zakładaj „nieskończoności”.
- **Przenoszenie kont:** tylko management account, przez `MoveAccount`, tylko w obrębie jednego roota (czyli w praktyce między OU).
- Nowe konto w OU **automatycznie dziedziczy** polityki przypisane do OU/root — nie trzeba nic robić.
- **Tagi** można nadawać root/OU/kontom (do 50), m.in. do raportowania i automatyzacji.

### Limity polityk (przydatne, gdy pytanie dotyczy „czy się zmieści”)

| Polityka | Max rozmiar | Max przypięć na root/OU/konto |
|---|---|---|
| SCP | 10 240 znaków | 10 |
| RCP | 5 120 znaków | 5 (w tym obowiązkowa `RCPFullAWSAccess`) |
| Polityki deklaratywne (każdy typ) | 10 000 znaków | 10 |
| Tag policy | 10 000 znaków | 10 |
| Backup policy | 10 000 znaków | 10 |
| AI services opt-out | 2 500 znaków | 5 |

Pamiętaj: polityki **odziedziczone** nie liczą się do limitu przypięć.

---

## 3. Feature sets — `ALL` vs `CONSOLIDATED_BILLING`

Przy tworzeniu organizacji wybierasz zestaw funkcji. Domyślnie (konsola/API bez parametru) jest **`ALL`**.

| Obszar | `ALL` (all features) | `CONSOLIDATED_BILLING` (tylko billing) |
|---|---|---|
| Jeden rachunek, wspólne wolumeny, RI/Savings Plans | ✅ | ✅ |
| SCP / RCP / polityki deklaratywne | ✅ | ❌ |
| OUs jako mechanizm polityk | ✅ | ❌ (brak polityk) |
| Trusted access + delegated administrator | ✅ | ❌ |
| Zaawansowane zarządzanie kontami (`CreateAccount`, delegated admin) | ✅ | ❌ |
| Organization trail (CloudTrail), Config aggregator | ✅ | ❌ |
| RAM: współdzielenie z organizacją/OU bez zaproszeń | ✅ | ❌ |
| IAM Identity Center — organization instance | ✅ | ❌ |
| Control Tower / Account Factory | ✅ | ❌ |
| StackSets z service-managed permissions | ✅ | ❌ |
| Service Catalog sharing do OU | ✅ | ❌ |
| Centralne włączanie GuardDuty/Security Hub/Inspector | ✅ | ❌ |
| Centralne root access management | ✅ | ❌ |

> **Uwaga:** dokładny zakres operacji dostępnych w CB-only bywa zmieniany przez AWS. Egzaminowo liczy się przede wszystkim: **brak SCP/RCP, brak polityk, brak trusted access i delegated adminów, brak integracji typu org trail/RAM/Control Tower.**

### Migracja CB-only → ALL

- **Jednokierunkowa** — nie wróci się do CB-only.
- **Konta zaproszone** muszą zaakceptować zmianę (handshake `ENABLE_ALL_FEATURES`); konta **utworzone** przez Organizations nie muszą (nie trzeba ich zgody).
- W trakcie migracji nowe zaproszenia są anulowane/ponawiane — uważaj w scenariuszach.
- Wymagana jest rola `AWSServiceRoleForOrganizations` w każdym koncie.

**Egzaminowo:** jeśli pytanie mówi „mamy organizację tylko z consolidated billing i chcemy SCP” → odpowiedź: *enable all features* (z akceptacją zaproszonych kont), a nie „utwórz nową organizację”.

---

## 4. Polityki organizacji — mapa

AWS Organizations ma dwie rodziny polityk:

1. **Polityki autoryzacyjne (authorization policies)** — kontrolują dostęp:
   - **SCP** — strona **tożsamości** (principals: IAM users/roles) w kontach członkowskich.
   - **RCP** — strona **zasobu** (resources) w kontach członkowskich, także dla podmiotów spoza organizacji.
2. **Polityki deklaratywne (declarative policies)** — ustawiają i utrzymują **konfigurację usług**, nie uprawnienia:
   - EC2, S3, Backup, Tag, AI services opt-out, Chatbot, Security Hub, Inspector, GuardDuty, Bedrock, Upgrade rollout, Shield Network Security Director.

| Polityka | Co ogranicza/ustawia | Poziom działania | Uwaga |
|---|---|---|---|
| **SCP** | maks. uprawnienia IAM users/roles | root, OU, konto | nie dotyczy management account i service-linked roles |
| **RCP** | maks. uprawnienia do zasobów | root, OU, konto | dotyczy też podmiotów zewnętrznych; tylko wybrane usługi |
| **Tag policy** | standaryzacja tagów (klucze, wartości, wielkość liter) | root, OU, konto | **nie wymusza** otagowania zasobów |
| **Backup policy** | plany AWS Backup dla zasobów po tagach | root, OU, konto | centralny backup, cross-account |
| **AI services opt-out** | rezygnacja ze zbierania danych przez usługi AI | root, OU, konto | zgodność/prywatność |
| **EC2 policy** | konfiguracja EC2 (np. publiczne snapshoty, IMDS) | root, OU, konto | polityka deklaratywna |
| **S3 policy** | konfiguracja S3 (np. Block Public Access) | root, OU, konto | nowsza funkcja |
| **GuardDuty / Security Hub / Inspector policy** | automatyczne włączanie i konfiguracja | root, OU, konto | wspiera delegated admin |
| **Bedrock policy** | wymuszanie guardrails | root, OU, konto | nowsza funkcja |
| **Chat applications policy** | dostęp aplikacji czatowych (Slack/Teams) | root, OU, konto | |
| **Upgrade rollout policy** | stopniowe aktualizacje zasobów | root, OU, konto | |
| **Shield Network Security Director policy** | centralne włączanie | root, OU, konto | |

Włączanie typu polityki robi się raz na root (`EnablePolicyType`). SCP są domyślnie włączone przy `ALL`; RCP trzeba włączyć osobno. Po wyłączeniu typu polityki wszystkie przypięcia przepadają.

---

## 5. SCP — Service Control Policy (najważniejsze na egzaminie)

### Co SCP robi, a czego nie robi

**Robi:**
- Ustawia **maksymalny pułap uprawnień** dla IAM users i roles w kontach członkowskich.
- Działa na **wszystkich IAM users i rolach** w kontach członkowskich — **także na root usera konta członkowskiego**.
- Warunki: region, sieć, MFA, principal ARN, tagi, organizacja (`aws:PrincipalOrgID`, `aws:PrincipalOrgPaths`, `aws:ResourceOrgID`).
- Dziedziczy się: root → OU → OU → konto; **każdy poziom musi pozwalać**, a jawny `Deny` na dowolnym poziomie wygrywa.

**Nie robi:**
- **Nie przyznaje uprawnień.** `Allow` w SCP to tylko filtr. IAM policy nadal musi pozwolić.
- **Nie działa na management account** (ani na jego root usera).
- **Nie działa na service-linked roles.**
- **Nie działa bezpośrednio na resource-based policies** (np. bucket policy dla zewnętrznego konta nie jest ograniczona SCP właściciela bucketu — SCP działa na principals z konta, w którym jest przypięty).
- **Nie zastępuje** permissions boundary ani IAM policy.
- Nie można w nim użyć `Principal`/`NotPrincipal`. W `Allow` element `Resource` może być tylko `"*"`; w `Deny` można podać ARN-y.

### Logika oceny (member account)

```
Jawny Deny (SCP, IAM, boundary, RCP, session policy)  → ODMOWA
Brak Allow w SCP (którymkolwiek poziomie)              → ODMOWA
IAM identity policy / resource policy nie pozwala      → ODMOWA
Boundary nie pozwala                                   → ODMOWA
Wszystkie warstwy pozwalają                            → ZEZWOLENIE
```

Efektywne uprawnienia = **część wspólna** tego, co pozwalają: IAM identity policy, permissions boundary, SCP, RCP (dla zasobów) i session policy.

### Dwa style: deny-list vs allow-list

- **Deny-list (zalecany start):** zostawiasz domyślną `FullAWSAccess` i dodajesz `Deny` na to, czego zabraniasz. Prostsze, mniej ryzykowne.
- **Allow-list:** **usuwasz `FullAWSAccess`** i dodajesz SCP, które jawnie pozwalają tylko wybrane usługi/akcje. Wszystko inne jest domyślnie zabronione. Wymaga testów — łatwo zablokować sobie IAM/STS/CloudTrail i odciąć dostęp.
- `FullAWSAccess` jest domyślnie przypięta do root, każdego OU i każdego konta. **Nie usuwaj jej bez planu** — bez niej wszystkie akcje w kontach członkowskich zaczną failować.

### Przykłady SCP

**1) Blokada opuszczania organizacji i zamykania konta** (od lipca 2026 konsola zakłada to automatycznie dla nowych organizacji):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyLeaveAndClose",
    "Effect": "Deny",
    "Action": ["organizations:LeaveOrganization", "account:CloseAccount"],
    "Resource": "*"
  }]
}
```

**2) Blokada regionów poza UE (z wyjątkami dla usług globalnych):**

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyOutsideEU",
    "Effect": "Deny",
    "NotAction": [
      "iam:*", "sts:*", "organizations:*", "route53:*", "cloudfront:*",
      "support:*", "budgets:*", "ce:*", "cur:*", "account:*",
      "s3:GetBucketLocation", "s3:ListAllMyBuckets"
    ],
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {"aws:RequestedRegion": ["eu-central-1", "eu-west-1"]}
    }
  }]
}
```

**3) Ochrona usług bezpieczeństwa i audytu** — `Deny` na wyłączanie CloudTrail, Config i GuardDuty (plus delegated admin i org trail):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "ProtectSecurityServices",
    "Effect": "Deny",
    "Action": [
      "cloudtrail:StopLogging", "cloudtrail:DeleteTrail", "cloudtrail:UpdateTrail",
      "config:StopConfigurationRecorder", "config:DeleteConfigurationRecorder",
      "config:DeleteDeliveryChannel",
      "guardduty:DeleteDetector", "guardduty:DisassociateFromAdministratorAccount"
    ],
    "Resource": "*"
  }]
}
```

**4) Zablokowanie root usera w kontach członkowskich:**

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyRootUser",
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "StringLike": {"aws:PrincipalArn": "arn:aws:iam::*:root"}
    }
  }]
}
```

(Uwaga: root w management account nie podlega SCP. Root w member account — tak.)

**5) Wymuszenie szyfrowania obiektów S3:**

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyUnencryptedPutObject",
    "Effect": "Deny",
    "Action": "s3:PutObject",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {"s3:x-amz-server-side-encryption": "AES256"}
    }
  }]
}
```

### Gdzie przypinać SCP

- Najpierw **testowe OU** (lub „policy staging OU”), potem kolejne OU, na końcu root.
- SCP na root działa na wszystkie konta członkowskie — to najsilniejsza broń i największe ryzyko.
- Nowe konta w OU dziedziczą SCP automatycznie.
- SCP dotyczy też delegated administratorów (to konta członkowskie).

---

## 6. RCP — Resource Control Policy

**RCP to lustrzane odbicie SCP po stronie zasobu.** Odpowiada na pytanie: *kto (także spoza organizacji) może dostać się do moich zasobów?*

- Dostępne **tylko w `ALL`**, nie w CB-only.
- Przypinane do root/OU/konta; dziedziczą się; **niczego nie przyznają**.
- Działają na **zasoby w kontach członkowskich** — nie na zasoby w management account.
- Obejmują **podmioty zewnętrzne** (np. partnera, inne konto spoza organizacji) — czego SCP nie potrafi.
- Nie dotyczą **service-linked roles** ani zarządzanych kluczy KMS; wyjątek: `kms:RetireGrant`.
- Wspierają tylko **wybrane usługi** — m.in. S3, SQS, KMS, Secrets Manager, CloudWatch Logs, DynamoDB, ECR, EventBridge, CloudFront, STS, IAM Roles Anywhere, CodeBuild/CodeCommit/CodePipeline, OpenSearch, WAF, Firewall Manager, Health, Support.
- Domyślnie przypięta jest `RCPFullAWSAccess` (nie da się jej odpiąć; liczy się do limitu 5).
- W RCP **można używać `Principal`** (inaczej niż w SCP).

### SCP vs RCP — tabela decyzyjna

| Kryterium | SCP | RCP |
|---|---|---|
| Strona kontroli | tożsamość (principal) | zasób (resource) |
| Kogo dotyczy | IAM users/roles w member accounts | każde podmioty (w tym zewnętrzne) sięgające do zasobów |
| Kierunek pytania | „co moi ludzie mogą robić?” | „kto może dostać się do moich zasobów?” |
| `Principal` w polityce | ❌ | ✅ |
| Management account | ❌ | ❌ (zasoby mgmt) |
| Service-linked roles | ❌ | ❌ |
| Przykład | blokada regionu, blokada usług | blokada dostępu spoza organizacji do S3/KMS |
| Wymagany feature set | `ALL` | `ALL` |

### Przykład RCP — S3 tylko dla organizacji

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyS3OutsideOrg",
    "Effect": "Deny",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": "*",
    "Condition": {
      "StringNotEquals": {"aws:PrincipalOrgID": "o-abc123example"}
    }
  }]
}
```

- `aws:PrincipalOrgID` = organizacja podmiotu wywołującego.
- `aws:PrincipalOrgPaths` = ścieżka OU podmiotu (np. tylko `Workloads/Prod`).
- `aws:ResourceOrgID` = organizacja właściciela zasobu (używane w SCP do „identity perimeter”).

**Uwaga praktyczna:** nadmiernie szeroki RCP (np. `Deny Principal:*` dla `s3:*` bez wyjątków) potrafi zablokować usługi AWS działające w Twoim imieniu. Testuj od pojedynczych kont, analizuj `AccessDenied` w CloudTrail, dopiero potem rozszerzaj na OU/root.

### Data perimeter — jak to układać

- **SCP**: „moje tożsamości mogą chodzić tylko do zasobów w mojej organizacji” (warunek `aws:ResourceOrgID`).
- **RCP**: „do moich zasobów mogą wchodzić tylko tożsamości z mojej organizacji” (warunek `aws:PrincipalOrgID`).
- Razem tworzą granicę danych (data perimeter). To lubiany temat scenariuszowy na Professional.

---

## 7. Polityki deklaratywne — co ustawiają

To nie są uprawnienia. To **pożądany stan konfiguracji** usług, utrzymywany centralnie.

| Polityka | Co robi | Typowy scenariusz |
|---|---|---|
| **Tag policy** | standaryzuje klucze/wartości tagów i wielkość liter; raport zgodności | ujednolicenie `CostCenter`, `Environment`; **nie wymusza** tagowania |
| **Backup policy** | centralne plany AWS Backup dla zasobów wybranych tagami | backup wszystkich kont, w tym nowych; cross-account copy |
| **AI services opt-out** | rezygnacja ze zbierania danych przez usługi AI | wymogi prywatności/compliance |
| **EC2 policy** | wymusza konfigurację EC2 (np. brak publicznych snapshotów/AMI, IMDS) | hardening na skalę organizacji |
| **S3 policy** | wymusza konfigurację S3 (np. Block Public Access) | ochrona przed publicznymi bucketami |
| **GuardDuty policy** | włącza GuardDuty i plany ochrony | automatyczne włączanie w nowych kontach |
| **Security Hub policy** | włącza standardy/kontrole | centralny postęp zgodności |
| **Inspector policy** | włącza skanowanie podatności | ciągłe skanowanie całej organizacji |
| **Bedrock policy** | wymusza guardrails dla wywołań modeli | governance AI |
| **Chat applications policy** | kontroluje dostęp z Slack/Teams | ograniczenie kanałów dostępu |
| **Upgrade rollout policy** | stopniowe, kontrolowane aktualizacje | ograniczenie ryzyka zmian |
| **Shield Network Security Director policy** | centralne włączanie | ochrona DDoS |

**Kluczowa różnica:** polityka deklaratywna na root **dotyczy też management account** (w przeciwieństwie do SCP/RCP).

**Tag policy — ważny niuans:** tag policy standaryzuje klucze/wartości i wielkość liter oraz — dla wskazanych typów zasobów — potrafi **egzekwować** zgodność tagowania (niezgodny request jest odrzucany). **Nie wymusza jednak samego faktu otagowania**: zasoby bez tagów w ogóle nie są oceniane. Do wymuszenia obecności tagów potrzebujesz SCP (warunek na `aws:RequestedTag`/`aws:ResourceTag`) albo reguły AWS Config (`required-tags`). Egzaminowo: „standardize tags” → tag policy; „require tags on every resource” → SCP/Config.

---

## 8. Co Organizations odblokowuje — katalog usług i wzorców

Mechanizmy:
- **Trusted access** — usługa może działać w całej organizacji.
- **Delegated administrator** — konto członkowskie administruje usługą w imieniu organizacji.
- **Organization trail / organization aggregator** — centralny zbiór logów i danych.
- **Auto-enable** — nowe konta automatycznie dostają konfigurację usługi.
- **Org-level resource sharing (RAM)** — bez zaproszeń.

| Usługa | Co daje Organizations | Wymaga `ALL`? | Delegated admin? |
|---|---|---|---|
| **CloudTrail** | organization trail — logi wszystkich kont do centralnego bucketu; nowe konta automatycznie | ✅ | ✅ |
| **AWS Config** | organization aggregator, reguły/conformance packs na całą organizację | ✅ | ✅ |
| **GuardDuty** | auto-enable, centralne plany ochrony, org policies | ✅ | ✅ |
| **Security Hub** | auto-enable, central configuration, standardy na org | ✅ | ✅ |
| **Amazon Inspector / Macie / Detective** | skanowanie i widoczność całej organizacji | ✅ | ✅ |
| **IAM Access Analyzer** | analiza polityk w całej organizacji | ✅ | ✅ |
| **Firewall Manager** | centralne WAF/Shield/SG policies na OU | ✅ | ✅ |
| **Security Lake / Audit Manager** | centralna zbiórka logów i dowodów | ✅ | ✅ |
| **RAM** | współdzielenie VPC/subnetów, TGW, Resolver rules, capacity reservations — bez zaproszeń | ✅ | ❌ |
| **IAM Identity Center** | organization instance, permission sets, dostęp do wszystkich kont z jednego miejsca | ✅ | ✅ |
| **Control Tower** | landing zone, Account Factory, controls (preventive/detective/proactive), drift | ✅ | ❌ |
| **CloudFormation StackSets** | service-managed permissions, wdrożenia na OU, auto-deploy do nowych kont | ✅ | ✅ |
| **Service Catalog** | udostępnianie portfolio organizacji/OU | ✅ | ✅ |
| **AWS Backup** | backup policies, cross-account backup | ✅ | ✅ |
| **AWS Health** | organizacyjny widok zdarzeń | ✅ | ✅ |
| **Trusted Advisor** | organizational view, priorytety | ✅ | ❌ (organizational view) |
| **Compute Optimizer / Cost Optimization Hub** | rekomendacje dla całej organizacji | ✅ | ✅ |
| **Cost Explorer / Budgets / CUR** | wspólny rachunek, wymiar „linked account”, cost categories | — (billing, także CB-only) | ❌ |
| **Resource Explorer** | multi-account search | ✅ | ✅ |
| **Systems Manager** | Explorer/Change Manager na org | ✅ | ✅ |
| **Marketplace / Private Marketplace** | wspólne licencje, prywatny katalog na OU | ✅ | ✅/❌ |
| **Service Quotas** | quota request template przy tworzeniu konta | ✅ | ❌ |
| **Artifact** | akceptacja umów za całą organizację | ✅ | ❌ |
| **Resilience Hub / DevOps Guru** | widok całej organizacji | ✅ | ✅ |
| **Security Incident Response** | ochrona całej organizacji | ✅ | ✅ |

**Wniosek:** Organizations to „warstwa sterowania”; poszczególne usługi (CloudTrail, Config, GuardDuty etc.) robią właściwą robotę. Na egzaminie pytanie zwykle brzmi „co włączyć”, a Organizations jest warunkiem wstępnym.

### Control Tower — co warto wiedzieć

- Wymaga **`ALL`** i jest zbudowany na Organizations.
- **Landing zone** = organizacja + konta bazowe + OU + polityki + logowanie.
- Domyślnie tworzy m.in. OU **Security** z kontami **log archive** i **audit**, opcjonalnie **Sandbox**.
- **Controls:** prewencyjne (SCP), detekcyjne (Config rules), proaktywne (CloudFormation hooks).
- **Account Factory** — tworzy nowe konta według szablonu, umieszcza w OU, aplikuje baseline.
- **Drift detection** — wykrywa ręczne zmiany poza Control Tower.
- **Account Factory for Terraform (AFT)** — alternatywa dla automatyzacji przez Terraform.

### IAM Identity Center — co warto wiedzieć

- **Account instance** (pojedyncze konto) vs **organization instance** (wymaga Organizations `ALL`).
- Centralne logowanie do wielu kont i aplikacji; permission sets zamiast ręcznych ról w każdym koncie.
- Identity source: directory Identity Center, AD (Managed Microsoft AD) lub zewnętrzny IdP (SAML/SCIM).
- Przypisania: użytkownik/grupa → permission set → konto (można wybierać wiele kont; nie myl z przypinaniem SCP do OU).

### RAM — co warto wiedzieć

- Współdzielenie zasobów: **VPC/subnety**, **Transit Gateway**, **Route 53 Resolver rules**, **on-demand capacity reservations**, License Manager, itd.
- W organizacji `ALL`: udział z **OU/organizacją** — uczestnicy **nie akceptują zaproszeń**; poza organizacją — handshake.
- Współdzielenie nie przenosi własności; właściciel nadal kontroluje zasób.
- VPC sharing: właściciel udostępnia subnet, uczestnicy tworzą w nim zasoby; nie mogą modyfikować subnetu.

---

## 9. Organizations włączone vs nie — pełne porównanie

### 9.1 Trzy stany

| Cecha | Konta standalone (bez Organizations) | Organizations `CONSOLIDATED_BILLING` | Organizations `ALL` |
|---|---|---|---|
| Rachunek | osobny dla każdego konta | jeden wspólny | jeden wspólny |
| Płatnik | każde konto samo | management account | management account |
| SCP/RCP | ❌ | ❌ | ✅ |
| OU/polityki | ❌ | ❌ | ✅ |
| Zaawansowane zarządzanie kontami (`CreateAccount`, delegated admin) | ręcznie | ❌ | ✅ |
| Org trail CloudTrail | ❌ | ❌ | ✅ |
| Config aggregator | ❌ | ❌ | ✅ |
| GuardDuty/Security Hub centralnie | ręcznie per konto | ❌ | ✅ |
| RAM sharing do OU/organizacji | ❌ (tylko zaproszenia) | ❌ | ✅ |
| IAM Identity Center org | ❌ | ❌ | ✅ |
| Control Tower | ❌ | ❌ | ✅ |
| StackSets service-managed | ❌ (tylko self-managed) | ❌ | ✅ |
| Service Catalog do OU | ❌ | ❌ | ✅ |
| Centralny root access | ❌ | ❌ | ✅ |
| Delegated admin | ❌ | ❌ | ✅ |
| Wspólne wolumeny/RI/SP | ❌ | ✅ | ✅ |

### 9.2 Co się zmienia dla pojedynczego konta po dołączeniu do organizacji

| Obszar | Przed | Po |
|---|---|---|
| Billing | własny rachunek i płatność | płaci management account; konto widzi swoje koszty informacyjnie |
| Uprawnienia | tylko IAM | IAM **i** SCP/RCP (mogą zabronić tego, co IAM pozwala) |
| Root user | pełna kontrola | SCP może zablokować roota; może zostać objęty centralized root access |
| Opuszczanie organizacji | — | możliwe tylko z rootem konta i jeśli SCP nie zabrania |
| Zamykanie konta | możliwe | możliwe, chyba że SCP zabrania |
| CloudTrail | własny trail | org trail obejmuje konto; członek nie może go modyfikować |
| Zasoby współdzielone | przez bucket policy/role | przez RAM (bez zaproszeń) |
| Tożsamość | osobni użytkownicy | IAM Identity Center z jednym loginem |
| Koszty | osobne | wspólne wolumeny, RI/SP, widok całej organizacji |
| Integracje usług | per konto | centralne (delegated admin, auto-enable) |

### 9.3 Co się **nie** zmienia

- **IAM nadal działa i nadal trzeba go używać** — SCP niczego nie przyzna.
- **Konta nadal są izolowane** — Organizations nie łączy VPC, nie współdzieli danych, nie przenosi uprawnień.
- **Nie ma automatycznego SSO** — to IAM Identity Center.
- **Nie ma automatycznie włączonego audytu/ochrony** — trzeba włączyć CloudTrail/Config/GuardDuty/Security Hub.
- **SCP nie chroni management account** — jedyna prawdziwa „dziura” w guardrailsach.
- **Organizations nie jest Control Tower** — Control Tower to gotowy landing zone na Organizations.

### 9.4 Billing — co dokładnie daje wspólny rachunek

- **Jeden rachunek** na organizację (a dokładnie: na „seller of record”/SOR — jeśli konta są w różnych SOR, może być więcej niż jeden rachunek).
- **Combined usage** — wspólne progi wolumenowe (np. S3, transfer), co obniża koszt.
- **RI i Savings Plans** — udziały w rabatach rozkładają się między konta; **sharing można wyłączyć** dla wybranego konta.
- **Cost Explorer / CUR / Budgets** — widok per konto (wymiar linked account), cost categories, tagi.
- **Member account** widzi tylko własne dane; po opuszczeniu organizacji **traci dostęp do historii Cost Explorer** z okresu członkostwa (dane zostają u management account).
- **Billing transfer** (nowsze) — przeniesienie odpowiedzialności billingowej między organizacjami (np. M&A); nie myl z „consolidated billing”.
- **Wspólny rachunek nie oznacza wspólnych uprawnień** — IAM pozostaje per konto; Organizations nie przenosi tożsamości.

### 9.5 Uprawnienia — jak myśleć o różnicy

Bez Organizations: `IAM policy` decyduje o wszystkim.
Z Organizations: `efektywne = IAM ∩ boundary ∩ SCP ∩ RCP` (+ session policy), a jawny deny wygrywa.

To dlatego w scenariuszach „administrator konta ma `AdministratorAccess`, ale nie może uruchomić instancji w regionie X” odpowiedź brzmi: **SCP na OU/root**.

---

## 10. Projektowanie OU — dobre praktyki

### Rekomendowany układ (wg AWS, uproszczony)

```
Root
├── Security OU          → log archive, security tooling (detekcja/reakcja)
├── Infrastructure OU    → network, shared services, backup
├── Workloads OU         → Prod / NonProd → per aplikacja/zespół
├── Sandbox OU           → eksperymenty, bez danych produkcyjnych
└── Suspended OU         → konta kwarantannowane (opcjonalnie)
```

Zasady:
1. **Grupuj według polityk, nie według org chartu.** OU to jednostka egzekwowania SCP, nie dział HR.
2. **Oddziel środowiska** (prod/non-prod) — najmniejszy blast radius.
3. **Wydziel Security i Infrastructure** — inne polityki, inne uprawnienia.
4. **Sandbox** — bez dostępu do danych produkcyjnych, ale z budżetem i limitami.
5. **Suspended/quarantine** — konto dochodzeniowe z bardzo restrykcyjnymi SCP.
6. **Policy staging OU** — testuj nowe SCP na wydzielonym OU przed rootem.
7. **Nie przesadzaj z głębokością** — 3–4 poziomy zwykle wystarczą; limit to 5.
8. **Wyjątki na poziomie konta** — jeśli jedno konto potrzebuje inaczej, przenieś je do innego OU, zamiast dodawać wyjątki w SCP.
9. **Nowe konta zawsze przez Account Factory** (Control Tower) — spójny baseline.
10. **Nazewnictwo i tagi kont** — ułatwiają raportowanie i automatyzację.

### Antywzorce

- Wszystko w root i „jakoś to będzie”.
- SCP allow-list wdrożony od razu na root bez testów (efekt: odcięcie IAM/STS).
- OU odpowiadające działom, a nie politykom.
- Trzymanie workloadów w management account.
- Brak log archive i centralnego CloudTrail.
- Polityki przypinane do pojedynczych kont zamiast do OU.
- Ignorowanie nowych kont (brak auto-enroll do GuardDuty/Config/Backup).

---

## 11. Nowości 2024–2026 (dla świadomości)

Egzamin SAP-C02 może jeszcze nie obejmować wszystkich tych funkcji, ale pytania „professional” często sprawdzają koncept, nie datę.

- **RCP (Resource Control Policies)** — GA pod koniec 2024; resource-centric guardrails, także dla podmiotów zewnętrznych.
- **Polityki deklaratywne** — EC2, Backup, Tag, AI opt-out, Chatbot; później Security Hub, Inspector, GuardDuty, Bedrock, S3, Shield NSD, upgrade rollout.
- **Centralized root access management** — zarządzanie rootem kont członkowskich z management account; usuwanie poświadczeń root i sesje uprzywilejowane; wymaga `ALL`.
- **Delegated administrator dla polityk Organizations** — delegowanie zarządzania politykami organizacji do konta członkowskiego (z granularnymi uprawnieniami).
- **Default security controls** — organizacje utworzone przez konsolę po **10 lipca 2026** dostają automatycznie SCP blokujący `organizations:LeaveOrganization` i `account:CloseAccount`; przez CLI/SDK/CloudFormation — nie, trzeba dodać ręcznie.
- **Billing transfer** — przenoszenie odpowiedzialności billingowej między organizacjami; wsparcie M&A.
- **Nowe polityki S3/GuardDuty/Security Hub/Inspector** — centralne włączanie i konfiguracja.
- **Management account w OU** — aktualna dokumentacja dopuszcza umieszczenie konta zarządzającego w OU; SCP/RCP i tak go nie obejmują. Nie opieraj na tym odpowiedzi, ale nie zdziw się, jeśli zobaczysz to w docs.

---

## 12. Typowe pułapki egzaminacyjne (lista do przejrzenia przed egzaminem)

1. **SCP nie przyznaje uprawnień** — jeśli IAM nie pozwala, SCP „Allow” nic nie da.
2. **SCP nie działa na management account** — workload w management account jest poza guardrailsami.
3. **SCP działa na root usera konta członkowskiego** — ale nie na root management account.
4. **SCP nie działa na service-linked roles** — nie zablokujesz w ten sposób usług zależnych.
5. **SCP nie działa bezpośrednio na resource-based policies** — do ochrony zasobów przed zewnętrznymi użyj RCP.
6. **RCP nie przyznaje uprawnień** i działa tylko na wybrane usługi; nie obejmuje management account.
7. **Allow-list wymaga usunięcia `FullAWSAccess`** — inaczej polityka „Allow” nic nie zmienia.
8. **Usunięcie `FullAWSAccess` bez zamiennika = awaria** — wszystkie akcje w kontach członkowskich failują.
9. **Nowe konto w OU dziedziczy SCP automatycznie** — nie trzeba nic robić; ale usługi (GuardDuty/Config) trzeba auto-włączyć.
10. **Konto należy do jednej organizacji** i jednego OU; przenoszenie tylko przez management account.
11. **Nie zmienisz management account** — trzeba nowej organizacji.
12. **Zaproszenia wygasają (15 dni)**, enable-all-features (90 dni); invited accounts muszą zaakceptować migrację.
13. **CB-only ≠ ALL** — brak SCP, integracji, delegated admin.
14. **Tag policy nie wymusza obecności tagów** — może egzekwować zgodność wartości dla wybranych typów zasobów, ale zasoby bez tagów nie są oceniane; „required tags” → SCP/Config.
15. **Backup policy ≠ backup włączony wszędzie automatycznie** — zasoby muszą pasować do reguł/tagów; sprawdź delegated admin.
16. **RAM sharing w organizacji nie wymaga zaproszeń** — poza organizacją wymaga.
17. **Cross-account dostęp nadal wymaga IAM role/resource policy** — Organizations tego nie zastępuje.
18. **SCP nie blokuje tworzenia zasobów „globalnych” przez region condition** — potrzebne wyjątki dla usług globalnych (IAM, STS, Route 53, CloudFront, Organizations, Support, Billing).
19. **SCP przypięta do root dotyczy wszystkich kont członkowskich** — testuj najpierw na OU.
20. **Delegated administrator podlega SCP** — to konto członkowskie.
21. **Konto po opuszczeniu organizacji traci historię Cost Explorer** z okresu członkostwa.
22. **RI/SP sharing można wyłączyć per konto** — brak rabatu na nowym koncie może mieć tę przyczynę.
23. **Organization trail nie może być modyfikowany przez konto członkowskie** — ale trzeba go włączyć i chronić SCP.
24. **Control Tower wymaga `ALL`** i „przejmuje” zarządzanie OU; ręczne zmiany powodują drift.
25. **Polityka deklaratywna na root dotyczy też management account** — inaczej niż SCP/RCP.

---

## 13. Scenariusze egzaminacyjne (pytanie → odpowiedź → dlaczego)

**1. 300 kont, chcesz wspólnych guardrails i automatycznego baseline dla nowych kont.**
→ **Control Tower + Account Factory + SCP na OU + org trail.** Organizations daje strukturę, Control Tower gotowy landing zone i automatyczne konta, SCP egzekwuje guardrails.

**2. Administratorzy kont nie mogą wyłączyć CloudTrail/Config/GuardDuty.**
→ **SCP na root/OU** blokujące `StopLogging`/`DeleteTrail`/`StopConfigurationRecorder`/`DeleteDetector` + **delegated admin** + **organization trail**. Sam org trail nie wystarczy, jeśli członek może go wyłączyć.

**3. Konto członkowskie nie może opuścić organizacji ani się zamknąć.**
→ **SCP `Deny organizations:LeaveOrganization` + `account:CloseAccount`** na root. (Od 07.2026 konsola zakłada to automatycznie.)

**4. Zewnętrzny partner ma dostęp tylko do jednego bucketu, ale nie do innych zasobów organizacji.**
→ **Bucket policy** dla roli partnera + **RCP** blokujący podmiotom spoza organizacji dostęp do pozostałych zasobów (np. `Deny` na `s3:*` gdy `aws:PrincipalOrgID` ≠ Twoje). SCP nie ograniczy partnera, bo działa tylko na principals w Twojej organizacji.

**5. Chcesz, by wszystkie nowe konta miały backup, GuardDuty i standardy Security Hub.**
→ **Delegated admin** dla Backup/GuardDuty/Security Hub + **auto-enable** + **backup policy** i **polityki deklaratywne** na OU/root.

**6. 500 użytkowników ma dostęp do 100 kont z jednym loginem i AD.**
→ **IAM Identity Center organization instance** + **permission sets** + przypisania grup; AD jako identity source. Nie buduj ról ręcznie w każdym koncie.

**7. 50 kont ma korzystać ze wspólnych subnetów VPC.**
→ **AWS RAM** — udział subnetami z OU/organizacją; uczestnicy nie akceptują zaproszeń; **wymaga `ALL`**.

**8. Nowe konto ma od razu mieć tagi, SCP i logowanie do centralnego bucketu.**
→ **Account Factory** (baseline przez StackSets/CloudFormation) + **SCP na OU** + **org trail**. Nowe konto dziedziczy SCP, ale CloudTrail/Config trzeba mieć skonfigurowane org-wide.

**9. Dział finansów chce osobno rozliczać działy, ale płatnik jest jeden.**
→ **Consolidated billing** + **cost allocation tags / cost categories / CUR**; raport per linked account. Faktury per konto są informacyjne, płaci management.

**10. Rabaty RI nie trafiają na nowe konto.**
→ Sprawdź **RI/SP sharing** (można wyłączyć per konto), **SOR** i region; ewentualnie typ rabatu. To nie jest problem SCP/OU.

**11. Wszystkie konta mają mieć zakaz tworzenia publicznych bucketów S3.**
→ **S3 Block Public Access** włączony na poziomie kont w całej organizacji + **polityka deklaratywna S3** + ewentualnie SCP/RCP; samo SCP na `s3:PutBucketPolicy` nie wystarczy dla wszystkich ścieżek.

**12. Musisz zablokować używanie niezatwierdzonych regionów, ale IAM/STS muszą działać.**
→ **SCP z `Deny` + `aws:RequestedRegion`** i `NotAction` dla usług globalnych; test na OU staging.

**13. Konto członkowskie ma `AdministratorAccess`, ale nie może uruchomić EC2 w eu-west-1.**
→ **SCP** przypięta do OU/root — filtr działa ponad IAM.

**14. Chcesz chronić dane przed dostępem z zewnątrz, także przez resource-based policies.**
→ **RCP** (resource perimeter) + **SCP** (identity perimeter) + `aws:PrincipalOrgID` / `aws:ResourceOrgID`.

**15. Management account ma workload i chcesz go objąć SCP.**
→ **Nie da się** — przenieś workload do konta członkowskiego; management account tylko do zadań administracyjnych.

**16. Firma przejęła inną firmę i chce wspólnego rachunku, ale osobnej organizacji.**
→ **Billing transfer** (przeniesienie odpowiedzialności billingowej) albo migracja kont do jednej organizacji; nie ma „łączenia organizacji”.

**17. Potrzebujesz SCP, ale organizacja była utworzona jako CB-only.**
→ **Enable all features** (invited accounts akceptują; jednokierunkowo).

**18. Nowe konto nie ma jeszcze SCP?**
→ Ma — dziedziczy z OU/root. Jeśli nie ma, sprawdź, czy nie jest w root i czy polityka jest przypięta.

---

## 14. Pytania testowe (z odpowiedziami)

**1.** Które stwierdzenie o SCP jest prawdziwe?
A) SCP przyznaje uprawnienia, jeśli IAM ich nie przyznaje.
B) SCP działa na konto zarządzające.
C) SCP ogranicza maksymalne uprawnienia, ale nie przyznaje dostępu.
D) SCP działa na service-linked roles.
**Odp: C.** SCP to filtr; management account i service-linked roles są poza nim.

**2.** Chcesz zablokować podmiotom spoza organizacji dostęp do bucketów S3 w kontach członkowskich. Co wybierzesz?
A) SCP na root
B) RCP na root/OU
C) Bucket policy w każdym buckecie
D) IAM policy w każdym koncie
**Odp: B.** RCP działa resource-centric i obejmuje podmioty zewnętrzne; SCP nie obejmuje external principals.

**3.** Organizacja ma tylko consolidated billing. Potrzebujesz SCP. Co robisz?
A) Tworzysz nową organizację
B) Włączasz all features (z akceptacją zaproszonych kont)
C) Włączasz SCP w ustawieniach konta
D) Nic — SCP są domyślnie włączone
**Odp: B.** Migracja CB→ALL jest jednokierunkowa, invited accounts muszą zaakceptować.

**4.** Konto w OU ma SCP allow-list (tylko S3 i EC2) i `AdministratorAccess`. Co może robić?
A) Wszystko, bo AdministratorAccess
B) Tylko S3 i EC2 (i to, co IAM pozwala)
C) Nic
D) Tylko IAM
**Odp: B.** SCP zawęża IAM; efektywne = część wspólna.

**5.** Nowe konta mają automatycznie dostawać guardrails i centralny CloudTrail. Najlepsze podejście?
A) Ręcznie konfigurować każde konto
B) Control Tower Account Factory + SCP na OU + organization trail
C) Tylko SCP na root
D) Tylko CloudTrail w management account
**Odp: B.** Account Factory automatyzuje baseline; SCP dziedziczą się z OU; org trail obejmuje nowe konta.

**6.** Chcesz zablokować root usera w kontach członkowskich, ale nie w management account. Co zrobić?
A) SCP z `Deny` na `aws:PrincipalArn` root — zadziała na member, nie na management
B) IAM policy w management account
C) RCP
D) Nie da się
**Odp: A.** SCP działa na root member account; management account jest wyłączony.

**7.** Chcesz ustandaryzować wartości tagu `Environment`, ale nie blokować tworzenia zasobów bez tagów. Co wybierzesz?
A) SCP `Deny` bez tagu
B) Tag policy
C) Config rule
D) RCP
**Odp: B.** Tag policy normalizuje; nie wymusza tagowania.

**8.** 100 kont ma współdzielić Transit Gateway. Co jest potrzebne?
A) Tylko peering VPC
B) AWS RAM — udział TGW z organizacją/OU
C) SCP `Allow`
D) CloudFront
**Odp: B.** RAM; w organizacji `ALL` bez zaproszeń.

**9.** Delegated administrator GuardDuty nie może włączyć ochrony w nowym koncie mimo uprawnień w GuardDuty. Co jest najbardziej prawdopodobną przyczyną?
A) SCP przypięta do OU, w którym jest delegated administrator, blokuje akcje GuardDuty
B) Brak RAM
C) CloudTrail jest wyłączony
D) Brak SCP
**Odp: A.** Delegated administrator to konto członkowskie, więc podlega SCP. (Trusted access również musi być włączony — bez niego rejestracja delegated admina by się nie udała.)

**10.** Konto członkowskie ma workload w management account i SCP na root. Czy workload jest chroniony?
A) Tak
B) Nie — SCP nie obejmuje management account
C) Tylko jeśli włączono RCP
D) Tylko po włączeniu Control Tower
**Odp: B.**

**11.** Chcesz, aby wszystkie nowe konta miały włączone GuardDuty i Security Hub. Co wybierzesz?
A) Ręcznie w każdym koncie
B) Delegated admin + auto-enable + polityki deklaratywne na OU/root
C) SCP `Allow guardduty:*`
D) CloudTrail
**Odp: B.**

**12.** Które stwierdzenie o OU jest prawdziwe?
A) Konto może być w wielu OU
B) OU można zagnieżdżać do 5 poziomów pod rootem
C) OU przyznaje uprawnienia
D) OU zastępuje IAM
**Odp: B.**

---

## 15. Ściąga — „jeśli w pytaniu..., to...”

| Sygnał w pytaniu | Odpowiedź |
|---|---|
| „centrally enforce”, „guardrails”, „prevent accounts from…” | SCP (na OU/root) |
| „block external principals”, „data perimeter”, „resource-based policy protection” | RCP |
| „new accounts automatically”, „baseline”, „landing zone” | Control Tower / Account Factory + SCP + org trail |
| „one login”, „SSO”, „permission sets” | IAM Identity Center organization instance |
| „share VPC/subnets/TGW/Resolver rules” | AWS RAM (org/OU share) |
| „central logs/audit all accounts” | CloudTrail organization trail (+ Config aggregator) |
| „threat detection all accounts” | GuardDuty delegated admin + auto-enable |
| „posture/standards all accounts” | Security Hub delegated admin + central configuration |
| „backup all accounts” | AWS Backup backup policy + delegated admin |
| „enforce tags” / „require tags” | SCP (warunek na tagi) lub Config `required-tags`; tag policy standaryzuje i może egzekwować wartości, ale nie wymusza otagowania |
| „standardize tags” | Tag policy |
| „opt out AI data collection” | AI services opt-out policy |
| „block public S3/EBS” | Block Public Access + polityki deklaratywne (S3/EC2) + ewentualnie SCP/RCP |
| „can’t delete trail/Config/GuardDuty” | SCP deny + delegated admin |
| „account can’t leave/close” | SCP deny `LeaveOrganization`/`CloseAccount` |
| „management account workload” | Nie rób tego — SCP nie ochroni; przenieś do member account |
| „consolidated billing, RI discounts” | Wspólny rachunek, sharing RI/SP, SOR, region |
| „one bill per department” | Consolidated billing + cost allocation tags/cost categories |
| „change management account” | Nie da się; nowa organizacja |
| „join existing accounts” | Zaproszenie (handshake 15 dni), weryfikacja e-mail, root dostęp |
| „SCP does not grant” | Zawsze prawda — IAM musi przyznać |

---

## 16. Mapowanie na SAP-C02 (domena 1)

- **Organizational complexity:** multi-account strategy, OU design, SCP/RCP guardrails, Control Tower, IAM Identity Center, RAM, central logging.
- **Security:** delegated admin, org trail, Config aggregator, GuardDuty/Security Hub/Inspector, centralized root access, data perimeter.
- **Cost:** consolidated billing, RI/SP sharing, cost allocation, budgets, CUR, Cost Categories.
- **Automation/migration:** Account Factory, StackSets, Service Catalog, Service Quotas templates.

---

## 17. Źródła (docs)

- AWS Organizations User Guide — koncepcje i feature sets: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html
- Enabling all features: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_support-all-features.html
- SCP: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html
- SCP syntax: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_syntax.html
- RCP: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html
- Authorization policies (SCP vs RCP): https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_authorization_policies.html
- Quotas: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_reference_limits.html
- Usługi integrujące się: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_services_list.html
- Consolidated billing: https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html
- Default security controls: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_security_default_controls.html
- Best practices management account: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices_mgmt-acct.html

---

*Powodzenia na egzaminie. Najczęstszy błąd na SAP-C02 to odpowiadanie „IAM policy”, gdy pytanie wymaga „SCP na OU” — i odwrotnie.*
