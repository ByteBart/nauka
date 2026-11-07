# CloudWatch vs GuardDuty vs AWS Config vs Hooks — notatki do egzaminu SAP-C02

> **Zakres:** cztery mechanizmy, które na egzaminie Professional łatwo pomylić: **monitoring**, **detekcja zagrożeń**, **zgodność konfiguracji** i **proaktywne kontrole**.
> **Stan wiedzy:** listopad 2026.
> **Uwaga do „hooks”:** w AWS nie ma usługi o nazwie „Hooks”. Najczęściej chodzi o **CloudFormation Hooks** — proaktywne kontrole uruchamiane przed utworzeniem/aktualizacją zasobu. Na nich opierają się **AWS Control Tower proactive controls**. Osobno istnieje **AWS Config proactive evaluation** (ocena przed wdrożeniem, ale **bez blokowania**). W notatkach rozdzielam te trzy rzeczy, bo egzamin lubi je mieszać.
> **Klucz:** CloudWatch = „czy działa i jak”, GuardDuty = „czy to atak”, Config = „czy konfiguracja jest zgodna i co się zmieniło”, Hooks = „czy nie wdrożyć czegoś niezgodnego przez CloudFormation”.

---

## 0. Najkrótsza możliwa odpowiedź

| Mechanizm | Kategoria | Jedno zdanie |
|---|---|---|
| **CloudWatch** | observability / monitoring | Zbiera metryki i logi, robi alarmy, dashboardy i automatyczne reakcje operacyjne. |
| **GuardDuty** | threat detection | Ciągle analizuje logi i przepływy, wykrywa podejrzane i złośliwe zachowania, generuje **findings**. |
| **AWS Config** | configuration compliance / audit | Rejestruje konfigurację zasobów i jej historię, ocenia zgodność regułami, może remediować. |
| **CloudFormation Hooks** | proactive governance | Sprawdza właściwości zasobów **przed** wdrożeniem przez CloudFormation i może **zablokować** operację. |

**Czego żaden z nich nie robi:**
- CloudWatch **nie wykrywa ataków** (to nie GuardDuty) i **nie śledzi konfiguracji** (to nie Config).
- GuardDuty **nie naprawia** i **nie ocenia zgodności** — tylko wykrywa i raportuje findings.
- Config **nie blokuje** wdrożenia — ocenia po zmianie (detekcyjnie) albo przed nią (proaktywnie), ale bez preventive block.
- Hooks **nie działają poza CloudFormation** — konsola/API/SDK tworzące zasób z pominięciem CFN ich nie uruchomią.

---

## 1. Mapa pojęć — kto za co odpowiada

```
Co chcę wiedzieć?                                  Wybieram
─────────────────                                  ────────
Czy zasób działa wydajnie?                         CloudWatch
Czy aplikacja ma błędy / wysokie CPU / latency?    CloudWatch (+ X-Ray / Application Signals)
Czy ktoś się włamał / kopie krypto / kradnie dane? GuardDuty
Czy konfiguracja jest zgodna z polityką?           AWS Config
Co zmieniło się w konfiguracji i kiedy?            AWS Config (+ CloudTrail)
Czy zapobiec wdrożeniu złego zasobu przez CFN?     CloudFormation Hooks
Czy zablokować API niezależnie od narzędzia?       SCP / IAM
Czy audytować wywołania API (kto, co, kiedy)?      CloudTrail
Czy zebrać findings z wielu usług?                 Security Hub
Czy znaleźć podatności?                            Inspector
Czy znaleźć dane wrażliwe w S3?                    Macie
Czy zbadać incydent?                               Detective
Czy sprawdzić dostęp z zewnątrz?                   IAM Access Analyzer
```

**Trzy osie, które trzeba rozróżnić:**
1. **Moment działania:** przed wdrożeniem (proactive) vs po zmianie (detective) vs ciągły runtime (monitoring/threat).
2. **Przedmiot:** metryki/logi vs zachowanie/aktywność vs konfiguracja vs właściwości zasobu w szablonie.
3. **Efekt:** alarm/finding/compliance status vs blokada wdrożenia.

---

## 2. CloudWatch — monitoring i observability

### Co to jest

Amazon CloudWatch to usługa **monitorowania** zasobów i aplikacji w czasie rzeczywistym: metryki, logi, alarmy, dashboardy, a także funkcje APM (Application Signals), Synthetics, RUM, SLO i analiza logów.

### Co obejmuje

| Element | Co robi | Egzaminowo |
|---|---|---|
| **Metrics** | serie czasowe z wymiarami (namespace, dimensions), standardowe i customowe; rozdzielczość 1 s (high resolution) | domyślne metryki AWS nie zawierają pamięci/dysku EC2 — potrzebny **CloudWatch agent** |
| **Alarms** | monitorują metrykę względem progu; stany `OK`/`ALARM`/`INSUFFICIENT_DATA`; akcje: SNS, Auto Scaling, EC2 (stop/terminate/reboot/recover), SSM | alarm może być **composite**; **anomaly detection**; „treat missing data” |
| **Logs** | log groups/streams; retencja konfigurowalna (domyślnie **bezterminowo**); Logs Insights (SQL/PPL); metric filters; subscription filters | **metric filter** zamienia log na metrykę; **subscription filter** strumieniuje logi do Lambda/Kinesis/Firehose |
| **Dashboards** | widoki metryk/logów; cross-account i cross-region | nie służą do alarmowania same w sobie |
| **Application Signals / APM** | automatyczne KPI aplikacji (latency, errors, requests), SLO | nowsze, ale egzamin może pytać o „APM” |
| **Synthetics** | canaries — skrypty symulujące użytkownika | monitorowanie endpointów/API |
| **RUM** | dane z realnych sesji użytkowników | frontend |
| **Cross-account observability** | centralne konto monitorujące + source accounts; link przez Organizations | odpowiednik „centralnego dashboardu” dla metryk/logów/traces |
| **Agent** | metryki systemowe (memory, disk, swap, processes), logi, traces z EC2/on-prem | brak agenta = brak metryk pamięci/dysku |

### Kiedy używać

- „Monitoruj CPU/ pamięć/ dysk”, „alarmuj gdy…”, „dashboard operacyjny”, „zbieraj logi aplikacji”, „analizuj logi zapytaniami”, „SLO/APM”, „canary checks”.

### Czego nie robi

- **Nie wykrywa zagrożeń** — anomalia metryki to nie threat detection.
- **Nie śledzi konfiguracji zasobów** — to Config.
- **Nie jest audit logiem API** — to CloudTrail (choć CloudTrail może wysyłać logi do CloudWatch Logs).
- **Nie przechowuje logów z EC2 automatycznie** — trzeba zainstalować agenta lub skonfigurować usługę.

### Klasyczne pułapki

- **CloudWatch vs CloudTrail:** CloudTrail = „kto wykonał jakie API”; CloudWatch = „jak system się zachowuje”.
- **CloudWatch vs Config:** Config = konfiguracja i historia; CloudWatch = metryki/logi.
- **Metryki domyślne EC2** nie mają pamięci/dysku — wymagany agent.
- **Log retention** domyślnie „Never expire” — koszt rośnie; ustawiaj retencję.
- **Alarm bez metryki** = `INSUFFICIENT_DATA`; sprawdź namespace/wymiary/region.
- **High-resolution metrics** kosztują więcej; nie każda usługa je publikuje.

### Przykład alarmu (CLI)

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "HighCPU" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:sns:eu-central-1:111122223333:ops
```

---

## 3. GuardDuty — detekcja zagrożeń

### Co to jest

Amazon GuardDuty to **threat detection service**: ciągle monitoruje, analizuje i przetwarza dane źródłowe oraz logi, używając threat intelligence, ML i reguł, i generuje **findings** o potencjalnie złośliwej aktywności.

### Źródła danych

**Fundamentalne (włączane automatycznie):**
- CloudTrail **management events**,
- **VPC Flow Logs**,
- **DNS logs**.

**Protection plans (rozszerzone):**
- **S3 Protection** — dostęp do danych S3 (CloudTrail data events),
- **EKS Protection** — audit logi EKS,
- **RDS Protection** — loginy do Aurora/RDS,
- **Lambda Protection** — network activity Lambda,
- **Runtime Monitoring** — zdarzenia OS-level na EKS/EC2/ECS (agent),
- **Malware Protection** — skanowanie EBS, S3 (nowe obiekty), AWS Backup,
- **AI Protection** — zagrożenia dla workloadów AI (Bedrock/SageMaker),
- **Extended Threat Detection** — wieloetapowe ataki łączące wiele źródeł.

### Co produkuje

- **Findings** z severity, typem, zasobem, informacjami o zagrożeniu; retencja **90 dni** (eksport do S3/Security Hub możliwy).
- Integracje: **EventBridge** (reakcja near-real-time), **Security Hub** (agregacja), **Detective** (śledztwo).
- **Suppression rules / filters**, sample findings do testów.

### Multi-account

- Przez **AWS Organizations** (zalecane) lub legacy invitations.
- **Delegated administrator** + **auto-enable** dla nowych kont.
- GuardDuty jest **regionalne** — włączasz per region.

### Kiedy używać

- „Wykryj compromised credentials”, „crypto mining”, „malware”, „anomalous login”, „data exfiltration”, „suspicious IP/DNS”, „atak na S3/EKS/RDS”.
- Reakcja: finding → EventBridge → Lambda/SSM/SOAR lub Security Hub.

### Czego nie robi

- **Nie naprawia** i nie blokuje — to nie SCP/Config remediation.
- **Nie ocenia zgodności** (Config) i **nie skanuje podatności** (Inspector).
- **Nie klasyfikuje danych** (Macie).
- **Nie robi śledztwa** (Detective) — choć nowe „Investigation” to wsparcie AI, nie zastępuje Detective.

### Pułapki

- GuardDuty **nie wymaga włączania CloudTrail/VPC Flow Logs** — sam pobiera fundamentalne źródła; nie myl z „muszę włączyć CloudTrail, żeby GuardDuty działało”.
- Findings są **regionalne**; multi-region = włącz w każdym regionie.
- **30-dniowy trial**; potem koszt zależy od wolumenu.
- GuardDuty ≠ Security Hub: GuardDuty wykrywa, Security Hub agreguje i ocenia standards.
- GuardDuty ≠ Inspector: Inspector szuka podatności, GuardDuty — aktywnych zagrożeń.
- **Brak automatycznej remediacji** — potrzebny EventBridge/Lambda/SSM.

---

## 4. AWS Config — zgodność i historia konfiguracji

### Co to jest

AWS Config daje **szczegółowy widok konfiguracji zasobów**: jakie zasoby istnieją, jak są skonfigurowane, jak są powiązane i **jak zmieniały się w czasie**. Używa **configuration items**, **configuration history**, **snapshots** i **reguł** do oceny zgodności.

### Elementy

| Element | Co robi |
|---|---|
| **Configuration recorder** | rejestruje wybrane typy zasobów; bez recordera nie ma danych |
| **Delivery channel** | wysyła snapshoty/historię do **S3** i powiadomienia do **SNS** |
| **Configuration item (CI)** | migawka konfiguracji zasobu w danym momencie |
| **Configuration history** | zmiany konfiguracji w czasie; relacje między zasobami |
| **Rules** | oceniają zgodność: AWS managed, custom Lambda, Guard; wyniki `COMPLIANT`/`NON_COMPLIANT`/`ERROR`/`NOT_APPLICABLE` |
| **Triggers** | configuration change, periodic, hybrid |
| **Evaluation modes** | **detective** (po wdrożeniu) i **proactive** (przed wdrożeniem, bez blokowania) |
| **Conformance packs** | zestaw reguł + remediacji wdrażany jako całość (także org-wide) |
| **Remediation** | automatyczna (SSM Automation) lub ręczna; **po** wykryciu niezgodności |
| **Aggregators** | centralny widok multi-account i multi-region |
| **Advanced queries** | zapytania SQL-like po bieżącym stanie konfiguracji |

### Kiedy używać

- „Czy zasoby są zgodne z polityką?”, „jaka była konfiguracja 1 stycznia?”, „co się zmieniło?”, „które zasoby są niezgodne?”, „wdroż poprawkę automatycznie”, „raport zgodności dla audytu”.

### Czego nie robi

- **Nie blokuje** tworzenia zasobu — reguły oceniają po zmianie; proactive evaluation też **nie blokuje** (tylko ocenia).
- **Nie wykrywa ataków** — to GuardDuty.
- **Nie monitoruje metryk/logów** — to CloudWatch.
- **Nie jest CloudTrail** — CloudTrail rejestruje wywołania API (kto/co), Config rejestruje stan konfiguracji.
- **Nie jest drift detection CloudFormation** — drift porównuje stack do szablonu; Config śledzi faktyczną konfigurację i zgodność.

### Pułapki

- **Config jest regionalne**; multi-region wymaga włączenia w każdym regionie + aggregator.
- **Recorder musi być włączony** i musi obejmować dany typ zasobu — inaczej reguła nie ma czego oceniać.
- **Reguły są detekcyjne**; „prevent” to nie Config, tylko SCP/Hooks.
- **Remediation jest po fakcie** — może działać automatycznie, ale nie zapobiega utworzeniu zasobu.
- **Config ≠ Security Hub**: Config ocenia zgodność, Security Hub agreguje findings/standards.
- **Koszt** zależy od liczby configuration items i reguł.

---

## 5. Hooks — proaktywne kontrole CloudFormation

### Co to jest

**CloudFormation Hooks** to mechanizm, który pozwala sprawdzić konfigurację zasobów **przed** ich utworzeniem/aktualizacją (a także przed usunięciem, zależnie od hooka). Hook może:
- **fail** — przerwać operację i nie wdrożyć niezgodnych zasobów,
- **warn** — tylko ostrzec i przepuścić.

Hooki są implementowane m.in. jako:
- **AWS Control Tower proactive controls** (gotowe kontrole),
- **Guard rules** (CloudFormation Guard — policy-as-code),
- **Lambda functions**,
- **custom hooks** (CloudFormation CLI, registry).

### Gdzie działają

- Podczas operacji **CloudFormation** (create/update/delete stacka) oraz dla zasobów **AWS Cloud Control API**.
- **Nie** działają, gdy zasób jest tworzony bezpośrednio przez konsolę, API, SDK lub inne IaC (np. Terraform) — chyba że dane narzędzie też korzysta z hooków/guard.
- Control Tower proactive controls: `preCreate`/`preUpdate`, ograniczenia m.in. **secure strings/secrets nie są rozwiązywane** przed hookiem.

### Do czego służą

- Wymuszenie **dozwolonych typów instancji**, **szyfrowania**, **IMDSv2**, **braku publicznych bucketów**, **tagów**, **backupów RDS**, **approved AMI** — zanim zasób powstanie.
- „Shift-left” compliance: błąd w pipeline zamiast niezgodnego zasobu w chmurze.

### Hooks vs Config proactive evaluation

| | CloudFormation Hooks | Config proactive evaluation |
|---|---|---|
| Kiedy | przed operacją CFN | przed wdrożeniem (API/ocena) |
| Czy blokuje? | **Tak** (fail) lub ostrzega | **Nie** — tylko ocenia |
| Zakres | zasoby CFN/Cloud Control | wybrane typy zasobów/reguły |
| Implementacja | Lambda/Guard/Control Tower/custom | reguła Config w trybie `PROACTIVE` |
| Typowe pytanie | „zapobiec wdrożeniu przez CFN” | „ocenić, czy byłoby zgodne” |

### Hooks vs SCP vs Config

| Mechanizm | Moment | Co widzi | Czy blokuje |
|---|---|---|---|
| **SCP** | każde wywołanie API | akcję i warunki (np. region, principal) | tak — ale nie zna właściwości zasobu |
| **Hooks** | operacja CFN | właściwości zasobu z szablonu | tak — ale tylko CFN/Cloud Control |
| **Config** | po zmianie (lub proactive) | konfigurację zasobu | nie |
| **IAM** | każde wywołanie API | akcję/zasób | tak — ale bez logiki compliance |

### Pułapki

- „Hooks” to **nie usługa** — nie ma „włącz AWS Hooks”; włączasz hooki/CFN Hooks lub Control Tower proactive controls.
- **Hooks nie obejmują zasobów spoza CloudFormation** — to najczęstsza pułapka.
- **Hooks nie monitorują runtime** — to nie GuardDuty/CloudWatch.
- **Hooks nie remediują** — mogą tylko zablokować/ostrzec.
- **Control Tower proactive controls** to hooks, ale nie wszystkie hooki są Control Tower controls.
- **Config proactive evaluation nie blokuje** — nie myl z hookami.

---

## 6. Główna tabela: CloudWatch vs GuardDuty vs Config vs Hooks

| Kryterium | **CloudWatch** | **GuardDuty** | **AWS Config** | **Hooks (CFN)** |
|---|---|---|---|---|
| **Kategoria** | observability | threat detection | configuration compliance / audit | proactive governance |
| **Pytanie, na które odpowiada** | „Jak działa?” | „Czy to atak?” | „Czy zgodne? Co się zmieniło?” | „Czy pozwolić na wdrożenie?” |
| **Dane** | metryki, logi, traces (APM) | CloudTrail mgmt, VPC Flow, DNS, S3/EKS/RDS/Lambda, runtime | configuration items, historia, snapshoty | właściwości zasobów z szablonu/operacji CFN |
| **Moment** | ciągły (near-real-time) | ciągły | po zmianie + periodycznie; proactive opcjonalnie | przed utworzeniem/aktualizacją |
| **Wynik** | alarm, dashboard, log, SLO | finding (severity, typ) | compliance status, historia, raport | PASS/FAIL/WARN; blokada stacka |
| **Czy blokuje?** | nie | nie | nie | **tak** (fail) |
| **Czy naprawia?** | akcje alarmu (Auto Scaling, SSM, EC2) | nie | tak — remediation (SSM) | nie |
| **Zasięg** | regionalny; cross-account observability | regionalny; org delegated admin | regionalny; aggregator multi-account/region | per konto/region; Control Tower na OU |
| **Włączanie** | domyślnie metryki/logi usług; agent dla EC2 | włączasz usługę per region; protection plans | włączasz recorder per region | rejestrujesz/aktywujesz hooki; CT controls |
| **Typowe pytanie** | „monitoruj/alarmuj” | „wykryj zagrożenie” | „zgodność/audyt/historia” | „zapobiec wdrożeniu” |
| **Nie myl z** | CloudTrail, Config | Security Hub, Inspector | CloudTrail, CFN drift | Config, SCP |

**Skrót do zapamiętania:**
- **CloudWatch = metryki i logi → alarm.**
- **GuardDuty = logi i przepływy → finding.**
- **Config = konfiguracja → compliance status + historia.**
- **Hooks = szablon → blokada wdrożenia.**

---

## 7. Preventive vs proactive vs detective vs responsive

To rozróżnienie bardzo często pojawia się w pytaniach SAP-C02.

| Kategoria | Cel | Przykłady |
|---|---|---|
| **Preventive** (zapobiega) | zablokować akcję/uprawnienie | **SCP**, IAM policies, permissions boundary, bucket policy, S3 Block Public Access |
| **Proactive** (sprawdza przed wdrożeniem) | wykryć niezgodność zanim zasób powstanie | **CloudFormation Hooks**, **Control Tower proactive controls**, CloudFormation Guard w CI/CD, Config **proactive** evaluation (ocena bez blokady) |
| **Detective** (wykrywa po fakcie) | wykryć niezgodność/zagrożenie | **AWS Config rules**, **GuardDuty**, **Security Hub**, CloudTrail, Inspector, Macie, Access Analyzer |
| **Responsive** (reaguje) | automatyczna reakcja/naprawa | **Config remediation (SSM)**, EventBridge → Lambda/SSM, Security Hub automated response, SOAR |

**Egzaminowe pytania:**
- „Chronić przed utworzeniem niezgodnego zasobu przez CloudFormation” → **Hooks / Control Tower proactive controls**.
- „Wykryć niezgodność po wdrożeniu” → **Config rules**.
- „Zablokować API niezależnie od narzędzia” → **SCP/IAM**.
- „Wykryć atak” → **GuardDuty**.
- „Zmierzyć wydajność” → **CloudWatch**.

---

## 8. Sąsiedzi, których nie wolno mylić

| Usługa | Rola | Czym nie jest |
|---|---|---|
| **CloudTrail** | audit wywołań API: kto, co, kiedy, skąd | nie monitoruje metryk, nie ocenia konfiguracji |
| **Security Hub (CSPM)** | agreguje findings i ocenia standards (CIS, PCI itd.) | nie wykrywa sam z siebie ataków — dostaje findings z GuardDuty/Inspector/Macie/Config |
| **Inspector** | skanowanie podatności (EC2, ECR, Lambda) | nie wykrywa aktywnych ataków (GuardDuty) |
| **Macie** | klasyfikacja danych wrażliwych w S3 | nie wykrywa ataków na inne zasoby |
| **Detective** | śledztwo i wizualizacja incydentu | nie zastępuje GuardDuty |
| **IAM Access Analyzer** | wykrywanie dostępu z zewnątrz (bucket policy, role trust itd.) | nie monitoruje ruchu |
| **EventBridge** | routing eventów/findings | nie jest kolejką ani monitoringiem |
| **Audit Manager** | zbieranie dowodów audytowych | nie jest Config rules |
| **CloudFormation drift** | porównanie stacka do szablonu | nie jest historią konfiguracji jak Config |
| **Trusted Advisor** | rekomendacje best practices | nie jest automatyczną regułą compliance |
| **X-Ray / Application Signals** | tracing/APM | nie jest monitoringiem infrastruktury jak CloudWatch metrics |

---

## 9. Jak się łączą — wzorce integracji

- **CloudTrail → CloudWatch Logs → metric filter/alarm**: klasyczny sposób na alert o zdarzeniu API (np. root login, zmiana IAM).
- **GuardDuty → EventBridge → Lambda/SSM**: automatyczna reakcja na finding (np. izolacja instancji, rotacja kluczy).
- **GuardDuty → Security Hub**: agregacja i priorytetyzacja findings.
- **Config → EventBridge → Lambda/SSM**: reakcja na zmianę compliance; **Config remediation** może użyć SSM Automation.
- **Config → Security Hub**: Config rules jako źródło findings/compliance.
- **CloudWatch alarm → SNS/EventBridge**: powiadomienia i automatyzacja operacyjna.
- **CloudWatch → Auto Scaling**: alarm-based scaling.
- **Hooks → CFN stack fail**: blokada wdrożenia; błąd widoczny w pipeline.
- **Control Tower**: preventive (SCP), detective (Config), proactive (Hooks) — trzy typy controls w jednym landing zone.
- **CloudWatch cross-account observability ↔ Organizations**: centralne konto monitorujące.
- **Config aggregator ↔ Organizations**: centralny widok zgodności; delegated admin.
- **GuardDuty delegated admin ↔ Organizations**: centralne włączanie i findings.

---

## 10. Typowe pułapki egzaminacyjne

1. **CloudWatch ≠ GuardDuty** — alarm na CPU nie jest detekcją ataku.
2. **GuardDuty ≠ Security Hub** — GuardDuty wykrywa, Security Hub agreguje.
3. **GuardDuty ≠ Inspector** — zagrożenia vs podatności.
4. **GuardDuty ≠ Macie** — aktywność vs klasyfikacja danych.
5. **Config ≠ CloudTrail** — konfiguracja vs wywołania API.
6. **Config ≠ CloudFormation drift** — historia/faktyczna konfiguracja vs porównanie do szablonu.
7. **Config nie blokuje** — nawet proactive evaluation nie blokuje.
8. **Hooks blokują, ale tylko CloudFormation/Cloud Control** — konsola/API/Terraform poza zasięgiem.
9. **Config proactive evaluation ≠ Hooks** — pierwsze ocenia, drugie może zablokować.
10. **SCP ≠ Hooks** — SCP działa na API i nie zna właściwości zasobu; Hooks znają właściwości, ale tylko CFN.
11. **Control Tower proactive controls to Hooks**; preventive to SCP, detective to Config.
12. **Metryki pamięci/dysku EC2 wymagają agenta** — nie ma ich domyślnie.
13. **Logi CloudWatch domyślnie bezterminowe** — koszt; ustaw retencję.
14. **Alarm bez danych** = `INSUFFICIENT_DATA` — sprawdź metrykę/region/wymiary.
15. **GuardDuty jest regionalne** — włącz w każdym regionie; findings 90 dni.
16. **Config jest regionalne** — aggregator do centralnego widoku.
17. **GuardDuty nie wymaga ręcznego włączania CloudTrail/VPC Flow** — sam je pobiera.
18. **Security Hub nie zastępuje GuardDuty/Config** — to agregator.
19. **Hooks nie remediują** — tylko blokują/ostrzegają.
20. **CloudWatch cross-account observability ≠ Config aggregator ≠ GuardDuty delegated admin** — trzy różne mechanizmy centralizacji.

---

## 11. Scenariusze egzaminacyjne

**1. Wykryć skompromitowane klucze IAM.**
→ **GuardDuty** (findings o compromised credentials); reakcja przez EventBridge → Lambda/SSM.

**2. Alarm, gdy CPU > 80% przez 10 minut.**
→ **CloudWatch alarm** + SNS/Auto Scaling.

**3. Sprawdzić, kto zmienił security group i jaka była konfiguracja wcześniej.**
→ **AWS Config** (configuration history) + **CloudTrail** (kto wykonał API).

**4. Zablokować wdrożenie publicznego bucketu S3 przez CloudFormation.**
→ **CloudFormation Hooks** (fail) lub **Control Tower proactive control**; alternatywnie **SCP** blokujący publiczne `s3:PutBucketPolicy`/`s3:PutBucketAcl`, ale SCP działa grubo i nie sprawdza wszystkich właściwości bucketu.

**5. Automatycznie naprawić niezgodny zasób (np. bucket S3 bez Block Public Access).**
→ **Config rule** + **remediation** (SSM Automation) — po wykryciu.

**6. Centralny dashboard incydentów bezpieczeństwa z wielu kont.**
→ **Security Hub** + **GuardDuty** (delegated admin) + **Config** (aggregator).

**7. Zeskanować EC2/ECR pod kątem podatności.**
→ **Inspector**.

**8. Znaleźć dane osobowe/wrażliwe w S3.**
→ **Macie**.

**9. Alert o logowaniu root usera.**
→ **CloudTrail → CloudWatch Logs → metric filter/alarm** (lub EventBridge rule na CloudTrail); GuardDuty też może flagować root activity, ale klasyczna odpowiedź to CloudWatch/Trail.

**10. Wymusić dozwolone typy instancji w szablonach CloudFormation.**
→ **Hooks** (fail) lub **Control Tower proactive control**.

**11. Widok zgodności 100 kont w jednym miejscu.**
→ **Config aggregator** (+ delegated admin); raportowanie przez Security Hub/Audit Manager.

**12. Wykryć kopanie kryptowalut na EC2.**
→ **GuardDuty** (CryptoCurrency findings).

**13. Zbadać, jak doszło do incydentu.**
→ **Detective** (po findingach z GuardDuty).

**14. Wykryć zmianę konfiguracji poza CloudFormation (drift).**
→ **Config** (configuration history) lub **CloudFormation drift detection** — zależnie od pytania: „historia/konfiguracja” → Config; „stack vs template” → drift.

**15. Egzekwować tagi przy tworzeniu zasobów przez CFN.**
→ **Hooks** (property-level) + **Config rule** (detekcja) + ewentualnie SCP; tag policy tylko normalizuje.

**16. Wykryć, że bucket policy pozwala na dostęp z zewnątrz.**
→ **IAM Access Analyzer**; ewentualnie Config rule / Macie (dane).

---

## 12. Pytania testowe (z odpowiedziami)

**1.** Który mechanizm wykrywa skompromitowane poświadczenia AWS?
A) CloudWatch
B) GuardDuty
C) AWS Config
D) CloudFormation Hooks
**Odp: B.** GuardDuty analizuje CloudTrail/VPC Flow/DNS i generuje findings.

**2.** Który mechanizm rejestruje historię konfiguracji zasobów?
A) CloudWatch Logs
B) GuardDuty
C) AWS Config
D) Hooks
**Odp: C.** Config przechowuje configuration items/historię.

**3.** Chcesz zablokować wdrożenie niezgodnego zasobu przez CloudFormation. Co wybierzesz?
A) Config rule
B) CloudFormation Hook
C) CloudWatch alarm
D) GuardDuty
**Odp: B.** Hook może failować operację; Config tylko ocenia.

**4.** Który mechanizm NIE blokuje wdrożenia?
A) SCP
B) Hook
C) Config rule
D) IAM policy
**Odp: C.** Config jest detekcyjny (nawet proactive evaluation nie blokuje).

**5.** Metryki pamięci i dysku EC2 są niedostępne. Dlaczego?
A) trzeba włączyć GuardDuty
B) trzeba zainstalować CloudWatch agent
C) trzeba włączyć Config
D) trzeba użyć Hooks
**Odp: B.** Domyślne metryki EC2 ich nie obejmują.

**6.** Co robi GuardDuty?
A) ocenia zgodność konfiguracji
B) wykrywa zagrożenia i generuje findings
C) naprawia niezgodne zasoby
D) blokuje wdrożenia CFN
**Odp: B.**

**7.** Który mechanizm jest proaktywny i może przerwać stack?
A) Config rule
B) CloudFormation Hook
C) GuardDuty
D) CloudWatch alarm
**Odp: B.**

**8.** Który mechanizm agreguje findings z GuardDuty, Inspector i Config?
A) CloudWatch
B) Security Hub
C) CloudTrail
D) Hooks
**Odp: B.**

**9.** Do wykrywania podatności w EC2/ECR służy:
A) GuardDuty
B) Inspector
C) Config
D) Macie
**Odp: B.**

**10.** Config proactive evaluation:
A) blokuje wdrożenie
B) ocenia przed wdrożeniem, ale nie blokuje
C) zastępuje Hooks
D) wykrywa ataki
**Odp: B.**

**11.** Który mechanizm jest preventive (blokuje API niezależnie od narzędzia)?
A) SCP
B) Config
C) GuardDuty
D) CloudWatch
**Odp: A.**

**12.** Control Tower proactive controls są implementowane jako:
A) SCP
B) CloudFormation Hooks
C) Config rules
D) GuardDuty findings
**Odp: B.**

**13.** Który mechanizm jest audit logiem wywołań API?
A) CloudWatch
B) CloudTrail
C) Config
D) GuardDuty
**Odp: B.**

**14.** Alarm CloudWatch przeszedł w `INSUFFICIENT_DATA`. Co sprawdzić?
A) uprawnienia GuardDuty
B) metrykę, wymiary, region i okres
C) hooki
D) Config recorder
**Odp: B.**

**15.** Który mechanizm pozwala automatycznie naprawić niezgodny zasób?
A) GuardDuty
B) Config remediation
C) CloudWatch
D) Hooks
**Odp: B.** Config remediation (SSM Automation) działa po wykryciu.

---

## 13. Ściąga — „jeśli w pytaniu..., to...”

| Sygnał | Odpowiedź |
|---|---|
| „monitor CPU/memory/disk”, „alarm”, „dashboard” | CloudWatch |
| „logi aplikacji”, „Logs Insights”, „metric filter” | CloudWatch Logs |
| „APM/latency/error rate/SLO” | CloudWatch Application Signals / X-Ray |
| „compromised credentials”, „crypto mining”, „malware” | GuardDuty |
| „threat detection”, „findings” | GuardDuty |
| „configuration history”, „co się zmieniło” | AWS Config (+ CloudTrail) |
| „compliance”, „noncompliant resources” | AWS Config rules |
| „auto-remediate noncompliant” | Config remediation |
| „multi-account compliance view” | Config aggregator |
| „block noncompliant CFN deployment” | CloudFormation Hooks |
| „Control Tower proactive control” | CloudFormation Hooks |
| „prevent API action” | SCP/IAM |
| „who did what API call” | CloudTrail |
| „aggregate findings/standards” | Security Hub |
| „vulnerabilities” | Inspector |
| „sensitive data in S3” | Macie |
| „investigate incident” | Detective |
| „external access” | IAM Access Analyzer |
| „hooks” | CloudFormation Hooks (nie usługa) |
| „Config proactive” | ocena bez blokowania |
| „Hooks vs Config” | Hooks blokują, Config ocenia |
| „Hooks vs SCP” | Hooks znają właściwości, SCP blokuje API |

---

## 14. Mapowanie na SAP-C02

- **Domain 1 (Organizational complexity):** Config aggregator, GuardDuty delegated admin, Control Tower controls (preventive/detective/proactive), CloudWatch cross-account observability.
- **Domain 2 (New solutions):** event-driven reactions (EventBridge + Lambda/SSM), Hooks w pipeline, Security Hub.
- **Domain 3 (Continuous improvement):** CloudWatch metrics/alarms, Config compliance, remediation, GuardDuty findings, incident response.
- **Domain 4 (Migration/modernization):** monitoring po migracji, baseline compliance, IaC guardrails (Hooks/Guard).

---

## 15. Źródła (docs)

- CloudWatch — what is: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html
- CloudWatch alarms: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html
- CloudWatch Logs: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html
- GuardDuty — what is: https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html
- GuardDuty findings: https://docs.aws.amazon.com/guardduty/latest/ug/findings_management.html
- AWS Config — what is: https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html
- Config rules: https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html
- Config proactive evaluation: https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_turn-on-proactive-rules.html
- Config aggregators: https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html
- CloudFormation Hooks: https://docs.aws.amazon.com/cloudformation-cli/latest/hooks-userguide/what-is-cloudformation-hooks.html
- Control Tower proactive controls: https://docs.aws.amazon.com/controltower/latest/controlreference/proactive-controls.html
- Security Hub: https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html

---

*Najczęstszy błąd: odpowiedź „Config” na pytanie o zablokowanie wdrożenia (to Hooks/SCP), „CloudWatch” na pytanie o wykrycie ataku (to GuardDuty) i „GuardDuty” na pytanie o historię konfiguracji (to Config).*
