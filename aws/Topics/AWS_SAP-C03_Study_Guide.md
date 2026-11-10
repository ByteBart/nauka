# AWS Certified Solutions Architect – Professional (SAP-C03) — Master Study Guide
## Everything you need to pass, organised by the five official domains

> **Built on:** the official SAP-C03 exam guide (tasks + skills, published 27 Oct 2026) and its SAP-C02→SAP-C03 comparison page. Verified **November 2026**.
> **Use with:** [AWS_SAP-C03_Delta_Study_Plan.md](AWS_SAP-C03_Delta_Study_Plan.md) (what changed + schedule) and the existing books in this folder, which are cross-linked per topic.
> **Exam facts:** 75 questions (65 scored + 10 unscored), **180 minutes**, **pass 720**, five domains: **D1 25 % · D2 25 % · D3 15 % · D4 15 % · D5 20 %**. SAP-C02 retired 16 Nov 2026; SAP-C03 live 17 Nov 2026.
> **How to use this guide:** read one domain section, do the hands-on lab for the same domain, then 15 timed practice questions (§10). The “which service wins” tables (§8) are the highest-yield material in the last week.

---

## 0. High-level map of the exam

| Domain | Weight | What it really tests | Biggest traps |
|---|---|---|---|
| **D1 Architecture Design & Implementation Built for the Cloud** | **25 %** | serverless/event-driven design, containers & service mesh, AI/ML integration, hybrid + global networking, lakehouse data | choosing Bedrock vs SageMaker vs custom; Lattice vs PrivateLink vs TGW; standard vs FIFO vs Kinesis; multi-tenant isolation |
| **D2 Security, Compliance & Governance** | **25 %** | identity architecture, encryption & PQC, multi-account governance, compliance/data residency, responsible AI | Cognito user pools vs identity pools; Verified Access vs Verified Permissions vs Identity Center; KMS vs CloudHSM; ML-DSA vs ML-KEM; SCP vs RCP |
| **D3 Cost-Optimized Architecture Design** | **15 %** | purchase models, data-transfer costs, allocation/showback, storage/DB cost, AI cost | per-GB costs (NAT/TGW/PrivateLink/AZ crossing); Aurora I/O-Optimized; Cost Categories vs tags; Bedrock token cost |
| **D4 Resilience, Migration & Business Continuity** | **15 %** | migration frameworks, DR strategies, resilience engineering + chaos | RTO/RPO strategy matching; FIS vs Resilience Hub vs ARC; ransomware protection |
| **D5 Operational Excellence & Automation** | **20 %** | IaC/CDK, DevSecOps pipelines, observability/SLOs, automation | CFN vs CDK vs Hooks; cross-account pipelines; ADOT vs X-Ray; controlled remediation patterns |

---

## 1. Exam strategy

- **Budget 2.4 min/question.** First pass: answer everything you know; flag long scenarios; second pass for flagged ones.
- **Scoring is compensatory** — you can be weak in one domain. But with **D1 + D2 = 50 %**, spend most of your time there.
- **Every scenario is a constraint puzzle:** cost, latency, security, operational effort, **compliance**. The “most appropriate” answer satisfies all constraints, not just the cheapest.
- **New-service questions are usually one-liners:** “which for chaos testing?” → FIS; “which for cross-account data collaboration?” → Clean Rooms; “which for PQC signatures?” → KMS ML-DSA.
- **Know what is out of scope:** AI/ML model training, OS internals, security forensics, app coding — don't waste hours there.

---

# D1 — Architecture Design and Implementation Built for the Cloud (25 %)

## D1.1 Event-driven and serverless pipelines

| Building block | Key facts | Exam trigger |
|---|---|---|
| **Amazon EventBridge** | rules on an event bus; filters (content-based); multiple targets; schema registry; archive & replay; cross-account targets; event-bus-to-event-bus forwarding for cross-Region/account patterns; SaaS/partner sources | “route events from many sources”, “replay past events”, “decouple producers/consumers” |
| **Amazon SQS Standard** | at-least-once, best-effort ordering, nearly unlimited throughput; visibility timeout, long polling, DLQ, redrive | buffer/decouple, retry, backpressure |
| **Amazon SQS FIFO** | exactly-once *processing* (dedup ID), strict ordering per **message group**; lower throughput; FIFO + DLQ | “ordering matters”, “no duplicates” |
| **Amazon SNS** | pub/sub fan-out; standard + FIFO topics; message filtering (subscription filter policy); DLQ for subscriptions | fan-out to many subscribers, mobile push, email |
| **Amazon Kinesis Data Streams** | shards (throughput unit), partition key → ordering **within a shard**, retention 1–365 days, replay, on-demand vs provisioned mode | streaming ingestion, multiple consumers, replay, ordering per key |
| **Amazon Data Firehose** | managed delivery to S3/Redshift/OpenSearch/Splunk; optional Lambda transform, format conversion, compression; near-real-time (buffered) | “load streaming data into S3/Redshift with no code” |
| **AWS Lambda** | event sources (SQS, Kinesis, DDB streams, S3, API GW…); concurrency/reserved/provisioned; retries, DLQ, destinations; 15-min limit | glue logic, serverless compute |

**Design rules**
- **Idempotency:** if the same message may arrive twice (SQS standard, Lambda retries, Step Functions retries), use a dedup key in DynamoDB with conditional write; FIFO gives dedup only within the 5-minute dedup window.
- **Ordering:** SQS FIFO (per message group) or Kinesis (per partition key). Never assume global ordering.
- **Backpressure:** queues buffer bursts; DLQ + redrive policy for poison messages; alarm on `ApproximateAgeOfOldestMessage`.
- **Serverless data pipeline classic:** S3 (raw) → EventBridge/Lambda or Kinesis → Glue/Athena → curated S3 (Iceberg) → Redshift Spectrum/QuickSight.

## D1.2 Durable workflows and orchestration

| Option | When | Key facts |
|---|---|---|
| **Step Functions Standard** | long-running, auditable, exactly-once workflow | up to **1 year**; state machine + states; task tokens for human approval/callbacks; integrations (Lambda, ECS, EMR, Glue, Bedrock, SNS/SQS, EventBridge); priced per state transition; visible execution history |
| **Step Functions Express** | high-volume, short-lived (≤5 min) | at-least-once semantics; priced per request + GB-s; for streaming/event processing |
| **Lambda durable functions** | long-running code with checkpoints | checkpoint/replay model; use for multi-step functions that outlive a single invocation (still verify current limits) |
| **Saga / compensating transactions** | distributed transaction | explicit compensation states; avoid 2PC |

**Exam pattern:** “multi-step process with human approval / retries / months-long waits” → Step Functions Standard with callback/task-token (often plus SNS approval email or Bedrock agent step).

## D1.3 Containers and service mesh

| Decision | Options | Rule |
|---|---|---|
| Orchestration | **ECS** (AWS-native, simpler), **EKS** (Kubernetes ecosystem), **Fargate** (serverless compute for either) | “Kubernetes tooling/portability” → EKS; “least operational overhead for simple services” → ECS/Fargate |
| Compute | EC2 launch type vs Fargate vs **Fargate Spot** (interruption-tolerant) | batch/CI → Fargate Spot |
| Image lifecycle | **ECR**: scan on push, basic/enhanced scanning, lifecycle policies (expire untagged/old), cross-Region replication, pull-through cache | reduce registry cost and CVE exposure |
| Service-to-service | **VPC Lattice** (cross-VPC, cross-account, IAM auth, service networks), **ECS Service Connect** (per-service mesh inside/among ECS services via Cloud Map), **App Mesh** (legacy) | “many VPCs/accounts, service-to-service auth and observability” → Lattice |
| Scaling | ECS service auto scaling, **Application Auto Scaling**, HPA/Cluster Autoscaler/Karpenter, Fargate scaling | target tracking for steady utilisation; schedule for known peaks |
| Identity | **EKS Pod Identity** (no OIDC provider needed; trust `pods.eks.amazonaws.com`) vs **IRSA** (OIDC) | new designs → Pod Identity; legacy → IRSA |

**VPC Lattice vs PrivateLink vs TGW**

| | Lattice | PrivateLink (interface endpoints) | Transit Gateway |
|---|---|---|---|
| Purpose | service-to-service (L7) networking across VPCs/accounts | expose a *service* privately to consumers | route traffic between VPCs/VPN/DX (L3) |
| Client | IAM-authenticated service calls via a service network | any client reaching the endpoint ENI | any routed traffic |
| Routing | Lattice data plane, no VPC route-table work | endpoint ENI + SG | attachments + route tables |
| Use when | microservice mesh, multi-account service sharing | SaaS/partner service exposure, AWS service private access | hub-and-spoke VPC topology |

## D1.4 Ingress, edge and real-time

| Requirement | Service | Notes |
|---|---|---|
| L7 HTTP routing + WAF + Cognito auth | **ALB** | host/path rules, redirects, fixed response; WAF integration |
| L4 TCP/UDP, static IP, TLS passthrough | **NLB** | PrivateLink producer side, Global Accelerator endpoint, extreme performance |
| REST/HTTP/WebSocket APIs, auth, throttling, usage plans | **API Gateway** | REST vs HTTP vs WebSocket; IAM/Cognito/Lambda authorizers; private API via VPC endpoint |
| Lightweight edge rewrites (headers, URL, A/B) | **CloudFront Functions** | viewer request/response only, sub-ms |
| Heavier edge logic (origin calls, auth, image resizing) | **Lambda@Edge** | viewer/origin request/response, 5–30 s |
| Real-time bidirectional | **API Gateway WebSocket** | `$connect/$disconnect/$default` routes; store connection IDs (DynamoDB); fan-out via Lambda |
| Global anycast acceleration | **Global Accelerator** | static anycast IPs, endpoint groups, traffic dials, multi-Region failover |
| CDN + caching + WAF + origin protection | **CloudFront + Origin Shield** | reduces origin load and cross-Region data cost |

## D1.5 Multi-tenant isolation

| Model | Isolation | Cost | Use when |
|---|---|---|---|
| **Silo** (tenant-specific resources/accounts) | strongest (separate VPC/bucket/DB/KMS key) | highest | regulated tenants, noisy neighbours, per-tenant encryption |
| **Pool** (shared resources + tenant key) | logical (tenant ID in partition key, row-level security, per-tenant encryption context) | lowest | many small tenants, SaaS scale |
| Hybrid | mixed | medium | tiered tenants (enterprise silo, free pool) |

Design levers: tenant ID in keys/tags, **Cognito tenant context**, DB **row-level security**, **tenant-specific KMS keys/encryption context**, per-tenant rate limits/quotas, per-tenant observability and cost allocation (tags + Cost Categories), tenant-aware backups/restore, noisy-neighbour isolation via separate queues/shards/cells.

## D1.6 AI/ML integration (new, heavily tested)

**Amazon Bedrock** — managed access to foundation models (Anthropic, Amazon Nova, Meta, Mistral, etc.):
- **On-demand** vs **provisioned throughput** (reserved model units for steady high volume); **batch inference** for offline jobs; **cross-Region inference profiles** for capacity/latency.
- **Knowledge Bases (RAG):** ingest documents → chunk → embed → vector store → retrieve + augment prompt. Vector store options include OpenSearch Serverless, Aurora PostgreSQL (pgvector), and others — pick based on existing data platform.
- **Guardrails:** content filters, denied topics, sensitive-information (PII) redaction, contextual/grounding checks, word filters — applied at inference time.
- **Agents / AgentCore:** see quick card in §7.
- **Model invocation logging** to CloudTrail/S3/CloudWatch for audit (D2 compliance requirement).
- **Evaluations** (Bedrock Evaluations / AgentCore Evaluations): automated quality, groundedness, task success — used in D5 pipelines.

**RAG architecture checklist:** ingestion pipeline (S3 → chunking → embeddings), vector index refresh strategy, chunk size/overlap, metadata filtering, hybrid search, citation/grounding, guardrails, caching, cost per query, latency budget, data access control (per-tenant index/prefix + Lake Formation/IAM).

**Agents architecture checklist:** tool definitions (Lambda/API/MCP via AgentCore Gateway), **Identity** (agent → AWS/tools auth via IdP, OAuth), **Memory** (short/long-term), **Policy** (deterministic tool-call guardrails, Cedar/Dogwood), **Observability** (OpenTelemetry traces), **Evaluations** (quality gates), human approval for risky actions (Step Functions callback).

**SageMaker AI:** real-time endpoints (low latency), serverless inference (spiky/low volume), asynchronous inference (large payloads/long jobs), batch transform (offline); model registry/monitoring for drift.

**Selection rule:** managed foundation models + RAG + agents → **Bedrock**; custom training/fine-tuning/hosting your own model → **SageMaker AI**; classical ML predictions → SageMaker or service-native ML (Forecast/Personalize are out of scope as individual entries in C03).

## D1.7 Hybrid cloud integration

| Need | Service | Exam keywords |
|---|---|---|
| Dedicated private link to AWS | **Direct Connect** (+ DX gateway for multi-Region, SiteLink for DC-to-DC) | consistent latency, large transfer, private |
| Encrypted internet/VPN backup | **Site-to-Site VPN** (2 tunnels per connection) | quick setup, redundancy with DX |
| Hub for VPC/VPN/DX | **Transit Gateway** | many VPCs, segmentation via route tables, transitive routing |
| Global network policy | **Cloud WAN** | “global network, policy-driven, segments, attachments” |
| Data sync/migration | **DataSync** (NFS/SMB/S3, agents, scheduling, verification), **Storage Gateway** (File/Volume/Tape), **Transfer Family** (SFTP/FTPS/AS2 into S3/EFS), **Snowball** family (offline), **Data Transfer Terminal** (physical ingest at AWS locations) | “move 100 TB”, “SMB/NFS to S3”, “SFTP endpoint” |
| AWS on-prem/metro | **Outposts** (AWS hardware in your DC), **Local Zones** (low-latency metro extension), **EKS Anywhere / ECS Anywhere** | “data residency”, “single-digit-ms local processing”, “run ECS/EKS on-prem” |
| Hybrid DNS | **Resolver inbound/outbound endpoints**, **private hosted zones**, split-horizon, conditional forwarding | on-prem↔AWS name resolution |

## D1.8 Network architecture

- **Multi-VPC connectivity:** peering (non-transitive, no overlapping CIDRs), **Transit Gateway** (transitive, segmentation, shared via RAM), **PrivateLink** (service consumption without full network merge), **Lattice** (service-level), **Cloud WAN** (global policy).
- **IP address management:** **VPC IPAM** — pools, allocations across accounts/Regions, avoid overlaps, plan for growth.
- **Network security:** **AWS Network Firewall** (stateful/stateless rules, IDS/IPS signatures, TLS SNI filtering), **WAF** (L7, managed/rate rules), **Shield** (DDoS), **Gateway Load Balancer** (insert third-party appliances), SG/NACL, VPC Flow Logs, Reachability Analyzer.
- **IPv6/dual-stack:** VPC gets `/56`, subnets `/64`; route IPv6 `::/0` to IGW for public, to **egress-only IGW** for private egress; IPv6-only subnets; **DNS64 + NAT64** on NAT gateway for IPv4 destinations from IPv6 clients.
- **Global traffic:** Route 53 routing policies (simple, weighted, latency, failover, geolocation, geoproximity, multivalue), health checks; Global Accelerator for anycast and fast failover; CloudFront for content.

## D1.9 Data, storage, databases

**Lakehouse (new emphasis):** S3 + **AWS Lake Formation** (fine-grained permissions, LF-Tags, cross-account sharing) + **Apache Iceberg** tables (ACID, time travel, schema evolution) + Glue Data Catalog + Athena/Redshift Spectrum/EMR/Spark. Exam trigger: “single copy of data, open table format, analytics + governance”.

**Analytics pipelines:** MSK (managed Kafka) for streaming, Kinesis for AWS-native streaming, Glue for ETL/catalog, EMR (managed Spark) / EMR Serverless, Athena (serverless SQL on S3), Redshift (warehouse; RA3, Serverless, Spectrum, data sharing), AppFlow (SaaS → AWS), QuickSight for BI.

**Database selection matrix**

| Workload | Pick | Notes |
|---|---|---|
| OLTP relational, managed | **RDS** (MySQL/PostgreSQL/SQL Server/Oracle) | Multi-AZ for HA, read replicas for scale |
| High-performance/multi-Region relational | **Aurora** (MySQL/PostgreSQL-compatible) | Global Database, Serverless v2 ACU scaling, I/O-Optimized for high I/O |
| Key-value, single-digit-ms at any scale | **DynamoDB** | on-demand vs provisioned + auto scaling; global tables; DAX for read-heavy |
| Warehouse/OLAP | **Redshift** | RA3, Serverless, Spectrum, concurrency scaling |
| Cache | **ElastiCache** (Redis/Memcached) or **MemoryDB** (durable Redis-compatible) | MemoryDB = primary database durability, not just cache |
| Data sharing | **Lake Formation / Redshift data sharing / Clean Rooms** | cross-account analytics without copying |

**Storage by access pattern:** S3 (objects; classes + lifecycle), EBS (block; gp3 baseline, io2 for high IOPS), EFS (shared NFS; lifecycle to IA/Archive), FSx (Windows/Lustre/NetApp/OpenZFS), instance store (ephemeral). Caching: ElastiCache, DAX, CloudFront.

---

# D2 — Security, Compliance and Governance (25 %)

## D2.1 Identity architecture

| Need | Service | Key facts |
|---|---|---|
| Workforce SSO to many accounts | **IAM Identity Center** | IdP (SAML 2.0 + SCIM), permission sets → IAM roles in accounts, groups flat (no nesting), org instance required for account access |
| App user directory + social login | **Cognito user pools** | sign-up/sign-in, MFA, OAuth 2.0/OIDC, hosted UI, JWT |
| App users → temporary AWS credentials | **Cognito identity pools** | STS credentials, IAM roles per identity, fine-grained via role mapping |
| EKS pods → AWS | **EKS Pod Identity** (trust `pods.eks.amazonaws.com`) or **IRSA** (OIDC) | no long-lived keys; Pod Identity avoids per-cluster OIDC setup |
| On-prem/hybrid workloads → AWS | **IAM Roles Anywhere** | X.509 certs, trust anchor, profile → temporary credentials, no access keys |
| Workforce → private apps without VPN | **AWS Verified Access** | IdP + device trust, access policies per group, logs/audit; fronted by ALB/NLB |
| Fine-grained app authorisation | **Amazon Verified Permissions** | Cedar policy store, policy evaluation API; it authorises, it does not authenticate |
| Least privilege | IAM roles, policies, **permissions boundaries**, session policies, **external ID**, ABAC (tags), Access Analyzer (findings, unused access, policy generation) | explicit deny wins; prefer roles over users |
| Data perimeter | RCP + SCP + resource policies with `aws:PrincipalOrgID`, VPC endpoint policies, `aws:SourceVpc`/`aws:SourceVpce` | “deny access from outside the org”, “only via endpoint” |

**Data perimeter model:** trusted identities (org only) + trusted resources (org-owned) + expected networks (VPC endpoints). Implement with SCP/RCP guardrails, resource-policy conditions, and Access Analyzer to find exceptions.

## D2.2 Encryption and key management

| Layer | Options | Exam notes |
|---|---|---|
| At rest in AWS services | **SSE-S3 / SSE-KMS (AWS managed vs customer managed) / DSSE-KMS / SSE-C (deprecated direction)** | CMK + rotation for control; key policy must allow the caller; KMS key per Region; multi-Region keys for DR |
| Key management | **AWS KMS** (key policies, grants, rotation, quotas, S3 Bucket Keys to cut KMS calls), **AWS CloudHSM** (single-tenant HSM, FIPS 140-2 Level 3, key custody) | CloudHSM when regulation demands dedicated HSM or custom key store; KMS custom key store backs KMS with your CloudHSM |
| In transit | TLS 1.3, **ACM** (public certs with DNS validation; private certs), **AWS Private CA** (internal PKI), ALB/NLB/CloudFront termination | automatic renewal with DNS validation |
| **Post-quantum** | **KMS ML-DSA** (signatures; FIPS 204; key specs ML_DSA_44/65/87) and **KMS ML-KEM** (key encapsulation for key establishment; FIPS 203; ML_KEM_512/768/1024; `ENCAPSULATE_DECAPSULATE`; Encapsulate/Decapsulate APIs) + **hybrid PQ TLS** | ML-DSA = integrity/authenticity; ML-KEM = confidentiality/key establishment. “Harvest now, decrypt later” is the motivation; migrate long-lived data first |

## D2.3 Multi-account governance

| Capability | Service / pattern | Exam notes |
|---|---|---|
| Org structure | **AWS Organizations**, nested OUs (≤5 levels) | SCPs inherit; management account exempt |
| Guardrails | **SCPs** (max permissions, member accounts) and **RCPs** (resource-side guardrails limiting external access) | SCP limits principals in the account; RCP limits who can reach resources; neither grants |
| Landing zone + account vending | **AWS Control Tower** + **Account Factory** (Service Catalog–backed), controls: preventive/detective/proactive, drift detection | “standardised multi-account setup at scale” |
| Central logging | CloudTrail **organization trail** → log archive account; Config **aggregator**; CloudWatch Logs **cross-account subscriptions**; **Security Lake** (OCSF normalised) | include **AI model invocation logging** (Bedrock) for compliance |
| Security services | **GuardDuty** (threat detection, delegated admin), **Security Hub CSPM** (posture + findings aggregation), **Inspector** (vulnerability scanning), **Macie** (sensitive data), **Detective** (investigation) | delegated administrator pattern |
| Sharing | **AWS RAM** (VPC subnets, TGW, Resolver rules, PHZ association, License configs) | share centrally, manage locally |
| Policy & compliance automation | Config rules + conformance packs, Security Hub standards, tag policies, **CFN Hooks** (proactive) | “automated compliance monitoring” |
| Patch/scan | **SSM Patch Manager**, Inspector findings → remediation | operational + security overlap |

## D2.4 Compliance, data residency and evidence

- **Data sovereignty/residency:** constrain Regions via SCP (deny other Regions), use only in-Region services, Local Zones/Outposts where data must stay on-prem, S3 replication only to approved Regions, avoid global services that move data unexpectedly (understand each service's data flow).
- **Audit trails:** CloudTrail (management + data events for sensitive buckets/lambdas), Config history, VPC Flow Logs, AI invocation logs, immutable log archive (S3 Object Lock) in a separate account.
- **Evidence:** **AWS Artifact** (compliance reports + agreements), Audit Manager-style evidence gathering, Well-Architected reviews, threat modelling (STRIDE/attack trees) as a design artefact.
- **Data protection:** classification/tagging, Macie, encryption + access control + least privilege, backup immutability.

## D2.5 Security and responsible AI (new)

- **Bedrock Guardrails:** content filters, denied topics, PII/sensitive info redaction, contextual grounding, word filters — attach to model invocations (also for agents).
- **AgentCore Identity:** agent-to-tool/AWS authentication via existing IdP (Cognito/Okta/Entra), scoped credentials — avoids embedding secrets in agents.
- **AgentCore Policy:** deterministic tool-call guardrails (natural language or Cedar-compatible policy) at the AgentCore Gateway before execution.
- **Human oversight:** Step Functions callback/approval before sensitive actions; audit every agent action via OTEL traces + CloudTrail.
- **Access for AI apps:** model invocation logging, per-tenant data isolation in RAG indexes, prompt-injection defence (treat retrieved content as untrusted), output validation and guardrails.

---

# D3 — Cost-Optimized Architecture Design (15 %)

## D3.1 Pricing and purchase models

| Lever | Details | Trigger |
|---|---|---|
| **Savings Plans** | Compute (EC2/Fargate/Lambda, flexible) vs EC2 Instance (family/Region) vs **Database Savings Plans** (RDS/Aurora/DynamoDB? confirm current scope) | steady baseline, mixed estate |
| **Reserved Instances / Reserved capacity** | EC2, RDS, ElastiCache, Redshift, OpenSearch, DynamoDB reserved capacity | stable, well-known configurations |
| **Spot** | up to 90 % off, interruption-tolerant, diversify types/AZs, `price-capacity-optimized` | batch, CI, stateless workers, Fargate Spot |
| **Provisioned capacity** | Bedrock provisioned throughput, DynamoDB provisioned, DAX, other services | predictable high utilisation |
| **Compute choice** | **Graviton** (better price/performance), Lambda (per-request/per-GB-s, Graviton), Step Functions Standard vs Express, Fargate vs EC2 | “lowest cost at same performance” |
| **Rightsizing/scaling** | Compute Optimizer, target tracking, scheduled scaling, predictive scaling | variable demand with known patterns |

## D3.2 Data transfer and network costs

- **CloudFront:** caching cuts origin egress; **Origin Shield** adds a cache layer (fewer origin fetches); data transfer from AWS origins to CloudFront is generally not charged as egress; use for global content.
- **AZ awareness:** cross-AZ traffic is billed; co-locate chatty components; use Multi-AZ for HA but avoid cross-AZ chatty designs; NLB/ALB cross-AZ options.
- **Private connectivity:** VPC endpoints (gateway endpoints free for S3/DynamoDB; interface endpoints hourly + per GB) vs NAT gateway (per-GB processing) — for high-volume S3/DynamoDB use **gateway endpoints**.
- **TGW / PrivateLink pricing:** per-attachment/hour + per-GB processed — factor into hub-and-spoke designs; Cloud WAN has its own pricing model.
- **Replication:** S3 CRR (per-GB + request + inter-Region transfer), DynamoDB global tables (replicated write costs), cost-aware replication (only needed prefixes/classes).
- **AI/ML:** **cross-Region inference routing** can reduce cost/latency by serving from cheaper/available Regions; batch vs real-time inference; caching.

## D3.3 Allocation, visibility and showback

| Tool | Use |
|---|---|
| **Cost allocation tags + tag policies** | attribute spend; enforce tags with SCP/Config/tag policies |
| **AWS Cost Categories** | group costs by business dimension (team/project/env) without tag-perfect hygiene |
| **CUR / CUR 2.0 + Athena** | line-item detail, unit economics, chargeback |
| **Cost Explorer / Budgets / Cost Anomaly Detection** | trends, alerts, anomaly response; Budgets actions for guardrails |
| **Split cost allocation** | allocate shared EKS/ECS costs to teams (via CUR) |
| **Bedrock token usage** | monitor model usage/cost per app/tenant via usage types/Cost Categories |

## D3.4 Storage, database and observability cost

- **S3:** Intelligent-Tiering for unknown patterns; lifecycle to IA/Glacier; abort incomplete MPUs; expire noncurrent versions; check 128 KB floors.
- **EBS:** **gp3** (baseline IOPS/throughput independent of size) vs gp2; io2 only for extreme IOPS; snapshot lifecycle via **DLM**; delete unattached volumes.
- **EFS:** lifecycle to IA/Archive for cold files; provisioned throughput vs burst only when needed.
- **DynamoDB:** on-demand vs provisioned + auto scaling; reserved capacity for steady; TTL for expiry.
- **RDS/Aurora:** reserved instances; **Aurora Serverless v2** for variable; **Aurora I/O-Optimized** for I/O-heavy workloads; stop dev instances.
- **Redshift:** Serverless for intermittent; RA3 + reserved nodes for steady; Spectrum for S3 data.
- **Backups:** AWS Backup lifecycle/cold storage, vault lock only where required.
- **Observability cost:** log filtering before ingestion, metric aggregation, retention tuning, sampling; avoid debug-level logs in production.

---

# D4 — Resilience, Migration and Business Continuity (15 %)

## D4.1 Migration strategies

| Strategy | Tools | Notes |
|---|---|---|
| **Rehost** (lift & shift) | **AWS Transform / Transform MGN** (formerly Application Migration Service) — agent-based block replication, test/cutover | fastest migration, least change |
| **Replatform** | RDS/Aurora, ECS/Fargate, Lambda for components | reduce ops without rewrite |
| **Refactor** | microservices decomposition, serverless, **strangler fig** | highest effort, highest long-term value |
| **Repurchase / Retire / Retain** | SaaS replacement, decommission, hybrid keep | portfolio decisions |
| Data migration | **DMS** (+ Schema Conversion), **DataSync**, Snowball, Transfer Family, DTT | DMS for DB (CDC), DataSync for files |
| Discovery/waves | migration portfolio assessment, dependency mapping, wave planning | group by dependency and risk |

**AWS Transform** (new): agentic modernisation service covering rehost (Transform MGN), Windows/.NET modernisation and mainframe transformation — know it exists and what problem it solves; exact sub-features change quickly.

## D4.2 Disaster recovery

| Strategy | RTO/RPO | Cost | Key services |
|---|---|---|---|
| Backup & restore | hours–days / hours | lowest | AWS Backup, S3 versioning, snapshots |
| Pilot light | tens of minutes–hours / minutes | low | replicated core data + minimal infra |
| Warm standby | minutes / seconds–minutes | medium | scaled-down full stack always running |
| Multi-site active/active | near-zero / near-zero | highest | multi-Region active traffic |

Tools and patterns:
- **AWS Elastic Disaster Recovery (DRS):** continuous block-level replication to a staging area, recovery instances on demand.
- **Route 53 health checks + failover routing**, **Global Accelerator** endpoint groups, **Amazon Application Recovery Controller (ARC)** readiness checks + routing controls for zonal/Regional failover.
- **AWS Resilience Hub:** define resilience policy (RTO/RPO targets), assess apps, detect drift, run recommended tests, integration with FIS.
- **AWS Fault Injection Service (FIS):** chaos experiments — targets, actions, **stop conditions** (alarms), experiment templates, safety levers; validate DR/failover.
- **Multi-cluster containers:** EKS/ECS in multiple Regions/clusters, GitOps deployment, global load balancing, data replication.

## D4.3 Business continuity and resilience engineering

- **Backup governance:** AWS Backup policies per OU/account, cross-account/cross-Region copies, vault lock (immutability), audit.
- **Ransomware:** immutable backups (Vault Lock), **S3 Object Lock** (governance/compliance, legal hold), **EBS snapshot lock**, MFA delete, isolated recovery account, least-privilege backup roles, restore testing.
- **Resilience patterns:** redundancy at every layer, self-healing (health checks + replacement), **bulkhead** isolation, **circuit breakers**, retries with **exponential backoff + jitter**, timeout budgets, queue-based load levelling, cell-based architecture for blast-radius control, graceful degradation.
- **Testing:** FIS experiments with steady-state hypotheses, DR game days, failover drills, chaos in pre-prod first.

---

# D5 — Operational Excellence and Automation (20 %)

## D5.1 Infrastructure as code

| Tool | Facts | Exam trigger |
|---|---|---|
| **AWS CloudFormation** | declarative templates; stacks; change sets; nested stacks; **StackSets** (multi-account/Region); drift detection; needs IAM service role for StackSets | “manage many accounts consistently” |
| **AWS CDK** | defines infrastructure in code (TypeScript/Python/Java…), synthesises CloudFormation; **constructs** (L1–L3); **aspects**; bootstrap; CDK Pipelines | “reusable, developer-friendly IaC” |
| **Policy as code** | **CFN Hooks** (proactive validation), CDK aspects, Config rules (detective), SCP/RCP (preventive) | “prevent non-compliant resources before creation” |
| **Secrets** | **Secrets Manager** (rotation, native RDS/Redshift integration) vs **SSM Parameter Store** (SecureString; no native rotation) | “rotate DB credentials automatically” → Secrets Manager |
| Multi-account/Region | StackSets with service-managed permissions + Organizations | account vending/standardisation |

## D5.2 CI/CD and DevSecOps

- **CodePipeline** (stages/actions; cross-account via roles; artifact bucket KMS), **CodeBuild** (buildspec, caching, VPC access), **CodeDeploy** (EC2/Lambda/ECS; blue/green, canary, linear; rollback), **CodeCommit** (repos).
- **Security scanning in pipeline:** **Inspector** (vulnerabilities), **ECR image scanning**, dependency scanning, static analysis, **artifact signing** (e.g. Signer) and verification at deploy.
- **Multi-account pipelines:** source in tooling account, deploy via cross-account roles to workload accounts; central artifact bucket; promotion gates.
- **AI quality gates:** **Bedrock Evaluations / AgentCore Evaluations** as a pipeline stage (groundedness, task success), guardrail regression tests.
- **Deployment strategies:** blue/green (instant rollback), canary (limit blast radius), rolling; feature flags where supported.

## D5.3 Observability and monitoring

| Layer | Services | Notes |
|---|---|---|
| Metrics/logs/alarms | **CloudWatch** (metrics, Logs, Logs Insights, alarms, dashboards, embedded metric format) | custom metrics; log groups retention |
| Distributed tracing | **X-Ray** (traces/segments/subsegments, annotations, sampling, service map) | trace Lambda/API/DB flows |
| Standardised instrumentation | **ADOT** (OpenTelemetry collector/distro → X-Ray, CloudWatch, Prometheus) | vendor-neutral, single SDK |
| Containers | **Container Insights** (ECS/EKS) + Prometheus/Grafana (Managed) | cluster/node/pod metrics |
| APM + SLOs | **CloudWatch Application Signals** (service map, SLOs, error budgets) | define SLI/SLO and alert on burn rate |
| User experience | **CloudWatch Synthetics** (canaries), **RUM** (real user monitoring) | “monitor UX/availability end-to-end” |
| AI/ML | Bedrock/AgentCore **Observability** (OTEL), model latency, token usage, error rates, **model drift** (SageMaker Model Monitor), agent quality (Evaluations) | “monitor inference latency/drift” |
| Multi-account | CloudWatch cross-account subscriptions, central log archive, Security Lake | aggregate in a log account |

## D5.4 Operational automation and resilient systems

- **Health & capacity:** ASG/ECS health checks, ELB health checks, Service Quotas monitoring, capacity planning from metrics.
- **Controlled remediation:** circuit breakers, rate limits, bulkheads, retries with exponential backoff + jitter, idempotent handlers, dead-letter queues — “safe automation” when auto-remediation could cause harm.
- **Auto scaling:** target tracking, step scaling, scheduled scaling, **predictive scaling** (ASG), ECS service scaling, Lambda concurrency controls.
- **Failure recovery:** health-check replacement, ECS task replacement, Lambda retries + destinations, Multi-AZ failover (RDS), Route 53 failover.
- **Runbooks & incident response:** SSM Automation runbooks (parameterised, approval steps, rate control), EventBridge → SSM/Lambda, Incident Manager-style paging (if in scope), post-incident analysis.
- **Patching:** SSM Patch Manager baselines, maintenance windows, Inspector findings → automated remediation pipelines.

---

# 7. New-service quick cards (memorise one line each)

| Service | One-line exam definition |
|---|---|
| **Amazon Bedrock** | managed foundation-model API: invoke models, Knowledge Bases (RAG), Guardrails, Agents, Evaluations, provisioned throughput |
| **Bedrock AgentCore** | agentic platform: Runtime, Memory, Gateway (MCP), Identity, Code Interpreter, Browser, Policy, Observability, Evaluations, Registry, Payments — any framework/model |
| **VPC Lattice** | L7 service-network for service-to-service traffic across VPCs/accounts with IAM auth and observability (no route-table plumbing) |
| **AWS Cloud WAN** | policy-driven global network (core network, segments, attachments) for many VPCs/VPN/DX across Regions |
| **AWS Fault Injection Service** | managed chaos engineering: experiment templates, targets, actions, stop conditions/alarms |
| **AWS Resilience Hub** | assess apps against RTO/RPO resilience policy, detect drift, recommend/execute tests |
| **Amazon Application Recovery Controller** | readiness checks + routing control for reliable zonal/Regional failover |
| **Amazon Security Lake** | central OCSF-normalised security data lake (CloudTrail, VPC Flow, Route 53, Security Hub, custom sources) with subscribers |
| **AWS Verified Access** | zero-trust access to private apps without VPN: IdP + device trust + per-group access policies |
| **Amazon Verified Permissions** | Cedar-based fine-grained authorisation service for your applications |
| **AWS Clean Rooms** | privacy-preserving cross-account data collaboration without sharing raw data (analysis rules, differential privacy options) |
| **AWS Lake Formation** | central permissions/catalog layer over S3 data lakes, LF-Tags, cross-account sharing, Iceberg support |
| **AWS CDK** | IaC in general-purpose languages, synthesises CloudFormation; constructs/aspects/pipelines |
| **AWS CodeCommit** | managed Git repositories (pipeline source) |
| **Amazon Data Lifecycle Manager** | automated EBS snapshot/AMI lifecycle policies |
| **Amazon MemoryDB** | Redis-compatible, durable in-memory database (not just a cache) |
| **AWS Local Zones** | AWS infrastructure in metro areas for low-latency single-digit-ms workloads |
| **AWS Transform** | agentic modernisation service (rehost/Transform MGN, Windows/.NET, mainframe) |
| **AWS Data Transfer Terminal** | physical data-ingest locations for very large migrations |
| **AWS Distro for OpenTelemetry (ADOT)** | vendor-neutral telemetry collector/instrumentation → X-Ray/CloudWatch/Prometheus |
| **AWS Cost Categories** | cost grouping by business dimension, independent of tag hygiene, used for showback |
| **KMS post-quantum keys** | ML-DSA (signatures) and ML-KEM (key encapsulation) in KMS + hybrid PQ TLS |

---

# 8. Decision tables — “which service wins”

| Scenario | Answer | Why |
|---|---|---|
| Events from many AWS accounts/Regions, route + replay | **EventBridge** (event bus, archive/replay, cross-account targets) | rules, filtering, replay |
| Ordering per customer + duplicates forbidden | **SQS FIFO** (message groups + dedup) | ordering/exactly-once processing |
| Millions of events, multiple consumers, replay | **Kinesis Data Streams** | shards, retention, replay |
| Load streaming data to S3/Redshift without code | **Data Firehose** | managed delivery + transform |
| Human-approval workflow over days | **Step Functions Standard** | task tokens/callbacks, durable, auditable |
| High-volume short event orchestration | **Step Functions Express** | per-request pricing |
| Cross-VPC/account service-to-service with IAM auth | **VPC Lattice** | service networks, no peering sprawl |
| Expose one service privately to many consumers | **PrivateLink (NLB + interface endpoint)** | consumer keeps own network |
| Many VPCs + VPN + DX hub | **Transit Gateway** (or Cloud WAN for global policy) | transitive routing, segmentation |
| WebSockets/chat/presence | **API Gateway WebSocket** | connection management + Lambda |
| Lightweight edge rewrite | **CloudFront Functions** | sub-ms, cheap |
| Heavy edge logic / origin auth | **Lambda@Edge** | full compute at edge |
| RAG over enterprise documents | **Bedrock Knowledge Bases** | managed ingestion/retrieval |
| Agent with tools + memory + identity | **Bedrock AgentCore** | Runtime/Gateway/Memory/Identity |
| Custom model / fine-tuning / own endpoint | **SageMaker AI** | training + hosting control |
| Warehouse on S3 data | **Redshift Spectrum / Athena** | serverless query vs warehouse |
| Cross-account analytics without copying data | **Clean Rooms / Lake Formation** | governed sharing |
| Workforce SSO to 30 accounts | **IAM Identity Center** | permission sets + assignments |
| App sign-in with social/email + OAuth | **Cognito user pools** | user directory + tokens |
| App users need AWS credentials | **Cognito identity pools** | STS credentials + IAM roles |
| Pods need AWS permissions | **EKS Pod Identity** | no OIDC setup, no keys |
| On-prem server needs AWS permissions | **IAM Roles Anywhere** | X.509 → temp creds |
| Private app access without VPN, per group | **Verified Access** | IdP + device posture |
| Fine-grained app authorisation | **Verified Permissions** | Cedar policies |
| Quantum-resistant signatures | **KMS ML-DSA** | FIPS 204 |
| Quantum-resistant key establishment | **KMS ML-KEM** | FIPS 203 |
| Dedicated HSM required | **CloudHSM** | single-tenant HSM |
| Block public access org-wide | **S3 Block Public Access + SCP/RCP** | preventive guardrails |
| Automated compliance checks | **Config rules / Security Hub CSPM** | detective |
| Prevent misconfigured resources in IaC | **CloudFormation Hooks** | proactive |
| Central security data for SIEM | **Security Lake** | OCSF normalisation |
| Chaos test an AZ failure | **FIS** | experiment + stop conditions |
| Measure DR readiness, detect drift | **Resilience Hub** | policy assessment |
| Zonal/Regional failover control | **ARC routing control** | readiness checks |
| Ransomware-proof backups | **Vault Lock + Object Lock + snapshot lock** | immutability |
| Immutable audit logs | **S3 Object Lock (compliance)** | WORM |
| Cross-account artifact promotion | **CodePipeline cross-account roles** | role delegation |
| Vulnerability scanning in pipeline | **Inspector + ECR scan on push** | shift-left |
| Trace Lambda→API→DB | **X-Ray / ADOT** | segments + service map |
| SLO/error budget monitoring | **CloudWatch Application Signals** | SLI/SLO, burn-rate alerts |
| Canary + real user monitoring | **CloudWatch Synthetics + RUM** | UX health |
| Monitor model drift | **SageMaker Model Monitor** | data/model quality |

---

# 9. Exam traps (SAP-C03 edition)

1. **“SCPs apply to the management account.”** No — member accounts only (RCPs likewise resource-side guardrails).
2. **“SCP grants permissions.”** Never — it only caps.
3. **“Verified Access and Verified Permissions are the same.”** No — Verified Access = zero-trust *network* access to apps; Verified Permissions = application *authorisation* (Cedar).
4. **“Cognito user pools are for AWS credentials.”** No — user pools authenticate users; **identity pools** issue AWS credentials.
5. **“ML-KEM makes signatures.”** Wrong — ML-KEM is key encapsulation; signatures are ML-DSA.
6. **“CloudHSM is mandatory for KMS compliance.”** Only when dedicated HSM/key custody required; KMS custom key store bridges both.
7. **“Lattice replaces Transit Gateway.”** No — Lattice is L7 service networking; TGW is L3 routing.
8. **“Cloud WAN is just a bigger TGW.”** It is a policy-driven global network construct — use it for global segmentation, not just hub routing.
9. **“Resilience Hub runs the tests for you.”** It assesses/recommends; FIS executes experiments.
10. **“FIS needs no safety controls.”** Always set stop conditions and least-privilege targets.
11. **“ARC replaces Route 53 failover.”** ARC gives readiness checks + routing controls; Route 53 does DNS failover — often combined.
12. **“Security Lake replaces CloudTrail.”** It aggregates/normalises data; CloudTrail remains the source.
13. **“Gateway endpoints work for all services.”** S3/DynamoDB only; interface endpoints for the rest.
14. **“NAT gateway is HA across AZs.”** Per-AZ redundancy only — one per AZ.
15. **“Cross-AZ traffic is free.”** It is billed; design for AZ locality.
16. **“Aurora Serverless is always cheaper.”** Only for intermittent/variable load; steady high load → provisioned/reserved.
17. **“Aurora I/O-Optimized is always cheaper.”** Only for I/O-heavy workloads; compare both models.
18. **“Step Functions Express is exactly-once.”** Express is at-least-once; Standard is exactly-once.
19. **“SQS FIFO gives global ordering.”** Ordering per **message group** only.
20. **“Kinesis guarantees ordering across shards.”** Only within a shard (partition key).
21. **“Bedrock Guardrails replace IAM.”** They filter content; access control is still IAM/AgentCore Identity/Verified Permissions.
22. **“RAG means fine-tuning the model.”** No — retrieve + augment the prompt; fine-tuning is a different lever.
23. **“AI agents can be given broad IAM roles; the guardrail handles safety.”** No — least privilege + AgentCore Policy/Identity + human approval.
24. **“Data residency is achieved by encryption.”** Encryption ≠ residency; control Region/transfer.
25. **“Everything in the old SAP-C02 blueprint still matters equally.”** Mostly, but removed services (IoT, WorkSpaces, Media, SES, Batch, Proton, Service Catalog…) are gone, and new weights shift study time.

---

# 10. Practice questions

> Answer before reading §10.1. One best answer unless stated.

**Q1.** A company needs a multi-step order-fulfilment workflow that waits days for a human approval and must be exactly-once with a full audit trail. What do you use?
A. SQS FIFO + Lambda
B. Step Functions Standard with a callback task token
C. Step Functions Express
D. EventBridge Scheduler + Lambda

**Q2.** 40 microservices across 12 VPCs in 5 accounts need IAM-authenticated service-to-service calls without VPC peering. Which service?
A. Transit Gateway
B. VPC Lattice
C. PrivateLink interface endpoints for each service
D. CloudFront

**Q3.** A team must give an application's users fine-grained authorisation (“can this user edit this document?”) using Cedar policies outside the app code. Which service?
A. Cognito user pools
B. Verified Access
C. Verified Permissions
D. IAM Identity Center

**Q4.** Which KMS key type is used for quantum-resistant **digital signatures**?
A. ML-KEM
B. ML-DSA
C. RSA-2048
D. ECDSA P-256

**Q5.** A workload must survive an AZ failure with minimal blast radius and needs a controlled failover mechanism with readiness checks. Which combination?
A. Route 53 failover only
B. ARC readiness checks + routing controls
C. S3 CRR
D. Multi-AZ RDS

**Q6.** Which tool executes controlled chaos experiments with stop conditions to validate DR?
A. Resilience Hub
B. Fault Injection Service
C. CloudWatch Synthetics
D. AWS Backup

**Q7.** A lakehouse needs open table format with ACID/time travel on S3, governed by Lake Formation. Which format?
A. Parquet only
B. Apache Iceberg
C. CSV
D. JSON

**Q8.** Two companies want to collaborate analytically without sharing raw data; each side must control allowed queries. Which service?
A. Redshift data sharing
B. AWS Clean Rooms
C. S3 cross-account replication
D. Lake Formation only

**Q9.** Pods in EKS need AWS permissions without OIDC providers or long-lived keys. Which feature?
A. IRSA
B. EKS Pod Identity
C. Instance profile
D. Access keys in secrets

**Q10.** On-premises servers must call AWS APIs with temporary credentials derived from X.509 certificates. Which service?
A. IAM Identity Center
B. IAM Roles Anywhere
C. Cognito identity pools
D. Verified Access

**Q11.** An application must authenticate users with Google/Apple and issue OAuth tokens, then let them call API Gateway. Which combination?
A. Identity Center + permission sets
B. Cognito user pools (tokens) + API Gateway authorizer
C. Verified Permissions + IAM
D. CloudHSM + ACM

**Q12.** A global company needs policy-driven segmentation across 60 VPCs, 20 VPNs and DX across 4 Regions with a single network policy. Which service?
A. Transit Gateway peering mesh
B. AWS Cloud WAN
C. VPC peering
D. Global Accelerator

**Q13.** A high-volume event-driven app needs at-least-once processing with short workflows (<5 min) at minimum cost. Which orchestration?
A. Step Functions Standard
B. Step Functions Express
C. SQS + Lambda only
D. SWF

**Q14.** Which statement about NLB vs ALB is correct?
A. ALB supports static IPs and TLS passthrough
B. NLB is L4, supports static IPs and TLS passthrough; ALB is L7
C. Both are L7
D. NLB integrates with WAF directly

**Q15.** Audit logs must be immutable for 7 years with legal hold capability. What do you use?
A. S3 versioning + lifecycle
B. S3 Object Lock (compliance mode) on a log archive account
C. Glacier only
D. EBS snapshots

**Q16.** A company must cut NAT gateway costs for millions of S3 requests/day from private subnets. Best fix?
A. Larger NAT gateway
B. Gateway VPC endpoint for S3
C. Interface endpoint for S3
D. CloudFront

**Q17.** A SaaS has 5,000 small tenants with strict per-tenant isolation and per-tenant encryption keys. Which model?
A. Pooled everything
B. Silo per tenant (accounts/resources) with per-tenant KMS keys
C. Shared KMS key, tenant tag
D. Single account, no isolation

**Q18.** Model responses must be filtered for PII and blocked topics before users see them. Which Bedrock feature?
A. Knowledge Bases
B. Guardrails
C. Provisioned throughput
D. Batch inference

**Q19.** Which combination gives the least operational overhead for a serverless API with variable traffic?
A. EC2 ASG + ALB
B. API Gateway + Lambda + DynamoDB on-demand
C. EKS + Fargate
D. ECS EC2

**Q20.** A pipeline must validate infrastructure templates before deployment and block non-compliant resources. Which mechanism?
A. Config rules only
B. CloudFormation Hooks (proactive validation)
C. GuardDuty
D. Inspector

**Q21.** Which service provides a Zonal/Regional failover control plane with routing controls and readiness checks?
A. ARC
B. Resilience Hub
C. Trusted Advisor
D. Service Quotas

**Q22.** A DR requirement: RTO 15 minutes, RPO seconds, minimal cost. Which strategy?
A. Backup & restore
B. Pilot light
C. Warm standby
D. Multi-site active/active

**Q23.** Which service normalises security findings across accounts into OCSF and feeds a SIEM?
A. Security Lake
B. GuardDuty
C. Macie
D. Inspector

**Q24.** A container image registry must block deployment of vulnerable images. Which controls?
A. ECR scan on push + pipeline gate on findings
B. IAM policy only
C. S3 Object Lock
D. VPC Flow Logs

**Q25.** Which statement about Bedrock provisioned throughput is true?
A. It is cheaper for spiky, low-volume traffic
B. It reserves model capacity for steady, high-volume workloads
C. It is required for all invocations
D. It disables Guardrails

## 10.1 Answers

**A1 — B.** Step Functions Standard: durable, auditable, exactly-once, task tokens for human approval/callbacks.
**A2 — B.** VPC Lattice: L7 service network across VPCs/accounts with IAM auth; no peering/route plumbing.
**A3 — C.** Verified Permissions: Cedar policies for application authorisation decisions.
**A4 — B.** ML-DSA = digital signatures (FIPS 204). ML-KEM = key encapsulation (FIPS 203).
**A5 — B.** ARC readiness checks + routing controls; Route 53 alone is only DNS.
**A6 — B.** FIS runs chaos experiments with targets, actions and stop conditions.
**A7 — B.** Apache Iceberg: ACID, time travel, schema evolution on S3, Lake Formation-governed.
**A8 — B.** Clean Rooms: privacy-preserving collaboration with analysis rules.
**A9 — B.** EKS Pod Identity: no OIDC provider, temporary credentials via the pod identity agent.
**A10 — B.** IAM Roles Anywhere: X.509 trust anchor → temporary AWS credentials.
**A11 — B.** Cognito user pools issue OIDC/JWT tokens; API Gateway validates them.
**A12 — B.** Cloud WAN: policy-driven global core network with segments.
**A13 — B.** Express workflows: high volume, short duration, per-request pricing.
**A14 — B.** NLB = L4 (TCP/UDP, static IP, TLS passthrough); ALB = L7 (routing, WAF).
**A15 — B.** Object Lock compliance mode in an isolated log-archive account (+ legal hold).
**A16 — B.** Gateway endpoint for S3 keeps traffic private and avoids NAT per-GB charges.
**A17 — B.** Silo model for strict isolation + per-tenant KMS keys.
**A18 — B.** Guardrails: content filters, denied topics, PII redaction.
**A19 — B.** API Gateway + Lambda + DynamoDB on-demand = least ops, scales with traffic.
**A20 — B.** CloudFormation Hooks validate before resource creation (proactive).
**A21 — A.** Application Recovery Controller.
**A22 — C.** Warm standby (RTO minutes, RPO seconds; pilot light is too slow for 15 min).
**A23 — A.** Security Lake normalises to OCSF and supports SIEM subscribers.
**A24 — A.** ECR scan on push + pipeline gate (Inspector for deeper scanning).
**A25 — B.** Provisioned throughput reserves capacity for steady high-volume inference.

---

# 11. Rapid recall

| Fact | Value |
|---|---|
| Exam | 75 Q (65 scored + 10 unscored), 180 min, pass **720** |
| Domain weights | 25 / 25 / 15 / 15 / 20 |
| Step Functions | Standard = exactly-once, ≤1 yr; Express = at-least-once, ≤5 min |
| SQS FIFO ordering | per **message group**; dedup window ≈5 min |
| Kinesis ordering | per **shard** (partition key) |
| Lattice vs PrivateLink vs TGW | L7 service network / private service exposure / L3 routing hub |
| Cognito | user pools = authN+tokens; identity pools = AWS credentials |
| EKS Pod Identity | trust `pods.eks.amazonaws.com`; no OIDC provider |
| Roles Anywhere | X.509 certs → temporary AWS creds for on-prem |
| Verified Access vs Permissions | network zero-trust vs application authorisation (Cedar) |
| ML-DSA vs ML-KEM | signatures (FIPS 204) vs key encapsulation (FIPS 203) |
| Security Lake | OCSF-normalised security data lake |
| FIS vs Resilience Hub vs ARC | chaos execution / assessment / failover control |
| DR strategies | backup&restore, pilot light, warm standby, active-active |
| Ransomware | Vault Lock + Object Lock + EBS snapshot lock + MFA delete |
| Data transfer | gateway endpoint free (S3/DDB); NAT/TGW/PrivateLink per-GB |
| Aurora cost modes | Serverless v2 (variable) / I/O-Optimized (I/O-heavy) / reserved (steady) |
| Observability | X-Ray + ADOT + Container Insights + Synthetics/RUM + Application Signals (SLOs) |
| IaC policy | CFN Hooks (proactive) + Config rules (detective) + SCP/RCP (preventive) |
| Migration tools | Transform/Transform MGN, DMS(+SCT), DataSync, Snowball, DTT |

---

# 12. Glossary

| Term | Meaning |
|---|---|
| **AgentCore** | Bedrock agentic platform (runtime, memory, gateway, identity, policy, observability, evaluations) |
| **ADOT** | AWS Distro for OpenTelemetry |
| **ARC** | Application Recovery Controller — readiness checks + routing control |
| **Blue/green** | two environments, switch traffic, instant rollback |
| **Canary** | shift a small % of traffic first |
| **Cedar** | AWS open-source authorisation policy language (Verified Permissions, AgentCore Policy) |
| **Clean Rooms** | privacy-preserving cross-account data collaboration |
| **Cloud WAN** | policy-driven global network |
| **DLM** | Data Lifecycle Manager — EBS snapshot/AMI lifecycle |
| **EDR/DRS** | Elastic Disaster Recovery — block-level replication + recovery |
| **FIS** | Fault Injection Service — chaos experiments |
| **Guardrails** | Bedrock content/PII/topic safety controls |
| **Iceberg** | open table format (ACID, time travel) |
| **Lake Formation** | data-lake permissions & catalog layer |
| **Lattice** | L7 service networking across VPCs/accounts |
| **ML-DSA** | post-quantum signature algorithm (FIPS 204) |
| **ML-KEM** | post-quantum key encapsulation (FIPS 203) |
| **OCSF** | Open Cybersecurity Schema Framework (Security Lake) |
| **RCP** | resource control policy (Organizations guardrail on resource access) |
| **RAG** | retrieval-augmented generation |
| **Resilience Hub** | app resilience assessment against RTO/RPO policy |
| **SLO/SLI** | objective / indicator used for reliability targets |
| **Strangler fig** | incremental refactor pattern replacing legacy piece by piece |
| **Silo/pool** | multi-tenant isolation models |
| **Verified Access** | zero-trust access to private applications |
| **Verified Permissions** | fine-grained application authorisation (Cedar) |

---

# 13. Sources

- [SAP-C03 exam guide](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03.md) · [comparison with SAP-C02](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/sap-03-comparison.md) · [Domain 1](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain1.md) · [Domain 2](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain2.md) · [Domain 3](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain3.md) · [Domain 4](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain4.md) · [Domain 5](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain5.md) · [in-scope services](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/sap-03-in-scope-services.md)
- [Amazon Bedrock AgentCore overview](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- [Post-quantum keys in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/pqkeys.html)
- [AWS certification page](https://aws.amazon.com/certification/certified-solutions-architect-professional/)
