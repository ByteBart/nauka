## 25.21 Checks — 60 Pytań z Odpowiedziami

1. **Q: Co to jest Amazon SageMaker?** A: Pełni zarządzana platforma ML pokrywająca cały cykl życia modelu — dane, trening, wdrożenie, monitoring.

2. **Q: Jaka jest różnica między SageMaker a Amazon Bedrock?** A: SageMaker = własne modele (budowanie/trenowanie). Bedrock = gotowe Foundation Models (LLM) przez API bez infrastruktury.

3. **Q: Co to jest SageMaker Studio?** A: Unified IDE (przeglądarka) dla całego workflow ML — notebooki, training, pipelines, experiments, monitoring.

4. **Q: Jak izolować sieciowo SageMaker Training Job?** A: Konfiguracja VPC + `EnableNetworkIsolation: true` — brak dostępu do internetu z kontenera.

5. **Q: Co to jest Feature Store?** A: Centralne repozytorium cech ML — online (niski latency, inference) + offline (historia, training).

6. **Q: Różnica między Feature Store Online a Offline?** A: Online = in-memory, milisekundy, tylko aktualny rekord. Offline = S3/Parquet, historia, do trenowania.

7. **Q: Co to jest SageMaker Pipelines?** A: Orchestrator ML workflow — DAG kroków (processing, training, evaluation, deploy) z wersjonowaniem i wizualizacją w Studio.

8. **Q: Kiedy użyć Serverless Inference zamiast Real-Time Endpoint?** A: Gdy ruch jest rzadki/nieregularny i koszt 24/7 instancji jest za wysoki. Akceptujesz cold start kilku sekund.

9. **Q: Co to jest SageMaker Autopilot?** A: AutoML — automatyczne tworzenie, trenowanie i optymalizacja modeli. Można zobaczyć wygenerowany kod (explainability AutoML).

10. **Q: Co to jest SageMaker Canvas?** A: No-code ML dla biznesu — drag & drop UI, brak potrzeby kodu. Dla analityków bez DS background.

11. **Q: Co to jest SageMaker JumpStart?** A: Hub gotowych modeli i rozwiązań — pretrained modele (HuggingFace, Stability AI) + one-click deploy + fine-tuning.

12. **Q: Jak działa A/B testing w SageMaker?** A: Multi-variant endpoint z wagami (np. 90/10 traffic split) — TrafficRoutingPolicy per variant.

13. **Q: Co to jest SageMaker Model Monitor?** A: Automatyczne monitorowanie dryfu danych i jakości modelu na produkcji — CloudWatch alarms gdy próg przekroczony.

14. **Q: Jakie typy dryftu wykrywa Model Monitor?** A: Data quality drift, model quality drift, bias drift, feature attribution drift.

15. **Q: Co to jest SageMaker Clarify?** A: Narzędzie do wykrywania bias i explainability (SHAP values) — przed i po trenowaniu.

16. **Q: Jaka instancja jest najtańsza do trenowania DL?** A: ml.trn1.* (AWS Trainium) — do 50% taniej niż p3.

17. **Q: Jaka instancja jest najtańsza do inference DL?** A: ml.inf1.* / ml.inf2.* (AWS Inferentia) — do 70% taniej niż GPU.

18. **Q: Co to jest SageMaker Neo?** A: Kompiluje modele (TF, PyTorch, MXNet, ONNX) do zoptymalizowanego kodu dla docelowego hardware (edge, cloud).

19. **Q: Co to jest SageMaker Ground Truth?** A: Human-in-the-loop data labeling — Mechanical Turk, prywatna workforce lub vendor workforce.

20. **Q: Co to jest SageMaker HyperPod?** A: Managed klaster do distributed training bardzo dużych modeli (LLM). Automatyczne recovery węzłów.

21. **Q: Jakie są 4 typy inference w SageMaker?** A: Real-Time, Serverless, Batch Transform, Async Inference.

22. **Q: Kiedy Async Inference?** A: Duże payloady (video, audio, duże dokumenty) wymagające >60s przetwarzania. Wyniki do S3.

23. **Q: Co to jest SageMaker Experiments?** A: Tracking eksperymentów ML — parametry, metryki, artefakty per run. Analogia MLflow.

24. **Q: Czy SageMaker jest regionalny?** A: Tak — wszystkie zasoby (Studio domain, endpoints, training jobs) są regionalne.

25. **Q: Jak SageMaker przechowuje notebooki w Studio?** A: EFS (Elastic File System) — persistowane między sesjami, per user.

26. **Q: Co to jest Amazon Rekognition?** A: Managed CV service — detekcja obiektów, twarzy, tekstu w obrazach i wideo.

27. **Q: Różnica Rekognition a Textract?** A: Rekognition = obiekty/twarze/sceny. Textract = ekstrakcja strukturyzowanego tekstu z dokumentów (tabele, formularze).

28. **Q: Co to jest Amazon Comprehend?** A: Managed NLP — sentiment, entities, key phrases, PII detection, language detection.

29. **Q: Co to jest Comprehend Medical?** A: Specjalizacja Comprehend dla tekstu medycznego — diagnoza, leki, dawki, procedury, anatonia.

30. **Q: Co to jest Amazon Textract?** A: OCR + document understanding — wyciąga tekst, tabele i key-value pairs z PDF/obrazów.

31. **Q: Jak wysłać duży dokument do Textract?** A: `StartDocumentAnalysis` (async) z SNS notification → S3 output. Dla małych: synchroniczny `AnalyzeDocument`.

32. **Q: Co to jest Amazon Polly?** A: Text-to-Speech — konwertuje tekst na naturalnie brzmiącą mowę. Wspiera SSML.

33. **Q: Jakie są typy głosów Polly?** A: Standard, Neural (NTTS), Long-form, Generative.

34. **Q: Co to jest Amazon Lex?** A: Managed chatbot/konwersacyjne AI — intents, slots, fulfillment przez Lambda. Rdzeń Alexa.

35. **Q: Kiedy Lex zamiast Bedrock dla chatbota?** A: Lex dla strukturyzowanych dialogów, IVR, call center (integracja z Connect). Bedrock dla otwartych konwersacji, LLM reasoning.

36. **Q: Co to jest Amazon Kendra?** A: Managed enterprise search z NLP — semantyczne wyszukiwanie w dokumentach korporacyjnych.

37. **Q: Różnica Kendra vs Bedrock Knowledge Bases?** A: Kendra = enterprise search (keyword + semantic), drogi. Bedrock KB = RAG dla LLM, tańszy, nowszy.

38. **Q: Co to jest Amazon Forecast?** A: Managed time-series forecasting — predykcja popytu, zapasów, ruchu. DeepAR+ pod spodem.

39. **Q: Co to jest Amazon Personalize?** A: Managed system rekomendacji — user-item interactions, real-time personalizacja jak na Amazon.com.

40. **Q: Jakie przepisy (recipes) oferuje Personalize?** A: User-Personalization, Popularity-Count, HRNN-Metadata, Similar-Items, Personalized-Ranking.

41. **Q: Co to jest Amazon Translate?** A: Neural Machine Translation — 75+ języków, custom terminology dla terminologii branżowej.

42. **Q: Co to jest Amazon Transcribe?** A: Speech-to-Text — transkrypcja audio/video. Speaker diarization, Custom Vocabulary, PII Redaction.

43. **Q: Różnica Transcribe a Transcribe Medical?** A: Transcribe Medical wyspecjalizowany w terminologii medycznej (diagnoza, leki, nazwy anatomiczne).

44. **Q: Co to jest AWS Panorama?** A: Edge computer vision — urządzenie + SDK do ML inference na kamerach IP bez wysyłania wideo do chmury.

45. **Q: Co to jest Amazon Lookout for Metrics?** A: Anomaly detection dla metryk biznesowych — automatycznie wykrywa anomalie bez ML expertise.

46. **Q: Co to jest AWS Trainium?** A: Custom chip AWS wyłącznie do trenowania DL — do 50% taniej niż GPU p3/p4.

47. **Q: Co to jest AWS Inferentia?** A: Custom chip AWS wyłącznie do inference DL — do 70% taniej niż GPU. inf1 (stary), inf2 (nowy).

48. **Q: Czym różni się Inferentia od Trainium?** A: Trainium = trening. Inferentia = inference. Oba tańsze niż GPU dla DL.

49. **Q: Co to jest Deep Learning AMI?** A: Pre-konfigurowane AMI z frameworkami ML (TF, PyTorch, CUDA) — szybki start bez konfiguracji.

50. **Q: Co to jest SageMaker Inference Recommender?** A: Testuje model na różnych typach instancji i rekomenduje optymalny pod kątem kosztu i latencji.

51. **Q: Jak zabezpieczyć dane treningowe w SageMaker?** A: S3 + KMS CMK, VPC dla dostępu do danych, EBS volume encryption, inter-container TLS.

52. **Q: Co to jest Bedrock Guardrails?** A: Filtry treści dla modeli Bedrock — NSFW, hate speech, PII anonymization/blocking.

53. **Q: Co to jest Bedrock Knowledge Bases?** A: Managed RAG — indeksuje dokumenty (S3), tworzy embeddings, przechowuje w vector store (OpenSearch Serverless), odpowiada na pytania przez FM.

54. **Q: Jakie vector stores wspiera Bedrock Knowledge Bases?** A: OpenSearch Serverless, Pinecone, Redis Enterprise Cloud, MongoDB Atlas, Aurora PostgreSQL (pgvector).

55. **Q: Co to jest Bedrock Agents?** A: Autonomiczne agenty AI — planują, wywołują narzędzia (Lambda/APIs), obsługują multi-step tasks.

56. **Q: Co to jest SageMaker Model Registry?** A: Centralne repozytorium wersji modeli — approval workflow (pending/approved/rejected), cross-account sharing.

57. **Q: Jak wdrożyć model zatwierdzony w Model Registry?** A: Pipeline reaction na zmianę statusu modelu → EventBridge → Lambda/Pipeline → create endpoint.

58. **Q: Jak działa distributed training Data Parallel?** A: Każdy węzeł ma pełny model, różne batch danych → gradienty synchronizowane między węzłami.

59. **Q: Kiedy potrzebne Model Parallel training?** A: Model za duży na 1 GPU (np. 70B parametrów) → model podzielony między kilka GPU/węzłów.

60. **Q: Firma chce wykrywać uszkodzenia produktów na taśmie produkcyjnej. Najlepsza usługa?** A: Amazon Lookout for Vision — specjalizowany w detekcji defektów wizualnych w produkcji przemysłowej. Alternatywnie Rekognition Custom Labels.

---
**Pytanie 1:** Czym różni się AWS Backup od natywnych snapshotów EBS?

> AWS Backup to centralny, policy-based service zarządzający backupami wielu usług jednocześnie. Natywne snapshoty EBS są tworzone ręcznie lub przez Data Lifecycle Manager tylko dla EBS. AWS Backup dodaje: cross-account, cross-region, Vault Lock, Audit Manager, Backup Policies przez Organizations.

---

**Pytanie 2:** Jak wymusić backup na wszystkich kontach w Organization bez nadawania uprawnień IAM użytkownikom tych kont?

> Użyj **Backup Policies** przez AWS Organizations. Działają podobnie do SCP — stosowane do OU lub kont, nie można ich nadpisać. Management account definiuje plan backup i wymusza go na wszystkich kontach w OU.

---

**Pytanie 3:** Co to jest Backup Vault Lock i kiedy użyć Compliance Mode?

> Vault Lock = WORM protection — Recovery Points nie mogą być usunięte przed retention period. **Compliance Mode**: nieodwracalne, żaden principal (nawet root) nie może usunąć backupu. Używaj dla HIPAA, PCI DSS, SEC Rule 17a-4, FINRA.

---

**Pytanie 4:** Jaka jest różnica między Governance Mode a Compliance Mode w Vault Lock?

> **Governance Mode**: admin z IAM może wyłączyć lock (przez 3-dniowy cooling period). **Compliance Mode**: po upływie cooling period (3 dni) — **nieodwracalne**, nikt nie może wyłączyć ani usunąć recovery pointów przed retention.

---

**Pytanie 5:** Jak skonfigurować cross-region backup dla RDS?

> W Backup Rule dodaj `CopyActions` z `DestinationBackupVaultArn` wskazującym na vault w DR regionie. Vault musi istnieć przed skonfigurowaniem copy action.

---

**Pytanie 6:** Czy AWS Backup obsługuje S3? Jakie są wymagania?

> Tak. Wymaganie: **S3 Versioning musi być włączone** na buckecie. AWS Backup tworzy ciągły backup (continuous backup) umożliwiający restore do dowolnego punktu w czasie (PITR — Point-In-Time Recovery).

---

**Pytanie 7:** Czym jest Recovery Point w AWS Backup?

> Recovery Point = pojedyncza kopia zapasowa zasobu (snapshot EBS, backup RDS, kopia EFS). Ma własny ARN, datę utworzenia, status, rozmiar i czas retencji. Przechowywany w Backup Vault.

---

**Pytanie 8:** Jak AWS Backup chroni przed ransomware?

> Cross-account backup do dedykowanego backup account + Vault Lock Compliance Mode. Nawet jeśli konto prod zostanie zaatakowane (ransomware zaszyfruje dane, usunie snapshoty), backup account jest oddzielny — atakujący nie ma dostępu do Vault Lock protected recovery points.

---

**Pytanie 9:** Jaką rolę IAM używa AWS Backup do tworzenia backupów?

> `AWSBackupDefaultServiceRole` — AWS-managed role z uprawnieniami do tworzenia/usuwania snapshotów, backupów RDS, DynamoDB etc. Można też użyć custom role z minimalnymi uprawnieniami (least privilege).

---

**Pytanie 10:** Czy AWS Backup obsługuje VMware on-premises?

> Tak — przez **AWS Backup Gateway** (wirtualny appliance OVA instalowany na vSphere). Backup VMs do AWS Backup Vault. Wymaga vSphere 6.7+.

---

**Pytanie 11:** Co to jest Backup Selection i jakie są sposoby wyboru zasobów?

> Backup Selection definiuje które zasoby są objęte planem. Trzy metody: (1) **By tag** — wszystkie zasoby z określonym tagiem, (2) **By ARN** — konkretne zasoby, (3) **Wildcard ARN** — np. wszystkie instancje EC2 `arn:aws:ec2:*:*:instance/*`.

---

**Pytanie 12:** Jak monitorować nieudane backup jobs?

> CloudWatch metryki: `NumberOfBackupJobsFailed`, `NumberOfCopyJobsFailed`. EventBridge rules na event `AWS Backup Backup Job State Change` → SNS/Lambda. Sprawdź też AWS Backup console → Jobs.

---

**Pytanie 13:** Czym jest AWS Backup Audit Manager?

> Usługa weryfikacji zgodności polityk backupowych z frameworkami compliance (CIS, HIPAA, PCI DSS, SOC2). Automatycznie sprawdza kontrole (np. "czy każdy zasób ma backup plan?") i generuje raporty do S3.

---

**Pytanie 14:** Ile Backup Plans można mieć per account?

> **100** Backup Plans per account (soft limit, można zwiększyć przez Service Quotas).

---

**Pytanie 15:** Jak działa lifecycle w Backup Rule?

> `MoveToColdStorageAfterDays` — przenieś Recovery Point do cold storage (tańszy, wolniejszy dostęp) po N dniach. `DeleteAfterDays` — usuń Recovery Point po N dniach. Cold storage: min 90 dni przechowywania, pierwsze 90 dni liczone od przeniesienia.

---

**Pytanie 16:** Czy można przywrócić backup cross-account bezpośrednio?

> Nie bezpośrednio. Najpierw skopiuj Recovery Point do vaultu w docelowym koncie (`copy-job`), potem uruchom `start-restore-job` w docelowym koncie.

---

**Pytanie 17:** Jak AWS Backup obsługuje DynamoDB?

> On-demand backup (pełny snapshot) + **continuous backup** (PITR — Point-In-Time Recovery do 35 dni wstecz). Item-level restore: możesz przywrócić konkretną tabelę do nowej tabeli. Wymaga włączenia PITR w planie.

---

**Pytanie 18:** Co to jest StartWindowMinutes w Backup Rule?

> Czas (minuty) jaki AWS Backup czeka na start backupu po zaplanowanej godzinie. Jeśli backup nie zacznie się w tym oknie — job failed. Domyślnie 60 min. Zwiększ dla dużych, wolnych środowisk.

---

**Pytanie 19:** Czym różni się AWS Backup dla EFS od EFS natywnych backupów?

> AWS Backup dla EFS używa inkrementalnych backupów (szybsze, tańsze). Natywny backup EFS nie istnieje — tylko przez AWS Backup lub Rsync/DataSync. AWS Backup EFS wspiera item-level restore (konkretne pliki/katalogi).

---

**Pytanie 20:** Czy Backup Vaults są globalne czy regionalne?

> **Regionalne** — każdy vault jest w konkretnym regionie. Dane nie opuszczają regionu bez Copy Action. Masz osobne vaultsy w us-east-1 i eu-west-1.

---

**Pytanie 21:** Jak włączyć continuous backup dla RDS?

> W Backup Rule ustaw `EnableContinuousBackup: true`. Wymaga RDS w trybie automated backup. Umożliwia PITR do dowolnego punktu w ostatnich 35 dniach.

---

**Pytanie 22:** Jak AWS Backup Vault Lock różni się od S3 Object Lock?

> Analogiczna koncepcja (WORM) ale dla różnych zasobów. S3 Object Lock chroni obiekty S3. Vault Lock chroni **Recovery Points** w Backup Vault (snapshoty EC2, RDS, EFS itd). Oba mają Governance i Compliance mode.

---

**Pytanie 23:** Czy AWS Backup obsługuje Redshift?

> Tak. AWS Backup może tworzyć snapshoty Redshift cluster i Redshift Serverless namespace. Cross-region i cross-account copy obsługiwane.

---

**Pytanie 24:** Jak sprawdzić status backup job?

```bash
aws backup list-backup-jobs \
  --by-state FAILED \
  --by-resource-type RDS
```

---

**Pytanie 25:** Jakie są koszty AWS Backup?

> Per GB backed up (~$0.05/GB warm, ~$0.01/GB cold) + restore (~$0.02/GB). Dodatkowe koszty: cross-region data transfer, DynamoDB PITR ($0.20/GB), Audit Manager ($1/resource/miesiąc). Brak opłaty za Backup Plans/Rules/Selections.

---

**Pytanie 26:** Jak AWS Backup integruje się z AWS Config?

> AWS Config Rule `BACKUP_PLAN_EXISTS` sprawdza czy zasób ma backup plan. Możesz użyć Config + AWS Backup Audit Manager do wykrywania zasobów bez wymaganego backupu.

---

**Pytanie 27:** Co to jest CompletionWindowMinutes?

> Maksymalny czas (minuty) na ukończenie backup job. Jeśli backup nie skończy się w tym oknie — AWS Backup oznaczy job jako `EXPIRED`. Domyślnie: 8 godzin (480 min). Zwiększ dla bardzo dużych woluminów EBS lub baz danych.

---

**Pytanie 28:** Jak chronić Backup Vault przed przypadkowym usunięciem recovery points przez admina?

> Użyj **Vault Access Policy** z `Deny` na `backup:DeleteRecoveryPoint` dla wszystkich principalów oprócz dedykowanej roli. Lub włącz **Vault Lock** — Governance Mode dla elastyczności, Compliance Mode dla pełnej ochrony.

---

**Pytanie 29:** Czy AWS Backup obsługuje FSx for Windows z VSS?

> Tak. AWS Backup dla FSx for Windows File Server obsługuje **application-consistent backups** przez VSS (Volume Shadow Copy Service). Zapewnia spójność danych dla SQL Server, Exchange działających na FSx.

---

**Pytanie 30:** Jak AWS Backup różni się od AWS DRS (Disaster Recovery Service)?

> **AWS Backup**: kopie zapasowe zasobów (snapshoty, backupy), restore trwa minuty-godziny. **AWS DRS (CloudEndure)**: continuous replication bloków dyskowych, failover w minutach, działa jak hot standby. Backup = RPO godziny, RTO minuty-godziny. DRS = RPO sekundy, RTO ~15 min.

---

**Pytanie 31:** Jakie usługi obsługuje item-level restore w AWS Backup?

> **DynamoDB** (table/item), **EFS** (file/directory), **FSx for Windows** (file/folder przez VSS), **S3** (object-level).

---

**Pytanie 32:** Jak działa Backup Selection po tagach dla nowych zasobów?

> Automatycznie — gdy nowy zasób (np. EC2) zostaje uruchomiony z tagiem `BackupPolicy=daily`, AWS Backup automatycznie obejmuje go backupem. Nie trzeba ręcznie dodawać do planu. To główna zaleta tag-based selection.

---

**Pytanie 33:** Jaka jest minimalna częstotliwość backup w AWS Backup?

> 1 godzina (cron expression lub rate expression). Dla ciągłego backupu (PITR): continuous backup dla RDS/Aurora, S3, DynamoDB.

---

**Pytanie 34:** Czy AWS Backup może backupować zasoby w innych regionach?

> Nie — AWS Backup tworzy backupy **w tym samym regionie** gdzie są zasoby. Copy Action kopiuje Recovery Point do innego regionu *po* stworzeniu backupu. Backup musi być inicjowany z regionu zasobu.

---

**Pytanie 35:** Co to jest Backup Gateway i czym różni się od Storage Gateway?

> **Backup Gateway**: wirtualny appliance do backupu VMware VMs do AWS Backup. **Storage Gateway**: hybrydowy storage (S3, EBS volumes, Tape) dla on-premises aplikacji. Backup Gateway = tylko backup, Storage Gateway = hybrydowy storage.

---

**Pytanie 36:** Jak AWS Backup obsługuje zaszyfrowane zasoby (CMK)?

> Backup szyfrowanych zasobów wymaga dostępu do klucza KMS. Vault może używać innego CMK niż zasób. W cross-account backup: docelowe konto musi mieć dostęp do CMK z konta źródłowego lub użyć własnego CMK do re-encryption.

---

**Pytanie 37:** Jak ustawić alert gdy backup job się nie powiedzie?

```bash
# EventBridge rule na failed backup job
aws events put-rule \
  --name backup-failure-alert \
  --event-pattern '{
    "source": ["aws.backup"],
    "detail-type": ["Backup Job State Change"],
    "detail": {"state": ["FAILED", "EXPIRED", "ABORTED"]}
  }'
# Target: SNS topic → email/PagerDuty
```

---

**Pytanie 38:** Czy możesz usunąć Backup Vault z recovery points w środku?

> Nie — Vault musi być **pusty** (zero recovery points) przed usunięciem. Najpierw usuń wszystkie recovery points lub poczekaj na wygaśnięcie retention. Z Vault Lock Compliance Mode — nie możesz usunąć recovery points → nie możesz usunąć vaultu przed retention.

---

**Pytanie 39:** Czym jest `AWSBackupDefaultServiceRole`?

> AWS-managed IAM role z uprawnieniami dla AWS Backup do: tworzenia/usuwania snapshotów EBS, backupów RDS, DynamoDB, EFS, FSx. Tworzona automatycznie przy pierwszym użyciu AWS Backup. Można użyć custom role z minimum permissions.

---

**Pytanie 40:** Jak działa retention w cold storage dla AWS Backup?

> Po przeniesieniu do cold storage: **minimum 90 dni** przechowywania w cold (liczonych od przeniesienia). `DeleteAfterDays` musi być ≥ `MoveToColdStorageAfterDays + 90`.

---

**Pytanie 41:** Jakie usługi AWS Backup obsługuje dla compliance HIPAA?

> AWS Backup jest HIPAA-eligible. Audit Manager ma wbudowany HIPAA framework. Vault Lock Compliance Mode spełnia wymóg nienaruszalności backupów. Szyfrowanie KMS CMK. Cross-account isolation backup.

---

**Pytanie 42:** Jak AWS Backup integruje się z AWS CloudTrail?

> Wszystkie API calls AWS Backup są logowane w CloudTrail: CreateBackupJob, DeleteRecoveryPoint, StartRestoreJob, PutBackupVaultLockConfiguration. Audytowanie kto/kiedy stworzył/usunął backup.

---

**Pytanie 43:** Czy można tagować Recovery Points?

> Tak. AWS Backup automatycznie kopiuje tagi z zasobu do Recovery Point. Można też dodać własne tagi do Recovery Points niezależnie od zasobu. Ułatwia wyszukiwanie i organizację backupów.

---

**Pytanie 44:** Czym jest Backup Window?

> Czas (UTC) w którym AWS Backup preferuje uruchomić backup job. Definiowany przez `StartWindowMinutes` od `ScheduleExpression`. Uruchamiaj backupy poza godzinami szczytowymi produkcji.

---

**Pytanie 45:** Jak AWS Backup wspiera RTO/RPO?

> **RPO**: Continuous backup (PITR) dla RDS/S3/DynamoDB = RPO ~sekundy. Hourly backup = RPO 1h. Daily backup = RPO 24h. **RTO**: Cross-region backup pozwala restore w DR regionie. Czas restore zależy od rozmiaru zasobu (EBS snapshot restore = natychmiastowe, RDS = minuty-godziny).

---

**Pytanie 46:** Czy AWS Backup może zastąpić AWS DLM (Data Lifecycle Manager)?

> Częściowo. AWS Backup obsługuje EBS snapshots i EC2 AMI. DLM to prostsze, tańsze rozwiązanie tylko dla EBS/EC2 bez zarządzania cross-account/cross-service. Dla multi-service/multi-account: AWS Backup. Dla prostego EBS w jednym koncie: DLM lub AWS Backup — oba działają.

---

**Pytanie 47:** Jak ograniczyć kto może tworzyć backup jobs do vaultu?

> Vault Access Policy — resource-based policy na vault. Możesz zezwolić tylko określonym rolom/kontom na `backup:StartBackupJob`, `backup:CopyIntoBackupVault` etc.

---

**Pytanie 48:** Jak AWS Backup obsługuje Aurora?

> Aurora Cluster Snapshot (nie per-instance). Continuous backup (PITR do 35 dni) jeśli enabled. Cross-region copy obsługiwane. Nie możesz robić backup pojedynczego Aurora instance — tylko klaster.

---

**Pytanie 49:** Jak sprawdzić wszystkie zasoby bez backup planu?

> AWS Backup Audit Manager → Control: `BACKUP_RESOURCES_PROTECTED_BY_BACKUP_PLAN`. Lub AWS Config rule: `backup-plan-min-frequency-and-min-retention-check`. Zwróci listę niezgodnych zasobów.

---

**Pytanie 50:** Czy AWS Backup obsługuje Neptune?

> Tak. Neptune cluster snapshots przez AWS Backup. Cross-region i cross-account copy. Nie obsługuje item-level restore (przywraca cały klaster Neptune).

---

**Pytanie 51:** Co to jest "Warm Storage" i "Cold Storage" w AWS Backup?

> **Warm storage**: szybki dostęp, restore w minutach/godzinach, ~$0.05/GB. **Cold storage**: tańszy (~$0.01/GB), czas do pierwszego bajtu restore dłuższy (podobny do Glacier). Lifecycle policy przenosząca do cold po N dniach oszczędza koszty przechowywania długoterminowych backupów.

---

**Pytanie 52:** Jak AWS Backup różni się od AWS Backup for S3 vs S3 Cross-Region Replication?

> **AWS Backup for S3**: kopia zapasowa zawartości bucketu, PITR, przechowywana w Backup Vault. **S3 CRR**: synchronizacja obiektów między bucketami w czasie rzeczywistym (nie backup). CRR replikuje każdą zmianę/usunięcie — delete marker też. Backup for S3 zachowuje historię — możesz przywrócić usunięty obiekt.

---

**Pytanie 53:** Ile Recovery Points można przechowywać w jednym vault?

> Brak twardego limitu (soft: miliony recovery points). Praktyczny limit — koszt storage. AWS zaleca jednego vault per region per account dla większości przypadków.

---

**Pytanie 54:** Jak działa Vault Lock cooling period?

> Po wywołaniu `PutBackupVaultLockConfiguration` vault jest w stanie "pending lock" przez **3 dni** (72 godziny). W tym czasie możesz zmienić konfigurację lub usunąć lock. Po 3 dniach — lock wchodzi w życie (dla Compliance Mode: nieodwracalnie). W Governance Mode: można wyłączyć z odpowiednimi IAM uprawnieniami.

---

**Pytanie 55:** Jak AWS Backup obsługuje DocumentDB?

> AWS Backup tworzy snapshoty DocumentDB cluster. Cross-region i cross-account copy. Restore do nowego klastra DocumentDB. Continuous backup nie jest dostępny dla DocumentDB (tylko on-demand snapshoty przez Backup Plan).

---

**Pytanie 56:** Czy można wykonać natychmiastowy (on-demand) backup poza harmonogramem?

```bash
aws backup start-backup-job \
  --backup-vault-name prod-vault \
  --resource-arn arn:aws:rds:us-east-1:123456789:db:prod-mysql \
  --iam-role-arn arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole \
  --lifecycle '{"DeleteAfterDays": 7}'
```
> Tak — `start-backup-job` API. Przydatne przed dużymi zmianami (deployment, migracja).

---

**Pytanie 57:** Jak Backup Policies w Organizations różnią się od Backup Plans?

> **Backup Plans**: tworzone i zarządzane w konkretnym koncie AWS. **Backup Policies**: definiowane na poziomie Organization (management account), automatycznie stosowane do kont/OU. Backup Policy = mechanizm dystrybucji Backup Plan do wielu kont.

---

**Pytanie 58:** Co sprawdza kontrola `BACKUP_RECOVERY_POINT_ENCRYPTED` w Audit Manager?

> Weryfikuje czy wszystkie recovery points w vaultach są zaszyfrowane. Jeśli vault używa default AWS managed key = compliant. Jeśli vault nie ma szyfrowania = non-compliant. Ważna kontrola dla PCI DSS i HIPAA.

---

**Pytanie 59:** Jaka jest różnica między `backup:CreateBackupVault` a `backup:CopyIntoBackupVault`?

> `CreateBackupVault`: uprawnienie do tworzenia nowych vaultów. `CopyIntoBackupVault`: uprawnienie do kopiowania Recovery Points **do** tego vaultu z innego konta/regionu. Kluczowe dla cross-account backup — konto źródłowe potrzebuje `CopyIntoBackupVault` na docelowym vaulcie.

---

**Pytanie 60:** Firma musi udowodnić że backupy nie były zmodyfikowane przez ostatnie 7 lat (SEC compliance). Jak skonfigurować AWS Backup?

> 1. Utwórz dedykowane konto backup (oddzielone od prod). 2. Utwórz Backup Vault z KMS CMK. 3. Włącz **Vault Lock Compliance Mode** z `min-retention-days=2555` (7 lat) i `max-retention-days=2556`. 4. Poczekaj cooling period (3 dni) — lock staje się nieodwracalny. 5. Backup Policies przez Organizations wymuszają backup z kont prod do tego vault. 6. CloudTrail + Audit Manager generują raporty compliance. 7. Żaden principal nie może usunąć recovery points — spełnia SEC Rule 17a-4.

---
## 19.20 Checks — 60 Pytań z Odpowiedziami

**1. Jaka jest maksymalna wielkość obiektu w S3?**
> 5 TB. Pojedynczy PUT max 5 GB → powyżej wymagany Multipart Upload.

**2. S3 jest serwisem regionalnym czy globalnym?**
> Dane w buckecie są **regionalne**. Namespace bucketów jest **globalny** (unikalna nazwa na świecie).

**3. Która klasa S3 ma najdłuższy minimalny czas przechowywania?**
> **S3 Glacier Deep Archive** — 180 dni.

**4. Czy S3 Standard-IA ma opłatę za pobranie danych?**
> Tak — opłata per GB przy retrieve. Dlatego używana dla rzadkiego dostępu z dużymi obiektami.

**5. Co się dzieje gdy usuniesz obiekt z włączonym wersjonowaniem bez podania Version ID?**
> Tworzony jest **Delete Marker** — obiekt jest "ukryty" ale nie usunięty. Wersje nadal istnieją.

**6. Jakie wymagania musi spełniać bucket aby działała replikacja S3?**
> Wersjonowanie musi być włączone na **obu** bucketach (source i destination).

**7. Czy obiekty istniejące przed włączeniem replikacji są automatycznie replikowane?**
> **Nie** — replikuje się tylko nowe obiekty po włączeniu. Stare wymagają S3 Batch Replication.

**8. Jaka jest różnica między S3 Object Lock Compliance a Governance?**
> **Compliance** — nikt (nawet root) nie może usunąć przed końcem okresu. **Governance** — można obejść z uprawnieniem `s3:BypassGovernanceRetention`.

**9. Czym różni się SSE-KMS od SSE-S3?**
> SSE-KMS używa CMK (Customer Managed Key) w AWS KMS — daje audit w CloudTrail i kontrolę nad kluczem. SSE-S3 używa klucza zarządzanego przez S3, brak auditu per-object.

**10. Dlaczego SSE-KMS może powodować throttling przy dużym ruchu S3?**
> Każdy GET/PUT = wywołanie KMS API. KMS ma limity (5,500–30,000 req/s). Rozwiązanie: **S3 Bucket Key** (jeden klucz per bucket, ~99% redukcja KMS calls).

**11. Co to jest S3 Transfer Acceleration?**
> Upload przez CloudFront edge locations → prywatna sieć szkieletowa AWS → bucket. Szybszy dla globalnych klientów. Dodatkowy koszt.

**12. Kiedy Multipart Upload jest wymagany?**
> Obiekty **> 5 GB** — wymagany. Zalecany dla > 100 MB.

**13. Ile requestów GET na sekundę może obsłużyć jeden prefix S3?**
> 5,500 GET/HEAD per sekundę per prefix. Dla większego ruchu: użyj wielu prefiksów.

**14. Co to jest S3 Select?**
> SQL query na pojedynczym obiekcie S3 (CSV, JSON, Parquet). Pobiera tylko potrzebne dane — tańszy transfer. Max 5 GB obiekt.

**15. Kiedy S3 Select vs Athena?**
> S3 Select = jeden obiekt. Athena = wiele obiektów, wiele tabel, złożone query.

**16. Co to jest Requester Pays w S3?**
> Koszt transferu danych ponosi osoba pobierająca (requester), nie właściciel bucketa. Requester musi być uwierzytelniony.

**17. Czy S3 Static Website Hosting obsługuje HTTPS?**
> **Nie** — tylko HTTP. HTTPS wymaga CloudFront przed S3.

**18. Co to jest S3 Intelligent-Tiering?**
> Automatyczne przenoszenie obiektów między tierami (frequent → infrequent → archive) na podstawie wzorca dostępu. Opłata monitoringowa, bez opłaty za retrieve.

**19. Jaka jest minimalna wielkość obiektu dla S3 Standard-IA?**
> Technicznie brak minimum, ale obiekty < 128 KB są naliczane jak 128 KB — nieopłacalne dla małych plików.

**20. Co to jest Legal Hold w S3 Object Lock?**
> Niezależna od retention period blokada — uniemożliwia usunięcie dopóki aktywna. Może być włączona/wyłączona w dowolnym momencie (wymaga odpowiedniego IAM uprawnienia).

**21. Czy Delete Markers są replikowane przy CRR?**
> Domyślnie **nie**. Można włączyć `DeleteMarkerReplication` w konfiguracji replikacji.

**22. Co to jest Replication Time Control (RTC)?**
> Gwarancja replikacji 99.99% obiektów w **15 minutach**. Dodatkowy koszt. Daje metryki w CloudWatch.

**23. Ile bucketów można mieć per konto AWS?**
> Domyślnie **100** (soft limit), max **1000** po złożeniu support case.

**24. Co to jest S3 Multi-Region Access Point?**
> Globalny endpoint routujący requesty do najbliższego bucketa. Obsługuje failover między regionami. Działa z replikacją bi-directional.

**25. Jak wymusić szyfrowanie SSE-KMS na buckecie?**
> Bucket policy z `Deny` gdy `s3:x-amz-server-side-encryption != "aws:kms"`.

**26. Co to jest S3 Batch Operations?**
> Zarządzana usługa do operacji na miliardach obiektów jednocześnie: kopiowanie, tagowanie, Object Lock, wywołanie Lambda, restore z Glacier.

**27. Co to jest S3 Storage Lens?**
> Organizational-wide dashboard widoczności S3: rozmiar, liczba obiektów, koszty, aktywność — na poziomie konta, regionu, bucketa.

**28. Kiedy używać S3 One Zone-IA?**
> Dla danych **odtwarzalnych** (np. thumbnails, re-processable cache) z rzadkim dostępem. Jeden AZ = tańszy ale brak resilience na utratę AZ.

**29. Jakie są typy retrieval w Glacier Flexible Retrieval?**
> Expedited (1-5 min), Standard (3-5h), Bulk (5-12h). Expedited = najdroższy.

**30. Jak chronić bucket S3 przed ransomware?**
> Kombinacja: Object Lock (WORM), Versioning, MFA Delete, Block Public Access, Macie (wykrycie anomalii), CloudTrail (audit).

**31. Co to jest S3 Access Point?**
> Named endpoint z własną polityką dostępu, przypisany do bucketa. Umożliwia granularną kontrolę dostępu per aplikacja/VPC.

**32. Jaka jest maksymalna ważność Presigned URL dla IAM Role?**
> Presigned URL wygasa gdy wygasną credentials roli (max 12h dla AssumeRole). Dla IAM user (long-term credentials): max 7 dni.

**33. Co to jest S3 Object Ownership i jakie ma tryby?**
> Kontroluje kto jest właścicielem uploadzonych obiektów. Tryby: `BucketOwnerEnforced` (ACL wyłączone), `BucketOwnerPreferred`, `ObjectWriter`.

**34. Jaka jest domyślna enkrypcja S3 od 2023?**
> Od stycznia 2023 — domyślnie **SSE-S3** (AES-256) dla wszystkich nowych obiektów.

**35. Co to jest S3 Glacier Instant Retrieval?**
> Klasa dla archiwalnych danych wymagających **natychmiastowego** dostępu (ms latency). Tańsza niż Standard, droższa niż Flexible. Min 90 dni przechowywania.

**36. Jak działa S3 Lifecycle — czy można przejść z Glacier do Standard przez lifecycle?**
> **Nie** — lifecycle transitions są jednokierunkowe (waterfall w dół). Przywrócenie z Glacier to ręczny restore, nie lifecycle.

**37. Co musi być włączone zanim użyjesz S3 Object Lock?**
> Wersjonowanie — musi być włączone **przy tworzeniu bucketa** (nie można włączyć Object Lock po fakcie bez kontaktu z AWS Support).

**38. Czy S3 CRR replikuje obiekty zaszyfrowane SSE-C?**
> **Nie** — obiekty z SSE-C (customer-provided key) nie są replikowane.

**39. Jak zoptymalizować koszty KMS przy S3 SSE-KMS?**
> Włącz **S3 Bucket Key** — jeden klucz danych per bucket, redukuje wywołania KMS API o ~99%.

**40. Co to jest S3 Block Public Access i na jakim poziomie działa?**
> Nadrzędne ustawienie blokujące publiczny dostęp do S3. Działa na poziomie **konta** (blokuje wszystkie buckety) i **bucketa**. Nadrzędne nad bucket policy i ACL.

**41. Jaka jest różnica między S3 EventBridge a bezpośrednim powiadomieniem SNS/SQS/Lambda?**
> EventBridge = zaawansowane filtrowanie (po metadata, typie zdarzenia), routing do 20+ serwisów, archiwizacja eventów. SNS/SQS/Lambda = prostsze, tańsze, niższe latency.

**42. Co to jest DSSE-KMS?**
> Dual-layer SSE-KMS — dwa niezależne klucze KMS szyfrują dane. Dla wymagań compliance (CJIS, ITAR). Podwójny koszt KMS.

**43. Jak działa S3 Transfer Acceleration technicznie?**
> Upload do najbliższego CloudFront edge location → prywatna sieć szkieletowa AWS (nie publiczny internet) → bucket w docelowym regionie.

**44. Czy S3 Replication działa między różnymi kontami AWS?**
> Tak — wymaga dodatkowych uprawnień w bucket policy na destination i odpowiedniej IAM role z trust policy.

**45. Kiedy użyć S3 Glacier Deep Archive zamiast Flexible Retrieval?**
> Gdy dane są potrzebne raz na rok lub rzadziej, akceptowalny czas retrieval 12-48h, wymagana długoterminowa retencja (7-10 lat). Najtańsza klasa S3.

**46. Co to jest S3 Inventory?**
> Tygodniowy/dzienny raport CSV/ORC/Parquet wszystkich obiektów w buckecie. Źródło dla S3 Batch Operations i analiz.

**47. Jak działa MFA Delete?**
> Wymaga token MFA przy: trwałym usunięciu wersji obiektu i wyłączeniu wersjonowania. Włączyć może tylko root account przez AWS CLI.

**48. Co to jest S3 Replication ownership override?**
> Opcja wymuszająca zmianę właściciela replikowanych obiektów na właściciela destination bucketa. Ważne przy cross-account replication.

**49. Jakie formaty obsługuje S3 Select?**
> CSV, JSON, Apache Parquet. Kompresja: GZIP, BZIP2 (dla CSV/JSON).

**50. Co to jest S3 Object Tagging?**
> Key-value metadata przypisane do obiektu (max 10 tagów). Używane w: lifecycle rules, bucket policies (warunki), cost allocation, S3 Batch Operations.

**51. Jak S3 obsługuje eventual consistency?**
> Od 2020: S3 zapewnia **strong consistency** dla wszystkich operacji PUT/DELETE — natychmiastowo spójne po zapisie. Nie ma już "read after write" problemu.

**52. Co to jest S3 Glacier Vault Lock?**
> Compliance WORM policy dla Glacier Vault (nie S3 bucket). Dla legacy Glacier — S3 Object Lock jest nowoczesnym odpowiednikiem.

**53. Jaka jest różnica między S3 CRR a SRR?**
> CRR = replikacja między regionami (DR, compliance, latency reduction). SRR = replikacja w tym samym regionie (test/prod, log aggregation, backup).

**54. Co to jest S3 Outposts?**
> S3 API na AWS Outposts (on-premises hardware). Dane pozostają w data center klienta. S3 APIs + S3 on Outposts storage class.

**55. Jak zabezpieczyć dostęp do S3 z EC2 bez credentials?**
> IAM Role przypisana do EC2 (Instance Profile). Bucket policy może też wymagać dostępu przez VPC Endpoint (Gateway Endpoint — bezpłatny).

**56. Co to jest S3 Versioning "suspended" state?**
> Nowe obiekty dostają `null` jako Version ID. Stare wersje z poprzednich Version ID są zachowane. Wersjonowanie można wznowić.

**57. Jaki jest koszt VPC Gateway Endpoint dla S3?**
> **Bezpłatny** — S3 Gateway Endpoint (i DynamoDB) są darmowe. Interface Endpoints (PrivateLink) mają koszt.

**58. Co to jest S3 Lifecycle minimum dla One Zone-IA?**
> 30 dni w S3 Standard przed przejściem do One Zone-IA (jeśli transition z Standard). Min przechowywanie w One Zone-IA: 30 dni.

**59. Jak audytować dostęp do S3?**
> Dwie opcje: **S3 Server Access Logging** (tani, do innego bucketa) lub **CloudTrail Data Events** (droższy, ale w CloudTrail, integracja z CloudWatch/Athena).

**60. Co to jest S3 Intelligent-Tiering Archive tiers?**
> Opcjonalne dodatkowe tiery w Intelligent-Tiering: Archive Instant Access (90 dni), Archive Access (90 dni, async), Deep Archive Access (180 dni, async). Włączane osobno, tańsze niż standardowe tiers.

# 📝 Test Your Knowledge — AWS SAP-C02

> Format: **[ DOMENA ]** Pytanie **;** Krótka odpowiedź <br><br> * Rozszerzenie odpowiedzi. <br>

