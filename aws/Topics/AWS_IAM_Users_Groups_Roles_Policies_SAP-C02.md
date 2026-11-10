# AWS IAM — users, groups, roles, policies, policy evaluation and federation
## Study book for AWS Certified Solutions Architect – Professional (SAP-C02)

> **Scope:** the complete IAM surface the exam tests — identities (root, users, groups, roles, federated identities), policy types and JSON structure, **policy evaluation logic** (union vs intersection, explicit deny), permissions boundaries, SCPs/RCPs, federation and IAM Identity Center, ABAC, audit tooling, quotas, design patterns, exam traps and practice questions.
> **Knowledge state:** November 2026, verified against current AWS documentation (links in §21).
> **Key idea:** IAM questions are almost always one of three things: **who is the principal** (authN), **which policy types apply and how they combine** (authZ), or **which guardrail limits the maximum** (SCP, permissions boundary, session policy). Get those three right and most scenarios answer themselves.
> **Especially relevant:** **IAM groups cannot be nested** — they contain only users, are not principals, and a user can belong to at most **10** groups. Everything else in this book builds on the flat model.

---

## 0. TL;DR — the answer you should be able to give in 30 seconds

| Concept | One-line rule |
|---|---|
| **Root user** | all-powerful identity of the account; use only for the handful of root-only tasks; MFA + no access keys |
| **IAM user** | long-term identity with credentials (password/access keys); belongs to groups; for legacy/exception use |
| **IAM group** | a **flat** container of users for attaching policies; **cannot be nested**, **cannot be a Principal**, max **10 groups per user** |
| **IAM role** | an identity **assumed** via STS; no long-term credentials; defined by **trust policy + permissions policy** |
| **Identity-based policy** | managed/inline policy attached to user/group/role — *grants* permissions |
| **Resource-based policy** | attached to the resource (S3, SQS, KMS, trust policy…) — *grants* to principals |
| **Permissions boundary** | sets the **maximum** for a user/role; intersection with identity policies; never grants |
| **SCP** | Organisation-level guardrail for **member accounts**; never grants; not applied to the management account |
| **Session policy** | further restricts a temporary session (AssumeRole / GetFederationToken) |
| **Evaluation** | explicit **Deny** wins anywhere; same-account identity+resource = **union**; with boundaries/SCPs/session policies = **intersection**; cross-account = **both sides** must allow |
| **Federation** | SAML 2.0 / OIDC → `AssumeRoleWithSAML` / `AssumeRoleWithWebIdentity`; workforce → **IAM Identity Center** |

**One-liner:** *Identities are users, groups (flat), roles and federated principals; permissions are granted by identity- and resource-based policies, capped by permissions boundaries, session policies and SCPs, and any explicit deny wins.*

---

## 1. Exam context — how IAM is tested in SAP-C02

- SAP-C02: **75 questions, 180 minutes, scaled score 750/1000**, multiple choice and multiple response.
- Domains (approx.): **1 — Organizational Complexity (26 %)**, **2 — New Solutions (29 %)**, **3 — Continuous Improvement (25 %)**, **4 — Migration & Modernization (20 %)**.
- IAM appears mostly in **Domain 1** (multi-account access, cross-account roles, SCPs, Identity Center, consolidated governance) and **Domain 2** (secure designs, least privilege, data perimeters), plus **Domain 3** (auditing existing permissions, Access Analyzer, credential reports).
- Typical question shapes:
  1. “Design **cross-account** access between A and B” → bucket policy/role trust + identity policy on both sides.
  2. “Restrict the whole OU to approved services” → **SCP**.
  3. “Allow a team to create users, but only with limited permissions” → **permissions boundary**.
  4. “Give 5,000 employees access to 40 accounts without creating IAM users” → **IAM Identity Center + federation**.
  5. “Why is access denied although the policy looks right?” → explicit deny from SCP/boundary/session policy, or the wrong principal (group used as principal).
  6. “Which statement about groups is true?” → **no nesting, no principal, 10-group limit**.

---

## 2. The identity model

```
AWS account
├── root user (email + password; all-powerful)
├── IAM users        ── belong to ──► IAM groups (flat; max 10 groups/user)
│                                        └── policies (managed/inline)
├── IAM roles        ── trust policy (who may assume) + permissions policy (what they get)
├── federated identities (SAML / OIDC / IAM Identity Center users)
└── policies: identity-based, resource-based, permissions boundaries, SCP/RCP, session policies
```

| Identity | Credentials | Can be a Principal? | Typical use |
|---|---|---|---|
| **Root user** | email + password, optional access keys | yes (account root ARN) | root-only tasks |
| **IAM user** | password, access keys, MFA, SSH keys, signing certs | yes | legacy apps, break-glass, CI with long-term keys (avoid) |
| **IAM group** | no credentials | **no** | attach policies to sets of users |
| **IAM role** | temporary STS credentials; trust policy | yes | AWS services, cross-account, federation, EC2/ECS/Lambda |
| **Federated principal** | external IdP IdP token / assertion | yes (`Federated`) | workforce/IdP access via roles or Identity Center |

Design rule of thumb (and exam answer): **humans → IAM Identity Center; AWS services/workloads → IAM roles; IAM users → legacy/exceptions.**

---

## 3. IAM groups — flat by design

### 3.1 The rule

- A group is **a collection of IAM users** used to attach permissions once for many users.
- **Groups cannot be nested.** They contain **only users** — not other groups, not roles. AWS docs: *“User groups can't be nested; they can contain only users, not other IAM groups.”*
- A user can belong to **multiple groups** (max **10** — hard limit, not adjustable), and the permissions are the **union** of all group + user policies.
- **No default group** exists that contains every user; if you want one (“AllUsers”), you create it and add every user manually.

### 3.2 Groups are not principals

You **cannot** put a group in a policy `Principal` — not in an S3 bucket policy, KMS key policy, SQS/SNS policy or a role trust policy. `Principal` names **authenticated** identities (accounts, users, roles, role sessions, federated principals, services). Groups define *permissions*, not authentication.

### Why — the mechanism

- A `Principal` is resolved at **request time**: AWS authenticates the caller and knows *which identity* (user ARN / role session ARN / federated principal / service) is making the request. **A group never makes a request** — it has no credentials, cannot sign an API call and cannot be authenticated.
- Groups exist only on the **identity-based policy** side: you attach a policy to a group and every member user inherits it. That is the entire semantics of a group.
- There is **no `aws:PrincipalGroup` condition key** and no way to ask “is the caller in group X?” in a policy. `aws:PrincipalArn` matches user/role/session ARNs, not groups.
- The group ARN (`arn:aws:iam::123456789012:group/Developers`) exists only for administration of the group resource — it is **not a valid principal ARN**.

### What happens if you try

The policy is **rejected when saved** with a validation error — a group ARN is not an accepted principal type (the exact error text varies by service; IAM/S3/KMS typically return `MalformedPolicy` / “Invalid principal in policy”). It is not silently ignored, but do not expect a runtime “deny” — you will not be able to persist the policy.

```json
// ❌ INVALID — group is not a principal
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::123456789012:group/Developers"},
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

### Correct patterns instead

| Goal | Correct mechanism |
|---|---|
| **Same-account** users of a group need access | Attach an **identity-based policy to the group** (this is exactly what groups are for). No `Principal` at all. |
| **Cross-account** access for an application/team | Create a **role**; the resource policy (bucket policy) lists the **role ARN** (or the role's trust policy + `sts:AssumeRole` on the caller side). |
| Delegate to another **account** without naming principals | Bucket policy with `Principal: {"AWS": "123456789012"}` (account principal = delegate to that account's admins, who then use identity policies). |
| **Workforce** access to many accounts | **IAM Identity Center** permission sets assigned to **groups** — this is an *assignment*, not a `Principal` in a resource policy. |
| **Tag-based** access for many principals | **ABAC** with `aws:PrincipalTag` / `aws:ResourceTag` (tags, not groups). |

```json
// ✅ VALID — role ARN as principal (cross-account), plus identity policy on the caller side
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::111122223333:role/app-reader"},
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

```json
// ✅ VALID — group policy is identity-based: no Principal element
{
  "Effect": "Allow",
  "Action": ["s3:GetObject"],
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

### Trust policies specifically

- A **trust policy is a resource-based policy on the role**, so it needs `Principal` — and a group can never be that principal. **A group cannot assume a role.**
- Users who belong to a group assume a role only because **the user (or its account/role) is trusted**, and the user's identity policy allows `sts:AssumeRole`. Valid trust-policy principals: account root/ID, role ARN, role session, IAM user ARN, federated principal, service principal.
- Typical cross-account trust policy: `"Principal": {"AWS": "arn:aws:iam::111122223333:role/deployer"}` or `{"AWS": "111122223333"}` (the whole account, with `aws:PrincipalOrgID`/`aws:SourceAccount` conditions for safety).

### Consequences

- To give an external party access, grant to a **role** (preferred), a specific user, or the whole account — never to a group.
- To organise people, use **roles + Identity Center groups** (Identity Center groups *are* assignable in permission-set assignments) — but note Identity Center also does not support nested groups (§11.2).

| Where a group legally appears | Where it never appears |
|---|---|
| identity-based policy attached to the group | `Principal` in S3/KMS/SQS/SNS/Lambda policies |
| role assignments in IAM Identity Center (permission sets) | role **trust policies** |
| IAM group membership administration | permissions boundaries (users/roles only) |
| | SCP/RCP element (organisations, not IAM groups) |

### 3.3 Limits

| Limit | Value | Adjustable |
|---|---|---|
| Groups per user | **10** | no |
| Groups per account | 300 (max 500) | yes |
| Managed policies per group | **10** | no |
| Inline policies (aggregate size) per group | 5,120 characters | no |
| Group name length | 128 characters | no |

### 3.4 How to “emulate” hierarchy (since nesting is impossible)

1. **Multiple groups per user** — combinations of team × role × environment, e.g. `app1-dev`, `app1-readonly`, `prod-access`; the union gives the “inherited” effect.
2. **Naming conventions + paths** (`/teams/app1/dev/`) — organise resources; paths **do not** create permission inheritance.
3. **Roles instead of groups** — for workloads and cross-account access; a role encapsulates both “who can assume” and “what they can do”.
4. **IAM Identity Center** — groups synced from AD/IdP + **permission sets**; hierarchy is modelled by permission-set + assignment design, not nested groups.
5. **AWS Organizations OUs** — these **can be nested** (up to 5 levels) and SCPs inherit down, but they apply to **accounts**, not users.
6. **Permissions boundaries + SCPs** — enforce the “ceiling” that a flat group model cannot express.

### 3.5 Groups — exam traps

- “Nest groups to mirror the org chart” → **impossible**; the answer is multiple groups/roles or an IdP + Identity Center.
- “Add the group to the bucket policy Principal” → **invalid**.
- “User needs 15 groups” → impossible; consolidate policies into managed policies or re-architect to roles.
- “Default group of all users” → does not exist.
- A role is **not** a group member (roles are assumed, not assigned to groups).

---

## 4. IAM users

- Long-term identity with **console password** (optional) and up to **2 access keys** per user (`Access keys per user: 2`, not adjustable), plus MFA devices (`MFA devices per user: 8`) and SSH/signing certificates.
- Users are **global** (IAM is not regional), but sign-in and STS calls go to regional endpoints.
- **Account limit:** 5,000 users (not adjustable) — one reason large organisations move to federation.
- Best practice: no long-term access keys for humans; use Identity Center + roles; rotate keys; MFA everywhere; no root access keys.
- **Access Advisor** shows the last-accessed services per user/role — the tool for least-privilege clean-up.
- **Credential report** is a CSV of all users, credentials, MFA status, key age — compliance evidence.
- Users can be added to groups only (not roles).

---

## 5. IAM roles

### 5.1 Structure

A role has **two policies**:
1. **Trust policy** (resource-based policy on the role) — *who* may assume it and under which conditions (`sts:AssumeRole`, or `AssumeRoleWithSAML`/`AssumeRoleWithWebIdentity`).
2. **Permissions policy** (identity-based) — *what* the assumed session can do.

The role itself has no long-term credentials: a principal calls STS, gets temporary credentials (default **1 hour**, configurable 15 min–12 h via max session duration), and acts as the **role session** (`arn:aws:sts::acct:assumed-role/role-name/session-name`).

### 5.2 Taxonomy 1 — by trust policy (who can assume the role)

The IAM console's “Create role” wizard exposes exactly these five trust types:

| Trust type (console) | Trust policy `Principal` | STS API | Typical use |
|---|---|---|---|
| **AWS service** | `{"Service": "ec2.amazonaws.com"}` | service-defined | service role for EC2, Lambda, ECS, EMR, … |
| **AWS account** | account ID / root ARN (`{"AWS": "111122223333"}`) | `AssumeRole` | cross-account access and delegation |
| **Web identity** | `{"Federated": "arn:aws:iam::acct:oidc-provider/..."}` | `AssumeRoleWithWebIdentity` | GitHub Actions, Cognito, Google, mobile/web apps |
| **SAML 2.0 federation** | `{"Federated": "arn:aws:iam::acct:saml-provider/name"}` | `AssumeRoleWithSAML` | corporate SSO (AD FS, Entra ID, Okta) |
| **Custom trust policy** | anything valid (role ARN, user ARN, role session, conditions) | `AssumeRole` | role-to-role, MFA-conditioned, `sts:ExternalId` for third parties |

### 5.3 Taxonomy 2 — by management and lifecycle

| Category | Who creates it | Permissions | Key facts |
|---|---|---|---|
| **Customer-created role** | you | any policies you attach (+ optional boundary, tags) | the normal role |
| **Service role** | you (often via the service wizard) | you choose/attach | trust policy names the service principal; the service assumes it; e.g. `AWSBatchServiceRole`, `AWSElasticBeanstalkService` |
| **Service-linked role (SLR)** | **the AWS service** | **fixed by the service — cannot be edited** | name starts `AWSServiceRoleFor…`; trust = that service; **exempt from SCPs**; only the service's `DeleteServiceLinkedRole` can remove it |
| **Roles created by AWS features/tools** | AWS service / feature | service-managed | `OrganizationAccountAccessRole` (new Organizations account), `AWSControlTowerExecution` (Control Tower), `AWSCloudFormationStackSetExecutionRole`, Identity Center roles `AWSReservedSSO_<PermissionSet>_<random>` |

**SLR vs service role** is a classic exam distinction: a *service role* is a normal role **you** create for a service; a *service-linked role* is **created by the service**, has non-editable permissions, is not limited by SCPs, and can't be deleted while the service needs it.

#### 5.3.1 Examples of service-linked roles

These are the ones you are most likely to meet in a scenario (names are exact; exact permissions vary by service — check the service's SLR documentation):

| Service | SLR name | Trust principal | What it enables |
|---|---|---|---|
| **EC2 Spot** | `AWSServiceRoleForEC2Spot` | `spot.amazonaws.com` | launch/terminate/tag Spot Instances on your behalf |
| **EC2 Spot Fleet** | `AWSServiceRoleForEC2SpotFleet` | `spotfleet.amazonaws.com` | manage Spot Fleet target capacity |
| **EC2 Auto Scaling** | `AWSServiceRoleForAutoScaling` | `autoscaling.amazonaws.com` | scale and manage instances (terminate/detach during scaling) |
| **Elastic Load Balancing** | `AWSServiceRoleForElasticLoadBalancing` | `elasticloadbalancing.amazonaws.com` | manage load-balancer resources on your behalf |
| **Amazon ECS** | `AWSServiceRoleForECS` | `ecs.amazonaws.com` | create/manage ENIs and register targets for services (`awsvpc`) |
| **Amazon RDS** | `AWSServiceRoleForRDS` | `rds.amazonaws.com` | call EC2, SNS, CloudWatch Logs, Kinesis to manage DB instances |
| **AWS CloudTrail** | `AWSServiceRoleForCloudTrail` | `cloudtrail.amazonaws.com` | deliver trail logs to CloudWatch Logs |
| **AWS Organizations** | `AWSServiceRoleForOrganizations` | `organizations.amazonaws.com` | support trusted access between Organizations and AWS services |
| **AWS Backup** | `AWSServiceRoleForBackup` | `backup.amazonaws.com` | create/manage backups and restores across services |
| **Amazon GuardDuty** | `AWSServiceRoleForAmazonGuardDuty` | `guardduty.amazonaws.com` | manage detectors/findings and call dependent services |
| **AWS Security Hub** | `AWSServiceRoleForSecurityHub` | `securityhub.amazonaws.com` | run security checks and aggregate findings |
| **Amazon OpenSearch Service** | `AWSServiceRoleForAmazonOpenSearchService` | `es.amazonaws.com` | create/manage VPC, EC2 and ELB resources for domains |
| **Application Auto Scaling** | `AWSServiceRoleForApplicationAutoScaling_<Namespace>` (e.g. `…_ECSService`, `…_DynamoDBTable`) | `application-autoscaling.amazonaws.com` | scale ECS services, DynamoDB tables, Aurora replicas, etc. |
| **AWS Config** | `AWSServiceRoleForConfig` | `config.amazonaws.com` | evaluate and record resource configurations |

#### 5.3.2 SLR lifecycle and permissions (exam facts)

- **ARN path:** every SLR lives under `/aws-service-role/…`, e.g. `arn:aws:iam::123456789012:role/aws-service-role/ec2spot.amazonaws.com/AWSServiceRoleForEC2Spot`.
- **Creation:** automatic on first use of the feature, via the service wizard/API, or explicitly with `iam:CreateServiceLinkedRole`. Without that permission the service operation fails — a common CI/CloudFormation error (“Unable to create service-linked role”).
- **Permissions:** defined by the service, usually as an AWS managed policy named `…ServiceRolePolicy`; an IAM admin can **view but not edit** them, and the policy can't be attached to another entity.
- **Trust policy:** only the linked service can assume the role.
- **Deletion:** delete the related resources first, then `aws iam delete-service-linked-role --role-name …` (or let the service do it).
- **Limits:** SLRs count toward the roles-per-account limit, but **only SLRs may exceed it**.
- **SCP-exempt:** SCPs do not restrict SLR permissions (verified §9/§15).
- **Indirect permissions:** a principal allowed to call the service can indirectly reach whatever the SLR can reach (e.g. RDS's SLR calling EC2/SNS/Logs) — a favourite security nuance.
- Some services let you **pass an existing role** instead of using the SLR (`iam:PassRole`).

### 5.4 Taxonomy 3 — concrete roles and their exam keywords

| Role | What it is / keyword |
|---|---|
| **EC2 instance role (+ instance profile)** | credentials delivered via IMDSv2; `iam:PassRole` needed to attach it |
| **Lambda execution role** | trust `lambda.amazonaws.com`; what the function can access |
| **ECS task role vs execution role** | task role = what the app may do; execution role = what the ECS agent does (pull image, logs, secrets) |
| **EKS IRSA / EKS Pod Identity** | OIDC trust to the cluster's service-account issuer, or trust to `pods.eks.amazonaws.com` |
| **Cross-account access role** | trust policy allows a principal in another account; used instead of sharing keys |
| **`OrganizationAccountAccessRole`** | created when you create an account in Organizations; management account assumes it to administer the account |
| **SAML federation role** | `AssumeRoleWithSAML`; corporate SSO to console/CLI |
| **OIDC/web-identity role** | CI/CD (GitHub Actions), Cognito (authenticated/unauthenticated identities) |
| **IAM Roles Anywhere role** | on-premises workloads using X.509 certificates instead of keys |
| **Break-glass role** | emergency admin access: MFA condition, alarms, vaulted credentials (a pattern, not an AWS “type”) |
| **Third-party role with `sts:ExternalId`** | prevents the confused-deputy problem for vendors |
| **Identity Center permission-set role** | created and managed per account by Identity Center (`AWSReservedSSO_…`) |
| **Delegated-administrator role** | Security Hub/GuardDuty/CloudTrail admin account role (Organizations delegated admin) |
| **MFA-conditioned role** | trust policy with `aws:MultiFactorAuthPresent` for privileged human access |

### 5.5 Key role facts for the exam

- **AssumeRole default duration is 1 hour**; max is the role's `MaxSessionDuration` (1–12 h), except **role chaining** (assuming a role with a role session) is capped at **1 hour**.
- **`iam:PassRole`** is required whenever a service (e.g. EC2, Lambda, ECS) is asked to attach a role to a resource — a classic exam detail.
- **External ID** (`sts:ExternalId`) prevents the *confused deputy* problem when a third party assumes your role.
- **Trust policy conditions**: `aws:PrincipalOrgID`, `aws:SourceAccount`, `aws:SourceArn`, `aws:ExternalId`, `aws:MultiFactorAuthPresent`, `aws:PrincipalTag`.
- **Session policies** and **session tags** further scope an assumed session (intersection; tags enable ABAC).
- **Role trust policy is a resource-based policy** — `Principal` is required there; it cannot reference a group.
- Deleting and recreating a role/user that is referenced by ARN in a trust policy **breaks** the relationship (IAM stores the principal ID) — re-save the policy.

---

## 6. Policy types

| Type | Attached to | Grants or limits? | Notes |
|---|---|---|---|
| **Identity-based (managed)** | user, group, role | grants | reusable, versioned (5 versions), max **6,144 chars** |
| **Identity-based (inline)** | single entity | grants | user ≤2,048; group ≤5,120; role ≤10,240 chars |
| **Resource-based** | S3 bucket, SQS, SNS, KMS, Lambda, Secrets Manager, **role trust policy** | grants | requires `Principal`; supports cross-account |
| **Permissions boundary** | user or role (**not groups**) | **limits** | advanced; intersection with identity policies |
| **SCP / RCP** | Organisation root/OU/account | **limits** | member accounts only; never grants |
| **Session policy** | temporary session (AssumeRole / GetFederationToken) | **limits** | passed at session creation |
| **ACL** | S3 bucket/object etc. | grants | legacy; not IAM policies |

### 6.1 JSON policy anatomy

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "AllowS3ReadTeamBucket",
    "Effect": "Allow",
    "Principal": {"AWS": "arn:aws:iam::111122223333:role/app-reader"},
    "Action": ["s3:GetObject", "s3:ListBucket"],
    "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"],
    "Condition": {
      "Bool": {"aws:SecureTransport": "true"},
      "StringEquals": {"aws:PrincipalOrgID": "o-example123"}
    }
  }]
}
```

- `Version` must be `2012-10-17` (the only valid value).
- Any policy type uses the same grammar: `Effect`, `Action`/`NotAction`, `Resource`/`NotResource`, `Condition`, optional `Sid`.
- **`Principal` appears only in resource-based policies** (and trust policies) — never in identity-based policies.
- `NotPrincipal` with `Deny` is an anti-pattern (it denies principals that have permissions boundaries — §8).
- Policy variables (`${aws:username}`) and condition keys are evaluated per request.
- **No wildcard matching inside principal names/ARNs** — only `*` as all.

---

## 7. Policy evaluation logic — the core of IAM exam questions

### 7.1 Default and order

1. **Default deny** — nothing is allowed unless a policy grants it.
2. **Explicit deny** in *any* applicable policy → **deny** (wins over every allow).
3. Otherwise evaluate the grants per policy-type combination below.

### 7.2 Same account: identity + resource policies = **union**

Within one account, if an action is allowed by **either** identity-based or resource-based policy, it is allowed (explicit deny in either defeats it). This is the only place where policies are combined as a union.

### 7.3 Boundaries, SCPs, session policies = **intersection**

| Combination | Result |
|---|---|
| Identity policies + **permissions boundary** | **intersection** — the action must be allowed by both |
| Identity policies + **SCP/RCP** | **intersection** — must be allowed by the SCP and the identity policy |
| Identity + boundary + SCP | **intersection of all three** |
| Session policy (+ boundary) + identity policies | **intersection of all** |

Consequences:
- A **permissions boundary never grants** anything — it only caps.
- An **SCP never grants** anything — it only caps (and `FullAWSAccess` is attached by default).
- Removing a boundary/SCP can *increase* effective permissions.

### 7.4 Resource-based policies vs boundaries — the subtle part

- Within the same account, a resource-based policy that grants a permission directly to an **IAM user ARN** (or a **role session ARN**) is **not limited by an implicit deny** in the identity policy, permissions boundary or session policy — but it **is** defeated by an **explicit deny** in any of them.
- A resource-based policy that grants to an **IAM role ARN** *is* limited by implicit denies in the role's permissions boundary/session policy.
- Cross-account resource policies require the caller to also have identity-based permissions in their own account (§7.5).

### 7.5 Cross-account: **both sides**

For an authenticated principal in account A to use a resource in account B:
1. Account B's **resource policy** (bucket policy, key policy, role trust policy) must allow A's principal; **and**
2. Account A's **identity policy** must allow the action (for `AssumeRole`, `sts:AssumeRole` on the role ARN).
3. For SSE-KMS resources, B's **KMS key policy** must also allow the caller.

Anonymous access is the exception: no identity policy exists, so only `Principal: "*"` in the resource policy (plus no deny) can grant it.

### 7.6 The decision table (memorise)

| Question | Answer |
|---|---|
| Deny anywhere? | **Deny** |
| Same account, identity **or** resource allows? | **Allow** |
| Boundary/SCP/session policy applies? | Must be allowed there too (**intersection**) |
| Different account? | Both accounts' policies must allow |
| Nothing explicitly allows? | **Deny** (implicit) |

### 7.7 Root user

- The root user is **not limited by identity-based policies or permissions boundaries** — it has full access by definition.
- **SCPs do apply** to the root user of **member** accounts (not the management account).
- Root can be blocked by **explicit denies** in SCPs/RCPs and by some resource policies; resource-based `Deny` on a bucket applies to root too.
- Root-only tasks include: changing account settings/email/root password (standalone accounts), closing/reopening an account, restoring IAM permissions when the last admin is locked out, activating IAM access to Billing, some tax invoices, enabling S3 MFA delete, editing an S3 bucket policy that denies all principals, registering as an RI Marketplace seller.
- **Best practice:** MFA on root, no root access keys, no daily use, CloudTrail alarms on root activity; Organizations can centralise and even remove member-account root credentials.

---

## 8. Permissions boundaries

- A **managed policy used as a maximum** for a **user or role** (not groups, not other policies).
- Effective permissions = **intersection** (identity policies ∩ boundary).
- **Never grants** permissions by itself; adding one can only reduce what a user can do.
- **Explicit deny** inside the boundary blocks even resource-based grants.
- Standard use case: **delegating IAM administration** — let a team create users/roles but only with a required boundary (condition `iam:PermissionsBoundary`), so they can never escalate.
- Exam keywords: “delegate user creation”, “maximum permissions”, “ceiling”, “prevent privilege escalation”.
- Anti-pattern: `NotPrincipal` + `Deny` in a resource policy — it always denies principals **that have a boundary**, even if `NotPrincipal` excludes them.

---

## 9. SCPs, RCPs and AWS Organizations

- **SCPs** set the **maximum available permissions for IAM users and roles in member accounts**. They **never grant** permissions.
- Attached at **root, OU or account**; **OUs can be nested** (up to 5 levels) and SCPs **inherit** — a permission must be allowed at **every** level above the account.
- **Not applied to the management account**; applied to **member-account root users** too (except the not-restricted list: service-linked roles, management-account actions, etc.).
- The management account can restrict **member** root users but not itself.
- SCPs **do not directly affect resource-based policies** (e.g. a bucket policy allowing an external account still works even if the owner's SCP would deny it to its own users).
- **Allow-list vs deny-list strategies**: deny-list is easier initially; allow-list is stronger but operationally heavy. `FullAWSAccess` is attached by default — removing it without a replacement blocks everything.
- **RCPs (resource control policies)** — newer Organizations policy type that limits access to resources; effective permissions = intersection of SCP, RCP and identity/resource policies.
- **Not restricted by SCPs:** actions in the management account, actions by service-linked roles, a few legacy/support tasks.

### 9.1 SCP vs permissions boundary vs identity policy

| | Scope | Grants? | Applies to |
|---|---|---|---|
| Identity policy | single identity | **yes** | that user/role (via groups too) |
| Permissions boundary | single user/role | no — caps | that identity |
| SCP | whole account(s) | no — caps | all users/roles (and member root) |
| RCP | resources | no — caps | resource access across accounts |

---

## 10. Session policies and session tags

- **Session policy**: an inline policy passed when creating a temporary session (`AssumeRole`, `GetFederationToken`). Effective permissions = role's policies ∩ session policy (∩ boundary). Used by identity brokers to hand out narrowed sessions.
- **Session tags**: key/value pairs passed at `AssumeRole` (up to 50) or derived from SAML assertions; used as `aws:PrincipalTag` in ABAC policies.
- **Role chaining** (role session assuming another role) caps the session at **1 hour**, and the second role only receives the intersection of trusted sessions.

---

## 11. Federated access, IAM Identity Center and ABAC

### 11.1 Federation with IAM

| Pattern | STS API | Trust policy `Principal` | Typical use |
|---|---|---|---|
| **SAML 2.0** | `AssumeRoleWithSAML` | `Federated: arn:aws:iam::acct:saml-provider/name` | corporate SSO (AD FS, Okta, Entra ID) to console/CLI |
| **OIDC / web identity** | `AssumeRoleWithWebIdentity` | `Federated: arn:aws:iam::acct:oidc-provider/...` | CI/CD (GitHub Actions), mobile/web apps (Cognito, Google, Facebook) |
| **Custom identity broker** | `AssumeRole` / `GetFederationToken` | role or user principal | self-built portal; `GetFederationToken` is legacy |

Key points:
- Federation **avoids long-term IAM users**; external identity authenticates, then STS issues temporary credentials for a role.
- **Conditions** (`aws:PrincipalOrgID`, `token.actions.githubusercontent.com:sub`, `saml:sub`, `aws:SourceAccount`) restrict *which* federated identities may assume the role.
- **OIDC for CI/CD** is the standard answer for “GitHub Actions must deploy to AWS without storing access keys”.

### 11.2 IAM Identity Center — workforce SSO (successor to AWS SSO)

- **What it is:** the AWS solution for connecting **workforce users** to **AWS accounts** and **AWS managed applications** (plus SAML apps), with one federation point.
- **Identity sources:** Identity Center's own directory (create users directly), **AWS Managed Microsoft AD**, or an **external IdP** (SAML 2.0 + SCIM for provisioning).
- **Two instance types:**
  - **Organization instance** — deployed in the **Organizations management account**; the only instance that can manage access to AWS accounts; best practice for production.
  - **Account instance** — bound to a single account; only for isolated deployments of select AWS managed applications (no multi-account permissions).
- **Permission sets:** templates of AWS managed / customer managed / inline policies (+ optional **permissions boundary**, session duration). **Assigning** a permission set to users/groups for accounts makes Identity Center **create and manage IAM roles in those accounts**; users assume them through the AWS access portal or AWS CLI.
- **Session durations:** permission-set session default **1 h**, max **12 h**; AWS access portal session default **8 h**, max **90 days**.
- **Not IAM users:** Identity Center users/groups live in the **Identity Store**, not in IAM; per-account access is always through the generated IAM roles.
- **CLI/SSO:** `aws configure sso`, `aws sso login` — short-lived credentials, no long-term access keys.
- **Groups cannot be nested** — same flat model as IAM; hierarchy is modelled with permission sets + assignments.
- Requires **AWS Organizations** (org instance) and **multi-account permissions** enabled.
- **Exam keywords:** “5,000 employees”, “40 AWS accounts”, “no IAM users”, “centralised access”, “SAML IdP”, “SCIM”, “permission sets”.
- It is **not** a replacement for workload roles (EC2/Lambda still use IAM roles); machine-to-machine outside the org uses **OIDC federation** or IAM Roles Anywhere.

### 11.3 IAM Identity Center vs IAM users — the comparison the exam asks for

| Dimension | **IAM Identity Center** | **IAM user** |
|---|---|---|
| What it is | workforce **SSO/federation** service across accounts & apps | a **per-account identity** inside IAM |
| Where the identity lives | Identity Store (own directory / AD / external IdP) | IAM in **each account** |
| Credentials | temporary credentials via SSO roles; no long-term keys | long-term **password** + **access keys** (2/user) |
| Scope | all accounts in the Organisation (one sign-in) | one account only; cross-account needs AssumeRole |
| How permissions are defined | **permission sets** → Identity Center-managed IAM roles in each account | managed/inline policies, attached directly or via **groups** |
| Groups | synced from IdP; used for **assignments**; **no nesting** | IAM groups; attach policies; **no nesting; max 10/user** |
| Federation | native (SAML 2.0, SCIM); one IdP integration | manual roles per account (SAML/OIDC trust policies) |
| MFA | via IdP or Identity Center | per user (virtual/hardware/FIDO) |
| CLI/console | AWS access portal + `aws sso login` (auto-refresh) | console sign-in / access keys in tools & CLI |
| Multi-account onboarding | assign group → permission set → accounts | create users/roles in every account (or federate) |
| Limits | no per-account user quota problem | **5,000 users/account** |
| Cost | **no additional charge** for Identity Center itself (directory/AD options may cost) | no separate charge; credential-management overhead |
| Audit | sign-in/audit events, CloudTrail, trusted identity propagation | CloudTrail, credential report, Access Advisor |
| Best practice | **humans** (workforce) | legacy apps, break-glass, tools that cannot federate |

**When IAM users are still legitimate:** legacy applications that need static access keys (tools that can't assume roles), SES SMTP credentials, some third-party integrations, an emergency break-glass identity, and APIs where federation isn't possible. Everything else: **Identity Center for people, IAM roles for workloads**.

**Migration path (exam scenario):** connect the IdP to Identity Center → create permission sets matching current IAM policies → assign groups to accounts → move users off IAM user credentials → remove console access for IAM users → keep only documented exceptions (prove it with Access Analyzer + credential report).

### 11.4 ABAC with tags

**ABAC** = authorisation based on **attributes**, which AWS implements as **tags**: the tags on the **principal** and the tags on the **resource** must match for access to be granted. One policy then serves hundreds of teams without edits when new resources appear.

The four tag dimensions:

| Dimension | Condition key | Set by | Use |
|---|---|---|---|
| **Principal tags** | `aws:PrincipalTag/<key>` | admin tags on the IAM user/role | who the caller is (team, cost centre) |
| **Session tags** | also `aws:PrincipalTag/<key>` | passed at `AssumeRole`, OIDC/SAML federation, IdP | dynamic directory attributes; expire with the session |
| **Resource tags** | `aws:ResourceTag/<key>` | whoever tagged the resource (controlled by policy) | which resource it is (team, project, tenant) |
| **Request tags** | `aws:RequestTag/<key>`, `aws:TagKeys` | the API call itself | control what tags may be set at creation |

Example — one policy for all teams:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "TeamAccessOnlyToOwnResources",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::shared-data/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/team": "${aws:PrincipalTag/team}"
        }
      }
    }
  ]
}
```

ABAC vs RBAC:

| | **RBAC (classic IAM)** | **ABAC** |
|---|---|---|
| Permissions defined by | job function; policy lists concrete resources | tag match between principal and resource |
| New resources | policy must be updated | automatically covered if correctly tagged |
| Number of policies | grows with teams/resources | one (or a few) parameterised policies |
| Scaling | manual | automatic |
| Requires | policy management | **tagging discipline** + tag governance |
| Risk | policy sprawl | **untrusted tags** → privilege escalation |
| Best for | small/medium estates, stable roles | multi-tenant, project-based, directory-driven access |

Governance requirements (where the exam questions hide):

- **Tags must be trustworthy.** Principal tags must be controlled by admins, never by the principal itself; session tags must come only from a trusted broker (IdP / AssumeRole conditions).
- **Restrict who can tag and what they can tag:** `aws:TagKeys` / `aws:RequestTag` conditions, deny tag changes on existing resources (`ec2:CreateTags`/`DeleteTags`, `s3:PutObjectTagging`), and enforce tags at creation (deny when `aws:RequestTag/team` is null).
- **Tag limits:** 50 tags per IAM user/role (and most AWS resources), **10 tags per S3 object**.
- **Tag keys and values are case-sensitive** — inconsistent casing silently breaks ABAC.
- S3 specifics: bucket/object tag conditions use `aws:ResourceTag` and `s3:ExistingObjectTag`; uploads must be constrained with `s3:RequestObjectTagKeys`.

Exam triggers & anti-patterns:

- “Hundreds of projects/teams, a policy per team is unmanageable” → **ABAC**.
- “Access must follow employee attributes from the corporate directory” → **session tags from SAML/OIDC + ABAC**.
- “Users can tag their own resources” → **anti-pattern**; tags become an escalation path.
- “ABAC, but tags are missing/inconsistent” → it will deny (or misgrant) — fix tagging first.

---

## 12. Audit, analysis and guardrails

| Tool | Purpose |
|---|---|
| **IAM Access Analyzer** | findings for resources shared with external entities (S3, roles, KMS, SQS, …); policy validation; unused-permission analysis; policy generation from CloudTrail |
| **Credential report** | CSV of all users, MFA, key ages, password status |
| **Access Advisor / service last accessed** | which services each user/role actually used → least privilege |
| **CloudTrail** | who did what, when (API audit); root activity alarms |
| **AWS Config** | compliance rules (e.g. root MFA, no public buckets) |
| **Security Hub / Trusted Advisor** | org-wide security posture checks |
| **IAM policy simulator** | test policy outcomes for a principal/action/resource |
| **SCP/RCP in Organizations** | preventive guardrails |
| **Permissions boundaries** | per-identity ceilings (delegation) |

---

## 13. Limits and quotas worth memorising

| Quota | Default | Adjustable |
|---|---|---|
| Users per account | 5,000 | no |
| Groups per account | 300 (max 500) | yes |
| **Groups per user** | **10** | **no** |
| Roles per account | 1,000 (max 10,000) | yes |
| Customer managed policies per account | 1,500 (max 10,000) | yes |
| Managed policies per group | **10** | no |
| Managed policies per user | 10 (max 20) | yes |
| Managed policies per role | 20 (max 25) | yes |
| Access keys per user | 2 | no |
| MFA devices per user | 8 | no |
| Managed policy size | 6,144 chars | no |
| Inline policy size (user / group / role) | 2,048 / 5,120 / 10,240 chars | no |
| Role trust policy size | 2,048 (max 8,192) | yes |
| Role session duration | 15 min–12 h (default 1 h) | via role setting |
| Role **chaining** session | **1 h** | no |
| STS request rate | 600 req/s per account/Region | ticket |

---

## 14. Design patterns the exam expects

### 14.1 Cross-account access (role-based, preferred)

1. Account B creates role `AppReader` with a trust policy allowing account A's role/user.
2. Account A's identity policy allows `sts:AssumeRole` on that role ARN.
3. Account A's principal assumes the role and uses the **temporary** session credentials.
4. B's role permissions policy limits what the session can do; add `aws:PrincipalOrgID` to prevent outsiders.

**Why not share IAM users/keys:** no long-term secrets, credentials expire, CloudTrail shows the role session, revocation is one action.

### 14.2 Multi-account governance (Organizations)

- **Accounts** created in Organizations; **nested OUs** (up to 5 levels) for hierarchy.
- **SCPs** attached to OUs/accounts to deny unapproved services/regions, enforce MFA, deny public S3, prevent CloudTrail disable.
- **Centralised logging** account (CloudTrail org trail, S3 log bucket), **security tooling** account (GuardDuty/Security Hub delegated admin), **shared services** account (Transit Gateway/VPC).
- **Identity Center** in the management or delegated admin account; **permission sets** → account assignments by group.
- **RCP + SCP + resource policies** together form a **data perimeter** (deny access from outside `aws:PrincipalOrgID` / outside `aws:SourceVpc`).

### 14.3 Break-glass

- Root: MFA, no keys, alarms.
- A dedicated **break-glass IAM role** with `AdministratorAccess` + strict trust conditions (MFA, specific principal, source IP), secret stored in a vault, CloudTrail alarm on use, and a rotation/audit runbook.

### 14.4 Workloads

- **EC2/ECS/Lambda** → IAM role (instance profile / task role / execution role), never embedded keys.
- **CI/CD outside AWS** → **OIDC federation** to a role with short sessions.
- **On-premises** → IAM Roles Anywhere or STS with federation.
- **Cross-account deployment** → assume a deployment role; `iam:PassRole` on the target role.

---

## 15. Exam traps

1. **“Groups can be nested.”** No — flat, users only.
2. **“Groups can be used in `Principal`.”** No — never.
3. **“A user's policies and group policies are intersected.”** No — **union** (same account, identity policies); intersection applies to boundaries/SCPs/session policies.
4. **“SCPs apply to the management account.”** No.
5. **“SCPs grant permissions.”** No — they only cap.
6. **“SCPs affect resource-based policies.”** Not directly — they limit principals in the account, not the bucket policy's grant to outsiders.
7. **“Permissions boundaries grant access.”** No — they cap; identity policies still needed.
8. **“Boundaries can be attached to groups.”** No — users and roles only.
9. **“Explicit deny inside a boundary doesn't block resource-based grants.”** It does — explicit deny always wins.
10. **“Cross-account access needs only the bucket policy.”** No — identity policy on the caller side too (and KMS key policy for SSE-KMS).
11. **“Root is limited by SCPs.”** Only in member accounts — and the management account root is unrestricted.
12. **“Root can be restricted with an IAM policy.”** No — identity policies don't apply to root; use SCP/RCP/resource policies.
13. **“Role chaining allows 12-hour sessions.”** No — chained sessions are capped at 1 hour.
14. **“`iam:PassRole` isn't needed when launching EC2 with a role.”** It is.
15. **“Deleting a role referenced in a trust policy is harmless.”** No — matching breaks; re-save the policy.
16. **“Federation requires IAM users.”** No — it's the alternative to them.
17. **“Identity Center groups can be nested.”** No.
18. **“Access keys are the recommended way for CI/CD.”** No — OIDC federation / roles.
19. **“ABAC works without tags.”** No — it depends on tag hygiene and `aws:PrincipalTag`/`aws:ResourceTag`.
20. **“Access Analyzer blocks access.”** No — it **reports** findings (and can archive); blocking comes from policies/BPA.
21. **“Service-linked roles can be restricted by SCPs.”** No.
22. **“A role has permissions by itself without a permissions policy.”** No — trust policy only says who can assume; permissions come from policies.
23. **“A user with no policies has access if an SCP allows.”** No — SCPs never grant.
24. **“A group can be listed in a resource-policy `Principal`.”** No — groups are not principals; the policy is rejected at save time. Attach an identity policy to the group, or grant to a role/account instead.
25. **“ABAC works with whatever tags users set themselves.”** No — untrusted tags are a privilege-escalation path; restrict tagging with `aws:TagKeys`/`aws:RequestTag` and control principal tags.
26. **“Identity Center users are IAM users.”** No — they live in the Identity Store and access accounts through Identity Center-managed IAM roles (permission sets).
27. **“IAM Identity Center is just console SSO.”** No — it manages account access across the Organisation (permission sets + assignments), application access and CLI credentials; only the *account instance* is the limited apps-only variant.
28. **“Service-linked roles can be edited or restricted by SCPs.”** No — permissions are fixed by the service and SCPs don't apply to SLRs.
29. **“EC2 instances can use any IAM role directly.”** The role must be attached through an **instance profile** (or the ECS/EKS equivalents) for temporary credentials to be delivered.
30. **“A cross-account role's trust policy is enough.”** No — the caller's identity policy must also allow `sts:AssumeRole` (both sides).
31. **“A role can be added to an IAM group.”** No — groups contain only users; roles are assumed, and a role can trust another role instead.
32. **“A service-linked role can be deleted like any IAM role.”** No — related resources must be deleted first; the service owns the lifecycle (`DeleteServiceLinkedRole`).
33. **“An SLR is optional; the service works without it.”** No — if it cannot be created (e.g. the caller lacks `iam:CreateServiceLinkedRole`), the service operation fails.
34. **“You can edit an SLR's permission policy if needed.”** No — SLR permissions are view-only and defined by the service (AWS managed `…ServiceRolePolicy`).

---

## 16. Practice questions (SAP-C02 style)

> Answer before reading §17.

**Q1.** Can IAM groups be nested to mirror an organisation chart?
A. Yes, up to 5 levels
B. No — groups contain only users and cannot contain other groups; use multiple groups/roles or Identity Center
C. Yes, if the groups are in the same path
D. Only with SCPs

**Q2.** A team wants to grant read access to an S3 bucket by adding an IAM group to the bucket policy `Principal`. What happens?
A. It works
B. The policy is invalid — groups are not principals; grant to a role or specific principals instead
C. It works only for same-account groups
D. It requires a permissions boundary

**Q3.** A user is in three groups: group A allows S3 read, group B allows S3 write, group C denies S3 delete. What is the effective S3 permission?
A. Read only
B. Read and write; delete denied (explicit deny wins)
C. Intersection = nothing
D. Behaviour is undefined

**Q4.** An SCP denies `ec2:*` in an OU; the account's IAM role has `AdministratorAccess`. What happens?
A. AdministratorAccess wins
B. All EC2 actions are denied for principals in that account
C. EC2 actions are denied only for root
D. SCPs don't affect IAM roles

**Q5.** A company must let a contractor create IAM users but only with a fixed maximum permission set. What do you use?
A. SCP
B. Permissions boundary attached to created users + `iam:PermissionsBoundary` condition
C. ACL
D. Session policy

**Q6.** Account A's Lambda must write to account B's S3 bucket. What is required?
A. Bucket policy in B allowing A's role; identity policy in A allowing s3:PutObject
B. Only the bucket policy in B
C. Only an IAM policy in A
D. Make the bucket public

**Q7.** Which statement about the management account in AWS Organizations is true?
A. SCPs apply to its root user
B. SCPs do not affect users/roles in the management account
C. SCPs apply only to its root user
D. SCPs grant extra permissions there

**Q8.** A role is assumed by a role session and then assumes another role. What is the maximum session duration of the second role?
A. 12 hours
B. 1 hour
C. 8 hours
D. Same as the first role

**Q9.** An EC2 instance must access DynamoDB without stored credentials. What is the correct design?
A. Access keys in user data
B. IAM role attached via instance profile; IMDSv2 delivers temporary credentials
C. Root access keys in the AMI
D. A group with DynamoDB permissions

**Q10.** Which service gives employees access to 40 AWS accounts via their corporate IdP without creating IAM users?
A. IAM groups
B. IAM Identity Center with SAML federation and permission sets
C. Access Analyzer
D. SCPs

**Q11.** A resource-based policy grants `secretsmanager:GetSecretValue` to an IAM user in the same account. The user has an identity policy that doesn't allow it and a permissions boundary with no explicit deny. What happens?
A. Denied — implicit deny in identity policies
B. Allowed — same-account resource-based grants are not limited by implicit denies (but an explicit deny would block)
C. Denied — boundaries always block
D. Allowed only with MFA

**Q12.** Which is NOT restricted by SCPs?
A. Member-account IAM users
B. Member-account root user
C. Service-linked roles
D. Member-account IAM roles

**Q13.** How many groups can a single IAM user belong to?
A. 5
B. 10 (not adjustable)
C. 300
D. Unlimited

**Q14.** GitHub Actions must deploy to AWS without long-term keys. Which approach?
A. IAM user with access keys in secrets
B. OIDC provider + role trust policy for `token.actions.githubusercontent.com`
C. Root keys in repository variables
D. SAML federation to GitHub

**Q15.** A trust policy grants a third-party vendor's account `sts:AssumeRole`. Which additional control prevents the confused-deputy problem?
A. `sts:ExternalId` condition
B. MFA condition
C. Permissions boundary
D. SCP allow-list

**Q16.** Which statement about permissions boundaries is correct?
A. They can be attached to groups
B. They grant permissions on their own
C. They cap users/roles; effective permissions are the intersection with identity policies
D. They override SCPs

**Q17.** An IAM role has a trust policy but no permissions policy. What can the assumed session do?
A. Everything
B. Nothing (implicit deny)
C. Read-only
D. Only STS calls

**Q18.** Which tool identifies resources shared with external entities?
A. Credential report
B. IAM Access Analyzer
C. AWS Config
D. CloudTrail

**Q19.** A policy must be written once and work for hundreds of teams where each team can access only its own bucket prefix and resources are tagged `team=<name>`. What two mechanisms?
A. SCP + boundary
B. ABAC using `aws:PrincipalTag/team` and `aws:ResourceTag/team` (with session tags)
C. One group per team with a boundary
D. Root-only access

**Q20.** What happens if you delete and recreate an IAM role referenced by ARN in another role's trust policy?
A. Nothing; ARNs are stable
B. The trust relationship breaks because IAM stores the role's unique principal ID; re-save the policy to repair it
C. The role is automatically re-linked
D. Both roles are deleted

**Q21.** A team lead wants to grant S3 read access to the `Developers` IAM group by adding the group's ARN to the bucket policy `Principal`. What happens, and what is the correct approach?
A. It works; groups can be used as principals in bucket policies
B. The policy is rejected — groups are not principals. The correct approach is an identity-based policy attached to the group (same account), or a role/account principal for cross-account access
C. It works only if the group is in the same account
D. It works after attaching a permissions boundary to the group

**Q22.** A company has 300 projects. Each project has EC2 instances and S3 prefixes tagged `project=<name>`, and employees move between projects frequently. The security team wants to avoid maintaining 300 policies. Which approach fits?
A. One group and one policy per project
B. ABAC: one parameterised policy matching `aws:PrincipalTag/project` to `aws:ResourceTag/project`, with session tags from the IdP and strict tagging controls
C. A permissions boundary per project
D. Put all projects in one account with a shared admin role

**Q23.** A company has 2,000 employees and 25 AWS accounts in one Organisation. It wants a single sign-in, no IAM users, group-based access and central audit. What should it deploy?
A. IAM users + IAM groups replicated in each account
B. IAM Identity Center (organisation instance) with an external IdP, permission sets and group assignments
C. A root user per account shared by the team
D. IAM roles with long-term access keys distributed to employees

**Q24.** Which role category is created automatically by an AWS service, has permissions that cannot be edited, and is exempt from SCPs?
A. Service role
B. Service-linked role (SLR)
C. Cross-account role
D. Break-glass role

**Q25.** GitHub Actions must upload to S3 without storing AWS access keys. Which role trust configuration fits?
A. Trust policy with `{"Service": "s3.amazonaws.com"}`
B. Web identity (OIDC) trust to `token.actions.githubusercontent.com`, with conditions on the repository/subject
C. SAML 2.0 trust to GitHub
D. `OrganizationAccountAccessRole`

**Q26.** A CloudFormation deployment fails while creating an EC2 Spot Fleet with an “Unable to create service-linked role” error. What is missing?
A. `iam:PassRole` on the Spot Fleet role
B. `iam:CreateServiceLinkedRole` for `arn:aws:iam::*:role/aws-service-role/*`
C. `ec2:CreateServiceLinkedRole`
D. `iam:AttachRolePolicy` on `AWSServiceRoleForEC2Spot`

---

## 17. Answer key with explanations

**A1 — B.** Groups are flat containers of users; nesting is not supported. The hierarchy is emulated with multiple groups per user, roles, or Identity Center permission sets.

**A2 — B.** `Principal` requires an authenticated identity — a group is not one. Grant to a role (preferred) or specific principals; never a group.

**A3 — B.** Same-account identity policies combine as a union (read + write allowed), and an explicit deny in any policy (group C) wins over allows.

**A4 — B.** SCPs cap permissions for all users and roles (including root) in member accounts; AdministratorAccess can't exceed the SCP ceiling.

**A5 — B.** A permissions boundary sets the maximum; the condition `iam:PermissionsBoundary` forces the contractor to attach it. SCPs are account-wide, not per-identity.

**A6 — A.** Cross-account requires the resource policy in B **and** the identity policy in A (plus KMS key policy if SSE-KMS).

**A7 — B.** SCPs don't affect the management account. They do affect member-account root users.

**A8 — B.** Role chaining caps the session at 1 hour, regardless of the second role's max session duration.

**A9 — B.** Attach an IAM role through an instance profile; IMDSv2 delivers rotating temporary credentials. Never embed keys.

**A10 — B.** Identity Center + SAML federation + permission sets + account assignments — no IAM users needed.

**A11 — B.** Within the same account, resource-based grants to an IAM user ARN are not limited by implicit denies in identity policies/boundaries; an explicit deny in any policy would still block.

**A12 — C.** SCPs don't restrict service-linked roles (nor the management account).

**A13 — B.** 10 groups per user, hard limit.

**A14 — B.** OIDC federation (web identity) — short-lived credentials, no stored keys.

**A15 — A.** `sts:ExternalId` — a secret shared with the third party, required in the AssumeRole call, preventing confused-deputy attacks.

**A16 — C.** Boundaries apply to users/roles, never grant, and intersect with identity policies.

**A17 — B.** The trust policy only says who may assume; without a permissions policy the session has implicit deny for everything else.

**A18 — B.** Access Analyzer reports external/public access findings and unused access.

**A19 — B.** ABAC with session/principal tags and resource tags in a single parameterised policy.

**A20 — B.** IAM converts ARNs in trust policies to unique principal IDs; deleting the role breaks the link and you must re-save the policy with the new ARN.

**A21 — B.** `Principal` must be an authenticated identity; a group never authenticates, so a group ARN fails policy validation (the policy can't be saved). For same-account access, attach an identity policy to the group — that is the group's purpose. For cross-account, grant to a role ARN or the account ID (account principal), and remember the caller side also needs `sts:AssumeRole`/identity permissions.

**A22 — B.** ABAC with one parameterised policy matching principal tags to resource tags. It scales automatically to new resources, follows employees who change projects via session tags from the IdP, and needs tag governance (trusted tag sources, restricted `aws:TagKeys`/`aws:RequestTag`). RBAC would mean 300 policies and constant updates.

**A23 — B.** IAM Identity Center (organisation instance) + external IdP (SAML 2.0 + SCIM) + permission sets assigned to groups: one sign-in for all accounts, temporary credentials, central audit, no IAM users. Replicating IAM users/groups per account doesn't scale, root sharing and distributed access keys are anti-patterns.

**A24 — B.** A service-linked role is created and maintained by the service, has fixed permissions you cannot change, is not restricted by SCPs, and can only be removed via the service's `DeleteServiceLinkedRole`. A service role (A) is a normal role you create for a service.

**A25 — B.** Web identity federation: OIDC provider for GitHub, trust policy to `token.actions.githubusercontent.com`, conditions on `sub` (repo/branch). No long-term credentials are stored anywhere.

**A26 — B.** The deployment principal needs `iam:CreateServiceLinkedRole` on `arn:aws:iam::*:role/aws-service-role/*` so EC2 Spot can create `AWSServiceRoleForEC2Spot`. It is not `iam:PassRole` (the SLR is created, not passed) and there is no EC2-specific create action.

---

## 18. Rapid recall

| Fact | Value |
|---|---|
| Groups | flat, users only, **10 per user**, no nesting, not principals |
| Groups per account | 300 → 500 |
| Managed policies per group | 10 (not adjustable) |
| Users per account | 5,000 (not adjustable) |
| Access keys per user | 2 |
| MFA devices per user | 8 |
| Managed policy / inline limits | 6,144 chars; user 2,048, group 5,120, role 10,240 |
| Role session | default 1 h; 15 min–12 h via role setting |
| Role chaining | **1 h max** |
| Evaluation | deny > allow; union identity+resource; intersection boundary/SCP/session; cross-account both |
| SCP scope | member accounts only, not management; caps incl. member root; no SLRs |
| Boundary scope | users and roles only |
| Principal types | account, user, role, role session, OIDC/SAML federated, service, `*` |
| Root-only examples | close/reopen account, restore IAM permissions, enable S3 MFA delete, activate IAM billing access |
| STS default request quota | 600 req/s per account/Region |
| ABAC | tag match (principal ↔ resource); needs trustworthy tags; 50 tags/resource, **10 per S3 object**; session tags from IdP |
| Identity Center | permission sets → managed IAM roles per account; session 1–12 h; portal session max 90 days; org instance for account access |
| IAM users vs Identity Center | IAM users: long-term per-account credentials, 5,000/account, flat groups; Identity Center: workforce SSO, temp credentials, multi-account |
| Role types | 5 trust types: **service, account, web identity, SAML 2.0, custom**; service role vs **SLR** (service-created, fixed, SCP-exempt); EC2 needs an **instance profile** |
| Role session | default **1 h**, 15 min–12 h via role setting; **chaining = 1 h**; session policy + session tags can narrow |
| SLR ARN path | `arn:aws:iam::acct:role/aws-service-role/<service>.amazonaws.com/AWSServiceRoleFor<Name>` |
| SLR examples | `AWSServiceRoleForEC2Spot`, `…EC2SpotFleet`, `…AutoScaling`, `…ElasticLoadBalancing`, `…ECS`, `…RDS`, `…CloudTrail`, `…Organizations`, `…Backup`, `…AmazonGuardDuty`, `…SecurityHub`, `…ApplicationAutoScaling_*` |
| SLR creation permission | `iam:CreateServiceLinkedRole` on `arn:aws:iam::*:role/aws-service-role/*` |

### Phrases for written answers

- “Groups are **flat**; use multiple groups, roles, or Identity Center to model hierarchy.”
- “**Explicit deny wins**; boundaries and SCPs **intersect**, identity + resource policies **union**.”
- “Cross-account access needs **both sides** and a **role** — never shared IAM users.”
- “Use **OIDC federation** instead of access keys for CI/CD.”
- “**SCPs cap**, they never grant; management account is exempt.”
- “Use a **permissions boundary** to delegate IAM administration without privilege escalation.”

---

## 19. S3/EC2-specific IAM tie-ins

- **S3** bucket policies are resource-based; `Principal` required; cross-account needs both sides; presigned URLs act as the signer; Block Public Access prevents public policies (§22 of the S3 book).
- **KMS** key policies must allow the caller for SSE-KMS objects — identity permission alone is insufficient.
- **EC2 roles** via instance profile + IMDSv2; **`iam:PassRole`** required to attach roles to EC2/ECS/Lambda.
- **Lambda execution roles** vs resource-based policies on the function (for cross-account invocation).
- **SQS/SNS** resource policies enable cross-account messaging without IAM role chaining.

---

## 20. Glossary

| Term | Meaning |
|---|---|
| **Principal** | authenticated entity in a resource policy (account, user, role, role session, federated, service, `*`) |
| **Trust policy** | resource-based policy on a role defining who can assume it |
| **Identity-based policy** | policy attached to user/group/role |
| **Resource-based policy** | policy attached to a resource (S3, KMS, SQS, role trust) |
| **Permissions boundary** | managed policy capping a user's/role's maximum permissions |
| **SCP** | Organization service control policy — guardrail for member accounts |
| **RCP** | Organization resource control policy — resource-side guardrail |
| **Session policy** | inline policy passed at session creation, intersecting role permissions |
| **Session tag** | tag passed at AssumeRole, used via `aws:PrincipalTag` (ABAC) |
| **ABAC** | attribute-based access control using tags/attributes |
| **Permission set** | Identity Center template of IAM policies (+ optional boundary) assigned to users/groups for accounts; provisions IAM roles |
| **Organization instance** | Identity Center instance in the Organizations management account — the only type that manages AWS account access |
| **Trusted identity propagation** | Identity Center feature that passes the user's identity to downstream apps so audit (CloudTrail) is per user |
| **Federation** | authenticating via external IdP (SAML/OIDC) to assume roles |
| **IAM Identity Center** | workforce SSO/IdP integration with permission sets and account assignments |
| **Role chaining** | assuming a role with a role session — capped at 1 hour |
| **Service role** | normal role you create whose trust policy names an AWS service principal |
| **Service-linked role (SLR)** | service-created, fixed-permission role (`AWSServiceRoleFor…`), SCP-exempt |
| **Instance profile** | container that attaches a role to an EC2 instance |
| **Role session** | temporary principal `assumed-role/<role>/<session>` issued by STS |
| **PassRole** | permission to attach a role to a service (`iam:PassRole`) |
| **External ID** | shared secret in trust policy preventing confused deputy |
| **Credential report** | CSV audit of users' credentials/MFA |
| **Access Advisor** | last-accessed service data for least privilege |
| **Access Analyzer** | findings for external/public access and unused permissions |

---

## 21. Sources (verified November 2026)

- [IAM user groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)
- [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [Permissions boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)
- [AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)
- [Service control policies (SCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [AWS JSON policy elements: Principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)
- [IAM and AWS STS quotas](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-quotas.html)
- [IAM endpoints and quotas (AWS General Reference)](https://docs.aws.amazon.com/general/latest/gr/iam-service.html)
- [Users, groups, and provisioning in IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/users-groups-provisioning.html)
- [AWS Certified Solutions Architect – Professional exam guide (SAP-C02, PDF)](https://docs.aws.amazon.com/pdfs/aws-certification/latest/solutions-architect-professional-02/solutions-architect-professional-02.pdf)
