# AWS EventBridge vs Event Bus — notatki do egzaminu SAP-C02

> **Zakres:** Amazon EventBridge, event bus, reguły, targety, cross-account, Pipes, Scheduler, archive/replay — czyli najczęstsze pytania „event-driven” na egzaminie Professional.
> **Stan wiedzy:** listopad 2026. Uwaga: AWS wprowadza nowy model **Custom Event Bus** (z subscriberami, retencją i FIFO). Egzamin SAP-C02 w zdecydowanej większości nadal opiera się na modelu klasycznym (default bus + rules + targets), dlatego oba są opisane — klasyczny jako główny, nowy jako osobna sekcja.
> **Najważniejsze na start:** EventBridge to **serwis**. Event bus to **zasób w tym serwisie**. Nie wybierasz „albo EventBridge, albo event bus” — event bus działa wewnątrz EventBridge, tak jak topic działa w SNS, a queue w SQS.

---

## 0. Najkrótsza możliwa odpowiedź

- **Amazon EventBridge** — serverless, regionalny serwis do budowania architektur sterowanych zdarzeniami: przyjmuje **eventy**, filtruje je i dostarcza do **targetów**. W jego skład wchodzą: event buses, rules, targets, Pipes, Scheduler, Schema Registry, API destinations, archive/replay, global endpoints.
- **Event bus** — „router zdarzeń” w EventBridge: odbiera eventy od źródeł, dopasowuje je do reguł (lub subscriberów) i wysyła do targetów. To **kontener/logiczny zasób**, a nie osobny serwis.
- **Typy busów (model klasyczny):**
  - **default bus** — jeden na konto i region; automatycznie odbiera eventy z usług AWS,
  - **custom bus** — tworzysz sam dla własnych aplikacji; może być wiele,
  - **partner bus** — odbiera eventy od SaaS partnerów (przez partner event source).
- **Rules → targets:** reguła ma **event pattern** (filtr treści) albo harmonogram; maks. **5 targetów na regułę**; event może pasować do wielu reguł.
- **EventBridge to nie kolejka:** klasyczny bus nie przechowuje eventów („brak dopasowania = event przepada”); retencję daje dopiero **archive** (lub nowy Custom Event Bus).
- **Do czego EventBridge, a do czego nie:** routing/wielu odbiorców/filtrowanie/SaaS/AWS events → EventBridge; buforowanie/decoupling → SQS; fan-out i powiadomienia (e-mail, SMS, mobile push) → SNS; strumienie/analiza → Kinesis; orkiestracja → Step Functions.

**Zdanie-klucz na egzamin:** *EventBridge = usługa; event bus = zasób (router). Default bus dostaje eventy AWS, custom bus — Twoje eventy, partner bus — eventy SaaS. Reguły są per bus.*

---

## 1. EventBridge jako serwis — co się na niego składa

EventBridge to nie tylko „bus”. W skład serwisu wchodzą:

| Element | Co robi | Kiedy użyć |
|---|---|---|
| **Event buses** | routing eventów: źródło → bus → reguła → target | many-to-many, filtrowanie treścią, eventy z AWS/SaaS/aplikacji |
| **Rules** | dopasowanie eventu (event pattern) lub harmonogram; wskazanie targetów | reakcja na event albo cykliczne wywołanie |
| **Targets** | odbiorcy: Lambda, SQS, SNS, Step Functions, Kinesis, API destinations, inny event bus, SSM, ECS, Batch itd. | dostarczenie eventu |
| **Pipes** | integracja **point-to-point**: źródło → filtr → enrichment → target | SQS→Lambda, DynamoDB Streams→Step Functions, Kinesis→SQS |
| **Scheduler** | cron/rate/one-time, strefy czasowe, retry, DLQ; miliony harmonogramów | zamiana „scheduled rules”, zadania cykliczne |
| **Schema Registry / Schema Discovery** | katalog schematów eventów + generowanie kodu (bindings) | kontrakty eventów, developer experience |
| **API destinations + connections** | wywołanie zewnętrznego HTTP(S) API jako target | integracja z systemem spoza AWS |
| **Archive & replay** | zapis eventów i ponowne odtworzenie | odzyskanie po błędzie, nowy konsument, testy |
| **Global endpoints** | failover eventów do busa w innym regionie | odporność regionalna |
| **Event sources (V2)** | wciąganie eventów z usługi AWS/SaaS na nowy Custom Event Bus | nowy model (2026) |

Na egzaminie pytanie „co wybrać?” prawie zawsze sprowadza się do jednego z: **bus + rule + target**, **Pipes**, **Scheduler**, **archive/replay**, **global endpoints**.

---

## 2. Event bus jako zasób — anatomia

```
Źródło eventu                 EventBridge                        Odbiorcy
─────────────                 ───────────                        ────────
usługa AWS ─┐
aplikacja  ─┼──►  EVENT BUS  ──►  RULE (event pattern)  ──►  Lambda
SaaS       ─┘        │                    │                    SQS
                     │                    ├──►  SQS             SNS
                     │                    └──►  Step Functions
                     └── (opcjonalnie) archive → replay
```

Najważniejsze cechy busa:
- Jest **regionalny** — bus w `eu-central-1` nie odbiera eventów z `us-east-1`.
- Ma **reguły** (w modelu klasycznym) albo **subscriberów** (nowy Custom Event Bus).
- Reguła należy **do jednego busa** i działa tylko na eventy tego busa.
- Event może pasować do **wielu reguł** — wtedy trafia do targetów każdej z nich.
- Jeden event może trafić do maks. **5 targetów w jednej regule**; chcesz więcej — wiele reguł, SNS fan-out albo Pipes.
- **Brak dopasowania = brak akcji**; klasyczny bus nie buforuje eventów.
- Bus ma **resource-based policy** — tym otwiera się go na inne konta/organizacje.

### Hierarchia pojęć

```
EventBridge (serwis)
└── Event bus (zasób, router)
    ├── Rule (filtr + targety; max 5)
    │   ├── Event pattern
    │   ├── Target + IAM role
    │   └── Input transformer / DLQ / retry
    ├── Archive (opcjonalnie)
    └── Resource policy (cross-account)
```

### Event — struktura

Event to JSON o stałych polach najwyższego poziomu:

```json
{
  "version": "0",
  "id": "1234-...",
  "source": "aws.ec2",
  "detail-type": "EC2 Instance State-change Notification",
  "account": "111122223333",
  "time": "2026-11-07T08:00:00Z",
  "region": "eu-central-1",
  "resources": ["arn:aws:ec2:eu-central-1:111122223333:instance/i-..."],
  "detail": { "state": "running", "instance-id": "i-..." }
}
```

- **source** — kto wygenerował event (`aws.ec2`, `com.myapp.orders`).
- **detail-type** — typ eventu.
- **detail** — właściwa treść; to po niej najczęściej filtrujesz.
- **account / region** — pomocne w regułach cross-account.
- **id** — unikalny identyfikator; przydatny do idempotencji (delivery jest at-least-once).

---

## 3. EventBridge vs event bus — tabela, która rozwiewa nieporozumienie

| Pytanie | EventBridge | Event bus |
|---|---|---|
| Co to jest? | **serwis** (usługa AWS) | **zasób** wewnątrz serwisu |
| Zakres | cały serwis: buses, rules, targets, Pipes, Scheduler, schema, archive, endpoints | pojedynczy router eventów |
| Czy można „wybrać jedno zamiast drugiego”? | Nie — bus jest częścią EventBridge | Nie — bus istnieje w EventBridge |
| Co konfigurujesz? | uprawnienia IAM, bus, rules, pipes, schedules, schematy, archiwa, endpointy | reguły/subscriberów, targety, politykę busa, archive |
| Ile ich jest? | jeden serwis per region | wiele busów: 1 default + custom + partner (limity niżej) |
| Analogia | SNS jako usługa | topic w SNS; queue w SQS |
| Typowe pytanie egzaminacyjne | „Jak skierować eventy z wielu źródeł do wielu odbiorców z filtrowaniem?” | „Na którym busie ma być reguła? default, custom czy partner?” |

**Najczęstsze nieporozumienie:** „EventBridge czy event bus?” — to pytanie jak „SNS czy topic?”. Poprawna odpowiedź: EventBridge to usługa, a event bus to jeden z jej elementów. W praktyce pytanie egzaminacyjne dotyczy **typu busa** (default/custom/partner) albo **mechanizmu** (bus vs Pipes vs Scheduler).

### Co wybrać: bus, Pipes czy Scheduler?

| Potrzeba | Wybór | Dlaczego |
|---|---|---|
| Wiele źródeł → wielu odbiorców, filtrowanie po treści | **event bus + rules** | many-to-many, content-based routing |
| Jedno źródło → jeden target + filtr/enrichment | **Pipes** | point-to-point, prostsze niż reguła, wbudowany enrichment |
| Harmonogram cron/rate, one-time | **Scheduler** | następca scheduled rules, strefy czasowe, retry, DLQ |
| Odtworzenie historii eventów | **archive + replay** | zapis i ponowne podanie do źródłowego busa |
| Failover regionu | **global endpoints** | Route 53 health check + replikacja |
| Integracja z zewnętrznym HTTP API | **API destination** | target HTTPS z connection/auth |

---

## 4. Typy event busów (klasyczny model egzaminacyjny)

### 4.1 Default event bus

- **Jeden na konto na region**, tworzony automatycznie.
- Odbiera eventy **z usług AWS** (EC2 state change, S3, CloudFormation, CloudTrail itd.).
- Domyślnie przyjmuje eventy tylko z **własnego konta**; cross-account wymaga resource policy.
- Nie da się go usunąć (możesz nim zarządzać przez reguły).
- **CloudWatch Events → EventBridge:** to ten sam bazowy mechanizm/API (`events:`); EventBridge to rozwinięcie CloudWatch Events. Historyczne pytania o „CloudWatch Events” mają odpowiedzi jak dla default bus.

### 4.2 Custom event bus

- Tworzysz go sam (`CreateEventBus`), możesz mieć wiele (limit klasyczny: 100 na konto/region).
- Służy do eventów **własnych aplikacji** (`PutEvents`) oraz do eventów z innych kont/regionów.
- Ma własne reguły, targety, archive i resource policy.
- Typowy wzorzec: osobny bus per domena (orders, payments, inventory), żeby rozdzielić ruch i uprawnienia.
- Można na niego wysyłać eventy z **innego busa** (bus jako target) — także cross-account i cross-region.

### 4.3 Partner event bus

- Odbiera eventy od **SaaS partnerów** (Stripe, Datadog, Auth0, PagerDuty, Shopify itd.).
- Wymaga **partner event source** utworzonego przez partnera dla Twojego account ID.
- Partner event source trzeba **skojarzyć z partner event busem**; eventy wysłane do nieskojarzonego source są **odrzucane**.
- Tylko konto partnera może publikować do partner busa — `PutPermission` na partner busie zwraca błąd.
- Na partner busie tworzysz reguły tak jak na każdym innym busie.

### 4.4 „Bus jako target” — nie typ, ale ważny wzorzec

- Reguła może mieć jako target **inny event bus** — w tym samym koncie, w innym koncie lub w innym regionie.
- Cross-account: odbiorca musi mieć **resource policy** na busie, a nadawca **IAM role** (wymagane dla nowych targetów od 2 marca 2023 r.).
- Cross-region: target bus w innym regionie jest wspierany; do failoveru używa się global endpoints.
- To podstawa wzorca **centralnego event busa** (hub-and-spoke).

### 4.5 Nowy Custom Event Bus (model 2026) — dla świadomości

W 2026 AWS dodał drugi model busa: **Custom Event Bus** (ARN `event-busv2/...`), obok **Custom Event Bus - Classic** (czyli dotychczasowych rules + targets).

| Cecha | Custom Event Bus (nowy) | Custom Event Bus - Classic |
|---|---|---|
| Routing | **subscriberzy** (każdy ma własny filtr i **jeden** target) | reguły + do 5 targetów |
| Retencja | wbudowana, **1–365 dni** | brak; tylko archive |
| Kolejność | **FIFO w obrębie event group**, per subscriber | brak gwarancji |
| Replay | pozycja startowa subscribera na zachowanych eventach | replay z archive |
| Deduplikacja | przy publikacji, okno **5 minut** (content lub klucz) | brak |
| Publikacja | `PutEvents` (JSON) + `PutRawEvents` (JSON/Avro/Protobuf/bytes) | `PutEvents` (JSON) |
| Cross-account | udostępnianie busa przez **AWS RAM** (managed permissions), konsument tworzy własne subscribery | resource policy +/lub forward na bus w innym koncie |
| Limity | 5 busów/region, 100 wpisów na request | 100 busów/region, 10 wpisów na request |

**Egzaminowo:** pytania SAP-C02 nadal opisują klasyczny model. Nowy model traktuj jako ciekawostkę/„future direction”, chyba że pytanie wprost mówi o subscriberach, retencji na busie, FIFO lub RAM.

---

## 5. Reguły, wzorce i targety

### 5.1 Event pattern — filtrowanie treści

Reguła dopasowuje event po polach, np.:

```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": { "state": ["running", "stopped"] }
}
```

Dostępne operatory (klasyczne):
- dokładna wartość: `"state": ["running"]`,
- `prefix`, `suffix`, `anything-but`, `numeric` (zakresy), `exists`, `null`,
- wildcard `*` w niektórych polach (osobny limit reguł z wildcardami).

**Ważne:** wzorzec dopasowuje **tylko eventy z danego busa**; event może pasować do wielu reguł; brak dopasowania = event jest ignorowany.

### 5.2 Input transformer / input path

- **Input path** — wyciągasz fragment eventu do targetu.
- **Input transformer** — budujesz nowy JSON z pól eventu (zmienne `$.detail...`) i stałych; przydatne, gdy target oczekuje konkretnego formatu (SQS, API destination, Step Functions).
- Transformacja nie zmienia eventu na busie — tylko to, co dostaje target.

### 5.3 Targety i uprawnienia

Typowe targety: Lambda, SQS (standard/FIFO), SNS, Step Functions, Kinesis, Firehose, API destinations, inny event bus, ECS task, Batch, SSM Run Command/Automation, CodeBuild/CodePipeline, CloudWatch Logs, Redshift Data API, SageMaker Pipeline. (Scheduler to osobny mechanizm harmonogramów, nie target busa.)

- EventBridge potrzebuje **IAM role** do wywołania targetu (trust `events.amazonaws.com`).
- Dla targetów szyfrowanych KMS (SQS/SNS) key policy musi pozwolić `events.amazonaws.com` na `kms:Decrypt`/`GenerateDataKey`.
- **DLQ** (standard SQS) przechwytuje eventy, których nie udało się dostarczyć; retry policy definiuje próby i wiek eventu.
- Dostarczanie jest **at-least-once** — mogą wystąpić duplikaty; projektuj idempotentne konsumenty.

### 5.4 Harmonogramy

- **Scheduled rule** — legacy; potrafi cron/rate, ale ma ograniczenia.
- **EventBridge Scheduler** — zalecany: cron/rate/one-time, strefy czasowe, flexible time windows, retry, DLQ, templated/universal targets (270+ usług). Używaj, gdy pytanie mówi o harmonogramach na skalę.

---

## 6. Co EventBridge robi poza busami

### 6.1 EventBridge Pipes

- Integracja **point-to-point**: jedno źródło → (filtr) → (enrichment) → jeden target.
- Źródła: m.in. SQS, Kinesis Data Streams, DynamoDB Streams, MSK, Amazon MQ.
- Wzbogacanie: Lambda, Step Functions, API destination, itd.
- Targety: Lambda, Step Functions, SQS, SNS, Kinesis, EventBridge bus, API destination itd.
- **Pipes ≠ bus:** bus robi many-to-many; pipe robi jeden-do-jednego.
- Częsty wzorzec: pipe ze źródłem DynamoDB Streams i targetem event bus, który następnie fan-outuje event do wielu odbiorców.

### 6.2 EventBridge Scheduler

- Serverless scheduler: harmonogramy cron/rate i jednorazowe.
- Miliony zadań, 270+ usług i tysiące akcji API przez universal target.
- Retry, DLQ, flexible time windows, strefa czasowa.
- Zastępuje scheduled rules; egzaminowo: „cron job” → Scheduler, nie „scheduled rule”.

### 6.3 Schema Registry / Schema Discovery

- Schematy eventów AWS są dostępne automatycznie; dla własnych eventów możesz wgrać schemat lub włączyć **discovery**.
- Generuje **code bindings** (Java, Python, TypeScript itd.).
- Pomaga utrzymać kontrakt eventów i walidację; nie jest wymagany do routingu.

### 6.4 API destinations + connections

- Target typu HTTPS do zewnętrznego API (np. system SaaS, webhook).
- **Connection** przechowuje konfigurację auth (API key, OAuth, basic).
- Używaj, gdy event ma „wyjść” poza AWS do HTTP API; do odbioru webhooków użyj API Gateway → `PutEvents`.

### 6.5 Archive & replay

- **Archive** zapisuje eventy z jednego źródłowego busa według wzorca; retencja domyślnie **bezterminowa** (można ustawić dni).
- **Replay** odtwarza eventy **tylko do busa źródłowego**; można wybrać zakres czasu i reguły.
- Eventy w replayu dostają pole `replay-name`; nie trafiają ponownie do archiwum (chroni przed pętlą).
- Replay nie gwarantuje kolejności; ma opóźnienie — zaleca się odczekać ~10 minut od publikacji.
- Limit: **10 równoczesnych replayów** na konto/region; replaye są usuwane po 90 dniach.

### 6.6 Global endpoints

- Failover eventów między regionami na podstawie **Route 53 health check**.
- Wymaga busa o tej samej nazwie w regionie secondary (dla custom busów).
- Opcjonalna **replikacja eventów** do obu regionów; bez niej powrót do primary wymaga ręcznego resetu health checka.
- RTO/RPO ok. **360 s** (maks. 420 s) przy zalecanej konfiguracji alarmów.
- Dotyczy **custom events**; obsługuje klasyczne ARN-y `event-bus/` (odrzuca `event-busv2/`).
- Używaj do odporności regionalnej, a nie jako zamiennika cross-region routingu reguł.

---

## 7. EventBridge vs SNS vs SQS vs Kinesis vs Step Functions

| Usługa | Model | Główne zastosowanie | Kiedy NIE |
|---|---|---|---|
| **EventBridge** | bus + reguły/targety; filtrowanie treści; wiele targetów | routing eventów z AWS/SaaS/aplikacji, event-driven, integracje, archiwum/replay | kolejkowanie, strumienie, powiadomienia mobilne, request/response |
| **SNS** | pub/sub, topic, subskrybenci (SQS, Lambda, HTTP, e-mail, SMS, mobile push) | fan-out, powiadomienia, prosty pub/sub | zaawansowane filtrowanie po treści, SaaS/AWS events, replay |
| **SQS** | kolejka, pull, bufor | decoupling, buforowanie, retry, backpressure, FIFO | routing many-to-many, filtrowanie, eventy AWS |
| **Kinesis Data Streams** | strumień, shardy, kolejność per shard | analityka strumieniowa, wysokie throughput, wielokrotni konsumenci | proste eventy aplikacyjne, SaaS integration |
| **Step Functions** | orkiestracja workflow | złożone procesy, retry, human approval | prosty routing eventów |

**Egzaminowe skróty:**
- „react to AWS service event” → **EventBridge (default bus)**.
- „fan-out to many subscribers / e-mail / SMS / mobile push” → **SNS**.
- „buffer/decouple/retry” → **SQS**.
- „streaming analytics, millions of records, ordering per shard” → **Kinesis**.
- „orchestrate steps with state machine” → **Step Functions**.
- „SaaS events” → **EventBridge partner bus**.
- „cron” → **EventBridge Scheduler**.

---

## 8. Wzorce architektoniczne

### 8.1 Content-based routing (bus + rules)

Jeden custom bus, wiele reguł: każda reguła filtruje po `detail-type`/`detail` i kieruje do innego targetu. Zaleta: źródło nie zna odbiorców; dodanie konsumenta = nowa reguła.

### 8.2 Fan-out

- Reguła z wieloma targetami (max 5), albo
- bus → SNS → wiele SQS (klasyczny fan-out), albo
- wiele reguł na tym samym busie (każda z innym targetem).
Uwaga na limit 5 targetów na regułę.

### 8.3 Central event bus (hub-and-spoke)

- Każde konto/domena ma własny bus; eventy są forwardowane do centralnego busa (bus jako target).
- Cross-account: resource policy na centralnym busie + IAM role w kontach źródłowych.
- Cross-region: centralny bus w regionie docelowym; do failoveru — global endpoints.

### 8.4 Cross-account

1. Odbiorca: resource policy na busie (allow `events:PutEvents` dla konta/organizacji; można ograniczyć `events:source`/`events:account`).
2. Nadawca: reguła z targetem = ARN busa odbiorcy + IAM role.
3. Odbiorca: reguła dopasowująca eventy (dobrze dodać `account` do wzorca).
4. SCP działają na rolę nadawcy — jeśli SCP blokuje `events:PutEvents`, event nie wyjdzie.

### 8.5 SaaS integration

Partner event source → partner bus → reguła → target. Pamiętaj: eventy do nieskojarzonego source są odrzucane; tylko partner publikuje.

### 8.6 Schedule + event bus

Scheduler wywołuje Lambda/Step Functions albo publikuje event na bus; bus rozdziela go do odbiorców. Alternatywa dla scheduled rules (legacy).

### 8.7 Pipes + bus

Pipe robi źródło→enrichment→bus; bus robi fan-out do wielu odbiorców. Typowy wzorzec dla DynamoDB Streams → wzbogacenie → bus.

### 8.8 Replay

Archive na busie → po naprawie błędu replay do tego samego busa → reguły ponownie dostarczają eventy. Pamiętaj o idempotencji konsumentów (eventy mogą się zdublować).

### 8.9 Multi-region

Dwa busy o tej samej nazwie w dwóch regionach + global endpoint + health check + replikacja. Alternatywnie reguły cross-region do busa w regionie zapasowym (bez automatycznego failoveru).

---

## 9. Limity i zachowania — tabela egzaminacyjna

| Element | Wartość (model klasyczny) | Uwaga |
|---|---|---|
| Event buses per account/region | 100 (limit zwiększalny) + default | nowy Custom Event Bus: 5 |
| Rules per bus | 300 (domyślnie, zwiększalne) | scheduled rules też się liczą |
| Targets per rule | **5** | więcej → wiele reguł / SNS / Pipes |
| PutEvents | do **10 wpisów** na request | request max **1 MB** łącznie |
| Rozmiar eventu | do **1 MB** (przy jednym wpisie) | większy payload → wskaż S3 |
| Event pattern | 2048 znaków (klasyczny) | nowy model: filtry inne limity |
| Kolejność | **brak gwarancji** (klasyczny) | FIFO per shard w Kinesis; FIFO per event group w V2 |
| Dostarczanie | **at-least-once** | idempotencja po stronie konsumenta |
| Retencja na busie | brak (klasyczny) | archive: domyślnie bezterminowo |
| Replay | tylko do źródłowego busa | 10 równoczesnych; brak gwarancji kolejności |
| Cross-account target | resource policy + IAM role | nowe targety od 03.2023 wymagają roli |
| Global endpoints | RTO/RPO ~360 s (max 420 s) | tylko klasyczne ARN-y `event-bus/` |
| EventBridge Scheduler | miliony harmonogramów | retry, DLQ, time windows |
| Pipes | 1000 pipe'ów/account (typowo) | point-to-point |

> Limity mogą się zmieniać — na egzaminie pytane są przede wszystkim **targets per rule = 5**, **PutEvents = 10 wpisów**, **brak kolejności** i **at-least-once**.

---

## 10. Uprawnienia i bezpieczeństwo

| Warstwa | Mechanizm | Po co |
|---|---|---|
| Kto może publikować/zarządzać busem | **IAM identity policy** | `events:PutEvents`, `events:PutRule`, `events:PutTargets` |
| Cross-account publish | **resource-based policy na busie** | grant dla konta/organizacji; można ograniczyć source/account |
| Wywołanie targetu | **IAM role** z trust `events.amazonaws.com` | Lambda, SQS, SNS, Step Functions itd. |
| Target szyfrowany KMS | **key policy** dla `events.amazonaws.com` | `kms:Decrypt`, `kms:GenerateDataKey` |
| SCP | organizacja | może zablokować `events:PutEvents` mimo uprawnień IAM |
| Partner bus | tylko partner event source | `PutPermission` na partner busie nie działa |
| Szyfrowanie w spoczynku | klucze AWS (klasyczny) / opcjonalny KMS (V2) | dane eventów/archiwum |

**Cross-account — pełna lista warunków:**
1. Receiver: resource policy na busie (`events:PutEvents` dla account/org).
2. Sender: reguła z targetem bus ARN + IAM role.
3. Receiver: reguła dopasowująca event (dodaj `account` do patternu).
4. SCP nadawcy nie blokuje `events:PutEvents`.
5. Dla partner busa: tylko partner.

**Cross-region:** target bus w innym regionie jest wspierany; do automatycznego failoveru użyj global endpoints. EventBridge jest regionalny, więc „jeden bus na świat” nie istnieje.

---

## 11. Nowości 2025–2026 (dla świadomości)

- **Custom Event Bus (V2)** — nowy model: subscriberzy zamiast reguł, retencja 1–365 dni, FIFO per event group, dedup 5 min, `PutRawEvents` (Avro/Protobuf/bytes), udostępnianie przez AWS RAM, universal targets.
- **Event sources** jako zasób — wciąganie eventów AWS/SaaS na bus V2.
- **Cross-account sharing przez AWS RAM** — managed permissions: publish-only, subscribe-only, event-source, full access.
- **EventBridge Pipes** — coraz więcej źródeł/targetów; integracje z DynamoDB Streams, MSK, MQ.
- **EventBridge Scheduler** — stopniowe wypieranie scheduled rules; egzamin może już mówić „Scheduler zamiast scheduled rule”.
- **Global endpoints** — nadal tylko klasyczne bus ARN-y; V2 na razie poza global endpoints.
- **API destinations** — integracje z zewnętrznymi HTTP API bez Lambda.

**Egzaminowo:** jeśli pytanie nie wspomina o subscriberach/retencji/FIFO/RAM, odpowiadaj modelem klasycznym (default/custom/partner + rules + targets).

---

## 12. Typowe pułapki egzaminacyjne

1. **„EventBridge vs event bus” to nie alternatywa** — bus jest częścią EventBridge; pytanie dotyczy raczej typu busa lub mechanizmu.
2. **Default bus odbiera eventy AWS** — nie trzeba go tworzyć; jest jeden na konto/region i nie da się go usunąć.
3. **Custom bus dla własnych eventów** — możesz publikować też na default, ale praktyka i pytania egzaminacyjne wskazują custom bus.
4. **Partner bus wymaga partner event source** — bez skojarzenia eventy są odrzucane; tylko partner publikuje.
5. **Reguła działa tylko na swoim busie** — event na innym busie jej nie aktywuje.
6. **Brak dopasowania = event przepada** — klasyczny bus nie buforuje; chcesz retencję → archive/V2.
7. **Max 5 targetów na regułę** — więcej → wiele reguł, SNS fan-out albo Pipes.
8. **Cross-account wymaga obu stron** — resource policy na busie odbiorcy i IAM role u nadawcy; SCP nadawcy też działa.
9. **Cross-account pattern powinien zawierać `account`** — inaczej reguły mogą łapać eventy z nieznanych kont.
10. **Kolejność nie jest gwarantowana** (klasyczny) — do kolejności użyj SQS FIFO/Kinesis/V2 event group.
11. **At-least-once** — duplikaty są możliwe; konsument musi być idempotentny.
12. **Replay tylko do źródłowego busa** — nie do innego busa/konta.
13. **Replay nie zachowuje kolejności** i ma opóźnienie; `replay-name` w evencie.
14. **Archive domyślnie bezterminowy**, ale to osobny koszt; replay kosztuje.
15. **Scheduled rules to legacy** — do harmonogramów Scheduler.
16. **Pipes to nie bus** — point-to-point, bez reguł many-to-many.
17. **EventBridge to nie Kinesis** — nie do strumieni/analizy o wysokim throughput.
18. **EventBridge to nie SNS** — brak e-mail/SMS/mobile push; SNS ma subskrybentów protokołowych.
19. **EventBridge to nie SQS** — brak buforowania/backpressure; eventy nie czekają na konsumenta.
20. **Payload > 1 MB** — nie przejdzie; wzorzec: zapisz w S3 i wyślij referencję.
21. **`PutEvents` zwraca 200, nawet gdy bus nie istnieje** — event jest po cichu odrzucany; sprawdzaj `FailedEntryCount` i metryki.
22. **Reguły z wildcardami mają osobny limit** — nie zakładaj nieskończonej liczby.
23. **Targety KMS** — bez key policy EventBridge nie dostarczy eventu.
24. **Global endpoints nie zastępują cross-region reguł** — to failover, nie routing.
25. **EventBridge jest regionalny** — bus w jednym regionie nie widzi eventów z innego.

---

## 13. Scenariusze egzaminacyjne

**1. Reakcja na zmianę stanu instancji EC2 → uruchom Lambda.**
→ Reguła na **default bus** z event pattern `source=aws.ec2`, `detail-type=EC2 Instance State-change Notification`, `detail.state=running`; target Lambda.

**2. Aplikacja zamówień publikuje eventy, 5 zespołów chce różnych podzbiorów.**
→ **Custom bus** + 5 reguł z różnymi event patterns i targetami (SQS/Lambda); źródło nie zna konsumentów.

**3. Odbieranie eventów ze Stripe/Datadog.**
→ **Partner event source** od partnera + **partner bus** + reguła; bez source eventy są odrzucane.

**4. 50 kont ma wysyłać eventy do centralnego busa w koncie security.**
→ Cross-account: resource policy na centralnym busie (org/accounts) + IAM role w każdym koncie + reguła z targetem bus ARN; opcjonalnie `account` w patternie.

**5. Po wdrożeniu błędu chcesz ponownie przetworzyć eventy z ostatnich 24 h.**
→ **Archive** na busie + **replay** do tego samego busa; konsumenci idempotentni.

**6. Aplikacja ma działać przy awarii regionu.**
→ **Global endpoints** + bus o tej samej nazwie w regionie secondary + health check + replikacja.

**7. Co 15 minut uruchamiaj zadanie.**
→ **EventBridge Scheduler** (cron/rate); nie scheduled rule.

**8. SQS → wzbogacenie przez Lambda → Step Functions.**
→ **EventBridge Pipes** (źródło SQS, enrichment Lambda, target Step Functions).

**9. Reguła ma wysłać event do 7 targetów.**
→ Limit 5 na regułę: **dwie reguły** z tym samym patternem lub **SNS fan-out** / Pipes.

**10. Eventy muszą być przetwarzane w kolejności per klient.**
→ Klasyczny EventBridge nie gwarantuje kolejności: **SQS FIFO**/Kinesis; w nowym modelu **V2 event group + FIFO subscriber**.

**11. Payload 5 MB.**
→ Nie przejdzie (limit ~1 MB): zapisz w **S3** i wyślij referencję (S3 event/URL) w evencie.

**12. Wysyłanie eventów do zewnętrznego systemu przez HTTPS.**
→ **API destination** + **connection** (auth) jako target reguły.

**13. Eventy nie docierają do Lambdy — jak debugować?**
→ Sprawdź: zły bus, brak dopasowania patternu, brak uprawnień targetu (role), DLQ, metryki `Invocations`/`FailedInvocations`, CloudTrail.

**14. SCP blokuje `events:PutEvents` w koncie źródłowym.**
→ Cross-account event nie wyjdzie, mimo resource policy; SCP działa na rolę/principal nadawcy.

**15. Firma chce „jednego event busa na wszystko”.**
→ Antywzorzec: jeden bus utrudnia uprawnienia i izolację; raczej bus per domena + centralny bus integracyjny.

**16. Trzeba wysłać event z AWS do innego regionu bez failoveru.**
→ Reguła z targetem **event bus w innym regionie** (cross-region target).

---

## 14. Pytania testowe (z odpowiedziami)

**1.** Czym jest event bus w EventBridge?
A) Osobnym serwisem do kolejkowania
B) Routerem eventów wewnątrz EventBridge
C) Targetem reguły
D) Zamiennikiem SQS
**Odp: B.** Bus to zasób w EventBridge; odbiera eventy i kieruje je do reguł/targetów.

**2.** Który bus automatycznie odbiera eventy z usług AWS?
A) custom
B) partner
C) default
D) cross-account
**Odp: C.** Default bus jest tworzony per konto/region.

**3.** Chcesz odbierać eventy z SaaS (np. Stripe). Co tworzysz?
A) custom bus + PutEvents
B) partner event source + partner bus
C) default bus + regułę
D) SQS FIFO
**Odp: B.** Partner source trzeba skojarzyć z partner busem.

**4.** Ile targetów można przypisać do jednej reguły (klasycznie)?
A) 1
B) 5
C) 10
D) bez limitu
**Odp: B.**

**5.** Aplikacja publikuje własne eventy. Który bus jest najlepszy?
A) default
B) custom
C) partner
D) global endpoint
**Odp: B.** Custom bus dla własnych aplikacji i cross-account.

**6.** Eventy mają być odtworzone po błędzie. Co włączasz?
A) SQS DLQ
B) archive + replay
C) global endpoints
D) Scheduler
**Odp: B.** Archive zapisuje, replay odtwarza do źródłowego busa.

**7.** Reguła na busie A nie reaguje na eventy z busa B w tym samym koncie. Dlaczego?
A) bo reguły są per bus
B) bo EventBridge nie działa w jednym koncie
C) bo brak SQS
D) bo trzeba użyć SNS
**Odp: A.** Reguła działa tylko na eventy swojego busa.

**8.** Cross-account eventy nie docierają. Co jest wymagane po stronie odbiorcy?
A) tylko IAM user
B) resource policy na busie
C) SQS DLQ
D) Kinesis
**Odp: B.** Plus IAM role po stronie nadawcy, gdy reguła wysyła event do busa docelowego (bus jako target).

**9.** Które stwierdzenie o kolejności jest prawdziwe dla klasycznego EventBridge?
A) zawsze FIFO
B) brak gwarancji kolejności
C) kolejność per reguła
D) kolejność per target
**Odp: B.** Do FIFO użyj SQS FIFO/Kinesis/V2 event group.

**10.** Chcesz uruchamiać zadanie o 3:00 w strefie Europe/Warsaw. Co wybierzesz?
A) scheduled rule
B) EventBridge Scheduler
C) SNS
D) SQS
**Odp: B.** Scheduler wspiera strefy czasowe i jest zalecany.

**11.** Reguła ma wysłać event do 6 różnych kolejek SQS. Jak to zrobić?
A) 6 targetów w jednej regule
B) wiele reguł lub SNS fan-out
C) Pipes
D) Kinesis
**Odp: B.** Limit 5 targetów na regułę.

**12.** Który wzorzec jest point-to-point z enrichmentem?
A) event bus
B) Pipes
C) Scheduler
D) Schema Registry
**Odp: B.**

**13.** Jak obsłużyć event większy niż 1 MB?
A) wysłać bezpośrednio
B) zapisać w S3 i wysłać referencję
C) użyć SNS
D) użyć Kinesis
**Odp: B.**

**14.** Co robi global endpoint?
A) replikuje eventy do wielu busów w tym samym regionie
B) failover eventów do busa w innym regionie
C) tworzy nowy bus
D) zastępuje archive
**Odp: B.**

**15.** Który mechanizm jest następcą scheduled rules?
A) Pipes
B) Scheduler
C) Schema Registry
D) API destination
**Odp: B.**

---

## 15. Ściąga — „jeśli w pytaniu..., to...”

| Sygnał | Odpowiedź |
|---|---|
| „react to AWS service event” | EventBridge + default bus + rule |
| „custom app publishes events” | custom bus + PutEvents |
| „SaaS events” | partner event source + partner bus |
| „route by event content” | EventBridge rule + event pattern |
| „many consumers, different filters” | EventBridge custom bus + rules (lub SNS fan-out + SQS) |
| „point-to-point + enrichment” | EventBridge Pipes |
| „cron / one-time / time zone” | EventBridge Scheduler |
| „replay events” | archive + replay |
| „regional failover” | global endpoints |
| „send to external HTTP API” | API destination + connection |
| „cross-account events” | resource policy na busie + IAM role + rule target bus ARN |
| „cross-region events” | rule target bus w innym regionie |
| „ordering” | SQS FIFO / Kinesis (klasyczny EventBridge nie gwarantuje) |
| „buffer/decouple” | SQS, nie EventBridge |
| „e-mail/SMS/mobile push” | SNS, nie EventBridge |
| „streaming analytics” | Kinesis, nie EventBridge |
| „workflow orchestration” | Step Functions |
| „> 5 targets” | wiele reguł / SNS / Pipes |
| „payload > 1 MB” | S3 pointer pattern |
| „event lost, no rule” | brak dopasowania = drop; dodaj regułę/archive |
| „duplicates” | at-least-once → idempotencja |
| „EventBridge vs event bus” | usługa vs zasób; bus jest w EventBridge |
| „CloudWatch Events” | to samo co EventBridge (default bus) |

---

## 16. Mapowanie na SAP-C02

- **Domain 1 (Organizational complexity):** cross-account event routing, central event bus, organizacje + resource policies + SCP, multi-region.
- **Domain 2 (New solutions):** event-driven architectures, Pipes, Scheduler, API destinations, SaaS integration.
- **Domain 3 (Continuous improvement):** archive/replay, observability, DLQ, retry, idempotencja.
- **Domain 4 (Migration/modernization):** zastępowanie batchy eventami, integracje SaaS, decoupling.

---

## 17. Źródła (docs)

- What is Amazon EventBridge: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html
- Event bus concepts: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is-how-it-works-concepts.html
- Event bus (default i Classic): https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-bus.html
- Custom Event Bus (nowy): https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-custom-bus.html
- Rules: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html
- Targets: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-targets.html
- Cross-account: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-cross-account.html
- Cross-region: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-cross-region.html
- Archive/replay: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-archive.html
- Pipes: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-pipes.html
- Scheduler: https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html
- Global endpoints: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-global-endpoints.html
- Quotas: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-quota.html
- SaaS/partner events: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-saas.html
- Event bus permissions: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-bus-perms.html

---

*Najczęstszy błąd: odpowiedź „SNS”, gdy pytanie wymaga filtrowania po treści i routingu z wielu źródeł AWS/SaaS (EventBridge), oraz odpowiedź „event bus”, gdy pytanie dotyczy serwisu jako całości (EventBridge).*
