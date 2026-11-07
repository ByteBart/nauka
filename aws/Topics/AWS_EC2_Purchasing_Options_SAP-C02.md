# EC2 purchasing options — Savings Plans vs Reserved Instances vs Spot vs Capacity Reservations
## Study notes for AWS Certified Solutions Architect – Professional (SAP-C02)

> **Scope:** On-Demand, Savings Plans, Reserved Instances, Spot Instances, On-Demand Capacity Reservations, Capacity Blocks for ML, Dedicated Hosts/Instances, cost math, portfolio strategy, exam traps and practice questions.
> **Knowledge state:** November 2026, verified against current AWS documentation (links in §21).
> **Key idea:** There are **two independent dimensions** — the *billing/discount vehicle* (how you pay less) and the *capacity assurance mechanism* (whether capacity is held for you). Confusing them is the #1 source of wrong answers in this topic.
> **Prices are not in this document.** Every discount figure is a *maximum* (“up to X %”) that depends on Region, instance family, platform, term and payment option. Never memorise dollar amounts — memorise the mechanics and the decision rules.

---

## 0. TL;DR — the answer you should be able to give in 30 seconds

| Option | What it really is | Max discount | Commitment | Capacity guaranteed? | Interruptible? |
|---|---|---|---|---|---|
| **On-Demand** | Pay-as-you-go list price | — (baseline) | none | no | no |
| **Compute Savings Plan** | $/hour commitment, applies to any EC2 family/size/OS/tenancy/Region + Fargate + Lambda | up to **66 %** | 1 or 3 yr | no | no |
| **EC2 Instance Savings Plan** | $/hour commitment, locked to one instance family in one Region | up to **72 %** | 1 or 3 yr | no | no |
| **Standard RI** | Discount tied to a configuration; regional or zonal; can be modified, can be sold | up to **72 %** | 1 or 3 yr | **only if zonal** | no |
| **Convertible RI** | Discount tied to a configuration; can be exchanged for another Convertible RI | up to **66 %** | 1 or 3 yr | **only if zonal** | no |
| **Spot** | Unused EC2 capacity from Spot pools | up to **90 %** | none | no | **yes — 2 min notice** |
| **Capacity Reservation** | Reserves capacity in one AZ; billed at On-Demand rate whether used or idle | **0 %** (layer SP/regional RI on top) | none (future-dated: 14-day min) | **yes** | no (except *interruptible* CRs, a newer variant) |
| **Capacity Blocks for ML** | Time-boxed GPU capacity for ML workloads | supply/demand pricing | fixed window | **yes, for the window** | no |

**One-liner for the exam:** *Savings Plan = flexible $/hour discount, no capacity. Reserved Instance = rigid configuration discount, zonal version also holds capacity. Spot = 90 % off, interruptible. Capacity Reservation = full On-Demand price, buys availability — always layer a Compute SP or Regional RI to discount it.*

---

## 1. Exam context — how this topic is tested in SAP-C02

- SAP-C02 (at the time of writing): **75 questions, 180 minutes, scaled score 750/1000**, multiple choice and multiple response.
- Four domains (approx. weights): **1 — Design Solutions for Organizational Complexity (26 %)**, **2 — Design for New Solutions (29 %)**, **3 — Continuous Improvement for Existing Solutions (25 %)**, **4 — Accelerate Workload Migration and Modernization (20 %)**.
- There is **no separate “cost” domain** in the Pro exam (unlike SAA-C03). Cost optimisation is woven through the domains, mostly in **Domain 3** (determine cost-optimisation strategies, review and improve existing architectures) and **Domain 1** (multi-account billing, governance, consolidated billing, SCPs, tagging).
- Exam style: long scenario, several plausible options, one “most cost-effective” or “most appropriate” answer. The winning answer usually satisfies **all stated constraints** (capacity guarantee, compliance, term, flexibility), not only the cheapest one.
- Related file in this folder: [AWS_CUR_CostExplorer_Budgets_SAP-C02.md](AWS_CUR_CostExplorer_Budgets_SAP-C02.md) — the *analysis* side of cost management (CUR, Cost Explorer, Budgets). This document is the *purchasing* side.

---

## 2. The mental model — two independent dimensions

### 2.1 Dimension A: billing / discount vehicle

Answers: *“How do I pay less for the compute I actually use?”*
Options: On-Demand, Savings Plans, Reserved Instances, Spot.

### 2.2 Dimension B: capacity assurance

Answers: *“Is capacity guaranteed to be available for me?”*
Options: Capacity Reservations (immediate or future-dated), Capacity Blocks for ML, and — as a side effect — **zonal Reserved Instances**.

A Capacity Reservation gives you **no discount**. AWS bills it at the equivalent On-Demand rate whether or not you launch instances into it. You then apply a **discount on top**: a Compute Savings Plan or a **Regional** (not zonal) Reserved Instance.

### 2.3 Decision tree

```
Do you need capacity held in a specific AZ (compliance, DR, surge, licence)?
├─ YES → Capacity Reservation (immediate-use, or future-dated if booked ahead)
│         └─ add a Compute Savings Plan or Regional RI to discount the bill
│         └─ short GPU/ML window? → Capacity Blocks for ML
└─ NO
   Can the workload be interrupted/restarted (stateless, batch, CI/CD)?
   ├─ YES → Spot (diversified, price-capacity-optimized) + On-Demand or SP baseline
   └─ NO
      Is usage predictable for ≥1 year?
      ├─ NO  → On-Demand (and re-evaluate later)
      └─ YES → Will the instance family / OS / tenancy / Region stay frozen?
               ├─ NO  → Compute Savings Plan (most flexible) or Convertible RI
               └─ YES → EC2 Instance Savings Plan or Standard RI (deepest discount)
                        └─ need per-socket/per-core BYOL? → Dedicated Hosts
```

### 2.4 The three “gotchas” hidden in the diagram

1. **A Savings Plan never reserves capacity.** “Our SLA requires guaranteed capacity in `eu-west-1a`” → a Savings Plan is not the answer.
2. **A Capacity Reservation never saves money by itself.** “We need to reduce cost and guarantee capacity” → two purchases, not one.
3. **Spot is not a baseline.** It is always a *supplement* to an On-Demand/Savings Plan floor.

---

## 3. On-Demand Instances — the reference point

- Pay per use at the published rate, **no commitment**, no upfront payment, no capacity guarantee (launch fails with an **Insufficient Capacity Error, ICE** if the AZ has no capacity).
- Billing granularity: **per second with a 60-second minimum** for most platforms; some platforms (e.g. SUSE) are billed per hour.
- Can be stopped/terminated at any time; you pay only while running (EBS charges continue while stopped).
- It is the correct answer for: new/unproven workloads, short-lived or unpredictable usage, spiky traffic, workloads being migrated, and as the **baseline that makes Spot safe**.
- On-Demand is also the **discount baseline**: all RI/SP discounts are expressed as a discount *against* the On-Demand rate, and an unused Capacity Reservation is billed *at* the On-Demand rate.

---

## 4. Savings Plans (SP)

### 4.1 Definition

A **commitment to spend a fixed amount of dollars per hour** on eligible compute for **1 or 3 years**, in exchange for lower rates. Examples in AWS documentation: **one year = 365 days (31,536,000 s)**, **three years = 1,095 days (94,608,000 s)**.

- You are **billed the commitment** whether or not you consume it (the SP's discount is applied to eligible usage up to the commitment; unused commitment is not refunded).
- Discounted prices are **locked for the whole term**.
- Payment options: **All upfront**, **Partial upfront**, **No upfront** — the same three as RIs.
- The **price per unit does not depend on the size of the commitment** (a $1/h plan and a $50/h plan get the same rate for the same configuration).

### 4.2 The three types

| Type | Max discount | Applies to | Scope |
|---|---|---|---|
| **Compute Savings Plan** | up to **66 %** | EC2 (any family, size, OS, tenancy, Region), **AWS Fargate**, **AWS Lambda** | account/Organisation-wide |
| **EC2 Instance Savings Plan** | up to **72 %** | EC2 only, within **one instance family in one Region** (size, OS, tenancy can vary) | one family + Region |
| **SageMaker AI Savings Plan** | varies | SageMaker AI instance usage (any family/size/component/Region) | account/Organisation-wide |

Why the two EC2 types exist: Compute SP ≈ *Convertible RI* economics (more flexible, smaller discount); EC2 Instance SP ≈ *Standard RI* economics (deeper discount, less flexible). AWS itself diagrams it this way:

| | Compute SP | EC2 Instance SP | Convertible RI | Standard RI |
|---|---|---|---|---|
| Max discount vs On-Demand | up to 66 % | up to 72 % | up to 66 % | up to 72 % |
| Commitment measured in **money** | ✓ | ✓ | — | — |
| Any instance family automatically | ✓ | — | — | — |
| Any instance size automatically | ✓ | ✓ | via size flexibility* | via size flexibility* |
| Any tenancy / OS automatically | ✓ | ✓ | — | — |
| Fargate (ECS/EKS) automatically | ✓ | — | — | — |
| Lambda automatically | ✓ | — | — | — |
| Any Region automatically | ✓ | — | — | — |
| Term 1 or 3 years | ✓ | ✓ | ✓ | ✓ |

\* Regional Standard/Convertible RIs give instance size flexibility (Linux/UNIX, shared tenancy only — see §5.5). Convertible RIs require a **manual exchange** to change family/OS/tenancy.

### 4.3 What a Savings Plan does **not** cover

- **Spot usage** — a Spot Instance is never discounted by an SP, and **Spot spend does not consume your SP commitment**. (An SP on top of Spot is a classic wrong answer.)
- **Usage already covered by Reserved Instances** — RIs are applied first; the SP only sees what is left.
- **Dedicated Host charges** — Dedicated Hosts are billed per host and have their own reservation model (§9).
- **Storage, data transfer, RDS, ElastiCache, Redshift, OpenSearch, DynamoDB** etc. — SP covers compute only (EC2, Fargate, Lambda; SageMaker via its own SP).
- **Capacity** — an SP is a price agreement, not a reservation.

### 4.4 How the discount is applied

- Applied **hourly**, automatically, with no action needed after purchase.
- The SP applies the discounted rate to eligible usage up to its hourly commitment; usage above the commitment is billed at On-Demand (or Spot) rates.
- If your usage is **below** the commitment, you still pay the commitment — the difference is effectively lost for that hour.
- **On-Demand Capacity Reservations are eligible**: the SP discounts both the instances running inside the CR and the **unused** portion of the CR, with discounts preferentially applied to the instance usage first.
- SP prices differ for **SUSE Linux Enterprise Server (SLES)** versus the corresponding RI price.

### 4.5 Scope, sharing and Organisation behaviour

- By default, an SP **applies across all accounts in the consolidated billing family / AWS Organisation** (“automatically applies across all accounts in AWS Organizations by default”).
- RI and SP discount sharing is a **billing preference** — the management account can change discount-sharing settings for the Organisation; when sharing is on, the buying account’s benefit flows to other accounts (this is how central teams buy commitments for the whole org).
- Consolidated billing treats all member accounts as **one account** for the purpose of applying discounts.

### 4.6 Rules of the plan lifecycle (exam facts)

| Rule | Detail |
|---|---|
| Term | 1 or 3 years only |
| Payment | All upfront / Partial upfront / No upfront |
| Modification | **Cannot be modified** (no exchange of plans) |
| Cancellation | **Cannot be cancelled** during the term |
| Return window | Can be **returned within 7 days** of purchase **if the hourly commitment is ≤ $100/hour** |
| Expiry | At the end of the term the discount stops; usage reverts to On-Demand (no auto-renew) |
| Queued purchases | You can queue a purchase to start in the future (AWS documents queuing up to ~3 years ahead) |
| Limits | **No purchase limit** (unlike RI quotas) |
| Recommendations | AWS Cost Explorer **Savings Plans recommendations** (based on 7/30/60-day lookback) |
| Monitoring | Cost Explorer **coverage** and **utilisation** reports; Budgets can alert on utilisation |

### 4.7 Coverage vs utilisation — the two metrics that matter

- **Coverage** = share of eligible spend that is *actually discounted*. Rule of thumb: target **~70–90 %** coverage; 100 % means you are probably over-committed.
- **Utilisation** = share of the commitment that is *actually used*. Rule of thumb: keep it **above ~95 %**; below that you are paying for discounts you do not use.
- Buy decisions are made from Cost Explorer recommendations, based on historical usage — never from gut feel.

### 4.8 When a Savings Plan is the right answer

- Mixed compute estate (EC2 across families/Regions + Fargate + Lambda).
- Modernisation / migration programme (workloads will change shape).
- Need the **simplest** commitment model (no exchanges, no modifications, no AZ decisions).
- Multi-Region or multi-account environments.
- You already need to discount On-Demand Capacity Reservations.

### 4.9 When a Savings Plan is **not** the right answer

- Capacity must be guaranteed → use a Capacity Reservation.
- Workload is interruptible and cheap → Spot alone is better than SP (SP never applies to Spot).
- Family/Region **and** discount depth matter more than flexibility → EC2 Instance SP or Standard RI.
- Usage is not stable for 12 months → don't commit; stay On-Demand.
- Non-EC2 services → their own reservations (RDS, ElastiCache, Redshift, OpenSearch, DynamoDB reserved capacity …).

---

## 5. Reserved Instances (RI)

### 5.1 Definition

An RI is **not a physical instance** — it is a **billing discount** applied to matching On-Demand usage in the account. If a matching On-Demand instance is already running, the discount applies immediately and automatically (no restart needed). Maximum discount **up to 72 %**.

### 5.2 Attributes that determine price and matching

1. **Instance type** (family + size)
2. **Region**
3. **Tenancy** (shared / dedicated)
4. **Platform / OS** (Linux/Unix, Windows, RHEL, SUSE, …)
5. **Term** — 1 or 3 years
6. **Payment option** — All upfront / Partial upfront / No upfront
7. **Offering class** — Standard / Convertible
8. **Scope** — Regional / Zonal

### 5.3 Regional vs Zonal — the most examined distinction

| | **Regional RI** | **Zonal RI** |
|---|---|---|
| Scope | any AZ in one Region | one specific AZ |
| Capacity reservation | **no** | **yes** — reserves capacity in that AZ |
| Instance size flexibility | **yes** (Linux/UNIX, shared tenancy) | no |
| Cross-account discount sharing | yes (via consolidated billing) | yes for the *discount*; the **capacity is only for the owning account** |
| Applies to a Capacity Reservation | **yes** | **no** (a zonal RI discount does not apply to CR usage) |
| Normal use case | discount steady usage with flexibility | discount + guaranteed capacity for a fixed config in one AZ |

Note: because zonal-RI capacity cannot be shared with other accounts, AWS docs recommend an **On-Demand Capacity Reservation** (shareable via AWS RAM) when you need to share guaranteed capacity across accounts.

### 5.4 Offering classes

| | **Standard RI** | **Convertible RI** |
|---|---|---|
| Max discount | up to **72 %** | up to **66 %** |
| Modify (AZ, scope, size in family) | ✓ | ✓ |
| Exchange for different family/OS/tenancy | ✗ | ✓ (manual exchange, new RI of **equal or higher value**) |
| Sell on the **RI Marketplace** | ✓ | ✗ |
| Typical use | stable, well-understood workload | expected platform change (e.g. version/OS/family migration) |

Both classes: **cannot be cancelled** after purchase; **do not auto-renew** (at expiry you keep the instances, billed On-Demand).

### 5.5 Instance size flexibility and the normalization factor

Applies to **Regional RIs** only, and only for **Linux/UNIX with shared (default) tenancy**.

Normalization factors (memorise the pattern: each size step doubles the number of units):

| Size | Factor | Size | Factor |
|---|---|---|---|
| nano | 0.25 | 2xlarge | 16 |
| micro | 0.5 | 3xlarge | 24 |
| small | 1 | 4xlarge | 32 |
| medium | 2 | 8xlarge | 64 |
| large | 4 | 12xlarge | 96 |
| xlarge | 8 | 16xlarge | 128 |
| | | 24xlarge | 192 |
| | | 32xlarge | 256 |

- A reservation’s **footprint** = number of instances × normalization factor. Example: four `t2.medium` RIs = 4 × 2 = **8 units** → covers one `t2.large` (4) plus two `t2.medium` (2 + 2).
- The discount is applied **from the smallest to the largest instance size** within the family.
- **Not supported** for: zonal RIs, Windows / Windows+SQL / RHEL / SUSE / Linux+SQL platforms, dedicated tenancy, and certain GPU/accelerator families (e.g. G4ad, G4dn, G5, G5g, G6, G6e, G6f, Gr6, Gr6f, hpc7a, P5, Inf1, Inf2, u7i-6tb/u7i-8tb).
- Bare-metal sizes use the factor of the equivalent largest virtualised size in the same family (`i3.metal` = 128).

### 5.6 Modification rules (Standard or Convertible; no fee)

You can modify:

| Attribute | Supported | Limits |
|---|---|---|
| Availability Zone (within Region) | Linux & Windows | — |
| Scope (regional ↔ zonal) | Linux & Windows | zonal → regional **loses the capacity reservation**; regional → zonal **loses AZ + size flexibility** |
| Instance size | **Linux/UNIX only** | same family **and generation**, default tenancy, size flexibility required, **same footprint** |

Additional rules:
- You can **split** (10 in `us-east-1a` → 5 + 5 in two AZs) and **merge** reservations (four `t2.small` → one `t2.large`), as long as the footprint matches.
- The modified reservation **ends on the original reservation’s end date** (a 3-year RI with 16 months left becomes a 16-month reservation) and shows a $0 fixed price.
- Effective immediately at the hour of the request; no new invoice, no fee.
- Cannot modify a reservation that is listed on the Marketplace, not yet active, or has a pending modification.
- You cannot cancel an RI — modification/exchange/sale are the only exits.

### 5.7 Exchange (Convertible only)

- Exchange one or more Convertible RIs for another Convertible RI with a **different family/OS/tenancy/term**.
- Manual action; the new RI must be of **equal or higher value** than the remaining value of the ones exchanged.
- Standard RIs **cannot be exchanged** — only modified (and sold).

### 5.8 RI Marketplace

- **Standard RIs only** can be listed and sold to third parties (Convertible RIs cannot be sold).
- Buyers purchase **the remaining term** at a price set by the seller (often below AWS price).
- Selling is the exit path when a Standard RI no longer matches anything you run.

### 5.9 How RI discounts are applied (ordering)

- An RI discounts matching On-Demand usage in the account; no restart required.
- **RIs are applied before Savings Plans** (an SP does not apply to usage already covered by an RI).
- With consolidated billing: the benefit is applied **first in the purchasing account**, then to qualifying usage in other accounts.
- Special ordering rule: if another account has an **unused zonal RI** for a specific AZ, it is applied to your usage **before** your own Regional RI — AWS does this to maximise utilisation and lower the total bill.
- **Unused RIs still cost you**: with No Upfront you keep paying the discounted hourly rate; with Partial/All Upfront the money is already spent.
- Expired RIs do not renew; instances continue running at On-Demand.

### 5.10 Quotas and limits

- **Zonal RIs**: default **20 per Availability Zone** (increase via support/quotas).
- **Regional RIs**: default **20 per Region**.
- Savings Plans have **no purchase limit**.

### 5.11 RI vs SP — the current AWS position

- AWS **recommends Savings Plans over Reserved Instances** as the default commitment model: “the easiest and most flexible way to save money”, same maximum discount, commitment measured in $/hour instead of instance configuration.
- RIs still matter in the exam for: **zonal capacity** (a single purchase giving discount + AZ capacity), the **RI Marketplace**, **Convertible exchange**, and because **most other services only offer RIs** (RDS, ElastiCache, Redshift, OpenSearch…).
- Expect questions phrased as “a company bought RIs three years ago and wants to modernise” → migrate to Savings Plans.

---

## 6. Spot Instances

### 6.1 Definition and mechanics

- Spot uses **spare EC2 capacity**; the hourly price is set by AWS and adjusted gradually based on long-term supply and demand (**no bidding model** — you cannot pay more to keep an instance).
- A **Spot capacity pool** = unused instances with the same **instance type + Availability Zone**.
- Maximum discount: **up to 90 %** vs On-Demand.
- No commitment, no capacity guarantee, and **no guarantee of getting the capacity or keeping it**.

### 6.2 Interruption

- AWS gives a **2-minute Spot Instance interruption notice**, then the instance is **terminated by default** (or **stopped** / **hibernated** if that behaviour was configured at launch).
- **EC2 instance rebalance recommendation**: an earlier signal that a Spot instance is at elevated risk, letting you move work *before* the 2-minute notice. Use **Capacity Rebalancing** in Auto Scaling groups / EC2 Fleet to react automatically.
- Capture both signals with an **EventBridge rule** and trigger checkpointing or graceful shutdown.
- **Persistent Spot requests** are automatically resubmitted after interruption; **one-time** requests are not.

### 6.3 How to request Spot (current best practice)

| API | Use it? | Notes |
|---|---|---|
| `CreateAutoScalingGroup` | ✓ yes | preferred when you want AWS to manage the instance lifecycle and maintain target capacity |
| `CreateFleet` (`instant` type) | ✓ yes | preferred when you manage the lifecycle yourself or don’t need auto scaling; supports mixed instance types |
| `RunInstances` | acceptable for simple cases | cannot mix instance types in one request |
| `RequestSpotFleet`, `RequestSpotInstances` | ✗ legacy | strongly discouraged, no planned investment |

### 6.4 Allocation strategies (Auto Scaling groups / EC2 Fleet)

| Strategy | Behaviour | When |
|---|---|---|
| `lowest-price` | cheapest pools first | cost-first, interruption-tolerant, short jobs |
| `capacity-optimized` | picks pools with the **most** spare capacity | workloads where interruption is expensive (long-running batch, HPC) |
| `price-capacity-optimized` | most available pools **that are also cheap**; **AWS-recommended default** | general best balance of cost and interruption risk |
| `diversified` | spreads across all pools | maximum aggregate capacity over time |

Notes: EC2 Fleet defaults to `lowest-price` per unit; you can override. Spot Fleet (legacy) uses `spread`/`lowest-price`/`capacity-optimized` — but `price-capacity-optimized` is the modern recommendation.

### 6.5 Best practices (the checklist the exam likes)

1. **Be flexible** — rule of thumb: **at least 10 instance types** per workload, and **all AZs** of the Region.
2. Use **attribute-based instance type selection** (vCPU/memory attributes) so new instance types are used automatically.
3. Use **Spot placement score** (1–10) to pick Region/AZ before launching — a point-in-time recommendation, not a guarantee.
4. Run inside an **Auto Scaling group or EC2 Fleet** and think in **aggregate capacity** (vCPU/memory units), not individual instances.
5. **Diversify** instance types and AZs; use **price-capacity-optimized**.
6. Make the app **fault-tolerant**: checkpointing, idempotent work, SQS/queue decoupling, graceful shutdown on the 2-minute notice.
7. Keep a **stable baseline** on On-Demand or a Savings Plan; add Spot on top for acceleration/cost.
8. Use **managed integrations**: Amazon EMR, AWS Batch, EKS (managed node groups/Karpenter), ECS, Elastic Beanstalk, SageMaker AI, GameLift.

### 6.6 Where Spot fits

Stateless web tiers behind a load balancer, batch processing, CI/CD runners, big data/EMR, HPC, rendering, containerised workloads (ECS/EKS), data analytics, ML training that can checkpoint, test environments.

### 6.7 Where Spot does **not** fit (anti-patterns)

- Stateful databases or anything with in-memory state you cannot lose.
- Tightly coupled HPC that cannot tolerate losing a node.
- Workloads whose **entire** target capacity must be available at all times.
- Anything where a licence is tied to a specific instance/uptime.
- **Failover-to-On-Demand designs** — AWS explicitly discourages trying to replace interrupted Spot with On-Demand on the fly; it disturbs your own Spot capacity and may not succeed.
- Using Spot as the *only* capacity for a 24/7 business-critical service.

### 6.8 Spot billing and discounts

- You pay the **Spot price** while running; savings up to 90 %.
- **Savings Plans do not apply to Spot** and Spot spend does not count toward SP commitment.
- RIs do not apply to Spot.
- Interrupted-instance billing depends on platform and who interrupted (partial/full hour) — do not rely on a general “you never pay for the last hour” rule.

---

## 7. On-Demand Capacity Reservations (ODCR)

### 7.1 Definition

A Capacity Reservation reserves EC2 capacity in a **specific Availability Zone**, for **any duration**, for instances with matching **instance type, platform, AZ and tenancy**.

- **No discount.** Billed at the **equivalent On-Demand rate whether you use it or not** (including any regional surcharge for Dedicated Instances). Per-second billing with a 60-second minimum.
- Billing starts when the reservation is **provisioned**; unused capacity appears on the bill as **“unused reservation”**.
- Cancel an **immediate-use** CR at any time to stop incurring charges.

### 7.2 Billing example (the one to remember)

Create a CR for **20 × m4.large Linux** in one AZ, run **15** matching instances:

- You are billed for **15 running instances** + **5 unused reservation hours** — all at the On-Demand rate.
- When an instance runs *inside* the CR, there is **no separate charge for the reservation itself** — you just pay for the instance.
- If a **Compute SP** or **Regional RI** matches, it discounts the instance usage and then the unused CR hours (used-first preference).

### 7.3 Immediate-use vs future-dated

| | **Immediate-use CR** | **Future-dated CR** |
|---|---|---|
| Start | now | a chosen future date |
| Commitment | none; modify/cancel anytime | **commitment duration** (minimum **14 days**); cannot decrease instance count or duration during it |
| Cancellation | free | may incur a **cancellation charge** |
| Use case | cover current workloads, DR, sudden demand | planned seasonal surges, DR tests, compliance windows |
| Constraints | — | minimum **32 vCPUs**; families **C, G, I, M, R, T, U, X**; can **postpone** the start date before delivery; **for incremental instances, not to cover already-running instances** |

### 7.4 Targeted vs open

- By default a CR **automatically matches** any instance with matching attributes — including instances launched **on your behalf** by managed services (Auto Scaling, ECS, EKS, EMR, Batch, Elastic Beanstalk, ParallelCluster, PCS). This is called an **open** reservation.
- You can **target** a reservation to specific workloads/instance launches, or enforce that instances run **only** in a CR/CR group.
- For Auto Scaling, the launch template must reference the reservation (or CR group) — otherwise the ASG will not use it.

### 7.5 Sharing and cross-account behaviour

- A CR is **not transferable** between accounts, but can be **shared via AWS Resource Access Manager (RAM)**.
- **Zonal RI capacity cannot be shared across accounts** — if multiple accounts need guaranteed AZ capacity, use a shared CR.
- CRs support **Capacity Reservation Fleets**, **CR groups**, **move** and **split** operations, and work in **cluster placement groups** only (spread/partition not supported), plus Local Zones, Wavelength and Outposts.

### 7.6 Limits to remember

- Active and unused CRs **count against your On-Demand Instance quota**.
- **Cannot** be used with **Dedicated Hosts** (but **can** be used with Dedicated Instances).
- **Windows BYOL not supported**; **RHEL BYOL is supported**.
- A CR does not guarantee that a **hibernated** instance can resume.
- AWS now also offers **interruptible Capacity Reservations** (a newer, cheaper CR variant that can be reclaimed) — mention it only if a question is explicitly about that trade-off.

### 7.7 Discount layering (exam-critical)

| Discount vehicle | Applies to a Capacity Reservation? |
|---|---|
| **Compute Savings Plan** | **yes** |
| **EC2 Instance Savings Plan** | yes (if family/Region match) |
| **Regional RI** | **yes** |
| **Zonal RI** | **no** |
| Spot | n/a (different capacity model) |

### 7.8 When to use a CR

- Business-critical workloads needing **capacity assurance** in a specific AZ.
- Disaster recovery / pilot-light / warm-standby capacity must exist *before* failover.
- Seasonal or event-driven capacity planning (planned surges).
- Compliance/regulatory “guaranteed capacity” requirements.
- Licence constraints tied to specific instance type/AZ.
- Cross-account guaranteed capacity (shared via RAM).

### 7.9 When **not** to use a CR

- You only want a discount → SP or RI.
- Capacity is needed for an existing running fleet → use **immediate-use**, not future-dated.
- Batch/elastic demand with tolerance for interruption → Spot.
- GPU training for a few days → Capacity Blocks for ML (usually cheaper than holding a CR 24/7).

---

## 8. Capacity Blocks for ML

- **What:** reserve **GPU/accelerator capacity on a future date** for a bounded window; instances are placed in EC2 UltraClusters for low-latency networking.
- **Why:** ML training/fine-tuning/experiments need GPUs for **days or weeks**, and you don’t want to pay for a reservation while the GPUs sit idle.
- **Window:** start time up to **8 weeks** ahead; durations from **1 day up to 14 days** in 1-day increments, or **7-day increments up to 6 months**; offerings can start as soon as **~30 minutes** ahead.
- **Size:** up to **64 instances per block**; up to **256 instances per Organisation** per date; available GPU types include P4/P5/P6 and Trainium (Trn1/Trn2), region-dependent.
- **Rules:** **no cancellation**; must **target the reservation ID**; **no placement groups**; ends at **11:30 UTC** (termination process begins 11:00 UTC on the final day); default tenancy only; platforms: Linux/Unix, RHEL, RHEL with HA, SUSE, Ubuntu Pro.
- **Sharing:** instance Capacity Blocks can be shared via RAM within the same Organisation; **UltraServer Capacity Blocks cannot be shared**.
- **Pricing:** based on supply and demand, updated periodically — not the On-Demand rate.
- **Exam trigger:** “GPU instances for a training job next month”, “we don’t want to pay for reserved capacity when not in use”, “temporary accelerator capacity”.

---

## 9. Dedicated Hosts, Dedicated Instances and tenancy

This is the other “capacity/discount” family the exam tests, usually via **licensing**.

| | **Dedicated Host** | **Dedicated Instance** |
|---|---|---|
| Physical server | fully dedicated to you | dedicated to a single customer account |
| Billing | **per host** | **per instance** |
| Sockets/cores/host ID visibility | ✓ | ✗ |
| Host & instance affinity / targeted placement | ✓ | ✗ |
| BYOL (per-socket, per-core, per-VM) | ✓ full support (Windows Server, SQL Server, SUSE, RHEL…) | partial (SQL Server via License Mobility + Software Assurance, Windows VDA) |
| Automatic instance recovery | ✓ | ✓ |
| Capacity Reservations | ✗ | ✓ |
| Cross-account capacity sharing | ✓ (RAM) | ✗ |

Key Dedicated Host facts:
- **Dedicated Host Reservations** give up to **70 %** off On-Demand Dedicated Host pricing; 1-year or 3-year terms, All/Partial/No upfront; **you must have active Dedicated Hosts allocated before you can buy the reservation**.
- **Host resource groups** (with AWS License Manager) automate host allocation and instance placement for BYOL; Auto Scaling groups can use a launch template that specifies a host resource group.
- **Host affinity** keeps instances on the same host over stop/start; **auto-placement** chooses a host for you.
- Limitations: **RDS not supported on Dedicated Hosts**; hosts cannot be launched into placement groups; RHEL/SUSE on Dedicated Hosts requires **your own AMIs** (except high-memory u-series); quotas are per instance family per Region.
- **Savings Plans do not apply** to Dedicated Host charges — the DH reservation is the discount mechanism.
- RIs *can* be bought with **dedicated tenancy** (for Dedicated Instances), but those RIs do **not** get instance size flexibility (and the discount model is different from a full host).

**Exam trigger:** “per-socket SQL Server licence”, “we must control physical cores”, “compliance requires single-tenant hardware with host visibility” → **Dedicated Hosts**. “Single-tenant hardware but no host-level visibility needed” → Dedicated Instances.

---

## 10. Master comparison matrix

| Dimension | On-Demand | Compute SP | EC2 Instance SP | Standard RI | Convertible RI | Spot | Capacity Reservation | Capacity Block |
|---|---|---|---|---|---|---|---|---|
| Max discount | 0 % | 66 % | 72 % | 72 % | 66 % | 90 % | 0 % | market |
| Commitment | none | $/hr 1–3 yr | $/hr 1–3 yr | config 1–3 yr | config 1–3 yr | none | none (future: ≥14 d) | fixed window |
| Measured in | hours | $/hour | $/hour | instances | instances | capacity | instances | instances |
| Capacity guarantee | ✗ | ✗ | ✗ | **zonal only** | **zonal only** | ✗ | **✓** | **✓** |
| Interruptible | ✗ | ✗ | ✗ | ✗ | ✗ | **✓** | ✗ (interruptible variant) | ✗ |
| Flexible across family | ✓ | **✓** | ✗ | ✗ (size only) | exchange | ✓ | ✗ | ✗ |
| Flexible across Region | ✓ | **✓** | ✗ | ✗ | ✗ | ✓ | ✗ | ✗ |
| Flexible across OS/tenancy | ✓ | **✓** | ✓ | ✗ | exchange | ✓ | ✗ | ✗ |
| Size flexibility | ✓ | ✓ | ✓ | regional only | regional only | ✓ | ✗ | ✗ |
| Covers Fargate / Lambda | — | **✓ / ✓** | ✗ / ✗ | ✗ | ✗ | — | ✗ | ✗ |
| Applies to a CR | — | ✓ | ✓ | regional RI only | regional RI only | — | — | — |
| Cancellable | ✓ | 7-day return* | 7-day return* | ✗ (sell/modify) | ✗ (exchange/modify) | ✓ | immediate CR ✓ | ✗ |
| Auto-renew | — | ✗ | ✗ | ✗ | ✗ | — | n/a | n/a |
| Shareable across accounts | — | ✓ (default) | ✓ (default) | discount ✓ / capacity ✗ | discount ✓ / capacity ✗ | — | ✓ (RAM) | instance CB only |
| Billing granularity | 1 s (60 s min) | hourly commitment | hourly commitment | hourly | hourly | per second* | 1 s (60 s min) | per block |
| Best for | unpredictable, new | mixed modern estate | stable EC2 family | stable EC2 config | config likely to change | batch, CI/CD, fault-tolerant | compliance, DR, guarantee | GPU ML windows |

\* Savings Plans: within 7 days, commitment ≤ $100/hr. Spot: interrupted-instance billing varies by platform/who interrupted.

---

## 11. Cost math and worked examples

> All figures below are **illustrative** — the percentages are assumptions, not AWS price lists. The point is the method and the ordering of magnitude.

### 11.1 Baseline: one always-on instance

Assume an instance with a **$0.100/hour** On-Demand rate, running 8,760 h/year → **$876/year**.

| Option | Assumed discount | Effective $/h | Annual cost | Notes |
|---|---|---|---|---|
| On-Demand | 0 % | 0.100 | **$876** | reference |
| 1-yr Compute SP, no upfront | ~33 % | 0.067 | ~$587 | monthly, flexible |
| 1-yr EC2 Instance SP, all upfront | ~40 % | 0.060 | ~$526 (paid upfront) | family-locked |
| 3-yr EC2 Instance SP, all upfront | ~62 % | 0.038 | ~$333/yr equivalent | 3-yr lock-in |
| 3-yr Standard RI, all upfront | ~62 % | 0.038 | ~$333/yr equivalent | same economics, weaker reporting |
| Spot (interruptible) | 70–90 % | 0.010–0.030 | $88–$263 | may be reclaimed |
| Capacity Reservation | 0 % | 0.100 | **$876 even if idle** | discount only if SP/regional RI layered |

Ordering to remember: **Spot > 3-yr EC2 Instance SP ≈ 3-yr Standard RI > 1-yr SP/RI > On-Demand ≈ Capacity Reservation (before layering)**.

### 11.2 Portfolio example — the answer AWS expects in a design question

100 instance-equivalents × 8,760 h × $0.10/h → **$87,600/year all On-Demand**.

Architecture:

- **60** steady baseline instances → **3-yr EC2 Instance SP** (all upfront, ~62 % off)
- **25** variable/burst instances → **On-Demand**
- **15** fault-tolerant batch instances → **Spot** (~70 % off)

| Component | Computation | Annual cost |
|---|---|---|
| Baseline (SP) | 60 × 8,760 × $0.038 | ~$19,973 |
| Burst (On-Demand) | 25 × 8,760 × $0.100 | ~$21,900 |
| Batch (Spot) | 15 × 8,760 × $0.030 | ~$3,942 |
| **Total** | | **~$45,815** |

→ **~48 % lower** than all-On-Demand, with capacity risk confined to the batch tier.

### 11.3 Payment options — how to reason (not memorise)

| Payment | Upfront cash | Monthly bill | Discount | Cash-flow notes |
|---|---|---|---|---|
| **All upfront** | highest | none (for the commitment) | best | best discount; cash locked; not refundable |
| **Partial upfront** | some | reduced hourly | middle | balanced |
| **No upfront** | none | full discounted hourly | smallest | easiest to approve; needs billing history (RI No Upfront requires it); overcommitment risk is the same |

Rule of thumb: the difference between payment options is typically a few percentage points of discount — **the bigger decision is term length and commitment size**, not the payment option.

### 11.4 Break-even and overcommitment

- With a No-Upfront SP of $C/hour, your bill for eligible compute is effectively `max(discounted usage, C)`. If usage is below `C`, the excess is waste.
- Break-even for a 1-yr No-Upfront SP is roughly **the committed spend divided by the discount** — if you cannot confidently forecast that much usage for the whole term, commit less.
- AWS practice: start with **conservative coverage (~70 %)** and increase; track **utilisation > 95 %**.

### 11.5 Common financial mistakes

1. Committing to the **peak** instead of the **floor** of usage.
2. Buying 3-yr SP/RI for a workload that is about to be re-platformed (containers → Fargate, EC2 → Lambda).
3. Discounting Spot in your head and then budgeting as if Spot were guaranteed.
4. Leaving a Capacity Reservation idle with no SP/Regional RI covering it (paying full On-Demand for zero throughput).
5. Forgetting RI/SP expiry — the discount silently disappears and the bill jumps.
6. Buying RIs when the same discount with more flexibility is available as a Compute SP.
7. Buying an SP for a Region where the workload will not run (EC2 Instance SPs are Region-bound).
8. Not enabling **cost allocation tags** / **cost categories**, so you cannot attribute commitments to teams.
9. Assuming a zonal RI can cover shared capacity across accounts (it cannot) or cover a Capacity Reservation (it does not).
10. Refreshing the bill after a reservation expires and blaming the workload.

---

## 12. The portfolio playbook (what a good SAP-C02 answer looks like)

### 12.1 Reference architecture

```
            ┌──────────────────────────────────────────────┐
            │  1. Right-size first (Compute Optimizer)     │
            │     — no purchasing option fixes waste       │
            └──────────────────────────────────────────────┘
                                │
   ┌────────────────────────────┼────────────────────────────┐
   │                            │                            │
Steady baseline            Variable / burst            Fault-tolerant batch
   │                            │                            │
3-yr Compute SP            On-Demand                    Spot
(or EC2 Instance SP)       (grow commitment            (price-capacity-optimized,
   │                        as pattern solidifies)      diversified, checkpointed)
   │                            │                            │
Capacity must be guaranteed in an AZ?
   └─ yes → Capacity Reservation + (SP or Regional RI discount)
            GPU window → Capacity Blocks for ML
```

### 12.2 The seven-step method

1. **Right-size and modernise** (Compute Optimizer, Cost Explorer) — commitments on oversized instances multiply waste.
2. **Separate usage tiers**: steady floor / variable / interruptible / must-be-guaranteed.
3. **Cover the floor** with a 1-yr or 3-yr SP (Compute SP by default; EC2 Instance SP if family and Region are frozen).
4. **Keep variable capacity On-Demand**; revisit after 2–3 months of data.
5. **Move tolerant workloads to Spot**, diversified, with a stable baseline and interruption handling.
6. **Buy guaranteed capacity only where a constraint demands it** (CR), and discount it with SP/Regional RI.
7. **Monitor coverage/utilisation monthly**, adjust commitments at renewal, and re-run the analysis after any re-platforming.

### 12.3 RI → Savings Plans migration (classic SAP-C02 scenario)

- Inventory RIs: term, expiry, platform, family, util. Under-utilised RIs are candidates for **marketplace sale** (Standard) or **exchange** (Convertible).
- Do **not** buy new RIs for EC2 when a Compute SP covers the same usage more flexibly.
- As RIs expire, replace coverage with an SP sized from **post-expiry** usage (not current usage, or you double-cover).
- Keep existing RIs running to term; don't stack SP coverage on top of RI-covered usage (SP only sees the uncovered remainder).
- Convertible RIs can be **exchanged** rather than abandoned.
- Recompute coverage after each step; aim for utilisation > 95 %.

---

## 13. Decision checklist (run this on every scenario question)

1. **Is capacity guaranteed?** → CR / Capacity Block / zonal RI. Otherwise SP/RI/Spot.
2. **Can it be interrupted?** → Spot.
3. **Is usage predictable for a full term?** → no: On-Demand.
4. **Is the configuration frozen?** → EC2 Instance SP / Standard RI; else Compute SP / Convertible RI.
5. **Does it involve Fargate or Lambda?** → only Compute SP.
6. **Is it a non-EC2 service?** → service-specific reservations (RDS/ElastiCache/Redshift/OpenSearch/DynamoDB reserved capacity).
7. **Is there per-socket/per-core licensing?** → Dedicated Hosts (+ host reservation).
8. **Is it a short GPU/ML window?** → Capacity Blocks for ML.
9. **Multi-account?** → share via consolidated billing (SP/RI discount) or RAM (CR capacity).
10. **Is the requirement “simplest” or “least operational overhead”?** → Savings Plans (single commitment, no exchanges, no AZ decisions).

### 13.1 Scenario → answer map

| Scenario keywords | Answer |
|---|---|
| steady EC2 + Fargate + Lambda, multi-Region, will modernise | Compute Savings Plan, 3 yr |
| same instance family/Region for 3 years, max discount | EC2 Instance Savings Plan or Standard RI (3 yr, all upfront) |
| guarantees capacity in one AZ, 2 years | Capacity Reservation (SP/Regional RI for the discount) |
| single purchase that discounts *and* reserves capacity in an AZ | Zonal Reserved Instance |
| nightly batch, 500 instances, interruption OK | Spot + ASG/EC2 Fleet, price-capacity-optimized, diversified |
| Spot getting interrupted constantly | more instance types (≥10), multiple AZs, capacity rebalancing, capacity-optimized |
| compliance/DR needs capacity *today* | Immediate-use Capacity Reservation |
| capacity needed for a known future date, no current usage | Future-dated Capacity Reservation |
| GPU training for 5 days next month | Capacity Blocks for ML |
| per-socket SQL Server BYOL | Dedicated Hosts + Dedicated Host Reservation |
| single-tenant hardware, no visibility needs | Dedicated Instances (CR supported) |
| RDS/ElastiCache/Redshift cost reduction | service-specific Reserved Instances (SP not eligible) |
| unused 3-yr Standard RIs after migration | sell on RI Marketplace; modify within family; do not expect cancellation |
| commitment too large, utilisation 40 % | cannot cancel; return within 7 days if ≤$100/hr; right-size next purchase |
| multi-account guaranteed capacity | CR shared via RAM |
| 24/7 stateful database | RI/SP — never Spot |

---

## 14. Adjacent services and tooling the exam expects you to know

### 14.1 Cost analysis and governance

| Tool | Role in this topic |
|---|---|
| **AWS Cost Explorer** | RI/SP **recommendations**, **coverage**, **utilisation**, forecasting, cost by tag/account/Region; the default place to buy/return SPs and see RI recommendations |
| **AWS Budgets** | alerts on cost/usage/RI/SP utilisation; **budget actions** (e.g. apply an IAM policy, stop instances, SCP) |
| **AWS Cost Anomaly Detection** | ML-based spend anomaly alerts (a misconfigured Spot fleet or forgotten CR shows up here) |
| **CUR / CUR 2.0** | line-item detail with `savingsPlan/*` and `reservation/*` columns for chargeback; see [AWS_CUR_CostExplorer_Budgets_SAP-C02.md](AWS_CUR_CostExplorer_Budgets_SAP-C02.md) |
| **AWS Compute Optimizer** | rightsizing + idle-resource recommendations (EC2, ASG, EBS, Lambda, ECS/Fargate) — the step *before* any commitment |
| **AWS Trusted Advisor** | cost-optimisation checks (idle resources, underutilised RIs) |
| **Cost Optimization Hub** | aggregated, deduplicated optimisation recommendations across accounts/Regions, with effort/impact filters |
| **Cost allocation tags / Cost Categories / Billing Conductor** | attribute and show back commitments & discounts to teams (chargeback/showback) |
| **AWS Organizations / consolidated billing** | sharing of RI/SP discounts, volume pricing, SCP guardrails (e.g. deny expensive instance families, enforce tags) |
| **Well-Architected — Cost Optimization pillar** | cloud financial management, expenditure awareness, cost-effective resources, matching supply to demand, optimising over time |

### 14.2 Non-EC2 reservations — the differentiator

Savings Plans cover **only EC2, Fargate and Lambda** (plus SageMaker via its own plan). For other services the reservation model is per-service — for example:

- **RDS / Aurora Reserved Instances** (1 or 3 yr, All/Partial/No upfront)
- **ElastiCache reserved nodes**
- **Redshift reserved nodes**
- **OpenSearch reserved instances**
- **DynamoDB reserved capacity** (provisioned capacity units)
- **MemoryDB / other data services** their own reservations

If a question says “reduce the cost of RDS and ElastiCache” and the answers mention Compute Savings Plans → that answer is wrong.

### 14.3 Spot integrations

Amazon EMR, AWS Batch, Amazon EKS (managed node groups / Karpenter), Amazon ECS, AWS Elastic Beanstalk, Amazon SageMaker AI, Amazon GameLift — use these managed integrations rather than hand-rolling Spot fleets.

---

## 15. Exam traps and distractors (the “wrong answers” AWS plants)

1. **“Capacity Reservations provide a discount.”** No — they are billed at On-Demand rates; the discount comes from SP/Regional RI layered on top.
2. **“Savings Plans reserve capacity.”** No.
3. **“Zonal RI discounts apply to Capacity Reservations.”** No — only Regional RIs and SPs do.
4. **“Savings Plans apply to Spot.”** No; Spot spend doesn’t even consume the commitment.
5. **“Compute SP has a Region limit.”** No — it applies across Regions, OS, tenancy and families.
6. **“EC2 Instance SP covers Fargate/Lambda.”** No.
7. **“RIs auto-renew.”** No.
8. **“You can cancel an RI.”** No — modify, exchange (Convertible) or sell (Standard) only.
9. **“Standard RIs can be exchanged.”** No — only modified/sold.
10. **“Convertible RIs can be sold on the Marketplace.”** No — only Standard.
11. **“Increase the Spot bid to avoid interruption.”** There is no bidding; the fix is diversification/allocation strategy.
12. **“Spot interruption gives 30 s.”** It is **2 minutes**.
13. **“Fail over to On-Demand when Spot is interrupted.”** AWS discourages this as a strategy.
14. **“Buy a future-dated CR to cover today’s instances.”** Future-dated CRs are for incremental instances; use immediate-use.
15. **“A zonal RI’s capacity can be shared with other accounts.”** No — only the discount; use a shared CR.
16. **“Dedicated Hosts can be discounted with a Savings Plan.”** No — DH reservations are the mechanism (up to 70 %).
17. **“Spot is fine for the database tier.”** Never for stateful/tightly-coupled workloads.
18. **“Instance size flexibility works for Windows RIs.”** No — Linux/UNIX shared tenancy only.
19. **“Buy 3-year commitments on day one of a migration.”** Right-size and observe first; overcommitment is unrecoverable.
20. **“SP/RI forgets the discount at expiry.”** It does — and the bill jumps; plan renewals.
21. **“A $100/hr SP can be returned after 30 days.”** Return window is **7 days**, and only if commitment ≤ **$100/hr**.
22. **“Savings Plans can be modified.”** No — purchase a new plan instead (queued purchases are possible).
23. **“Capacity Reservation gives unlimited capacity.”** It is limited by your **On-Demand quota** (and by instance count reserved).
24. **“GPU capacity for a week is best bought as a 1-year CR.”** Capacity Blocks for ML exist precisely to avoid 24/7 reservation cost.

---

## 16. Practice questions (SAP-C02 style)

> Try to answer before reading §17. Each question has one best answer.

**Q1.** A company runs 300 instances across three Regions, plus Fargate tasks and Lambda functions. The architecture will be modernised over the next 24 months and instance families will change. The CFO wants the simplest possible commitment model with the largest discount that does not limit flexibility. What should you recommend?
A. Standard RIs, 3 years, all upfront
B. Compute Savings Plan, 3 years
C. EC2 Instance Savings Plan, 3 years
D. On-Demand with Savings Plans recommendations disabled

**Q2.** A trading platform must guarantee that 40 `c5.2xlarge` instances can run in `eu-west-1a` for the next two years. The company already owns a Compute Savings Plan covering that usage. What is the correct combination?
A. 40 zonal RIs
B. 40 regional RIs
C. An On-Demand Capacity Reservation for 40 instances in `eu-west-1a`; the Savings Plan discounts the usage
D. 40 EC2 Instance Savings Plans

**Q3.** Which single purchase both lowers the price of EC2 usage and guarantees capacity in a specific Availability Zone?
A. Compute Savings Plan
B. Regional Reserved Instance
C. Zonal Reserved Instance
D. Spot Fleet

**Q4.** A nightly batch job runs 6 hours on 500 instances and can restart from checkpoints. Interruptions doubled last month after the job was pinned to a single instance type in one AZ. What is the *most effective* change?
A. Buy 3-year Standard RIs for the instance type
B. Use Spot with at least 10 instance types across all AZs, `price-capacity-optimized`, and Capacity Rebalancing
C. Switch to On-Demand and use Savings Plans
D. Increase the Spot bid price

**Q5.** A company owns ten regional `m5.large` Linux RIs (shared tenancy) in `us-east-1`. It now runs `m5.xlarge` instances. How many `m5.xlarge` instances are fully covered?
A. 2
B. 5
C. 10
D. 20

**Q6.** An engineer owns one `m5.2xlarge` regional Linux RI in `us-east-1a` and now needs four `m5.large` instances in `us-east-1b`. What is possible?
A. Nothing; RIs cannot be modified
B. Modify the RI: change AZ to `us-east-1b` and split the reservation into four `m5.large` (same family/generation, footprint 16 = 4 × 4)
C. Exchange it for a `c5.large` RI
D. Cancel the RI and buy a new one

**Q7.** A company expects to move from `m5` to `m6i` and from Linux to Windows within 18 months, and wants a committed discount meanwhile. What is the best fit?
A. Standard RI, 3 years
B. Convertible RI (or a Compute Savings Plan), because the configuration will change
C. Zonal RI
D. Spot

**Q8.** A company bought 3-year Standard RIs for `m5` in `eu-west-1`, then migrated everything to `m6i` and no longer runs `m5`. What are the valid options?
A. Cancel the RIs
B. Convert the Standard RIs to Convertible RIs
C. Sell the Standard RIs on the Reserved Instance Marketplace, and/or modify them within the same family/generation
D. Exchange them for `m6i` RIs

**Q9.** A team wants to reduce the cost of Amazon RDS and ElastiCache. Which statement is correct?
A. A Compute Savings Plan covers both
B. An EC2 Instance Savings Plan covers both
C. Each service has its own Reserved Instance/reserved-node model; Savings Plans do not cover them
D. Only Spot can reduce their cost

**Q10.** A company reserves 20 `m4.large` Linux instances in a Capacity Reservation and runs 15 matching instances. There is no SP or RI. What is billed?
A. 15 instances only
B. 15 instances plus 5 unused reservation hours, all at the On-Demand rate
C. 20 instances at On-Demand, but the 5 unused are free
D. Nothing until the instances are launched

**Q11.** A Capacity Reservation is in place and the company also owns zonal RIs for the same instances. Which is true?
A. The zonal RI discounts the Capacity Reservation
B. The zonal RI discount does not apply to Capacity Reservations; a Regional RI or Savings Plan would
C. The CR is free because of the zonal RI
D. Zonal RIs and CRs cannot be used together at all

**Q12.** A company has run the same workload for six months and wants guaranteed capacity **starting today** in one AZ. What should it buy?
A. A future-dated Capacity Reservation
B. An immediate-use Capacity Reservation
C. A Capacity Block for ML
D. A zonal RI purchased from the Marketplace

**Q13.** An Auto Scaling group mixes On-Demand and Spot capacity. Which configuration is recommended for a workload with a high cost of interruption?
A. `lowest-price` only, one instance type, one AZ
B. `capacity-optimized` (or `price-capacity-optimized`), many instance types, all AZs, On-Demand base capacity
C. 100 % Spot with no baseline
D. One persistent Spot request per instance

**Q14.** A company runs a 24/7 stateful relational database on EC2 with a strict RTO. Which purchasing option is **least** appropriate?
A. On-Demand
B. Reserved Instance
C. Savings Plan
D. Spot

**Q15.** An enterprise must use its existing per-socket SQL Server licences and requires visibility into physical cores. Which option fits?
A. Dedicated Instances
B. Dedicated Hosts with a host resource group and Dedicated Host Reservation
C. Compute Savings Plan
D. Spot Instances

**Q16.** A company has a Savings Plan commitment of $10/hour but only $6/hour of eligible usage. What happens, and what can it do?
A. AWS refunds the difference automatically
B. It is billed the full $10/hour commitment; the plan cannot be cancelled; it can be returned within 7 days if the commitment is ≤ $100/hour, and future purchases should be right-sized from Cost Explorer recommendations
C. It can modify the plan down to $6/hour
D. The unused $4/hour rolls over to next month

**Q17.** A company’s EC2 usage consists only of AWS Fargate tasks and Lambda functions in multiple Regions. Which commitment product applies?
A. EC2 Instance Savings Plan
B. Compute Savings Plan
C. Standard Reserved Instance
D. Zonal Reserved Instance

**Q18.** A data team runs a fault-tolerant nightly EMR job that must finish before 07:00. Budget is the top priority. Which is the best design?
A. All On-Demand
B. Spot-only, with the whole cluster on Spot
C. On-Demand or Savings Plan baseline equal to the time-critical capacity, plus Spot to accelerate — diversified across instance types/AZs
D. 3-year Standard RIs for the entire transient cluster

**Q19.** An ML team needs 32 `p5.48xlarge` instances for five days in six weeks’ time. What is the most cost-effective, appropriate purchase?
A. A 1-year regional RI
B. Capacity Blocks for ML
C. On-Demand with a Compute Savings Plan
D. A 12-month Dedicated Host

**Q20.** Four accounts in one AWS Organisation need guaranteed capacity in the same AZ, and the company wants one purchase. What should you build?
A. One zonal RI in each account
B. One Capacity Reservation shared across accounts via AWS Resource Access Manager (and an SP or Regional RI for the discount)
C. One Standard RI in the management account
D. Four future-dated Capacity Reservations

---

## 17. Answer key with explanations

**A1 — B.** Compute SP is the only option that covers EC2 (any family/size/OS/Region) + Fargate + Lambda and is the simplest commitment. C is more restrictive (family + Region); A locks the configuration for 3 years.

**A2 — C.** Guaranteed AZ capacity = Capacity Reservation. The SP provides the discount, including on unused CR hours. Zonal RIs (A) would reserve capacity but only for the owning account and would not combine with the existing SP (SP doesn't apply to RI-covered usage). Regional RIs (B) give no capacity.

**A3 — C.** A zonal RI is the only *single* option with both a discount and an AZ capacity reservation. (A/B give no capacity; D is not a purchase model.)

**A4 — B.** The problem is concentration in one pool and lack of interruption handling. Diversifying ≥10 types across AZs + `price-capacity-optimized` + Capacity Rebalancing addresses the cause. There is no bidding (D); RIs (A) are irrelevant for Spot; On-Demand (C) abandons the 90 % savings.

**A5 — B.** Ten `m5.large` = 40 units; each `m5.xlarge` = 8 units → 5 fully covered.

**A6 — B.** Modification is allowed for Linux (AZ change + size change within family/generation) provided the footprint matches: 16 units (`m5.2xlarge`) = 4 × 4 units (`m5.large`), and split/merge is supported.

**A7 — B.** The configuration will change, so Standard RI or zonal RI would be wasted. Convertible RI can be exchanged; Compute SP needs no exchange at all and is the simpler, AWS-recommended option.

**A8 — C.** Standard RIs cannot be cancelled or converted to Convertible, but can be **sold** on the RI Marketplace and **modified** within the same family/generation (e.g. a different size in `m5`). If a match is impossible, the RI simply runs its term (or sells).

**A9 — C.** Savings Plans cover EC2, Fargate and Lambda only. RDS/ElastiCache/Redshift/OpenSearch have their own reservations.

**A10 — B.** CRs bill the equivalent On-Demand rate for the reserved count whether used or unused: 15 used + 5 unused.

**A11 — B.** Only Regional RIs and Savings Plans discount CR usage; zonal RI discounts do not apply.

**A12 — B.** Future-dated CRs cover incremental future instances, not existing ones. Immediate-use is required to cover today’s fleet.

**A13 — B.** Capacity-optimized / price-capacity-optimized, broad instance-type and AZ diversification, and an On-Demand (or SP) base give both availability and lower cost. A single pool and 100 % Spot are the classic anti-patterns.

**A14 — D.** Spot can be reclaimed with 2 minutes’ notice — unsuitable for a stateful database with a strict RTO.

**A15 — B.** Per-socket BYOL + physical core visibility = Dedicated Hosts. Host resource groups + a Dedicated Host Reservation (up to 70 % off) make it economical.

**A16 — B.** The commitment is billed regardless of usage; SPs cannot be modified or cancelled, only returned within 7 days if ≤ $100/hour. The corrective action is right-sizing the next commitment using Cost Explorer recommendations.

**A17 — B.** Compute SP is the only one covering Fargate and Lambda.

**A18 — C.** Define the deadline, cover the time-critical baseline with On-Demand/SP, and add diversified Spot to accelerate and reduce cost (AWS’s “hybrid Spot without risking deadlines” guidance).

**A19 — B.** Capacity Blocks for ML are designed for short, future-dated GPU windows and avoid paying for a 24/7 reservation.

**A20 — B.** Zonal RI capacity cannot be shared across accounts; a CR can be shared via RAM. The discount then comes from an SP or Regional RI.

---

## 18. Rapid recall — numbers, limits and phrases

| Fact | Value |
|---|---|
| Compute SP max discount | 66 % |
| EC2 Instance SP / Standard RI max discount | 72 % |
| Convertible RI max discount | 66 % |
| Spot max discount | 90 % |
| Dedicated Host Reservation max discount | 70 % |
| SP/RI term options | 1 or 3 years |
| SP return window | 7 days, commitment ≤ $100/hour |
| Spot interruption notice | 2 minutes |
| Spot flexibility rule of thumb | ≥ 10 instance types, all AZs |
| Spot placement score range | 1–10 |
| Default RI quotas | 20 zonal per AZ; 20 regional per Region |
| SP purchase limit | none |
| Billing granularity (On-Demand/CR) | per second, 60-second minimum |
| Future-dated CR minimum commitment | 14 days |
| Future-dated CR minimum size | 32 vCPU |
| Future-dated CR families | C, G, I, M, R, T, U, X |
| Capacity Block lead time | up to 8 weeks ahead |
| Capacity Block max size | 64 instances/block; 256 per Organisation |
| Normalization factors | nano 0.25 → small 1 → large 4 → xlarge 8 → 2xlarge 16 → 4xlarge 32 → 8xlarge 64 → 16xlarge 128 → 24xlarge 192 → 32xlarge 256 |

### Phrases to use in written answers

- “Commit only the **baseline**; buy the floor, not the peak.”
- “Layer a **Compute Savings Plan** on top of the **Capacity Reservation** so the guaranteed capacity is not billed at full On-Demand.”
- “Diversify Spot across **at least ten instance types and all Availability Zones** and use **price-capacity-optimized**.”
- “Standard RIs can be **modified or sold**, Convertible RIs can be **exchanged** — neither can be cancelled.”
- “Savings Plans are the **AWS-recommended** commitment model; RIs remain relevant for zonal capacity and for non-EC2 services.”
- “Coverage ~70–90 %, utilisation > 95 %.”
- “Right-size with **Compute Optimizer** before committing; validate with **Cost Explorer** recommendations.”

---

## 19. Glossary

| Term | Meaning |
|---|---|
| **On-Demand** | pay-as-you-go pricing; no commitment; no capacity guarantee |
| **Savings Plan** | commitment to a $/hour spend on eligible compute for 1–3 years |
| **Compute SP** | flexible SP covering EC2 anywhere (any family/size/OS/tenancy/Region) + Fargate + Lambda |
| **EC2 Instance SP** | SP bound to one instance family in one Region; deeper discount |
| **Reserved Instance** | discount for a specific configuration; regional or zonal; Standard or Convertible |
| **Regional RI** | RI covering any AZ in a Region; size flexibility; no capacity |
| **Zonal RI** | RI in one AZ; reserves capacity; no size flexibility; capacity not shareable |
| **Standard RI** | max discount, modifiable, sellable, not exchangeable |
| **Convertible RI** | lower discount, exchangeable, modifiable, not sellable |
| **Normalization factor** | relative size units used for RI size flexibility/footprint math (small = 1) |
| **Instance size footprint** | instances × normalization factor; must match when modifying RIs |
| **RI Marketplace** | third-party marketplace where Standard RIs can be sold/bought |
| **Spot Instance** | unused EC2 capacity at up to 90 % off; interruptible with 2-minute notice |
| **Spot capacity pool** | unused instances of one type in one AZ |
| **Rebalance recommendation** | early warning that a Spot instance may be interrupted |
| **Interruption notice** | 2-minute warning that Spot capacity will be reclaimed |
| **Allocation strategy** | how a fleet picks pools: lowest-price / capacity-optimized / price-capacity-optimized / diversified |
| **Spot placement score** | 1–10 likelihood of getting requested Spot capacity in a Region/AZ |
| **Capacity Reservation (ODCR)** | reserved capacity in one AZ; billed at On-Demand rate whether used or not |
| **Future-dated CR** | CR scheduled for a future window; commitment + possible cancellation charge |
| **Interruptible CR** | newer CR variant that can be reclaimed, at lower cost |
| **Capacity Block for ML** | time-boxed reserved GPU/accelerator capacity for ML workloads |
| **UltraCluster / UltraServer** | tightly coupled GPU infrastructure used by Capacity Blocks |
| **Dedicated Host** | physical server dedicated to you; per-host billing; BYOL; host affinity |
| **Dedicated Instance** | instance on single-tenant hardware; per-instance billing; CR supported |
| **Host resource group** | License Manager construct that automates BYOL host allocation/placement |
| **BYOL** | bring your own licence (per-socket/per-core/per-VM) |
| **Consolidated billing** | one bill for an AWS Organisation; enables RI/SP discount sharing |
| **Coverage** | % of eligible spend actually covered by a discount |
| **Utilisation** | % of a commitment that is actually consumed |
| **Tenancy** | shared vs dedicated (instance- or host-level) |
| **Insufficient Capacity Error (ICE)** | launch failure when the AZ lacks capacity on On-Demand |

---

## 20. Minimum viable knowledge for exam day

If you remember only five things:

1. **SP = flexible $/hour discount without capacity; RI = configuration discount; zonal RI = discount + AZ capacity; Spot = 90 % but interruptible; CR = On-Demand price for guaranteed capacity.**
2. **Compute SP (66 %) covers EC2 anywhere + Fargate + Lambda; EC2 Instance SP (72 %) is locked to a family + Region.**
3. **Capacity Reservations are not discounts** — always pair with a Compute SP or Regional RI (zonal RIs do not count).
4. **Spot is a supplement, never a baseline** — diversify ≥10 types, all AZs, `price-capacity-optimized`, handle the 2-minute notice.
5. **Reliability first, then discount:** the “cheapest” answer that violates a stated constraint (capacity, interruption tolerance, licensing, RTO) is always wrong in SAP-C02.

---

## 21. Sources (verified November 2026)

- [Choosing a purchasing option for Amazon EC2 — AWS Decision Guide](https://docs.aws.amazon.com/decision-guides/latest/ec2-purchasing-options-aws-how-to-choose/ec2-purchasing-options-aws-how-to-choose.html)
- [What are Savings Plans? — AWS Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html)
- [Compute Savings Plans and Reserved Instances — AWS Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/sp-ris.html)
- [Reserved Instances for Amazon EC2 overview — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-reserved-instances.html)
- [How Reserved Instance discounts are applied — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/apply_ri.html)
- [Modify Reserved Instances — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-modifying.html)
- [Spot Instances — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
- [Best practices for Amazon EC2 Spot — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-best-practices.html)
- [Reserve compute capacity with EC2 On-Demand Capacity Reservations — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-reservations.html)
- [Capacity Reservation pricing and billing — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/capacity-reservations-pricing-billing.html)
- [Capacity Blocks for ML — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-blocks.html)
- [Amazon EC2 Dedicated Hosts — Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html)
- [AWS Certified Solutions Architect – Professional exam guide (SAP-C02, PDF)](https://docs.aws.amazon.com/pdfs/aws-certification/latest/solutions-architect-professional-02/solutions-architect-professional-02.pdf)
