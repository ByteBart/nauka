# Amazon VPC — default vs custom VPC, subnets, routing, security and connectivity
## Study book for AWS Certified Solutions Architect – Professional (SAP-C02)

> **Scope:** VPC fundamentals the exam tests — VPC anatomy, **default VPC vs a newly created VPC**, subnets (public/private, sizing, auto-assign IP), route tables and gateways (IGW, NAT, egress-only IGW), security groups vs NACLs, VPC endpoints, peering/Transit Gateway, DNS, monitoring, traps and practice questions.
> **Knowledge state:** November 2026, verified against current AWS documentation (links in §16).
> **Short answer to the core question:** a **default VPC** is a pre-built, internet-ready VPC (CIDR `172.31.0.0/16`, one public `/20` subnet per AZ, an internet gateway and a `0.0.0.0/0` route) that exists in every Region, while a **newly created VPC** is an **empty shell**: you choose the CIDR, and it has **no subnets, no internet gateway, no internet route, DNS hostnames off and no public IPs** until you build them.

---

## 0. TL;DR

| | **Default VPC** | **Newly created VPC** |
|---|---|---|
| Exists when? | automatically, one per Region, for accounts created after Dec 2013 | only when you create it (console/API/IaC) |
| IPv4 CIDR | fixed **`172.31.0.0/16`** | **you choose** (e.g. `10.0.0.0/16`), /16–/28 |
| Subnets | one **`/20` public subnet per AZ** (4,096 IPs each) | **none** |
| Internet gateway | **attached** | **none** |
| Main route table | `0.0.0.0/0 → IGW` + local | **local only** |
| Public IPv4 on launch | **yes** in default subnets | **no** (subnet attribute off) |
| DNS hostnames | **enabled** | **disabled** (`enableDnsHostnames=false`) |
| DNS resolution | enabled | enabled (`enableDnsSupport=true`) |
| Default SG / NACL | created | created (same default rules) |
| Internet access “out of the box” | yes (instances are public) | **no** — must attach IGW + routes (or NAT) |
| Best for | quick experiments, simple public sites | **anything production / security-sensitive** |

---

## 1. Exam context

- SAP-C02 (75 questions, 180 min, 750/1000; domains ≈ 26 % organisational complexity, 29 % new solutions, 25 % continuous improvement, 20 % migration/modernisation).
- VPC appears mostly in **Domain 1** (multi-account landing zones, connectivity, hybrid) and **Domain 2** (secure, highly available designs), plus **Domain 3** (troubleshooting existing networks).
- Question shapes: “instances cannot reach the internet — why?”, “design public/private subnets across 3 AZs”, “connect VPCs”, “secure S3 access without internet”, “DNS does not resolve”, “default VPC — is it safe?”.

---

## 2. VPC anatomy (the minimum model)

A VPC is a logically isolated network in one Region with:

- **CIDR block** (IPv4 required, IPv6 optional) — e.g. `10.0.0.0/16`;
- **Subnets** — each in **one AZ**, with a portion of the VPC CIDR; **public** if its route table sends `0.0.0.0/0` to an **internet gateway**, otherwise private;
- **Route tables** — main (implicit for subnets without an explicit association) + custom ones;
- **Gateways** — internet gateway (IGW), NAT gateway/instance, egress-only IGW, virtual private gateway, Transit Gateway attachment;
- **Security groups** (stateful, instance/ENI level) and **network ACLs** (stateless, subnet level);
- **DNS**: Route 53 Resolver (`169.254.169.253` / VPC CIDR + 2), private hosted zones, Resolver endpoints;
- **Endpoints**: gateway endpoints (S3, DynamoDB) and interface endpoints (PrivateLink).

Traffic flow is decided by **routes** (where packets go) and **enforced** by **security groups + NACLs** (whether packets are allowed).

---

## 3. Default VPC vs newly created VPC — the details

### 3.1 What AWS creates for you in a default VPC

From the AWS docs, a default VPC includes:

1. a VPC with CIDR **`172.31.0.0/16`** (65,536 private IPv4 addresses);
2. a **`/20` default subnet in each Availability Zone** (4,096 addresses each, minus the 5 reserved);
3. an **internet gateway** attached;
4. a route in the **main route table**: `0.0.0.0/0 → IGW` (plus `172.31.0.0/16 → local`);
5. a **default security group**;
6. a **default network ACL**;
7. the account's **default DHCP options set** (AmazonProvidedDNS).

Consequences:

- Default subnets are **public**: instances get a **public IPv4** and **public + private DNS hostnames** on launch.
- A **nondefault subnet** you add to a default VPC does **not** auto-assign public IPs (unless you change the subnet attribute).
- You can modify everything (route tables, NACLs, SG rules), add CIDRs, add VPNs, add subnets; AWS may even add a default subnet for a new AZ — **unless you modified the default VPC**.
- You can make a default subnet **private** by removing the `0.0.0.0/0 → IGW` route (then instances lose internet access).
- Auto-created components are created by **AWS**, not by you — IAM policies don't block those side effects (e.g. a `Deny CreateInternetGateway` policy does not prevent the default VPC's IGW; you'd have to deny `CreateDefaultVpc` too).
- The default VPC can be **deleted**; you can later recreate a default VPC with `CreateDefaultVpc` — it will be a **new** VPC (new ID, same standard components), not the old one.

### 3.2 What a newly created VPC actually contains

When you create a VPC (console “**VPC only**”, `CreateVpc` API, or IaC):

- the VPC with **your chosen CIDR** (`/16`–`/28`);
- a **main route table** with only the `local` route;
- a **default security group** (deny inbound, allow all outbound) and **default NACL** (allow all);
- **nothing else** — no subnets, no IGW, no NAT, no endpoints;
- `enableDnsSupport = true`, **`enableDnsHostnames = false`** (so no public DNS hostnames);
- no public IPv4 assignment on launch.

To make it usable you must at minimum: **create subnets**, (for public) **attach an IGW**, **create a route table with `0.0.0.0/0 → IGW`** and **associate it with the public subnets**, enable **auto-assign public IPv4** on those subnets, and (for private subnets) add a **NAT gateway/instance** route.

> **Careful with the console:** the “**VPC and more**” wizard creates VPC + subnets + IGW + NAT + endpoints in one go. That is **not** the default behaviour of the VPC service — it's a convenience wizard. For the exam, “newly created VPC” means the bare VPC (`VPC only`).

### 3.3 Side-by-side

| Component | Default VPC | New VPC (`VPC only`) |
|---|---|---|
| CIDR | `172.31.0.0/16` (fixed) | chosen by you |
| Subnets | `/20` per AZ, public | none |
| Internet gateway | attached | none (create + attach) |
| Main route table | `local` + `0.0.0.0/0 → IGW` | `local` only |
| Public IPv4 auto-assign | **on** (default subnets) | **off** by default |
| DNS hostnames (`enableDnsHostnames`) | **true** | **false** |
| DNS resolution (`enableDnsSupport`) | true | true |
| Default SG / NACL | yes | yes |
| Outbound internet | works immediately | none until IGW/NAT + routes |
| IPv6 | not assigned by default (can associate) | not assigned by default |
| Deletion | possible; recreate via `CreateDefaultVpc` | you own it |

### 3.4 Why the exam cares

- **“Why can't my new instance reach the internet?”** In a new VPC: no IGW, no `0.0.0.0/0` route, no public IPv4, and possibly `enableDnsHostnames=false`. In a default VPC all four are already satisfied.
- **Security posture:** the default VPC is convenient but by design **public-by-default** (public IPs, open IGW). Production landing zones normally avoid it; AWS Control Tower / security best practice is to **not launch production workloads in the default VPC**, and many organisations **delete unused default VPCs** in every Region/account.
- **DNS:** with `enableDnsHostnames=false`, instances lack public DNS names; private hosted zones and interface-endpoint private DNS require both DNS attributes enabled.
- **Landing-zone questions:** “no default VPCs, no public IPs, central egress” → custom VPCs + NAT/egress firewall, or **VPC Block Public Access** to hard-block internet gateways.

### 3.5 Can you “convert” one into the other?

- You can **modify a default VPC** into a private/controlled network (remove IGW route, change CIDRs, add tables), but it keeps its fixed original CIDR and public baggage.
- You cannot make a **custom VPC** “the default VPC”. The clean pattern is: build a proper custom VPC (per environment/account, chosen CIDRs, private subnets, NAT/endpoints) and treat the default VPC as a bootstrap/dev convenience.

---

## 4. Subnets

- Each subnet belongs to **one AZ** and one VPC; CIDR must fit in the VPC range; subnet sizes `/16`–`/28`.
- **5 addresses are reserved per subnet**: network address, VPC router, DNS (VPC CIDR + 2), future use, broadcast — so a `/24` has **251 usable** IPv4 addresses.
- **Public subnet** = route table has `0.0.0.0/0 → IGW` (and ideally the subnet auto-assigns public IPs). **Private subnet** = no IGW route (egress via NAT or endpoints only).
- **Public IPv4 auto-assign** is a *subnet* attribute (default on for default subnets, off for custom ones). Even in a public subnet, an instance needs a public IP to be reachable from the internet.
- Design: at least **2 AZs** (production: 3), separate subnets for public/private tiers, non-overlapping CIDRs across VPCs/accounts (plan for peering/TGW).
- **IPv6**: associate an IPv6 CIDR with the VPC, then a `/64` with subnets; IPv6 subnets are public if routed to an IGW, private if routed to an **egress-only IGW**.

---

## 5. Routing and gateways

### 5.1 Gateway types at a glance

| Gateway | Direction | Notes |
|---|---|---|
| **Internet gateway (IGW)** | bidirectional internet | one per VPC; NAT for public IPv4; route target `0.0.0.0/0` |
| **NAT gateway** | outbound only | managed, **per-AZ**, public (EIP) or **private** (no EIP); IPv4 + IPv6 via **NAT64/DNS64**; billed hourly + per GB |
| **NAT instance** | outbound only | self-managed EC2 in a public subnet; cheaper at low throughput, but you patch/scale/HA it |
| **Egress-only IGW** | outbound only (IPv6) | IPv6-only egress; no IPv4 |
| **Virtual private gateway / TGW attachment** | hybrid/VPC-to-VPC | VPN, Direct Connect, Transit Gateway |
| **Gateway endpoint** | to S3/DynamoDB | route-table target, no internet, free |
| **Interface endpoint** | to AWS services/PrivateLink | ENI in subnet, SG, DNS, hourly + data charges |

### 5.2 NAT gateway vs NAT instance — the comparison the exam asks for

Both give **outbound-only** NAT for private subnets (no unsolicited inbound), but they differ on almost everything else. AWS recommends NAT gateways.

| Attribute | **NAT gateway** | **NAT instance** |
|---|---|---|
| Managed by | **AWS** (no patching, no OS) | **you** (OS patches, software, HA scripts) |
| Availability | **redundant within its AZ**; deploy one per AZ for an AZ-independent design | single EC2 instance; failover needs a script/Auto Scaling |
| Bandwidth | **5 Gbps baseline, auto-scales to 100 Gbps**; 1M → 10M packets/s | limited by instance type/size |
| Cost model | hourly + **per GB processed** | EC2 instance hours (+ EIP); no per-GB NAT charge |
| Sizing | uniform offering | you choose instance type and size |
| Public IP | **Elastic IP required** for a public NAT GW; private NAT GW has none | EIP or public IP (changeable) |
| Private IP | auto-selected (primary can be specified; secondary IPs possible) | you specify |
| Security groups | **not supported** | supported (on the instance and resources behind it) |
| NACLs | on the subnet | on the subnet |
| Port forwarding | **not supported** | possible with custom configuration |
| Bastion host | **not supported** | **supported** |
| IPv6 | supported via **DNS64/NAT64** | IPv4 NAT; IPv6 egress via egress-only IGW |
| Fragmentation | forwards fragmented **UDP**; **drops fragmented TCP/ICMP** | reassembles UDP, TCP and ICMP fragments |
| Timeout behaviour | sends **RST** (no FIN) to the client | sends **FIN** |
| Metrics | NAT gateway CloudWatch metrics (`ActiveConnectionCount`, `BytesIn/Out`, `PacketDropCount`, `ErrorPortAllocation`) | standard EC2 metrics |
| Connection limits | **55,000 simultaneous connections per unique destination per IPv4 address** (up to 8 IPv4 addresses per NAT GW) | instance-dependent |
| MTU | up to 8500; keep internet-path MTU ≤1500; PMTUD + MSS clamping | instance MTU |
| Type | **public** (EIP → IGW) or **private** (no EIP → TGW/VGW/other VPCs) | public subnet with EIP/public IP |

Operational facts to remember:

- A NAT gateway is created **in one AZ** and is redundant *inside that AZ*. If its AZ fails, instances in other AZs routing through it lose internet → **one NAT gateway per AZ + per-AZ route tables**.
- **You cannot route traffic to a NAT gateway through a VPC peering connection** (`Client → Peering → NAT → Internet` is unsupported). `Client → NAT → Peering → Destination` is supported (return-to-sender behaviour).
- **Site-to-Site VPN / Direct Connect via a virtual private gateway cannot route to a NAT gateway**; via a **Transit Gateway** it can.
- NAT gateway has **no security group**; filter with NACLs on its subnet and SGs on the instances behind it. NAT gateway uses **ports 1024–65535**.
- A **NAT instance requires source/destination check disabled**, a public subnet and a public IP/EIP.
- The AWS NAT AMI (Amazon Linux 2018.03) is **end-of-life** — AWS recommends migrating to a NAT gateway; if you need a NAT instance, build your own AMI from a current Amazon Linux.
- **Regional NAT gateways** (newer feature) expand automatically across AZs — check current docs if a scenario mentions them.
- Cost trap: at high traffic the **per-GB NAT gateway charge** can exceed a large NAT instance; at very low/sporadic traffic a small NAT instance can be cheaper — but with operational overhead.

### 5.3 Route-table rules

- **Longest prefix wins**; the `local` route always exists and can't be removed.
- **Main route table** applies to subnets without an explicit association; best practice is to make main private and use explicit public tables.
- A route table can be associated with many subnets; a subnet has at most one route table.
- You cannot route a subnet to an IGW in *another* VPC — peering/TGW must be used, and only the **destination** side controls its own routes.

---

## 6. Security groups vs network ACLs

| | **Security group** | **Network ACL** |
|---|---|---|
| Level | instance/ENI | subnet |
| State | **stateful** (return traffic auto-allowed) | **stateless** (must allow both directions) |
| Rules | **allow only** | **allow and deny**, numbered, evaluated in order (lowest first) |
| Default | deny all inbound, allow all outbound | allow all (default NACL) |
| Association | multiple SGs per ENI | one NACL per subnet (one NACL can serve many subnets) |
| Use | primary access control | coarse subnet-level guardrails (e.g. block a CIDR) |

Exam traps: SG can't deny; NACL can't be stateful; ephemeral ports must be allowed in NACL for return traffic; SG referencing another SG works only within the same VPC/peered VPC context.

---

## 7. VPC endpoints (private access to AWS services)

- **Gateway endpoint** — for **S3 and DynamoDB** only; added as a **route-table target**; free; no SG; used to keep S3 traffic off the internet and away from NAT costs.
- **Interface endpoint (AWS PrivateLink)** — an **ENI in your subnet** with an SG; supports most services; **private DNS** can override the public name (requires `enableDnsSupport` + `enableDnsHostnames`); billed hourly + per GB.
- **Endpoint policies** restrict which resources/actions are reachable; combine with **bucket policies** for defence in depth.
- **PrivateLink** can also expose *your* services to other VPCs/accounts without peering.

---

## 8. VPC peering and Transit Gateway (basics)

- **Peering**: one-to-one connection between two VPCs (same/other account/Region); requires non-overlapping CIDRs; **non-transitive** (A–B and B–C does not give A–C); you must add routes **on both sides** and allow traffic in SGs.
- **Transit Gateway**: hub-and-spoke router for many VPCs, VPNs and Direct Connect; **transitive**; route tables/associations/attachments; the answer for “dozens of VPCs, central connectivity, segmentation”.
- **Direct Connect / Site-to-Site VPN** connect on-premises; use TGW for scale; DX gateway for multi-Region.

---

## 9. DNS in a VPC — attributes, private zones, delegation and cross-VPC resolution

### 9.1 Two independent VPC attributes

| Attribute | Question it answers | Default | Effect when `false` |
|---|---|---|---|
| **`enableDnsSupport`** (DNS resolution) | May this VPC use the Route 53 Resolver (169.254.169.253)? | **true everywhere** | no DNS through the AWS resolver at all — public names, AWS service names and private-zone names all fail; you must supply custom DNS via DHCP options |
| **`enableDnsHostnames`** (DNS hostnames) | Do instances with a public IPv4 get a public DNS hostname, and may the VPC use private-DNS features? | **true in the default VPC, false in new VPCs** | no public DNS hostname; **private hosted zones and interface-endpoint private DNS stop working** |

They are **not the same thing**: *support* controls **whether resolution works**, *hostnames* controls **whether names are assigned to your instances / private zones are usable**. `enableDnsHostnames` only has an effect when `enableDnsSupport = true`.

| `enableDnsSupport` | `enableDnsHostnames` | Practical result |
|---|---|---|
| false | any | Resolver unusable; only custom DNS servers from the DHCP option set |
| true | false | private names (`ip-10-0-0-5.ec2.internal`) resolve; no public hostnames; no PHZ / private endpoint DNS |
| true | true | full: public hostnames, private hosted zones, interface-endpoint private DNS, auto reverse-DNS rules |

### 9.2 Private hosted zones (PHZ)

- A Route 53 zone with **private visibility**; records resolve **only inside VPCs associated with the zone**.
- Requirements in **every** VPC that must resolve it: **zone associated with the VPC** + `enableDnsSupport=true` + `enableDnsHostnames=true`.
- **Same account:** associate additional VPCs in the Route 53 console/API.
- **Cross-account:** the zone owner must first **authorize the association** (API/CLI — the console can't do it), then the VPC owner associates the VPC with the zone.
- **Split-horizon DNS:** keep a public zone and a private zone with the same name; VPC-associated resolution uses the private one.
- PHZs do not leak outside the associated VPCs — association is the whole access-control mechanism.

### 9.3 Delegation (NS records) — how far it gets you

- **Delegation in DNS** = an `NS` record in the parent zone pointing at the child zone's name servers.
- **Public zones:** works normally, including cross-account delegation (the child zone can live in another account).
- **Private zones:** the Route 53 Resolver answers a private name **only for VPCs associated with that private zone**. So an `NS` delegation **alone does not make a private child zone resolvable** from a VPC that isn't associated with it — the working pattern is: **associate the child PHZ with the VPC** (cross-account authorized if needed), optionally keeping the `NS` record in the parent zone to make ownership/split explicit.
- For **hybrid** names (on-prem domains), don't use delegation to Route 53 — use **Resolver forwarding rules** (per domain → on-prem DNS IPs through an outbound endpoint), shareable across accounts via **AWS RAM**, plus **inbound endpoints** so on-prem can resolve AWS private names.

### 9.4 DNS across VPCs: peering vs Transit Gateway

**VPC peering**

- The peering connection has a **DNS resolution option** (off by default). When enabled **and** both VPCs have DNS hostnames + resolution on, the **public IPv4 DNS hostname** of an instance in the peer VPC resolves to its **private IPv4** address.
- This option does **not** share private hosted zones: for PHZ names the zone must still be **associated** with the resolving VPC.

**Transit Gateway**

- TGW is a **Layer-3 router** — it **does not propagate or resolve DNS**. Attaching a VPC to a TGW does not make another VPC's private hosted zone resolvable, and resolver queries (169.254.169.253) are **per-VPC**, not routed through TGW.
- Options to resolve names from another VPC:
  1. **Associate the PHZ with every VPC** that needs it (directly or via cross-account authorization) — simplest and most reliable.
  2. **Central DNS VPC**: host the private zones there, add Resolver **inbound** and **outbound** endpoints, and share **forwarding rules via AWS RAM** to spoke accounts (spokes associate the shared rule with their VPCs). This is the standard hybrid/multi-account pattern.
  3. Point spoke VPCs' **DHCP `domain-name-servers`** at a DNS endpoint reachable over TGW (e.g. the central inbound endpoint IPs) — you then own the full DNS path and lose some Resolver conveniences.

### 9.5 “What makes this name resolve?” — quick matrix

| Goal | What is needed |
|---|---|
| public DNS name of a peer-VPC instance → private IP | peering + **DNS resolution enabled** on both sides + DNS enabled in both VPCs |
| name from a private hosted zone | VPC **associated** with the zone + both DNS attributes true |
| AWS service names / S3 private DNS | `enableDnsSupport`; interface endpoint with **private DNS** for the service name |
| on-prem name inside a VPC | forwarding rule → on-prem DNS via **outbound endpoint** (share rules via RAM) |
| on-prem resolving AWS private names | **inbound endpoint** + on-prem forwarders |
| VPC attached to TGW resolving another VPC's PHZ | **associate the PHZ** with that VPC (or central Resolver design) — TGW alone never suffices |
| public hostname assigned to an instance | `enableDnsHostnames=true` **and** a public IPv4 |

- You cannot filter DNS traffic to/from the Route 53 Resolver with security groups or NACLs.
- Reverse DNS for private IPs uses Resolver's **auto-defined system rules**; you can disable them and forward PTR queries elsewhere (useful with Active Directory).

---

## 10. Monitoring and troubleshooting

| Tool | Use |
|---|---|
| **VPC Flow Logs** | record accepted/rejected traffic at ENI/subnet/VPC level to CloudWatch Logs/S3; no full packet capture |
| **Reachability Analyzer** | path analysis between source and destination (static config check) |
| **Network Access Analyzer** | find unintended network access paths against a desired policy |
| **CloudWatch metrics** | NAT gateway bytes/packets, EIP status, VPN tunnels |
| **Traffic Mirroring** | copy packets for deep inspection (security appliances) |
| **VPC Block Public Access** | account/Region-level block of internet-gateway access (newer guardrail) |
| **AWS Config / Security Hub** | detect default VPC usage, open SGs, public IPs |

---

## 11. Exam traps

1. **“A new VPC has internet access by default.”** No — no IGW, no route, no public IP.
2. **“A newly created VPC gives instances public DNS names.”** No — `enableDnsHostnames=false`; enable it explicitly.
3. **“Default VPC has one subnet.”** No — one per AZ (all public).
4. **“Default VPC CIDR can be changed at creation.”** No — it is always `172.31.0.0/16`.
5. **“Subnets are AZ-independent.”** No — one subnet = one AZ.
6. **“A /24 subnet has 256 usable addresses.”** No — **251** (5 reserved).
7. **“Security groups can deny traffic.”** No — allow-only; use NACLs for deny.
8. **“NACLs are stateful.”** No — stateless; return ports must be allowed.
9. **“Peering is transitive.”** No.
10. **“You can peer VPCs with overlapping CIDRs.”** No.
11. **“Gateway endpoints work for any AWS service.”** No — S3 and DynamoDB only; use interface endpoints for the rest.
12. **“NAT gateway supports IPv6.”** No — egress-only IGW does.
13. **“Deleting the default VPC breaks my account.”** No, but it's the network behind quick-start resources; recreate with `CreateDefaultVpc` if needed.
14. **“Default VPC is a security best practice.”** No — it is a convenience; production uses custom VPCs.
15. **“`enableDnsHostnames` is what enables DNS resolution.”** No — resolution is `enableDnsSupport`; hostnames control public DNS names and private-DNS features.
16. **“Transit Gateway propagates DNS between attached VPCs.”** No — TGW is Layer 3 only; associate the private hosted zone or use central Resolver endpoints/rules.
17. **“NS delegation makes a private hosted zone resolvable from any VPC.”** No — the VPC must be associated with the private hosted zone.
18. **“Enabling DNS resolution on a peering connection shares private hosted zones.”** No — it maps public DNS hostnames to private IPs; PHZ still needs association.
19. **“A new VPC resolves private names out of the box.”** Only if a private zone is associated and `enableDnsHostnames` is on (default false).
20. **“One NAT gateway is highly available across AZs.”** No — redundant *within its AZ*; other AZs lose internet if it fails.
21. **“NAT gateways can have security groups.”** No — use NACLs on the subnet and SGs on the resources behind it.
22. **“NAT gateway doesn't support IPv6.”** It does, via **NAT64/DNS64**; for IPv6-only egress use an egress-only IGW.
23. **“A NAT instance is always cheaper.”** Only at low throughput; NAT gateway charges per GB can beat a large instance at scale.
24. **“You can route to a NAT gateway through a peering connection.”** Not supported (`Client → Peering → NAT → Internet`).

---

## 12. Practice questions

> Answer before reading §13.

**Q1.** An instance launched into a newly created VPC cannot reach the internet. Which combination explains it?
A. No IGW, no `0.0.0.0/0` route to IGW, no public IPv4 assigned
B. Security group blocks all outbound by default
C. The VPC has no DNS resolver
D. Subnets are always private in a new VPC

**Q2.** What is the IPv4 CIDR of the default VPC and the size of its subnets?
A. `10.0.0.0/16`, `/24` subnets
B. `172.31.0.0/16`, `/20` subnet per AZ
C. `192.168.0.0/16`, `/16` subnets
D. You choose both

**Q3.** Which statement about default subnets is true?
A. They are private and require NAT
B. They are public: instances get public IPv4 and public DNS hostnames
C. They never auto-assign public IPs
D. They exist only in one AZ

**Q4.** Instances in a new VPC have private IPs but no public DNS hostnames. What must you enable?
A. `enableDnsSupport` only
B. `enableDnsHostnames` (and `enableDnsSupport` must be true)
C. IPv6
D. NAT gateway

**Q5.** Why is the default VPC discouraged for production?
A. It has no internet access
B. It is public-by-default (public IPs, IGW, open routes) and lacks custom segmentation
C. It supports only one AZ
D. It cannot be modified

**Q6.** Which is stateful and allow-only?
A. Network ACL
B. Security group
C. Route table
D. DHCP option set

**Q7.** Private subnets need outbound IPv6 internet access. What do you use?
A. NAT gateway
B. Egress-only internet gateway
C. IGW
D. Gateway endpoint

**Q8.** You must connect 30 VPCs with central routing and segmentation. What is the best choice?
A. Full mesh VPC peering
B. AWS Transit Gateway
C. One VPC with many subnets
D. Direct Connect

**Q9.** Which is true about a gateway VPC endpoint for S3?
A. It creates an ENI in your subnet
B. It is a route-table target and is free
C. It works for all AWS services
D. It requires a NAT gateway

**Q10.** A subnet is `10.0.1.0/24`. How many usable IPv4 addresses?
A. 256
B. 255
C. 251
D. 250

**Q11.** You delete the default VPC. What happens to your custom VPCs?
A. They are deleted too
B. Nothing — they are independent; you can recreate a default VPC later
C. The account loses internet access
D. All subnets are detached

**Q13.** A private hosted zone must be resolvable by instances in a VPC. Which combination is required?
A. Only `enableDnsSupport=true`
B. The zone associated with the VPC **and** `enableDnsSupport=true` **and** `enableDnsHostnames=true`
C. A VPC peering connection
D. A gateway VPC endpoint

**Q15.** A production workload runs in three AZs, peaks at ~20 Gbps of egress to the internet, and the team wants no operational overhead for NAT. What should you use?
A. One NAT gateway in one AZ
B. One NAT gateway per AZ, with per-AZ private route tables
C. One large NAT instance with an Auto Scaling group
D. An internet gateway directly attached to the private subnets

**Q16.** A team needs outbound filtering with security groups plus port forwarding and a bastion host in the same device. What is the best fit?
A. NAT gateway
B. NAT instance
C. Egress-only internet gateway
D. Gateway VPC endpoint

---

## 13. Answers

**A1 — A.** A new VPC has no gateway, no internet route and no public IP assignment; the default SG actually allows all outbound.

**A2 — B.** Default VPC: `172.31.0.0/16`, one `/20` per AZ (4,096 addresses, 5 reserved).

**A3 — B.** Default subnets are public and auto-assign public IPv4 + public DNS names; nondefault subnets in a default VPC do not.

**A4 — B.** `enableDnsHostnames` is false for new VPCs; it requires `enableDnsSupport` (true by default). Both must be on for public hostnames/private DNS.

**A5 — B.** The default VPC is internet-ready and public-by-default; production landing zones use custom VPCs with private subnets, NAT/endpoints and segmentation.

**A6 — B.** Security groups: stateful, allow-only.

**A7 — B.** Egress-only IGW for IPv6; NAT gateway is IPv4-only.

**A8 — B.** Transit Gateway (transitive hub) — peering is non-transitive and a full mesh is unmanageable.

**A9 — B.** Gateway endpoints are route-table targets for S3/DynamoDB, free, no ENI.

**A10 — C.** 256 − 5 reserved = 251.

**A11 — B.** Custom VPCs are independent; the default VPC can be recreated with `CreateDefaultVpc`.

**A12 — B.** VPC Block Public Access blocks internet-gateway access at account/Region level (routes/SGs alone can be re-added).

**A13 — B.** A private hosted zone resolves only in VPCs **associated** with it, and the VPC needs both `enableDnsSupport=true` and `enableDnsHostnames=true`. Peering and gateway endpoints don't grant zone access.

**A14 — B.** TGW does not carry DNS or share private zones. Associate `corp.internal` with VPC B (authorize the association first if the zone is in another account), or centralise DNS with Resolver inbound/outbound endpoints and RAM-shared forwarding rules.

**A15 — B.** NAT gateways are redundant within an AZ and scale automatically (5 Gbps → 100 Gbps), but you need one **per AZ** with per-AZ route tables to survive an AZ failure. A single NAT instance is not HA and is bandwidth-bound.

**A16 — B.** NAT instances support security groups, port forwarding and use as a bastion host; NAT gateways support none of those (no SG, no port forwarding, no bastion).

---

## 14. Rapid recall

| Fact | Value |
|---|---|
| Default VPC CIDR | `172.31.0.0/16` |
| Default subnet size | `/20` per AZ (4,096 addresses) |
| VPC CIDR range | `/16`–`/28` |
| Reserved IPs per subnet | **5** |
| Default VPC DNS | `enableDnsHostnames=true`, `enableDnsSupport=true` |
| New VPC DNS | `enableDnsHostnames=false`, `enableDnsSupport=true` |
| Public subnet | route `0.0.0.0/0 → IGW` |
| NAT gateway | IPv4 only, in public subnet, needs EIP |
| Egress-only IGW | IPv6 outbound only |
| Default SG | deny inbound, allow all outbound (stateful) |
| Default NACL | allow all (stateless) |
| Peering | one-to-one, non-transitive, non-overlapping CIDRs |
| Gateway endpoint | S3 / DynamoDB, route target, free |
| Resolver address | `169.254.169.253`, also VPC CIDR + 2 |
| Default VPC components | VPC, subnet/AZ, IGW, main route, SG, NACL, DHCP options |
| enableDnsSupport | DNS resolution via the Route 53 Resolver — default **true** everywhere |
| enableDnsHostnames | public DNS hostnames + private-DNS features — default **true only in the default VPC** |
| PHZ requirements | VPC **associated** with the zone + `enableDnsSupport` + `enableDnsHostnames` |
| PHZ cross-account | zone owner **authorizes** the VPC association first (API/CLI) |
| Peering DNS option | public DNS hostname → private IP; off by default; both VPCs need DNS enabled |
| TGW and DNS | TGW does **not** propagate DNS — associate the PHZ or use central Resolver endpoints + RAM-shared rules |
| NAT gateway | **per-AZ**, redundant within the AZ; **5 → 100 Gbps**; 1M → 10M pps; **55,000 conns**/destination/IPv4; **no SG** |
| NAT gateway vs NAT instance | managed vs self-managed; SG / port forwarding / bastion = **instance only**; timeout sends **RST** vs **FIN** |
| NAT gateway limits | ports **1024–65535**; **no** route via peering (`Client→Peering→NAT`); **no** route from VPN/DX via VGW (via TGW yes) |
| NAT instance | EC2 in a public subnet, **source/destination check disabled**, bandwidth = instance type |

---

## 15. Glossary

| Term | Meaning |
|---|---|
| **VPC** | logically isolated network in one Region with a CIDR |
| **Default VPC** | pre-built per-Region VPC (172.31.0.0/16) with public subnets and IGW |
| **Subnet** | range of IPs in one AZ; public or private by route table |
| **Main route table** | route table used by subnets without an explicit association |
| **Internet gateway (IGW)** | bidirectional NAT for public IPv4 |
| **NAT gateway** | managed outbound-only NAT, per-AZ, public (EIP) or private; IPv6 via NAT64 |
| **NAT instance** | self-managed EC2 doing NAT; supports SGs, port forwarding, bastion |
| **Egress-only IGW** | outbound-only IPv6 gateway |
| **Security group** | stateful, allow-only firewall on ENIs |
| **NACL** | stateless, allow/deny firewall on subnets |
| **Gateway endpoint** | route-table target for S3/DynamoDB private access |
| **Interface endpoint** | ENI-based PrivateLink endpoint with SG and private DNS |
| **Transit Gateway** | transitive hub for VPC/VPN/DX connectivity |
| **enableDnsSupport / enableDnsHostnames** | VPC-level DNS resolution / public-hostname attributes |
| **VPC Block Public Access** | account/Region guardrail blocking IGW-based internet access |
| **Flow Logs** | traffic records at ENI/subnet/VPC level |
| **enableDnsSupport** | VPC attribute controlling DNS resolution via the Route 53 Resolver |
| **enableDnsHostnames** | VPC attribute controlling public DNS hostnames and private-DNS features |
| **Private hosted zone (PHZ)** | Route 53 zone resolvable only inside VPCs associated with it |
| **Resolver endpoint** | **inbound** (DNS into AWS) or **outbound** (DNS from AWS to your network) |
| **Forwarding rule** | Resolver rule sending a domain's queries to target DNS IPs; shareable via AWS RAM |

---

## 16. Sources (verified November 2026)

- [Default VPCs](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)
- [Default VPC components](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc-components.html)
- [Default subnets](https://docs.aws.amazon.com/vpc/latest/userguide/default-subnet.html)
- [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/create-vpc.html)
- [Understanding Amazon DNS / DNS attributes](https://docs.aws.amazon.com/vpc/latest/userguide/AmazonDNS-concepts.html)
- [VPC DNS attributes — view and update](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns-updating.html)
- [Associating more VPCs with a private hosted zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-associate-vpcs.html)
- [Associating a VPC with a private hosted zone in a different account](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-associate-vpcs-different-accounts.html)
- [Managing Resolver forwarding rules (and sharing via RAM)](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-rules-managing.html)
- [Enable DNS resolution for a VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-dns.html)
- [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [NAT gateway basics (bandwidth, connections, MTU)](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html)
- [Compare NAT gateways and NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html)
- [NAT instances](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)
- [AWS Certified Solutions Architect – Professional exam guide (SAP-C02, PDF)](https://docs.aws.amazon.com/pdfs/aws-certification/latest/solutions-architect-professional-02/solutions-architect-professional-02.pdf)
