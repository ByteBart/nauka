# SAP-C02 → SAP-C03 — delta guide and study plan
## For someone who already prepared for SAP-C02

> **Purpose:** everything that changed between the retiring **SAP-C02** (last day **16 November 2026**) and the new **SAP-C03** (live **17 November 2026**), mapped to a concrete chapter list and study plan for a candidate who has *already* done the SAP-C02 syllabus.
> **Basis:** the official SAP-C03 exam guide and its “Comparison of SAP-C02 and SAP-C03” page (published with registration on 27 October 2026). Verified November 2026 — re-check the AWS certification page before booking.
> **Headline:** roughly **two thirds of your SAP-C02 knowledge carries over**; what changes is (1) the blueprint is reorganised into **5 domains** instead of 4, (2) **security gets its own 25 % domain**, (3) **generative AI / agents and “built for the cloud” patterns** are now graded content, and (4) **resilience engineering, DevSecOps/observability, lakehouse data and post-quantum cryptography** are new mandatory skills.

---

## 0. TL;DR — the ten things that matter

1. **Exam shape:** 75 questions total (65 scored + 10 unscored), 180 minutes, **passing score 720** (down from 750), price unchanged.
2. **Five domains, new weights:** D1 Architecture Design & Implementation **25 %**, D2 Security, Compliance & Governance **25 %**, D3 Cost-Optimized Architecture **15 %**, D4 Resilience, Migration & Business Continuity **15 %**, D5 Operational Excellence & Automation **20 %**.
3. **Security is now its own domain** — it was spread across the four C02 domains; with 25 % it is the single most examinable area next to D1.
4. **GenAI + agentic architecture is new graded content** — Amazon Bedrock, Bedrock Guardrails, AgentCore/AgentCore Identity, Knowledge Bases, RAG, SageMaker AI inference, Bedrock Evaluations.
5. **“Built for the cloud” patterns expanded:** Step Functions durable execution, VPC Lattice/service mesh, ECS Service Connect, multi-tenant isolation, WebSocket/API Gateway real-time, ECR image scanning & lifecycle, ingress patterns.
6. **Resilience is now explicit:** AWS Fault Injection Service (chaos engineering), AWS Resilience Hub, Application Recovery Controller, multi-cluster EKS/ECS DR, ransomware protection (S3 Object Lock, EBS snapshot lock, Backup Vault Lock).
7. **Ops/automation is 20 %:** IaC (CloudFormation + **AWS CDK**), policy as code (CFN Hooks), CI/CD & DevSecOps (CodePipeline/CodeBuild/CodeDeploy/**CodeCommit**, cross-account pipelines, Inspector scanning), observability (X-Ray, **ADOT**, Container Insights, Synthetics, RUM, Application Signals, SLOs).
8. **Data:** lakehouse (Lake Formation + **Apache Iceberg**), **AWS Clean Rooms**, Security Lake, MSK, AppFlow, cross-account data sharing, data cataloguing.
9. **Cryptography:** **post-quantum** (KMS with **ML-DSA** signatures and **ML-KEM** key agreement), CloudHSM, ACM/Private CA, TLS 1.3.
10. **New services in scope** (20): Bedrock, AgentCore, VPC Lattice, Cloud WAN, Resilience Hub, Fault Injection Service, Verified Access, Verified Permissions, Security Lake, Lake Formation (parent), Clean Rooms, CDK, CodeCommit, DLM, MemoryDB, Application Auto Scaling, Local Zones, AWS Transform, Data Transfer Terminal, ADOT, Cost Categories, **Reserved Instances**. **Removed:** IoT family, End User Computing (WorkSpaces/AppStream), Media (Transcoder/Kinesis Video), SES, Batch, Amplify, App Runner, Proton, Service Catalog, Lightsail, CodeGuru, Managed Blockchain, Pinpoint, CodeArtifact, Data Exchange, Device Farm, Health Dashboard, License Manager, Wavelength.

---

## 1. Exam facts at a glance

| | **SAP-C02** (retiring) | **SAP-C03** (new) |
|---|---|---|
| Scored questions | 65 | **65** |
| Unscored (pretest) | 10 | 10 |
| Total / time | 75 / 180 min | 75 / 180 min |
| Passing score | 750 | **720** |
| Domains | 4 | **5** |
| Domain weights | 26 / 29 / 25 / 20 | **25 / 25 / 15 / 15 / 20** |
| Price | $300 | $300 |
| Last day / first day | **16 Nov 2026** / — | — / **17 Nov 2026** |
| Credential validity | 3 years | 3 years |
| Retake | 14 days | 14 days |
| Format | Pearson VUE test centre / online proctored | same |

**Timing implication:** ~2.4 minutes per question remains — the bottleneck is long scenario reading, not knowledge recall.

---

## 2. What the reorganisation means for your prep

You are **not starting over**. The functional content (~two thirds) is the same, but:

| Your SAP-C02 strength | Fate on SAP-C03 |
|---|---|
| Multi-account, Organizations, SCPs, Control Tower | Moved into **D2 Security/Governance** (still crucial, now with RCPs + Control Tower Account Factory) |
| Hybrid networking, TGW, Direct Connect, VPC design | Moved into **D1** (25 %) — plus **IPv6/dual-stack**, **Cloud WAN**, **VPC Lattice** |
| Migration/modernisation (MGN, DMS, refactor patterns) | Moved into **D4** (15 %) — plus AWS Transform and multi-cluster DR |
| Cost optimization (SP/RI/Spot, S3 tiers, CUR) | Moved to **D3** (15 %) — plus AI/ML cost, EFS lifecycle, EBS gp3, Cost Categories, Bedrock token accounting |
| Security (IAM, KMS, encryption) | **Own domain D2 (25 %)** — plus Cognito/OAuth, Verified Access, Security Lake, PQC, responsible AI |
| Resilience/DR | **D4 (15 %)** — plus FIS, Resilience Hub, ARC, container DR |
| Operational excellence (EventBridge, SSM, CloudWatch) | **D5 (20 %)** — plus CDK, DevSecOps, observability tooling, SLOs, AI monitoring |

**Effort split for someone who finished C02:** expect **~40–60 hours** of new learning + hands-on, then **~20–30 hours** of practice exams. If you already work daily with Bedrock/containers, fewer.

---

## 3. Domain map: SAP-C02 → SAP-C03

| SAP-C03 domain | Weight | Built from (SAP-C02) | Net new content |
|---|---|---|---|
| **D1 Architecture Design & Implementation Built for the Cloud** | 25 % | C02 D2 “Design for New Solutions” (29 %) + parts of C02 D3 edge patterns + D1 VPC/network security | GenAI/agents/RAG, durable workflows, service mesh/Lattice, multi-tenant, WebSocket, IPv6, lakehouse, hybrid compute (Outposts/Local Zones/EKS Anywhere) |
| **D2 Security, Compliance & Governance** | 25 % | C02 D1 identity/governance + security spread across all domains | Cognito/OAuth app auth, EKS Pod Identity, IAM Roles Anywhere, Verified Access, PQC, Security Lake, responsible-AI controls, compliance evidence (AWS Artifact), threat modelling |
| **D3 Cost-Optimized Architecture Design** | 15 % | C02 D3 cost skills + C02 D2.6/D2.5 | AI/ML cost optimization, cross-Region inference routing, Cost Categories/split cost allocation, EFS lifecycle, EBS gp3, Database Savings Plans, DLM, cost-effective observability |
| **D4 Resilience, Migration & Business Continuity** | 15 % | C02 D4 migration + DR skills | FIS/chaos engineering, Resilience Hub, ARC, multi-cluster container DR, ransomware protection (Object Lock/EBS snapshot lock/Backup Vault Lock), AWS Transform |
| **D5 Operational Excellence & Automation** | 20 % | C02 D3 operational + IaC/CI-CD parts | CDK, policy as code (CFN Hooks), DevSecOps scanning/signing, cross-account pipelines, ADOT/X-Ray, Container Insights, Synthetics/RUM, Application Signals, SLOs, AI/ML monitoring, controlled remediation patterns |

---

## 4. The full addition list (official delta, grouped for study)

### 4.1 Domain 1 — architecture built for the cloud (25 %)

**Cloud-native & serverless**
- Event-driven messaging: idempotency, ordering, delivery guarantees (EventBridge, SQS FIFO/standard, SNS, Kinesis partition keys)
- Stateless apps with externalized state (DynamoDB, S3, ElastiCache)
- Serverless data processing/analysis pipelines (Kinesis Data Streams, Glue, Athena, Lambda)
- **Durable multi-step workflows** (Step Functions, durable Lambda functions)
- Cross-account / cross-Region event routing (event buses, forwarding)
- Container orchestration (EKS, ECS, Fargate), **auto scaling** (Lambda concurrency, HPA, Cluster Autoscaler, ECS service scaling, Application Auto Scaling)
- **Service mesh** (VPC Lattice, ECS Service Connect), **ECR** scans + lifecycle policies
- **Ingress patterns** (ALB, NLB, AWS Load Balancer Controller, API Gateway), edge (CloudFront Functions, Lambda@Edge)
- **WebSocket / publish-subscribe real-time** (API Gateway WebSocket)
- **Multi-tenant isolation** (siloed vs pooled, Cognito tenant context, row-level security, tenant-specific encryption)

**AI/ML integration (new task)**
- **GenAI integrations** (Bedrock, incl. multi-account patterns)
- **AI agent architectures** (Bedrock AgentCore, Knowledge Bases)
- ML inference (SageMaker AI endpoints)
- **RAG** — ingestion + retrieval, vector stores

**Hybrid & network**
- Hybrid connectivity redundancy/failover (DX, VPN, TGW); data sync (DataSync, Storage Gateway, Transfer Family); legacy modernization (API GW, Lambda, MQ)
- **Hybrid DNS** (Resolver endpoints, PHZ, split-horizon, conditional forwarding)
- **Hybrid compute** (EKS Anywhere, ECS Anywhere, Outposts, Local Zones)
- Multi-VPC (peering, PrivateLink), VPC design with **IPAM**, network security (Network Firewall, WAF, GWLB, SG/NACL, Flow Logs)
- **IPv6 adoption & dual-stack migration**
- Global routing (Route 53 policies, Global Accelerator)

**Data/storage/db**
- **Lakehouse**: S3, Lake Formation, Redshift Spectrum, **Apache Iceberg**
- Real-time + batch analytics (EMR, Redshift, MSK, AppFlow)
- DB selection, scaling/HA (read replicas, Aurora Global DB, DynamoDB global tables, RDS Multi-AZ)
- Storage by access pattern (S3/EBS/EFS/FSx), caching (ElastiCache, DAX)
- **Data cataloguing & cross-account sharing** (Glue Data Catalog, Lake Formation, Clean Rooms)

### 4.2 Domain 2 — security, compliance, governance (25 %)

- Identity federation (Identity Center, SAML 2.0, OIDC, SCIM)
- **Application authN/authZ** (Cognito user pools/identity pools, OAuth 2.0, **EKS Pod Identity**)
- Least privilege (roles, policies, boundaries, external ID, session policies, ABAC)
- **Data perimeter & external access validation** (Access Analyzer, VPC endpoint policies, **AWS Verified Access**)
- **Workload identities for hybrid/multi-cloud** (IAM Roles Anywhere)
- Encryption at rest (customer managed keys, rotation), in transit (TLS 1.3, ACM, Private CA), **CloudHSM**, **post-quantum KMS (ML-DSA, ML-KEM)**
- Governance: Organizations/OUs, Control Tower + Account Factory, **centralised logging incl. AI model invocations**, Security Lake, Security Hub, Config aggregation, **RAM sharing**, **SCPs + RCPs**, Config rules/Security Hub CSPM, Inspector/Patch Manager
- Compliance: data sovereignty, **data residency**, audit trails incl. AI, **AWS Artifact** evidence, threat modelling
- **Responsible AI**: Bedrock Guardrails, AgentCore Identity, human-approval workflows (Step Functions)

### 4.3 Domain 3 — cost (15 %)

- Purchase options (provisioned capacity, RIs, Savings Plans, Spot, On-Demand), Graviton, Step Functions Standard/Express, Lambda, Compute Optimizer, scheduled/target-tracking scaling, Fargate Spot
- **AI/ML cost**: service tier selection, batching, caching (Bedrock/SageMaker AI); **cross-Region inference routing**
- Network/data transfer: CloudFront + Origin Shield, private connectivity, AZ-aware design, NAT costs, PrivateLink/TGW data processing, replication choices
- **Allocation/visibility**: tagging, CUR/CE, **tag policies + SCP enforcement**, **Cost Categories, split cost allocation, Bedrock token usage**, Budgets + Cost Anomaly Detection
- Storage/DB: S3 Intelligent-Tiering/lifecycle/Glacier, **EFS lifecycle**, **EBS gp3/gp2/io2**, DynamoDB capacity modes, RDS reserved, Aurora Serverless, Aurora I/O-Optimized, **Database Savings Plans**, **AWS Backup + DLM**, **cost-effective observability (log filtering, metric aggregation, retention)**

### 4.4 Domain 4 — resilience, migration, continuity (15 %)

- Discovery/portfolio, migration waves; rehost (MGN/**AWS Transform**), DMS/DataSync, replatform, refactor (strangler fig), mainframe/legacy modernisation
- DR by RTO/RPO/cost; **AWS Elastic Disaster Recovery**; Resilience Hub + SSM runbooks; multi-Region DR (Route 53 health checks, Global Accelerator, **Application Recovery Controller**); DR testing; **multi-cluster EKS/ECS**
- Backup governance (AWS Backup, cross-account), **FIS failover tests**, redundancy/fault tolerance/self-healing, **ransomware protection** (S3 Object Lock, EBS snapshot lock, Backup Vault Lock), **chaos engineering** (FIS experiment templates, steady-state hypotheses)

### 4.5 Domain 5 — operational excellence & automation (20 %)

- IaC: CloudFormation **and AWS CDK**, **policy as code** (CFN Hooks, CDK aspects, Config rules, SCPs), secrets (Secrets Manager, Parameter Store), StackSets multi-account/Region
- CI/CD: CodePipeline/CodeBuild/CodeDeploy/**CodeCommit**, **security scanning & artifact signing** (Inspector, ECR scanning), **cross-account/Region pipelines**, **Bedrock Evaluations**, blue/green/canary/rolling
- Observability: CloudWatch, **X-Ray**, **ADOT**; **Container Insights**; multi-account log aggregation; alarms/SNS/SSM; **AI/ML metrics (latency, drift, agent performance)**; **Synthetics + RUM**; **SLOs (Application Signals)**
- Automation: health monitoring/capacity/Service Quotas; **controlled remediation (circuit breakers, rate limits, bulkhead, retry with backoff)**; auto scaling (target tracking, predictive); automated failure recovery (health checks, ECS task replacement, Lambda retries); **automated runbooks/incident response**

---

## 5. Deletions and recategorisations — nothing to *unlearn*

Only a few items were deleted from the blueprint text:
- “Application upgrade path” and “Delegating to AWS” (from the old C02 D2 identity task)
- “Migration-tool security” (from the old C02 D4 migration task)

AWS explicitly says these may still be implicit elsewhere; do not study them less, just don't expect dedicated questions. Everything else was **consolidated** — e.g. C02’s separate tasks for VPC design, network security, event routing, edge, autoscaling, containers were merged into fewer, broader C03 tasks. Net effect: **same depth, fewer duplicated task statements** — no content holes for you.

---

## 6. In-scope / out-of-scope service changes

### 6.1 Newly in scope (20)

| Service | Why it matters on SAP-C03 |
|---|---|
| **Amazon Bedrock** | D1 GenAI/RAG, D2 responsible AI, D3 AI cost, D5 evaluations |
| **Bedrock AgentCore / AgentCore Identity** | agent architectures, agent access control |
| **Amazon VPC Lattice** | service mesh / multi-VPC service networking (D1) |
| **AWS Cloud WAN** | global network management (D1 network) |
| **AWS Resilience Hub** | DR assessment/testing (D4) |
| **AWS Fault Injection Service** | chaos engineering / failover tests (D4) |
| **Amazon Application Auto Scaling** | scaling policies (D1/D5) |
| **AWS CDK** | IaC (D5) |
| **AWS CodeCommit** | CI/CD (D5) |
| **AWS Distro for OpenTelemetry (ADOT)** | observability (D5) |
| **Amazon Security Lake** | centralised security data (D2) |
| **AWS Verified Access** | zero-trust app access (D2) |
| **Amazon Verified Permissions** | fine-grained app authorisation (D2) |
| **AWS Clean Rooms** | cross-account data collaboration (D1/D3) |
| **AWS Lake Formation** | lakehouse permissions/catalog (D1/D2/D3) |
| **Amazon Data Lifecycle Manager** | EBS snapshot lifecycle (D3) |
| **Amazon MemoryDB** | caching/DB selection (D1/D3) |
| **AWS Local Zones** | hybrid/edge compute (D1) |
| **AWS Transform** (incl. Transform MGN) | migration/modernisation (D4) |
| **AWS Data Transfer Terminal** | data transfer for migrations (D1/D4) |
| **AWS Cost Categories** + **Reserved Instances** | cost allocation (D3) |

### 6.2 Removed from scope — do not spend time

- **Entire categories:** all AWS IoT services, End User Computing (WorkSpaces, AppStream 2.0), Media Services (Elastic Transcoder, Kinesis Video Streams).
- **Individual services:** SES, Pinpoint, Amplify, App Runner, Batch, CodeArtifact, Data Exchange, Device Farm, Health Dashboard, License Manager, Proton, Service Catalog, Lightsail, CodeGuru, Managed Blockchain, Wavelength.
- **Individual AI/ML services replaced by parent entries** (Bedrock, SageMaker AI): Comprehend, Fraud Detector, Kendra, Lex, Personalize, Polly, Rekognition, Textract, Transcribe, Translate — no need to memorise per-service APIs, but know the category.
- **Newly out of scope:** Braket, Comprehend Medical, Transcribe Medical, IVS, RDS Custom for Oracle, DeepComposer, DeepRacer, Elemental MediaConvert/MediaLive, Ground Station.

---

## 7. Chapter list to cover

### Tier A — new / high-risk chapters (must study, in this order)

| # | Chapter | Covers (D1–D5 skills) | Est. time |
|---|---|---|---|
| A1 | **Amazon Bedrock & RAG foundations** | models, KBs, vector stores, ingestion/retrieval, multi-account patterns, quotas/costs, Evaluations | 6 h |
| A2 | **AI agents & responsible AI** | AgentCore, AgentCore Identity, Guardrails, human-approval workflows, AI access control | 4 h |
| A3 | **SageMaker AI inference & AI cost** | endpoints, batching, caching, cross-Region inference routing, token accounting | 3 h |
| A4 | **Cloud-native workflow & event patterns** | Step Functions Standard vs Express + durable execution, SQS FIFO, EventBridge routing, WebSocket APIs | 4 h |
| A5 | **Containers & service mesh** | EKS/ECS/Fargate, ECR scanning/lifecycle, Lattice, ECS Service Connect, Pod Identity, Container Insights | 5 h |
| A6 | **Multi-tenant architecture** | silo/pool models, Cognito tenant context, row-level security, tenant encryption, per-tenant scaling/cost | 3 h |
| A7 | **Security architecture for apps** | Cognito user/identity pools, OAuth 2.0, Verified Access, Verified Permissions, Roles Anywhere, EKS Pod Identity | 5 h |
| A8 | **Post-quantum cryptography** | ML-DSA (signatures) vs ML-KEM (key agreement) in KMS, ACM, migration strategy, CloudHSM distinction | 3 h |
| A9 | **Governance v2** | Control Tower + Account Factory, RCPs alongside SCPs, Security Lake, central AI-invocation logging, Artifact, threat modelling | 5 h |
| A10 | **Resilience engineering** | FIS experiments, Resilience Hub, ARC, multi-cluster DR, ransomware protection (Object Lock/snapshot lock/Vault Lock) | 5 h |
| A11 | **Observability & SLOs** | ADOT/X-Ray, Application Signals, Synthetics, RUM, AI/ML metrics + drift, log aggregation, cost of observability | 4 h |
| A12 | **IaC & DevSecOps** | CDK vs CloudFormation, CFN Hooks/policy-as-code, StackSets, CodePipeline cross-account, Inspector/ECR scanning, artifact signing, Bedrock Evaluations in pipeline | 5 h |
| A13 | **Lakehouse & data collaboration** | Lake Formation permissions, Iceberg, Glue Data Catalog, Clean Rooms, MSK/AppFlow/EMR/Redshift Spectrum | 5 h |
| A14 | **IPv6/dual-stack, Cloud WAN & global networking** | IPv6 adoption, Cloud WAN, IPAM, Global Accelerator vs Route 53 routing, Lattice vs PrivateLink vs TGW | 4 h |
| A15 | **Migration modernisation v2** | AWS Transform/Transform MGN, DMS/DataSync, strangler fig, mainframe/DB modernisation, replatform targets | 3 h |

**Tier A total ≈ 64 h** (fewer if you already use Bedrock/containers daily).

### Tier B — reorganised/expanded chapters (re-study with new lens)

| # | Chapter | What changes vs your C02 notes |
|---|---|---|
| B1 | Security & compliance domain | Same IAM/KMS core, but arranged as a 25 % domain: add Cognito/Verified Access/PQC/Security Lake/Artifact/threat modelling |
| B2 | Cost domain | Add AI cost, cross-Region inference, Cost Categories, split cost allocation, EFS/EBS specifics, Database Savings Plans, AI-observability cost |
| B3 | Resilience/DR | Add FIS/Resilience Hub/ARC/container DR/ransomware locks |
| B4 | Ops & automation | Add CDK, policy-as-code, DevSecOps, SLOs, AI monitoring, controlled remediation patterns |
| B5 | Networking | Add IPv6/dual-stack, Cloud WAN, Lattice, IPAM; keep TGW/DX/VPN/PrivateLink |
| B6 | Data & analytics | Add Lakehouse/Iceberg/Clean Rooms/Security Lake/AppFlow/MSK |
| B7 | GenAI responsible-AI controls | New: Guardrails, agent access, human oversight |

### Tier C — revision only (your SAP-C02 work still applies)

These you can treat as **spaced repetition**, not fresh study:
- Multi-account structures, Organizations/OU/SCP, consolidated billing, Control Tower basics
- IAM core (roles, policies, evaluation, Identity Center, ABAC, boundaries, Access Analyzer)
- VPC core (subnets, routing, NAT, SG/NACL, endpoints, peering/TGW, DNS/PHZ)
- S3 tiering/lifecycle/encryption/access control; EBS/EFS/FSx selection; RDS/Aurora/DynamoDB basics
- EC2 purchasing (SP/RI/Spot/CR), Compute Optimizer, CUR/Cost Explorer/Budgets/Anomaly Detection
- DR patterns (backup/restore, pilot light, warm standby, active-active), RTO/RPO maths
- EventBridge/SQS/SNS patterns, Step Functions basics, SSM Automation, CloudWatch/Config/GuardDuty/Security Hub
- Hybrid connectivity (DX/VPN/TGW), migration tools (MGN/DMS/DataSync)

---

## 8. Your existing study books — update plan

| Book in this folder | Status for SAP-C03 | What to add |
|---|---|---|
| [AWS_S3_Storage_Classes_Tiering_Lifecycle_SAP-C02.md](AWS_S3_Storage_Classes_Tiering_Lifecycle_SAP-C02.md) | ✅ ~95 % valid | EFS lifecycle policies, EBS gp2→gp3, S3 Object Lock for ransomware, cost-effective log retention |
| [AWS_VPC_Networking_SAP-C02.md](AWS_VPC_Networking_SAP-C02.md) | ✅ ~90 % valid | IPv6/dual-stack, IPAM, Cloud WAN, VPC Lattice vs PrivateLink, Route 53 polices/Global Accelerator, hybrid DNS (already partly there) |
| [AWS_IAM_Users_Groups_Roles_Policies_SAP-C02.md](AWS_IAM_Users_Groups_Roles_Policies_SAP-C02.md) | ✅ ~85 % valid | Verified Access, Verified Permissions, IAM Roles Anywhere, EKS Pod Identity, Cognito/OAuth, PQC in KMS |
| [AWS_EC2_Purchasing_Options_SAP-C02.md](AWS_EC2_Purchasing_Options_SAP-C02.md) | ✅ valid | Graviton, Fargate Spot, Lambda concurrency, Database Savings Plans (cross-link) |
| [AWS_CUR_CostExplorer_Budgets_SAP-C02.md](AWS_CUR_CostExplorer_Budgets_SAP-C02.md) | ✅ valid | Cost Categories, split cost allocation, Bedrock token monitoring, Budgets actions |
| [AWS_Organizations_OU_SAP-C02.md](AWS_Organizations_OU_SAP-C02.md) | ✅ valid | RCPs, Control Tower Account Factory, Security Lake/central logging for AI |
| [AWS_EventBridge_vs_EventBus_SAP-C02.md](AWS_EventBridge_vs_EventBus_SAP-C02.md) | ✅ valid | Cross-account/cross-Region event routing patterns, SQS FIFO ordering/idempotency |
| [AWS_CloudWatch_GuardDuty_Config_Hooks_SAP-C02.md](AWS_CloudWatch_GuardDuty_Config_Hooks_SAP-C02.md) | ✅ valid | ADOT/X-Ray, Container Insights, Synthetics, RUM, Application Signals/SLOs, AI metrics |
| [AWS_SSM_Automation_SAP-C02.md](AWS_SSM_Automation_SAP-C02.md) | ✅ valid | Incident-response runbooks, Patch Manager, Resilience Hub integration, controlled remediation |

**New books to create while studying Tier A:** Bedrock/RAG, Agents/Responsible AI, Containers & Lattice, Security-for-apps (Cognito/Verified/PQC), Resilience engineering (FIS/Resilience Hub/ARC), Observability & SLOs, IaC & DevSecOps, Lakehouse & Clean Rooms, IPv6/Cloud WAN, Migration v2.

---

## 9. Hands-on labs checklist (what makes the new content stick)

- [ ] Bedrock: invoke a model, create a **Knowledge Base**, build a simple **RAG** app, apply **Guardrails**, check token cost in Cost Explorer
- [ ] Bedrock **AgentCore**: create an agent with a tool, test **AgentCore Identity** access control
- [ ] **SageMaker AI endpoint** + batching/caching discussion; compare inference cost
- [ ] **Step Functions** Standard vs Express + a durable workflow with a human-approval task
- [ ] **API Gateway WebSocket** + SQS FIFO ordering/idempotency demo
- [ ] **VPC Lattice** service network between two VPCs; **ECS Service Connect** on a service
- [ ] **EKS Pod Identity** + ECR image scanning + lifecycle policy
- [ ] **Multi-tenant** demo: pooled DynamoDB with tenant keys + row-level authorisation via **Verified Permissions**
- [ ] **Cognito** user pool + OAuth 2.0 flow; **Verified Access** for internal app
- [ ] **KMS PQC**: create **ML-DSA** signing key and sign/verify; **ML-KEM** key agreement flow
- [ ] **Control Tower** + Account Factory (lab account), **RCP** example
- [ ] **Security Lake** + **Security Hub** + CloudTrail org trail; AI-invocation logging
- [ ] **FIS** experiment (e.g. EC2 stop / AZ failure) + **Resilience Hub** assessment + **ARC** routing control
- [ ] Ransomware drill: **S3 Object Lock** + **EBS snapshot lock** + **Backup Vault Lock**
- [ ] **ADOT + X-Ray** tracing, **Container Insights**, **CloudWatch Synthetics** canary, **RUM**, **Application Signals SLO**
- [ ] **CDK** stack + **CFN Hooks**/policy-as-code + **StackSets** multi-account
- [ ] **CodePipeline** cross-account deploy with **Inspector/ECR scanning** + **Bedrock Evaluations** gate
- [ ] **Lake Formation** + **Iceberg** table + **Clean Rooms** collaboration
- [ ] **Cloud WAN** + IPv6 VPC/subnet + Global Accelerator endpoint group
- [ ] **AWS Transform/MGN** test migration + **DMS** replication

---

## 10. Study plan

### 10.1 Two-week “delta sprint” (you already passed/finished C02 prep — full-time)

| Day | Focus |
|---|---|
| 1–2 | Tier B1 (security domain) + A7 + A8 |
| 3–4 | A1 + A2 + A3 (Bedrock/RAG/agents/responsible AI/AI cost) |
| 5–6 | A4 + A5 + A6 (workflows, containers/mesh, multi-tenant) |
| 7 | A14 (IPv6/Cloud WAN/Lattice/global) + Tier B5 revision |
| 8–9 | A10 + A11 + Tier B3 + B4 (resilience, observability, SLOs) |
| 10–11 | A12 + A13 (IaC/DevSecOps, lakehouse/Clean Rooms) |
| 12 | A9 + A15 (governance v2, migration v2) + labs catch-up |
| 13 | Full practice exam #1 + review |
| 14 | Full practice exam #2 + weak-domain drills |

### 10.2 Four-to-six week plan (part-time, ~2 h/day + weekend labs)

| Week | Content |
|---|---|
| 1 | Security domain (B1 + A7 + A8) + labs: Cognito, Verified Access, PQC |
| 2 | GenAI/agents (A1–A3) + labs: KB/RAG, Guardrails, AgentCore |
| 3 | Cloud-native + containers (A4–A6) + labs: Step Functions, Lattice, Pod Identity |
| 4 | Resilience + observability (A10, A11) + labs: FIS, Resilience Hub, ADOT, Synthetics |
| 5 | IaC/DevSecOps + data lakehouse + networking (A12–A14) + labs |
| 6 | Governance + migration (A9, A15), full practice exams ×2, weak areas |

### 10.3 Daily structure that works for SAP-level scenarios

1. 30 min — read one chapter section (official guide skills + docs).
2. 30 min — hands-on lab for the same skill.
3. 30 min — 10–15 practice questions on the same domain, timed at 2.4 min/question.
4. 30 min — write the “wrong-answer trap” notes (why each distractor fails).

---

## 11. Exam-day strategy

- **65 scored questions / 180 min** → average **2.4 min**; long scenarios will eat 4–5 min, so budget: first pass ~2 min, flag long ones, second pass for flagged.
- **720 passing score** (lower than C02’s 750) but the new pool may feel less predictable in the first months — do not rely on memorised C02 question dumps.
- **New in-scope services are also new to question writers** — expect straightforward scenario questions (e.g. “which service for cross-account data collaboration?” → Clean Rooms; “which for chaos testing?” → FIS; “which for PQC signatures?” → KMS ML-DSA).
- **Scoring is compensatory** — you can fail a domain and pass overall; prioritise D1 + D2 (50 % combined).
- **Retake**: 14 calendar days, full fee — plan the first attempt for a date that leaves room for one retake.

---

## 12. Risk register — what *not* to over-invest in

| Risk | Mitigation |
|---|---|
| Old C02 dumps/practice tests | Use only SAP-C03 material published after 27 Oct 2026; old tests still valuable for D1/D2 core scenarios |
| Removed services (IoT, Media, EUC, SES, Batch, Proton, Service Catalog, Lightsail…) | Skip entirely; AWS explicitly removed them |
| Deep AI/ML model training or data science | Out of scope by the guide — study *architecture* (inference, RAG, agents, controls, cost), not training algorithms |
| Memorising per-service AI/ML APIs (Comprehend, Textract, Translate…) | Replaced by Bedrock/SageMaker parent entries — know categories only |
| Ignoring the new D5 (20 %) because “ops was never my strength” | 20 % is more than the old cost domain — CDK/DevSecOps/SLO/observability are learnable in a week |
| Skipping hands-on | New domains are lab-heavy; scenarios about Guardrails, FIS, Lattice, Clean Rooms are hard to answer from theory alone |

---

## 13. Gap-check checklist (tick when you can explain each in one sentence)

**D1 (25 %)**
- [ ] Event idempotency, ordering, delivery guarantees across EventBridge/SQS/SNS/Kinesis
- [ ] Step Functions Standard vs Express vs durable Lambda execution
- [ ] VPC Lattice vs PrivateLink vs TGW vs Cloud WAN
- [ ] Multi-tenant silo vs pool isolation patterns
- [ ] Bedrock RAG ingestion/retrieval + Knowledge Bases
- [ ] AgentCore agent + AgentCore Identity access control
- [ ] IPv6/dual-stack migration steps
- [ ] Lake Formation + Iceberg lakehouse

**D2 (25 %)**
- [ ] Identity Center vs Cognito vs Verified Access/Permissions
- [ ] IAM Roles Anywhere for hybrid workloads
- [ ] ML-DSA vs ML-KEM and where KMS fits
- [ ] Security Lake + central AI-invocation logging
- [ ] RCP vs SCP
- [ ] Bedrock Guardrails + human approval workflow
- [ ] Data residency controls
- [ ] AWS Artifact evidence collection

**D3 (15 %)**
- [ ] Bedrock/SageMaker cost levers (tier, batching, caching, cross-Region inference)
- [ ] Cost Categories + split cost allocation
- [ ] EFS lifecycle, EBS gp3, Database Savings Plans, DLM
- [ ] Cost of observability (log filtering, metric aggregation, retention)

**D4 (15 %)**
- [ ] FIS experiment design + steady-state hypothesis
- [ ] Resilience Hub assessment + ARC routing control
- [ ] Multi-cluster EKS/ECS DR
- [ ] Object Lock / snapshot lock / Vault Lock ransomware pattern
- [ ] AWS Transform / MGN / DMS roles

**D5 (20 %)**
- [ ] CDK vs CloudFormation vs CFN Hooks
- [ ] Cross-account CodePipeline with Inspector/ECR scanning
- [ ] X-Ray/ADOT tracing, Container Insights, Synthetics, RUM
- [ ] SLOs with Application Signals
- [ ] AI/ML observability (latency, drift, agent performance)
- [ ] Controlled remediation (circuit breaker, backoff, bulkhead)

---

## 14. One-page recall map

```
SAP-C03 = 75 Q / 180 min / pass 720 / 5 domains
D1 25%  Cloud-native + AI/ML + hybrid + network + data
D2 25%  Identity + crypto (PQC) + governance (RCP, CT) + compliance + responsible AI
D3 15%  Pricing + data transfer + allocation (Cost Categories) + storage/DB + AI cost
D4 15%  Migration (Transform) + DR (FIS, Resilience Hub, ARC) + BC + ransomware
D5 20%  IaC (CDK, Hooks) + CI/CD DevSecOps + observability (ADOT, SLO, AI) + automation
NEW:  Bedrock, AgentCore, Lattice, Cloud WAN, FIS, Resilience Hub, Security Lake,
      Verified Access/Permissions, Clean Rooms, CDK, CodeCommit, DLM, MemoryDB,
      Local Zones, AWS Transform, Data Transfer Terminal, ADOT, Cost Categories
GONE: IoT, WorkSpaces/AppStream, Media, SES, Batch, Amplify, App Runner, Proton,
      Service Catalog, Lightsail, CodeGuru, Managed Blockchain, Pinpoint, Wavelength,
      CodeArtifact, Data Exchange, Device Farm, Health Dashboard, License Manager
```

---

## 15. Sources

- [SAP-C03 exam guide (official)](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03.md)
- [Comparison of SAP-C02 and SAP-C03 (official delta)](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/sap-03-comparison.md)
- [SAP-C03 Domain 1](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain1.md) · [Domain 2](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain2.md) · [Domain 3](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain3.md) · [Domain 4](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain4.md) · [Domain 5](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain5.md)
- [SAP-C03 in-scope and out-of-scope services](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/sap-03-in-scope-services.md)
- [AWS certification page](https://aws.amazon.com/certification/certified-solutions-architect-professional/)
- [SAP-C02 exam guide (for reference)](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-02/solutions-architect-professional-02.md)
