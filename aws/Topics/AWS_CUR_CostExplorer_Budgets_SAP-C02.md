# CUR vs Cost Explorer vs AWS Budgets — notatki do egzaminu SAP-C02

> **Zakres:** trzy filary cost managementu: **analiza** (Cost Explorer), **dane szczegółowe** (CUR / CUR 2.0) i **monitorowanie/alerty/akcje** (AWS Budgets).
> **Stan wiedzy:** listopad 2026.
> **Najważniejsze:** to nie są zamienniki. Cost Explorer odpowiada na „dlaczego tyle płacę i ile będę płacić?”, CUR daje „surowe dane do własnej analizy”, a Budgets pilnuje „nie przekrocz progu i zrób coś, gdy go przekroczysz”.

---

## 0. Najkrótsza możliwa odpowiedź

| Narzędzie | Rola | Jedno zdanie |
|---|---|---|
| **AWS Cost Explorer** | interaktywna analiza i prognoza | Wizualizuje i analizuje koszty/zużycie, filtruje, grupuje, prognozuje i pokazuje rekomendacje RI/SP. |
| **CUR (Cost and Usage Report)** | najbardziej szczegółowy eksport danych | Dostarcza **line item**-y (godzinowe/dzienne/miesięczne), z podziałem na produkt, zasób i tagi, do S3 — do Athena/QuickSight/Redshift. |
| **AWS Budgets** | monitoring progów i akcje | Śledzi koszt/zużycie/RI/SP względem progu, wysyła alerty (actual/forecast) i może wykonać **budget action**. |

**Zdanie-klucz na egzamin:** *Cost Explorer = analiza; CUR = szczegółowe dane; Budgets = alerty i akcje. Do alertów nie potrzebujesz CUR; do chargebacku/unit economics potrzebujesz CUR.*

---

## 1. Cost Explorer — analiza kosztów

### Co to jest

AWS Cost Explorer to narzędzie do **przeglądania i analizowania kosztów oraz zużycia**. Udostępnia wykresy, tabele, filtry, grupowania, prognozy i rekomendacje.

### Co potrafi

- **Filtrowanie i grupowanie** po: usłudze, koncie linked, regionie, tagu, cost category, typie zużycia, operacji API, typie instancji itd.
- **Zakresy czasu**: bieżący miesiąc + do **13 miesięcy wstecz** (domyślnie dane dzienne/miesięczne).
- **Dane opcjonalne**: miesięczne dane **multi-year** oraz godzinowe/dzienne dane za **poprzednie 14 dni** (po włączeniu).
- **Prognoza** kosztów na **18 miesięcy** do przodu.
- **Raporty RI/SP**: utilization/coverage, rekomendacje zakupu Reserved Instances i Savings Plans.
- **Cost categories** i tagi kosztowe (po aktywacji).
- **Eksport CSV** i **Cost Explorer API** (`GetCostAndUsage`, `GetCostForecast`, `GetDimensionValues`, `GetReservationUtilization`, `GetSavingsPlansUtilization` itd.).
- **Billing views** (2025+) — analiza w zakresie zdefiniowanego widoku, także cross-account.
- Cost Explorer korzysta z **tego samego zbioru danych** co CUR — różnica to sposób dostępu i szczegółowość.

### Ograniczenia

- Domyślnie **zagregowane** dane — do poziomu pojedynczego zasobu/line item użyj **CUR**.
- **Historia 13 miesięcy** domyślnie; dłużej tylko przez multi-year export/CUR.
- **Odświeżanie co najmniej raz na 24 h** (zależnie od danych billingowych).
- **Nie da się wyłączyć** Cost Explorer po włączeniu.
- **Brak alertów i akcji** — od tego są Budgets, Cost Anomaly Detection, EventBridge.
- **API płatne**: **$0.01 za paginated request**; konsola bez opłat.

### Kiedy używać

- „Dlaczego rachunek wzrósł w zeszłym tygodniu?”
- „Jaki jest trend kosztów per usługa/region/konto?”
- „Ile będę płacić w przyszłym miesiącu?”
- „Czy RI/SP są dobrze wykorzystane?”
- Szybka analiza ad-hoc bez budowania data lake.

---

## 2. CUR — Cost and Usage Report

### Co to jest

AWS Cost and Usage Reports (CUR) to **najbardziej szczegółowy zbiór danych o kosztach i zużyciu**. Raport jest dostarczany do **S3** (który sam posiadasz) i zawiera **line item**-y dla każdej kombinacji produktu, typu zużycia i operacji.

### Co zawiera

- **Line items** — szczegółowe pozycje kosztowe.
- **Granulacja**: godzinowa, dzienna lub miesięczna.
- **Wymiary**: konto, region, usługa, typ zużycia, operacja, tagi, **resource ID** (jeśli włączone), cost categories.
- **Kolumny**: `identity/*`, `bill/*`, `lineItem/*`; opcjonalnie `savingsPlan/*`, `reservation/*`, `pricing/*`, `product/*`.
- **Koszty**: unblended, blended, amortized, net; RI/SP; refunds/credits; support fees; taxes (zależnie od kolumn).
- **Split cost allocation data** dla ECS/EKS — alokacja kosztów współdzielonych zasobów (CUR/CUR 2.0).
- **Manifest** i wersje raportu (overwrite lub new versions).

### Jak działa

- Raport trafia do **S3**; pierwszy plik może pojawić się do **24 h**.
- Aktualizacje: **co najmniej raz dziennie, do 3 razy dziennie**.
- W trakcie miesiąca raport jest **kumulatywny** (month-to-date) i ma charakter **szacunkowy**; finalizuje się po wystawieniu faktury.
- Po zakończeniu miesiąca powstaje nowy raport dla kolejnego miesiąca.
- Po finalizacji mogą pojawić się korekty (refunds, credits, support fees) — np. Support 6–7 dnia następnego miesiąca.
- Duże raporty są dzielone na wiele plików (> ~1 mln wierszy / > 1 GB).
- **CUR 2.0 / AWS Data Exports** — nowszy, zalecany sposób: SQL, wybór kolumn/filtrowanie, eksport do S3; **legacy CUR** nadal dostępny.

### Integracje

- **Amazon Athena** — zapytania SQL po plikach w S3.
- **Amazon Redshift** — ładowanie do hurtowni.
- **Amazon QuickSight** — dashboardy.
- **AWS Glue** — katalog danych.
- **Data Exports** — CUR 2.0, FOCUS, carbon emissions, cost optimization recommendations.

### Kiedy używać

- **Chargeback/showback** i rozliczanie zespołów/projektów.
- **Unit economics** (koszt na tenant/klienta/transakcję).
- **Data lake** i zaawansowana analiza z joinami z innymi danymi.
- **Poziom zasobu** i szczegółowe audyty faktury.
- Długoterminowa historia (ograniczona tylko lifecycle w S3).
- Wymóg „najbardziej szczegółowych danych” — egzaminowy keyword.

### Czego nie robi

- **Nie wysyła alertów** i **nie wykonuje akcji**.
- **Nie jest interaktywny** — wymaga Athena/QuickSight/Redshift.
- **Nie zawiera prognozy** (forecast jest w CE/Budgets).
- **Wymaga inżynierii** (bucket, uprawnienia, crawler, zapytania, koszty S3/Athena/QuickSight).

---

## 3. AWS Budgets — progi, alerty, akcje

### Co to jest

AWS Budgets śledzi koszty/zużycie oraz metryki RI/SP względem zdefiniowanego progu i wysyła **alerty**, a opcjonalnie wykonuje **budget actions**.

### Typy budżetów

| Typ | Co monitoruje |
|---|---|
| **Cost budget** | koszt (stały lub zmienny cel) |
| **Usage budget** | zużycie wybranej usługi |
| **RI utilization budget** | wykorzystanie RI (alert, gdy spada poniżej progu) |
| **RI coverage budget** | pokrycie RI (alert, gdy spada poniżej progu) |
| **Savings Plans utilization budget** | wykorzystanie SP |
| **Savings Plans coverage budget** | pokrycie SP |

### Alerty

- **Actual** — po naliczeniu kosztu; alert zwykle **raz na okres budżetu** przy pierwszym przekroczeniu progu.
- **Forecast** — na podstawie prognozy; może się powtarzać, jeśli prognoza waha się wokół progu.
- Kanały: **e-mail** (do 10 adresów na alert) i **SNS** (jeden topic na alert).
- Prognoza budżetu wymaga **ok. 5 tygodni danych** — dopiero wtedy forecast alerts zaczną działać.
- Można ustawiać **custom period** (np. projekt/fiscal year; maks. 3 lata) oraz budżety miesięczne/kwartalne/roczne.
- **Budgets nie służą do analizy historycznej** — to nie Cost Explorer.

### Budget actions

Po przekroczeniu progu Budgets może (automatycznie lub po **manual approval**):
- zastosować **IAM policy** lub **SCP** (np. zablokować tworzenie nowych zasobów),
- **zatrzymać instancje EC2**,
- **zatrzymać instancje RDS**.

Ważne ograniczenia:
- Z **management account** możesz nałożyć **SCP** na inne konto, ale **nie możesz targetować instancji EC2/RDS w innym koncie**.
- Akcje wymagają **roli IAM** dla Budgets.
- W ASG zatrzymana instancja może zostać zastąpiona — budget action „stop” bywa nieskuteczny bez dodatkowej blokady uprawnień.
- Można wiele akcji na tym samym progu.

### Aktualizacja i zasięg

- Dane aktualizowane **do 3 razy dziennie** (co najmniej raz dziennie; zwykle 8–12 h po poprzedniej aktualizacji) — alerty mogą się spóźnić względem rzeczywistego zużycia.
- Budżet jest widoczny dla konta, które go utworzyło (i użytkowników z dostępem); management account może śledzić konto członkowskie.
- **Billing views** (2025+) pozwalają tworzyć budżety w zakresie filtered cost/usage **cross-account** bez dostępu do management account.
- Tagi na budżetach służą do **kontroli dostępu/audytu**, **nie do alokacji kosztów** (nie pojawią się w CE/CUR jako tagi kosztowe).

### Kiedy używać

- „Nie przekrocz miesięcznego budżetu” / „alert przy 80%”.
- „Automatycznie zatrzymaj dev EC2, gdy budżet przekroczony”.
- „Zablokuj tworzenie zasobów po przekroczeniu progu” (SCP/IAM action).
- „Monitoruj utilization/coverage RI/SP”.
- Guardrails dla zespołów/projektów.

### Koszt

Klasycznie: **pierwsze 2 budżety bez opłat**, kolejne ok. **$0.02/dzień za budżet** — sprawdź aktualny cennik AWS, bo polityka free tier bywa zmieniana.

---

## 4. Główna tabela porównawcza

| Kryterium | **Cost Explorer** | **CUR / CUR 2.0** | **AWS Budgets** |
|---|---|---|---|
| **Cel** | analiza, trendy, prognoza, rekomendacje | szczegółowy eksport danych | monitoring progów, alerty, akcje |
| **Poziom szczegółowości** | zagregowany (usługa/konto/region/tag) | **line item**, opcjonalnie resource ID | zagregowany wg filtra budżetu |
| **Granulacja** | dzienna/miesięczna; opcjonalnie godzinowa/dzienna za 14 dni; multi-year monthly | godzinowa/dzienna/miesięczna | okres budżetu (dzień/miesiąc/kwartał/rok/custom) |
| **Historia** | 13 miesięcy domyślnie; multi-year opcjonalnie | tak długo, jak trzymasz dane w S3 | bieżący okres + prognoza; brak analizy historycznej |
| **Interfejs** | konsola + API | S3 → Athena/QuickSight/Redshift | konsola + API + SNS/e-mail |
| **Świeżość** | ≥ raz/24 h | ≥ raz/dzień, do 3×/dzień | do 3×/dzień (zależnie od billing data) |
| **Prognoza** | tak, 18 miesięcy | nie | tak (alerty forecast; ~5 tygodni danych) |
| **Alerty** | nie (są osobne usługi) | nie | **tak** (actual/forecast) |
| **Akcje** | nie | nie | **tak** (IAM/SCP, stop EC2/RDS) |
| **Koszt** | konsola free; API $0.01/request | brak opłaty za raport; płacisz S3/Athena/QuickSight | 2 budżety free, potem ~$0.02/dzień/budżet |
| **Wymaga inżynierii** | niska | wysoka | niska |
| **Typowe pytanie** | „dlaczego tyle?” / „ile będzie?” | „dane do chargebacku/analizy” | „alert/akcja przy progu” |
| **Kiedy nie** | poziom zasobu, data lake | szybkie alerty, ad-hoc | głęboka analiza, line items |

---

## 5. Kiedy co wybrać — drzewo decyzyjne

```
Czy potrzebujesz alertu lub automatycznej akcji?
├── tak → AWS Budgets
└── nie
    ├── Czy potrzebujesz surowych line itemów / poziomu zasobu / chargebacku?
    │   ├── tak → CUR (CUR 2.0 / Data Exports) + Athena/QuickSight
    │   └── nie → Cost Explorer
    └── Czy potrzebujesz wykryć anomalię bez progu?
        └── tak → Cost Anomaly Detection
```

**Dodatkowe reguły:**
- **Prognoza** → CE (18 mies.) lub Budgets (alert forecast).
- **Rekomendacje RI/SP** → CE (rekomendacje) / Cost Optimization Hub.
- **Cross-account** → CE/Data Exports/Budgets z billing views; management account widzi org.
- **Godzinowe dane** → CUR (hourly) lub CE granular (14 dni).
- **Historia > 13 mies.** → CUR/Data Exports.
- **Koszty per tag/cost category** → aktywuj cost allocation tags; CE/CUR/Budgets (filtry).

---

## 6. Wspólne pojęcia: tagi, cost categories, billing views

### Cost allocation tags

- Tagi muszą zostać **aktywowane jako cost allocation tags** w Billing and Cost Management, żeby pojawiły się w CE/CUR/Budgets.
- Tagi user-defined vs AWS-generated.
- **Cost categories** — mapowanie kosztów na wymiary biznesowe (np. team, project) niezależne od tagów; używane w CE/CUR/Budgets.
- **Tagi na budżetach** (resource tags) służą do IAM/audytu, **nie** do alokacji kosztów.

### Billing views

- Nowy mechanizm (2025+) do definiowania **zakresu danych** (konta, filtry) dla CE, Budgets i Data Exports.
- Umożliwia **decentralizację**: zespół widzi/budżetuje swoje koszty bez dostępu do management account.
- Budżety tworzone w billing view są widoczne tylko w tym widoku.

### Consolidated billing / Organizations

- **Management account** widzi koszty całej organizacji; member account widzi własne (zależnie od uprawnień).
- CUR utworzony w management account obejmuje wszystkie konta organizacji (consolidated billing).
- Budgets: management account może tworzyć budżety monitorujące konta członkowskie; budget actions na SCP mogą działać cross-account, ale stop EC2/RDS już nie.
- Po opuszczeniu organizacji przez konto: budżet śledzi tylko koszty po opuszczeniu; historia sprzed opuszczenia nie jest uwzględniana w alertach.

---

## 7. Sąsiedzi, których nie wolno mylić

| Usługa | Rola | Czym nie jest |
|---|---|---|
| **Cost Anomaly Detection** | ML-owe wykrywanie anomalii kosztowych + alerty | nie jest budżetem progowym; nie wymaga progu |
| **Cost Optimization Hub** | rekomendacje optymalizacji (rightsizing, SP/RI, idle) | nie jest CUR/Budgets |
| **Savings Plans / RI reports** | utilization/coverage | nie są budżetem |
| **Billing and Cost Management console** | centrum ustawień, faktur, tagów | nie jest narzędziem analitycznym jak CE |
| **Data Exports** | eksport CUR 2.0/FOCUS/carbon/S3 | nie jest alertem |
| **AWS Organizations** | konsolidacja rachunku | nie analizuje kosztów |
| **Trusted Advisor** | rekomendacje best practices | nie jest cost managementem |

**Cost Anomaly Detection vs Budgets:** budżet = próg, który sam ustawiasz; anomaly detection = model uczy się wzorca i alarmuje o nietypowych odchyleniach (bez progu). Często używa się obu.

---

## 8. Typowe pułapki egzaminacyjne

1. **CUR ≠ Cost Explorer** — CUR to surowe line itemy; CE to interaktywna analiza zagregowana.
2. **Cost Explorer nie daje poziomu zasobu** — do resource ID/line item użyj CUR.
3. **CE API kosztuje** ($0.01 za paginated request); konsola jest bezpłatna.
4. **CE nie da się wyłączyć** po włączeniu.
5. **CE domyślnie 13 miesięcy**; multi-year/granular trzeba włączyć.
6. **CUR nie ma alertów ani prognozy** — nie wybieraj go na „alert przy 80%”.
7. **Budgets nie służą do analizy historycznej** — tylko bieżący okres/forecast.
8. **Budget actions** nie zatrzymają instancji EC2/RDS w innym koncie z management account (SCP można nałożyć).
9. **Budget action na ASG** może być nieskuteczny — ASG odtworzy instancję.
10. **Forecast alerts wymagają ~5 tygodni danych**.
11. **Actual alert** zwykle raz na okres; **forecast alert** może się powtarzać.
12. **Budgets aktualizowane do 3×/dzień** — alert może przyjść po fakcie.
13. **Tagi na budżetach nie są tagami kosztowymi** — nie pojawią się w CE/CUR.
14. **Cost allocation tags trzeba aktywować** — inaczej nie ma ich w analizie.
15. **CUR jest kumulatywny w miesiącu** i szacunkowy do finalizacji faktury.
16. **CUR może być duży** — wiele plików, wymaga Athena/QuickSight, nie Excel.
17. **CUR 2.0 / Data Exports** to nowszy, zalecany sposób; legacy CUR wciąż istnieje.
18. **Billing views** pozwalają na cross-account budgets/analizę bez management account.
19. **Po opuszczeniu organizacji** budżet nie uwzględnia historii sprzed opuszczenia.
20. **Cost Anomaly Detection ≠ Budgets** — anomalia bez progu vs próg.
21. **CUR nie zastępuje faktury** — to dane billingowe; finalne korekty mogą przyjść później.
22. **CE forecast ≠ Budget forecast** — CE prognozuje koszty; Budgets prognozuje względem progu i alertuje.

---

## 9. Scenariusze egzaminacyjne

**1. Alert przy 80% miesięcznego budżetu 10 000 USD.**
→ **AWS Budgets** (cost budget + actual/forecast alert + SNS/e-mail).

**2. Automatycznie zablokuj tworzenie nowych zasobów po przekroczeniu budżetu.**
→ **Budgets + budget action** (IAM policy/SCP), opcjonalnie manual approval.

**3. Zatrzymaj instancje dev, gdy budżet zostanie przekroczony.**
→ **Budget action** (stop EC2) — pamiętaj o ASG; rozważ SCP/IAM.

**4. Rozlicz koszty per zespół/projekt z dokładnością do zasobu.**
→ **CUR + Athena/QuickSight** (line items, resource IDs, tagi/kategorie kosztów).

**5. Ad-hoc: „dlaczego rachunek za S3 wzrósł w zeszłym tygodniu?”**
→ **Cost Explorer** (filtry, grupowanie, wykres).

**6. Prognoza kosztów na kolejne 18 miesięcy.**
→ **Cost Explorer** (forecast).

**7. Monitoruj utilization RI poniżej 80%.**
→ **Budgets RI utilization budget** (lub raporty RI w CE).

**8. Godzinowe koszty z ostatnich 14 dni.**
→ **Cost Explorer** granular data (po włączeniu) lub **CUR hourly**.

**9. Długoterminowy data lake kosztów (> 13 mies.).**
→ **CUR/Data Exports** do S3 + Athena/QuickSight.

**10. Cross-account budget dla działu bez dostępu do management account.**
→ **Billing view** + **Budgets**; ewentualnie budżet w management account.

**11. Wykryj nietypowy wzrost kosztów bez ustalania progu.**
→ **Cost Anomaly Detection** (nie Budgets).

**12. Chargeback z podziałem na tenantów/klientów.**
→ **CUR** (line items + tagi/cost categories) + Athena.

**13. Alert o przekroczeniu budżetu ma trafić na Slacka.**
→ **SNS → Chatbot/Slack** (albo e-mail); Budgets wysyła do SNS.

**14. Potrzebujesz rekomendacji zakupu Savings Plans.**
→ **Cost Explorer** (rekomendacje) / **Cost Optimization Hub**.

**15. Audyt faktury: credits, refunds, support fees.**
→ **CUR** (line items; korekty po finalizacji).

---

## 10. Pytania testowe (z odpowiedziami)

**1.** Które narzędzie dostarcza najbardziej szczegółowe dane (line item) o kosztach?
A) Cost Explorer
B) CUR
C) AWS Budgets
D) Trusted Advisor
**Odp: B.**

**2.** Które narzędzie służy do alertów o przekroczeniu progu?
A) CUR
B) Cost Explorer
C) AWS Budgets
D) Cost Anomaly Detection
**Odp: C.**

**3.** Do ilu miesięcy wstecz Cost Explorer domyślnie pokazuje dane?
A) 3
B) 6
C) 13
D) 36
**Odp: C.** (plus bieżący miesiąc; multi-year opcjonalnie).

**4.** Ile kosztuje wywołanie Cost Explorer API?
A) bezpłatnie
B) $0.01 za paginated request
C) $1 za GB
D) $0.02/dzień
**Odp: B.**

**5.** Które narzędzie pozwala automatycznie zatrzymać instancje EC2 po przekroczeniu budżetu?
A) CUR
B) Budgets (budget action)
C) Cost Explorer
D) Config
**Odp: B.**

**6.** Czy CUR wysyła alerty?
A) tak, domyślnie
B) nie
C) tylko e-mail
D) tylko przez SNS
**Odp: B.** CUR to eksport danych.

**7.** Co jest potrzebne, by prognoza budżetu działała?
A) 1 dzień danych
B) ok. 5 tygodni danych
C) 1 rok danych
D) nic
**Odp: B.**

**8.** Gdzie trafia CUR?
A) do S3
B) do CloudWatch
C) do SQS
D) do DynamoDB
**Odp: A.**

**9.** Które narzędzie jest najlepsze do chargebacku per zasób?
A) Budgets
B) Cost Explorer
C) CUR + Athena/QuickSight
D) Trusted Advisor
**Odp: C.**

**10.** Co robi budget action z SCP z management account?
A) zatrzymuje instancje w innym koncie
B) nakłada SCP na inne konto
C) tworzy CUR
D) wysyła prognozę
**Odp: B.** Stop EC2/RDS w innym koncie nie jest wspierany z management account.

**11.** Co to jest billing view?
A) faktura
B) zdefiniowany zakres danych kosztowych (cross-account) dla CE/Budgets/Data Exports
C) typ CUR
D) rola IAM
**Odp: B.**

**12.** Które narzędzie wykrywa anomalie bez progu?
A) Budgets
B) Cost Anomaly Detection
C) CUR
D) Cost Explorer
**Odp: B.**

**13.** Czy tagi na budżetach są tagami kosztowymi?
A) tak
B) nie — służą do dostępu/audytu
C) tylko w CUR
D) tylko w CE
**Odp: B.**

**14.** Które narzędzie ma prognozę na 18 miesięcy?
A) CUR
B) Cost Explorer
C) Budgets
D) Config
**Odp: B.**

**15.** Co jest prawdą o aktualizacji CUR?
A) real-time
B) co najmniej raz dziennie, do 3 razy
C) raz w miesiącu
D) tylko na żądanie
**Odp: B.**

---

## 11. Ściąga — „jeśli w pytaniu..., to...”

| Sygnał | Odpowiedź |
|---|---|
| „alert przy 80% budżetu” | AWS Budgets |
| „automatycznie zatrzymaj/zablokuj po przekroczeniu” | Budgets budget action |
| „prognoza kosztów” | Cost Explorer (18 mies.) / Budgets forecast alert |
| „dlaczego rachunek wzrósł” | Cost Explorer |
| „chargeback/showback” | CUR + Athena/QuickSight |
| „unit economics / koszt per tenant” | CUR |
| „poziom zasobu / line item” | CUR |
| „historia > 13 miesięcy” | CUR/Data Exports |
| „godzinowe dane” | CUR hourly / CE granular (14 dni) |
| „cross-account budżet bez management account” | billing view + Budgets |
| „wykryj anomalię bez progu” | Cost Anomaly Detection |
| „rekomendacje RI/SP” | Cost Explorer / Cost Optimization Hub |
| „dane do S3” | CUR / Data Exports |
| „Athena/QuickSight/Redshift” | CUR |
| „budżet RI utilization/coverage” | AWS Budgets |
| „alert na Slacka” | Budgets → SNS → Chatbot |
| „nie da się wyłączyć” | Cost Explorer |
| „API $0.01/request” | Cost Explorer |
| „CUR 2.0” | Data Exports (nowszy, zalecany) |
| „koszty per tag” | aktywuj cost allocation tags; CE/CUR |
| „tagi na budżecie” | tylko IAM/audyt, nie alokacja |
| „po opuszczeniu organizacji” | budżet nie uwzględnia historii sprzed |

---

## 12. Mapowanie na SAP-C02

- **Domain 1 (Organizational complexity):** consolidated billing, cross-account billing views, chargeback/showback, CUR dla organizacji.
- **Domain 2 (New solutions):** budżety i actions jako guardrails, anomaly detection, cost-aware architecture.
- **Domain 3 (Continuous improvement):** Cost Explorer do analizy trendów, CUR do data lake, Budgets do alertów.
- **Domain 4 (Migration/modernization):** analiza kosztów migracji, tagowanie, optymalizacja RI/SP.

---

## 13. Źródła (docs)

- Cost Explorer — what is: https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html
- Cost Explorer — advanced data: https://docs.aws.amazon.com/cost-management/latest/userguide/ce-advanced-cost-analysis.html
- Cost Explorer API: https://docs.aws.amazon.com/cost-management/latest/userguide/ce-api.html
- CUR — what is: https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html
- AWS Data Exports / CUR 2.0: https://docs.aws.amazon.com/cur/latest/userguide/what-is-data-exports.html
- Budgets — managing costs: https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html
- Budget actions: https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-controls.html
- Budgets best practices: https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-best-practices.html
- Budgets with billing views: https://docs.aws.amazon.com/cost-management/latest/userguide/access-data-budgets.html

---

*Najczęstszy błąd: wybieranie CUR na pytanie o alert (to Budgets), Budgets na pytanie o chargeback (to CUR) i CUR na pytanie o szybką analizę trendu (to Cost Explorer).*
