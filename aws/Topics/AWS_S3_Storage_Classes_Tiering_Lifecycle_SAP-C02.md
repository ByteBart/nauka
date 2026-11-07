# Amazon S3 storage classes, tiering, lifecycle, encryption and access control
## Study book for AWS Certified Solutions Architect – Professional (SAP-C02)

> **Scope:** the complete S3 surface the exam tests — every storage class and tier, the S3 cost model, archiving and restore mechanics, S3 Lifecycle rules (transitions, expiration, versioning, delete markers, multipart uploads), transition constraints, billing/timing behaviour, server-side encryption headers (SSE-S3, SSE-KMS, DSSE-KMS, SSE-C), access control (public buckets, Block Public Access, presigned URLs, access points, static hosting, logging/audit), monitoring tools, design recipes, exam traps and practice questions.
> **Knowledge state:** November 2026, verified against current AWS documentation (links in §24).
> **Key idea:** There are **two separate mechanisms** that people blur together: the **storage class** (what you *pay per GB*, decided per object) and **S3 Lifecycle** (the *automation that moves or deletes* objects over time). An object's class is a property; lifecycle is the process that changes it. Most exam questions are about choosing the class and then writing the rule that gets you there — correctly and cheaply.
> **No dollar amounts are quoted as facts.** All prices in §14 are **illustrative assumptions** for the maths; verify live prices on the S3 pricing page.

---

## 0. TL;DR — the answer you should be able to give in 30 seconds

| Class / tier | Designed for | Retrieval latency | Min storage duration | Min billable size | Cost posture |
|---|---|---|---|---|---|
| **S3 Standard** | >1 access/month | milliseconds | none | none | baseline |
| **S3 Express One Zone** | latency-critical, single AZ | single-digit ms | none | none | high storage, low request cost |
| **S3 Intelligent-Tiering** | unknown / changing access | ms (auto tiers); optional archive tiers | none | none (but <128 KB never monitored) | storage + per-object monitoring fee |
| **S3 Standard-IA** | <1 access/month, ms needed | milliseconds | **30 days** | **128 KB** | cheaper storage + retrieval fee |
| **S3 One Zone-IA** | recreatable, <1/month | milliseconds | **30 days** | **128 KB** | cheapest ms class, **1 AZ only** |
| **S3 Glacier Instant Retrieval** | ~1 access/quarter, ms needed | milliseconds | **90 days** | **128 KB** | cheapest ms-retrieval archive |
| **S3 Glacier Flexible Retrieval** | 1–2 accesses/year, minutes–hours OK | minutes to 12 h | **90 days** | 40 KB overhead | archived; restore required |
| **S3 Glacier Deep Archive** | <1 access/year, hours OK | 12 h standard / 48 h bulk | **180 days** | 40 KB overhead | cheapest storage in AWS |

**One-liner for the exam:** *Standard for hot data, Intelligent-Tiering when you can't predict access, IA classes for cool data you still need in milliseconds, Glacier Instant Retrieval for quarterly ms access, Glacier Flexible Retrieval for yearly archives with hours-level restore, Deep Archive for compliance retention where 12–48 hours is acceptable — and let Lifecycle rules walk objects down that waterfall while you delete noncurrent versions and abort stuck multipart uploads.*

---

## 1. Exam context — how this topic is tested in SAP-C02

- SAP-C02: **75 questions, 180 minutes, scaled score 750/1000**, multiple choice and multiple response.
- Domains (approx.): **1 — Design Solutions for Organizational Complexity (26 %)**, **2 — Design for New Solutions (29 %)**, **3 — Continuous Improvement for Existing Solutions (25 %)**, **4 — Accelerate Workload Migration and Modernization (20 %)**.
- S3 storage classes and lifecycle appear mostly in:
  - **Domain 3** — “determine cost-optimisation strategies”, “review and improve existing architectures” (the classic “reduce storage cost without losing data” question);
  - **Domain 2** — data lakes, backups, DR, media, analytics pipelines;
  - **Domain 4** — migration of on-premises archives / tape replacement;
  - **Domain 1** — multi-account data platforms, compliance and retention (Object Lock, governance).
- Question styles you will see:
  1. “Which storage class is **most cost-effective** given access pattern X and retrieval time Y?”
  2. “Write/choose the **lifecycle rule** that achieves the retention policy” (often with a trap about noncurrent versions, small objects, or a transition that is not allowed).
  3. “Why is the bill **not** going down after the lifecycle rule was added?”
  4. “Design a **retention/compliance** solution” (Object Lock + lifecycle + Glacier class + restore SLA).
  5. “Fix a **backup/DR** design” with an RTO that rules out Deep Archive.
- The winning answer satisfies **all stated constraints** — durability, availability, retrieval time, retention, immutability, cost. The cheapest class that breaks a constraint is the wrong answer.

Related books in this folder: [AWS_EC2_Purchasing_Options_SAP-C02.md](AWS_EC2_Purchasing_Options_SAP-C02.md) (commitment discounts), [AWS_CUR_CostExplorer_Budgets_SAP-C02.md](AWS_CUR_CostExplorer_Budgets_SAP-C02.md) (cost analysis tooling).

---

## 2. The S3 cost model — what you are actually paying for

Every S3 cost question reduces to **four components plus penalties**:

| Component | What it means | How to reduce it |
|---|---|---|
| **1. Storage** | $/GB-month for the storage class; S3 Standard has volume tiers (price per GB drops as monthly volume grows) | move cold data to cheaper classes |
| **2. Requests** | per 1,000 PUT/GET/LIST/etc.; IA and Glacier classes charge **per request more** than Standard (except Express One Zone, which charges less) | fewer, bigger objects; avoid chatty LIST; aggregate small files |
| **3. Retrieval / data transfer** | IA and Glacier classes charge **per GB retrieved**; internet egress is charged separately in every class | keep hot data in Standard; don't make IA the default for daily-read data |
| **4. Minimums and penalties** | **minimum storage duration** (30/90/180 days), **minimum billable object size** (128 KB for IA/Glacier IR), **40 KB metadata overhead** (Glacier FR/Deep Archive) | don't tier objects you will delete/overwrite soon; don't tier tiny objects |
| **5. Lifecycle/transition requests** | each transitioned object = one request charge | use size filters; aggregate small objects |
| **6. Monitoring fee** | Intelligent-Tiering charges per **object monitored** per month | don't use IT for millions of tiny objects you never access |

**The fundamental trade-off:** cheaper storage per GB always comes with *more expensive or slower access* — retrieval fee, restore delay, minimum duration, or lower durability/availability (One Zone). Tiering is only profitable when **storage savings > transition cost + retrieval cost + minimum-duration penalties**.

---

## 3. Storage classes — the catalogue

### 3.1 Comparison table (the one to memorise)

| Storage class | Code | Designed for | Durability | Availability | AZs | Min duration | Min billable size | Retrieval |
|---|---|---|---|---|---|---|---|---|
| S3 Standard | `STANDARD` | frequent access, >1/month | 11 nines | 99.99 % | ≥3 | none | none | ms |
| S3 Express One Zone | `EXPRESS_ONEZONE` | latency-critical (single AZ) | 11 nines | 99.95 % | **1** | none | none | single-digit ms |
| S3 Intelligent-Tiering | `INTELLIGENT_TIERING` | unknown/changing | 11 nines | 99.9 % | ≥3 | none | none | ms (archive tiers: minutes–hours after restore) |
| S3 Standard-IA | `STANDARD_IA` | <1/month, ms | 11 nines | 99.9 % | ≥3 | 30 days | 128 KB | ms + per-GB fee |
| S3 One Zone-IA | `ONEZONE_IA` | recreatable, <1/month | 11 nines | 99.5 % | **1** | 30 days | 128 KB | ms + per-GB fee |
| S3 Glacier Instant Retrieval | `GLACIER_IR` | ~1/quarter, ms | 11 nines | 99.9 % | ≥3 | 90 days | 128 KB | ms + per-GB fee |
| S3 Glacier Flexible Retrieval | `GLACIER` | 1–2/year, minutes–hours | 11 nines | 99.99 % *after restore* | ≥3 | 90 days | 40 KB overhead | restore: expedited 1–5 min, standard 3–5 h, bulk 5–12 h |
| S3 Glacier Deep Archive | `DEEP_ARCHIVE` | <1/year, hours | 11 nines | 99.99 % *after restore* | ≥3 | 180 days | 40 KB overhead | restore: standard ≤12 h, bulk ≤48 h |
| Reduced Redundancy | `REDUCED_REDUNDANCY` | **deprecated — do not use** | 99.99 % (0.01 % annual loss) | 99.99 % | ≥3 | none | none | ms |
| S3 Outposts | `OUTPOSTS` | S3 on Outposts only | — | — | on-prem | none | none | local |

Key facts burned into this table:
- **All classes except One Zone-IA and Express One Zone survive the loss of an AZ** (data is stored in ≥3 AZs, or 1 AZ with different trade-offs).
- **Only Standard is 99.99 % available** among the multi-AZ non-archived classes; IA is 99.9 %; One Zone-IA is 99.5 %; Express One Zone 99.95 %.
- **Archived classes** (Glacier FR, Deep Archive) are not readable in real time — you must **restore** a temporary copy first.
- **Glacier Instant Retrieval is NOT archived** — it is readable with `GET` in milliseconds, no restore.
- **Reduced Redundancy Storage is legacy** — “we recommend not using this storage class; S3 Standard is more cost-effective”.

### 3.2 S3 Standard (`STANDARD`)

- Default class for every upload that does not specify one.
- Frequent access, millisecond latency, high throughput, ≥3 AZs, 99.99 % availability, 11 nines durability.
- No minimum duration, no minimum object size, no retrieval fee.
- Use for: active datasets, websites, mobile apps, data lakes' hot zone, anything you cannot predict or that is read often.

### 3.3 S3 Express One Zone (`EXPRESS_ONEZONE`)

- A **performance** class, not a cost-optimisation class: **single-digit-millisecond, consistent latency**, up to **10× faster** than S3 Standard, for latency-sensitive workloads (ML training/inference feeds, analytics, media editing, HPC checkpoints).
- Stored in **directory buckets** in **one AZ of your choice**; designed for **99.95 % availability** in that AZ, 11 nines durability; data is redundantly stored on multiple devices *within* the AZ but is **not** resilient to AZ loss.
- **Request costs are lower than S3 Standard** (AWS pages cite 50–80 % lower depending on the page — don't memorise the number; memorise “lower request cost”). Storage cost per GB is higher.
- Access model differs: **session-based authorisation** (`CreateSession` returns temporary credentials scoped to the bucket, valid 5 minutes), **zonal and regional endpoints**, gateway VPC endpoints supported.
- Limits worth knowing: ACLs are disabled (bucket-owner-enforced Object Ownership); **Block Public Access is fully enabled by default and cannot be modified**; SSE-S3/SSE-KMS only (no SSE-C, no DSSE-KMS); **server access logs not supported** (use CloudTrail); no checksum MD5.
- **Lifecycle for directory buckets supports expiration and aborting incomplete multipart uploads only — no transitions between classes** (and the lifecycle service principal needs an explicit `s3express:CreateSession` ReadWrite permission).
- Use for: co-located compute + storage needing the lowest latency; not for durable multi-AZ data, not for archival.

### 3.4 S3 Intelligent-Tiering (`INTELLIGENT_TIERING`)

- The **only class that automatically moves individual objects between access tiers** based on observed access — “ideal when access patterns are unknown or changing”.
- **No retrieval fees.** You pay a small **monthly monitoring & automation fee per object**, plus storage.
- **Three automatic tiers:**
  - **Frequent Access** — where every object starts;
  - **Infrequent Access** — after **30 consecutive days** without access;
  - **Archive Instant Access** — after **90 consecutive days** without access, still **millisecond** latency.
- **Two optional archive tiers** (activate per bucket):
  - **Archive Access** — objects not accessed for a configurable minimum of **90 days** (up to **730 days**);
  - **Deep Archive Access** — not accessed for a configurable minimum of **180 days** (up to **730 days**).
- Restoring from either archive tier moves the object back to **Frequent Access**; IT restores take **no `Days` parameter** and require `RestoreObject` first.
- **Objects smaller than 128 KB are not monitored and not tiered** — they always stay in Frequent Access (and are billed there).
- There are **no minimum storage durations**; you can delete/transition at any time.
- **Conflicts:** “S3 Intelligent-Tiering is favored by S3 Lifecycle over any storage class, aside from Glacier Flexible Retrieval and Glacier Deep Archive.”
- Monitoring/automation fee is per object — a bucket with 100 million tiny objects can pay more in monitoring than it saves.
- **Billing exception:** transitions *into* Intelligent-Tiering are billed only **after the physical transition completes** (all other transitions bill from the moment the rule is satisfied).
- Use for: unknown/changing access patterns, multi-tenant data lakes, “we don't know what's hot”, SaaS customer data you can't predict.

### 3.5 S3 Standard-IA (`STANDARD_IA`)

- Millisecond access, ≥3 AZs, 99.9 % availability, **30-day minimum storage duration**, **128 KB minimum billable object size**, **per-GB retrieval fee**.
- Intent: long-lived, infrequently accessed data (≈ once a month or less) that still needs real-time access — backups, older data, secondary copies.
- AWS recommends it for **your primary or only copy of data that can't be re-created** (because it retains multi-AZ durability).
- Early deletion/overwrite/transition before 30 days → charged for the remaining days.
- Objects <128 KB are billed **as 128 KB**.

### 3.6 S3 One Zone-IA (`ONEZONE_IA`)

- Same millisecond access and 30-day/128 KB rules, but data sits in **one AZ**; designed for **99.5 % availability**; **not resilient to AZ loss**.
- Cheaper than Standard-IA.
- Use for: **re-creatable** data (thumbnails, derived data, transcodes, replicas when using Cross-Region Replication), secondary copies.
- Never for the only copy of critical data.

### 3.7 S3 Glacier Instant Retrieval (`GLACIER_IR`)

- **Long-term archive with millisecond retrieval.** Not archived — `GET` works directly, no restore.
- **90-day minimum storage duration**, **128 KB minimum billable size**, per-GB retrieval fee.
- Cheaper storage than Standard-IA, higher access costs; designed for data accessed about **once a quarter** (medical images, news archives, media assets, compliance data needing immediate access).
- Use when the data is cold **but** a retrieval SLA of milliseconds is mandatory — the classic exam differentiator vs Glacier Flexible Retrieval.

### 3.8 S3 Glacier Flexible Retrieval (`GLACIER`)

- **Archived** — you must restore a temporary copy before reading.
- **90-day minimum storage duration**; **40 KB per-object overhead** (32 KB charged at the class rate + 8 KB charged at Standard rates); per-GB retrieval fee.
- Retrieval options: **Expedited** (typically 1–5 minutes; can be guaranteed with **provisioned capacity**), **Standard** (typically 3–5 hours; minutes–5 h with Batch Operations), **Bulk** (typically 5–12 hours, **free**).
- Designed for archive data accessed **once or twice a year**, minutes-to-hours restore acceptable: backups, DR copies, long-term media.
- Objects >5 TB: any option can take up to ~48 h (300 MB/s retrieval throughput).

### 3.9 S3 Glacier Deep Archive (`DEEP_ARCHIVE`)

- **Lowest-cost storage in AWS.** Archived; 180-day minimum duration; 40 KB overhead; per-GB retrieval fee.
- Retrieval: **Standard** ≤12 h (9–12 h with Batch Operations), **Bulk** ≤48 h; **no Expedited**.
- Designed for data accessed **less than once a year** over multiple years — regulatory retention, tape replacement, long-term backups.
- The classic exam answer when “retain 7–10 years, retrieval within 12 hours, lowest cost” appears.

### 3.10 Legacy: Reduced Redundancy Storage and S3 Outposts

- **RRS**: 99.99 % durability (average annual expected loss 0.01 % of objects); deprecated, **more expensive than Standard** — never choose it.
- **S3 Outposts (`OUTPOSTS`)**: for buckets on Outposts; encrypted with SSE-S3 (optionally SSE-C); **SSE-KMS not supported**; other classes are invalid for Outposts buckets (`InvalidStorageClass`).

> **Note on the standalone Amazon Glacier service:** the S3 Glacier *classes* live inside Amazon S3 and are accessed through S3 APIs. The separate Amazon Glacier service (vaults/archives) is legacy, has no S3 features, and is **not** where you should put data. Objects in S3 Glacier classes cannot be read through the Glacier service.

---

## 4. Choosing a storage class — decision tree

```
Is the data read more than once a month?
├─ YES → S3 Standard            (latency-critical? consider Express One Zone)
└─ NO
   Do you know the access pattern (or can you predict it)?
   ├─ NO → S3 Intelligent-Tiering
   └─ YES
      Is real-time (millisecond) access required?
      ├─ YES
      │    Is the data recreatable / a replica?
      │    ├─ YES → S3 One Zone-IA
      │    └─ NO  → Is it accessed about once a month or more?
      │             ├─ YES → S3 Standard-IA
      │             └─ NO (≈ quarterly) → S3 Glacier Instant Retrieval
      └─ NO (restore acceptable)
           Can you wait minutes–hours (up to 12 h) for retrieval?
           ├─ YES (1–2×/year) → S3 Glacier Flexible Retrieval
           └─ NO, need 12–48 h and want the absolute cheapest → S3 Glacier Deep Archive
```

### 4.1 Access-pattern cheat sheet

| Access frequency | Latency needed | Class |
|---|---|---|
| daily / continuous | ms | Standard (or Express One Zone) |
| unknown or changing | ms | Intelligent-Tiering |
| ~once a month | ms | Standard-IA |
| ~once a quarter | ms | Glacier Instant Retrieval |
| 1–2× per year | minutes–hours | Glacier Flexible Retrieval |
| <1× per year, retention | 12–48 h | Glacier Deep Archive |
| recreatable only | ms | One Zone-IA |

### 4.2 Classes you should almost never choose in a design answer

- **RRS** — deprecated and more expensive.
- **One Zone-IA for primary data** — single AZ defeat.
- **Glacier classes for data that is read daily** — retrieval fees and restore times break the design.
- **Intelligent-Tiering for tiny-object, purely archival workloads** — monitoring fee without savings.

---

## 5. Archiving and restore mechanics

### 5.1 What “archived” means

Archived objects are still S3 objects in your bucket, but they are **not readable in real time**. You call `RestoreObject` (or the console / SDK / CLI / S3 Batch Operations) to create a **temporary copy**; when the restore completes you can `GET` the object. The object's **storage class does not change** — it stays `GLACIER`/`DEEP_ARCHIVE`; you are paying for **both** the archived object and the temporary copy.

- The temporary copy lives for the number of **`Days`** you specify; S3 rounds the expiry to the **next midnight UTC** after the restore completes. Reissuing a restore **extends** the expiry.
- Only **one restore request per object at a time**; incremental days can be added to an in-progress/completed restore.
- Restores are asynchronous; one restore per object; up to **1,000 restore requests/second per account per Region** (throttling returns `ThrottlingException`); large datasets restore at up to **1–2 PB/day** per account. Use **S3 Batch Operations** to restore many objects with one job.
- You can get notified when a restore completes via **S3 Event Notifications** / EventBridge.

### 5.2 Retrieval options and times (memorise this table)

| Storage class / tier | Expedited | Standard | Bulk |
|---|---|---|---|
| **Glacier Flexible Retrieval** | 1–5 min (provisioned capacity for guarantees) | 3–5 h (minutes–5 h via Batch Operations) | 5–12 h (**free**) |
| **Deep Archive** | **not available** | ≤12 h (9–12 h via Batch Operations) | ≤48 h (cheaper) |
| **Intelligent-Tiering Archive Access** | 1–5 min | 3–5 h (minutes via Batch Ops) | 5–12 h |
| **Intelligent-Tiering Deep Archive Access** | not available | ≤12 h (≤9 h via Batch Ops) | ≤48 h |

Additional rules:
- **Expedited retrieval is a premium feature** and is charged at the Expedited rate.
- **Provisioned capacity** (Glacier FR) guarantees Expedited capacity: each unit supports **at least 3 Expedited retrievals per 5 minutes** and up to **300 MB/s** of retrieval throughput; capacity is **per AWS account** (the requester buys it, not the bucket owner).
- For **Intelligent-Tiering**, **Standard retrievals are free**, and Bulk is free for IT and Glacier FR. Restoring from IT archive tiers moves the object back to **Frequent Access**.
- Objects **larger than 5 TB** can take up to ~48 h with up to 300 MB/s retrieval throughput, regardless of tier.

### 5.3 The cost of a restore

Restoring is not free even when the retrieval tier is “free”:
1. **Retrieval fee** per GB for IA/Glacier classes (Expedited costs more than Standard, Standard more than Bulk).
2. **Request charges** per 1,000 restore/GET requests.
3. **Storage for the temporary copy** at S3 Standard rates while it exists.
4. The **archived copy keeps billing** in its class.

So the classic exam answer to “is it cheaper to restore frequently?” is: **if you retrieve often, you chose the wrong class** — go up a tier (Deep Archive → FR → IR → IA → Standard).

### 5.4 Restore SLA design rules

| RTO / latency requirement | Minimum class to choose |
|---|---|
| milliseconds, frequent access | Standard |
| milliseconds, infrequent (quarterly) | Standard-IA or Glacier IR |
| minutes (urgent restores) | Glacier FR with Expedited + provisioned capacity |
| hours (3–5 h) | Glacier FR Standard |
| ≤12 h | Glacier Deep Archive Standard or Glacier FR |
| 12–48 h | Glacier Deep Archive Bulk (cheapest) |

---

## 6. S3 Lifecycle — concepts and configuration elements

### 6.1 What it is

An **S3 Lifecycle configuration** is a set of **rules** attached to a **bucket** (one configuration per bucket, **up to 1,000 rules**, limit not adjustable; rule `<ID>` unique in the bucket, ≤255 characters; `<Status>` `Enabled` or `Disabled`). Rules apply to **existing and future objects**.

### 6.2 Two action families

| Family | Actions | Effect |
|---|---|---|
| **Transition** | `Transition`, `NoncurrentVersionTransition` | move an object (current or noncurrent version) to another storage class |
| **Expiration** | `Expiration`, `NoncurrentVersionExpiration`, `ExpiredObjectDeleteMarker` | delete the object/version, or clean up expired delete markers |
| **Cleanup** | `AbortIncompleteMultipartUpload` | abort multipart uploads that never completed |

### 6.3 The filter

A rule can apply to all objects or to a subset. Filters:

- **Key prefix** — one prefix per rule;
- **Object tags** — one or more; multiple tags are AND-ed; each tag key must be unique;
- **Object size** — `ObjectSizeGreaterThan` / `ObjectSizeLessThan` (bytes; values are **excluded**; max must be greater than min);
- **Combination** of prefix + tags/size → wrap everything in `<And>` (logical AND);
- **Empty filter** → applies to **all objects in the bucket**.

If you want different actions for different prefixes, write **separate rules** (one prefix per rule).

### 6.4 Day-based vs date-based rules

| | Day-based (`Days`, `NoncurrentDays`, `DaysAfterInitiation`) | Date-based (`Date`) |
|---|---|---|
| Semantics | days since object creation / since becoming noncurrent / since MPU initiation | absolute date (ISO 8601, **midnight UTC**) |
| Calculation | creation time + N days, **rounded up to the next midnight UTC** | exact date |
| One-time? | — | **No** — a date-based rule keeps applying after the date passes while enabled (to new objects too) |
| Console support | yes | **cannot create in the console** (view/disable/delete only) |
| Mixing | you **cannot use `Date` and `Days` in the same rule** | — |

A date in the past makes all qualifying objects immediately eligible.

### 6.5 The four questions to answer for any rule

1. **Which objects?** (prefix / tags / size)
2. **Which versions?** (current, noncurrent, or both)
3. **Which action, when?** (transition at day X, expire at day Y, abort MPU at day Z)
4. **What happens to versioning and replication?** (delete markers, locked versions, pending replicas)

---

## 7. Lifecycle actions in detail

### 7.1 Transition (current versions)

Moves the **current version** of eligible objects to the destination class when `Days` elapse. If both `Date` and `Days` are absent, the action applies immediately. Example: `logs/` → `STANDARD_IA` at 30 days, `GLACIER` at 90 days, expire at 365 days (one rule with three actions is valid when all actions share the same filter).

### 7.2 Expiration (current versions)

| Bucket state | What `Expiration` does |
|---|---|
| **Nonversioned** | permanently removes the object |
| **Versioning enabled** | adds a **delete marker** as the new current version; the old current version becomes **noncurrent** — the data is **not** deleted and keeps billing |
| **Versioning suspended** | creates a delete marker with **null** version ID, replacing any null version → effectively deletes that object |

Expiration applies to **current versions only** — noncurrent versions need their own rule. If the current version is already a delete marker, S3 does nothing. A delete marker with **zero noncurrent versions** is an **expired object delete marker** and can be removed automatically.

### 7.3 Noncurrent versions

- `NoncurrentVersionTransition` — transition noncurrent versions `NoncurrentDays` after they became noncurrent; optionally keep at least **`NewerNoncurrentVersions` (1–100)** newer versions before acting.
- `NoncurrentVersionExpiration` — **permanently delete** noncurrent versions after `NoncurrentDays`; optionally retain `NewerNoncurrentVersions` (1–100).
- **Both** thresholds (`NoncurrentDays` **and** `NewerNoncurrentVersions`) must be exceeded for the action to run.
- To use `NewerNoncurrentVersions` you **must provide a `<Filter>`** element; otherwise S3 returns `InvalidRequest`.
- Days noncurrent = days since the **successor version was created** (not since the object was created).
- **Noncurrent version actions have no effect in a nonversioned bucket.**

### 7.4 Expired object delete markers

- `ExpiredObjectDeleteMarker` (`true`) removes delete markers that are the only remaining version — the final cleanup that makes versioned buckets shrink.
- **Cannot be combined with `Days`** in the same rule; **cannot be used with a tag-based filter**.
- If a rule has `Days` expiration, S3 performs the delete-marker cleanup automatically when the marker is old enough.

### 7.5 Abort incomplete multipart uploads

- `AbortIncompleteMultipartUpload` with `DaysAfterInitiation` — aborts uploads not completed within N days and deletes their parts.
- **Expiration does NOT remove incomplete multipart uploads** — without this rule, abandoned parts bill forever (classic cost-leak exam question).
- **Cannot be used in a rule with a tag-based filter.**

### 7.6 Versioning behaviour summary

| Action | Nonversioned | Versioning enabled | Versioning suspended |
|---|---|---|---|
| `Transition` | transitions the object | transitions current version | same as enabled |
| `Expiration` | permanent delete | adds delete marker | adds null-version delete marker (deletes null version) |
| `NoncurrentVersionTransition` | no effect | transitions noncurrent versions | same as enabled |
| `NoncurrentVersionExpiration` | no effect | permanently deletes noncurrent versions | same as enabled |

Best practice for restoring a previous version under lifecycle: **copy** the noncurrent version to make it current (method 1), rather than permanently deleting the current version (method 2) — lifecycle is eventually consistent and might delete the noncurrent version you were trying to promote.

---

## 8. The transition waterfall — what is allowed and what is not

### 8.1 Supported transitions (current versions)

| From | To |
|---|---|
| **Standard** | Standard-IA, Intelligent-Tiering, One Zone-IA, Glacier IR, Glacier FR, Glacier DA |
| **Standard-IA** | Intelligent-Tiering, One Zone-IA, Glacier IR, Glacier FR, Glacier DA |
| **One Zone-IA** | Glacier FR, Glacier DA *(not Standard-IA, not Glacier IR)* |
| **Glacier Instant Retrieval** | Glacier FR, Glacier DA |
| **Glacier Flexible Retrieval** | Glacier DA only |
| **Glacier Deep Archive** | **nothing** (one-way) |
| **Intelligent-Tiering** | by current tier: Frequent/Infrequent → One Zone-IA, Glacier IR, FR, DA; Archive Instant Access → Glacier IR, FR, DA; Archive Access → FR, DA; Deep Archive Access → DA |

Waterfall (downhill only):

```
Standard ─┬→ Standard-IA ─┬→ One Zone-IA ──→ Glacier FR ──→ Deep Archive
          ├→ One Zone-IA ─┘                              ↑
          ├→ Intelligent-Tiering ────────────────────────┤
          ├→ Glacier IR ─────────────────────────────────┤
          └→ Glacier FR / Deep Archive (directly) ───────┘
```

### 8.2 Going “up” the waterfall is impossible with lifecycle

- You **cannot** transition Glacier → Standard, Deep Archive → FR, One Zone-IA → Standard, etc.
- To “warm up” archived data you must **restore** the object and **copy** it (CopyObject with the new storage class) — the restore is temporary; the copy is the real change.
- A single lifecycle rule **cannot chain** transitions faster than the destination class's minimum storage duration. Example from AWS docs: you cannot transition to Glacier IR at day 4 and to Deep Archive at day 20 — the Deep Archive transition must be **at least day 94** (4 + 90-day minimum). You *can* express that as two rules, but you still pay the minimum-duration charges.

### 8.3 The 128 KB default transition size (changed September 2024)

- Since **September 2024**, S3 prevents objects **smaller than 128 KB** from transitioning **to any storage class by default** — because the per-object transition request often costs more than the storage saved.
- **Before September 2024**, small objects could transition only to Glacier FR/Deep Archive. Old configurations keep the old behaviour until edited; editing a rule moves the bucket to the new default.
- You can override with an **object size filter** (`ObjectSizeGreaterThan`/`ObjectSizeLessThan`) — but do the maths first.
- There is an optional request header / setting to change the default minimum transition size (AWS docs are inconsistent on the exact header name between pages — CLI flag `--transition-default-minimum-object-size`, GET output field `TransitionDefaultMinimumObjectSize`). Don't memorise the name; memorise **“the default is 128 KB, and it is overridable”**.

### 8.4 Other transition constraints

- You **cannot** transition objects with **`Pending` or `Failed` replication status** (versioning-enabled/suspended buckets).
- Transitions of encrypted objects keep the encryption unchanged.
- **Directory buckets (Express One Zone): expiration only, no transitions.**
- No transitions into `REDUCED_REDUNDANCY` (and no reason to).

---

## 9. Lifecycle timing, billing and async behaviour

### 9.1 Configuration propagation

- A new or updated lifecycle configuration takes **a few minutes** to propagate; **rule policy updates can take up to 15 minutes**.

### 9.2 Actions are asynchronous — no completion guarantee

- Once a rule is satisfied, S3 **queues** the transition/expiration; the physical action can lag **days or even weeks**.
- When S3 performs the action (transition or delete-marker creation), the timestamp is set to **midnight UTC of the day the action occurred**.
- You should **not rely on Lifecycle alone for deletions with a fixed legal/contractual deadline**. Instead:
  1. run **S3 Inventory** with the **Lifecycle Expiration Date** field to find objects past their expiration date that still exist;
  2. delete/transition them explicitly with **S3 Batch Operations**.
- Disabling or deleting a rule stops **new** scheduling after a short delay; already-scheduled objects are **unscheduled** and are not acted on.

### 9.3 Billing timing (exam favourite)

- **Billing changes when the rule is satisfied**, not when the action completes: as soon as the transition time elapses, you are charged the **destination** class rate even if the object is physically still in the source class. Likewise, expiry stops storage billing at expiry time even if deletion lags.
- **Exception:** transitions **into Intelligent-Tiering** are billed only **after the physical transition completes**.
- For Glacier FR/Deep Archive transitions, the **40 KB overhead** and the **minimum storage duration** charges (90/180 days) start when the **rule is satisfied**, even before the physical move.

### 9.4 Charges that apply to lifecycle itself

- Lifecycle configuration is **free**; you pay **transition request charges per object** and **expiration** happens without extra request charges (but minimum-duration penalties apply if you expire early).
- There is **no data retrieval charge for a lifecycle transition** (it is S3-internal movement), but there **is** a per-request ingestion/transition charge — and the same charge applies if you set the class with `PUT`/`COPY`.

### 9.5 Tag-based rules and evaluation

- Tag-filtered rules are evaluated **daily**; matching objects are queued for asynchronous processing, and S3 **re-evaluates tags at execution time**.
- If the tag is gone at execution, the transition/expiration does **not** proceed — but the race is not deterministic, so “removing the tag cancels it” is not guaranteed.
- To reliably prevent an action, **remove the tag** (tag changes propagate faster) rather than editing rules (up to 15 min); to confirm an action actually happened, wait for a **Lifecycle Event Notification** (EventBridge).

### 9.6 Restrictions that surprise people

- **A bucket policy cannot block lifecycle transitions or deletions** — even a policy denying all principals does not stop lifecycle.
- **Object Lock:** lifecycle does not act on noncurrent versions of Object-Locked objects; retention/legal hold blocks permanent deletion until it expires (delete markers may still be created).
- **Replication:** objects with `Pending`/`Failed` replication status are excluded from lifecycle actions.
- **Annotations** (S3 Metadata feature) are not lifecycle-managed and are always billed at Standard/Frequent rates; they are deleted with the object version.

---

## 10. Conflicting rules — how S3 decides

S3 Lifecycle generally **optimises for cost**:
- two overlapping **expiration** rules → the **shorter** expiration wins;
- two overlapping **transition** rules → S3 transitions to the **lower-cost** class;
- Intelligent-Tiering is favoured over any class **except** Glacier FR and Deep Archive.

When one object is eligible for **multiple actions on the same day**, precedence is:

1. **Permanent deletion** beats transition;
2. **Transition** beats **delete-marker creation**;
3. If eligible for **both Glacier FR and Standard-IA/One Zone-IA** transitions → S3 chooses **Glacier FR**.

Also:
- An object is eligible for **only one lifecycle action per day**;
- Two rules targeting the same objects at the same time are legal but confusing — write non-overlapping rules where possible;
- Example: rule A expires `logs/` at 365 days, rule B transitions `logs/` to Standard-IA at 30 days → no conflict at 30 days; at 365 days expiration wins.

---

## 11. Lifecycle × versioning × replication × Object Lock

### 11.1 Versioning (the #1 “why didn't the bill drop” cause)

- Enabling versioning means **every overwrite/delete keeps the old data as a noncurrent version** — billed at the same class unless you transition noncurrent versions.
- Expiration of the current version only **adds a delete marker**; the noncurrent versions remain and keep billing.
- The complete versioned-bucket lifecycle therefore needs **three ingredients**: current-version transition/expiration, `NoncurrentVersionTransition` / `NoncurrentVersionExpiration`, and `ExpiredObjectDeleteMarker` (or a `Days` expiration that lets S3 clean them up).
- `NewerNoncurrentVersions` (1–100) lets you retain N newer versions and delete the rest — the “limited version history” pattern.

### 11.2 Replication

- Lifecycle **does not replicate**. The **replica bucket has its own lifecycle** — configure it independently (replicas are separate objects; often the right target is a cheaper class such as One Zone-IA or Glacier).
- Replication options include **replicating delete markers** and **replicating metadata/tags**; lifecycle actions on the source are not automatically reflected.
- **S3 Replication Time Control (RTC)** provides a 15-minute replication SLA and replication metrics — relevant when a question mentions RPO.
- Existing objects can be replicated with **S3 Batch Replication**.
- Objects with `Pending`/`Failed` replication status are excluded from lifecycle.

### 11.3 Object Lock (compliance / WORM)

- Requires **versioning enabled** at bucket creation (and can't be disabled later).
- Two retention modes: **Governance** (users with special permission can bypass) and **Compliance** (nobody can delete/extend-shorten until retention expires); plus **legal hold**.
- Lifecycle **cannot permanently delete** a version that is under retention or legal hold — the data stays and keeps billing. Design retention periods so that lifecycle expiration happens **after** the lock period.
- Object Lock works with Glacier classes — the combination “Compliance-mode retention + Deep Archive + lifecycle” is the standard 7–10-year regulatory answer.

### 11.4 Event notifications

- S3 can send **EventBridge events** for lifecycle actions (expiration and transition), restores completing, and inventory completion — useful to verify that “the lifecycle actually ran” and to trigger downstream cleanup.

---

## 12. Lifecycle recipes (copy these)

### 12.1 Application logs (7 days hot, 30 days searchable, 1 year archive, then delete)

```json
{
  "Rules": [
    {
      "ID": "logs-tiering",
      "Filter": {"Prefix": "logs/"},
      "Status": "Enabled",
      "Transitions": [
        {"Days": 7,  "StorageClass": "STANDARD_IA"},
        {"Days": 30, "StorageClass": "GLACIER"}
      ],
      "Expiration": {"Days": 365},
      "AbortIncompleteMultipartUpload": {"DaysAfterInitiation": 7}
    }
  ]
}
```

### 12.2 Versioned application bucket (keep 5 versions, purge the rest)

```json
{
  "Rules": [
    {
      "ID": "version-cleanup",
      "Filter": {"Prefix": ""},
      "Status": "Enabled",
      "NoncurrentVersionExpiration": {
        "NoncurrentDays": 30,
        "NewerNoncurrentVersions": 5
      },
      "Expiration": {"ExpiredObjectDeleteMarker": true}
    }
  ]
}
```

### 12.3 Data lake (bronze / silver / gold)

- `raw/` → transition to `INTELLIGENT_TIERING` after 30 days (patterns unknown), optional Archive Instant Access (automatic) or archive tiers for >1 year.
- `processed/` → `STANDARD_IA` at 30 days, `GLACIER_IR` at 90 days (analytics occasionally re-reads), expire per data-governance policy.
- `curated/` → keep in `STANDARD` (or Express One Zone if query latency is critical).

### 12.4 Compliance archive (10 years, immutable, 12-hour retrieval)

- Upload directly to `DEEP_ARCHIVE` or transition at day 1–7.
- **Object Lock COMPLIANCE** with a 10-year retention; **lifecycle expiration only after retention ends** (otherwise the data stays anyway and you must monitor it).
- Never set a minimal-`Days` expiration that fires before the lock period — it will be blocked and confuse cost reporting.
- Use **S3 Inventory** with the Object Lock fields to audit retention and to prove nothing was deleted early.

### 12.5 Backup target

- Backup data → `STANDARD_IA` or `INTELLIGENT_TIERING`; long-term copies → `GLACIER_FLEXIBLE` or `DEEP_ARCHIVE` depending on restore RTO.
- Always add `AbortIncompleteMultipartUpload` (backup tools fail mid-upload all the time; orphaned parts are pure waste).
- Never put the only backup in One Zone-IA.
- Versioning + lifecycle noncurrent rules to control cost; Object Lock for ransomware/immutability.

### 12.6 Media / thumbnails

- Originals → `GLACIER_IR` (retrieval on demand, ms) after the editing window.
- Thumbnails/transcodes (recreatable) → `ONEZONE_IA`.
- Hot catalogue → `STANDARD`.

---

## 13. Monitoring, analysis and bulk operations

### 13.1 S3 Storage Class Analysis

- Analyses access patterns (by prefix and/or tags) to tell you **when to transition data from Standard to Standard-IA**.
- **Only recommends Standard → Standard-IA.** It does not analyse One Zone-IA, Glacier classes, or Intelligent-Tiering.
- First results appear in the console in **24–48 hours**; it observes for **30 days or longer** before giving a result.
- Up to **1,000 filter configurations per bucket**; daily CSV export to a destination bucket (same Region).
- Costs money (per objects monitored) — use it on large, well-defined prefixes.

### 13.2 S3 Inventory

- Daily or **weekly** (Sundays UTC) plus **initial report**, for a bucket or a prefix; formats **CSV (GZIP), ORC, Parquet**; current versions or **all versions**.
- Includes **storage class**, **Intelligent-Tiering access tier and reference date**, **Lifecycle Expiration Date**, replication status, encryption, Object Lock retention/legal hold, checksums, object owner, ACL, version ID, `IsLatest`, multipart flag.
- **First report can take up to 48 hours**; destination bucket must be in the **same Region** (can be another account); inventory is **eventually consistent**; it does **not** use `List` APIs and does not affect request rates.
- Query with **Amazon Athena** — this is the “audit my tiering” tool, and the only practical way to prove compliance deadlines were met.

### 13.3 S3 Storage Lens

- Organisation-wide dashboards and metrics (accounts, Regions, buckets, prefixes) — the answer to “how do I see S3 cost/usage across 200 accounts?”.
- **Free metrics vs advanced metrics & recommendations** (paid); advanced dashboards can be exported to S3, published to CloudWatch, and grouped by prefixes/tags/regions/accounts (**Storage Lens groups**).
- Useful metrics for this topic: storage by class, object counts, **noncurrent version** storage, incomplete multipart uploads, encryption/replication coverage.
- Retention of metrics: up to **15 months** (advanced/CloudWatch statistics).

### 13.4 CloudWatch

- **Daily bucket storage metrics are free** (`BucketSizeBytes`, `NumberOfObjects`) — good for trend alarms, not class-level breakdown.
- **Request metrics** are opt-in, 1-minute, **charged like custom metrics**, and delivered **best-effort**.

### 13.5 S3 Batch Operations

- The way to act on **existing** objects at scale: **Copy** (with a new storage class → “convert 50 million objects now”), **Restore**, **Replace tags**, **Delete**, **Invoke Lambda**, **Replicate** (Batch Replication).
- Takes a **manifest** (e.g., an Inventory report) or a list of objects; tracks progress and retries failures.
- Use it when lifecycle timing is unacceptable, when you need an immediate class change, or when you must delete objects by a deadline.

### 13.6 Other tools that show up in the same questions

- **AWS Config** rules for lifecycle/versioning/Object Lock compliance (detect buckets without lifecycle).
- **AWS CloudTrail** for bucket-level API audit (not per-object reads unless data events enabled).
- **AWS Backup** for S3 backup policies (independent of lifecycle rules).
- **S3 Event Notifications / EventBridge** for reacting to object creation and lifecycle events.
- **Cost allocation tags** to attribute buckets to applications/teams.

---

## 14. Worked cost maths (illustrative)

> Assumptions (round numbers, **not** current AWS prices): Standard **$0.023/GB-month**, Standard-IA **$0.0125**, One Zone-IA **$0.010**, Glacier IR **$0.004**, Glacier FR **$0.0036**, Deep Archive **$0.00099**; transition request **$0.01 per 1,000 objects**; illustrative retrieval fees **$0.01/GB** (IA/IR), **$0.03/GB** (FR Expedited), **$0.02/GB** (DA). Verify all of it on the pricing page — the **method** is what matters.

### 14.1 1 TB (1,024 GB) stored for a month

| Class | Storage/month | Notes |
|---|---|---|
| Standard | $23.55 | baseline |
| Standard-IA | $12.80 | + retrieval fee on access, 30-day min |
| One Zone-IA | $10.24 | + single-AZ risk |
| Glacier IR | $4.10 | + retrieval fee; 90-day min |
| Glacier FR | $3.69 | + 40 KB/object overhead; restore to read |
| Deep Archive | $1.01 | + 40 KB overhead; 12–48 h restore |

### 14.2 When does a transition pay for itself?

Transition cost ≈ `(number of objects / 1,000) × $0.01`. Monthly storage saving ≈ `GB × (Standard price − target price)`.

**Case A — 1 TB in 1,000 objects of ~1 GB each:** total transition cost = **$0.01**; Standard→Standard-IA saves **$10.75/month** → pays back in **days**. Tiering obviously wins.

**Case B — 1 TB in 1 million objects of ~1 MB each:** transition cost ≈ **$10**; the same monthly saving of $10.75 → payback ≈ **1 month** (ignore retrieval). Acceptable, but only if the objects are genuinely cold and large enough to avoid the 128 KB billing floor.

**Case C — 1 TB in 10 million objects of ~100 KB each:** every object is billed as **128 KB** in IA classes → billed volume grows to ~1.28 TB (≈28 % more), so the monthly saving shrinks to ~**$7** (instead of ~$10.75), while transition costs ≈ **$100** (10 M objects) → payback **over a year**, before any retrieval or per-request charges. **Small objects are not a tiering problem, they are an object-size problem** — aggregate small files first (e.g. into Parquet/tar shards).

### 14.3 The 40 KB overhead matters for tiny archives

For 10 million objects in Glacier FR/Deep Archive, the **40 KB of metadata per object** adds **400 GB** of billable storage (32 KB at the class rate + 8 KB at Standard rate) — often larger than the actual data for small objects. Aggregate small files before archiving, or accept that per-object archives of tiny files are uneconomical.

### 14.4 Early-deletion penalty example

A 1 TB dataset transitioned to Glacier IR on day 10 and deleted on day 40: you are charged the **90-day minimum** on that volume (i.e., the remaining ~60 days), regardless of the actual residency. The same applies to IA (30 days) and Deep Archive (180 days).

### 14.5 Retrieval break-even

If you read 10 % of a 1 TB Standard-IA dataset every month, retrieval fees and requests can cancel the storage saving versus Standard — the rule of thumb is: **the cooler the class, the fewer bytes you should be reading per month**. If a workload reads more than a few percent of the archive per month, use the class one step warmer (or Intelligent-Tiering).

---

## 15. Design playbook — scenario → architecture

| Scenario keywords | Recommended design |
|---|---|
| “unknown/changing access patterns” | default bucket class `INTELLIGENT_TIERING`; no lifecycle transitions needed |
| “reduce storage cost, data read once a month, ms latency” | lifecycle → Standard-IA at 30 days |
| “data read once a quarter, must be immediately available” | lifecycle → Glacier Instant Retrieval at 90 days |
| “backups 1–2×/year restore, restore in hours” | lifecycle → Glacier Flexible Retrieval; Expedited + provisioned capacity if urgent |
| “7-year compliance retention, restore within 12 h, cheapest” | Deep Archive + Object Lock COMPLIANCE + lifecycle |
| “recreatable thumbnails/derived data” | One Zone-IA |
| “data lake bronze/silver/gold” | raw → Intelligent-Tiering; processed → IA/IR; curated → Standard |
| “bill didn't drop after lifecycle” | versioning: add `NoncurrentVersionExpiration` + delete-marker cleanup; check replication-pending objects |
| “small files, 10 M objects” | aggregate into larger objects (e.g., Parquet/tar) **before** tiering |
| “must delete by legal date, not just eventually” | Inventory (Lifecycle Expiration Date field) + Batch Operations delete |
| “convert existing 100 M objects now” | S3 Batch Operations Copy with new storage class |
| “ml training needs sub-ms/max throughput” | S3 Express One Zone directory bucket co-located with compute |
| “temporary ML checkpoints” | Express One Zone (or Standard) + short expiration lifecycle; no transitions |
| “SaaS per-tenant data, unpredictable” | Intelligent-Tiering + prefix per tenant; Storage Lens by prefix |
| “multi-account storage cost visibility” | S3 Storage Lens organisation dashboard + cost allocation tags |
| “audit what's actually in the bucket” | S3 Inventory + Athena |

### 15.1 The standard tiering ladder (safe default)

```
Day 0        → Standard (hot)
Day 30-ish   → Standard-IA (cool, ms)          [skip if access unknown → IT]
Day 90-ish   → Glacier Instant Retrieval       [only if ms needed, quarterly]
Day 90–365   → Glacier Flexible Retrieval      [restore in hours]
Day 365+     → Glacier Deep Archive            [restore 12–48 h]
Day N        → Expire (align with retention policy and Object Lock)
+ always: abort incomplete MPUs; expire noncurrent versions; delete expired markers
```

Rules of thumb:
- Never tier data you will delete within the minimum duration.
- Never tier objects <128 KB without measuring; aggregate first.
- Keep a **hot tier for the working set**, archive only the long tail.
- **One class per data tier per prefix** — avoid overlapping rules.

---

## 16. Exam traps and distractors

1. **“Lifecycle expiration deleted my data but the bill still shows storage.”** Versioning: expiration adds a **delete marker**; noncurrent versions remain. Add `NoncurrentVersionExpiration`.
2. **“The lifecycle rule runs but nothing happens.”** Check: rule disabled? prefix/tag mismatch? object <128 KB (default no transition)? transition not supported from that class? replication status pending/failed? objects under Object Lock?
3. **“Transition Glacier → Standard.”** Not possible via lifecycle — restore + copy.
4. **“Deep Archive objects transitioned back.”** Deep Archive is one-way; restore gives a temporary copy with the same class.
5. **“Glacier Instant Retrieval requires restore.”** No — it is **not archived**; `GET` works in milliseconds.
6. **“Glacier Flexible Retrieval can transition to Glacier Instant Retrieval.”** No — only down to Deep Archive.
7. **“One Zone-IA can be transitioned to Glacier Instant Retrieval.”** No — only to FR/DA.
8. **“Intelligent-Tiering has retrieval fees.”** No; it has a per-object monitoring/automation fee instead.
9. **“Intelligent-Tiering tiers objects of any size.”** Only objects **≥128 KB** are monitored; smaller ones stay in Frequent Access.
10. **“Lifecycle moves objects immediately.”** Asynchronous queue; can lag days–weeks; don't rely on it for a legal deadline.
11. **“Billing follows the physical move.”** Billing changes when the **rule is satisfied** (exception: transitions into Intelligent-Tiering).
12. **“A deny-all bucket policy stops lifecycle.”** No — lifecycle bypasses bucket policies.
13. **“Transition at day 4 to Glacier IR, then day 20 to Deep Archive.”** Impossible in one rule — must respect the 90-day minimum of the destination (≥ day 94) or use two rules and pay the minimum.
14. **“Small objects transition automatically.”** Since Sept 2024, **<128 KB does not transition by default** to any class.
15. **“IA is cheaper for daily reads.”** Retrieval fees + 128 KB floor can make it more expensive.
16. **“One Zone-IA is fine for the only copy of important data.”** Single AZ.
17. **“Glacier classes are the separate Amazon Glacier service.”** No — they are S3 classes; the vault service is legacy and separate.
18. **“Expiration deletes old versions automatically.”** No — only current versions; delete markers and noncurrent versions need their own rules.
19. **“Expired object delete markers can be removed with a tag-filtered rule.”** Not allowed — no tag filter with `ExpiredObjectDeleteMarker` (same for `AbortIncompleteMultipartUpload`).
20. **“Date-based rules run once.”** They keep applying after the date while enabled; also can't be created in the console.
21. **“Lifecycle config is per prefix/account.”** It is **per bucket**, up to **1,000 rules**.
22. **“Transitions cost nothing.”** One request per transitioned object; small objects make this dominant.
23. **“Glacier FR/DA have no overhead for tiny objects.”** 40 KB per object (32 KB + 8 KB), so per-object archives of tiny files are uneconomical.
24. **“Unfinished multipart uploads are cleaned by expiration.”** No — add `AbortIncompleteMultipartUpload`.
25. **“S3 Storage Class Analysis recommends Glacier classes.”** Only Standard → Standard-IA.
26. **“Lifecycle deletes objects under legal hold.”** No — Object Lock retention/legal hold blocks permanent deletion (delete markers may appear).
27. **“S3 Express One Zone is durable across AZs.”** Single AZ by design (99.95 % availability).
28. **“Lifecycle transitions work on directory buckets.”** Expiration/abort only — **no transitions**.
29. **“CloudWatch gives per-class storage breakdown for free.”** Daily bucket totals are free; class-level and per-tier data come from Storage Lens/Inventory.
30. **“Restore is a one-time permanent recall.”** It creates a temporary copy that expires after `Days`; reissue to extend; the object stays archived.
31. **“Customer managed key = SSE-C headers.”** No — a *customer managed* key is a **KMS** key (`aws:kms` + optional key-id). SSE-C is a *customer-provided* key with the three `...customer-*` headers (§21).
32. **“Send the KMS key-id on GET so S3 can decrypt.”** No — SSE-KMS reads must **not** include encryption headers (HTTP 400); access is authorised by `kms:Decrypt`.
33. **“SSE-C behaves like KMS: upload once, then anyone with IAM permission can read.”** No — every reader must send the key, AWS services can't natively decrypt SSE-C, and since April 2026 SSE-C is blocked on new buckets until explicitly enabled.
34. **“Rotating a KMS key breaks existing objects.”** No — KMS keeps old key material for decryption; S3 data keys are re-wrapped transparently. (SSE-C key rotation is entirely your problem.)
35. **“Presigned URLs need a public bucket.”** No — they work on private buckets, **even with all Block Public Access settings on**; the URL authorises the request as the signer.
36. **“Presigned URLs can last 30 days.”** No — max **7 days** with long-term IAM user credentials; with temporary credentials the URL dies when the session does (often 1 hour, EC2 instance profiles ≤ ~6 h).
37. **“A presigned URL can be revoked individually.”** No — only short expiry, credential rotation, or a policy deny; treat it as a bearer token.
38. **“Block Public Access blocks presigned requests.”** No — BPA evaluates *policies and ACLs*; presigned requests are authenticated, so BPA doesn't apply to them.
39. **“Public read also exposes the object listing.”** No — `s3:GetObject` and `s3:ListBucket` are separate grants (object ARN `/*` vs bucket ARN).
40. **“Making a bucket public is just a flag.”** No — it's the *effective* result of a public bucket/access-point policy or public ACL, and BPA can prevent it entirely.
41. **“KMS-encrypted buckets can serve an S3 static website to anonymous users.”** No — SSE-KMS has no anonymous access; use CloudFront + **OAC** (OAI doesn't support SSE-KMS).
42. **“An IAM policy granting s3:GetObject lets anonymous users read a bucket.”** No — anonymous requests can only be allowed by a resource-based policy or ACL with `Principal: "*"`.
43. **“Cross-account bucket access needs only a bucket policy.”** No — the caller's identity policy must also allow the action (both sides), plus the KMS key policy for SSE-KMS.
44. **“A presigned URL stays valid for its full TTL even if the signer is deleted.”** No — revoking/deactivating the signer's credentials (or expiring the session) kills the URL immediately.

---

## 17. Practice questions (SAP-C02 style)

> Answer before reading §18.

**Q1.** An application writes 2 TB of logs per day. Logs are queried constantly for the first 7 days, occasionally for the next 30 days, and never after that — but the company must retain them for 2 years. Retrieval for old logs within a few hours is acceptable. What is the most cost-effective lifecycle design?
A. Standard → Deep Archive at 7 days → expire at 2 years
B. Standard → Standard-IA at 7 days → Glacier Flexible Retrieval at 30 days → expire at 730 days
C. Standard → Glacier Instant Retrieval at 30 days → expire at 2 years
D. Standard for 2 years, then delete

**Q2.** A bucket holds 10 million 50 KB thumbnail objects, accessed a few times a year with millisecond latency required. The team proposes S3 Standard-IA. What is the best recommendation?
A. Standard-IA is correct and will halve storage cost
B. Keep them in S3 Standard (or aggregate them into larger objects), because objects <128 KB are billed as 128 KB in IA classes and per-object costs dominate
C. Move them to Glacier Instant Retrieval
D. Move them to One Zone-IA

**Q3.** A data lake receives objects from many teams. Nobody can predict which datasets will become hot. The company wants automatic savings with no retrieval fees and no operational effort. Which class?
A. Standard-IA
B. Intelligent-Tiering
C. One Zone-IA
D. Glacier Instant Retrieval

**Q4.** A media company regenerates thumbnails and transcodes at any time from source masters. The derived files are read rarely but must be available in milliseconds. Which class is most appropriate?
A. One Zone-IA
B. Standard
C. Glacier Flexible Retrieval
D. Deep Archive

**Q5.** A regulator requires 7 years of immutable records, restorable within 12 hours, at the lowest possible cost. Which design?
A. Glacier Instant Retrieval + versioning
B. Deep Archive + Object Lock (Compliance mode) + lifecycle rule that expires only after retention ends
C. Standard-IA + cross-region replication
D. Intelligent-Tiering with Archive Access

**Q6.** A finance team needs quarterly reports available with millisecond retrieval for five years. Which class is designed for this?
A. Standard-IA
B. Glacier Instant Retrieval
C. Glacier Flexible Retrieval
D. One Zone-IA

**Q7.** A lifecycle rule transitions `data/` to `GLACIER_IR` at 30 days and to `DEEP_ARCHIVE` at 60 days. What happens?
A. Both transitions run
B. The rule is invalid/ineffective for the second transition — Deep Archive cannot follow 30 days later because Glacier IR has a 90-day minimum; the earliest valid Deep Archive transition is ~day 120
C. S3 overrides the first transition
D. Nothing happens because 30 < 128 KB

**Q8.** Versioning is enabled. A 90-day expiration rule was added three months ago, but storage cost did not fall. What is the most likely cause and fix?
A. Lifecycle is disabled by default — enable it
B. Expiration creates delete markers only; noncurrent versions remain. Add `NoncurrentVersionExpiration` (and expired-delete-marker cleanup)
C. Versioning blocks all lifecycle actions
D. The bucket policy denies deletion

**Q9.** A bucket is billed for storage that does not match any visible object. What should you check first?
A. Cross-region replication failures
B. Incomplete multipart uploads — add `AbortIncompleteMultipartUpload`
C. Object Lock retention
D. Storage Lens is disabled

**Q10.** A legal team says objects must be deleted on the exact retention deadline. What should you design?
A. Lifecycle expiration — it is exact
B. Lifecycle expiration plus S3 Inventory with the Lifecycle Expiration Date field and S3 Batch Operations to delete objects that missed the deadline
C. Object Lock in Compliance mode
D. A date-based lifecycle rule

**Q11.** Which tool tells you whether to move data from Standard to Standard-IA and gives you the recommended age for the transition?
A. S3 Storage Class Analysis
B. S3 Storage Lens
C. S3 Inventory
D. CloudWatch request metrics

**Q12.** A company must convert 80 million existing objects from Standard to Standard-IA immediately, and new uploads must not be affected. What should you use?
A. A lifecycle rule with `Days: 0`
B. S3 Batch Operations Copy with the target storage class
C. Glacier restore
D. S3 Replication

**Q13.** A team must restore 5 TB from Deep Archive next month with 48 hours of tolerance and minimum cost. Which retrieval tier?
A. Expedited
B. Standard
C. Bulk
D. Provisioned capacity

**Q14.** Expedited restores from Glacier Flexible Retrieval are occasionally refused during peak periods. How do you guarantee capacity?
A. Use Standard retrievals
B. Purchase provisioned capacity units (each supports ≥3 Expedited retrievals per 5 minutes, up to 300 MB/s)
C. Move objects to Deep Archive
D. Increase the restore `Days` value

**Q15.** A source bucket replicates to a DR bucket in another Region. The source bucket has a lifecycle rule that moves objects to Glacier Deep Archive after 90 days. What happens to the replicas?
A. Replicas automatically transition too
B. Replicas are separate objects; the destination bucket needs its own lifecycle configuration
C. Replication fails when the source object becomes Glacier
D. Replicas are deleted when the source expires

**Q16.** A bucket has Object Lock with a 3-year retention and a lifecycle rule that expires objects at 1 year. What happens?
A. Objects are deleted at 1 year
B. Deletion is blocked until retention expires; the data keeps billing — the rules must be aligned
C. Lifecycle overrides Object Lock
D. Object Lock prevents all lifecycle actions

**Q17.** An ML team needs temporary checkpoints in a directory bucket, deleted after 7 days. What is true?
A. Lifecycle can transition checkpoints to Standard-IA
B. Lifecycle supports expiration and abort of incomplete MPUs only; transitions are not supported, and the lifecycle service principal needs `s3express:CreateSession`
C. Directory buckets do not support lifecycle
D. Checkpoints must be deleted manually

**Q18.** Objects of 60 KB are not transitioning to Standard-IA despite an enabled lifecycle rule. Why?
A. Standard-IA cannot be a lifecycle target
B. Since September 2024, objects <128 KB are not transitioned by default; add a size filter to override (but measure the cost)
C. The rule needs a tag filter
D. The bucket is versioned

**Q19.** Data is accessed about once a month with millisecond latency, retained for years, and must survive the loss of an AZ. Which class?
A. Standard-IA
B. One Zone-IA
C. Glacier Instant Retrieval
D. Deep Archive

**Q20.** A team uploads objects whose future access pattern is unknown and wants the best default without manual lifecycle work. What should the bucket's default storage class be?
A. STANDARD_IA
B. INTELLIGENT_TIERING
C. GLACIER_IR
D. ONEZONE_IA

**Q21.** A security team requires objects encrypted with a key the company creates, rotates and audits itself in AWS KMS, and only principals with explicit KMS permissions may decrypt. Which request headers should the upload use?
A. `x-amz-server-side-encryption-customer-algorithm`, `-customer-key`, `-customer-key-MD5`
B. `x-amz-server-side-encryption: aws:kms` and `x-amz-server-side-encryption-aws-kms-key-id: <customer managed key ARN>`
C. `x-amz-server-side-encryption: AES256`
D. No headers — S3 encrypts everything with SSE-S3 automatically

**Q22.** An application stores objects with SSE-KMS using a customer managed key. A developer's GET request includes `x-amz-server-side-encryption: aws:kms` and the key-id header. What happens?
A. Works — the headers are required for decryption
B. HTTP 400 BadRequest — GET/HEAD for SSE-KMS must not include encryption request headers; the caller needs `kms:Decrypt`
C. HTTP 403 — the key must be sent as an SSE-C key
D. Works only if S3 Bucket Keys are enabled

**Q23.** A legacy application encrypts objects with SSE-C. Which statement is true?
A. S3 stores the key, so readers only need IAM permission
B. Every PUT and every GET/HEAD must send the same three `x-amz-server-side-encryption-customer-*` headers over HTTPS; since April 2026 SSE-C must also be explicitly enabled on new general-purpose buckets (`BlockedEncryptionTypes: NONE`)
C. SSE-C can be set as the bucket's default encryption
D. SSE-C is fully supported in the S3 console

**Q24.** Which configuration makes an S3 bucket publicly readable?
A. An IAM identity policy granting `s3:GetObject` to a role
B. A bucket policy granting `s3:GetObject` to `Principal: "*"` with no limiting condition
C. A bucket policy granting `s3:GetObject` to a fixed partner account ID
D. Enabling S3 Transfer Acceleration

**Q25.** A company uses **account-level** Block Public Access with all four settings on, and now needs a public static website. What is the recommended approach?
A. Disable Block Public Access at the bucket level — account settings are only defaults
B. Keep the bucket private and serve the site through CloudFront with Origin Access Control and a custom domain
C. Embed presigned URLs in the HTML
D. Use the S3 website endpoint, which bypasses Block Public Access

**Q26.** A partner needs a download link valid for **30 days**. What do you build?
A. A presigned URL with a 30-day expiry
B. A CloudFront signed URL (or signed cookie) with a long custom expiry
C. A public bucket
D. A presigned URL re-issued every day for 30 days

**Q27.** A Lambda function (1-hour role session) generates presigned URLs with a 12-hour expiry. Users report `403` after about an hour. Why?
A. The bucket policy changed
B. Temporary credentials expire with the role session; a presigned URL can never outlive its signer's credentials — refresh the session or use CloudFront signed URLs
C. S3 caps presigned URLs at one hour
D. Block Public Access blocked them

**Q28.** Static website content sits in a bucket encrypted with **SSE-KMS** and must be reachable by anonymous visitors. What is required?
A. A bucket policy with `Principal: "*"` for `s3:GetObject`
B. A CloudFront distribution with **OAC** (OAI doesn't support SSE-KMS)
C. SSE-C encryption
D. Presigned URLs embedded in `index.html`

**Q29.** Account A's role must read objects in account B's bucket. What is required?
A. A bucket policy in B granting the role, **and** an identity policy in A allowing `s3:GetObject` (plus `kms:Decrypt` on the key policy for SSE-KMS)
B. Only the bucket policy in B
C. Only an IAM policy in A
D. Make the bucket public

**Q30.** You must reject all non-TLS access to a bucket. What do you add?
A. A bucket-policy `Deny` when `aws:SecureTransport` is `false`
B. Block Public Access
C. A CORS rule
D. Presigned URLs for every object

**Q31.** Which statement about **MFA Delete** is true?
A. Any IAM administrator can enable it
B. Only the **root user** can enable it; it applies to versioned buckets and requires the `x-amz-mfa` header for permanent version deletions and versioning changes; it isn't available in the console
C. It encrypts objects with an MFA code
D. It replaces Object Lock for compliance

---

## 18. Answer key with explanations

**A1 — B.** Hot week in Standard, then Standard-IA for the occasional reads, then Glacier FR for the retention tail (hours-level restore fits), expire at 2 years. Deep Archive at 7 days is too aggressive (occasional reads days 8–30) and C skips the cheap IA step.

**A2 — B.** IA classes bill a **128 KB minimum object size**; 50 KB objects are billed as 128 KB, erasing the saving, and per-object request costs dominate. Aggregate small objects or stay in Standard.

**A3 — B.** Intelligent-Tiering is exactly for unknown/changing patterns; auto-tiering with no retrieval fees.

**A4 — A.** Derived data can be re-created, so single-AZ risk is acceptable; ms access required → One Zone-IA.

**A5 — B.** Deep Archive is the lowest-cost class and its 12-hour Standard restore meets the SLA; Object Lock Compliance gives immutability; lifecycle expiry must be ≥ retention.

**A6 — B.** Glacier Instant Retrieval is designed for ~quarterly access with millisecond retrieval and a 90-day minimum.

**A7 — B.** A single rule cannot chain before the source class's minimum duration. Glacier IR has 90 days → Deep Archive at ≥ day 120 (30 + 90). Two rules can express it, with minimum-duration charges.

**A8 — B.** Expiration only adds a delete marker; noncurrent versions keep billing. Add `NoncurrentVersionExpiration` and delete-marker cleanup.

**A9 — B.** Orphaned multipart parts from failed uploads are billed but invisible in listings — add `AbortIncompleteMultipartUpload`.

**A10 — B.** Lifecycle is asynchronous with no completion guarantee; Inventory + Batch Operations enforces the deadline.

**A11 — A.** Storage Class Analysis is the only tool that recommends Standard→Standard-IA transitions and the transition age.

**A12 — B.** Batch Operations copies existing objects with a new class immediately; lifecycle would need time and applies to all eligible objects.

**A13 — C.** Bulk restores finish within 48 hours and are the cheapest tier for Deep Archive.

**A14 — B.** Provisioned capacity guarantees Expedited capacity for Glacier FR; without it, Expedited requests can be throttled.

**A15 — B.** Replication copies objects, not lifecycle configurations; replicas follow the destination bucket's lifecycle. (Set it, or the DR bucket silently keeps everything in Standard.)

**A16 — B.** Object Lock retention blocks permanent deletion; lifecycle cannot override it, so the data stays (and bills) until retention ends. Align expiration with lock periods.

**A17 — B.** Directory bucket lifecycle = expiration + abort incomplete MPU only; no transitions; lifecycle principal requires `s3express:CreateSession` ReadWrite.

**A18 — B.** The September 2024 default blocks transitions of objects smaller than 128 KB to any class; a size filter can override it, but the economics rarely justify it.

**A19 — A.** Standard-IA is multi-AZ, ms latency, ~monthly access, with the 30-day minimum — fits. One Zone-IA fails the AZ-resilience requirement.

**A20 — B.** Setting `INTELLIGENT_TIERING` as the bucket default means every new object starts in the tiered class; no lifecycle rules needed.

**A21 — B.** A customer managed KMS key is requested with `x-amz-server-side-encryption: aws:kms` plus `x-amz-server-side-encryption-aws-kms-key-id` pointing at the key ARN; the KMS key policy then governs who can decrypt. A is SSE-C, C is SSE-S3.

**A22 — B.** For SSE-KMS, encryption request headers on GET/HEAD cause `HTTP 400 BadRequest`; the read is authorised by the caller's `kms:Decrypt` permission, not by re-sending headers.

**A23 — B.** SSE-C means the key travels with every request over HTTPS, S3 never stores it, and since April 2026 new general-purpose buckets block SSE-C until `BlockedEncryptionTypes: NONE` is set. SSE-C cannot be a bucket default, and the console does not support it.

**A24 — B.** “Public” means anonymous principals can act; only a resource policy (or ACL) with `Principal: "*"` can grant that. IAM identity policies apply to authenticated principals and never make a bucket public.

**A25 — B.** Account-level BPA is enforced and the most restrictive setting wins, so the bucket cannot be made publicly readable. The production pattern is CloudFront + OAC over a private bucket (with your own domain and TLS).

**A26 — B.** A presigned URL with IAM user credentials maxes out at 7 days (shorter with temporary credentials). For 30-day access use CloudFront signed URLs/cookies, or a cross-account IAM role the partner can assume.

**A27 — B.** A presigned URL expires at whichever comes first: its TTL or the signer's credential lifetime. Role sessions (and rotated container credentials) are typically 1–6 h, so regenerate URLs with fresh credentials or move to CloudFront signed URLs.

**A28 — B.** SSE-KMS doesn't permit anonymous access, and OAI doesn't support SSE-KMS. Use CloudFront with OAC and an ACM certificate; the bucket keeps BPA on and stays private.

**A29 — A.** Cross-account access requires an allow on both sides: the resource policy in the owning account and the identity policy in the caller's account; for SSE-KMS the key policy must also allow the role.

**A30 — A.** Deny when `aws:SecureTransport` is false (i.e. send a `Deny` with `Bool: {"aws:SecureTransport": "false"}`). BPA, CORS and presigned URLs do not enforce TLS.

**A31 — B.** MFA Delete is enabled/disabled only by the root user on a versioned bucket; permanent deletion of versions and versioning-state changes require the `x-amz-mfa` header over HTTPS, and the console can't do it.

---

## 19. Rapid recall — numbers and rules

| Fact | Value |
|---|---|
| Storage classes | 6 current + Express One Zone + Outposts; RRS deprecated |
| IA / One Zone-IA minimum duration | 30 days |
| Glacier IR / Glacier FR minimum duration | 90 days |
| Glacier Deep Archive minimum duration | 180 days |
| Minimum billable size (IA, One Zone-IA, Glacier IR) | 128 KB |
| Glacier FR / Deep Archive per-object overhead | 40 KB (32 KB class rate + 8 KB Standard rate) |
| Default minimum transition size (since Sept 2024) | 128 KB |
| Intelligent-Tiering tiering thresholds | 30 days (IA), 90 days (Archive Instant); optional Archive Access ≥90 days, Deep Archive Access ≥180 days, both max 730 days |
| Intelligent-Tiering minimum monitored object size | 128 KB |
| Lifecycle rules per bucket | 1,000 (not adjustable) |
| Lifecycle rule ID max length | 255 characters |
| `NewerNoncurrentVersions` range | 1–100 (needs a filter; both thresholds must be exceeded) |
| Restore request rate quota | 1,000 transactions/s per account per Region; 1–2 PB/day throughput |
| Glacier FR restore | Expedited 1–5 min; Standard 3–5 h; Bulk 5–12 h (free) |
| Deep Archive restore | Standard ≤12 h; Bulk ≤48 h; **no Expedited** |
| Provisioned capacity unit | ≥3 Expedited retrievals per 5 min, 300 MB/s |
| Lifecycle config propagation | minutes; rule policy updates up to 15 min |
| Lifecycle event timestamp | midnight UTC of the day the action occurred |
| Lifecycle actions per object per day | 1 |
| Storage Class Analysis observation period | 30+ days; results visible 24–48 h after setup; only Standard→Standard-IA |
| S3 Inventory delivery | daily or weekly; first report ≤48 h; CSV/ORC/Parquet |
| Storage Lens metrics retention | 15 months |
| Durability | 11 nines (all classes except deprecated RRS = 99.99 %) |
| Availability | Standard 99.99 %; IA/Glacier IR/IT 99.9 %; One Zone-IA 99.5 %; Express One Zone 99.95 % |
| SSE-C headers per request | 3 (`...customer-algorithm: AES256`, `...customer-key` base64, `...customer-key-MD5` base64) — on **every** PUT and **every** GET/HEAD, HTTPS only |
| SSE-KMS GET/HEAD | **no** encryption headers (sending them → HTTP 400 BadRequest) |
| SSE-KMS permissions | PUT `kms:GenerateDataKey`; GET `kms:Decrypt`; multipart needs both |
| SSE-C default (new buckets) | blocked since **April 2026** until `PutBucketEncryption` sets `BlockedEncryptionTypes: NONE` |
| Public bucket | resource policy or ACL granting anonymous principals (`Principal: "*"` / `AllUsers`); never the default |
| Block Public Access settings | 4 (`BlockPublicAcls`, `IgnorePublicAcls`, `BlockPublicPolicy`, `RestrictPublicBuckets`); applied at org/account/bucket/access point, **most restrictive wins** |
| New-bucket defaults (since April 2023) | all four BPA settings on + ACLs disabled (Object Ownership = bucket owner enforced) |
| Presigned URL maximum | **7 days** with IAM user credentials; console 1 min–12 h; temporary credentials → URL dies with the session |
| S3 website endpoint | requires public read and has **no HTTPS**; an SSE-KMS site must use CloudFront + **OAC** |
| MFA Delete | root account only to enable/disable; versioned buckets; `x-amz-mfa` header; not available in the console |
| Cross-account S3 access | caller's identity policy **and** bucket/access-point policy must allow; KMS key policy too for SSE-KMS |
| Server access log destination bucket | must use **SSE-S3**, not SSE-KMS, or log objects may be undeliverable/unreadable |
| Requester Pays | requester pays request + transfer (owner pays storage); anonymous access disallowed; `x-amz-request-payer` header |

### Phrases to use in written answers

- “Transition the **current** versions down the waterfall, expire **noncurrent** versions, and clean up **expired delete markers and incomplete multipart uploads**.”
- “Never tier objects smaller than **128 KB** — aggregate them first.”
- “Lifecycle is **asynchronous**; for a hard deletion deadline use **Inventory + Batch Operations**.”
- “Glacier Instant Retrieval is **not archived** — no restore; Glacier Flexible Retrieval and Deep Archive are.”
- “Intelligent-Tiering is the answer when access patterns are **unknown**, and it has **no retrieval fees**.”
- “One Zone-IA only for **re-creatable** data or replicas.”
- “Billing follows the **rule-satisfaction time**, not the physical move — except transitions into Intelligent-Tiering.”

---

## 20. S3 basics the same questions assume

| Topic | Exam-relevant fact |
|---|---|
| Durability | 11 nines (except RRS) — no backup class exists for durability; classes differ in cost/availability/retrieval |
| Consistency | S3 is strongly consistent for new PUTs and overwrites/deletes (read-after-write) |
| Versioning | Must be enabled to keep versions; cannot be “turned off”, only suspended; Object Lock requires it |
| Object Lock | WORM; Governance vs Compliance modes + legal hold; needs versioning at bucket creation |
| Replication | CRR/SRR, S3 RTC 15-min SLA, delete-marker replication option, Batch Replication for existing data |
| Multipart upload | Recommended for objects >100 MB; max object 5 TB; up to 10,000 parts; incomplete parts bill storage |
| Request rate | Scales per partitioned prefix (thousands of PUT and GET requests/s per prefix) — no need to randomise prefixes in modern S3 |
| Encryption | SSE-S3, SSE-KMS, DSSE-KMS, SSE-C; S3 Bucket Keys reduce KMS costs; transitions keep encryption — exact headers, the GET/HEAD asymmetry and the April 2026 SSE-C default are in §21 |
| Access control | Bucket policies, IAM, ACLs (mostly disabled now), Block Public Access, Access Points, Object Ownership — public-bucket, presigned-URL and static-site mechanics are in §22 |
| Performance | Transfer Acceleration for long-distance uploads; Mountpoint/S3A clients for file-like access |

---

## 21. S3 server-side encryption — SSE-S3, SSE-KMS, DSSE-KMS and SSE-C

Encryption is configured per object (or as the bucket default) and is **independent of the storage class** — a lifecycle transition keeps the object encrypted. But the *headers* and *permissions* differ per option, and that mix-up shows up both in real API calls and in exam questions.

### 21.1 Terminology — two different “customer” keys

| Option | Who holds the key material | Write headers | Read headers | KMS permissions |
|---|---|---|---|---|
| **SSE-S3** (default) | S3-managed AES-256 key | `x-amz-server-side-encryption: AES256` (optional) | none | — |
| **SSE-KMS, AWS managed key** (`aws/s3`) | AWS KMS, key you don't own | `x-amz-server-side-encryption: aws:kms` | **none** | PUT `kms:GenerateDataKey`; GET `kms:Decrypt` |
| **SSE-KMS, customer managed key** | AWS KMS key **you** created and own | `aws:kms` + `x-amz-server-side-encryption-aws-kms-key-id: <ARN>` | **none** | same |
| **DSSE-KMS** | KMS, double layer | `x-amz-server-side-encryption: aws:kms:dsse` | **none** | same |
| **SSE-C** | **you** — the key travels with each request | the three `x-amz-server-side-encryption-customer-*` headers | **the same three headers** | none (no KMS) |

**Terminology trap:** *customer managed key* = a **KMS** key you own (SSE-KMS, `aws:kms` + optional key-id). *Customer-provided key* = **SSE-C**, where you send the key material itself. The headers are completely different.

### 21.2 Exact headers

**SSE-KMS with a customer managed key:**

```http
x-amz-server-side-encryption: aws:kms
x-amz-server-side-encryption-aws-kms-key-id: arn:aws:kms:eu-west-1:123456789012:key/abcd-...
x-amz-server-side-encryption-context: {"team":"analytics"}       # optional
x-amz-server-side-encryption-bucket-key-enabled: true           # optional
```

- Omit the key-id header → S3 uses the **AWS managed key `aws/s3`**.
- The KMS key must be **symmetric** and in the **same Region** as the bucket; pass the full ARN (an alias resolves in the *requester's* account).
- AWS managed `aws/s3` objects cannot be shared cross-account; a customer managed key with a key policy can.

**SSE-C (three headers, always together):**

```http
x-amz-server-side-encryption-customer-algorithm: AES256
x-amz-server-side-encryption-customer-key: <base64 of the 256-bit key>
x-amz-server-side-encryption-customer-key-MD5: <base64 of the MD5 digest of the key>
```

- **HTTPS is mandatory.** S3 stores only a salted HMAC to validate future requests, never the key — **lose the key, lose the object**.
- No KMS permissions involved, but every reader must be given the key itself; AWS managed services can't natively decrypt SSE-C objects.

### 21.3 The GET/HEAD asymmetry (the #1 gotcha)

| Encryption | GET / HEAD |
|---|---|
| SSE-S3 | nothing |
| **SSE-KMS** (either key type) | **Send nothing.** Encryption request headers on GET/HEAD return `HTTP 400 BadRequest`; decryption is authorised by `kms:Decrypt`. |
| **SSE-C** | **Send the same three `customer-*` headers.** Without them the read fails. |

### 21.4 Copy, multipart and POST

| Operation | What changes |
|---|---|
| `CopyObject`, SSE-C **source** | add `x-amz-copy-source-server-side-encryption-customer-algorithm` + `-customer-key` + `-customer-key-MD5` |
| `CopyObject`, **destination** SSE-C | the normal three `customer-*` headers |
| `CopyObject`, destination SSE-KMS | `aws:kms` (+ optional key-id) |
| Default copy behaviour | the copy is **not** re-encrypted unless you ask; SSE headers apply to the destination only |
| Multipart **SSE-C** | headers on `CreateMultipartUpload` **and on every `UploadPart`** (same key); also on `CompleteMultipartUpload` when a bucket policy enforces SSE-C |
| Multipart **SSE-KMS** | headers on `CreateMultipartUpload` only |
| `POST` Object | same values as **form fields** instead of headers |

### 21.5 CLI equivalents

```bash
# SSE-KMS, customer managed key, bucket key on
aws s3api put-object --bucket b --key k --body f \
  --server-side-encryption aws:kms \
  --sse-kms-key-id arn:aws:kms:eu-west-1:123456789012:key/abcd-... \
  --bucket-key-enabled

# SSE-C upload and download (same three flags both times)
aws s3api put-object --bucket b --key k --body f \
  --sse-customer-algorithm AES256 --sse-customer-key "$KEY_B64" --sse-customer-key-md5 "$KEY_MD5_B64"
aws s3api get-object --bucket b --key k out.bin \
  --sse-customer-algorithm AES256 --sse-customer-key "$KEY_B64" --sse-customer-key-md5 "$KEY_MD5_B64"
```

### 21.6 Enforcing encryption with a bucket policy

Condition keys:

- `s3:x-amz-server-side-encryption` — `AES256`, `aws:kms`, `aws:kms:dsse`
- `s3:x-amz-server-side-encryption-aws-kms-key-id` — pin a specific key ARN
- `s3:x-amz-server-side-encryption-customer-algorithm` — `AES256` to require SSE-C

```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEquals": { "s3:x-amz-server-side-encryption": "aws:kms" },
    "Null": { "s3:x-amz-server-side-encryption-aws-kms-key-id": "true" }
  }
}
```

### 21.7 Operational facts and the April 2026 SSE-C change

- Every new object has been **SSE-S3 by default since 5 January 2023**; changing a bucket's default to KMS does **not** re-encrypt existing objects (use S3 Batch Operations Copy).
- **Since April 2026, SSE-C is disabled by default for new general-purpose buckets** (and for existing buckets in accounts with no SSE-C objects). SSE-C writes get **403 AccessDenied** until you explicitly enable it with `PutBucketEncryption` and `BlockedEncryptionTypes: NONE`. SSE-C also **cannot be used from the S3 console** (API/SDK/CLI only).
- **SSE-C is not supported on directory buckets (S3 Express One Zone)** — those support SSE-S3/SSE-KMS only. **S3 on Outposts** supports SSE-S3/SSE-C but **not SSE-KMS**.
- SSE-KMS is subject to **KMS request-rate quotas**; **S3 Bucket Keys** cut KMS calls dramatically (up to 99 % of request traffic) — the main lever for SSE-KMS cost and throttling.
- SSE-KMS/DSSE-KMS support an optional **encryption context** (`x-amz-server-side-encryption-context`); S3 automatically adds the default `aws:s3:arn` context (object ARN, or bucket ARN when a Bucket Key is used).
- Encryption is **orthogonal to the storage class**: transitions to IA/Glacier keep the encryption unchanged.

### 21.8 Encryption traps (exam)

1. “Customer managed key” → **KMS** headers (`aws:kms` + optional key-id), never the `customer-*` headers.
2. Sending KMS headers on GET/HEAD → **HTTP 400**, not “more secure”.
3. SSE-KMS without a key-id → **AWS managed `aws/s3`**, which cannot be used cross-account.
4. SSE-C → the key on **every** read, HTTPS only, and (since April 2026) explicitly enabled on the bucket.
5. SSE-C in the console / on directory buckets → **not supported**.
6. KMS key must be **symmetric** and in the **same Region**; multi-Region keys are treated as single-Region by S3.
7. Lifecycle transitions **preserve** encryption — no re-encryption step needed.

---

## 22. S3 access control and security — public buckets, Block Public Access, presigned URLs and static hosting

The book has treated S3 as a storage system; this section is the access-control layer that SAP-C02 tests alongside it (Domain 1 for organisational/cross-account complexity, Domain 2 for secure designs). Almost every S3 security question reduces to *who may call which action on which resource*, and *how do I share exactly one object without opening the bucket?*

### 22.1 Who can access a bucket — the model

| Mechanism | Applies to | Typical use |
|---|---|---|
| **IAM identity policy** | authenticated principals (users, roles, federated identities) | in-account permissions for services/roles |
| **Bucket policy** (resource-based) | the bucket and its objects | cross-account access, public access, TLS/VPC/encryption enforcement |
| **Access point policy** | requests made **through** that access point | per-application or per-dataset access, VPC-only endpoints |
| **ACLs** | bucket/object (legacy) | legacy only — disabled on new buckets |
| **Session policies, permissions boundaries, SCPs** | — | guardrails that can only *reduce* permissions |

Rules to internalise:

- **Buckets are private by default.** Only IAM principals in the owning account with an explicit `Allow` can access them.
- **Anonymous requests have no identity**, so they can only be allowed by a **resource-based policy or ACL** — never by an IAM identity policy. `Principal: "*"` (or the `AllUsers` / `AuthenticatedUsers` ACL groups) = “anyone”, subject to conditions and Block Public Access.
- **Same-account:** either an identity policy or a resource policy can allow access.
- **Cross-account:** both sides must allow — the caller's identity policy **and** the bucket policy (and the KMS key policy if SSE-KMS). Exception: anonymous requests need no identity policy, only `Principal: "*"` in the resource policy.
- **An explicit `Deny` in any applicable policy wins** — IAM policy, bucket policy, SCP, permissions boundary or session policy.
- A bucket policy can enforce behaviour on the owner too (e.g. deny non-TLS or unencrypted uploads), but it **cannot block S3 Lifecycle** (see §9.6).
- IAM, S3 and KMS are separate services: for SSE-KMS objects the caller also needs `kms:GenerateDataKey`/`kms:Decrypt`, and the **KMS key policy** must allow them.

### 22.2 Object Ownership and ACLs

| Setting | ACLs | Object ownership | Uploads accepted |
|---|---|---|---|
| **Bucket owner enforced** (default) | disabled — ACLs never affect access | bucket owner owns everything | no ACL, or `bucket-owner-full-control` |
| **Bucket owner preferred** | enabled | with the `bucket-owner-full-control` canned ACL the bucket owner owns the object | all |
| **Object writer** | enabled | the writing account owns the object and can re-grant via ACL | all |

- New buckets are created with **Bucket owner enforced** and **ACLs disabled**.
- With ACLs disabled, `PUT` requests carrying any other ACL fail with **`400 AccessControlListNotSupported`** (the `bucket-owner-full-control` ACL is still accepted).
- Re-enabling ACLs restores previously stored ACLs; objects written while ACLs were disabled stay owned by the bucket owner.
- **Recommendation:** leave ACLs disabled and manage access with bucket policies/IAM/access points. ACLs exist in exam questions mostly as legacy: `public-read`, `AllUsers`, `AuthenticatedUsers`.
- **Cross-account replication:** with Bucket owner enforced, replicas are owned by the **destination** bucket owner (no owner-override permission needed).

### 22.3 What a “public bucket” actually is

An S3 bucket is **public** when anonymous (unauthenticated) principals are granted access — typically by a bucket policy like:

```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

Other routes: an **access point policy** with a public grant, or a **public ACL** (legacy; both are disabled/nonexistent on modern buckets).

What S3 counts as “public” when enforcing Block Public Access: a bucket policy is treated as public **unless** it restricts access to *fixed values* — a specific AWS principal/service principal, fixed CIDR via `aws:SourceIp`, `aws:SourceArn`, `aws:SourceVpc`, `aws:SourceVpce`, `aws:SourceAccount`, `aws:SourceOwner`, or similar. Wildcards and policy variables make it public; **very broad IP ranges** (e.g. `0.0.0.0/1`, anything broader than `/8` IPv4 excluding RFC1918) are also evaluated as public. A **VPC-origin access point is never considered public**, whatever its policy says.

Important nuances:

- **Public read ≠ public listing.** `s3:GetObject` (object ARN `/*`) and `s3:ListBucket` (bucket ARN) are separate grants; a public-read bucket without `ListBucket` still serves any object whose key is known or guessable.
- Public access is **unauthenticated and unbilled to the requester** — the bucket owner pays requests and transfer (unless **Requester Pays** is on, see §22.11).
- Block Public Access **can prevent** a bucket from ever becoming public: with `BlockPublicPolicy` on, the `PutBucketPolicy` call itself fails.
- Public buckets are the classic S3 data-leak vector; most “we exposed data” exam scenarios end with “enable Block Public Access, audit with Access Analyzer, and serve through CloudFront + OAC”.

**Detecting public exposure:**

| Tool | What it gives you |
|---|---|
| `GetBucketPolicyStatus` | `IsPublic: true/false` for the effective policy |
| **IAM Access Analyzer for S3** | findings for buckets public to the internet or shared with other accounts; can fix, or archive with recorded intent |
| **AWS Trusted Advisor** | bucket permission checks |
| **AWS Security Hub** | S3 controls, e.g. foundational best practice control S3.8 (BPA on) |
| **AWS Config** | rules such as `s3-bucket-public-read-prohibited`, `s3-bucket-public-write-prohibited` |

### 22.4 Block Public Access (BPA)

Four independent settings, enforceable at **organisation → account → bucket → access point**:

| Setting | Effect when true |
|---|---|
| `BlockPublicAcls` | rejects `PutBucketAcl`/`PutObjectAcl`/`PutObject` requests that **include** a public ACL |
| `IgnorePublicAcls` | ignores all public ACLs (stored, but not honoured) |
| `BlockPublicPolicy` | rejects `PutBucketPolicy`/`PutAccessPointPolicy` if the policy allows public access |
| `RestrictPublicBuckets` | if a bucket has a public policy, only AWS service principals and in-account principals may access it — no cross-account access |

Enforcement model:

- **Most restrictive combination wins** across org/account/bucket/access point. A bucket-level “off” cannot loosen an organisation-level or account-level “on”.
- **Organisation-level BPA policies** are all-or-none (you can’t enable individual settings) and **override account settings**; account-level settings inherit org policies.
- **Access point BPA settings can only be set when the access point is created** — they can’t be changed later; they act as a passthrough to the underlying bucket.
- Configure BPA at the **account** level for `BlockPublicPolicy`: a user who can edit a bucket policy could otherwise turn off bucket-level BPA, but account-level blocking prevents the public policy anyway.
- **BPA doesn’t rewrite existing policies/ACLs** — turn it off and a stored public policy becomes effective again.
- **Defaults:** since April 2023, new buckets come with **all four settings on and ACLs disabled**. AWS recommends all four at the account level (Security Hub S3.8).

```bash
aws s3api put-public-access-block --bucket my-bucket \
  --public-access-block-configuration \
  BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

### 22.5 Presigned URLs — how to share a private object

A **presigned URL** is a URL signed with the credentials of a principal that already has permission. The signature covers the **bucket, key, HTTP method, expiry and signed headers**, so the URL is valid only for that object/operation. S3 treats the request as coming from the **signer** — the bucket stays private, no policy change is needed, and **Block Public Access is irrelevant** because the request is authenticated.

Facts to memorise:

- **Permissions are the signer's** — if the signer lacks `s3:GetObject`, the URL can never work. For **SSE-KMS** objects the signer also needs `kms:Decrypt` (a very common 403). For **SSE-C** objects the encryption headers must be supplied with every request, so presigned SSE-C URLs are programmatic-only.
- **It is a bearer token**: anyone who has it can use it, repeatedly, until expiry — there is no per-URL revocation.
- **Expiry limits:**

| Signed with | Maximum validity |
|---|---|
| S3 console | 12 hours (minimum 1 minute) |
| CLI/SDK + long-term IAM user credentials | **7 days** |
| CLI/SDK + temporary credentials (role, STS, instance profile, container) | never beyond the credential lifetime — role default 1 h, containers ~1–6 h, instance profiles ≤ ~6 h |

- Expiry is checked **when the request starts**: a download already in progress continues past expiry; a restart fails.
- **Uploads:** presigned `PUT` lets a browser/mobile client upload a specific key without AWS credentials (an existing object at that key is replaced); for large files presign each `UploadPart`. SigV4 presigned uploads support checksum headers.
- **Hardening without changing the model:** short TTLs, `s3:signatureAge` (reject signatures older than N minutes), network restrictions (`aws:SourceIp` / `aws:SourceVpc`) on the signer or bucket, and an explicit `Deny` for the signer when sharing should stop.
- Presigned URL **does not bypass** Object Lock/legal hold, versioning, encryption, or any policy condition — the signed request is evaluated like any other request.

**Troubleshooting presigned URLs:**

| Error | Cause |
|---|---|
| `403 AccessDenied` | signer lacks the permission, or a bucket policy explicitly denies |
| `403` on SSE-KMS object | signer lacks `kms:Decrypt` (or the key policy denies it) |
| `403 Request has expired` / `ExpiredToken` | TTL passed, or the signer's credentials expired first |
| `403 SignatureDoesNotMatch` | clock skew, proxy rewriting headers, or method/headers/region differ from what was signed |
| `403 HeadersNotSigned: if-range` | `Range` was signed but `If-Range` wasn't — add it to the signed headers |

**Presigned URLs vs CloudFront signed URLs** — both grant temporary access to private content, but different tools:

| | **S3 presigned URL** | **CloudFront signed URL / cookies** |
|---|---|---|
| Signing credential | IAM user/role credentials (SigV4) | CloudFront key pair (no AWS credentials exposed) |
| Max lifetime | 7 days (less for temporary credentials) | arbitrary expiry set by the signer |
| Scope | one object / one operation | path/prefix patterns, many files (cookies) |
| Caching/CDN | none | global edge cache, custom domain, TLS, WAF |
| Use when | one-off/short-lived direct S3 access | media sites, many users/objects, long sessions |

### 22.6 Static website hosting and CloudFront OAC

| | **S3 website endpoint** | **CloudFront + OAC** (recommended) |
|---|---|---|
| Bucket visibility | **must be public** | **private** |
| HTTPS | **no** (HTTP only on the S3 endpoint) | yes, with ACM certificate |
| Custom domain | only via CloudFront/Route 53 in front | native (alternate domain names) |
| Caching / WAF / geo | none | yes |
| SSE-KMS origin | **cannot work anonymously** | works — OAC signs requests to S3 |

Key facts:

- The S3 website endpoint (`bucket.s3-website-<region>.amazonaws.com`) only serves content if the bucket policy **grants public `s3:GetObject`**; it is HTTP-only and cannot do TLS on the AWS-provided endpoint.
- Website hosting configuration adds **index document**, **error document** and **redirect rules** (object-level redirect metadata or routing rules).
- With **SSE-KMS** encryption, anonymous access is impossible, so a CloudFront distribution with **Origin Access Control (OAC)** is mandatory — the legacy **OAI does not support SSE-KMS**.
- AWS also recommends **Amplify Hosting** for static sites on S3 (managed CDN + HTTPS) as the simplest deployment path.
- CORS configuration on the bucket is needed if browsers call S3 (or presigned URLs) cross-origin — a classic detail for browser-upload designs.

**Exam trigger:** “static website, bucket must stay private, HTTPS, custom domain” → **CloudFront + OAC**. “Public bucket and website endpoint” only for simple, non-sensitive, internal/test scenarios.

### 22.7 Cross-account and organisation access patterns

- **Grant pattern A — bucket policy.** Allow the external account/role directly in the bucket policy; the caller also needs an IAM identity policy allowing `s3:GetObject` (both sides).
- **Grant pattern B — access points.** Create an access point with its own policy for the partner/application; optionally a **VPC-origin** access point for private access from a VPC (never considered public). Access point policy works **in conjunction with** the bucket policy.
- **Grant pattern C — assume a role.** The partner assumes a role in your account that has S3 permissions (S3 itself doesn't need a cross-account bucket policy if the role lives in the bucket owner's account, but the trust policy must allow the partner).
- **Restrict to the organisation:** condition on `aws:PrincipalOrgID` (or explicitly `Deny` principals without it) to prevent access from accounts outside your AWS Organisation.
- **Guardrails:** SCPs (and, where available, AWS Organizations resource control policies) can deny actions/principals across all member accounts.
- **Cross-account replication:** the destination bucket must have a bucket policy allowing the replication role; with ACLs disabled, replicas are owned by the destination bucket owner.
- **Access points + Multi-Region Access Points:** an MRAP gives one global endpoint routing to buckets in multiple Regions, using S3 replication for failover — the answer for “global endpoint, multi-Region, active-active reads”.

Example — read access for one partner account, org-restricted:

```json
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::111122223333:role/partner-read"},
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::my-bucket/shared/*",
  "Condition": {"StringEquals": {"aws:PrincipalOrgID": "o-abc123"}}
}
```

### 22.8 Policy conditions worth memorising

| Condition key | Use |
|---|---|
| `aws:SecureTransport` | deny non-TLS (`"false"`) — the standard “TLS required” policy |
| `aws:SourceVpce` / `aws:SourceVpc` | allow only through a specific VPC endpoint |
| `aws:PrincipalOrgID` | restrict to accounts in your Organisation |
| `s3:prefix`, `s3:max-keys` | limit what `ListBucket` can list |
| `s3:x-amz-acl` | block/allow specific canned ACLs (legacy) |
| `s3:x-amz-server-side-encryption`, `…-aws-kms-key-id` | enforce an encryption type/key on upload |
| `s3:x-amz-server-side-encryption-customer-algorithm` | require SSE-C |
| `s3:ExistingObjectTag`, `s3:RequestObjectTagKeys` | tag-based access control |
| `s3:signatureAge`, `s3:authType` | presigned-URL limits (e.g. reject signatures older than 10 min) |
| `s3:DataAccessPointArn`, `s3:AccessPointNetworkOrigin` | restrict which access point / network origin is used |
| `s3:resourceAccount` | limit to buckets owned by a specific account |
| `aws:MultiFactorAuthPresent` | require MFA for sensitive operations |

TLS enforcement example:

```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"],
  "Condition": {"Bool": {"aws:SecureTransport": "false"}}
}
```

### 22.9 Protection features — versioning, MFA Delete, Object Lock

- **Versioning** keeps every overwrite/delete as a history; a delete in a versioned bucket adds a **delete marker** and the data still exists (§7.2). It is the first line of defence against accidental or malicious deletion and ransomware.
- **MFA Delete**: **only the root user** can enable or disable it; it requires **versioning**; permanent deletion of a version or a change to versioning state requires the `x-amz-mfa` header (serial + code) over HTTPS; the **console cannot perform these operations**. It protects the bucket’s version history even from most credential compromise.
- **Object Lock** (§11.3): WORM retention (Governance or Compliance) and legal hold; requires versioning; deletion blocked until retention/legal hold ends. The compliance pattern is Object Lock + versioning + Glacier/Deep Archive + lifecycle aligned with retention.
- **Replication** (§11.2) for DR/CRR; combine with versioning and cross-account replicas for ransomware resilience.

### 22.10 Logging and audit

| Tool | What it records | Notes |
|---|---|---|
| **Server access logs** | every request against the bucket | best-effort, delivered in batches (not real-time); target bucket needs a policy; **if the target bucket uses SSE-KMS, log objects may be unreadable — use SSE-S3 for the logging destination** |
| **CloudTrail** | management events (bucket config) always; **data events** (object-level API calls) opt-in and billed | the audit trail for “who deleted/changed what” |
| **IAM Access Analyzer for S3** | public/cross-account exposure findings | archive with recorded intent when public access is intentional |
| **Security Hub / Trusted Advisor / Config** | compliance checks (BPA, public read/write, encryption) | preventive/detective guardrails |
| **S3 Storage Lens** | usage/activity metrics per account/bucket/prefix | org-wide visibility (§13.3) |
| **CloudWatch metrics + EventBridge** | request metrics (opt-in) and object-level events | alarm on spikes, react to events |

### 22.11 Other features that show up in exam questions

- **Requester Pays** — the requester pays request + data-transfer charges; the owner always pays storage; **anonymous access is not allowed**, and requesters must send `x-amz-request-payer` (CLI `--request-payer`). Not usable as an access-log target bucket.
- **Transfer Acceleration** — uploads/downloads through CloudFront edge locations for long-distance transfers; extra charge; works with multipart uploads.
- **CORS** — bucket-level rule allowing browsers to make cross-origin requests (needed for browser-based presigned uploads and JS `fetch`).
- **Access points** — per-application endpoints + policies; **VPC-origin** access points bypass the public internet; MRAPs for global multi-Region endpoints.
- **S3 Access Grants** — newer identity-based model that maps corporate identities (IAM Identity Center) to bucket/prefix permissions without per-user policies.
- **Directory buckets (Express One Zone)** — session-based authorization, bucket-level-only policies, ACLs unavailable, **Block Public Access fully enabled and immutable** (§3.3).
- **SigV4** is the signing scheme for S3 API requests and presigned URLs; SigV2 is legacy.

### 22.12 If you remember only this

1. **Private is the default**; “public” = a resource policy/ACL granting `Principal: "*"` — and **BPA** is what prevents that.
2. **Block Public Access has 4 settings, applies at org/account/bucket/AP, most restrictive wins**, new buckets since April 2023, and it never rewrites existing policies.
3. **Presigned URLs work on private buckets (even with BPA on)**; permissions are the signer's; max **7 days** with IAM user creds, shorter with temporary credentials; **no per-URL revocation**.
4. **Static website on S3 = public + HTTP-only**; the production pattern is **CloudFront + OAC + private bucket**, mandatory for SSE-KMS.
5. **Cross-account needs both sides** (identity + resource policy, plus KMS key policy for SSE-KMS).
6. **MFA Delete is root-only**; **Object Lock** (not MFA Delete) is the compliance WORM tool.

---

## 23. Glossary

| Term | Meaning |
|---|---|
| **Storage class** | the durability/availability/price tier assigned per object |
| **Tiering** | moving objects to cheaper classes as they age |
| **Intelligent-Tiering** | automatic per-object tiering based on access, with monitoring fee, no retrieval fees |
| **IA (infrequent access)** | Standard-IA / One Zone-IA: ms access, retrieval fee, 30-day minimum, 128 KB floor |
| **Glacier Instant Retrieval** | archive-priced class with ms `GET`, 90-day minimum |
| **Glacier Flexible Retrieval** | archived class; restore in minutes–12 h |
| **Glacier Deep Archive** | lowest-cost archived class; 12–48 h restore; 180-day minimum |
| **Restore** | creating a temporary copy of an archived object for a number of `Days` |
| **Retrieval tier** | Expedited / Standard / Bulk for restores |
| **Provisioned capacity** | purchased guaranteed capacity for Expedited Glacier FR restores |
| **Minimum storage duration** | period you pay for even if you delete/transition earlier |
| **Minimum billable object size** | 128 KB floor for IA/Glacier IR |
| **40 KB overhead** | metadata added per object in Glacier FR/Deep Archive |
| **Lifecycle configuration** | bucket-level set of rules (≤1,000) with transition/expiration actions |
| **Transition** | lifecycle action changing an object's class |
| **Expiration** | lifecycle action deleting an object/version (or creating a delete marker) |
| **Noncurrent version** | an older version kept because versioning is on |
| **Delete marker** | placeholder object created by deleting in a versioned bucket |
| **Expired object delete marker** | a delete marker with no noncurrent versions left |
| **AbortIncompleteMultipartUpload** | lifecycle action removing stuck multipart uploads |
| **Waterfall** | the allowed downhill sequence of transitions |
| **Storage Class Analysis** | analytics that recommends Standard→Standard-IA transitions |
| **S3 Inventory** | daily/weekly manifest of objects with metadata (incl. storage class, lifecycle date) |
| **S3 Storage Lens** | org-wide storage usage/activity dashboard |
| **S3 Batch Operations** | bulk actions (copy with new class, restore, delete, tag, Lambda) on existing objects |
| **Object Lock** | WORM retention/legal hold; blocks lifecycle deletion until it expires |
| **Directory bucket** | bucket type for S3 Express One Zone, single AZ |
| **SSE-S3** | default S3-managed-key encryption (`AES256`) |
| **SSE-KMS** | encryption through a KMS key — AWS managed (`aws/s3`) or customer managed |
| **Customer managed key** | a KMS key you create and own; **not** the same as a customer-provided key |
| **SSE-C** | customer-provided key: key material sent in headers with every request |
| **DSSE-KMS** | dual-layer KMS encryption (`aws:kms:dsse`) |
| **S3 Bucket Key** | bucket-level KMS key that cuts KMS request volume/cost for SSE-KMS |
| **Encryption context** | extra unencrypted key/value data bound to the KMS encryption (AAD) |
| **Public bucket** | bucket whose policy/ACL grants anonymous access (`Principal: "*"` / `AllUsers`); not a bucket “type” |
| **Block Public Access (BPA)** | four settings at org/account/bucket/access-point level that block public ACLs and policies |
| **Object Ownership** | bucket setting controlling object ownership and whether ACLs are enabled (default: bucket owner enforced) |
| **ACL** | legacy per-resource access grant list (`AllUsers`, `AuthenticatedUsers`); disabled on new buckets |
| **Bucket policy** | resource-based policy attached to a bucket — the standard tool for cross-account and public access |
| **Access point** | named network endpoint with its own policy (optionally VPC-only) in front of a bucket/dataset |
| **Presigned URL** | time-limited URL signed with a principal's credentials; a bearer token limited to one object/operation |
| **CloudFront signed URL / cookie** | CloudFront-signed access using a CloudFront key pair (no AWS credentials needed) — good for many users/long TTLs |
| **OAC (Origin Access Control)** | CloudFront mechanism for serving a **private** S3 bucket; recommended over legacy OAI and required for SSE-KMS |
| **MFA Delete** | root-only, versioned-bucket protection requiring MFA for permanent version deletion/versioning changes |
| **Requester Pays** | bucket setting where the requester pays request + transfer; owner always pays storage |
| **CORS** | bucket configuration allowing browser cross-origin requests (needed for browser presigned uploads) |
| **S3 Access Grants** | newer identity-based grant model mapping corporate identities (IAM Identity Center) to S3 prefix permissions |

---

## 24. Sources (verified November 2026)

- [Understanding and managing Amazon S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
- [Understanding S3 Glacier storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/glacier-storage-classes.html)
- [How S3 Intelligent-Tiering works](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intelligent-tiering-overview.html)
- [High performance workloads / S3 Express One Zone](https://docs.aws.amazon.com/AmazonS3/latest/userguide/directory-bucket-high-performance.html)
- [Managing the lifecycle of objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
- [Lifecycle configuration elements](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intro-lifecycle-rules.html)
- [Transitioning objects using S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-transition-general-considerations.html)
- [Expiring objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-expire-general-considerations.html)
- [Setting a Lifecycle configuration on a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-set-lifecycle-configuration-intro.html)
- [Lifecycle configuration conflicts](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-conflicts.html)
- [Examples of S3 Lifecycle configurations](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-configuration-examples.html)
- [Working with archived objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/archived-objects.html)
- [Understanding archive retrieval options](https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects-retrieval-options.html)
- [S3 analytics — Storage Class Analysis](https://docs.aws.amazon.com/AmazonS3/latest/userguide/analytics-storage-class.html)
- [Cataloging with S3 Inventory](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-inventory.html)
- [Lifecycle for directory buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/directory-buckets-objects-lifecycle.html)
- [Monitoring with CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cloudwatch-monitoring.html)
- [AWS Certified Solutions Architect – Professional exam guide (SAP-C02, PDF)](https://docs.aws.amazon.com/pdfs/aws-certification/latest/solutions-architect-professional-02/solutions-architect-professional-02.pdf)
- [SSE-C — customer-provided keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerSideEncryptionCustomerKeys.html)
- [Specifying SSE-C (headers, multipart, presigned URLs)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/specifying-s3-c-encryption.html)
- [SSE-KMS — AWS KMS keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
- [Specifying SSE-KMS (headers, key ID, bucket keys)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/specifying-kms-encryption.html)
- [Blocking public access to S3 (Block Public Access)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
- [Download and upload objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
- [Hosting a static website using Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [Controlling object ownership and disabling ACLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html)
- [Managing access to shared datasets with access points](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-points.html)
- [Deleting an object from an MFA delete-enabled bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingMFADelete.html)
- [Requester Pays buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RequesterPaysBuckets.html)
- [Restricting access to an S3 origin (CloudFront OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [Using cross-origin resource sharing (CORS)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html)
- [Bucket policies for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html)
- [Reviewing bucket access using IAM Access Analyzer for S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-analyzer.html)
