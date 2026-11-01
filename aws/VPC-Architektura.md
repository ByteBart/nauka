# 📚 AWS SAP-C02 — Solutions Architect Professional Study Book

> **Author:** Claude AI | **Target Exam:** AWS Certified Solutions Architect – Professional (SAP-C02)  
> **Level:** Professional (assumes Associate-level knowledge)  
> **Language:** English  
> **Started:** 2026-10-28

---

> 💡 **How to use this book:**  
> Each chapter covers a service or topic in depth. Every chapter includes:  
> service overview · comparison with alternatives · reliability & security · Well-Architected Framework · 4 exam domains · limitations  
> Questions are accumulated in the **Test Your Knowledge** chapter at the end.

---

## 📋 Table of Contents

<!-- TOC auto-generated — updated with each new chapter -->

- [📚 AWS SAP-C02 — Solutions Architect Professional Study Book](#-aws-sap-c02--solutions-architect-professional-study-book)
  - [📋 Table of Contents](#-table-of-contents)
  - [🌐 Chapter 1: AWS Direct Connect (DX)](#-chapter-1-aws-direct-connect-dx)
  - [🏗️ Chapter 2: Private and Public Subnets](#️-chapter-2-private-and-public-subnets)
  - [🚪 Chapter 3: Internet Gateway and Alternatives](#-chapter-3-internet-gateway-and-alternatives)
  - [🔀 Chapter 4: AWS Transit Gateway (TGW)](#-chapter-4-aws-transit-gateway-tgw)
  - [📊 Chapter 5: VPC Flow Logs](#-chapter-5-vpc-flow-logs)
  - [🔐 Chapter 6: Remote Network to VPC Connectivity](#-chapter-6-remote-network-to-vpc-connectivity)
  - [🌍 Chapter 7: AWS Resource Scope — Global, Regional, Zonal and Edge](#-chapter-7-aws-resource-scope--global-regional-zonal-and-edge)
  - [🗄️ Chapter 8: Amazon Aurora](#️-chapter-8-amazon-aurora)
  - [🐘 Chapter 9: Amazon RDS](#-chapter-9-amazon-rds)
  - [⚡ Chapter 10: Amazon DynamoDB](#-chapter-10-amazon-dynamodb)
  - [📊 Chapter 11: Amazon Redshift](#-chapter-11-amazon-redshift)
  - [🔗 Chapter 12: Amazon Neptune](#-chapter-12-amazon-neptune)
  - [🚀 Chapter 13: Amazon ElastiCache](#-chapter-13-amazon-elasticache)
  - [📄 Chapter 14: Amazon DocumentDB](#-chapter-14-amazon-documentdb)
  - [📒 Chapter 15: Amazon QLDB](#-chapter-15-amazon-qldb)
  - [🔀 Chapter 16: VGW vs Direct Connect Gateway vs Transit Gateway — On-Premises Connectivity](#-chapter-16-vgw-vs-direct-connect-gateway-vs-transit-gateway--on-premises-connectivity)
  - [🔒 Chapter 17: Traffic Segmentation in AWS](#-chapter-17-traffic-segmentation-in-aws)
  - [💾 Chapter 18: AWS Backup](#-chapter-18-aws-backup)
  - [🔄 Chapter 19: AWS DataSync](#-chapter-19-aws-datasync)
  - [🌍 Chapter 20: Cross-Region Replication and Multi-Region Architecture](#-chapter-20-cross-region-replication-and-multi-region-architecture)
  - [🧪 Test Your Knowledge](#-test-your-knowledge)

---

## 🌐 Chapter 1: AWS Direct Connect (DX)

### 📖 Overview

AWS Direct Connect (DX) is a **dedicated, private network connection** between your on-premises data center (or colocation facility) and AWS. Unlike VPN over the internet, DX bypasses the public internet entirely — traffic travels over a private fiber link to an AWS Direct Connect location (DX PoP), then into the AWS backbone network.

> 🔑 **Exam key:** DX = consistent latency, high bandwidth, private connection. VPN = encrypted, over internet, variable latency. They are complementary, not mutually exclusive.

**Scope:** DX is a **regional** service. DX Gateways extend it globally.

---

### 🔌 Physical and Logical Setup

```
On-Premises DC → (your fiber/MPLS) → DX Location (PoP) → AWS Region
```

1. **Cross-connect** — physical fiber between your router and AWS DX router at the colocation facility
2. **Virtual Interface (VIF)** — logical 802.1Q VLAN on top of the physical connection
3. **BGP session** — routing protocol between your router and AWS

You do NOT need to be physically present at a DX location — you can use a **Direct Connect Partner** (Equinix, AT&T, Verizon, etc.) who has existing cross-connects with AWS.

---

### 🔗 Connection Types

| Type | Bandwidth Options | Who Provisions | Port Ownership |
|------|------------------|----------------|----------------|
| **Dedicated Connection** | 1 Gbps, 10 Gbps, 100 Gbps | AWS directly | You own the port |
| **Hosted Connection** | 50 Mbps – 10 Gbps (via partner) | DX Partner | Partner owns port |
| **Hosted VIF** | Shared port, single VIF | DX Partner | Partner owns port, shares capacity |

> 🔑 **Exam key:** Dedicated = SLA from AWS. Hosted = SLA from partner. Sub-1Gbps = must use hosted. 100Gbps = only dedicated.

---

### 🔀 Virtual Interfaces (VIFs)

| VIF Type | Connects To | Use Case |
|----------|-------------|----------|
| **Private VIF** | VPC via Virtual Private Gateway (VGW) or DX Gateway | Access private resources in VPC |
| **Public VIF** | AWS public services (S3, DynamoDB, EC2 public IPs) | Access AWS public endpoints without internet |
| **Transit VIF** | Transit Gateway via DX Gateway | Access multiple VPCs, Transit Gateway networks |

> ⚠️ **Limitation:** One Private VIF per VGW per DX connection. Use DX Gateway for multi-VPC.

---

### 🌍 Direct Connect Gateway (DXGW)

DX Gateway is a **globally scoped** resource that lets you use a **single DX connection to reach VPCs in multiple AWS Regions**.

```mermaid
graph LR
    OnPrem["🏢 On-Premises"] --> DXLoc["📍 DX Location"]
    DXLoc --> DXGW["🌍 DX Gateway\n(Global)"]
    DXGW --> VPC1["🔷 VPC us-east-1"]
    DXGW --> VPC2["🔷 VPC eu-west-1"]
    DXGW --> VPC3["🔷 VPC ap-southeast-1"]
    DXGW --> TGW["🔗 Transit Gateway"]
    TGW --> VPC4["🔷 VPC A"]
    TGW --> VPC5["🔷 VPC B"]
```

> 🔑 **Exam key:** DX Gateway does NOT allow VPC-to-VPC routing. It only provides on-prem → VPC connectivity. For VPC-to-VPC, add Transit Gateway.

**Limits:**
- Up to 10 VGWs (or 3 TGWs) per DX Gateway
- Up to 20 DX connections per DX Gateway

---

### ⚡ Link Aggregation Groups (LAG)

LAG bundles **multiple DX connections** into one logical connection for:
- Increased bandwidth (e.g., 4 × 10 Gbps = 40 Gbps logical)
- Active/active load sharing (LACP protocol)

> ⚠️ All connections in a LAG must be: same bandwidth, same DX location, same AWS device. Max 4 connections per LAG (Dedicated), 2 for Hosted.

---

### 🛡️ Resiliency Models

| Model | Setup | Use Case |
|-------|-------|----------|
| **No Resiliency** | Single connection, single location | Dev/test only |
| **Development** | Two connections, same location | Better than nothing |
| **High Resiliency** | Two connections, two locations | Production workloads |
| **Maximum Resiliency** | Two connections at two locations + two separate devices | Mission-critical (financial, healthcare) |

```mermaid
graph TD
    A["Maximum Resiliency"] --> B["DX Location 1\nDevice A\nConnection 1"]
    A --> C["DX Location 1\nDevice B\nConnection 2"]
    A --> D["DX Location 2\nDevice A\nConnection 3"]
    A --> E["DX Location 2\nDevice B\nConnection 4"]
    B --> VPC["AWS VPC"]
    C --> VPC
    D --> VPC
    E --> VPC
```

---

### 🔄 DX + Site-to-Site VPN as Backup

```mermaid
graph LR
    OnPrem["🏢 On-Premises"] -->|"Primary: DX\n(private, fast)"| VGW["Virtual Private\nGateway"]
    OnPrem -->|"Backup: VPN\n(encrypted, internet)"| VGW
    VGW --> VPC["🔷 VPC"]
```

**BGP failover mechanism:**
- Use **MED (Multi-Exit Discriminator)** to prefer DX over VPN
- Use **AS Path Prepending** to make VPN less preferred from AWS side
- DX fails → BGP withdraws routes → VPN takes over automatically

> 🔑 **Exam key:** DX does NOT encrypt traffic by default. For encryption over DX, run **IPSec VPN over the DX connection** (using a Public VIF to the VGW). This gives you private connectivity + encryption.

---

### 📡 BGP Routing Details

| Concept | Description |
|---------|-------------|
| **BGP ASN** | You need a private or public ASN for your router |
| **MED** | Lower MED = more preferred path. Set on DX to prefer it over VPN |
| **AS Path Prepending** | Append your ASN multiple times to make a path less preferred |
| **BGP Communities** | AWS uses communities to control route propagation scope |
| **Route Summarization** | Advertise aggregated prefixes to reduce BGP table size |

AWS advertises up to **1,000 routes** to your router per BGP session. Exceeding this drops the BGP session.

---

### 💰 Pricing Model

| Component | Cost |
|-----------|------|
| Port hour | Per hour for dedicated connection (1G/10G/100G) |
| Data transfer OUT | Per GB (lower than internet rates) |
| Data transfer IN | Free |
| VIF creation | Free |
| DX Gateway | Free (pay for data transfer) |

> 💡 DX data transfer OUT is ~50-70% cheaper than internet. Break-even at high sustained transfer volumes.

---

### 🆚 Direct Connect vs Alternatives

| Feature | DX | Site-to-Site VPN | Client VPN |
|---------|----|--------------------|------------|
| **Connection type** | Physical/dedicated | IPSec over internet | TLS over internet |
| **Bandwidth** | Up to 100 Gbps | Up to 1.25 Gbps | Per-user |
| **Latency** | Consistent, low | Variable | Variable |
| **Encryption** | No (add VPN on top) | Yes (IPSec) | Yes (TLS) |
| **Setup time** | Weeks–months | Minutes–hours | Minutes–hours |
| **Use case** | Hybrid cloud, migrations | Backup, small offices | Remote users |
| **Cost** | High fixed + transfer | Low fixed + transfer | Per-user hour |
| **SLA** | 99.99% (max resiliency) | No AWS SLA | No AWS SLA |

---

### ☁️ Azure Equivalent: ExpressRoute

| Feature | AWS Direct Connect | Azure ExpressRoute |
|---------|-------------------|--------------------|
| **Service** | Direct Connect | ExpressRoute |
| **Private peering** | Private VIF → VGW | Private Peering → VNet Gateway |
| **Microsoft peering** | Public VIF | Microsoft Peering |
| **Global reach** | DX Gateway | ExpressRoute Global Reach |
| **Encryption** | Optional (MACsec on Dedicated) | Optional (MACsec) |
| **Max bandwidth** | 100 Gbps | 100 Gbps (ExpressRoute Direct) |
| **Redundancy** | LAG + multiple locations | Redundant circuits required |
| **Pricing model** | Port + data transfer | Circuit + gateway + data |

> **MACsec (Layer 2 encryption):** Available on DX Dedicated connections (10G/100G). Encrypts at L2, before traffic even enters AWS network. Azure also supports MACsec on ExpressRoute Direct.

---

### 🐧 Open Source / Linux Equivalent

| Concept | AWS DX | Real World |
|---------|--------|------------|
| Physical layer | Cross-connect fiber | Dark fiber, MPLS circuit |
| Routing protocol | BGP | BGP (Quagga, FRRouting, BIRD) |
| Logical multiplexing | 802.1Q VLANs (VIFs) | 802.1Q trunk ports |
| Redundancy | LAG (LACP) | Linux bonding (mode 4 = LACP) |
| Traffic engineering | MED, AS prepend | BGP communities, local preference |

---

### 🏛️ Well-Architected Framework — Direct Connect

| Pillar | Consideration |
|--------|--------------|
| **Operational Excellence** | Use CloudWatch metrics for DX (BpsIngress, BpsEgress, ConnectionState). Set alarms on BGP flaps. |
| **Security** | DX traffic is NOT encrypted by default — use MACsec (Dedicated) or IPSec VPN over DX for encryption. Use Private VIF, never Public VIF for sensitive workloads unless needed. |
| **Reliability** | Use maximum resiliency model (2 locations × 2 devices). Add Site-to-Site VPN as tertiary backup. Test failover regularly. |
| **Performance Efficiency** | Use LAG for bandwidth aggregation. Choose DX location geographically close to your DC. Use 100G for high-throughput migrations. |
| **Cost Optimization** | Compare DX vs VPN break-even (usually >1 TB/month justifies DX). Use Hosted connections for lower initial commitment. Data IN is free. |
| **Sustainability** | DX reduces reliance on internet routing, fewer hops = less energy. Consolidate multiple connections via DX Gateway instead of individual circuits. |

---

### 📋 SAP-C02 Domain Coverage — Direct Connect

#### Domain 1: Design Solutions for Organizational Complexity
- Multi-account access via DX Gateway + Transit Gateway
- DX shared across AWS Organizations using Resource Access Manager (RAM)
- Centralized network account pattern with DX terminating in shared VPC

#### Domain 2: Design for New Solutions
- Choosing DX vs VPN for hybrid architecture based on bandwidth/latency SLA
- MACsec for encryption compliance requirements
- BGP design for deterministic routing (MED, communities)

#### Domain 3: Continuous Improvement for Existing Solutions
- Upgrading from VPN to DX for production workloads
- Adding DX as backup to existing DX (resiliency improvements)
- Moving from Hosted to Dedicated connection as bandwidth grows

#### Domain 4: Accelerate Workload Migration and Modernization
- DX 100G for large-scale data migration (DataSync over DX)
- Snow family + DX as complementary migration strategies
- Direct Connect for VMware Cloud on AWS connectivity

---

### 🔌 Connections — Deep Dive

#### Provisioning Flow

```
1. Request connection in AWS Console (choose Dedicated or Hosted)
   ↓
2. AWS issues LOA-CFA (Letter of Authorization – Connecting Facility Assignment)
   within 72 hours (Dedicated only)
   ↓
3. You (or your colocation provider) arrange physical cross-connect at DX facility
   using the LOA-CFA
   ↓
4. Connection state: ordering → pending → available
   ↓
5. Create Virtual Interface (VIF) on the connection
   ↓
6. Configure BGP on your router using IPs/ASN from VIF creation
   ↓
7. BGP session comes up → routes exchanged → traffic flows
```

#### Connection Types — Full Comparison

| Feature | Dedicated Connection | Hosted Connection | Hosted VIF |
|---------|---------------------|-------------------|------------|
| **Provisioned by** | AWS directly | DX Partner | DX Partner |
| **Port ownership** | You own the port | Partner owns the port | Partner owns the port |
| **Bandwidth options** | 1G, 10G, 100G only | 50 Mbps – 10 Gbps (flexible) | Shared partner port capacity |
| **Sub-1G possible** | ❌ No | ✅ Yes | ✅ Yes (best-effort) |
| **100G possible** | ✅ Yes | ❌ No | ❌ No |
| **LOA-CFA** | ✅ AWS issues to you | ❌ Partner handles | ❌ Not applicable |
| **SLA** | AWS SLA | Partner SLA (not AWS) | Partner SLA (not AWS) |
| **MACsec support** | ✅ Yes (10G/100G only) | ❌ No | ❌ No |
| **VIFs allowed** | Up to 50 VIFs | Exactly 1 VIF | Exactly 1 (it IS the VIF) |
| **Bandwidth guaranteed** | ✅ Yes (dedicated port) | ✅ Yes (dedicated bandwidth tier) | ❌ No (shared, best-effort) |
| **Physical presence at colo** | Required (or via agent) | ❌ Not required | ❌ Not required |
| **Setup time** | Weeks–months | Days–weeks | Days |
| **Best for** | High bandwidth, full control, compliance | Quick start, sub-1G, no colo presence | Low-cost testing, non-critical |

> 🔑 **Exam key traps:**
> 1. **100G = Dedicated only** — no hosted 100G exists
> 2. **MACsec = Dedicated only** (10G/100G) — L2 wire-speed encryption
> 3. **Sub-1G = Hosted only** — e.g. 100Mbps, 500Mbps require a DX Partner
> 4. **Hosted = 1 VIF max** — cannot create multiple VIFs on a hosted connection
> 5. **SLA source differs** — AWS SLA on Dedicated; partner SLA on Hosted — critical for compliance requirements
> 6. **Hosted VIF ≠ Hosted Connection** — Hosted VIF has NO guaranteed bandwidth (shared); Hosted Connection has a dedicated bandwidth tier

**Hosted VIF explained separately:**
A Hosted VIF is the most constrained option — the DX Partner gives you a single VIF on their already-shared port. You get ONE VIF, bandwidth is not guaranteed (best-effort shared), and you cannot add more VIFs. Use only for initial testing or non-critical workloads. For production: use at minimum a Hosted Connection.

#### Connection States

| State | Meaning |
|-------|---------|
| `ordering` | Request submitted, AWS processing |
| `pending` | LOA-CFA issued, waiting for cross-connect |
| `available` | Physical link up, ready for VIF creation |
| `down` | Physical layer issue |
| `deleted` | Connection removed |

#### MACsec (Layer 2 Encryption)

- Encrypts at **Layer 2** — before traffic enters AWS network
- Available only on **Dedicated 10G and 100G** connections
- Uses **CAK (Connectivity Association Key)** and **CKN (Connectivity Association Key Name)** pair
- Must be configured **before data flows** — enable on connection + configure on router
- Provides wire-speed encryption with no throughput penalty
- Not available on Hosted connections or 1G Dedicated

> 🔑 **Exam key:** MACsec = L2 encryption = Dedicated 10G/100G only. For encryption on Hosted or 1G, use IPSec VPN over Public VIF. For sub-1G bandwidth requirement = must use Hosted connection.

> ⚠️ **Exam trap:** 100G connections = Dedicated only. If question mentions 100G DX → only Dedicated applies.

---

### 🛡️ Resiliency Models — Deep Dive

AWS provides the **Resiliency Toolkit** in the DX console — a wizard that guides you through selecting and validating your resiliency model. It checks your configuration against best practices.

#### Four Resiliency Models

| Model | Connections | Locations | Devices | Survives | SLA |
|-------|------------|-----------|---------|----------|-----|
| **No Resiliency** | 1 | 1 | 1 | Nothing | None |
| **Development** | 2 | 1 | 2 | Single device failure | None |
| **High Resiliency** | 2 | 2 | 2 | Full location failure | 99.9% |
| **Maximum Resiliency** | 4 | 2 | 4 (2 per location) | Location + device failure simultaneously | 99.99% |

```mermaid
graph TD
    subgraph "No Resiliency"
        NR_OP["On-Prem"] --> NR_L1["DX Loc 1\nDevice A\nConn 1"]
        NR_L1 --> NR_VPC["VPC"]
    end

    subgraph "Development"
        DEV_OP["On-Prem"] --> DEV_L1A["DX Loc 1\nDevice A\nConn 1"]
        DEV_OP --> DEV_L1B["DX Loc 1\nDevice B\nConn 2"]
        DEV_L1A --> DEV_VPC["VPC"]
        DEV_L1B --> DEV_VPC
    end

    subgraph "High Resiliency"
        HR_OP["On-Prem"] --> HR_L1["DX Loc 1\nConn 1"]
        HR_OP --> HR_L2["DX Loc 2\nConn 2"]
        HR_L1 --> HR_VPC["VPC"]
        HR_L2 --> HR_VPC
    end

    subgraph "Maximum Resiliency"
        MR_OP["On-Prem"] --> MR_L1A["Loc 1\nDevice A\nConn 1"]
        MR_OP --> MR_L1B["Loc 1\nDevice B\nConn 2"]
        MR_OP --> MR_L2A["Loc 2\nDevice A\nConn 3"]
        MR_OP --> MR_L2B["Loc 2\nDevice B\nConn 4"]
        MR_L1A --> MR_VPC["VPC"]
        MR_L1B --> MR_VPC
        MR_L2A --> MR_VPC
        MR_L2B --> MR_VPC
    end
```

#### Adding VPN as Tertiary Backup

```mermaid
graph LR
    OP["On-Prem"] -->|"Primary: DX (MED=100)"| VGW["VGW/TGW"]
    OP -->|"Secondary: DX location 2 (MED=200)"| VGW
    OP -->|"Tertiary: VPN (MED=300 + AS prepend)"| VGW
    VGW --> VPC["VPC"]
```

BGP preference order (lower MED = more preferred):
1. DX Location 1 — MED 100
2. DX Location 2 — MED 200
3. Site-to-Site VPN — MED 300 + AS path prepended

> 🔑 **Exam key:** Maximum Resiliency = 99.99% SLA = 4 connections, 2 locations, 2 devices per location. High Resiliency = 99.9% = 2 connections at 2 locations. Development = 2 connections SAME location = no location-level HA.

---

### 🔀 Virtual Interfaces (VIFs) — Deep Dive

VIFs are **logical 802.1Q VLAN** interfaces on top of a physical DX connection. Each VIF has its own VLAN ID, BGP session, and routing configuration.

#### Private VIF

Connects your on-premises network to **private resources inside a VPC**.

| Property | Value |
|----------|-------|
| **Connects to** | Virtual Private Gateway (VGW) or DX Gateway |
| **IP type** | Private IPs only |
| **BGP peers** | Private or link-local IPs |
| **Max prefixes advertised to AWS** | 100 |
| **Max prefixes received from AWS** | 200 |
| **VLAN ID** | 1–4094 (unique per connection) |
| **Access** | VPC private resources only |
| **Cannot access** | S3 public endpoints, public AWS services |

> ⚠️ **BGP prefix limit:** Exceeding 100 advertised prefixes → BGP session drops. Use route summarization on your router.

#### Public VIF

Connects to **all AWS public IP ranges** globally — not tied to a VPC.

| Property | Value |
|----------|-------|
| **Connects to** | AWS public network (all regions) |
| **IP type** | Public IPs on both sides |
| **BGP peers** | Public IPs |
| **Access** | S3, DynamoDB, EC2 public IPs, CloudFront, all AWS public services |
| **Scope** | ALL AWS regions (not just the region where DX is) |
| **Use case** | S3 access without VPC, IPSec VPN over DX (for encryption) |

> 🔑 **Exam key:** Public VIF ≠ internet. It's a private path to AWS public endpoints. Use it for S3/DynamoDB without a VPC, or to establish VPN over DX for encryption.

> ⚠️ **BGP table size:** AWS advertises thousands of public prefixes via Public VIF — ensure your router can handle a large BGP table.

#### Transit VIF

Connects to a **DX Gateway associated with Transit Gateway**.

| Property | Value |
|----------|-------|
| **Connects to** | DX Gateway → Transit Gateway |
| **Requires** | DX Gateway (cannot connect directly to TGW) |
| **Enables** | Multi-region, multi-VPC access from single DX |
| **Replaces** | Multiple Private VIFs to multiple VGWs |
| **VIF limit** | 1 Transit VIF per DX connection per DX Gateway |

#### VIF Comparison Table

| Feature | Private VIF | Public VIF | Transit VIF |
|---------|------------|------------|-------------|
| **Destination** | VGW / DX GW | AWS public network | DX GW → TGW |
| **IPs** | Private | Public | Private |
| **Multi-VPC** | No (1 VGW) | N/A | Yes (via TGW) |
| **Multi-region** | Via DX GW | Yes (all regions) | Yes (via TGW) |
| **AWS services** | VPC resources only | All public services | VPC resources via TGW |
| **Cross-account VIF** | ✅ | ✅ | ✅ |
| **Max per Dedicated** | 50 | 50 | 50 |
| **Max per Hosted** | 1 | 1 | 1 |
| **Encryption** | No (add MACsec/VPN) | No | No |

> 🔑 **Exam key:** Hosted connection = exactly 1 VIF. Dedicated = up to 50 VIFs. Cross-account VIF allocation: owner creates VIF in their account but assigns it to another AWS account ID.

---

### ⚡ Link Aggregation Groups (LAGs) — Deep Dive

LAG bundles multiple physical DX connections into one **logical interface** using **LACP (802.3ad)** protocol.

#### Requirements — All Must Match

| Requirement | Detail |
|------------|--------|
| **Bandwidth** | All connections must be same speed (e.g., all 10G) |
| **Location** | All connections must be at same DX location |
| **AWS device** | All connections must terminate on same AWS device |
| **Connection type** | All Dedicated OR all Hosted (cannot mix) |

> ⚠️ **Critical limit:** LAG = same DX location = SPOF for location failure. LAG provides bandwidth + device-HA only, NOT location HA.

#### LAG Limits

| Parameter | Dedicated | Hosted |
|-----------|-----------|--------|
| **Max connections** | 4 | 2 |
| **Min active links** | Configurable (1–max) | Configurable |

#### Active/Active Behavior

- All links carry traffic **simultaneously** (not active/standby)
- LACP negotiates which ports participate
- Traffic hashed across links (by src/dst IP, ports — ECMP-style)
- One link fails → traffic redistributes to remaining links automatically

#### Min Links Setting

Set **minimum number of active links** for LAG to remain up. If active links drop below minimum → entire LAG goes down:
- Prevents split-brain where LAG operates at reduced capacity
- Example: 4-link LAG with min=2 → if 3 links fail, LAG goes down rather than limping on 1 link

#### VIFs on LAG

VIFs are created **on the LAG**, not on individual connections. When a link fails, VIFs remain up (still on LAG) — no BGP reconvergence needed.

```mermaid
graph LR
    OP["On-Prem Router"] -->|"10G fiber 1"| DEV["AWS DX Device\n(same location)"]
    OP -->|"10G fiber 2"| DEV
    OP -->|"10G fiber 3"| DEV
    OP -->|"10G fiber 4"| DEV
    DEV -->|"40G LAG (logical)"| VIF["VIF (on LAG)"]
    VIF --> VGW["VGW → VPC"]
```

#### LAG vs Multiple Independent Connections

| Aspect | LAG | Independent Connections |
|--------|-----|------------------------|
| **Bandwidth** | Combined (e.g., 4×10G = 40G logical) | Per-connection (use BGP ECMP) |
| **Management** | Single logical interface | Multiple VIFs to manage |
| **Failover** | Automatic (within LAG) | BGP reconvergence required |
| **Location HA** | ❌ (same location) | ✅ (can span locations) |
| **Device HA** | ✅ (different devices allowed) | ✅ |
| **BGP sessions** | One per VIF (on LAG) | One per VIF per connection |

> 🔑 **Exam key:** LAG = bandwidth aggregation + device HA. Does NOT give location HA. For location HA + bandwidth: use LAG at each location + multi-location setup.

---

### 🌍 Direct Connect Gateway (DXGW) — Deep Dive

DX Gateway is a **globally scoped** resource — it exists outside any region and can connect DX locations to VPCs in **any AWS region**.

#### The Problem DXGW Solves

**Without DXGW** — each VPC needs its own Private VIF:
```
DX connection → Private VIF → VGW → ONE VPC (one region only)
```
5 VPCs in 3 regions = 5 separate Private VIFs + 5 VGWs. Complex and expensive.

**With DXGW** — one Private VIF reaches everything:
```
DX connection → Private VIF → DXGW → VGW-1 (VPC-A, us-east-1)
                                    → VGW-2 (VPC-B, eu-west-1)
                                    → VGW-3 (VPC-C, ap-southeast-1)
                                    → TGW   (hundreds of VPCs via Transit VIF)
```

#### Key Properties

| Property | Value |
|----------|-------|
| **Scope** | Global (no region) |
| **Cost** | Free (pay data transfer only) |
| **Max DX connections** | 20 |
| **Max VGW associations** | 10 |
| **Max TGW associations** | 3 |
| **VIF type required** | Private VIF or Transit VIF |
| **Cross-account** | ✅ (share via RAM or manual proposal/accept) |

#### Association Types — Mutually Exclusive

> ⚠️ **Critical limitation:** Cannot mix VGW and TGW associations on the same DXGW. Choose one model.

| Mode | VIF Type | Use Case |
|------|----------|----------|
| DXGW + VGW | Private VIF | Single VPC per VGW, up to 10 VPCs |
| DXGW + TGW | Transit VIF | Multiple VPCs per TGW, multi-region |

#### Routing Behavior

- DXGW routes **on-prem → VPC only**
- Does NOT route VPC → VPC (inter-VPC traffic)
- For VPC-to-VPC: add Transit Gateway behind DXGW
- **Allowed prefixes**: explicitly configure which VPC CIDRs DXGW advertises to on-prem — prevents accidental route leakage

```mermaid
graph LR
    OP["🏢 On-Prem"] --> DX["DX Connection"]
    DX --> DXGW["🌍 DX Gateway\n(Global)"]
    DXGW --> VGW1["VGW → VPC-A\nus-east-1"]
    DXGW --> VGW2["VGW → VPC-B\neu-west-1"]
    DXGW --> TGW["TGW\nap-southeast-1"]
    TGW --> VPC_C["VPC-C"]
    TGW --> VPC_D["VPC-D"]
```

> 🔑 **Exam key:** DXGW does NOT allow VPC-A to reach VPC-B via DXGW. Add TGW for VPC-to-VPC routing. DXGW is NOT regional — one DXGW serves all regions.

#### DXGW alone vs DXGW + TGW

| Scenario | Use DXGW alone | Use DXGW + TGW |
|----------|---------------|----------------|
| Access ≤10 VPCs, no VPC-to-VPC | ✅ Simpler | Overkill |
| Access many VPCs | Limited (10 VGW max) | ✅ Scale |
| VPC-to-VPC via DX path | ❌ Not supported | ✅ TGW handles it |
| Multi-region VPC access | ✅ | ✅ |
| SD-WAN/Connect attachment | ❌ | ✅ |

---

### 🔒 Virtual Private Gateways (VGW) — Deep Dive

VGW is the **AWS-side termination point** for VPN tunnels and DX Private VIFs — attached to a single VPC.

#### Key Properties

| Property | Value |
|----------|-------|
| **Scope** | Regional (per VPC) |
| **Attachment** | One VPC at a time |
| **ASN** | Custom private ASN, set at creation — **cannot change** |
| **Default ASN** | 64512 (if not specified) |
| **Cost** | Free resource (pay for VPN/DX transfer) |
| **VPN tunnels** | Supports Site-to-Site VPN |
| **DX** | Supports Private VIF (direct or via DXGW) |

#### Route Propagation

VGW can **auto-propagate routes** into VPC route tables:
- Must enable "Route Propagation" on each route table
- Routes learned via BGP (DX or VPN) appear automatically
- Without propagation: must add static routes manually

> ⚠️ **Exam trap:** Route propagation must be **explicitly enabled per route table**. A new route table does NOT inherit propagation from the main route table.

#### VGW vs DXGW vs TGW

| Feature | VGW | DX Gateway | Transit Gateway |
|---------|-----|------------|-----------------|
| **Scope** | Single VPC | Global, multi-VPC | Regional, multi-VPC |
| **VPCs supported** | 1 | 10 (via VGW) / many (via TGW) | 5000 attachments |
| **VPN support** | ✅ | ❌ (use TGW or VGW) | ✅ |
| **DX support** | ✅ (Private VIF) | ✅ (Private/Transit VIF) | ✅ (Transit VIF via DXGW) |
| **VPC-to-VPC routing** | ❌ | ❌ | ✅ |
| **Cross-region** | ❌ | ✅ (via DXGW) | ✅ (via TGW peering) |
| **Cost** | Free | Free | Per attachment-hour + data |

> 🔑 **Exam key:** VGW = single VPC gateway for VPN/DX. DXGW = global hub for DX to multi-VPC. TGW = regional hub for any-to-any routing. VGW ASN cannot be changed after creation.

---

### 🤝 AWS Direct Connect — Partner Interconnect (APN)

AWS does not own the "last mile" — a network of **AWS Partner Network (APN) members** provides physical access to DX locations.

#### How the Partner Model Works

```
Customer DC → (last mile circuit) → Partner PoP/Exchange → (partner cross-connect) → AWS DX Device
```

- Partners have **existing physical ports** at AWS DX locations
- They sub-divide their capacity into **Hosted Connections** or **Hosted VIFs** for customers
- Customers never need physical presence at a DX facility

#### Major DX Partners

| Partner | Type | Notes |
|---------|------|-------|
| **Equinix Fabric** | Exchange | Global, multi-cloud exchange |
| **Megaport** | Exchange | SDN-provisioned, multi-cloud |
| **PacketFabric** | Exchange | Automated circuit provisioning |
| **AT&T, Verizon, BT** | Telco | Traditional MPLS/carrier circuits |
| **Tata, Lumen, NTT** | Telco | Global enterprise carriers |

#### Benefits of Partners

- Sub-1G connections (not available direct from AWS)
- Faster provisioning (days vs weeks for Dedicated)
- Last-mile managed service (partner provides DC-to-DX-location circuit)
- Some partners sell DX via **AWS Marketplace** — consolidated billing

#### Multicloud via Exchange Partners

Equinix Fabric, Megaport, PacketFabric act as **neutral interconnection exchanges**:

```mermaid
graph TD
    CustomerDC["🏢 Customer DC"] -->|"Last mile MPLS/fiber"| Exchange["🔄 Exchange PoP\n(Equinix/Megaport)"]
    Exchange -->|"Cross-connect"| AWSDX["☁️ AWS Direct Connect"]
    Exchange -->|"Cross-connect"| AzureER["☁️ Azure ExpressRoute"]
    Exchange -->|"Cross-connect"| GCPCI["☁️ GCP Cloud Interconnect"]
```

- Single physical cross-connect at exchange → reach multiple clouds
- Consistent private latency to all clouds
- Single billing point (the exchange provider)

> 🔑 **Exam key:** AWS has no native multicloud product. Multicloud connectivity = exchange partners (Equinix/Megaport). AWS Outposts/Local Zones = latency-sensitive hybrid but NOT multicloud.

---

### 🔗 Last Mile & Multicloud Connectivity

#### Last Mile — What It Is

The **last mile** is the physical circuit from your data center to the DX colocation facility. **AWS does not provide it** — you arrange it separately.

| Last Mile Option | Description | Typical Provider |
|----------------|-------------|-----------------|
| **MPLS circuit** | Managed Layer 3 VPN from telco | AT&T, Verizon, BT, Tata |
| **Dark fiber** | Unlit fiber — you run your own optics/routing | Zayo, Lumen, local providers |
| **Ethernet WAN** | Point-to-point Ethernet circuit | Various carriers |
| **SD-WAN over broadband** | Software-defined WAN using internet links | Various managed SD-WAN providers |
| **Partner managed** | DX Partner provides end-to-end circuit | Equinix, Megaport (managed) |

> ⚠️ **Latency note:** Last mile latency dominates total DX latency. Choose DX location **geographically close** to your DC. A 10ms last-mile circuit negates DX's latency benefits.

#### Last Mile Provisioning Timeline

| Step | Owner | Time |
|------|-------|------|
| DX Dedicated connection request | Customer | Day 0 |
| LOA-CFA issued | AWS | ≤72 hours |
| Last-mile circuit provisioning | Customer/telco | 2–8 weeks |
| Cross-connect installation at facility | Colocation | 1–5 business days |
| BGP configuration | Customer | Hours |
| **Total** | | **4–12 weeks typical** |

#### Multicloud Architecture Pattern

```mermaid
graph LR
    DC["🏢 Customer DC"] -->|"MPLS/dark fiber\n(last mile)"| PoP["🔄 Exchange PoP\n(Equinix/Megaport/PacketFabric)"]
    PoP -->|"Private VIF"| AWS["☁️ AWS\nDirect Connect"]
    PoP -->|"Private Peering"| Azure["☁️ Azure\nExpressRoute"]
    PoP -->|"Partner Interconnect"| GCP["☁️ GCP\nCloud Interconnect"]
    AWS --> AWSVPC["AWS VPCs"]
    Azure --> AzureVNet["Azure VNets"]
    GCP --> GCPProject["GCP Projects"]
```

#### AWS Hybrid Alternatives (not multicloud)

| Service | What it does | Use case |
|---------|-------------|----------|
| **AWS Outposts** | AWS rack in your DC | Ultra-low latency to AWS services |
| **AWS Local Zones** | AWS infrastructure near major cities | Low latency for specific geographies |
| **AWS Wavelength** | AWS at 5G carrier edge | Mobile/edge workloads |

> 🔑 **Exam key:** Outposts/Local Zones = AWS extending into your premises/edge. They do NOT connect to other clouds. Multicloud = exchange provider pattern only.

---

### 🗺️ Full DX Architecture: All Components End-to-End

This section maps every physical and logical component in a Direct Connect deployment and explains what each one is, where it sits, who owns it, and how it connects to the next component.

---

#### 🔭 Component Map

```mermaid
graph LR
    subgraph "Customer Premises"
        CR["🖥️ Customer Router\n(on-prem CE router)\nBGP peer, routes advertised"]
        CGW["📋 Customer Gateway\n(AWS logical resource)\nrepresents CR in AWS console"]
    end

    subgraph "DX Location / Colocation Facility"
        XC["🔌 Cross-Connect\n(physical fiber patch)\nbetween CR and DX router"]
        DXR["⚙️ Direct Connect Router\n(AWS-owned device at PoP)\nterminates physical port"]
    end

    subgraph "AWS Region"
        DXGW["🌍 DX Gateway\n(global, optional)\nmulti-VPC/multi-region"]
        VGW["🔒 Virtual Private Gateway\n(VGW)\nattached to one VPC"]
        VIF["🔀 Private VIF\n(802.1Q VLAN + BGP)\nlogical overlay on physical"]
        VPC["🔷 VPC\nresources accessible\nvia private IPs"]
    end

    CR -->|"Last Mile\n(MPLS/fiber/partner)"| XC
    XC --> DXR
    DXR -->|"AWS backbone"| VIF
    VIF -->|"via DXGW or direct"| VGW
    VGW --> VPC
    CGW -.->|"represents CR\nin AWS console"| CR
```

---

#### 📦 Component-by-Component Breakdown

##### 1. 🖥️ Customer Router (CR) — Customer-Owned

| Attribute | Detail |
|-----------|--------|
| **What it is** | Physical or virtual router at your data center or branch office |
| **Owner** | You (customer) |
| **Location** | Your premises (or your cage at a colocation facility) |
| **Role** | Terminates the last-mile circuit; establishes BGP session with AWS DX Router |
| **Protocol** | BGP (eBGP with AWS) — must use a private or public ASN |
| **Config required** | BGP neighbor config pointing to AWS VIF BGP peer IP; advertise your on-prem prefixes |
| **Examples** | Cisco ASR/ISR, Juniper MX/SRX, Palo Alto, pfSense, VyOS, Arista |
| **Key requirement** | Must support 802.1Q VLAN tagging (for VIF VLAN) and BGP |

> ⚠️ **Your router is entirely your responsibility** — AWS does not manage, monitor, or have access to it.

---

##### 2. 📋 Customer Gateway (CGW) — AWS Logical Resource

| Attribute | Detail |
|-----------|--------|
| **What it is** | An AWS resource that represents your on-prem router in the AWS console |
| **Owner** | You (created in AWS console/CLI) |
| **Location** | AWS (logical only — no physical component) |
| **Role** | Stores the public IP and BGP ASN of your Customer Router for AWS to reference |
| **Used with** | Site-to-Site VPN connections (NOT required for DX — DX uses VIF BGP config directly) |
| **Contains** | Public IP of your router, BGP ASN (optional for static VPN) |

> 🔑 **Exam key:** CGW is a **VPN concept**, not a DX concept. For Direct Connect, your router is configured directly in the VIF BGP settings — there is no "CGW" resource for DX. CGW is only used when setting up Site-to-Site VPN as DX backup.

---

##### 3. 🔌 Cross-Connect — Physical Layer

| Attribute | Detail |
|-----------|--------|
| **What it is** | Physical fiber patch cable at the DX colocation facility |
| **Owner** | The colocation facility arranges it; customer requests via LOA-CFA |
| **Location** | Inside the DX colocation facility (cage/suite) |
| **Role** | Physically connects YOUR router port to the AWS DX Router port |
| **How to get one** | AWS issues LOA-CFA (Letter of Authorization - Connecting Facility Assignment) → you give it to the colo facility → they install the cross-connect |
| **Timeline** | AWS issues LOA within 72h; colo facility installs in days–weeks |
| **If using a partner** | Partner already has cross-connect to AWS — you connect to partner, not AWS directly |

```
Customer Cage                    AWS Cage
┌─────────────┐  fiber patch  ┌──────────────────┐
│ Your Router │──────────────→│ AWS DX Router    │
│ Port Gi0/0  │               │ Port (dedicated) │
└─────────────┘               └──────────────────┘
         Both in same colocation building
```

---

##### 4. ⚙️ Direct Connect Router (AWS-Owned)

| Attribute | Detail |
|-----------|--------|
| **What it is** | Physical router owned and operated by AWS at the DX colocation PoP |
| **Owner** | AWS |
| **Location** | DX location (colocation facility — Equinix, CyrusOne, Interxion, etc.) |
| **Role** | Terminates the physical port; presents 802.1Q VLANs for VIFs; runs BGP with your router |
| **Visibility** | You do not manage or access this device — AWS abstraction |
| **What you see** | DX connection state (available/down), BGP session status, optical light levels via CloudWatch |
| **Redundancy** | For maximum resiliency: two connections at same location terminate on DIFFERENT AWS devices |

> 🔑 **Exam key:** "Development resiliency" = 2 connections, same location — they MAY be on different devices but AWS doesn't guarantee it. Only "Maximum resiliency" model explicitly requires 2 locations × 2 separate AWS devices.

---

##### 5. 🔀 Private VIF — Logical Overlay

| Attribute | Detail |
|-----------|--------|
| **What it is** | Logical 802.1Q VLAN + BGP session on top of the physical DX connection |
| **Owner** | You (created in DX console) |
| **Location** | Logical — spans from your router to VGW/DXGW |
| **VLAN ID** | 1–4094, must be unique per DX connection |
| **BGP peers** | Your router IP ↔ AWS VIF peer IP (both in /30 or /31 subnet) |
| **What it carries** | Private IP traffic to/from your VPC(s) |
| **Prefix limits** | Max 100 prefixes advertised TO AWS; max 200 prefixes received FROM AWS |
| **Connects to** | VGW (single VPC) OR DX Gateway (multi-VPC/multi-region) |
| **Per connection limit** | Dedicated: up to 50 VIFs. Hosted: exactly 1 VIF. |
| **MTU** | Default 1500 bytes; jumbo frames (9001 bytes) supported — must enable on VIF and configure router |

```
Physical Port (1G/10G/100G)
├── VLAN 100 → Private VIF → VGW → VPC-Production
├── VLAN 200 → Private VIF → DXGW → VPC-Development (another region)
└── VLAN 300 → Public VIF  → AWS public services
```

> 🔑 **Exam key:** Jumbo frames (MTU 9001) improve throughput for large data transfers. Must be enabled on BOTH the VIF (in DX console) AND your router interface. Mismatched MTU causes fragmentation or drops.

---

##### 6. 🔒 Virtual Private Gateway (VGW)

| Attribute | Detail |
|-----------|--------|
| **What it is** | AWS-managed gateway attached to a single VPC — terminates DX Private VIF or VPN |
| **Owner** | You (created and attached in VPC console) |
| **Location** | AWS Region — attached to one VPC |
| **Scope** | Regional, single-VPC |
| **ASN** | Configured at creation time (private or public); default varies; CANNOT change after creation |
| **Route propagation** | Can auto-propagate learned BGP routes into VPC route tables (must enable per route table) |
| **Connects to** | Private VIF (DX) AND/OR Site-to-Site VPN — same VGW can serve both simultaneously |
| **Limitation** | Only one VGW per VPC; only one VPC per VGW |

> ⚠️ **Route propagation must be explicitly enabled** on each VPC route table. Without it, DX/VPN learned routes are NOT automatically inserted — traffic won't flow even if BGP is up.

---

##### 7. 🌍 DX Gateway (DXGW) — Optional Global Hub

| Attribute | Detail |
|-----------|--------|
| **What it is** | Global resource that aggregates DX connections and distributes to multiple VGWs or TGWs |
| **Owner** | You |
| **Location** | Global (not tied to any region) |
| **Purpose** | Use ONE DX connection to reach VPCs in multiple regions |
| **Associates with** | Up to 10 VGWs OR up to 3 TGWs (not both on same DXGW) |
| **Routing** | On-prem → VPC only. Does NOT route VPC-to-VPC. |
| **Allowed prefixes** | Explicitly configured list of VPC CIDRs advertised to on-prem |

---

#### 🔄 Full Traffic Flow: Packet Walk

```
1. On-prem host (192.168.1.10) sends packet to VPC resource (10.0.1.50)

2. On-prem host → Customer Router
   Customer Router has BGP route: 10.0.0.0/16 via DX Private VIF

3. Customer Router → (last mile) → DX Location
   Packet tagged with VLAN ID of Private VIF (e.g., VLAN 100)

4. DX Location → Cross-Connect → AWS DX Router
   AWS DX Router sees VLAN 100, maps to Private VIF BGP session

5. AWS DX Router → Private VIF → VGW
   VGW receives packet destined for 10.0.1.50

6. VGW → VPC local routing → EC2 in subnet 10.0.1.0/24
   VPC route table: 10.0.0.0/16 local → delivered to EC2 ENI

Return path is symmetric — EC2 → VGW → DX Router → cross-connect → Customer Router → on-prem host
```

---

#### 🔁 Component Ownership Summary

| Component | Owner | Managed by | Physical? |
|-----------|-------|------------|-----------|
| Customer Router | You | You | Yes (or VM) |
| Customer Gateway | You | AWS console | No (logical) |
| Last Mile Circuit | You / Partner | Telco / Partner | Yes |
| Cross-Connect | Colo Facility | Colo Facility | Yes |
| DX Router (PoP) | AWS | AWS | Yes |
| Private VIF | You | AWS console | No (logical) |
| DX Gateway | You | AWS console | No (logical) |
| Virtual Private Gateway | You | AWS console | No (logical) |
| VPC Route Tables | You | AWS console | No (logical) |

---

#### ⚡ BGP Session Details

```
Your Router (BGP Peer)          AWS DX Router (BGP Peer)
─────────────────────────────────────────────────────────
IP: 169.254.x.x/30 (link-local) ↔  IP: 169.254.x.x/30 (AWS assigned)
ASN: your private ASN            ↔  ASN: VGW/DXGW ASN (e.g. 64512)
Advertise: your on-prem CIDRs        Advertise: VPC CIDR(s)
Auth: MD5 optional                   Auth: MD5 optional
BFD: recommended (fast failure)      BFD: supported
Hold timer: 90s default             Hold timer: 90s default
```

> 🔑 **Exam key:** AWS uses **link-local IPs (169.254.x.x)** for BGP peering on VIFs — these are NOT routable, only used for the BGP session itself. Your actual traffic uses your real private IPs.

---

#### 🔗 DX + VPN Side-by-Side (Same VGW)

```mermaid
graph LR
    subgraph "On-Premises"
        Router["Customer Router\n(BGP + IPSec capable)"]
    end

    subgraph "DX Location"
        DXR["DX Router\n(AWS PoP)"]
    end

    subgraph "AWS VPC"
        VGW["Virtual Private\nGateway (VGW)"]
        VPC["🔷 VPC\n10.0.0.0/16"]
    end

    Router -->|"Private VIF\nBGP, no encryption\nlow latency, consistent"| DXR
    DXR --> VGW
    Router -->|"Site-to-Site VPN\nIPSec encrypted\nover internet, backup"| VGW
    VGW --> VPC
```

> Same VGW can terminate both DX Private VIF AND Site-to-Site VPN simultaneously. BGP MED makes DX preferred. VPN activates automatically on DX failure.

---

## 🏗️ Chapter 2: Private and Public Subnets

### 📖 Overview

A **subnet** is a range of IP addresses within a VPC. Subnets are **zonal resources** — each subnet lives in exactly one Availability Zone. They subdivide a VPC's CIDR block for network segmentation and routing control.

> 🔑 **Exam key:** What makes a subnet "public" is NOT the IP addresses — it's having a **route to an Internet Gateway** in its route table. Remove that route → subnet becomes private.

**Scope:** Subnets are **AZ-scoped (Zonal)**.

---

### 🔢 CIDR and Subnet Sizing

| Concept | Detail |
|---------|--------|
| VPC CIDR | /16 to /28 |
| Subnet CIDR | Must be within VPC CIDR, /16 to /28 |
| Reserved IPs | AWS reserves **5 IPs** per subnet |
| IPv6 | /56 for VPC, /64 for subnets (mandatory size) |

**The 5 reserved IPs in each subnet (example: 10.0.1.0/24):**

| Address | Reserved For |
|---------|-------------|
| `10.0.1.0` | Network address |
| `10.0.1.1` | VPC router |
| `10.0.1.2` | DNS server (VPC+2) |
| `10.0.1.3` | Future AWS use |
| `10.0.1.255` | Broadcast (not used, but reserved) |

> 🔑 **Exam key:** /28 subnet = 16 IPs − 5 reserved = **11 usable**. Always subtract 5 for AWS reserves.

---

### 🌐 Public vs Private Subnet

```mermaid
graph TB
    Internet["🌍 Internet"] --> IGW["🚪 Internet Gateway\n(attached to VPC)"]
    IGW --> PublicRT["Public Route Table\n0.0.0.0/0 → IGW"]
    PublicRT --> PubSubnet["🟢 Public Subnet\n(has route to IGW)"]
    PubSubnet --> EC2Pub["EC2 with Public IP\nor Elastic IP"]

    PrivRT["Private Route Table\n0.0.0.0/0 → NAT GW"] --> PrivSubnet["🔴 Private Subnet\n(no route to IGW)"]
    PrivSubnet --> EC2Priv["EC2 — no public IP"]
    PubSubnet --> NATGW["🔄 NAT Gateway\n(in public subnet)"]
    NATGW --> PrivRT
```

| Feature | Public Subnet | Private Subnet |
|---------|--------------|----------------|
| Route to IGW | ✅ Yes | ❌ No |
| Instances can have public IPs | ✅ Yes | ❌ No |
| Direct inbound from internet | ✅ Possible | ❌ Not possible |
| Outbound to internet | ✅ Direct via IGW | Via NAT Gateway/Instance |
| Typical resources | Load balancers, bastion hosts, NAT GW | App servers, databases, Lambda in VPC |

---

### 🛣️ Route Tables

- Every subnet must be associated with **one route table** (default = main route table)
- Multiple subnets can share one route table
- Route table is evaluated **most-specific prefix first**

```
# Public subnet route table
10.0.0.0/16   →  local       (VPC local routing)
0.0.0.0/0     →  igw-xxx     (internet gateway)

# Private subnet route table
10.0.0.0/16   →  local
0.0.0.0/0     →  nat-xxx     (NAT gateway)
10.1.0.0/16   →  pcx-xxx     (VPC peering to another VPC)
172.16.0.0/12 →  vgw-xxx     (VPN/Direct Connect)
```

---

### 🔒 Network ACLs vs Security Groups

| Feature | Network ACL (NACL) | Security Group |
|---------|-------------------|----------------|
| **Scope** | Subnet level | Instance/ENI level |
| **State** | **Stateless** — must allow inbound AND outbound | **Stateful** — return traffic auto-allowed |
| **Rules** | Allow AND Deny | Allow only |
| **Rule evaluation** | Numbered order (lowest first), stops at first match | All rules evaluated |
| **Default** | Default NACL allows all | Default SG allows no inbound |
| **Use case** | Broad subnet-level blocks (deny IP ranges) | Fine-grained instance-level control |

> 🔑 **Exam key:** NACL is stateless — if you allow HTTP inbound (port 80), you must also allow **ephemeral ports outbound** (1024–65535). Security Groups don't have this problem.

---

### 🏢 Multi-Tier Architecture Pattern

```mermaid
graph TB
    Internet["🌍 Internet"] --> ALB["Application Load Balancer\n(Public Subnet)"]
    ALB --> App["App Servers\n(Private Subnet)"]
    App --> DB["RDS Database\n(Private Subnet - DB tier)"]
    App --> NATGW["NAT Gateway\n(Public Subnet)"]
    NATGW --> Internet
    DB --> |"No internet access"| X["🚫"]
```

**3-tier VPC design:**
| Tier | Subnet Type | Resources |
|------|------------|-----------|
| DMZ / Edge | Public | ALB, NLB, NAT GW, Bastion |
| Application | Private | EC2, ECS, Lambda, EKS nodes |
| Data | Private (isolated) | RDS, ElastiCache, DynamoDB VPC Endpoint |

---

### 🔵 IPv6 Subnets

- VPC: optional /56 IPv6 CIDR (from Amazon's pool or your own BYOIP)
- Subnet: fixed /64 (cannot change)
- IPv6 addresses are globally unique — **no NAT for IPv6**
- Egress-only for IPv6 = **Egress-Only Internet Gateway**
- Dual-stack: subnet can have both IPv4 and IPv6

---

### 🏛️ Well-Architected Framework — Subnets

| Pillar | Consideration |
|--------|--------------|
| **Operational Excellence** | Use consistent CIDR naming convention. Document subnet purpose via tags. Use VPC Flow Logs at subnet level for traffic visibility. |
| **Security** | Separate tiers into distinct subnets. Use NACLs as defense-in-depth. Never put databases in public subnets. Use dedicated isolated subnets for data tier. |
| **Reliability** | Deploy subnets across **minimum 2 AZs** (3 recommended). Size subnets generously — you cannot resize without recreating. |
| **Performance Efficiency** | Place resources in same AZ as dependent services to minimize cross-AZ data transfer costs and latency. |
| **Cost Optimization** | Cross-AZ data transfer costs $0.01/GB each direction. Co-locate tightly coupled services in same AZ. |
| **Sustainability** | Use IPv6 to eliminate NAT layer for IPv6-only workloads, reducing gateway resource consumption. |

---

### 📋 SAP-C02 Domain Coverage — Subnets

#### Domain 1: Design Solutions for Organizational Complexity
- Shared VPC (Resource Access Manager) — subnets shared to member accounts
- Centralized egress pattern: one private subnet per account, NAT GW in shared account

#### Domain 2: Design for New Solutions
- CIDR planning for multi-region, no overlapping addresses (VPC peering, TGW requirement)
- IPv6 dual-stack design for future-proofing

#### Domain 3: Continuous Improvement for Existing Solutions
- Adding AZs to existing deployment (add subnets per AZ)
- Migrating from single-AZ to multi-AZ subnet design

#### Domain 4: Accelerate Workload Migration and Modernization
- Subnet sizing for EKS node groups (large IP consumption per pod with VPC CNI)
- Subnet design for containers vs VMs (containers need more IPs)

---

## 🚪 Chapter 3: Internet Gateway and Alternatives

### 📖 Internet Gateway (IGW)

An Internet Gateway is a **horizontally scaled, redundant, highly available VPC component** that allows communication between instances in a VPC and the internet. It performs **NAT (Network Address Translation)** for instances with public IPv4 addresses.

- **Scope:** Regional (attached to VPC, not tied to specific AZ)
- **Cost:** Free (no hourly charge, no data transfer charge — pay for data transfer at VPC level)
- **Limits:** One IGW per VPC

> 🔑 **Exam key:** IGW itself is not the bottleneck — it scales automatically. You cannot control its bandwidth. The bottleneck is your EC2 instance's network capacity.

**How IGW works:**
1. EC2 in public subnet sends packet to internet
2. IGW translates private IP → Elastic IP (one-to-one NAT)
3. Response comes back, IGW translates EIP → private IP
4. For instances with auto-assigned public IPs: same process, but IP is not persistent

---

### 🔄 NAT Gateway

NAT Gateway allows **private subnet instances to initiate outbound internet connections** while blocking inbound initiated connections from the internet.

```mermaid
graph LR
    EC2["🖥️ EC2\nPrivate Subnet\n10.0.2.5"] -->|"1. Packet to 8.8.8.8"| NATGW["🔄 NAT Gateway\nPublic Subnet\nEIP: 1.2.3.4"]
    NATGW -->|"2. Translated:\nSrc=1.2.3.4"| IGW["🚪 IGW"]
    IGW -->|"3. To internet"| Internet["🌍 Internet"]
    Internet -->|"4. Response to 1.2.3.4"| IGW
    IGW --> NATGW
    NATGW -->|"5. Translated back\nto 10.0.2.5"| EC2
```

| Feature | NAT Gateway | NAT Instance |
|---------|------------|--------------|
| **Management** | Managed by AWS | Self-managed EC2 |
| **Availability** | Highly available within AZ | Single point of failure unless clustered |
| **Bandwidth** | Up to 100 Gbps (auto-scales) | Limited by instance type |
| **Security Groups** | Cannot attach | Can attach |
| **Port forwarding** | ❌ Not supported | ✅ Supported |
| **Bastion host** | ❌ Not possible | ✅ Can double as bastion |
| **Cost** | Per hour + per GB | EC2 cost only |
| **IPv6** | ❌ Not supported | ✅ Can configure |

> 🔑 **Exam key:** NAT Gateway is **AZ-scoped** for high availability. Deploy one NAT GW **per AZ** to avoid cross-AZ traffic charges and eliminate AZ as SPOF. Each private subnet's route table should point to NAT GW in the same AZ.

**NAT Gateway types:**
| Type | Use Case |
|------|---------|
| **Public** | Private subnet → internet (via EIP) |
| **Private** | Private subnet → other VPCs/on-prem (no internet, no EIP needed) |

---

### 🔵 Egress-Only Internet Gateway

Egress-Only IGW is for **IPv6 traffic only** — allows outbound IPv6 connections, blocks inbound initiated IPv6 traffic. It is the IPv6 equivalent of NAT Gateway.

| Feature | NAT Gateway | Egress-Only IGW |
|---------|------------|-----------------|
| **Protocol** | IPv4 | IPv6 |
| **Direction** | Outbound only (private subnets) | Outbound only |
| **Cost** | Per hour + per GB | Free |
| **Scope** | AZ-scoped | Regional (VPC-level) |
| **Inbound blocking** | ✅ Yes | ✅ Yes |

---

### 🔗 VPC Endpoints

VPC Endpoints allow private connectivity to AWS services **without traversing the internet**, no IGW, NAT GW, or public IP required.

#### Gateway Endpoints

| Property | Value |
|----------|-------|
| **Supported services** | Amazon S3 and DynamoDB only |
| **Cost** | Free |
| **Implementation** | Route table entry (prefix list → endpoint) |
| **Scope** | Regional |
| **DNS** | No DNS change required |

#### Interface Endpoints (powered by AWS PrivateLink)

| Property | Value |
|----------|-------|
| **Supported services** | 100+ AWS services + partner services |
| **Cost** | Per hour per AZ + per GB data processed |
| **Implementation** | ENI in your subnet with private IP |
| **Scope** | AZ-specific ENI, Regional service |
| **DNS** | Private DNS overrides service DNS to resolve to private IP |

```mermaid
graph LR
    EC2["🖥️ EC2\nPrivate Subnet"] -->|"HTTPS to s3.amazonaws.com"| GWE["📦 Gateway Endpoint\n(S3)"]
    GWE --> S3["☁️ Amazon S3"]

    EC2B["🖥️ EC2\nPrivate Subnet"] -->|"HTTPS to\nsqs.us-east-1.amazonaws.com"| ENI["🔌 Interface Endpoint\nENI in subnet"]
    ENI --> SQS["☁️ Amazon SQS"]
```

> 🔑 **Exam key:** Gateway endpoints add a **prefix list route** to your route table. Interface endpoints use **DNS resolution** — enable "Private DNS" on the endpoint so the standard service FQDN resolves to the private ENI IP.

---

### 🔒 AWS PrivateLink

AWS PrivateLink is the underlying technology for Interface Endpoints. It also enables you to **expose your own services** to other VPCs or AWS accounts without VPC peering or internet.

**Use case:** You have a service (SaaS) running in your VPC. Other customers can consume it via PrivateLink without seeing your VPC or needing peering.

```mermaid
graph LR
    ConsumerVPC["Consumer VPC"] --> InterfaceEP["Interface Endpoint\n(ENI)"]
    InterfaceEP -->|"PrivateLink"| NLB["Network Load Balancer\n(Provider VPC)"]
    NLB --> Service["Your Service\n(EC2/ECS/Lambda)"]
```

| Feature | PrivateLink | VPC Peering |
|---------|------------|-------------|
| **Traffic direction** | One-way (consumer → provider) | Two-way |
| **Overlapping CIDRs** | ✅ Works | ❌ Does not work |
| **Scalability** | Scales to many consumers | Complex at scale |
| **Routing** | No route table changes for consumer | Requires route table entries |
| **Use case** | SaaS, shared services | Full VPC connectivity |

---

### 🔀 Transit Gateway (TGW) as Internet Egress Alternative

For multi-VPC architectures, Transit Gateway enables **centralized internet egress** through a single NAT Gateway instead of one per VPC.

```mermaid
graph TB
    VPC1["VPC A\n(no NAT GW)"] --> TGW["🔀 Transit Gateway"]
    VPC2["VPC B\n(no NAT GW)"] --> TGW
    VPC3["VPC C\n(no NAT GW)"] --> TGW
    TGW --> EgressVPC["Egress VPC\n(has NAT GW + IGW)"]
    EgressVPC --> NAT["NAT Gateway"]
    NAT --> IGW["Internet Gateway"]
    IGW --> Internet["🌍 Internet"]
```

> 🔑 **Exam key:** Centralized egress via TGW reduces NAT GW costs for many VPCs. However, TGW has per-attachment and per-GB charges — do the math. For 3+ VPCs with significant egress, centralized is usually cheaper.

---

### 📊 Complete Egress/Ingress Options Comparison

| Service | Direction | IPv4/IPv6 | Cost | Use Case | Internet? |
|---------|-----------|-----------|------|----------|-----------|
| **Internet Gateway** | Both (inbound + outbound) | Both | Free | Public-facing resources | ✅ Yes |
| **NAT Gateway (Public)** | Outbound only | IPv4 | $0.045/hr + $0.045/GB | Private subnet internet access | ✅ Yes |
| **NAT Gateway (Private)** | Outbound only | IPv4 | $0.045/hr + $0.045/GB | VPC-to-VPC via TGW | ❌ No |
| **NAT Instance** | Outbound only | IPv4 | EC2 cost | Legacy, custom routing | ✅ Yes |
| **Egress-Only IGW** | Outbound only | IPv6 | Free | IPv6 private subnet egress | ✅ Yes |
| **Gateway Endpoint** | Outbound to S3/DynamoDB | IPv4 | Free | Private S3/DynamoDB access | ❌ No |
| **Interface Endpoint** | Outbound to AWS services | IPv4 | $0.01/hr/AZ + $0.01/GB | Private access to 100+ services | ❌ No |
| **PrivateLink** | Outbound to partner/own service | IPv4 | Same as Interface EP | SaaS, shared services | ❌ No |
| **Transit Gateway** | Both (between VPCs/on-prem) | Both | $0.05/hr/attach + $0.02/GB | Hub-spoke multi-VPC | Optional |

---

### 🏛️ Well-Architected Framework — Internet Gateway & Alternatives

| Pillar | Consideration |
|--------|--------------|
| **Operational Excellence** | Use VPC Flow Logs to monitor NAT GW traffic. Set CloudWatch alarms on NAT GW ErrorPortAllocation (port exhaustion). Tag all endpoints for cost allocation. |
| **Security** | Prefer VPC Endpoints over NAT GW for AWS service access — no internet exposure. Never put databases in public subnets. Use Security Groups on Interface Endpoints. |
| **Reliability** | Deploy NAT Gateway in each AZ used by private subnets. NAT GW is AZ-scoped — AZ failure removes NAT for that AZ. Use one per AZ. |
| **Performance Efficiency** | NAT GW auto-scales to 100 Gbps. Gateway Endpoints have no bandwidth limit. Interface Endpoints scale with NLB behind them. |
| **Cost Optimization** | Gateway Endpoints are free — always prefer over NAT GW for S3/DynamoDB. NAT GW charges per GB — optimize with S3 Transfer Acceleration or endpoints. Centralize egress with TGW for many VPCs. |
| **Sustainability** | VPC Endpoints eliminate internet routing for AWS service calls, reducing network hops and energy. Gateway Endpoints have no additional infrastructure footprint. |

---

### 📋 SAP-C02 Domain Coverage — IGW & Alternatives

#### Domain 1: Design Solutions for Organizational Complexity
- Centralized egress VPC pattern with TGW for large AWS Organizations
- Shared VPC endpoints via RAM to reduce Interface Endpoint costs

#### Domain 2: Design for New Solutions
- Choosing between IGW, NAT GW, PrivateLink based on direction and service type
- IPv6-only VPC design with Egress-Only IGW

#### Domain 3: Continuous Improvement for Existing Solutions
- Replacing NAT instances with NAT Gateways
- Adding VPC Endpoints to reduce NAT GW data processing costs
- Migrating from per-VPC NAT GW to centralized TGW egress

#### Domain 4: Accelerate Workload Migration and Modernization
- PrivateLink for exposing legacy services to new microservices without VPC peering sprawl
- Gateway Endpoints for S3-heavy migration workloads (DataSync, Storage Gateway)

---

## 🔀 Chapter 4: AWS Transit Gateway (TGW)

### 📖 Overview

AWS Transit Gateway (TGW) is a **regional, fully managed network transit hub** that connects VPCs and on-premises networks through a central gateway. Instead of creating a full mesh of VPC peering connections (which grows as N*(N-1)/2), Transit Gateway provides a hub-and-spoke model where every network attaches to the TGW and routing is controlled centrally.

> 🔑 **Exam key:** TGW solves the VPC peering mesh problem. 10 VPCs = 45 peering connections. With TGW = 10 attachments. At scale, TGW always wins on manageability.

**Scope:** TGW is a **Regional** resource. Cross-region connectivity is via TGW Peering (separate attachments).

**Default Limits:**
| Limit | Value |
|-------|-------|
| Attachments per TGW | 5,000 |
| VPCs per TGW | 5,000 |
| Bandwidth per VPN attachment | 10 Gbps |
| Aggregate bandwidth burst | 50 Gbps |
| Route tables per TGW | 20 |
| Routes per TGW route table | 10,000 |

---

### 🔗 Attachment Types

| Attachment Type | Connects To | Notes |
|----------------|-------------|-------|
| **VPC** | VPC (one subnet per AZ recommended) | Most common; VPC route tables need updating |
| **VPN** | Site-to-Site VPN connection | Supports ECMP when enabled |
| **Direct Connect Gateway** | DX Gateway → on-premises | Requires Transit VIF on DX side |
| **TGW Peering** | Another TGW (same or different region) | Static routes only, no BGP between TGWs |
| **Connect** | SD-WAN appliance via GRE tunnel | Supports BGP, higher bandwidth than VPN |

> 🔑 **Exam key:** Connect attachment is for SD-WAN solutions (Cisco, Fortinet). It uses GRE + BGP and supports up to 20 Gbps per Connect peer (vs 10 Gbps for VPN).

---

### 🛣️ Route Tables and Routing Domains

TGW has its **own route tables** — completely separate from VPC route tables.

**Key concepts:**

| Concept | Description |
|---------|-------------|
| **Association** | Links an attachment to ONE TGW route table. Attachment uses that RT to make routing decisions. |
| **Propagation** | Attachment advertises its CIDR routes INTO a TGW route table. |
| **Blackhole route** | Route that drops traffic (explicit drop, like a /dev/null route). |
| **Static route** | Manually added route in TGW RT. |

**How routing works:**
1. Packet arrives at TGW from Attachment A
2. TGW looks at the route table **associated** with Attachment A
3. Matches destination to a route → forwards to target attachment
4. Target attachment receives packet and delivers to its VPC/network

```mermaid
graph LR
    VPC_A["VPC A\n10.1.0.0/16"] -->|"attachment"| TGW["🔀 Transit Gateway"]
    VPC_B["VPC B\n10.2.0.0/16"] -->|"attachment"| TGW
    VPC_C["VPC C\n10.3.0.0/16"] -->|"attachment"| TGW
    VPN["On-Premises\n172.16.0.0/12"] -->|"VPN attachment"| TGW
    DX["Direct Connect\n(via DX Gateway)"] -->|"DXGW attachment"| TGW
```

---

### 🔀 Default vs Custom Route Tables — Network Segmentation

**Default (flat network) — both flags ON:**

```mermaid
graph TB
    subgraph "Default TGW Route Table"
        R1["10.1.0.0/16 → VPC-A-attach"]
        R2["10.2.0.0/16 → VPC-B-attach"]
        R3["10.3.0.0/16 → VPC-C-attach"]
        R4["172.16.0.0/12 → VPN-attach"]
    end
    VPC_A -->|"associated + propagates"| R1
    VPC_B -->|"associated + propagates"| R2
    VPC_C -->|"associated + propagates"| R3
    VPN_attach -->|"associated + propagates"| R4
```

All VPCs see each other = no segmentation.

**Custom route tables — network segmentation:**

```mermaid
graph TB
    subgraph "Prod Route Table"
        PR1["10.1.0.0/16 → Prod-VPC-attach"]
        PR2["172.16.0.0/12 → VPN-attach"]
        PR3["10.100.0.0/16 → SharedSvc-attach"]
    end
    subgraph "Dev Route Table"
        DR1["10.2.0.0/16 → Dev-VPC-attach"]
        DR2["10.100.0.0/16 → SharedSvc-attach"]
    end
    subgraph "Shared Services Route Table"
        SR1["10.1.0.0/16 → Prod-attach"]
        SR2["10.2.0.0/16 → Dev-attach"]
    end

    ProdVPC["Prod VPC\n10.1.0.0/16"] -->|"associated with Prod RT"| PR1
    DevVPC["Dev VPC\n10.2.0.0/16"] -->|"associated with Dev RT"| DR1
    SharedSvc["Shared Svc VPC\n10.100.0.0/16"] -->|"associated with Shared RT"| SR1
```

Prod and Dev cannot reach each other. Both can reach Shared Services. Shared Services can reach both.

> 🔑 **Exam key:** Network segmentation = turn OFF default association + propagation → create custom route tables per domain → manually control which attachments associate/propagate to which tables.

---

### ⚙️ Key Feature Flags — Detailed

These are TGW-level settings configured at creation or modifiable afterward (except Multicast):

#### 1. Default Route Table Association
| | |
|--|--|
| **Default** | **Enabled** |
| **Effect ON** | Every new attachment automatically associates with the TGW's default route table |
| **Effect OFF** | New attachments have no route table association — you must manually associate |
| **When to turn OFF** | When implementing network segmentation — you want full control over which RT each attachment uses |
| **Exam trap** | When OFF + you forget to manually associate an attachment, that attachment cannot route any traffic |

#### 2. Default Route Table Propagation
| | |
|--|--|
| **Default** | **Enabled** |
| **Effect ON** | Every new attachment automatically propagates its CIDRs into the default route table |
| **Effect OFF** | New attachments do not propagate routes anywhere — you must manually configure propagation |
| **When to turn OFF** | Network segmentation — you don't want prod VPC routes appearing in dev route tables |
| **Exam trap** | You can propagate the same attachment into MULTIPLE route tables simultaneously |

#### 3. Multicast Support
| | |
|--|--|
| **Default** | **Disabled** |
| **Can change after creation?** | ❌ **NO — must enable at TGW creation time** |
| **Protocol** | IGMPv2 (group membership), static multicast group membership also supported |
| **Use cases** | Media streaming, financial market data feeds, gaming, video conferencing infrastructure |
| **Cost** | Additional per-GB charge for multicast traffic |
| **Exam trap #1** | If asked how to enable multicast on existing TGW → must create NEW TGW |
| **Exam trap #2** | Multicast does NOT work across TGW peering (intra-TGW only) |

#### 4. DNS Support
| | |
|--|--|
| **Default** | **Enabled** |
| **Effect** | Enables resolution of public DNS hostnames of EC2 instances to private IPs when queried from VPCs attached to TGW |
| **Requirement** | VPC must also have "Enable DNS Hostnames" and "Enable DNS Support" enabled |
| **Example** | ec2-1-2-3-4.compute-1.amazonaws.com resolves to 10.x.x.x when queried from another attached VPC |
| **When to turn OFF** | Only if you don't need cross-VPC DNS resolution and want to reduce complexity |

#### 5. Security Groups Referencing Support
| | |
|--|--|
| **Default** | **Disabled** |
| **Effect** | Allows Security Group rules in VPC-A to reference Security Group IDs from VPC-B connected via TGW |
| **Scope** | Same region only (does not work across TGW peering or inter-region) |
| **Use case** | Micro-segmentation in multi-VPC architectures — app SG in VPC-A allows traffic only from specific SG in VPC-B |
| **Without this** | You must use CIDR ranges in SG rules for cross-VPC traffic (less precise) |
| **Exam note** | This is a newer feature; older exam questions may not include it |

#### 6. VPN ECMP Support
| | |
|--|--|
| **Default** | **Disabled** |
| **ECMP** | Equal-Cost Multi-Path routing |
| **Effect ON** | Traffic load-balanced across multiple VPN tunnels advertising the same prefixes |
| **Effect OFF** | BGP picks one active tunnel (active/standby) |
| **Requirement** | Must use **BGP** on VPN connections — static routing does NOT participate in ECMP |
| **Bandwidth** | Each VPN tunnel = up to 1.25 Gbps. With ECMP across 8 tunnels = ~10 Gbps aggregate |
| **Exam trap** | Static VPN + ECMP enabled = ECMP is ignored; static routes are distributed but not load-balanced |

---

### 🔢 Private ASN in BGP Context

**ASN (Autonomous System Number)** identifies routing domains in BGP.

| Range Type | Values |
|-----------|--------|
| 16-bit Public | 1 – 64511 |
| 16-bit Private | **64512 – 65534** |
| 32-bit Public | 65536 – 4199999999 |
| 32-bit Private | **4200000000 – 4294967294** |
| Reserved | 0, 65535, 4294967295 |

**In TGW context:**

| Setting | Default | Notes |
|---------|---------|-------|
| TGW Amazon Side ASN | **64512** | Configurable at creation; change requires TGW recreation |
| On-prem ASN | Your choice | Must be DIFFERENT from TGW ASN |
| VPN connection ASN | Per-connection | Can override per VPN attachment |

> 🔑 **Exam key:** BGP requires unique ASNs on each side of a peering session. If TGW uses 64512, your on-prem router must use a different ASN (e.g., 65000). Private ASNs are not routable on the internet — fine for private BGP sessions.

---

### 🏗️ Centralized Architecture Patterns

#### Pattern 1: Centralized Egress

```mermaid
graph TB
    VPC_A["Spoke VPC A\n(no NAT GW)"] --> TGW
    VPC_B["Spoke VPC B\n(no NAT GW)"] --> TGW
    VPC_C["Spoke VPC C\n(no NAT GW)"] --> TGW
    TGW["🔀 Transit Gateway"] --> EgressVPC["Egress VPC\n(has NAT GW + IGW)"]
    EgressVPC --> NAT["NAT Gateway\n(per AZ)"]
    NAT --> IGW["Internet Gateway"]
    IGW --> Internet["🌍 Internet"]
```

Route table setup: Spoke VPC TGW route tables have `0.0.0.0/0 → TGW`. TGW route table for spoke VPCs has `0.0.0.0/0 → Egress VPC attachment`.

#### Pattern 2: Centralized Ingress

Internet → ALB/NLB in Ingress VPC → TGW → Spoke VPC app servers. Reverse hairpinning via TGW.

#### Pattern 3: Centralized Security Inspection (East-West)

```mermaid
graph LR
    VPC_A["VPC A"] -->|"to VPC B"| TGW["TGW"]
    TGW -->|"0.0.0.0/0 route"| InspVPC["Inspection VPC\n(AWS Network Firewall\nor GWLB + appliance)"]
    InspVPC -->|"inspected traffic"| TGW
    TGW --> VPC_B["VPC B"]
```

#### Pattern 4: Shared Services VPC

DNS, Active Directory, monitoring, patch management in Shared Services VPC. All other VPCs access via TGW. Shared Services RT propagates to all; spoke RTs only propagate to Shared Services.

---

### 🆚 TGW vs VPC Peering vs PrivateLink

| Feature | Transit Gateway | VPC Peering | PrivateLink |
|---------|----------------|-------------|-------------|
| **Transitive routing** | ✅ Yes | ❌ No | N/A |
| **Overlapping CIDRs** | ❌ No (same as peering) | ❌ No | ✅ Yes |
| **Cross-account** | ✅ Yes (via RAM) | ✅ Yes | ✅ Yes |
| **Cross-region** | ✅ TGW Peering | ✅ Yes | ✅ (Inter-region PrivateLink) |
| **Bandwidth** | 50 Gbps burst | No limit (uses VPC bandwidth) | NLB-limited |
| **Cost** | $0.05/hr/attach + $0.02/GB | $0.01/GB (cross-AZ) | $0.01/hr/AZ + $0.01/GB |
| **Routing control** | Central RT | Per-VPC RT | N/A (service-level) |
| **Use case** | Many VPCs, on-prem, complex routing | Simple 1:1 VPC connectivity | Service exposure |
| **Complexity** | Medium | Low | Low-medium |
| **Max connections** | 5,000 attachments | 125 per VPC | Many endpoints |

> 🔑 **Exam key decision tree:**
> - Simple 2-3 VPC peering, no overlap issues → **VPC Peering** (cheaper)
> - 4+ VPCs, need transitive routing, on-prem → **TGW**
> - Expose one service across accounts/CIDRs → **PrivateLink**

---

### 🌍 Inter-Region TGW Peering

```mermaid
graph LR
    TGW_US["TGW us-east-1"] -->|"Peering attachment\n(encrypted, AWS backbone)"| TGW_EU["TGW eu-west-1"]
    TGW_US --> VPC_US["VPC us-east-1"]
    TGW_EU --> VPC_EU["VPC eu-west-1"]
```

| Feature | Value |
|---------|-------|
| **Routing** | **Static routes only** — no BGP between TGWs |
| **Encryption** | Automatic — traffic encrypted on AWS backbone |
| **Bandwidth** | Not guaranteed, subject to underlying inter-region capacity |
| **Cost** | TGW peering attachment + inter-region data transfer charges |
| **DNS** | DNS support flag does NOT work across TGW peering |
| **SG referencing** | Does NOT work across TGW peering |
| **Multicast** | Does NOT work across TGW peering |

> 🔑 **Exam key:** Inter-region TGW peering = static routes only. If you need dynamic routing across regions, use separate BGP sessions to on-premises as the routing exchange point.

---

### 💰 Pricing

| Component | Cost |
|-----------|------|
| Attachment hour | $0.05/hr per attachment (VPC, VPN, DX GW, peering) |
| Data processed | $0.02/GB in + $0.02/GB out through TGW |
| Peering attachment | $0.05/hr (each side pays) |
| **Free tier** | None |

> 💡 **Cost tip:** 1 VPC attachment running 24/7 = $0.05 × 24 × 30 = $36/month just for the attachment, before any data transfer. For a small number of VPCs with low traffic, VPC Peering (pay only for cross-AZ data) may be cheaper.

---

### ☁️ Azure Equivalent

| AWS | Azure | Notes |
|-----|-------|-------|
| Transit Gateway | **Azure Virtual WAN (vWAN)** | Both are managed hub-and-spoke transit services |
| VPC Peering | Azure VNet Peering | Both non-transitive, require non-overlapping CIDRs |
| TGW Connect | Azure SD-WAN integration | Both support third-party SD-WAN appliances |

**Key differences:**
- Azure vWAN is a global resource; AWS TGW is regional (use TGW peering for multi-region)
- Azure vWAN Standard Hub supports routing, firewall, VPN in one hub; AWS uses separate services (TGW + Network Firewall + VPN)
- Azure vWAN has built-in BGP peering capability; AWS TGW static routes for inter-TGW peering

---

### 🐧 Open Source / Linux Equivalent

| Concept | AWS TGW | Linux Equivalent |
|---------|---------|-----------------|
| Virtual router | Transit Gateway | Linux network namespaces + ip route |
| Route tables | TGW route tables | VRF (Virtual Routing and Forwarding) tables |
| BGP routing | VPN/DX BGP sessions | FRRouting (FRR) or BIRD daemon |
| VRF isolation | Route domains | `ip vrf exec <vrf>` per routing domain |
| Multicast | TGW multicast groups | `smcroute` / `pimd` (IGMPv2) |

Conceptually similar to **MPLS L3VPN**: route distinguishers (RD) per VRF = TGW route table per segment. Route targets (RT) control import/export = TGW propagation rules.

---

### 🏛️ Well-Architected Framework — Transit Gateway

| Pillar | TGW Consideration |
|--------|------------------|
| **Operational Excellence** | Use TGW Network Manager for centralized network visibility. Enable CloudWatch metrics (BytesIn/Out, PacketDropCountNoRoute). Use consistent tag naming for all TGW resources. Automate route table management via CloudFormation/CDK. |
| **Security** | Use custom route tables to isolate prod/dev/shared networks. Enable VPC Flow Logs on all TGW-attached VPCs. Use centralized Inspection VPC with AWS Network Firewall for east-west inspection. Use RAM to share TGW securely across accounts within AWS Organizations only. |
| **Reliability** | Deploy TGW in multiple AZs (attach VPC subnets across all AZs). Use redundant VPN attachments with ECMP for on-premises connectivity. Use DX + VPN for maximum reliability to on-premises. TGW is a regional service — for cross-region HA, use TGW peering with routing redundancy. |
| **Performance Efficiency** | Use Connect attachment (GRE) for SD-WAN — higher bandwidth than VPN. Enable VPN ECMP to aggregate VPN tunnel bandwidth. Place TGW subnet attachments in same AZ as heavy-traffic workloads to minimize cross-AZ hops. Use TGW for centralized egress to reduce number of NAT Gateways. |
| **Cost Optimization** | Evaluate TGW vs VPC Peering break-even: TGW is cost-effective for 4+ VPCs. Centralize NAT Gateways via TGW to reduce per-VPC NAT costs. Monitor TGW data processing costs in Cost Explorer. Use VPC Endpoints in spoke VPCs to reduce TGW data processing for AWS service traffic. |
| **Sustainability** | Centralized egress reduces infrastructure footprint (fewer NAT GWs). Shared Inspection VPC via TGW is more sustainable than per-VPC firewalls. TGW Connect attachment for SD-WAN can consolidate multiple physical appliances. |

---

### 📋 SAP-C02 Domain Coverage — Transit Gateway

#### Domain 1: Design Solutions for Organizational Complexity
- TGW shared via AWS RAM across AWS Organizations accounts
- Centralized Network account pattern (TGW + DX + firewalls in shared account)
- TGW route table isolation for multi-tenant network segmentation (prod/dev/shared)
- TGW Network Manager for multi-region, multi-account network topology visualization

#### Domain 2: Design for New Solutions
- Hub-and-spoke replacing VPC peering mesh (N*(N-1)/2 → N attachments)
- Centralized egress, ingress, and inspection architectural patterns
- TGW + DX Gateway for hybrid connectivity at enterprise scale
- Multicast architecture for media/financial services (plan at creation time)

#### Domain 3: Continuous Improvement for Existing Solutions
- Migrating from VPC peering mesh to TGW (non-disruptive: add TGW, test, remove peering)
- Adding centralized security inspection to existing TGW topology
- Upgrading VPN attachments with ECMP for additional bandwidth
- Transitioning from per-VPC NAT GW to centralized egress via TGW

#### Domain 4: Accelerate Workload Migration and Modernization
- TGW as connectivity backbone for lift-and-shift migrations (on-prem → VPCs)
- TGW + DX for high-throughput migration paths
- Connecting legacy on-premises networks to cloud-native services via TGW
- SD-WAN integration via Connect attachment for branch office modernization

---

## 📊 Chapter 5: VPC Flow Logs

### 📖 Overview

**VPC Flow Logs** captures **IP traffic metadata** flowing through Elastic Network Interfaces (ENIs) in your VPC. It records connection-level information — source, destination, ports, bytes, action — but **does NOT capture packet payloads**.

> 🔑 **Exam key:** Flow Logs = metadata, not packets. For full packet capture, use **VPC Traffic Mirroring**.

**Scope:** Regional service. Flow log data is stored in the destination you choose (CloudWatch Logs, S3, or Kinesis Firehose) in the same region.

**Can be enabled at three levels:**
| Level | Captures |
|---|---|
| **VPC** | All ENIs in the entire VPC |
| **Subnet** | All ENIs in that subnet |
| **ENI** | Single network interface |

> 💡 Multiple flow logs can be created per resource — e.g., one to S3 for long-term retention and one to CloudWatch for real-time alerting on the same ENI.

**Performance:** Captured **out-of-band** — zero impact on network throughput or latency.

---

### 📋 What Flow Logs Capture

| Field | Description | Example |
|---|---|---|
| `srcaddr` | Source IP address (post-NAT at ENI) | `10.0.1.45` |
| `dstaddr` | Destination IP address | `52.94.1.100` |
| `srcport` | Source port | `54321` |
| `dstport` | Destination port | `443` |
| `protocol` | IANA protocol number | `6` (TCP), `17` (UDP), `1` (ICMP) |
| `packets` | Packets transferred in aggregation window | `15` |
| `bytes` | Bytes transferred | `9842` |
| `start` | Unix timestamp of first packet | `1690000000` |
| `end` | Unix timestamp of last packet | `1690000600` |
| `action` | ACCEPT or REJECT | `ACCEPT` |
| `log-status` | OK / NODATA / SKIPDATA | `OK` |

**Log status meanings:**
- `OK` — records captured normally
- `NODATA` — no traffic in the aggregation interval (normal, informational)
- `SKIPDATA` — internal capacity issue; some records were dropped — investigate if persistent

---

### 📐 Log Record Format Versions

**Default v2 fields (original format):**
```
version account-id interface-id srcaddr dstaddr srcport dstport protocol packets bytes start end action log-status
```

| Version | Fields Added | Key Additions |
|---|---|---|
| **v2** | Base 14 fields | Core connection metadata |
| **v3** | `vpc-id`, `subnet-id`, `instance-id`, `tcp-flags`, `type`, `pkt-srcaddr`, `pkt-dstaddr` | Instance context, NAT tracing, TCP flag analysis |
| **v4** | `region`, `az-id`, `sublocation-type`, `sublocation-id` | AZ-level visibility, Outposts/Wavelength support |
| **v5** | `pkt-src-aws-service`, `pkt-dst-aws-service`, `flow-direction`, `traffic-path` | AWS service identification, ingress/egress, path tracing |

**Key v3+ fields explained:**
- `pkt-srcaddr` vs `srcaddr`: `srcaddr` = IP at ENI (may be NAT-translated); `pkt-srcaddr` = original source IP before NAT — use `pkt-srcaddr` to trace true origin through NAT Gateway
- `tcp-flags`: bitmask — `2`=SYN, `18`=SYN-ACK, `1`=FIN, `4`=RST — useful for detecting scan patterns (many SYNs, no SYN-ACK)
- `instance-id`: EC2 instance ID associated with the ENI — simplifies correlation without knowing ENI→instance mapping

**Custom format:** When creating a flow log, choose any subset of fields in any order — only those fields appear in records. Recommended: always include at minimum v5 fields for maximum exam and operational value.

---

### 🎯 Destinations

| Destination | Latency | Query Method | Best For |
|---|---|---|---|
| **CloudWatch Logs** | ~2–5 min | CloudWatch Insights, Metric Filters | Real-time alerting, dashboards |
| **S3** | ~5–15 min | Athena, S3 Select, Macie | Long-term retention, cost-effective at scale |
| **Kinesis Data Firehose** | Near real-time | OpenSearch, Splunk, S3 downstream | SIEM integration, streaming analytics |

**Choosing a destination:**
- **CloudWatch Logs** — set metric filters to count REJECT records and alarm on security incidents
- **S3 + Athena** — query terabytes cheaply; use Parquet format for best Athena performance
- **Kinesis Firehose** — integrate with existing SIEM (Splunk, OpenSearch Service); Firehose buffers and batches before writing downstream

> ⚠️ **TGW Flow Logs limitation:** TGW flow logs support only **S3 and CloudWatch Logs** — Kinesis Firehose is NOT supported for TGW flow logs.

---

### 📁 Log File Format (S3 Destination)

**S3 path structure:**
```
s3://bucket/AWSLogs/{account-id}/vpcflowlogs/{region}/{year}/{month}/{day}/
```

**File name:**
```
{account-id}_vpcflowlogs_{region}_{flow-log-id}_{date}_{time}_{hash}.log.gz
```

**Format options:**
| Option | Description | Use Case |
|---|---|---|
| **Text** (default) | Space-delimited, gzip compressed | Simple parsing, grep |
| **Parquet** | Columnar binary format | Athena queries (~20% smaller, faster scans) |

**Aggregation interval:**
| Setting | Interval | Cost | Use Case |
|---|---|---|---|
| Default | 10 minutes | Lower | General logging |
| 1-minute | 1 minute | Higher | Incident response, real-time troubleshooting |

**Hive-compatible prefix:** Optional setting that adds partition-style prefix:
```
s3://bucket/AWSLogs/year=2024/month=01/day=15/
```
Enables automatic partition discovery in Athena — reduces scan costs significantly for large datasets.

---

### 🌐 VPC Context

**Capture levels and their scope:**
```
VPC Flow Log (VPC level)
└── Subnet Flow Log (Subnet level)
    └── ENI Flow Log (ENI level)
```

**Traffic filter options** (set per flow log):
- `ALL` — both accepted and rejected traffic
- `ACCEPT` — only traffic allowed by SG/NACL
- `REJECT` — only traffic blocked by SG/NACL

> 🔑 **Exam key:** Flow logs are NOT retroactive — they only capture traffic after the flow log is created.

**Traffic NOT captured by VPC Flow Logs:**
| Traffic Type | Why Excluded | Alternative |
|---|---|---|
| DNS queries to VPC+2 resolver | Internal AWS DNS | Route 53 Resolver Query Logs |
| DHCP requests/responses | AWS DHCP service | CloudTrail (DHCP option sets) |
| Instance metadata (169.254.169.254) | Link-local, internal | CloudTrail |
| Amazon Time Sync (169.254.169.123) | Link-local NTP | N/A |
| Windows license activation | AWS internal KMS | N/A |
| VPC router traffic | Internal | N/A |

---

### 🔗 VPC Peering Context

Flow logs capture traffic at the **ENI level** — each VPC sees only its own side:

```mermaid
graph LR
    subgraph "VPC-A (10.0.0.0/16)"
        EC2A["EC2-A\n10.0.1.10"] -->|"srcaddr=10.0.1.10\ndstaddr=10.1.1.20"| FLOWA["📊 VPC-A Flow Log\nCaptures: outbound from EC2-A"]
    end
    FLOWA -->|"VPC Peering"| FLOWB
    subgraph "VPC-B (10.1.0.0/16)"
        FLOWB["📊 VPC-B Flow Log\nCaptures: inbound to EC2-B"] --> EC2B["EC2-B\n10.1.1.20"]
    end
```

**Key points:**
- VPC-A flow log: sees `srcaddr=10.0.1.10, dstaddr=10.1.1.20, action=ACCEPT`
- VPC-B flow log: sees `srcaddr=10.0.1.10, dstaddr=10.1.1.20, action=ACCEPT`
- Neither side logs "through the peering" — each logs its own ENI traffic
- **For full visibility: enable flow logs on BOTH VPCs**
- `pkt-srcaddr` = original source IP (useful when traffic passes through NAT before peering)

> 🔑 **Exam key:** To prove VPC-A is communicating with VPC-B, you need flow logs on BOTH sides — one side alone gives incomplete evidence.

---

### 🔀 Transit Gateway Flow Logs

TGW has **its own separate flow log capability** — independent from VPC flow logs.

```mermaid
graph LR
    subgraph "VPC-A"
        ENI_A["ENI\n📊 VPC Flow Log A\n(sees: src=A, dst=B)"]
    end
    ENI_A --> TGW
    subgraph "Transit Gateway"
        TGW["🔀 TGW\n📊 TGW Flow Log\n(sees: full east-west\nincl. src/dst VPC IDs\nattachment IDs)"]
    end
    TGW --> ENI_B
    subgraph "VPC-B"
        ENI_B["ENI\n📊 VPC Flow Log B\n(sees: src=A, dst=B)"]
    end
```

**TGW-specific flow log fields:**

| Field | Description |
|---|---|
| `tgw-id` | Transit Gateway ID |
| `tgw-attachment-id` | Source attachment ID |
| `tgw-src-vpc-account-id` | AWS account owning source VPC |
| `tgw-dst-vpc-account-id` | AWS account owning destination VPC |
| `tgw-src-vpc-id` | Source VPC ID |
| `tgw-dst-vpc-id` | Destination VPC ID |
| `tgw-src-subnet-id` | Source subnet ID |
| `tgw-dst-subnet-id` | Destination subnet ID |
| `tgw-pair-attachment-id` | Return-path attachment ID |

**Destinations for TGW flow logs:** S3 or CloudWatch Logs only (**no Kinesis Firehose**).

**Use case:** Audit which VPC is communicating with which VPC via TGW — essential for security and compliance in multi-account environments.

---

### 🛣️ Traffic Path Field (v5)

The `traffic-path` field (v5+) identifies how traffic exited the VPC:

| Value | Path |
|---|---|
| `1` | Through Internet Gateway (IGW) |
| `2` | Through Virtual Private Gateway (VGW) — DX or VPN |
| `3` | Through VPC Interface Endpoint (PrivateLink) |
| `4` | Through NAT Gateway |
| `5` | Through Transit Gateway |
| `6` | Through VPC Peering |
| `7` | Through Egress-Only Internet Gateway (IPv6) |
| `8` | Through Gateway VPC Endpoint (S3/DynamoDB) |

> 🔑 **Exam key:** `traffic-path` only populated for **egress** traffic. Not set for traffic staying within the VPC.

---

### ⚠️ Limitations

| Limitation | Detail |
|---|---|
| No payload capture | Metadata only — use Traffic Mirroring for packets |
| Not real-time | 2–15 min delay depending on destination |
| Not retroactive | Only captures traffic after flow log creation |
| SKIPDATA records | Internal capacity issue — records dropped (investigate if persistent) |
| Regional destination | CloudWatch Log Group must be in same region as VPC |
| IAM role required | Flow logs service needs `logs:CreateLogGroup`, `logs:PutLogEvents`, or S3 `PutObject` permissions |
| S3 bucket policy | Must allow `delivery.logs.amazonaws.com` as principal |
| TGW: no Firehose | TGW flow logs → S3 or CloudWatch only |
| Cannot capture DNS | VPC+2 resolver queries not logged — use R53 Resolver Query Logs |

---

### 🔒 Security and Operations Use Cases

| Use Case | How | Flow Log Field |
|---|---|---|
| **Detect port scanning** | Many distinct dstports from same srcaddr in short window | `srcaddr`, `dstport`, `action=REJECT` |
| **Rejected connection audit** | Filter `action=REJECT` — identifies SG/NACL blocks | `action` |
| **Identify unused SG rules** | No `ACCEPT` records for a specific port over 30 days | `dstport`, `action` |
| **Compliance evidence** | Log all connections to sensitive resources | `srcaddr`, `dstaddr`, `action` |
| **Troubleshoot connectivity** | REJECT = network block; ACCEPT + no data = app issue | `action`, `bytes` |
| **Top bandwidth consumers** | Sort by `bytes` DESC — find traffic hogs | `bytes`, `srcaddr` |
| **Lateral movement detection** | Unusual east-west ACCEPT flows between subnets | `srcaddr`, `dstaddr`, `vpc-id` |

**CloudWatch Metric Filter example — alert on REJECT spikes:**
```
# Metric filter pattern
[version, account, eni, src, dst, srcport, dstport, proto, packets, bytes, start, end, action="REJECT", logstatus]

# Create metric: VPCFlowLogRejects
# Alarm: VPCFlowLogRejects > 100 per 5 minutes → SNS notification
```

---

### 🔍 Athena Query Examples (S3 Destination)

First, create the Athena table:
```sql
CREATE EXTERNAL TABLE vpc_flow_logs (
  version int, account string, interfaceid string,
  sourceaddress string, destinationaddress string,
  sourceport int, destinationport int, protocol int,
  numpackets int, numbytes bigint, starttime int, endtime int,
  action string, logstatus string
)
PARTITIONED BY (year string, month string, day string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
LOCATION 's3://my-bucket/AWSLogs/123456789/vpcflowlogs/us-east-1/'
TBLPROPERTIES ('skip.header.line.count'='1');
```

**1. Top source IPs by bytes transferred:**
```sql
SELECT sourceaddress, SUM(numbytes) AS total_bytes
FROM vpc_flow_logs
WHERE year='2024' AND month='01' AND day='15'
GROUP BY sourceaddress
ORDER BY total_bytes DESC
LIMIT 20;
```

**2. All REJECT traffic in last 24h:**
```sql
SELECT sourceaddress, destinationaddress, destinationport, protocol, COUNT(*) AS reject_count
FROM vpc_flow_logs
WHERE action = 'REJECT'
  AND year='2024' AND month='01' AND day='15'
GROUP BY sourceaddress, destinationaddress, destinationport, protocol
ORDER BY reject_count DESC;
```

**3. Traffic between specific CIDR ranges:**
```sql
SELECT *
FROM vpc_flow_logs
WHERE (sourceaddress LIKE '10.0.1.%' AND destinationaddress LIKE '10.1.0.%')
   OR (sourceaddress LIKE '10.1.0.%' AND destinationaddress LIKE '10.0.1.%')
  AND year='2024' AND month='01' AND day='15';
```

**4. Find all traffic on a specific port:**
```sql
SELECT sourceaddress, destinationaddress, action, COUNT(*) AS connection_count
FROM vpc_flow_logs
WHERE destinationport = 22
  AND year='2024' AND month='01' AND day='15'
GROUP BY sourceaddress, destinationaddress, action
ORDER BY connection_count DESC;
```

---

### 🆚 Flow Logs vs Other Network Monitoring Tools

| Tool | Captures | Latency | Payload | Cost | Use Case |
|---|---|---|---|---|---|
| **VPC Flow Logs** | Connection metadata | 2–15 min | ❌ No | Low | Audit, troubleshoot, compliance |
| **VPC Traffic Mirroring** | Full packets (pcap) | Real-time | ✅ Yes | Higher (target instance) | Deep packet inspection, forensics |
| **AWS Network Firewall Logs** | Rule matches, domain hits | Near real-time | Alert only | Medium | Firewall audit trail |
| **CloudWatch Agent** | OS metrics + app logs | Real-time | N/A | Medium | Host-level visibility |
| **Route 53 Resolver Query Logs** | DNS queries | Minutes | N/A | Low | DNS auditing, data exfil detection |
| **AWS WAF Logs** | HTTP requests | Near real-time | Headers only | Low | Web request auditing |

> 🔑 **Exam key:** Customer asks "we need to capture full packets for forensics" → **Traffic Mirroring**, NOT Flow Logs. "We need to log all DNS queries" → **Route 53 Resolver Query Logs**, NOT Flow Logs.

---

### 🏛️ Well-Architected Framework — VPC Flow Logs

| Pillar | Consideration |
|---|---|
| **Operational Excellence** | Enable flow logs at VPC level on all VPCs by default via AWS Config rule or Service Control Policy. Use Athena + QuickSight for traffic dashboards. Automate flow log creation via CloudFormation/Terraform. |
| **Security** | Enable REJECT-only flow logs to CloudWatch for real-time security alerting. Use metric filters + alarms for port scan detection. Send to Security Lake (OCSF format) for centralized SIEM. Use GuardDuty (which uses flow logs internally) for automated threat detection. |
| **Reliability** | Flow logs themselves are highly available — AWS manages the capture infrastructure. Monitor for SKIPDATA records which indicate log delivery issues. Use multiple destinations (S3 + CloudWatch) for redundancy. |
| **Performance Efficiency** | Use Parquet format + Hive partitioning for Athena queries — dramatically reduces scan cost and time. Use 1-minute aggregation only when needed (higher cost). Filter to REJECT-only when ACCEPT logs aren't needed. |
| **Cost Optimization** | Flow logs to S3 are cheapest for long-term storage. Use S3 lifecycle policies to move old logs to Glacier. REJECT-only filter cuts volume ~90% in most environments. Parquet is ~20% smaller than text. |
| **Sustainability** | Minimize log volume with targeted filters (REJECT-only, specific subnet) rather than blanket VPC-level ALL-traffic logging. Use Parquet to reduce storage and compute for queries. |

---

### 📋 SAP-C02 Domain Coverage — VPC Flow Logs

#### Domain 1: Design Solutions for Organizational Complexity
- Centralized flow log aggregation across AWS Organizations using S3 with cross-account bucket policies
- AWS Security Lake for OCSF-normalized flow log data across all accounts
- RAM sharing of flow log destinations (Kinesis Firehose) across accounts

#### Domain 2: Design for New Solutions
- Architect real-time security alerting using Flow Logs → CloudWatch → Metric Filter → Alarm → SNS → Lambda
- Choose between CloudWatch vs S3 vs Firehose based on latency, query, and retention requirements
- Design TGW + VPC flow log strategy for full east-west traffic visibility in multi-VPC architectures

#### Domain 3: Continuous Improvement for Existing Solutions
- Add Parquet format and Hive partitioning to existing text-format flow logs for Athena cost reduction
- Upgrade from v2 to v5 format to gain `traffic-path` and `flow-direction` fields
- Implement 1-minute aggregation during incident investigations, revert to 10-minute after resolution

#### Domain 4: Accelerate Workload Migration and Modernization
- Use flow logs during migration to validate traffic patterns (which on-prem IPs are talking to which AWS IPs)
- Identify unused connections and firewall rules by analyzing REJECT vs ACCEPT patterns
- Enable TGW flow logs post-migration to audit cross-VPC communication in new landing zone architecture

---

### ☁️ Azure / Linux Equivalent

| Concept | AWS | Azure | Linux |
|---|---|---|---|
| Flow log capture | VPC Flow Logs | NSG Flow Logs | iptables LOG target / nftables |
| Destination | S3 / CloudWatch / Firehose | Storage Account | syslog / rsyslog / journald |
| Packet capture | Traffic Mirroring | Network Watcher packet capture | tcpdump / Wireshark |
| Query | Athena | Traffic Analytics (via Log Analytics) | grep / awk / ELK stack |
| Netflow export | N/A (managed) | N/A (managed) | ntopng / pmacct / softflowd |
| DNS query logs | R53 Resolver Query Logs | Azure DNS diagnostic logs | dnsmasq logs / bind9 querylog |

> **Azure NSG Flow Logs** store in Storage Account in JSON format with v1/v2 versions (v2 adds byte/packet counts). Azure Traffic Analytics (built on Log Analytics) provides pre-built dashboards similar to CloudWatch Insights with flow log data.

---

## 🔐 Chapter 6: Remote Network to VPC Connectivity

### 📖 Overview

This chapter covers **network-to-network (site-to-site)** connectivity options — connecting entire remote networks (on-premises data centers, branch offices, partner networks) to AWS VPCs. This is distinct from **AWS Client VPN**, which connects individual users to a VPC.

**Main options covered:**
- AWS Managed Site-to-Site VPN (via VGW or TGW)
- AWS VPN CloudHub (multi-site hub-and-spoke)
- Transit Gateway + Site-to-Site VPN (with Accelerated VPN)
- Software VPN on EC2 (OpenVPN, StrongSwan, pfSense)
- Third-party appliances from AWS Marketplace

**Scope:** All options are **regional** unless noted (Accelerated VPN uses Global Accelerator global PoPs).

> 🔑 **Exam key:** Site-to-Site VPN = network-to-network, IPSec over internet. Client VPN = user device-to-VPC, TLS. Direct Connect = private fiber, no internet. These are complementary options.

---

### 🔌 AWS Site-to-Site VPN (Managed)

#### What It Is

AWS-managed IPSec VPN over the public internet between a **Customer Gateway (CGW)** on your side and a **Virtual Private Gateway (VGW)** or **Transit Gateway** on the AWS side.

**Key characteristics:**
- Fully managed by AWS — no VPN endpoint instances to patch
- Two tunnels per VPN connection (each tunnel in a different AZ on the AWS side)
- Active/active or active/standby tunnel modes
- Encrypted by default (IPSec)
- Works over any internet connection

#### Components

| Component | What It Is | Key Detail |
|-----------|-----------|-----------|
| **Customer Gateway (CGW)** | AWS resource representing your on-prem VPN device | Stores public IP + BGP ASN of your router |
| **Customer Gateway Device** | Actual physical/virtual router at your site | Cisco, Juniper, pfSense, StrongSwan, etc. |
| **VPN Connection** | Logical IPSec connection between CGW and VGW/TGW | Contains 2 tunnels + routing config |
| **Virtual Private Gateway (VGW)** | AWS-side termination point | Attached to single VPC |
| **Transit Gateway (TGW)** | AWS-side termination (multi-VPC) | Enables access to all TGW-attached VPCs |

#### Technical Specifications

| Parameter | Value |
|-----------|-------|
| **IKE versions** | IKEv1, IKEv2 |
| **Encryption** | AES-128, AES-256 |
| **Integrity** | SHA-1, SHA-256, SHA-384, SHA-512 |
| **DH Groups** | 2, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24 |
| **Tunnel bandwidth** | Up to 1.25 Gbps per tunnel |
| **Max bandwidth (2 tunnels + ECMP)** | Up to 2.5 Gbps |
| **Dead Peer Detection (DPD)** | Enabled by default |
| **NAT-Traversal (NAT-T)** | Supported (UDP 4500) |
| **Routing** | Static or BGP (dynamic) |
| **BGP hold timer** | 90 seconds (30s keepalive) |

#### Static vs BGP Routing

| Feature | Static Routing | BGP Dynamic Routing |
|---------|---------------|---------------------|
| Configuration | Manual CIDR entries | Automatic route exchange |
| Failover between tunnels | Manual | Automatic |
| ECMP support | No | Yes (with TGW ECMP enabled) |
| Route changes propagation | Manual update required | Automatic |
| VPN CloudHub support | ❌ No | ✅ Yes |
| Complexity | Low | Medium |
| Use case | Simple, small-scale | Production, multi-site, HA |

> 🔑 **Exam key:** BGP is required for VPN CloudHub and for TGW VPN ECMP. Static routing works but lacks automatic failover between the two tunnels.

#### VPN Configuration Download

AWS console generates ready-to-use config files for 50+ device types (Cisco IOS, Cisco ASA, Juniper, Palo Alto, Fortinet, pfSense, StrongSwan, OpenVPN, Yamaha, etc.). Includes: pre-shared keys, IKE Phase 1/2 settings, tunnel IPs, BGP peer IPs.

```mermaid
graph LR
    OnPrem["🏢 On-Premises\nCustomer Gateway Device"] -->|"Tunnel 1 (AZ-a)"| VGW["🔒 Virtual Private\nGateway (VGW)"]
    OnPrem -->|"Tunnel 2 (AZ-b)"| VGW
    VGW --> VPC["🔷 VPC"]
    VPC --> EC2["EC2 Instances\nRDS, etc."]
```

> ⚠️ **Limitation:** VGW-based VPN connects to ONE VPC only. For multi-VPC access, use TGW-based VPN.

---

### 🔗 AWS VPN CloudHub

#### What It Is

VPN CloudHub is a **pattern** (not a separate AWS service) — it uses a single VGW with multiple CGWs attached to create a **hub-and-spoke WAN** where branch offices can communicate with AWS AND with each other through the VGW.

> 🔑 **Exam key:** CloudHub = VGW acting as hub. Branches talk to each other through AWS. Requires BGP. NOT a separate service — it's a VGW configuration pattern.

#### How It Works

1. Each branch office has its own CGW and VPN connection to the **same VGW**
2. Each branch uses a **unique BGP ASN** — required for route re-advertisement
3. VGW learns Branch-A routes via BGP, re-advertises to Branch-B → branch-to-branch routing
4. All branch-to-branch traffic transits through AWS VGW

```mermaid
graph TD
    BranchA["🏢 Branch A\nASN 65001"] -->|"VPN Tunnel"| VGW["🔒 VGW\n(Hub)"]
    BranchB["🏢 Branch B\nASN 65002"] -->|"VPN Tunnel"| VGW
    BranchC["🏢 Branch C\nASN 65003"] -->|"VPN Tunnel"| VGW
    VGW --> VPC["🔷 VPC"]
    BranchA -.->|"Branch-to-branch\nvia VGW"| BranchB
    BranchB -.->|"Branch-to-branch\nvia VGW"| BranchC
```

#### Requirements

- BGP (dynamic routing) — static routing **cannot** enable CloudHub
- Each branch must have a **unique BGP ASN**
- Each CGW must have a **unique public IP address**
- Maximum **10 CGWs per VGW**

#### CloudHub vs TGW for Multi-Site

| Feature | VPN CloudHub (VGW) | TGW + Multi-Site VPN |
|---------|-------------------|----------------------|
| Max sites | 10 CGWs per VGW | 5000 attachments per TGW |
| VPC access | Single VPC | All TGW-attached VPCs |
| ECMP support | No | Yes (TGW ECMP) |
| Accelerated VPN | No | Yes |
| Routing granularity | Limited | Full TGW route tables |
| Cost | VPN connection + data | TGW attachment + VPN + data |
| Complexity | Low | Medium-High |
| Use case | Small branch networks | Enterprise multi-VPC |

---

### 🔀 AWS Transit Gateway + Site-to-Site VPN

#### What It Is

Attach S2S VPN connections **directly to TGW** instead of VGW. This gives VPN-connected sites access to **all VPCs attached to TGW** — not just one.

#### Advantages Over VGW-Based VPN

- One VPN connection → access to hundreds of VPCs via TGW route tables
- **VPN ECMP**: enable on TGW → load-balance traffic across multiple VPN tunnels (doubles bandwidth per added tunnel)
- BGP routes propagate directly into TGW route tables
- Network segmentation via TGW route table domains (keep VPN traffic in its own RT)
- Supports **Accelerated VPN** (not available with VGW)

#### Accelerated Site-to-Site VPN

- Integrates with **AWS Global Accelerator** — traffic enters AWS network at nearest edge PoP (not at the VPN endpoint region)
- Reduces latency significantly for geographically distant sites
- Traffic: Remote Site → nearest AWS PoP (GA edge) → AWS backbone → TGW
- Only available with **TGW attachment** (not VGW)
- Additional cost: ~$0.025/hr per accelerator + data transfer fees

```mermaid
graph LR
    Branch1["🏢 Branch 1"] -->|"VPN"| TGW["🔀 Transit Gateway"]
    Branch2["🏢 Branch 2"] -->|"VPN"| TGW
    Branch3["🏢 Branch 3"] -->|"VPN"| TGW
    TGW --> VPCA["🔷 VPC-A\nProduction"]
    TGW --> VPCB["🔷 VPC-B\nDevelopment"]
    TGW --> VPCC["🔷 VPC-C\nShared Services"]
    TGW --> VPCD["🔷 VPC-D\nData"]
```

#### VGW-Based VPN vs TGW-Based VPN

| Feature | VGW-Based VPN | TGW-Based VPN |
|---------|--------------|---------------|
| VPCs reachable | 1 (attached VPC) | All TGW-attached VPCs |
| ECMP | No | Yes (enable on TGW) |
| Accelerated VPN | No | Yes |
| Network segmentation | No (all or nothing) | Yes (TGW route tables) |
| Max VPN connections | 10 per VGW | 5000 per TGW |
| Cost | Lower (no TGW) | Higher (TGW attachment fee) |
| Route propagation | VPC route table only | TGW route tables |
| BGP required for ECMP | N/A | Yes |

---

### 💻 AWS Software VPN (Unmanaged)

#### What It Is

Running open-source or commercial VPN software on **EC2 instances** in your VPC. You own and manage everything — installation, config, HA, patching, capacity.

#### When to Use

- Need features unavailable in AWS Managed VPN (WireGuard, custom cipher suites, SSL VPN on port 443)
- Cost optimization: small EC2 instance < hourly VPN connection cost at low traffic
- Compliance: specific cipher suites, key management, audit logging requirements
- Existing vendor licensing (Cisco, Palo Alto, Fortinet)
- Protocol flexibility (GRE, L2TP, WireGuard, SSL)

#### EC2 Requirements (All Software VPN)

```bash
# Enable IP forwarding
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p

# Disable source/destination check (required!)
# In AWS Console: EC2 → Actions → Networking → Change Source/Dest Check → Disable
# Or via CLI:
aws ec2 modify-instance-attribute --instance-id i-xxxx --no-source-dest-check
```

**Security Group rules for IPSec VPN:**
| Protocol | Port | Purpose |
|----------|------|---------|
| UDP | 500 | IKE (key exchange) |
| UDP | 4500 | NAT-T (IKE over NAT) |
| Protocol 50 | — | ESP (encrypted data) |
| Protocol 51 | — | AH (optional, auth only) |

**VPC Route Table:** Add static route: `<remote-site-CIDR> → <VPN EC2 ENI ID>`

#### Architecture Patterns

| Pattern | HA | Use Case |
|---------|-----|---------|
| Single EC2 | None (SPOF) | Dev/test only |
| Two EC2 + health check script (ENI reassign) | Active/standby | Simple HA |
| Two EC2 + NLB | Active/active | Production |
| ASG (1 min, 1 max) + NLB | Auto-recovery | Automated recovery |

---

#### OpenVPN on EC2 (Deep Dive)

**What OpenVPN is:**
- SSL/TLS-based VPN — uses **TCP 443** or **UDP 1194**
- NOT natively IPSec — uses its own TLS tunnel protocol (OpenSSL)
- Two editions: OpenVPN Community (open source, free) and OpenVPN Access Server (commercial, web UI)
- Operates in: site-to-site mode (routing between networks) OR client-to-site mode (remote users)
- Cross-platform: Linux, Windows, macOS, iOS, Android

```mermaid
graph LR
    Remote["🏢 Remote Site\nOpenVPN Client"] -->|"TLS/UDP 1194\nor TCP 443"| EC2["🖥️ EC2\n(OpenVPN Server)\nPublic Subnet"]
    EC2 -->|"Routing"| VPC["🔷 VPC\n10.0.0.0/16"]
    VPC --> App["App Servers\nRDS, etc."]
```

**Setup on EC2:**
```bash
# Amazon Linux 2
yum install -y openvpn easy-rsa

# Generate PKI
/usr/share/easy-rsa/3/easyrsa init-pki
/usr/share/easy-rsa/3/easyrsa build-ca
/usr/share/easy-rsa/3/easyrsa gen-req server nopass
/usr/share/easy-rsa/3/easyrsa sign-req server server
/usr/share/easy-rsa/3/easyrsa gen-dh
```

**server.conf (site-to-site):**
```
mode server
tls-server
dev tun
proto udp
port 1194
ca   /etc/openvpn/pki/ca.crt
cert /etc/openvpn/pki/server.crt
key  /etc/openvpn/pki/server.key
dh   /etc/openvpn/pki/dh.pem

server 10.8.0.0 255.255.255.0        # VPN tunnel address pool
push "route 10.0.0.0 255.255.0.0"   # advertise VPC CIDR to remote
route 192.168.1.0 255.255.255.0      # route to remote site network

cipher AES-256-GCM
tls-version-min 1.2
keepalive 10 120
persist-key
persist-tun
```

**HA with NLB:**
```mermaid
graph LR
    Remote["🏢 Remote Site"] --> NLB["⚖️ NLB\n(UDP 1194)"]
    NLB --> OVA["🖥️ OpenVPN EC2-A\nAZ-1"]
    NLB --> OVB["🖥️ OpenVPN EC2-B\nAZ-2"]
    OVA --> VPC["🔷 VPC"]
    OVB --> VPC
```

> ⚠️ **Note:** OpenVPN NLB HA requires stateless design or sticky sessions — NLB UDP doesn't guarantee session persistence without source IP affinity.

---

#### StrongSwan on EC2 (Deep Dive)

**What StrongSwan is:**
- Open-source **IKEv1/IKEv2 + IPSec** implementation for Linux
- Uses Linux kernel **XFRM** framework for IPSec (high performance, no userspace overhead)
- Industry standard — interoperates with Cisco, Juniper, Palo Alto, AWS Managed S2S VPN, Azure VPN Gateway
- Supports: PSK auth, certificate auth, EAP, X.509, RSA, ECDSA

**Key configuration files:**
| File | Purpose |
|------|---------|
| `/etc/ipsec.conf` | Connection definitions (peers, subnets, transforms) |
| `/etc/ipsec.secrets` | Pre-shared keys or cert private key references |
| `/etc/strongswan.conf` | Daemon settings (charon process config) |

```mermaid
graph LR
    OnPrem["🏢 On-Prem\nCisco/Juniper/pfSense"] -->|"IKEv2/IPSec\nESP"| SW["🖥️ EC2 StrongSwan\nPublic Subnet"]
    SW -->|"Routing"| VPC["🔷 VPC\n10.0.0.0/16"]
```

**/etc/ipsec.conf example:**
```
config setup
    charondebug="ike 1, knl 1"

conn aws-vpc
    left=%defaultroute
    leftid=54.1.2.3                     # EC2 Elastic IP
    leftsubnet=10.0.0.0/16              # VPC CIDR
    right=203.0.113.1                   # On-prem public IP
    rightsubnet=192.168.0.0/24          # On-prem LAN CIDR
    keyexchange=ikev2
    authby=psk
    ike=aes256-sha256-modp2048!         # Phase 1: AES256, SHA256, DH14
    esp=aes256-sha256-modp2048!         # Phase 2 (ESP)
    auto=start
    dpdaction=restart
    dpddelay=30s
    dpdtimeout=120s
    ikelifetime=8h
    lifetime=1h
    margintime=9m
    rekeymargin=3m
```

**/etc/ipsec.secrets:**
```
54.1.2.3 203.0.113.1 : PSK "YourStrongPreSharedKey!"
```

**EC2 setup:**
```bash
# Install
amazon-linux-extras install -y epel
yum install -y strongswan

# Enable IP forwarding
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p

# Start
systemctl enable strongswan
systemctl start strongswan

# Check status
ipsec statusall
ipsec status
```

**HA with FRRouting (BGP failover):**
```bash
# Install FRRouting for BGP
yum install -y frr
# Configure BGP to advertise/withdraw routes based on tunnel state
# Route monitor script: if ipsec tunnel down → withdraw BGP prefix → traffic fails to standby
```

> 🔑 **Exam key:** StrongSwan is the recommended open-source IPSec solution for EC2-based VPN. It interoperates natively with AWS Managed S2S VPN (both speak IKEv2). Use StrongSwan when you need standard IPSec without the cost of hardware appliances.

---

### 🛒 Third-Party VPN Appliances (AWS Marketplace)

AWS Marketplace offers EC2-based VPN/firewall AMIs from major vendors:

| Vendor | Product | Key Feature |
|--------|---------|------------|
| Cisco | CSR 1000v | Full IOS-XE, SD-WAN, BGP, MPLS |
| Palo Alto | VM-Series | NGFW + VPN, App-ID |
| Fortinet | FortiGate VM | NGFW + SD-WAN + VPN |
| Juniper | vSRX | Full JunOS routing + VPN |
| pfSense | pfSense CE | Open source, low cost |
| Check Point | CloudGuard | NGFW + VPN + threat prevention |
| Barracuda | CloudGen Firewall | SD-WAN + VPN |

**Deployment pattern:**
- Launch in **public subnet**
- Disable source/destination check
- Assign Elastic IP
- Manage via vendor web console or SSH
- HA: vendor-native (VRRP, HA pairs) or AWS-native (NLB + health checks)
- Licensing: BYOL or license-included (higher EC2 cost)

---

### 🆚 Comparison: All Remote Network to VPC Options

| Option | Managed By | Max Bandwidth | HA Built-in | BGP | Encryption | Cost Model | Best For |
|--------|-----------|--------------|-------------|-----|-----------|-----------|---------|
| **S2S VPN (VGW)** | AWS | 2.5 Gbps (2 tunnels) | 2 tunnels/AZ | Optional | Yes (IPSec) | $0.05/hr + $0.09/GB | Single VPC, simple hybrid |
| **S2S VPN (TGW)** | AWS | 2.5 Gbps/conn + ECMP | 2 tunnels/AZ | Optional | Yes (IPSec) | TGW + VPN + data | Multi-VPC, enterprise |
| **VPN CloudHub** | AWS | 2.5 Gbps per site | 2 tunnels | Required | Yes (IPSec) | Per VPN connection | Multi-branch without WAN |
| **Accelerated VPN** | AWS + GA | 2.5 Gbps/conn | 2 tunnels/AZ | Optional | Yes (IPSec) | VPN + GA fees | Distant sites, low latency |
| **OpenVPN on EC2** | You | EC2 NIC limit (~10Gbps) | NLB + 2 EC2 | No | TLS | EC2 + NLB | SSL VPN, port 443, flexibility |
| **StrongSwan on EC2** | You | EC2 NIC limit | EC2 pair + routing | Via FRRouting | IPSec | EC2 | Standard IPSec, enterprise interop |
| **Cisco CSR/Marketplace** | You (vendor) | Up to 10 Gbps | Vendor HA | Yes | IPSec/SSL | EC2 + license | Existing Cisco/vendor shops |
| **Direct Connect** | AWS + partner | Up to 100 Gbps | LAG + multi-location | Required | No (add MACsec) | Port/hr + data | Production, consistent latency |
| **DX + VPN** | AWS | DX bandwidth | DX primary + VPN backup | Yes | VPN for encryption | DX + VPN | Encrypted high-bandwidth hybrid |

---

### 👤 Site-to-Site VPN vs AWS Client VPN

| Feature | Site-to-Site VPN | AWS Client VPN |
|---------|-----------------|----------------|
| **Use case** | Network-to-network | User device-to-VPC |
| **Initiator** | VPN router/device | End-user computer/phone |
| **Protocol** | IPSec (IKEv1/v2) | OpenVPN (TLS) |
| **AWS endpoint** | VGW or TGW | Client VPN Endpoint (ENI in subnet) |
| **Authentication** | PSK or certificates | AD/LDAP, SAML, mutual cert |
| **Routing** | Static or BGP | Static (split tunnel or full tunnel) |
| **Scaling** | Fixed 2 tunnels | Auto-scales to ~20,000 concurrent |
| **Cost** | $0.05/hr + $0.09/GB | $0.10/hr endpoint + $0.05/hr per connection |
| **Split tunneling** | N/A | Supported |
| **MFA** | Not natively | Yes (via IdP + SAML) |
| **When to use** | Office/DC connectivity | Remote workers, contractors |

> 🔑 **Exam key:** Client VPN = per-user TLS (think "VPN app on laptop"). Site-to-Site VPN = router-to-router IPSec (think "office to cloud"). Wrong VPN type = wrong answer on exam.

---

### 🏛️ Well-Architected Framework — Remote Network Connectivity

| Pillar | Consideration |
|--------|--------------|
| **Operational Excellence** | Use CloudWatch metrics for VPN tunnel state (TunnelState = 0 means down). Set alarms. Download AWS-generated config files to reduce misconfiguration. Use AWS Network Manager for global VPN visibility. |
| **Security** | S2S VPN encrypts traffic in transit by default (IPSec). Use IKEv2 over IKEv1. Use certificate auth over PSK where possible. Software VPN: rotate PSKs/certs regularly. |
| **Reliability** | Always provision 2 tunnels (both active). Use BGP for automatic failover. For critical workloads: DX primary + VPN backup. For Software VPN: deploy HA pair in 2 AZs behind NLB. |
| **Performance Efficiency** | TGW VPN ECMP for bandwidth aggregation. Accelerated VPN for geographically distant sites. Choose appropriate EC2 instance size for Software VPN (network-optimized: c5n, m5n). |
| **Cost Optimization** | Compare: S2S VPN hourly vs EC2-based hourly — at low traffic, EC2 t3.micro StrongSwan can be cheaper. For high bandwidth/steady traffic, DX is cheaper per GB than VPN. CloudHub eliminates dedicated WAN circuits. |
| **Sustainability** | Consolidated hub-and-spoke (CloudHub or TGW) reduces total number of tunnel endpoints and associated compute. DX reduces internet routing hops = fewer devices = lower energy per GB. |

---

### 📋 SAP-C02 Domain Coverage — Remote Network Connectivity

#### Domain 1: Design Solutions for Organizational Complexity
- Multi-account VPN strategy: TGW shared via RAM → all accounts access on-prem via single TGW+VPN
- Network segmentation: separate TGW route tables per business unit, VPN routes only in appropriate RTs
- Centralized network account pattern: DX + VPN terminate in network account, spoke VPCs connect via TGW

#### Domain 2: Design for New Solutions
- Choosing VPN vs DX based on bandwidth, latency SLA, budget, timeline
- CloudHub for branch office connectivity without SD-WAN investment
- Accelerated VPN for latency-sensitive workloads connecting to AWS from distant locations
- Software VPN (StrongSwan) for custom cipher compliance requirements

#### Domain 3: Continuous Improvement for Existing Solutions
- Migrating from VGW-based VPN to TGW-based VPN for multi-VPC access
- Adding DX as primary, demoting VPN to backup (BGP MED manipulation)
- Upgrading from static to BGP routing to enable ECMP and auto-failover
- Replacing Software VPN with AWS Managed VPN to reduce operational overhead

#### Domain 4: Accelerate Workload Migration and Modernization
- VPN as first step in hybrid migration (fast to provision vs DX 4-12 week lead time)
- Multiple VPN connections with ECMP during migration for bandwidth
- Transitioning from VPN to DX as workloads stabilize in AWS
- SD-WAN integration via TGW Connect attachment (GRE) for modern branch networking

---

### ☁️ Azure / Open Source Equivalents

| AWS Service/Concept | Azure Equivalent | Open Source Equivalent |
|--------------------|-----------------|----------------------|
| Site-to-Site VPN (VGW) | Azure VPN Gateway (S2S) | StrongSwan, OpenVPN, WireGuard |
| VPN CloudHub | Azure Virtual WAN (Basic tier) | FRRouting BGP hub |
| TGW + VPN | Azure Virtual WAN (Standard) | FRRouting + Linux VRF + IPSec |
| Accelerated VPN | Azure ExpressRoute + VPN (no direct equiv) | WireGuard (lower overhead) |
| AWS Client VPN | Azure Point-to-Site VPN | OpenVPN Access Server |
| Software VPN (EC2) | Azure VM + StrongSwan/OpenVPN | Any VPN daemon on Linux VM |

> **WireGuard** — modern alternative to OpenVPN/IPSec. Faster, simpler, smaller codebase. Built into Linux kernel 5.6+. Not natively supported by AWS Managed VPN but works on EC2. Good for dev/test or cost-sensitive scenarios.

---

## 🌍 Chapter 7: AWS Resource Scope — Global, Regional, Zonal and Edge

### 📖 Overview

Every AWS resource has a **scope** — the boundary within which it exists and can be used. Understanding scope is critical for:
- **High Availability** design (multi-AZ, multi-region)
- **Disaster Recovery** planning (what needs cross-region replication?)
- **Data Residency / Compliance** (where does data physically live?)
- **IAM and access control** (which regions/accounts can use a resource?)
- **Cost optimization** (cross-AZ and cross-region transfers cost money)

> 🔑 **Exam key:** SAP-C02 frequently tests scope awareness. Wrong scope assumption = wrong answer. Know what's global vs regional vs zonal before answering architecture questions.

**Scope hierarchy:**
```
Global → Regional → AZ (Zonal) → Local Zone / Wavelength Zone
                               ↑ Edge Location (CDN/DNS PoPs — not deployable)
```

---

### 🗺️ Scope Types Explained

#### 🌐 Global Resources

- Exist **once per AWS account** — not tied to any region
- Accessible and usable from ALL regions simultaneously
- AWS manages availability — no regional failover needed
- **Examples:**

| Service/Resource | Notes |
|---|---|
| IAM (users, roles, policies, groups) | Created anywhere, usable everywhere |
| Route 53 | Hosted zones, health checks, DNS global |
| CloudFront | Distribution is global; serves from nearest edge |
| AWS Organizations | SCPs, OUs, accounts — account-level |
| AWS WAF (Classic) | v2 WAF is regional (except on CloudFront) |
| Direct Connect Gateway | Global hub for multi-region DX |
| S3 bucket namespace | Name is globally unique; DATA is regional |
| AWS Artifact | Compliance reports, global |
| AWS Support plan | Per account |
| Global Accelerator | Resource global; traffic served regionally |
| AWS Shield Advanced | Global DDoS protection at edge |

> 🔑 **Exam key:** IAM role created in "us-east-1 console" is still global — usable in eu-west-1, ap-southeast-1, everywhere. The console just shows your current region.

---

#### 🗺️ Regional Resources

- Exist within **one AWS region**
- NOT automatically replicated across regions
- Must be explicitly recreated, replicated, or deployed per region
- ARN includes region: `arn:aws:lambda:**us-east-1**:123456789012:function:my-fn`

| Service/Resource | Scope | Notes |
|---|---|---|
| VPC | Regional | Spans all AZs in the region |
| Security Group | Regional | Scoped to VPC (regional) |
| NACL | Regional | Scoped to VPC |
| Internet Gateway | Regional | Attached to VPC |
| Route Table | Regional | Scoped to VPC |
| Transit Gateway | Regional | Connect VPCs in same region |
| VPN Connection | Regional | Terminates at VGW or TGW |
| VGW (Virtual Private GW) | Regional | Attached to one VPC |
| NAT Gateway | Regional\* | \*Deployed per-AZ; see Common Confusions |
| ALB / NLB / CLB | Regional | Serves across AZs |
| Auto Scaling Group | Regional | Spans AZs |
| Lambda | Regional | Code deployed per region |
| ECS / EKS cluster | Regional | Runs tasks/pods across AZs |
| ECR | Regional | Container registry per region |
| RDS (service) | Regional | Instances are AZ; Multi-AZ = 2 AZs |
| DynamoDB (standard) | Regional | Global Tables = multi-region |
| S3 (data) | Regional | Data stays in bucket's region |
| SNS / SQS | Regional | Topics/queues per region |
| EventBridge | Regional | Bus per region |
| Step Functions | Regional | |
| CodePipeline / CodeBuild | Regional | |
| CloudWatch | Regional | Metrics, alarms, logs per region |
| X-Ray | Regional | |
| Secrets Manager | Regional | Cross-region replication available |
| KMS CMK | Regional | MRK = multi-region replica |
| Parameter Store | Regional | |
| CloudTrail | Regional + Global option | Can enable global service events |
| Config | Regional | Aggregator = cross-region/account |
| GuardDuty | Regional | Must enable per region |
| Security Hub | Regional | Must enable per region |
| Inspector | Regional | |
| Macie | Regional | |
| Systems Manager | Regional | |
| EBS Snapshots | Regional | Can copy to other regions |
| AMIs | Regional | Can copy to other regions |
| VPC Endpoints | Regional | |
| PrivateLink | Regional | |
| DX Connection | Regional | Physical port at PoP |
| ACM Certificate | Regional\* | \*us-east-1 required for CloudFront |
| Direct Connect Connection | Regional | |

---

#### 🏢 AZ (Zonal) Resources

- Exist within **one specific Availability Zone**
- Cannot be moved to another AZ — must snapshot/recreate
- Failure of that AZ = resource unavailable

| Resource | Notes |
|---|---|
| EC2 instance | Launched in one AZ; cannot move |
| EBS volume | Attached to EC2 in same AZ only |
| Subnet | Each subnet = one AZ |
| ENI (Elastic Network Interface) | Bound to subnet → bound to AZ |
| NAT Gateway (instance) | One per AZ for HA |
| RDS instance (Single-AZ) | One AZ; Multi-AZ = sync standby in second AZ |
| ElastiCache node | Per AZ |
| Redshift single-node | Per AZ |
| EFS mount target | One per AZ (EFS itself is regional) |
| FSx file system | Per AZ (FSx Multi-AZ = 2 AZs) |

> ⚠️ **Critical:** EBS volume in `us-east-1a` CANNOT attach to EC2 in `us-east-1b`. Must: snapshot → create new volume in `us-east-1b` → attach.

---

#### 📍 Local Zone Resources

- AWS infrastructure in **metro areas** (LA, Chicago, Dallas, Miami, etc.)
- Extension of parent region — same API, same console, same VPC
- **Must opt-in** per account in EC2 console settings
- Subset of services: EC2, EBS, VPC subnets, ELB, RDS, ElastiCache
- Use case: single-digit ms latency for users in that city
- Subnet in Local Zone = zonal resource in that Local Zone

```
Parent Region: us-west-2
  Local Zone: us-west-2-lax-1  (Los Angeles)
  Local Zone: us-west-2-den-1  (Denver)
```

> 🔑 **Exam key:** Local Zones are for **latency to end users** — studios, media production, gaming. Not for regulatory data residency (not a separate region). Data still counts as us-west-2.

---

#### 📡 Wavelength Zone Resources

- AWS compute **embedded at 5G carrier edge** (Verizon, Vodafone, KDDI, SKT)
- Ultra-low latency: **<10ms** to 5G mobile devices
- Traffic stays on carrier network — does NOT traverse public internet
- Services: EC2, EBS, VPC subnets
- Use cases: real-time gaming, AR/VR, autonomous vehicles, live video streaming
- Parent region required — Wavelength Zone is an extension of a region

| | Local Zone | Wavelength Zone |
|---|---|---|
| **Use case** | Low latency to city users | Ultra-low latency to 5G devices |
| **Network** | Internet | 5G carrier network |
| **Latency** | Single-digit ms | <10ms (carrier edge) |
| **Services** | More (RDS, ELB, etc.) | EC2, EBS, VPC only |
| **Provider** | AWS | AWS + carrier partnership |

---

#### 🌍 Edge Locations

- **400+ PoPs worldwide** — NOT deployable compute
- AWS-managed infrastructure for CDN, DNS, DDoS
- You do NOT launch instances here — you configure distributions/endpoints

| Service | What happens at edge |
|---|---|
| CloudFront | Caches content; TLS termination; WAF v2 |
| Route 53 | Anycast DNS resolution |
| Global Accelerator | Traffic ingestion; routed to regional endpoint |
| Shield Advanced | DDoS mitigation at edge |
| Lambda@Edge / CloudFront Functions | Code execution at edge PoP |

---

### 📋 Master Reference Table — Quick Scope Lookup

| Service | Scope | Key Exam Note |
|---|---|---|
| IAM | 🌐 Global | Created anywhere, works everywhere |
| Route 53 | 🌐 Global | DNS, anycast from edge |
| CloudFront | 🌐 Global | Cert must be in us-east-1 |
| AWS Organizations | 🌐 Global | SCPs apply account-wide |
| Direct Connect Gateway | 🌐 Global | One DXGW → multi-region VPCs |
| Global Accelerator | 🌐 Global | Anycast IPs, regional endpoints |
| S3 bucket name | 🌐 Global namespace | Data is regional |
| ACM (for CloudFront) | 🌐 Must be us-east-1 | CloudFront only reads us-east-1 ACM |
| VPC | 🗺️ Regional | Spans all AZs in region |
| Transit Gateway | 🗺️ Regional | Inter-region via TGW peering |
| Security Group | 🗺️ Regional | Scoped to VPC |
| ALB / NLB | 🗺️ Regional | Multi-AZ by design |
| Lambda | 🗺️ Regional | Deploy per region |
| DynamoDB (standard) | 🗺️ Regional | Global Tables = multi-region |
| DynamoDB Global Tables | 🌐 Multi-region | Active-active |
| S3 bucket data | 🗺️ Regional | Stays in bucket's region |
| KMS CMK | 🗺️ Regional | MRK = cross-region replica |
| CloudWatch | 🗺️ Regional | Metrics don't cross regions |
| ACM (for ALB/NLB) | 🗺️ Regional | Must match resource region |
| GuardDuty | 🗺️ Regional | Enable per region |
| EC2 instance | 🏢 AZ | Cannot move AZ |
| EBS volume | 🏢 AZ | Attach only in same AZ |
| Subnet | 🏢 AZ | One subnet = one AZ |
| ENI | 🏢 AZ | Bound to subnet |
| NAT Gateway | 🏢 AZ | One per AZ for HA |
| RDS instance | 🏢 AZ | Multi-AZ = 2nd AZ standby |
| EFS (data) | 🗺️ Regional | Mount targets are per-AZ |
| Local Zone subnet | 📍 Local Zone | Metro extension of region |
| Wavelength subnet | 📡 Wavelength | 5G carrier edge |
| CloudFront PoP | 🌍 Edge | 400+ locations |

---

### 🔑 Scope Implications for Architecture

#### High Availability

| Scope | HA Pattern |
|---|---|
| AZ resources | Deploy in **3 AZs minimum**; ALB distributes traffic |
| Regional resources | Single region = regional SPOF; active-passive or active-active cross-region |
| Global resources | Inherently HA — AWS manages |

#### Disaster Recovery

| Resource | Cross-Region Strategy |
|---|---|
| S3 | Cross-Region Replication (CRR) |
| RDS | Cross-region read replica → promote on failover |
| DynamoDB | Global Tables (active-active) |
| Lambda | Redeploy via IaC (CloudFormation StackSets) |
| EC2 AMI | Copy AMI to target region |
| EBS Snapshot | Copy to target region |
| KMS key | Create MRK or new CMK in target region |
| Route 53 | Health checks + failover routing policy |
| Secrets Manager | Cross-region replication (built-in) |

#### Data Residency / Compliance (GDPR, Data Sovereignty)

- **Regional resources** stay in their region — EU data in `eu-west-1` stays in EU ✅
- **Global resources** (IAM metadata, Route 53): stored on AWS backbone — not customer-selectable ⚠️
- **S3**: bucket data stays in region; name is global but data is not
- **CloudFront**: caches data at edge PoPs globally — use **Origin Shield** to limit caching regions; for GDPR use **geo-restriction** and separate distribution per region
- **SCP example**: deny any action where `aws:RequestedRegion` is outside EU

```json
{
  "Effect": "Deny",
  "Action": "*",
  "Resource": "*",
  "Condition": {
    "StringNotEquals": {
      "aws:RequestedRegion": ["eu-west-1", "eu-central-1"]
    }
  }
}
```

> ⚠️ This SCP does NOT restrict global services (IAM, Route 53, CloudFront) — they are exempt from region conditions. Use separate controls for those.

#### Cost: Cross-AZ and Cross-Region Transfer

| Transfer type | Cost |
|---|---|
| Same AZ, same VPC | **Free** |
| Cross-AZ (within region) | ~$0.01/GB each direction |
| Cross-region | $0.02–$0.09/GB (varies by region pair) |
| Internet egress | $0.09/GB (first 10TB/month) |
| CloudFront → origin | Cheaper than direct internet |
| DX data out | ~50–70% cheaper than internet |

> 🔑 **Exam cost trap:** NAT Gateway in one AZ serving private subnets in other AZs → cross-AZ charges. Best practice: one NAT GW **per AZ**.

---

### 🆚 Common Scope Confusions

#### NAT Gateway — Regional Service, AZ Deployment
- Service is **regional** (you manage via regional API)
- Physical instance is **per-AZ**
- **Best practice:** one NAT GW per AZ
- Single NAT GW = cross-AZ charges + AZ SPOF for all outbound internet

```mermaid
graph TD
    subgraph "Best Practice — 3 NAT GWs"
        AZ_A["AZ-A Private Subnet"] --> NATGW_A["NAT GW AZ-A"]
        AZ_B["AZ-B Private Subnet"] --> NATGW_B["NAT GW AZ-B"]
        AZ_C["AZ-C Private Subnet"] --> NATGW_C["NAT GW AZ-C"]
    end
    subgraph "Anti-Pattern — 1 NAT GW"
        AZ_A2["AZ-A Private Subnet"] -->|cross-AZ $| NATGW_S["NAT GW AZ-A only"]
        AZ_B2["AZ-B Private Subnet"] -->|cross-AZ $| NATGW_S
        AZ_C2["AZ-C Private Subnet"] -->|cross-AZ $| NATGW_S
    end
```

#### ACM — Regional AND Requires us-east-1 for CloudFront
- ALB/NLB/API Gateway: cert must be in **same region as the resource**
- CloudFront: cert must be in **us-east-1** regardless of where your origin is
- Common mistake: create ACM cert in eu-west-1 → try to attach to CloudFront → fails

#### S3 — Global Namespace, Regional Data
- Bucket name: globally unique (like a domain name)
- Bucket data: stored only in the region you chose at creation
- Replication: S3 Cross-Region Replication (CRR) is explicit — not automatic

#### EFS vs EBS — Regional vs AZ
| | EFS | EBS |
|---|---|---|
| **Scope** | Regional (data) | AZ (volume) |
| **Mount targets** | Per-AZ (mount from any AZ) | N/A — attached directly |
| **Multi-AZ access** | ✅ Yes (via mount targets) | ❌ No |
| **Protocol** | NFS | Block device |

#### KMS MRK (Multi-Region Key)
- Standard CMK: **regional** — cannot encrypt/decrypt across regions
- MRK: same key material replicated across chosen regions
- Same key ID prefix (`mrk-`) — interoperable across regions
- Use case: DynamoDB Global Tables, S3 CRR with SSE-KMS, cross-region Lambda

---

### 🗂️ Scope and ARN Format

ARN reveals scope:
```
arn:partition:service:region:account-id:resource-type/resource-id
```

| Scope | Region in ARN | Example |
|---|---|---|
| **Global** | **Empty** | `arn:aws:iam::123456789012:role/MyRole` |
| **Regional** | Region code | `arn:aws:lambda:us-east-1:123456789012:function:MyFn` |
| **AZ** | Region code (AZ in resource) | `arn:aws:ec2:us-east-1:123456789012:subnet/subnet-abc` |

> 🔑 **Exam key:** Empty region field in ARN = global resource. If you see `arn:aws:iam::...` — no region = IAM is global.

---

### 🔄 Scope Decision Flowchart

```mermaid
flowchart TD
    Q1{"Does the resource exist\nonce per AWS account\nacross ALL regions?"}
    Q1 -->|Yes| GLOBAL["🌐 GLOBAL\nIAM, Route 53, CloudFront\nOrganizations, DXGW"]
    Q1 -->|No| Q2{"Does it exist per\nAWS region, shared\nacross AZs?"}
    Q2 -->|Yes| REGIONAL["🗺️ REGIONAL\nVPC, TGW, Lambda, ALB\nDynamoDB, S3 data, KMS"]
    Q2 -->|No| Q3{"Does it exist in\none specific AZ only?"}
    Q3 -->|Yes| ZONAL["🏢 AZ / ZONAL\nEC2, EBS, Subnet, ENI\nNAT GW instance, RDS instance"]
    Q3 -->|No| Q4{"Is it at 5G\ncarrier edge?"}
    Q4 -->|Yes| WAVELENGTH["📡 WAVELENGTH\n5G carrier embedded"]
    Q4 -->|No| Q5{"Is it in a\nmetro extension?"}
    Q5 -->|Yes| LOCALZONE["📍 LOCAL ZONE\nMetro area extension"]
    Q5 -->|No| EDGE["🌍 EDGE LOCATION\nCloudFront PoP\nRoute 53, Shield"]
```

---

### 🌐 Azure Equivalent Scope Mapping

| AWS Scope | Azure Equivalent | Notes |
|---|---|---|
| 🌐 Global (IAM) | Azure AD / Entra ID (tenant-level) | Both are identity planes |
| 🌐 Global (Route 53) | Azure DNS (global) | |
| 🌐 Global (CloudFront) | Azure CDN / Front Door | Front Door is global |
| 🗺️ Regional | Azure Region | Same concept |
| 🏢 AZ (Zonal) | Azure Availability Zone | Same concept |
| 📍 Local Zone | Azure Edge Zone | Metro extension |
| 📡 Wavelength | Azure Edge Zone for Operators | 5G carrier edge |
| 🌍 Edge Location | Azure CDN PoP / Front Door PoP | CDN/DNS edge |

---

### 🏛️ Well-Architected Framework — Resource Scope

| Pillar | Scope Consideration |
|---|---|
| **Operational Excellence** | Tag resources with scope metadata (region, AZ). Use CloudFormation StackSets for consistent multi-region deployments. Document which resources are AZ-bound for runbook accuracy. |
| **Security** | Use SCPs to restrict which regions resources can be created in (data sovereignty). Understand IAM is global — compromised IAM credentials work in all regions. Enable GuardDuty/Security Hub in ALL regions (regional service — off by default per region). |
| **Reliability** | AZ resources: deploy in ≥3 AZs. Regional resources: plan cross-region DR. Global resources: inherently resilient. NAT GW: one per AZ to eliminate cross-AZ SPOF. |
| **Performance Efficiency** | Local Zones for sub-10ms user latency. Wavelength for <10ms 5G workloads. CloudFront edge for caching. Cross-AZ traffic adds latency — keep tightly-coupled services in same AZ. |
| **Cost Optimization** | Cross-AZ transfer ~$0.01/GB — minimize by placing dependent resources in same AZ. Cross-region transfer expensive — use regional services for local workloads. One NAT GW per AZ to avoid cross-AZ fees. |
| **Sustainability** | Minimize cross-region/cross-AZ data transfer — fewer network hops = less energy. Consolidate workloads in fewer regions if business requirements allow. Use CloudFront to serve cached content closer to users (less origin round-trips). |

---

### 📋 SAP-C02 Domain Coverage — Resource Scope

#### Domain 1: Design Solutions for Organizational Complexity
- Multi-account scope: IAM global vs regional service enablement (GuardDuty, Security Hub per region)
- SCP region restrictions for data sovereignty across AWS Organizations
- Resource Access Manager (RAM): share regional resources (TGW, subnets, Route 53 Resolver rules) across accounts

#### Domain 2: Design for New Solutions
- Choosing between regional deployment vs multi-region active-active
- ACM cert placement (regional vs us-east-1 for CloudFront)
- KMS MRK design for cross-region encrypted workloads
- Global Accelerator vs CloudFront — both global, different use cases

#### Domain 3: Continuous Improvement for Existing Solutions
- Adding Global Tables to existing DynamoDB (regional → multi-region)
- Moving from single-AZ NAT GW to per-AZ for HA and cost
- Enabling GuardDuty/Config in missing regions via Organizations automation

#### Domain 4: Accelerate Workload Migration and Modernization
- AMI and snapshot copy strategy for cross-region migration
- Using StackSets to deploy infrastructure in target region
- Data migration scope: S3 CRR, DMS for database replication across regions

---

## 🔀 Chapter 16: VGW vs Direct Connect Gateway vs Transit Gateway — On-Premises Connectivity

### 📖 Overview

When connecting on-premises networks to AWS VPCs, three gateway types are central to every architecture: **Virtual Private Gateway (VGW)**, **Direct Connect Gateway (DXGW / DCG)**, and **Transit Gateway (TGW)**. These are not mutually exclusive — they are often used together in layered designs. Understanding when each is the right tool (and when it breaks down) is critical for the SAP-C02 exam.

> 🔑 **Exam key:** VGW = one VPC, one attachment point. DXGW = global bridge between DX and VPCs (no VPC-to-VPC). TGW = hub-and-spoke for many VPCs, VPNs, and DX connections in a region.

**Analogy:**
- **VGW** = a single door in one room — works great for one room, useless if you have 20 rooms.
- **DXGW** = a shared hallway key — lets you reach many rooms from a single entry, but rooms still can't talk to each other through it.
- **TGW** = a central lobby — everyone connects here, and routing policies control who can talk to whom.

---

### 🔧 Virtual Private Gateway (VGW)

#### What it is

VGW is a **VPC-side endpoint** that terminates VPN tunnels (Site-to-Site VPN) or Direct Connect Private VIFs. It is **attached to exactly one VPC** and acts as the VPC's gateway for hybrid connectivity.

**Scope:** Regional. Tied to one VPC. One VGW per VPC.

```mermaid
graph LR
    OnPrem["🏢 On-Premises\nRouter"] -->|"IPSec VPN or\nDX Private VIF"| VGW["🔒 VGW\n(attached to VPC)"]
    VGW --> VPC["🔷 Single VPC\n10.0.0.0/16"]
```

#### Attachment Types

| Attachment | Protocol | Encryption | Bandwidth |
|------------|----------|------------|-----------|
| **Site-to-Site VPN** | IPSec IKEv1/v2 | AES-256 | Up to 1.25 Gbps per tunnel |
| **DX Private VIF** | BGP/802.1Q | None (private link) | Up to 100 Gbps (DX port speed) |

#### Routing

- Supports **static routes** and **dynamic BGP** (AS 64512 default, configurable)
- With BGP: VGW advertises VPC CIDR to on-premises; on-premises advertises prefixes to VPC
- Route propagation: enable **route propagation** on VPC route tables so BGP-learned routes from VGW automatically populate the route table (without manual static entries)

```bash
# Enable route propagation for a VGW on a route table
aws ec2 enable-vgw-route-propagation \
  --route-table-id rtb-0abc123 \
  --gateway-id vgw-0def456
```

#### Limitations

| Limit | Value |
|-------|-------|
| VPCs per VGW | **1** (one VGW per VPC) |
| VPN tunnels per VGW | Up to 10 VPN connections |
| Max bandwidth (VPN) | 1.25 Gbps per tunnel (no ECMP on VGW) |
| Transitive routing | ❌ Not supported |
| BGP prefix limit | 100 advertised routes to on-premises |
| ECMP | ❌ Not supported on VGW VPN |

> ⚠️ **Critical exam trap:** VGW does NOT support transitive routing. Traffic from on-premises through VGW into VPC **cannot** then route to another VPC (even if the VPCs are peered). For multi-VPC access from on-premises, use TGW.

#### When VGW breaks down

- You have **more than 1 VPC** that on-premises needs to reach → use DXGW or TGW
- You need **VPC-to-VPC routing** through a central point → use TGW
- You need **ECMP** over multiple VPN tunnels for higher bandwidth → use TGW
- You need to share connectivity across **multiple AWS accounts** → use TGW + RAM

---

### 🌍 Direct Connect Gateway (DXGW / DCG)

#### What it is

DXGW is a **globally scoped** AWS resource that acts as a bridge between a DX connection (via a VIF) and one or more VPCs across **any AWS Region** (except China). It decouples the physical DX connection from specific VPC or regional boundaries.

**Scope:** Global. Not tied to any region. One DXGW can serve VPCs in multiple regions.

```mermaid
graph LR
    OnPrem["🏢 On-Premises"] --> DXLoc["📍 DX Location\n(PoP)"]
    DXLoc -->|"Private VIF\nor Transit VIF"| DXGW["🌍 DX Gateway\n(Global Resource)"]
    DXGW -->|VGW association| VPC1["🔷 VPC A\nus-east-1"]
    DXGW -->|VGW association| VPC2["🔷 VPC B\neu-west-1"]
    DXGW -->|VGW association| VPC3["🔷 VPC C\nap-southeast-1"]
    DXGW -->|TGW association| TGW["🔗 Transit Gateway\nus-east-1"]
    TGW --> VPC4["🔷 VPC D"]
    TGW --> VPC5["🔷 VPC E"]
```

#### Relationship to DX and VIFs

| VIF Type | Connects To | DXGW Needed? |
|----------|-------------|--------------|
| **Private VIF** | VGW (single VPC) or DXGW | Optional (needed for multi-VPC/multi-region) |
| **Transit VIF** | DXGW → TGW | **Required** — Transit VIF must go through DXGW |
| **Public VIF** | AWS public endpoints (S3, etc.) | No — Public VIF bypasses DXGW |

> 🔑 **Exam key:** Transit VIF **always** requires a DXGW. You cannot attach a Transit VIF directly to a TGW — it must go DXGW → TGW.

#### VGW Association vs TGW Association

| Feature | DXGW + VGW (Private VIF) | DXGW + TGW (Transit VIF) |
|---------|--------------------------|--------------------------|
| VPCs reachable | Up to 10 VGWs (10 VPCs) | Hundreds via TGW |
| VPC-to-VPC routing | ❌ No | ✅ Yes (via TGW route tables) |
| BGP prefix limit | 100 prefixes to on-premises | 20,000 routes in TGW |
| Cost | Lower (no TGW per-attachment fee) | Higher (TGW attachment + processing fee) |
| Complexity | Lower | Higher but more scalable |

#### Critical DXGW Limitation: No VPC-to-VPC Routing

DXGW is a **hub-and-spoke to on-premises only**. Traffic flows: on-premises ↔ DXGW ↔ VPC. Traffic does NOT flow: VPC A → DXGW → VPC B.

> ⚠️ If you need VPC-to-VPC routing via on-premises (hairpinning), DXGW alone does not do it. Add TGW to the DXGW and use TGW route tables.

#### Limitations

| Limit | Value |
|-------|-------|
| VGWs per DXGW | **10** |
| TGWs per DXGW | **3** |
| DX connections per DXGW | 20 |
| Regions supported | All except China |
| VIF type required for TGW | Transit VIF only |
| BGP prefixes from on-prem | 20,000 (per Transit VIF) |
| Overlapping CIDRs | ❌ VPCs associated with same DXGW must have non-overlapping CIDRs |

---

### 🔗 Transit Gateway (TGW)

#### What it is

TGW is a **regional** network transit hub that interconnects VPCs, VPN connections, Direct Connect (via DXGW Transit VIF), and other TGWs (via peering). It replaces complex full-mesh VPC peering with a scalable hub-and-spoke model.

**Scope:** Regional. TGW is created in one region. Cross-region: use TGW peering.

```mermaid
graph TD
    TGW["🔗 Transit Gateway\n(Regional Hub)"]
    
    TGW --- VPC1["🔷 VPC A\n10.1.0.0/16"]
    TGW --- VPC2["🔷 VPC B\n10.2.0.0/16"]
    TGW --- VPC3["🔷 VPC C\n10.3.0.0/16"]
    TGW --- VPN1["🔒 Site-to-Site VPN\n(On-Premises Branch 1)"]
    TGW --- VPN2["🔒 Site-to-Site VPN\n(On-Premises Branch 2)"]
    TGW --- DXGW["🌍 DX Gateway\n→ DX Connection"]
    TGW --- TGW2["🔗 TGW Peer\n(another region)"]
```

#### Attachment Types

| Attachment Type | Description | Notes |
|----------------|-------------|-------|
| **VPC** | Attach VPC subnets to TGW | One attachment per VPC, choose AZ subnets |
| **VPN** | Site-to-Site VPN terminates on TGW | Supports ECMP across tunnels |
| **Direct Connect (Transit VIF)** | Via DXGW → Transit VIF | Required path for DX to TGW |
| **TGW Peering** | Connect TGWs across regions | Static routes only (no BGP) |
| **TGW Connect** | GRE over VPC for SD-WAN | Higher throughput than VPN |

#### Route Tables

TGW uses **route tables** (not subnet route tables — TGW-internal route tables):

```
TGW Route Table (Default):
  10.1.0.0/16  →  VPC A attachment
  10.2.0.0/16  →  VPC B attachment
  0.0.0.0/0    →  VPN attachment (default route to on-premises)
```

- Each attachment can be **associated** with one route table (receives routes from it)
- Each attachment can **propagate** routes into one or more route tables
- **Isolated routing:** create separate route tables to segment VPCs (e.g., prod VPCs cannot reach dev VPCs)

```mermaid
graph LR
    subgraph "Prod Route Table"
        ProdVPC1["VPC-Prod-A"]
        ProdVPC2["VPC-Prod-B"]
        DXConn["DX Connection"]
    end
    subgraph "Dev Route Table"
        DevVPC1["VPC-Dev-A"]
        DevVPC2["VPC-Dev-B"]
    end
    subgraph "Shared Services RT"
        SharedVPC["VPC-Shared\n(DNS, AD)"]
        ProdVPC1
        ProdVPC2
        DevVPC1
        DevVPC2
    end
```

#### ECMP on TGW

TGW supports **Equal-Cost Multi-Path (ECMP)** routing over multiple VPN tunnels:
- 2 VPN connections × 2 tunnels each = 4 active tunnels = up to **5 Gbps** aggregate
- On-premises router must support ECMP and BGP multi-path

> 🔑 **Exam key:** VGW does NOT support ECMP. TGW supports ECMP. For > 1.25 Gbps VPN bandwidth → use TGW with ECMP.

#### TGW Connect

TGW Connect is a **GRE (Generic Routing Encapsulation) tunnel** attachment type over an existing VPC or DX attachment:
- Used for SD-WAN appliances (Cisco, VMware, Palo Alto)
- Higher throughput than IPSec VPN (up to 5 Gbps per Connect attachment)
- BGP is required (no static routing for Connect)
- Runs over existing VPC attachment (Transport attachment)

#### TGW Network Manager

- Centralized management console for global network topology
- Visualize TGW peering, VPN connections, DX connections across regions
- Integrates with SD-WAN (via Connect peers)
- CloudWatch metrics: bytes in/out per attachment, packet drops

#### TGW Sharing via RAM

TGW can be shared across AWS accounts using **AWS Resource Access Manager (RAM)**:
- One TGW in one account (network hub account) shared to all other accounts
- Each account attaches its VPCs to the shared TGW
- Centralized network team controls routing; app teams just attach VPCs

> 🔑 **Exam key:** TGW + RAM = organization-wide hub-and-spoke networking. Essential for AWS Organizations multi-account architectures.

#### Limitations

| Limit | Value |
|-------|-------|
| VPC attachments per TGW | 5,000 |
| VPN connections per TGW | 5,000 |
| TGW peering connections | 10 per TGW |
| Route tables per TGW | 20 |
| Routes per route table | 10,000 (static) / 20,000 (dynamic) |
| Bandwidth per VPC attachment | Up to 50 Gbps burst |
| Max VPN bandwidth (ECMP) | ~5 Gbps aggregate |
| TGW is regional | ✅ Yes — cross-region requires peering |
| BGP on peered TGW connections | ❌ No — peering uses static routes only |

---

### 📊 Side-by-Side Comparison Table

| Feature | VGW | DXGW | TGW |
|---------|-----|------|-----|
| **Scope** | Regional (per VPC) | Global | Regional |
| **VPCs supported** | 1 | Up to 10 (VGW) or hundreds (via TGW) | Thousands |
| **Multi-region** | ❌ No | ✅ Yes (native) | ❌ No (requires peering) |
| **VPC-to-VPC routing** | ❌ No | ❌ No | ✅ Yes |
| **VPN termination** | ✅ Yes | ❌ No | ✅ Yes |
| **DX termination** | Via Private VIF | Via Private/Transit VIF | Via Transit VIF + DXGW |
| **ECMP (VPN)** | ❌ No | ❌ No | ✅ Yes |
| **Transitive routing** | ❌ No | ❌ No | ✅ Yes |
| **Shareable via RAM** | ❌ No | ❌ No | ✅ Yes |
| **Route tables** | VPC subnet route tables | N/A (pass-through) | Own TGW route tables |
| **BGP support** | ✅ Yes | ✅ Yes (via VIFs) | ✅ Yes |
| **Static routes** | ✅ Yes | ❌ No (BGP only for VIFs) | ✅ Yes |
| **Cost** | Low (no attachment fee) | Low (DX port cost) | Higher (per-attachment + per-GB) |
| **Complexity** | Low | Medium | High |
| **SD-WAN/GRE** | ❌ No | ❌ No | ✅ TGW Connect |
| **Max VPN bandwidth** | 1.25 Gbps | N/A | ~5 Gbps (ECMP) |
| **Azure equivalent** | VPN Gateway / ExpressRoute GW | N/A (similar concept to ExpressRoute circuit peering) | Azure Virtual WAN |

---

### 🏗️ Architecture Patterns

#### Pattern 1: Single VPC + VGW (Simple / Small Scale)

```mermaid
graph LR
    OnPrem["🏢 On-Premises\n192.168.0.0/16"] -->|"IPSec VPN\nBGP"| VGW["🔒 VGW"]
    VGW --> VPC["🔷 Single VPC\n10.0.0.0/16"]
```

**Use when:** One VPC, simple connectivity, low cost, small team. Startup or dev environment. No multi-VPC routing needed.

**Breaks when:** More VPCs needed, need VPC-to-VPC from on-premises, need ECMP bandwidth.

---

#### Pattern 2: Multi-VPC + DXGW (Medium Scale, Multi-Region)

```mermaid
graph LR
    OnPrem["🏢 On-Premises"] --> DX["📍 DX Connection"]
    DX -->|"Private VIF"| DXGW["🌍 DX Gateway"]
    DXGW -->|VGW assoc| VPC1["🔷 VPC us-east-1"]
    DXGW -->|VGW assoc| VPC2["🔷 VPC eu-west-1"]
    DXGW -->|VGW assoc| VPC3["🔷 VPC ap-southeast-1"]
```

**Use when:** Multi-region VPC access from on-premises, no need for VPC-to-VPC routing, up to 10 VPCs, single DX connection.

**Breaks when:** Need VPC-to-VPC routing, more than 10 VPCs, need shared services VPC accessible by all.

---

#### Pattern 3: Multi-VPC + TGW + DXGW (Enterprise)

```mermaid
graph TD
    OnPrem["🏢 On-Premises"] --> DXLoc["📍 DX Location"]
    DXLoc -->|"Transit VIF"| DXGW["🌍 DX Gateway"]
    DXGW -->|TGW assoc| TGW["🔗 Transit Gateway\nus-east-1"]
    TGW --> VPC1["🔷 Prod VPC A"]
    TGW --> VPC2["🔷 Prod VPC B"]
    TGW --> VPC3["🔷 Dev VPC"]
    TGW --> SharedVPC["🔷 Shared Services\n(DNS, AD, NTP)"]
    TGW -->|ECMP VPN backup| VPN["🔒 VPN Backup\n(on-premises)"]
```

**Use when:** Many VPCs (10+), VPC-to-VPC routing needed, shared services, centralized inspection, multi-account via RAM, DX + VPN failover.

---

#### Pattern 4: TGW Peering Across Regions

```mermaid
graph LR
    TGW1["🔗 TGW us-east-1"] <-->|"TGW Peering\n(static routes)"| TGW2["🔗 TGW eu-west-1"]
    TGW1 --> VPC_US1["🔷 VPC US-A"]
    TGW1 --> VPC_US2["🔷 VPC US-B"]
    TGW2 --> VPC_EU1["🔷 VPC EU-A"]
    TGW2 --> VPC_EU2["🔷 VPC EU-B"]
    OnPrem["🏢 On-Premises"] -->|DX + DXGW| TGW1
```

**Use when:** Global multi-region architecture, need VPCs in different regions to communicate, on-premises connected to one region's TGW and needs access to another region's VPCs.

**Key constraint:** TGW peering uses **static routes only** — no BGP between peered TGWs.

---

### 💰 Cost Comparison

| Cost Component | VGW | DXGW | TGW |
|---------------|-----|------|-----|
| **Gateway hourly** | ~$0.05/hr | Free | ~$0.05/hr per TGW |
| **VPN connection** | ~$0.05/hr per connection | N/A | ~$0.05/hr per VPN attachment |
| **VPC attachment** | Free (VGW attaches to VPC free) | Free | **~$0.05/hr per VPC attachment** |
| **Data processing** | None | None | **$0.02/GB** (processed by TGW) |
| **DX port** | Not included | Not included | Not included |
| **Data transfer out** | Standard AWS DTO | Standard AWS DTO | Standard AWS DTO |

> ⚠️ **Cost trap:** TGW charges **per attachment per hour** AND **per GB processed**. A large number of VPC attachments + high traffic volume = significant cost. For low-traffic, few-VPC scenarios, DXGW + VGW may be cheaper.

> 🔑 **Exam tip:** "Cost-effective multi-VPC on-premises connectivity with < 10 VPCs, no VPC-to-VPC needed" → DXGW + Private VIFs (no TGW attachment fees). "Need VPC-to-VPC + on-premises + scalable" → TGW (accept higher cost for flexibility).

---

### 🔐 Security Considerations

| Aspect | VGW | DXGW | TGW |
|--------|-----|------|-----|
| **Encryption in transit** | ✅ IPSec (VPN) / ❌ None (DX Private VIF) | ❌ DX is private but not encrypted | ✅ IPSec (VPN) / ❌ None (DX) |
| **MACsec** | ❌ No | ✅ Supported on DX Dedicated (link-layer) | ❌ No (but upstream DX can have MACsec) |
| **Route filtering** | BGP route filters | BGP route filters per VIF | TGW route tables (fine-grained) |
| **VPC isolation** | Implicit (1 VPC per VGW) | No inter-VPC traffic | Route table isolation |
| **Centralized inspection** | ❌ No | ❌ No | ✅ Via GWLB attachment |
| **IAM control** | IAM policies | IAM policies | IAM policies + RAM sharing |
| **CloudTrail** | ✅ API calls logged | ✅ | ✅ |

**Centralized Inspection with TGW + GWLB:**
```mermaid
graph TD
    OnPrem["🏢 On-Premises"] -->|VPN/DX| TGW["🔗 TGW"]
    TGW -->|"inspect route"| InspectionVPC["🔍 Inspection VPC\n(Palo Alto / Fortinet)\n+ GWLB"]
    InspectionVPC -->|"allow"| TGW
    TGW --> AppVPC["🔷 Application VPC"]
```

---

### 🛡️ Reliability & Resiliency

| Aspect | VGW | DXGW | TGW |
|--------|-----|------|-----|
| **Built-in HA** | ✅ Two tunnels per VPN connection (active/passive) | ✅ Managed by AWS | ✅ Multi-AZ redundant |
| **Active/Active failover** | ❌ (passive failover only on VPN) | N/A | ✅ ECMP enables active/active |
| **DX resiliency** | Depends on DX setup | Depends on DX setup | Same |
| **VPN as DX backup** | ✅ Both attach to same VGW | ✅ DX primary, VPN to VGW backup | ✅ DX (via DXGW) + VPN both to TGW |
| **BGP failover** | BGP detects DX failure, falls back to VPN | BGP-driven | BGP + route priorities |

**DX + VPN Failover Pattern (TGW):**

```mermaid
graph LR
    OnPrem["🏢 On-Premises"]
    OnPrem -->|"DX (primary)\nhigh bandwidth, low latency"| DXGW["🌍 DXGW"]
    OnPrem -->|"VPN (backup)\nlower bandwidth, over internet"| VPN["🔒 VPN Attachment"]
    DXGW -->|Transit VIF| TGW["🔗 TGW"]
    VPN --> TGW
    TGW --> VPCs["🔷 VPCs"]
```

Use **BGP MED (Multi-Exit Discriminator)** or **AS path prepending** to prefer DX over VPN. When DX fails → BGP withdraws DX routes → VPN routes take over automatically.

---

### 🌍 Scope Summary

| Service | Scope | Cross-region? | Cross-account? |
|---------|-------|---------------|----------------|
| **VGW** | Regional (per VPC) | ❌ No | ❌ No |
| **DXGW** | **Global** | ✅ Yes (native) | ✅ Yes (via DX partner account) |
| **TGW** | Regional | ❌ No (peering required) | ✅ Yes (via RAM) |

---

### ☁️ Azure / Open-Source Counterparts

| AWS | Azure | Notes |
|-----|-------|-------|
| **VGW (VPN)** | Azure VPN Gateway | Both terminate IPSec/IKEv2 VPN; Azure supports active/active by default; AWS VGW is active/passive (both tunnels up but only one active) |
| **VGW (DX)** | Azure ExpressRoute Gateway | Both provide private dedicated connectivity endpoint to VNet/VPC |
| **DXGW** | ExpressRoute Global Reach | Both allow single circuit to reach resources in multiple regions; Azure Global Reach connects circuits between locations, DXGW connects one circuit to many VPCs |
| **TGW** | Azure Virtual WAN (vWAN) | Both are regional hub-and-spoke. vWAN is fully managed with auto-BGP; TGW gives more manual control over route tables. vWAN supports Azure Firewall natively in hub; TGW requires separate inspection VPC |
| **TGW Connect (GRE)** | Azure vWAN NVA Integration | Both support SD-WAN NVA integration |

**Linux/Open-Source:**
- **StrongSwan** — open-source IPSec daemon that plays the role of VGW on the on-premises side
- **FRRouting (FRR)** / **Quagga** — open-source BGP/OSPF routing, equivalent to on-premises BGP router connecting to VGW/TGW
- **Open vSwitch (OVS)** — conceptually similar to TGW routing (virtual switch with flow tables), used in OpenStack/KVM environments

---

### 📋 Well-Architected Framework Table

| Pillar | VGW | DXGW | TGW |
|--------|-----|------|-----|
| **Operational Excellence** | Simple to manage, single VPC focus. Route propagation automates routing. | Simplifies multi-region connectivity management. Use CloudWatch for VIF metrics. | Higher operational complexity. TGW Network Manager centralizes visibility. Infrastructure as Code (CloudFormation/Terraform) essential at scale. |
| **Security** | IPSec encryption for VPN. DX Private VIF: private but unencrypted (add MACsec or application-layer TLS). IAM for config changes. | DX: unencrypted at link layer (MACsec on dedicated connections). Limit VIF creation via IAM. VPC CIDR isolation enforced (no overlapping CIDRs). | Centralized inspection via GWLB attachment. Route table isolation between VPC groups. RAM sharing: central network team controls TGW; app teams only attach VPCs (least privilege). |
| **Reliability** | Two VPN tunnels per connection (active/passive). DX failover to VPN via BGP. Single AZ VPN endpoint risk mitigated by AWS redundancy. | Managed by AWS (HA built in). Resiliency depends on DX physical redundancy (High or Maximum Resiliency models). Multiple DX connections to same DXGW for HA. | Built-in multi-AZ. ECMP enables active/active VPN. DX primary + VPN secondary for full hybrid HA. TGW peering for cross-region DR. |
| **Performance Efficiency** | VPN: max 1.25 Gbps. DX: up to 100 Gbps (port speed). No ECMP = bandwidth ceiling. | DX: up to 100 Gbps. No traffic processing overhead. Dedicated link = predictable latency. | ECMP unlocks multi-Gbps VPN. TGW Connect (GRE): 5 Gbps per Connect attachment. VPC attachments: up to 50 Gbps burst. Right-size based on traffic patterns. |
| **Cost Optimization** | Lowest cost for single-VPC scenarios. No attachment fees. | No TGW attachment fees. Cost-effective for < 10 VPCs, multi-region access. Main cost: DX port. | Per-attachment + per-GB fees add up. Optimize: consolidate VPCs, minimize cross-attachment traffic. Right-size to avoid unused attachments. TGW Connect cheaper than many VPN connections. |
| **Sustainability** | Low overhead for small scale. Avoids unnecessary resource sprawl. | Reuse single DX connection across many VPCs (vs. separate connections per VPC). | Consolidates many peer connections into one hub (replaces N² peering with N spokes). Reduces operational waste from complex mesh topologies. |

---

### 🎓 4 Exam Domains

#### Domain 1: Design Solutions for Organizational Complexity

**TGW + RAM for multi-account networking:**
- Central networking account owns TGW
- Share TGW via RAM to all accounts in AWS Organization
- Each account's network team attaches VPCs to shared TGW
- Centralized routing policy (route tables) controlled by network team
- No need to manage full-mesh VPC peering across accounts

**DXGW for multi-region on-premises access:**
- Single DX connection → DXGW → VPCs in multiple regions
- No need for per-region DX connections
- Reduces cost: one DX port serves global VPC access

**Example scenario:** "Company has 50 AWS accounts across 3 regions, all needing on-premises connectivity and VPC-to-VPC routing."
**Answer:** TGW in each region (shared via RAM) + TGW peering between regions + DXGW with Transit VIF connecting to each TGW.

---

#### Domain 2: Design for New Solutions

**VGW for greenfield simple workloads:**
- New startup: one VPC, one VPN to on-premises → VGW (simplest, cheapest)
- Add DXGW when expanding to multiple regions without VPC-to-VPC needs

**TGW for enterprise greenfield:**
- New enterprise AWS deployment: plan for many VPCs from day 1
- TGW from the start avoids painful VGW → TGW migration later
- Design isolated route tables for prod/dev/shared upfront

**DXGW + TGW for hybrid greenfield:**
- New large enterprise: DX ordered, multiple VPCs planned
- Architecture: DX → DXGW → Transit VIF → TGW → VPCs
- VPN as backup on same TGW

---

#### Domain 3: Continuous Improvement for Existing Solutions

**VGW → TGW migration:**

1. Create TGW in the region
2. Create new VPN attachment on TGW (keep existing VGW VPN running)
3. Verify routing on TGW works
4. Switch on-premises routing to prefer TGW VPN routes (BGP MED/AS prepend)
5. Detach VGW from VPC after traffic validated
6. Enable route propagation on VPC route tables from TGW

> ⚠️ Migration window: both VGW and TGW VPNs run simultaneously during transition. Brief BGP reconvergence when switching.

**Adding centralized inspection to existing TGW:**
- Add Inspection VPC with GWLB
- Create new TGW route table for inspection
- Reroute spoke VPC traffic through inspection route table
- No downtime on existing VPCs (just route table changes)

---

#### Domain 4: Accelerate Workload Migration and Modernization

**Using TGW during migration:**
- On-premises workloads migrating to AWS VPCs
- TGW provides consistent connectivity throughout migration
- Phased approach: migrate VPC by VPC, each attaches to TGW
- On-premises still reachable via same VPN/DX during migration

**SD-WAN integration via TGW Connect:**
- Modernizing on-premises WAN with SD-WAN (Cisco Meraki, VMware SASE)
- TGW Connect (GRE) allows SD-WAN appliance in VPC to connect to TGW
- Higher throughput than IPSec VPN for branch office connectivity
- Enables cloud-managed WAN with AWS as hub

---

### ⚠️ Key Exam Traps

| Trap | Correct Understanding |
|------|-----------------------|
| "Use DXGW for VPC-to-VPC routing" | ❌ DXGW does NOT route VPC-to-VPC. Add TGW. |
| "VGW supports multiple VPCs" | ❌ VGW = 1 VPC only. |
| "TGW peering uses BGP" | ❌ TGW peering = static routes only. |
| "Transit VIF connects directly to TGW" | ❌ Transit VIF → DXGW → TGW (DXGW is required middle). |
| "VGW supports ECMP" | ❌ VGW VPN = no ECMP. TGW VPN = yes ECMP. |
| "DXGW is regional" | ❌ DXGW is **global** — reaches VPCs in any region. |
| "VPN on VGW and TGW are the same cost" | ❌ TGW VPN has additional per-attachment fee AND per-GB processing fee. |
| "TGW supports overlapping CIDRs" | ⚠️ Partially — overlapping VPC CIDRs on same TGW are allowed but must use specific static routes (dynamic propagation won't work cleanly). Avoid overlapping where possible. |
| "DXGW requires one DX connection per region" | ❌ DXGW is global — one DX connection can reach VPCs in all regions via DXGW. |

---

## 🔒 Chapter 17: Traffic Segmentation in AWS

### 📖 Overview

**Traffic segmentation** = dividing network traffic into isolated groups so different workloads, tenants, or security zones cannot reach each other unless explicitly permitted. It is the foundational principle of **defense in depth** in AWS networking.

Without segmentation, a single compromised workload can reach everything on the same network. Segmentation limits **blast radius** — the damage an attacker or misconfigured application can cause.

> 🔑 **Exam key:** Segmentation ≠ encryption. Segmentation = isolation (can't reach). Encryption = confidentiality (can't read). You need both. SAP-C02 frequently tests choosing the *right* segmentation tool for the right scope.

**Scope:** Spans multiple levels — VPC boundary, subnet, instance, TGW route table, firewall inline.

---

### 🔧 Segmentation Mechanisms — Layer by Layer

| Mechanism | OSI Level | Scope | Stateful? | Cost |
|-----------|-----------|-------|-----------|------|
| **VPC** | L3 | Account/Region boundary | N/A | Free |
| **Subnet** | L3 | Within VPC, per AZ | N/A | Free |
| **Security Group (SG)** | L4 | Per ENI (instance) | ✅ Yes | Free |
| **NACL** | L3-L4 | Per Subnet | ❌ No | Free |
| **VPC Peering** | L3 | Cross-VPC (no transitive) | N/A | Data transfer fee |
| **TGW Route Tables** | L3 | Multi-VPC hub | N/A | Per attachment + data |
| **AWS Network Firewall** | L3-L7 | VPC inline | ✅ Yes | Per endpoint + data |
| **Gateway Load Balancer (GWLB)** | L3 | Inline transparent | ✅ Yes | Per endpoint + data |
| **Resource Access Manager (RAM)** | Control plane | Cross-account | N/A | Free |

---

### 🔷 VPC as Segmentation Boundary

VPC is the **hardest isolation boundary** in AWS. Two VPCs have:
- Separate IP address spaces (CIDRs)
- No routing between them by default
- Separate security group namespaces
- Separate DNS namespaces

```mermaid
graph TD
    subgraph "VPC A — Production (10.0.0.0/16)"
        A1["EC2 Web Tier"]
        A2["RDS Database"]
    end
    subgraph "VPC B — Development (10.1.0.0/16)"
        B1["EC2 Dev"]
        B2["RDS Dev"]
    end
    subgraph "VPC C — Shared Services (10.2.0.0/16)"
        C1["Active Directory"]
        C2["Monitoring"]
    end
    A1 -. "NO route by default" .- B1
    C1 --> A1
    C1 --> B1
```

**When to use VPC-level segmentation:**
- Different environments (prod/dev/staging)
- Different teams with different security postures
- Regulatory isolation requirements (PCI, HIPAA)
- Different blast-radius domains

---

### 🔲 Subnet Segmentation

Subnets segment within a VPC — typically by **tier** (web/app/data) and **AZ**.

| Subnet Type | Route Table Has | Internet Reachable? | Typical Use |
|-------------|----------------|---------------------|-------------|
| **Public** | IGW route | ✅ Yes (with EIP/Public IP) | ALB, NAT GW, Bastion |
| **Private** | NAT GW route | ❌ Inbound no, outbound yes | App servers, EKS nodes |
| **Isolated** | No internet route | ❌ No | Databases, internal services |

> 🔑 **Exam key:** Placing an RDS instance in an *isolated* subnet (no NAT GW) + Security Group restricting access to app subnet = correct layered segmentation. "Private" subnet still has outbound internet via NAT.

---

### 🛡️ Security Groups — Instance-Level Segmentation

Security Groups are **stateful allow-lists** attached to ENIs. Key properties:
- No explicit deny (only allow rules)
- Stateful: return traffic automatically allowed
- Can reference other SGs (not just CIDRs) — **cross-account SG referencing supported via VPC peering/TGW**
- Default SG: allows all inbound from same SG, all outbound

**Tiered SG pattern:**

```
ALB-SG  → allows 443 from 0.0.0.0/0
App-SG  → allows 8080 from ALB-SG only
DB-SG   → allows 5432 from App-SG only
```

This creates a **chain of trust** — DB is only reachable from app layer, app only from ALB.

> ⚠️ **Limitation:** SG rules evaluated as "any match = allow." No ordering. Cannot create deny rules. Use NACL for explicit denies.

---

### 🚫 NACLs — Subnet-Level Stateless Segmentation

NACLs operate at the **subnet boundary**, evaluate rules in order (lowest number first), and are **stateless** (must allow both inbound AND outbound explicitly).

**Use NACLs for:**
- Explicit IP block lists (deny specific CIDRs)
- Emergency lockdown (block a compromised IP range)
- Compliance requirements for subnet-level controls

**Limitation:** NACLs cannot reference Security Groups — CIDR-only. Not suitable for dynamic workloads with changing IPs.

---

### 🔀 TGW Route Tables — Multi-VPC Segmentation at Scale

Transit Gateway route tables are the **enterprise-scale segmentation mechanism**. Each TGW attachment (VPC, VPN, DX, peering) is associated with one route table and can propagate into others.

```mermaid
graph LR
    subgraph "TGW"
        RT_PROD["Route Table: PROD\nPropagates: Prod VPCs + On-Prem\nNo Dev routes"]
        RT_DEV["Route Table: DEV\nPropagates: Dev VPCs + Internet\nNo Prod routes"]
        RT_SHARED["Route Table: SHARED\nPropagates: All VPCs\nRead-only shared services"]
        RT_ONPREM["Route Table: ON-PREM\nPropagates: All VPCs\nFor DX/VPN attachment"]
    end
    PROD_VPC["Prod VPC"] --> RT_PROD
    DEV_VPC["Dev VPC"] --> RT_DEV
    SHARED_VPC["Shared Services VPC"] --> RT_SHARED
    DX["DX / VPN"] --> RT_ONPREM
```

**Key patterns:**
- **Flat (default):** all VPCs in one route table — full mesh
- **Segmented:** separate route tables per zone — no lateral movement
- **Centralized egress:** only Egress VPC has internet route; all others send default route to TGW
- **Centralized inspection:** all inter-VPC traffic routed through Inspection VPC (Network Firewall / 3rd-party NVA)

---

### 🔍 Centralized Inspection Pattern

Most advanced segmentation: force **all east-west and north-south traffic** through an inspection VPC.

```mermaid
flowchart TD
    SPOKE_A["Spoke VPC A\n(App)"] -->|"via TGW"| INSP["Inspection VPC\n(AWS Network Firewall\nor GWLB + 3rd-party)"]
    SPOKE_B["Spoke VPC B\n(Data)"] -->|"via TGW"| INSP
    INSP -->|"allowed traffic"| SPOKE_B
    INSP -->|"north-south"| IGW["Internet Gateway"]
    INSP -->|"blocked"| DROP["🚫 Drop"]
```

Components:
- **AWS Network Firewall** — stateful L3-L7 inspection, Suricata rules, domain filtering
- **Gateway Load Balancer (GWLB)** — transparent bump-in-the-wire for 3rd-party NVAs (Palo Alto, Fortinet, Check Point)
- **TGW with appliance mode** — keeps flows symmetric (required for stateful inspection)

> 🔑 **Exam key:** TGW **appliance mode** must be enabled on the inspection VPC attachment. Without it, return traffic may take a different AZ path, breaking stateful inspection.

---

### 📊 Segmentation Mechanisms — Full Comparison

| Requirement | Best Tool | Why |
|-------------|-----------|-----|
| Isolate prod from dev entirely | Separate VPCs | Hardest boundary |
| Isolate DB tier from web tier | Subnets + SG | Within-VPC tiering |
| Block specific malicious IP | NACL | Only tool with explicit deny |
| Multi-account VPC isolation with selective connectivity | TGW + Route Tables | Hub-and-spoke segmentation |
| Inspect all traffic L7 | Network Firewall + TGW | Deep packet inspection |
| Transparent 3rd-party NVA | GWLB | Bump-in-the-wire without changing routes |
| Regulatory zone isolation (PCI, HIPAA) | Separate VPCs + separate AWS accounts | Account boundary = strongest isolation |
| Micro-segmentation per workload | Security Groups (SG-to-SG rules) | ENI-level stateful |

---

### 🌍 Scope

| Resource | Scope |
|----------|-------|
| VPC | Regional |
| Subnet | AZ |
| Security Group | VPC-regional |
| NACL | Subnet (AZ) |
| TGW | Regional (peering for cross-region) |
| AWS Network Firewall | AZ (endpoint per AZ) |
| GWLB | Regional (endpoint per AZ) |

---

### ☁️ Azure / On-Premises Counterparts

| AWS | Azure | On-Premises |
|-----|-------|-------------|
| VPC | VNet | VLAN / VRF |
| Security Group | NSG (Network Security Group) | Firewall ACL |
| NACL | NSG subnet-level | ACL on L3 switch |
| TGW Route Tables | Azure Virtual WAN + route tables | MPLS VRF segmentation |
| Network Firewall | Azure Firewall | Palo Alto / Fortinet NGFW |
| GWLB | Azure Gateway Load Balancer | Transparent firewall cluster |

---

### 🔐 Security Considerations

- **Least-privilege by default:** new VPCs have no routes to each other. Must explicitly enable connectivity.
- **SG over NACL for workload security:** SGs are instance-aware, stateful, and support SG references. NACLs are a blunt subnet tool.
- **Log everything:** VPC Flow Logs capture accepted and rejected traffic. Essential for forensics and compliance.
- **Avoid 0.0.0.0/0 inbound on SG:** except on public-facing load balancers. App/DB layers must restrict source.
- **TGW route tables over VPC peering mesh:** at >5 VPCs, full-mesh peering becomes unmanageable (N×(N-1)/2 connections). TGW scales linearly.
- **Appliance mode for stateful NVAs:** without it, asymmetric routing breaks session state.

---

### 🛡️ Reliability & Resiliency

- **Multi-AZ Network Firewall endpoints:** deploy one endpoint per AZ, route AZ traffic to same-AZ endpoint (avoid cross-AZ traffic for stateful inspection)
- **GWLB endpoint per AZ:** same principle — keep traffic local to AZ for HA and latency
- **TGW is regional HA:** spans all AZs in region automatically — no per-AZ configuration needed
- **Security Group changes are instant:** no downtime. NACL changes are instant. Both are non-disruptive.
- **Avoid dependency on single inspection VPC AZ:** GWLB + multi-AZ NVA fleet for true HA

---

### 📋 Well-Architected Framework

| Pillar | Traffic Segmentation Consideration |
|--------|-------------------------------------|
| **Operational Excellence** | Use AWS Firewall Manager to centrally manage SGs, NACLs, Network Firewall policies across accounts. Automate policy deployment via CloudFormation / Terraform. |
| **Security** | Apply defense-in-depth: VPC boundary → subnet → SG → NACL → inline inspection. Never rely on a single control. Log with VPC Flow Logs + Firewall logs → Security Lake. |
| **Reliability** | Deploy Network Firewall / GWLB endpoints in every AZ. Enable TGW appliance mode for stateful inspection. Multi-AZ NVA fleet behind GWLB eliminates single AZ dependency. |
| **Performance Efficiency** | GWLB scales automatically to NVA fleet (no manual scaling). Network Firewall is fully managed — no capacity planning. Avoid cross-AZ inspection traffic (latency + cost). |
| **Cost Optimization** | Centralized inspection VPC amortizes NVA cost across all spoke VPCs. GWLB endpoint fee only in AZs you use. TGW attachment fee per VPC — evaluate if VPC peering is cheaper for small topologies. |
| **Sustainability** | Centralized NVAs use fewer instances than per-VPC appliances. Right-size NVA fleet with GWLB auto scaling — no over-provisioning. |

---

### 🎓 4 SAP-C02 Exam Domains

#### 1. Design Solutions for Organizational Complexity
- Multi-account segmentation: use **AWS Organizations + SCPs** to prevent cross-account VPC peering outside approved paths
- **AWS Firewall Manager** centrally enforces SG and Network Firewall policies across all accounts in an Org
- **TGW shared via RAM** across accounts — segmentation maintained via route tables per account/OU

#### 2. Design for New Solutions
- New app with PCI scope: place in **separate VPC + separate AWS account** for hard isolation
- Micro-segmentation: SG-per-workload referencing pattern
- New multi-tier app: public subnet (ALB) → private subnet (app) → isolated subnet (DB) with chained SGs

#### 3. Continuous Improvement for Existing Solutions
- Migrating from flat VPC (all SGs allow VPC CIDR) → tiered SG model
- Migrating from VPC peering mesh → TGW with segmented route tables
- Adding centralized inspection to existing hub-and-spoke TGW (insert Inspection VPC, update route tables)

#### 4. Accelerate Workload Migration and Modernization
- Lift-and-shift: map on-premises VLANs/VRFs to AWS subnets/VPCs
- Modernization: replace hardware firewalls with Network Firewall + GWLB
- SD-WAN integration: TGW Connect (GRE) for SD-WAN vendors → segmentation maintained in TGW route tables

---

### ⚠️ Key Exam Traps

| Common Wrong Answer | Correct Understanding |
|--------------------|-----------------------|
| "Use NACL to block traffic from another SG" | NACLs work on CIDRs only, not SG references |
| "Security Groups are stateless" | SGs are **stateful**. NACLs are stateless. |
| "VPC peering enables transitive routing" | VPC peering is **non-transitive**. A↔B, B↔C does NOT allow A↔C |
| "TGW route tables are optional — TGW is plug-and-play" | Default route table exists but flat — segmentation requires custom route tables |
| "Appliance mode is needed for all TGW attachments" | Only needed on the **inspection/appliance VPC** attachment |
| "GWLB changes application routing" | GWLB is **transparent** — no IP change, uses GENEVE encapsulation |
| "Network Firewall is global" | Network Firewall is **regional**, endpoint is **AZ-level** |

---

## 🧪 Test Your Knowledge

> 100+ Professional-level questions per topic.  
> Format: `[ DOMAIN ] Question ; Answer`  
> Followed by explanation (each sentence starts with `*` and ends with `<br>`)

<!-- Questions added after each chapter -->





---

## 🗄️ Chapter 8: Amazon Aurora

### 📖 Overview

**Amazon Aurora** is AWS's cloud-native relational database engine — a fully managed, MySQL and PostgreSQL-compatible RDBMS designed from the ground up to exploit distributed cloud storage. It delivers up to 5× the throughput of standard MySQL and 3× of PostgreSQL on the same hardware, while being fully compatible with those engines at the wire-protocol level.

> 🔑 **Exam key:** Aurora is NOT a separate engine — it is MySQL or PostgreSQL with a reengineered storage layer. Applications connecting to Aurora use the same drivers, same SQL, same tools as vanilla MySQL/PostgreSQL.

**Real-life analogy:** Think of Aurora as MySQL/PostgreSQL that had its storage layer swapped from a single-server disk to a distributed SAN spread across 3 AZs — the query engine stayed the same, but the storage became massively parallel and self-healing.

**Scope:** **Regional** service. Aurora Global Database extends to up to 5 secondary regions.

---

### 🏗️ Architecture

Aurora's core innovation is the **separation of compute from storage**. The storage layer is a **shared distributed volume** replicated 6 ways across 3 AZs — the DB instances are just compute heads reading/writing to this shared volume.

```mermaid
graph TB
    subgraph "Aurora Cluster"
        W["✏️ Writer Instance\n(Primary)"]
        R1["📖 Reader Instance 1\n(AZ-b)"]
        R2["📖 Reader Instance 2\n(AZ-c)"]
    end
    subgraph "Aurora Storage (auto-distributed)"
        S1["💾 Storage Node AZ-a\n(2 copies)"]
        S2["💾 Storage Node AZ-b\n(2 copies)"]
        S3["💾 Storage Node AZ-c\n(2 copies)"]
    end
    W -->|writes| S1
    W -->|writes| S2
    W -->|writes| S3
    R1 -->|reads| S1
    R1 -->|reads| S2
    R2 -->|reads| S2
    R2 -->|reads| S3
    CE["🌐 Cluster Endpoint\n(write)"] --> W
    RE["🌐 Reader Endpoint\n(read LB)"] --> R1
    RE --> R2
```

**Key storage facts:**
- **6 copies** of data across 3 AZs (2 per AZ)
- Writes acknowledged after **4/6 quorum**
- Reads use **3/6 quorum**
- Can tolerate loss of 2 copies for writes, 3 copies for reads
- Storage auto-grows in **10 GB increments** up to **128 TiB** (no pre-provisioning needed)
- **No I/O operations visible to the compute layer** — storage handles its own replication

---

### 🔧 Variants & Editions

| Variant | Engine Compatibility | Compute Model | Use Case |
|---------|---------------------|---------------|----------|
| **Aurora MySQL** | MySQL 5.7 / 8.0 | Provisioned instances | MySQL workloads |
| **Aurora PostgreSQL** | PostgreSQL 13–16 | Provisioned instances | PostgreSQL workloads |
| **Aurora Serverless v1** | MySQL 5.6/5.7, PG 10/11 | ACU auto-scales | Intermittent/unpredictable load |
| **Aurora Serverless v2** | MySQL 8.0, PG 13–16 | Fine-grained ACU scaling | Variable load, dev/test, prod |
| **Aurora Global Database** | MySQL or PostgreSQL | Multi-region | DR, global reads |
| **Aurora Multi-Master** | MySQL 5.6 only | Multiple writers | Write HA (deprecated, limited) |
| **Babelfish for Aurora PostgreSQL** | T-SQL wire protocol | Provisioned | SQL Server migration |

> 🔑 **Exam key:** Serverless v2 scales in fine-grained increments (0.5 ACU steps) while v1 had coarse doubling steps and a cold-start gap. Serverless v2 is the current recommendation.

---

### 🔗 Endpoints

| Endpoint Type | Points To | Use Case |
|--------------|-----------|----------|
| **Cluster Endpoint** | Current writer instance | All writes |
| **Reader Endpoint** | Load-balanced across all readers | Read scaling |
| **Instance Endpoint** | Specific instance | Diagnostics, pinned connections |
| **Custom Endpoint** | User-defined subset of instances | Tiered reader pools (large vs small) |

---

### 🔒 Connectivity & Security

- Deployed inside a **VPC** — you choose subnets via a **DB Subnet Group** (must span ≥2 AZs)
- Access controlled by **Security Groups** (not NACLs — SGs are stateful, sufficient)
- **No public internet access by default** — set `Publicly Accessible = true` only for dev/test
- **SSL/TLS in transit:** enforced via `require_secure_transport` parameter (MySQL) or `ssl_mode` (PG); Aurora issues its own TLS cert; download the AWS RDS CA bundle
- **IAM Database Authentication:** passwords replaced by short-lived IAM tokens (15-min expiry); uses `AmazonRDSIAMAuth` policy; no password management needed

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Key Management |
|-------|-----------|----------------|
| **At-rest (storage)** | AES-256, AWS KMS | AWS-managed key (`aws/rds`) or CMK |
| **At-rest (snapshots)** | Same KMS key as source cluster | Snapshots inherit cluster key |
| **At-rest (automated backups)** | Same KMS key | Cannot change key after creation |
| **In-transit** | SSL/TLS 1.2+ | AWS-managed cert (rotate with `rds-ca-2019`) |
| **Cross-account snapshot share** | Re-encrypt with target account CMK | Must share CMK with target account |

> ⚠️ **Encryption cannot be enabled on an existing unencrypted Aurora cluster.** Workaround: take unencrypted snapshot → copy snapshot with encryption enabled → restore from encrypted snapshot. This is a one-time migration.

> 🔑 **Exam key:** Encryption must be enabled at cluster creation. Once encrypted, all replicas, snapshots, and restored clusters are also encrypted with the same key.

---

### 📈 Scalability & HA

**Compute scaling:**
- **Vertical:** Change instance class (requires brief failover for writer change)
- **Horizontal read scaling:** Add up to **15 Aurora Replicas** (vs 5 for RDS)
- **Aurora Serverless v2:** Scales from 0.5 to 128 ACUs in <1 second; writer AND readers scale independently

**Storage scaling:**
- Automatic — grows in 10 GB steps up to 128 TiB; no manual intervention; storage never shrinks (you pay for high-water-mark)

**Connection scaling:**
- **RDS Proxy** in front of Aurora: pools connections, reduces connection overhead for Lambda-heavy workloads; maintains a persistent pool to Aurora; failover-transparent to applications

---

### 🏢 Multi-AZ

Aurora Multi-AZ is **inherent and automatic** — the storage layer always maintains 6 copies across 3 AZs regardless of whether you have reader instances. This is architecturally different from RDS Multi-AZ.

| Aspect | Aurora | RDS Multi-AZ |
|--------|--------|--------------|
| **Storage replication** | Always 6 copies / 3 AZs (built-in) | Synchronous to standby (1 copy) |
| **Standby serves reads?** | Yes (Aurora Replicas = read traffic) | No (standby is passive) |
| **Failover time** | ~30 seconds (promotes existing replica) | ~60-120 seconds (standby promotion) |
| **Failover mechanism** | DNS CNAME flip to existing reader | DNS flip to standby |
| **Cost of HA** | Zero storage overhead (built-in) | ~2× storage cost |

> 🔑 **Exam key:** Aurora always has 3-AZ storage even with a single instance. To also get compute HA, add at least one Aurora Replica in a different AZ — failover promotes it in ~30 seconds.

---

### 📖 Read Replicas

| Feature | Aurora Replica | RDS Read Replica |
|---------|---------------|-----------------|
| **Max count** | 15 | 5 (5 per source, up to 5 chains) |
| **Replication lag** | Milliseconds (shared storage) | Seconds–minutes (log shipping) |
| **Promotion to primary** | ~30 sec (no data copy needed) | Minutes (must catch up) |
| **Cross-region** | Via Aurora Global Database | Native cross-region replicas |
| **Read traffic routing** | Automatic via Reader Endpoint | Manual per-endpoint |
| **Cost of replica** | Instance cost only (shared storage) | Instance + storage cost |

> 🔑 **Exam key:** Aurora Replicas share the same storage volume as the writer — replication lag is near-zero because there's no log-shipping, just shared reads. This enables sub-30-second failovers.

---

### 🌍 Aurora Global Database

A **Global Database** spans up to **5 AWS Regions** (1 primary + 4 secondary):

```mermaid
graph LR
    P["🌎 Primary Region\nus-east-1\n(Writer + Readers)"]
    S1["🌍 Secondary Region\neu-west-1\n(Read-only Replicas)"]
    S2["🌏 Secondary Region\nap-southeast-1\n(Read-only Replicas)"]
    P -->|"< 1 sec replication\n(storage-level)"| S1
    P -->|"< 1 sec replication"| S2
```

| Feature | Value |
|---------|-------|
| Replication lag | Typically < 1 second |
| Secondary regions | Up to 4 |
| Reader instances per secondary | Up to 16 |
| Managed Planned Failover (RPO) | ~0 (synchronous before switch) |
| Unmanaged Failover (RTO) | < 1 minute |
| Use case | Global low-latency reads, cross-region DR |

> 🔑 **Exam key:** Global Database replication is **storage-level**, not log-based — it does NOT go through the database engine. This is why lag is <1 second and failover is fast. DynamoDB Global Tables is active-active; Aurora Global Database is active-passive (writes only in primary region).

---

### ⏱️ RPO / RTO Table

| Deployment | RPO | RTO | Notes |
|-----------|-----|-----|-------|
| Single instance, no replica | Hours (last backup) | Hours (restore from backup) | Not HA — dev/test only |
| Single instance + automated backup | 5 min (PITR) | 20-40 min (restore) | Backup-only recovery |
| Multi-AZ (replica in another AZ) | ~0 (shared storage) | ~30 sec (replica promotion) | Standard HA |
| Aurora Global Database (planned failover) | ~0 | ~1 min | Best RTO for regional failure |
| Aurora Global Database (unmanaged failover) | < 1 sec | < 1 min | Regional disaster |
| Serverless v2 | ~0 (if replicas) | ~30 sec | Same as provisioned with replicas |

---

### 💾 Backup & Restore

- **Automated backups:** Enabled by default; retention 1–35 days; stored in S3 (not visible to you); continuous backup to S3 (not snapshot-based)
- **Point-In-Time Recovery (PITR):** Restore to any second within retention window; creates a **new cluster** (not in-place restore)
- **Manual snapshots:** Retained until explicitly deleted; survive cluster deletion; can share cross-account; can copy cross-region
- **Backtrack:** Aurora MySQL only — rewind the cluster to a previous point **without restoring** (in-place, near-instant); backtrack window up to 72 hours; does NOT replace PITR; causes brief cluster pause
- **Database cloning:** Create a full copy of Aurora cluster using copy-on-write — near-instant, cost-efficient (only changed data stored separately); great for test environments

> 🔑 **Exam key:** Backtrack ≠ PITR. Backtrack is in-place rewind (Aurora MySQL only, brief outage). PITR creates a new cluster. Cloning uses copy-on-write — no data duplication cost.

---

### 🔄 Migration

#### From MySQL/PostgreSQL on-premises or RDS → Aurora

| Method | When to Use | Downtime |
|--------|-------------|----------|
| **mysqldump / pg_dump + restore** | Small DBs, simple schema | Yes — full downtime |
| **AWS DMS (homogeneous)** | Any size, minimal downtime | Minimal (CDC replication) |
| **RDS Snapshot → Aurora** | RDS MySQL/PG → Aurora (same engine) | Brief (restore time) |
| **Aurora native import** | From MySQL bin log | Minimal |
| **AWS SCT** | Schema conversion (heterogeneous) | Pre-migration step only |

#### AWS DMS for Aurora

- **Full Load + CDC:** DMS migrates existing data, then streams ongoing changes until cutover
- **Aurora as source:** Supported; DMS reads binary log (MySQL) or logical replication (PG)
- **Aurora as target:** Supported; DMS writes via standard MySQL/PG wire protocol
- **Homogeneous migration:** MySQL→Aurora MySQL — DMS handles without SCT (same engine family)
- **Heterogeneous migration:** Oracle/SQL Server→Aurora — requires **AWS Schema Conversion Tool (SCT)** first to convert schema and stored procedures, then DMS for data

> 🔑 **Exam key:** SCT converts the **schema and application code** (stored procs, functions, triggers). DMS moves the **data**. For Oracle→Aurora PostgreSQL: SCT first, DMS second.

---

### 🔗 Integration with Other AWS Services

| Service | Integration | Purpose |
|---------|-------------|---------|
| **RDS Proxy** | Sits in front of Aurora | Connection pooling, failover transparency |
| **Lambda** | Data API (HTTP) or direct VPC connection | Serverless → Aurora access |
| **S3** | `LOAD DATA FROM S3` / `SELECT INTO OUTFILE S3` | Bulk load/export |
| **IAM** | IAM DB authentication | Password-free access |
| **Secrets Manager** | Auto-rotation of master password | Credential management |
| **CloudWatch** | Metrics, Enhanced Monitoring, Performance Insights | Observability |
| **EventBridge** | RDS Events (failover, backup, etc.) | Automated responses |
| **CloudTrail** | API call audit | Governance |
| **AWS Backup** | Policy-driven backup | Cross-account, cross-region copies |
| **Kinesis Data Streams** | Aurora activity streams | Near-real-time DB audit |

---

### ⚖️ Comparison Table

| Feature | Aurora | RDS MySQL/PG | Oracle RDS | Self-managed MySQL | Azure SQL |
|---------|--------|-------------|------------|-------------------|-----------|
| Storage | Distributed, auto-grow | EBS, manual sizing | EBS, manual | Local disk | Managed |
| Max storage | 128 TiB | 64 TiB | 64 TiB | OS limit | 4 TiB (General Purpose) |
| Read replicas | 15 | 5 | 5 (Active Data Guard) | Manual | ~4 (Business Critical) |
| Replication lag | ms | Seconds | Seconds | Variable | ms (Business Critical) |
| Serverless | Yes (v2) | No | No | No | Yes (Serverless v2) |
| Multi-region | Global Database | Cross-region replica | Cross-region replica | Manual | Auto-failover groups |
| Backtrack | MySQL only | No | No | No | No |
| Wire-protocol compat | MySQL/PG | MySQL/PG | Oracle | MySQL | T-SQL |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max storage | 128 TiB |
| Max Aurora Replicas per cluster | 15 |
| Max Global Database secondary regions | 4 |
| Backtrack window | 72 hours (Aurora MySQL only) |
| Serverless v2 min ACU | 0.5 ACU |
| Serverless v2 max ACU | 128 ACU |
| Max DB instance classes | r6g, r7g, r8g, x2g families |
| Parameter group changes | Dynamic (no reboot) or static (requires reboot) |
| Encryption | Cannot enable on existing unencrypted cluster |
| Multi-Master | MySQL 5.6 only, being retired |

---

### 🏛️ Well-Architected Framework

| Pillar | Aurora Considerations |
|--------|-----------------------|
| **Operational Excellence** | Use Parameter Groups for config-as-code; enable Enhanced Monitoring + Performance Insights; use Aurora Events + EventBridge for automated ops; tag clusters for cost allocation |
| **Security** | Always encrypt at rest with CMK; enforce SSL/TLS in transit; use IAM DB Authentication; deploy in private subnets; use Secrets Manager for credential rotation; enable Activity Streams for audit |
| **Reliability** | Deploy at least one Aurora Replica in a different AZ for compute HA; use Aurora Global Database for regional DR; use RDS Proxy for connection resilience; test failover with `failover-db-cluster` API |
| **Performance Efficiency** | Use Aurora Serverless v2 for variable workloads; use Reader Endpoint for read scaling; use custom endpoints to separate OLTP from analytics replicas; use Performance Insights to identify bottlenecks |
| **Cost Optimization** | Aurora Serverless v2 for dev/test (scale to zero); use Aurora I/O-Optimized for high I/O workloads (predictable pricing); stop/start dev clusters; use reserved instances for predictable prod workloads |
| **Sustainability** | Serverless v2 scales to minimum when idle; shared storage avoids per-replica storage duplication; Graviton (r6g, r7g) instances offer better performance per watt |

---

### 📐 4 Exam Domains

**Domain 1 — Design Solutions for Organizational Complexity**
Aurora supports multi-account patterns via RAM (share Aurora clusters), cross-account snapshot sharing, and Aurora Global Database for multi-region org deployments. Use SCPs to enforce encryption requirements across org.

**Domain 2 — Design for New Solutions**
For new cloud-native applications: Aurora Serverless v2 for variable/unknown load. Aurora PostgreSQL with IAM auth + Secrets Manager for zero-credential-management. Aurora Global Database for global applications requiring <1 second cross-region read lag.

**Domain 3 — Continuous Improvement for Existing Solutions**
Migrate from RDS to Aurora for better read scaling (15 replicas vs 5), faster failover (30s vs 120s), and automatic storage growth. Enable Performance Insights to identify slow queries. Use Backtrack to recover from accidental deletes without full cluster restore.

**Domain 4 — Accelerate Workload Migration and Modernization**
Oracle/SQL Server → Aurora via SCT + DMS. Use Babelfish for Aurora PostgreSQL to run T-SQL workloads with minimal application changes. MySQL/PG RDS → Aurora via snapshot restore (minimal downtime).


---

## 🐘 Chapter 9: Amazon RDS

### 📖 Overview

**Amazon Relational Database Service (RDS)** is AWS's managed relational database service that handles provisioning, patching, backups, and failover for traditional RDBMS engines. Unlike Aurora (which re-engineered the storage layer), RDS runs the **standard, unmodified community or vendor engines** on managed EC2 + EBS infrastructure.

> 🔑 **Exam key:** RDS = managed lift-and-shift for existing relational engines. Aurora = cloud-native reengineered MySQL/PG. Choose RDS when you need a specific engine version, Oracle licensing, SQL Server features, or a non-Aurora engine.

**Scope:** **Regional** — DB instances are AZ-specific; Multi-AZ standby is in a different AZ of the same region.

**Supported engines:**
- Amazon Aurora (MySQL / PostgreSQL) — separate chapter
- MySQL (5.7, 8.0)
- PostgreSQL (12–16)
- MariaDB (10.4–10.11)
- Oracle (19c, 21c — BYOL or License Included)
- Microsoft SQL Server (2016–2022 — License Included or BYOL)

---

### 🏗️ Architecture

```mermaid
graph TB
    subgraph "RDS Multi-AZ Deployment"
        P["🖥️ Primary Instance\nAZ-a (EC2 + EBS)"]
        S["🖥️ Standby Instance\nAZ-b (EC2 + EBS)"]
        P -->|"Synchronous replication\n(block-level)"| S
    end
    subgraph "Read Replicas (optional)"
        R1["📖 Read Replica 1\nAZ-b or AZ-c"]
        R2["📖 Read Replica 2\nCross-region"]
    end
    P -->|"Async replication\n(engine log-based)"| R1
    P -->|"Async replication"| R2
    App["🌐 Application"] -->|"Write"| PE["Primary Endpoint"]
    App -->|"Read"| RE["Replica Endpoint"]
    PE --> P
    RE --> R1
```

**Key components:**
- **DB Instance:** EC2 compute running the DB engine
- **EBS Storage:** gp2, gp3, io1, io2 — you choose and manage size
- **DB Subnet Group:** spans ≥2 AZs; RDS places primary in one AZ, standby in another
- **Multi-AZ Standby:** synchronous replication; not readable; automatic failover
- **Read Replicas:** async replication; readable; manual promotion

---

### 🗂️ Storage Types

| Storage Type | IOPS | Use Case | Notes |
|-------------|------|----------|-------|
| **gp2** | 3 IOPS/GB baseline, burst to 3000 | General | Legacy; prefer gp3 |
| **gp3** | 3000 IOPS baseline, up to 16000 | General | Decouple IOPS from size |
| **io1** | Up to 40,000 IOPS | High-perf OLTP | More expensive |
| **io2** | Up to 64,000 IOPS, higher durability | Mission-critical | Best durability (99.999%) |
| **Magnetic** | Low | Legacy only | Do not use |

> 🔑 **Exam key:** gp3 allows you to provision IOPS independently of storage size — more cost-efficient than gp2 for large DBs that don't need proportional IOPS. For >40,000 IOPS needs, use Aurora or switch to io2.

---

### 🔒 Connectivity & Security

- Must be in a **VPC** with a **DB Subnet Group** (≥2 AZs)
- **Security Groups** control access — no public access by default
- **SSL/TLS enforced** via parameter `rds.force_ssl=1` (PG) or `require_secure_transport=ON` (MySQL)
- **IAM DB Authentication:** supported for MySQL and PostgreSQL; token valid 15 minutes
- **Kerberos authentication:** supported for Oracle and SQL Server (via AWS Managed Microsoft AD)
- **VPC Peering / PrivateLink:** access from other VPCs; RDS doesn't support PrivateLink directly — use RDS Proxy or an NLB in front

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **At-rest** | AES-256 via KMS | Must enable at creation; cannot add later |
| **Snapshots** | Inherit source DB key | Can re-encrypt on copy |
| **Automated backups** | Same key as DB | |
| **Read replicas** | Must also be encrypted if source is | Cross-region replica: needs CMK in target region |
| **In-transit** | SSL/TLS | Download RDS CA cert from AWS |
| **Oracle TDE** | Native Oracle TDE supported | Double-encryption possible (TDE + RDS encryption) |
| **SQL Server TDE** | Native SQL Server TDE supported | |

> ⚠️ **Cannot encrypt an existing unencrypted RDS instance directly.** Workaround: snapshot → copy with encryption → restore.

---

### 📈 Scalability & HA

**Vertical scaling:** Change DB instance class — requires brief downtime (1-2 min for Multi-AZ: failover to standby first, upgrade standby, failover back).

**Storage scaling:** Auto-scaling enabled via `Max Allocated Storage` parameter — RDS expands automatically when free space < 10% AND free space < 5 GiB AND low storage lasts >5 minutes.

**Read scaling:** Add Read Replicas (up to 5 per source for MySQL/PG/MariaDB).

**Connection scaling:** **RDS Proxy** — pools and manages connections; reduces connection churn; IAM auth; automatic failover; supported for MySQL, PG, SQL Server.

---

### 🏢 Multi-AZ

**Multi-AZ DB Instance (classic):** One standby in a different AZ — synchronous block-level replication. Standby is **not readable** — purely for HA.

**Multi-AZ DB Cluster (new, 2022):** Two readable standby instances across 3 AZs — synchronous replication. Standbys CAN serve reads. Faster failover (~35 sec). Available for MySQL 8.0 and PostgreSQL 13+.

| Feature | Multi-AZ Instance | Multi-AZ Cluster |
|---------|-----------------|-----------------|
| Standby count | 1 | 2 |
| Standby readable | No | Yes |
| Failover time | 60-120 sec | ~35 sec |
| Engines | All | MySQL 8.0, PG 13+ |
| Storage | EBS per instance | Local NVMe + EBS |
| Cost | ~2× | ~3× |

> 🔑 **Exam key:** Multi-AZ standby CANNOT serve reads for the classic deployment. If you want readable standbys on RDS (non-Aurora), use Multi-AZ DB Cluster or add Read Replicas.

**Failover triggers:**
- Primary AZ or DB instance failure
- DB instance class change
- OS patching
- Manual `reboot with failover` API call

**Failover mechanism:** DNS CNAME for the DB endpoint flips to the standby's IP. Applications must use the endpoint DNS name (not IP) to benefit from automatic failover.

---

### 📖 Read Replicas

| Feature | RDS Read Replica |
|---------|-----------------|
| **Max replicas** | 5 per source (MySQL/PG/MariaDB), chaining supported |
| **Replication** | Async, engine log-based (binlog / WAL streaming) |
| **Replication lag** | Seconds to minutes |
| **Cross-region** | Yes (MySQL, PG, MariaDB) |
| **Promotion** | Manual; creates independent DB instance |
| **Use after promotion** | No longer a replica; standalone DB |
| **Cross-region replica encryption** | Needs CMK in target region |
| **Oracle/SQL Server** | No Read Replicas (use Active Data Guard for Oracle separately) |

> 🔑 **Exam key:** RDS Read Replicas use async replication — there is always some lag. For zero-lag read scaling, use Aurora Replicas. Oracle RDS does NOT support read replicas natively on RDS.

---

### ⏱️ RPO / RTO Table

| Deployment | RPO | RTO |
|-----------|-----|-----|
| Single-AZ, no replica | Up to 1 day (last backup) | 20-40 min (restore) |
| Single-AZ + PITR | 5 min | 20-40 min |
| Multi-AZ (Instance) | ~0 (sync replication) | 60-120 sec |
| Multi-AZ (Cluster) | ~0 | ~35 sec |
| Cross-region Read Replica (promoted) | Seconds-minutes (async lag) | Minutes (manual promotion) |
| Cross-region Read Replica + automation | Seconds | 5-10 min (automated promotion via Lambda) |

---

### 💾 Backup & Restore

- **Automated backups:** Daily snapshot + transaction logs; retention 0–35 days; stored in S3 (managed by AWS)
- **PITR:** Restore to any second within retention window → creates **new DB instance**
- **Manual snapshots:** Survive instance deletion; shareable cross-account; copyable cross-region; no expiry
- **Snapshot restore:** Always creates a new DB instance (cannot restore in-place)
- **AWS Backup integration:** Policy-driven backup; cross-account and cross-region copies

> 🔑 **Exam key:** Setting backup retention to 0 disables automated backups entirely (and disables PITR). Manual snapshots are always retained regardless of retention setting.

---

### 🔄 Migration

#### Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **AWS DMS** | Data migration with CDC | Minimal-downtime migrations, heterogeneous |
| **AWS SCT** | Schema + code conversion | Heterogeneous migrations (Oracle→PG, SQL Server→MySQL) |
| **mysqldump / pg_dump** | Native export/import | Small DBs, offline migration |
| **Oracle Data Pump** | Oracle export/import | Oracle → Oracle RDS |
| **SQL Server BACPAC** | SQL Server backup format | SQL Server → RDS SQL Server |
| **RDS Snapshot Restore** | Same-engine restore | RDS → RDS (same engine) |
| **AWS MGN** | Server migration (not DB-specific) | Lift-and-shift entire server |

#### DMS Key Concepts

- **Replication instance:** EC2 instance running DMS engine — choose size based on data volume
- **Source endpoint:** Connection to source DB (on-premises, EC2, RDS, etc.)
- **Target endpoint:** Connection to target DB (RDS, Aurora, S3, Redshift, etc.)
- **Task types:** Full Load, CDC only, Full Load + CDC
- **Supported sources:** Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, SAP, IBM Db2, and more
- **Ongoing replication:** DMS can run continuously post-migration for live sync

#### SCT Key Concepts

- **Schema conversion:** DDL (tables, views, indexes) + stored procedures, functions, triggers
- **Conversion report:** Shows automated conversion rate + manual effort items
- **Extension pack:** DMS-provided replacement functions for unsupported source engine features
- **Typical flow:** SCT converts schema → deploy to target → DMS migrates data → validate → cutover

---

### 🔗 Integration with Other AWS Services

| Service | Purpose |
|---------|---------|
| **RDS Proxy** | Connection pooling, IAM auth, failover transparency |
| **Secrets Manager** | Auto-rotate DB credentials |
| **IAM** | DB authentication tokens |
| **CloudWatch** | Metrics, alarms, Enhanced Monitoring (per-process OS metrics) |
| **Performance Insights** | Query-level DB performance analysis |
| **EventBridge** | Respond to RDS events (failover, backup, maintenance) |
| **Lambda** | Trigger Lambda on RDS events |
| **S3** | Import/export data (MySQL: `LOAD DATA FROM S3`) |
| **CloudTrail** | API audit |
| **AWS Backup** | Centralized backup policy |
| **VPC** | Network isolation |

---

### ⚖️ Comparison: RDS vs Aurora vs Self-Managed

| Feature | RDS | Aurora | Self-Managed on EC2 |
|---------|-----|--------|---------------------|
| Engine | Standard community/vendor | MySQL/PG compat, reengineered | Any |
| Management | Fully managed | Fully managed | You manage everything |
| Read replicas | Up to 5 | Up to 15 | Manual |
| Replication lag | Seconds | Milliseconds | Variable |
| Storage | EBS (manual size) | Auto-grow to 128 TiB | You manage |
| Failover | 60-120 sec | ~30 sec | Manual |
| Oracle support | Yes (BYOL or LI) | No | Yes |
| SQL Server | Yes | No (Babelfish only) | Yes |
| Serverless | No | Yes (v2) | No |
| Backtrack | No | MySQL only | No |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max storage (gp3/io1) | 64 TiB |
| Max IOPS (io2) | 64,000 |
| Read replicas per source | 5 (MySQL/PG/MariaDB) |
| Backup retention | 0-35 days |
| DB parameter group changes | Dynamic or static (reboot required) |
| Multi-AZ failover time | 60-120 sec (instance), ~35 sec (cluster) |
| Oracle on RDS | No RAC, no Data Guard (use native DG on EC2 instead) |
| SQL Server | No cross-region read replicas |
| Encryption | Cannot add to existing unencrypted instance |

---

### 🏛️ Well-Architected Framework

| Pillar | RDS Considerations |
|--------|--------------------|
| **Operational Excellence** | Use Parameter Groups as code; automate backups via AWS Backup; use Performance Insights for query analysis; use RDS Events + EventBridge for automated remediation |
| **Security** | Enable encryption at creation (CMK preferred); enforce SSL; use IAM DB auth; keep instances in private subnets; use Secrets Manager for rotation; enable Enhanced Monitoring |
| **Reliability** | Always use Multi-AZ for production; use RDS Proxy for connection resilience; test failover regularly; set appropriate backup retention; use cross-region read replicas for DR |
| **Performance Efficiency** | Right-size instances with CloudWatch metrics; use gp3 for cost-effective IOPS; add read replicas for read-heavy workloads; use RDS Proxy to reduce connection overhead for Lambda |
| **Cost Optimization** | Use Reserved Instances (1- or 3-year) for predictable workloads; stop dev/test instances nights/weekends; use gp3 (cheaper than io1 for same IOPS); consider Aurora for MySQL/PG (better performance/cost at scale) |
| **Sustainability** | Graviton-based instances available (r6g, m6g); right-size to avoid idle over-provisioning; stop non-production instances when not in use |

---

### 📐 4 Exam Domains

**Domain 1 — Design Solutions for Organizational Complexity**
RDS supports multi-account patterns via cross-account snapshot sharing, AWS Backup cross-account policies, and RDS Proxy for centralizing connection management. Use SCPs to enforce encryption across org.

**Domain 2 — Design for New Solutions**
Choose RDS for Oracle (BYOL cost control) or SQL Server workloads requiring specific Windows features. Use Multi-AZ DB Cluster for near-Aurora HA on standard MySQL/PG. Use RDS Proxy + IAM auth for serverless architectures.

**Domain 3 — Continuous Improvement for Existing Solutions**
Migrate from single-AZ to Multi-AZ with zero downtime (AWS modifies in-place). Enable Performance Insights to identify top SQL queries. Enable storage auto-scaling to prevent capacity alerts. Enable PITR by setting backup retention > 0.

**Domain 4 — Accelerate Workload Migration and Modernization**
Oracle/SQL Server on-premises → RDS: SCT converts schema, DMS migrates data with CDC for minimal downtime. SQL Server → Aurora PostgreSQL via Babelfish for T-SQL compatibility. Consider migrating MySQL/PG RDS → Aurora for better scaling.


---

## ⚡ Chapter 10: Amazon DynamoDB

### 📖 Overview

**Amazon DynamoDB** is AWS's fully managed, serverless, NoSQL key-value and document database. It delivers single-digit millisecond performance at any scale — from a few requests per second to millions per second — with no servers to manage, no cluster to size, and no schema migrations.

> 🔑 **Exam key:** DynamoDB is the answer when you need: massive scale, single-digit ms latency, flexible/schemaless data, serverless, or key-value/document access patterns. It is NOT a relational DB — no joins, no complex SQL, no ACID across multiple tables (only single-item/transaction-level ACID).

**Real-life analogy:** A hashmap in the cloud — extremely fast lookups by key, but no support for "scan all rows and compute a sum" without designing for it.

**Scope:** **Regional** service. Global Tables extend to multiple regions (active-active).

**Open-source equivalent:** Apache Cassandra (wide-column, similar distribution model), Redis (key-value, in-memory), MongoDB (document).

---

### 🏗️ Architecture & Data Model

```mermaid
graph TB
    subgraph "DynamoDB Table"
        PK["🔑 Partition Key (Hash)\nRequired — determines partition"]
        SK["🗂️ Sort Key (Range)\nOptional — enables range queries"]
        ATTR["📋 Attributes\nArbitrary, schemaless per item"]
    end
    subgraph "Storage Layer"
        P1["Partition 1\n(AZ-a, AZ-b, AZ-c replicas)"]
        P2["Partition 2\n(AZ-a, AZ-b, AZ-c replicas)"]
        P3["Partition 3\n(AZ-a, AZ-b, AZ-c replicas)"]
    end
    PK --> P1
    PK --> P2
    PK --> P3
```

**Core concepts:**
- **Table:** Collection of items (rows)
- **Item:** A record; max 400 KB
- **Primary Key:** Partition key alone OR partition key + sort key (composite)
- **Partition Key:** Determines which physical partition stores the item (hash function)
- **Sort Key:** Allows range queries within a partition
- **Attributes:** Schemaless — each item can have different attributes (only PK required)
- **DynamoDB is inherently multi-AZ** — every partition has 3 replicas across 3 AZs; there is no "Multi-AZ option" to enable, it is always on

---

### 🔑 Multi-AZ (Built-in)

DynamoDB does **not have a Multi-AZ toggle** — it is **always multi-AZ**. Every write is synchronously replicated to at least 2 of 3 AZs before acknowledged (with strong consistency), or to a majority partition for eventual consistency.

| Consistency Model | Behavior | Cost |
|------------------|----------|------|
| **Eventually Consistent Read** | Returns data from any replica (may be slightly stale) | 0.5 RCU per 4 KB |
| **Strongly Consistent Read** | Returns most recent write (reads from leader) | 1 RCU per 4 KB |
| **Transactional Read** | ACID across multiple items | 2 RCU per 4 KB |
| **Transactional Write** | ACID across multiple items | 2 WCU per 1 KB |

> 🔑 **Exam key:** DynamoDB Global Tables is NOT the same as Multi-AZ. Multi-AZ is same-region 3-AZ replication (always on). Global Tables is active-active cross-region replication (optional, separate feature).

---

### 📊 Capacity Modes

| Mode | How It Works | Best For | Limits |
|------|-------------|----------|--------|
| **Provisioned** | Set RCU + WCU in advance; auto-scaling adjusts within set range | Predictable, steady workloads | Manual capacity planning |
| **On-Demand** | Pay per request; no capacity planning; scales instantly | Spiky, unpredictable workloads | 2× previous peak instantly |
| **Reserved Capacity** | 1- or 3-year commitment on provisioned RCU/WCU | Cost reduction for steady state | Provisioned mode only |

> 🔑 **Exam key:** On-Demand is more expensive per request than Provisioned but eliminates throttling for truly unpredictable workloads. Auto-scaling on Provisioned mode can handle gradual spikes but not instant traffic surges (takes minutes to scale). For flash sales or unknown load: On-Demand.

---

### 🗂️ Indexes

| Index Type | Key | Location | Use Case |
|-----------|-----|----------|----------|
| **Local Secondary Index (LSI)** | Same PK, different SK | Same partition | Query on alternate sort key within a partition |
| **Global Secondary Index (GSI)** | Different PK (and optional SK) | Separate partition space | Query on any attribute as if it were PK |

- **LSI:** Must be defined at table creation; up to 5 per table; uses table's provisioned capacity; strong consistency supported
- **GSI:** Can be added/deleted after creation; up to 20 per table; has its own RCU/WCU; only eventually consistent; GSI over-provisioning is common exam trap

> 🔑 **Exam key:** GSI has its own capacity — if GSI is throttled, writes to the base table that update the GSI will also be throttled. Always provision GSI capacity separately.

---

### 🌍 Global Tables (Cross-Region Active-Active)

**DynamoDB Global Tables** provide **active-active multi-region replication** — you can write to any region and reads/writes are replicated to all other regions within ~1 second.

```mermaid
graph LR
    R1["🌎 us-east-1\nRead/Write"]
    R2["🌍 eu-west-1\nRead/Write"]
    R3["🌏 ap-southeast-1\nRead/Write"]
    R1 <-->|"~1 sec async replication"| R2
    R2 <-->|"~1 sec async replication"| R3
    R1 <-->|"~1 sec async replication"| R3
```

| Feature | Value |
|---------|-------|
| Replication | Async, ~<1 sec typical |
| Consistency across regions | Eventual (last-writer-wins conflict resolution) |
| Conflict resolution | Last-writer-wins (based on timestamp) |
| Max regions | No hard limit |
| Requirement | Table must be empty at Global Table creation OR use Version 2019 |

> 🔑 **Exam key:** DynamoDB Global Tables = active-active (write to any region). Aurora Global Database = active-passive (write to primary only). This is a critical exam distinction.

---

### ⚡ DynamoDB Accelerator (DAX)

**DAX** is an in-memory cache for DynamoDB — fully managed, cluster-based, microsecond read latency.

| Feature | Value |
|---------|-------|
| Latency | Microseconds (vs ms for DynamoDB) |
| API | DynamoDB-compatible (same API, no app code change) |
| Cache type | Write-through for writes; item cache + query cache for reads |
| HA | Multi-AZ cluster (primary + replicas) |
| Not suitable for | Write-heavy workloads; strongly consistent reads; Scan-heavy workloads |
| VPC | Deployed inside your VPC |

> 🔑 **Exam key:** DAX is NOT suitable if you need strongly consistent reads (DAX always returns cached/eventually consistent data). Use DAX for read-heavy, latency-sensitive workloads (leaderboards, session stores, product catalogs).

---

### ⏱️ RPO / RTO Table

| Scenario | RPO | RTO |
|----------|-----|-----|
| AZ failure (standard) | ~0 (sync replicas) | Automatic, seconds | 
| Region failure (Global Tables) | ~1 sec (async replication lag) | Minutes (redirect DNS/app to other region) |
| Accidental delete (PITR) | Up to 35 days back | Minutes (restore to new table) |
| Table restore from backup | Last on-demand backup | 1-10 min (depends on table size) |

---

### 💾 Backup & Restore

| Type | Description |
|------|-------------|
| **On-Demand Backup** | Full backup at a point in time; stored in DynamoDB; no impact on performance; retained until deleted |
| **PITR (Point-In-Time Recovery)** | Enable per table; 35-day rolling window; restore to any second; restores to **new table** |
| **AWS Backup integration** | Policy-driven; cross-account; cross-region copy of DynamoDB backups |
| **Export to S3** | Export table data to S3 in JSON or ION format without using read capacity |

> 🔑 **Exam key:** PITR does not pause table operations — it uses DynamoDB's continuous backup logs. Restore creates a **new table** (not in-place). Export to S3 uses DynamoDB Streams internally, not read capacity.

---

### 📡 DynamoDB Streams

**DynamoDB Streams** captures a time-ordered log of item-level changes (insert, update, delete) — retained for 24 hours.

| Use Case | Pattern |
|----------|---------|
| Trigger Lambda on changes | Lambda event source mapping to Streams |
| Cross-region replication (custom) | Streams → Lambda → write to other region |
| Audit trail | Streams → Kinesis Data Firehose → S3 |
| Search index | Streams → Lambda → OpenSearch |
| Aggregation/analytics | Streams → Kinesis Data Streams → analytics |

> 🔑 **Exam key:** Global Tables uses DynamoDB Streams internally for replication — enabling Streams is a prerequisite for Global Tables.

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **At-rest** | AES-256 via KMS | Always encrypted; choose AWS-owned key, AWS-managed key, or CMK |
| **In-transit** | TLS 1.2+ | All API calls use HTTPS |
| **VPC Endpoint** | Gateway VPC Endpoint for DynamoDB | Traffic stays in AWS network, no internet |
| **Fine-grained access** | IAM condition keys (`dynamodb:LeadingKeys`) | Restrict users to their own items |

> 🔑 **Exam key:** DynamoDB is ALWAYS encrypted at rest — you cannot create an unencrypted table. The choice is which KMS key to use. AWS-owned key = no cost, no visibility. CMK = full control + cost.

---

### 🔄 Migration

| Scenario | Tool |
|---------|------|
| On-premises MongoDB → DynamoDB | AWS DMS (source: MongoDB; target: DynamoDB) |
| Cassandra → DynamoDB | AWS DMS + data modeling redesign |
| Relational DB → DynamoDB | Redesign data model first; DMS for data transfer |
| DynamoDB → DynamoDB (new table) | Data Pipeline, Export to S3 + import, DMS |
| Large migration | AWS DMS with Full Load + CDC |

> ⚠️ **DynamoDB migration requires data model redesign.** Relational → NoSQL is not a simple lift-and-shift — you must think about access patterns, denormalization, and partition key design first.

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max item size | 400 KB |
| Max partition key size | 2 KB |
| Max sort key size | 1 KB |
| Max attribute name length | 64 KB |
| Max table name length | 255 chars |
| LSI per table | 5 |
| GSI per table | 20 |
| Max tables per region | 2,500 (soft limit) |
| On-Demand max throughput | 40,000 RCU / 40,000 WCU (default, can increase) |
| DynamoDB Streams retention | 24 hours |
| PITR retention | 35 days |
| Max DynamoDB transaction items | 100 items |

---

### ⚖️ Comparison

| Feature | DynamoDB | MongoDB Atlas | Cassandra | Redis |
|---------|----------|--------------|-----------|-------|
| Model | Key-value / document | Document | Wide-column | Key-value (in-memory) |
| Scale | Unlimited (serverless) | Manual cluster | Manual cluster | Cluster-based |
| Latency | Single-digit ms | ms | ms | Microseconds |
| Query language | DynamoDB API, PartiQL | MongoDB query language | CQL | Redis commands |
| ACID transactions | Single-table, up to 100 items | Yes | Lightweight transactions | No |
| Global | Global Tables (active-active) | Atlas Global Clusters | Manual | Manual |
| Fully managed on AWS | Yes | No (Atlas is separate SaaS) | No (Keyspaces is managed) | ElastiCache |

---

### 🏛️ Well-Architected Framework

| Pillar | DynamoDB Considerations |
|--------|--------------------------|
| **Operational Excellence** | Use IaC (CloudFormation/CDK) for table definitions; use PartiQL for SQL-like queries; monitor consumed capacity vs provisioned to right-size |
| **Security** | Always use CMK for sensitive data; use VPC Gateway Endpoint (no internet traversal); use fine-grained IAM with `dynamodb:LeadingKeys` to restrict item access; enable CloudTrail for API audit |
| **Reliability** | Multi-AZ is built-in; enable PITR for accidental delete protection; use Global Tables for multi-region resilience; test Global Tables failover |
| **Performance Efficiency** | Design partition key for uniform distribution (avoid hot partitions); use DAX for microsecond reads; use GSIs for alternative access patterns; use On-Demand for spiky workloads |
| **Cost Optimization** | Use On-Demand for dev/test; use Provisioned + auto-scaling + Reserved Capacity for production; use TTL to auto-expire old items (free); export to S3 + Athena for analytics instead of Scan |
| **Sustainability** | Serverless — pay only for what you use; TTL removes unneeded data; On-Demand scales to zero (near-zero cost) when idle |

---

### 📐 4 Exam Domains

**Domain 1 — Design Solutions for Organizational Complexity**
DynamoDB tables are per-account/per-region. Use AWS Backup policies across org for consistent PITR coverage. IAM resource policies can restrict table access per OU. Global Tables must be set up per region explicitly.

**Domain 2 — Design for New Solutions**
DynamoDB for session stores, shopping carts, real-time leaderboards, IoT telemetry, user profiles — any access-pattern-driven, high-scale, low-latency workload. Use DAX layer for sub-ms reads. Use Streams + Lambda for event-driven architectures.

**Domain 3 — Continuous Improvement for Existing Solutions**
Enable PITR if not already. Convert Scan operations to Query by adding GSI on queried attributes. Move from Provisioned to On-Demand if workload is spiky. Use DAX to offload read capacity. Enable TTL to reduce storage costs.

**Domain 4 — Accelerate Workload Migration and Modernization**
MongoDB/Cassandra on-premises → DynamoDB via DMS + data model redesign. Move session state from ElastiCache (requires manual expiry management) to DynamoDB + TTL (simpler operations). Move relational DB hot-path reads to DynamoDB (NoSQL for speed, RDS for reporting).


---

## 📊 Chapter 11: Amazon Redshift

### 📖 Overview

**Amazon Redshift** is AWS's fully managed, petabyte-scale cloud data warehouse. It uses a **massively parallel processing (MPP)** architecture with **columnar storage** to execute complex analytical queries (OLAP) across billions of rows in seconds. It is NOT designed for OLTP — it's optimized for read-heavy analytical workloads.

> 🔑 **Exam key:** Redshift = data warehouse (OLAP). RDS/Aurora = transactional database (OLTP). Use Redshift for analytics, reporting, BI tools, aggregations over large datasets. Use RDS for transactional applications.

**Real-life analogy:** Redshift is like a column-oriented spreadsheet in the cloud — instead of reading entire rows, it reads only the columns needed for your query, making aggregations (SUM, AVG, GROUP BY) massively faster.

**Scope:** **Regional** service. Redshift clusters are in a specific region and AZ (single-AZ by default; Multi-AZ option available for RA3 nodes).

**Open-source equivalent:** Apache Hive, Apache Spark SQL, Greenplum, PostgreSQL (partial wire-protocol compatibility).

---

### 🏗️ Architecture

```mermaid
graph TB
    subgraph "Redshift Cluster"
        L["🖥️ Leader Node\n(Query Planning & Coordination)"]
        C1["⚙️ Compute Node 1\n(Slice 1 | Slice 2)"]
        C2["⚙️ Compute Node 2\n(Slice 3 | Slice 4)"]
        C3["⚙️ Compute Node 3\n(Slice 5 | Slice 6)"]
    end
    L -->|"Distributes query fragments"| C1
    L -->|"Distributes query fragments"| C2
    L -->|"Distributes query fragments"| C3
    C1 -->|"Returns partial results"| L
    C2 -->|"Returns partial results"| L
    C3 -->|"Returns partial results"| L
    Client["🌐 BI Tool / JDBC/ODBC"] -->|"SQL queries"| L
    S3["🪣 S3 Data Lake"] <-->|"COPY / UNLOAD\nSpectrum"| C1
    S3 <-->|"Spectrum"| C2
```

**Key components:**
- **Leader Node:** Receives queries, parses, creates execution plan, coordinates compute nodes, aggregates results
- **Compute Nodes:** Execute query slices in parallel; each node has local disk (SSD/HDD)
- **Node Slices:** Each compute node is divided into slices; each slice processes a portion of data
- **Columnar Storage:** Data stored column-by-column (not row-by-row); enables compression and column-skipping
- **Data Distribution Styles:** EVEN, KEY, ALL, AUTO — determines how rows are distributed across slices

---

### 🔧 Node Types & Cluster Variants

| Type | Nodes | Storage | Use Case |
|------|-------|---------|----------|
| **RA3 (recommended)** | 2–128 nodes | Managed storage (S3-backed, local SSD cache) | Decouple compute from storage, scale independently |
| **DC2** | 2–128 nodes | Local SSD | Fast local I/O, fixed storage |
| **DS2 (legacy)** | HDD | Legacy | Migrate to RA3 |
| **Redshift Serverless** | N/A | Managed | On-demand, no cluster management |

| Variant | Description |
|---------|-------------|
| **Provisioned Cluster** | Fixed cluster size; manual scaling; reserved instances available |
| **Redshift Serverless** | Auto-scales RPU; pay per use; no cluster management; cold-start latency |
| **Redshift Spectrum** | Query S3 data directly without loading; uses external tables |
| **Redshift ML** | Train and run ML models inside Redshift using SQL via SageMaker |
| **Data Sharing** | Share live data across clusters/accounts without copying |

---

### 🔒 Connectivity & Security

- Deployed in a **VPC** (Enhanced VPC routing recommended — forces COPY/UNLOAD through VPC)
- **Cluster Subnet Group:** Spans AZs; cluster placed in one AZ (Multi-AZ optional for RA3)
- **Security Groups** control access to port 5439 (default Redshift port)
- **Public accessibility** can be disabled; use **PrivateLink** for private access from other VPCs
- **SSL/TLS** enforced via cluster parameter `require_ssl`
- **IAM authentication** supported; Redshift also has native DB users
- **VPC Endpoints** for S3 access during COPY/UNLOAD (Enhanced VPC routing)

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **At-rest** | AES-256 via KMS or HSM | Enable at cluster creation; cannot change after |
| **Key management** | AWS KMS CMK or CloudHSM | HSM for dedicated hardware key storage |
| **In-transit** | SSL/TLS | Parameter `require_ssl = true` |
| **Column-level encryption** | Client-side via encryption functions | `encrypt()`, `decrypt()` built-in functions |
| **Data masking** | Dynamic Data Masking (Serverless/RA3) | Mask sensitive columns per role |

> ⚠️ **Encryption cannot be changed after cluster creation.** To encrypt an unencrypted cluster: create new encrypted cluster → UNLOAD data to S3 → COPY to new cluster. Or use snapshot + restore with encryption.

---

### 📈 Scalability & HA

**Scaling compute:**
- **Elastic Resize:** Add/remove nodes in ~15 minutes; cluster remains online (queries paused briefly)
- **Classic Resize:** Change node type or large size changes; cluster goes read-only; takes hours
- **Concurrency Scaling:** Auto-adds transient cluster capacity for read queries during bursts; first 1 hour/day free
- **Redshift Serverless:** Auto-scales RPU (Redshift Processing Units) based on workload

**Scaling storage (RA3):**
- RA3 decouples compute from storage — storage automatically expands in S3; local SSD is a cache
- Add compute nodes without moving data (data stays in managed S3-backed storage)

---

### 🏢 Multi-AZ

By default, a Redshift provisioned cluster is **single-AZ**. Options for HA:

| Option | Description | RTO |
|--------|-------------|-----|
| **Multi-AZ (RA3 only)** | Active-standby across 2 AZs; automatic failover | ~5-10 min |
| **Snapshot + restore** | Manual; restore to new cluster in different AZ | 30+ min |
| **Redshift Serverless** | Managed HA; AWS handles AZ resilience | Minimal |

> 🔑 **Exam key:** Redshift is primarily a data warehouse — short-term AZ failures are tolerated because data is already in S3 (for RA3). For OLTP-grade HA, use RDS/Aurora. Redshift's strength is query performance, not sub-minute failover.

---

### 📖 Read Replicas / Concurrency Scaling

Redshift does NOT have traditional Read Replicas. Instead:

| Feature | How It Works |
|---------|-------------|
| **Concurrency Scaling** | Auto-provisions additional read clusters during spikes; routes SELECT queries automatically |
| **Data Sharing (cross-cluster)** | Share live data to a separate "consumer" cluster for isolation |
| **Workload Management (WLM)** | Queue-based query prioritization; prevents small queries from being stuck behind large ones |
| **Short Query Acceleration (SQA)** | Auto-detects short queries and runs them in a dedicated fast queue |

---

### ⏱️ RPO / RTO Table

| Scenario | RPO | RTO |
|----------|-----|-----|
| Single-AZ cluster failure | 0 (no data loss, last snapshot) | 30+ min (restore) |
| RA3 Multi-AZ failover | ~0 | ~5-10 min |
| Snapshot restore (same region) | Hours (last snapshot) | 20-60 min |
| Cross-region restore | Hours (last cross-region snapshot) | 30-90 min |
| Redshift Serverless AZ failure | Minimal | Seconds–minutes (AWS managed) |

---

### 💾 Backup & Restore

- **Automated snapshots:** Every 8 hours or every 5 GB of change; default retention 1 day; up to 35 days
- **Manual snapshots:** Retained indefinitely; copy cross-region for DR
- **Cross-region snapshot copy:** Enable on cluster; specify target region; KMS key in target region required
- **Restore:** Creates a new cluster from snapshot (RA3 restores quickly — metadata only, data in S3)
- **AWS Backup:** Supports Redshift automated + manual snapshots

---

### 🔄 Migration

| Source | Method |
|--------|--------|
| **On-premises data warehouse (Teradata, Netezza)** | AWS SCT + DMS or native UNLOAD/COPY via S3 |
| **Oracle/SQL Server DW** | AWS SCT converts DDL + SQL; DMS or S3 staging for data |
| **S3 data lake** | `COPY` command directly from S3 |
| **RDS/Aurora** | DMS (Redshift as target); or export to S3 → COPY |
| **On-premises flat files** | Upload to S3 → `COPY` command |

**AWS SCT for Redshift:**
- Converts source schema to Redshift DDL
- Converts SQL queries, stored procedures (limited)
- Assessment report shows conversion complexity

**COPY command (primary bulk load method):**
```sql
COPY tablename FROM 's3://bucket/prefix'
IAM_ROLE 'arn:aws:iam::account:role/RedshiftRole'
FORMAT AS PARQUET;
```

---

### 🔗 Integration with Other AWS Services

| Service | Integration |
|---------|-------------|
| **S3** | COPY (load), UNLOAD (export), Spectrum (query in place) |
| **Glue** | ETL pipeline → Redshift; Glue Data Catalog as external schema |
| **Kinesis Data Firehose** | Streaming data → Redshift (via S3 staging) |
| **QuickSight** | BI dashboard directly on Redshift |
| **SageMaker** | Redshift ML trains models via SageMaker Autopilot |
| **Lake Formation** | Fine-grained access control on Spectrum tables |
| **DMS** | Replicate transactional DB data to Redshift for analytics |
| **EventBridge** | Respond to Redshift events (resize, restore, etc.) |
| **CloudWatch** | Cluster metrics, query performance monitoring |
| **IAM** | Role-based access; IAM DB authentication |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max nodes per cluster | 128 (RA3) |
| Max storage per RA3.16xlarge node | 64 TB managed storage |
| Max columns per table | 1,600 |
| Max tables per database | 100,000 |
| Max concurrent connections | 500 |
| Snapshot retention | 1-35 days (automated) |
| Redshift Serverless max RPU | 512 |
| COPY file size recommendation | 1 MB – 1 GB per file |
| Spectrum max S3 files per query | 100,000 |

---

### ⚖️ Comparison

| Feature | Redshift | BigQuery (GCP) | Snowflake | Athena |
|---------|----------|----------------|-----------|--------|
| Model | Columnar MPP | Columnar serverless | Columnar multi-cluster | Query-in-place (S3) |
| Storage | Managed (RA3) or local | Google-managed | Snowflake-managed | S3 |
| Scaling | Elastic resize | Serverless | Auto-scale | Serverless |
| Cost model | Instance + storage OR serverless | Per TB scanned | Storage + compute credits | Per TB scanned |
| SQL dialect | PostgreSQL-based | Standard SQL | ANSI SQL | Presto/Trino |
| Integration | Deep AWS | Deep GCP | Cloud-agnostic | Deep AWS (S3/Glue) |
| Best for | AWS-native DW workloads | GCP workloads | Multi-cloud DW | Ad-hoc S3 queries |

---

### 🏛️ Well-Architected Framework

| Pillar | Redshift Considerations |
|--------|--------------------------|
| **Operational Excellence** | Use Redshift Advisor for optimization recommendations; monitor WLM queue depths; use automatic table optimization (ATO); automate snapshots with lifecycle policies |
| **Security** | Enable encryption at creation (CMK); use Enhanced VPC routing; enforce SSL; use IAM roles for COPY/UNLOAD (not access keys); enable CloudTrail; use column-level security / dynamic data masking |
| **Reliability** | Use RA3 Multi-AZ for critical workloads; enable cross-region snapshot copy for DR; use Redshift Serverless for managed HA; test restore procedures |
| **Performance Efficiency** | Choose correct distribution key (avoid skew); use sort keys for common filter columns; use compression encodings; use Concurrency Scaling for bursts; use Redshift Spectrum for cold data; use SQA for mixed workloads |
| **Cost Optimization** | Use RA3 (decouple compute/storage — scale compute independently); use Concurrency Scaling (first 1 hr/day free); use Redshift Serverless for dev/test; use Reserved Instances for production; use Spectrum for infrequently queried data |
| **Sustainability** | RA3 managed storage uses S3 (durable, efficient); Redshift Serverless scales to zero; right-size compute nodes; use compression to reduce storage |

---

### 📐 4 Exam Domains

**Domain 1 — Organizational Complexity**
Use Data Sharing to share live data across accounts/clusters in an org without copying. Use Lake Formation for unified data governance across S3 and Redshift Spectrum. Cross-account snapshot sharing for DR.

**Domain 2 — Design for New Solutions**
New analytics platform: RA3 cluster + Spectrum for S3 data lake + QuickSight for BI. Use Kinesis Firehose for streaming ingestion. Use Redshift Serverless for development/occasional analytics. Use Redshift ML for in-database ML.

**Domain 3 — Continuous Improvement**
Enable Redshift Advisor recommendations. Switch DC2 to RA3 for storage flexibility. Enable Automatic Table Optimization. Add Concurrency Scaling for query bursts. Add Redshift Spectrum to query historical cold data in S3 without loading.

**Domain 4 — Migration**
Migrate Teradata/Netezza on-premises DW to Redshift via SCT + UNLOAD to S3 + COPY. Use DMS for ongoing replication from OLTP sources. Use Glue ETL for transformation during migration.


---

## 🔗 Chapter 12: Amazon Neptune

### 📖 Overview

**Amazon Neptune** is AWS's fully managed graph database service. It supports two popular graph query languages: **Gremlin** (property graph / Apache TinkerPop) and **SPARQL** (RDF graph / W3C standard). Neptune is purpose-built for applications with highly connected data where relationships between entities are as important as the entities themselves.

> 🔑 **Exam key:** Neptune = graph database. Use when relationships are first-class data (social networks, fraud detection, knowledge graphs, recommendation engines). Do NOT use Neptune for relational workloads or key-value patterns.

**Real-life analogy:** A graph database is like a map of roads and cities — not just storing cities (nodes) and roads (edges), but making it fast to answer "what's the shortest path?" or "which cities are reachable in 2 hops?"

**Scope:** **Regional** service. Multi-AZ supported.

**Open-source equivalents:** Neo4j (property graph), Apache Jena (RDF), JanusGraph (TinkerPop).

---

### 🏗️ Architecture

Neptune shares the same **distributed storage architecture as Aurora** — 6 copies of data across 3 AZs, storage auto-grows, quorum-based reads/writes.

```mermaid
graph TB
    subgraph "Neptune Cluster"
        W["✏️ Primary Instance\n(Reads + Writes)"]
        R1["📖 Read Replica 1\n(AZ-b)"]
        R2["📖 Read Replica 2\n(AZ-c)"]
    end
    subgraph "Distributed Storage (Aurora-like)"
        S1["💾 Storage AZ-a\n(2 copies)"]
        S2["💾 Storage AZ-b\n(2 copies)"]
        S3["💾 Storage AZ-c\n(2 copies)"]
    end
    W --> S1
    W --> S2
    W --> S3
    R1 --> S2
    R2 --> S3
    GQL["🌐 Gremlin / SPARQL\nClient"] --> W
```

**Graph models supported:**
- **Property Graph (Gremlin / openCypher):** Vertices (nodes) and edges with properties; TinkerPop API; Jupyter notebooks support
- **RDF (SPARQL):** Resource Description Framework; triples (subject-predicate-object); SPARQL 1.1

---

### 🔒 Connectivity & Security

- Deployed in a **VPC** with a **DB Subnet Group** (≥2 AZs)
- Accessible via HTTPS endpoints (Gremlin WebSocket, SPARQL REST)
- **No public access by default** — must use VPC or bastion
- **IAM authentication** supported (HTTP-based auth for Gremlin/SPARQL requests)
- **VPC Security Groups** control access
- **Port:** 8182 (default)
- **Neptune Streams:** Change log (like DynamoDB Streams) for graph changes

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **At-rest** | AES-256 via KMS | Enable at cluster creation; cannot change after |
| **In-transit** | TLS (enforced) | HTTPS-only; certificate validation required |
| **Snapshots** | Same KMS key as cluster | |
| **Cross-account** | Re-encrypt on snapshot copy | |

---

### 📈 Scalability & HA

- **Vertical:** Change instance class (requires brief failover)
- **Read scaling:** Up to **15 Neptune Replicas** (same as Aurora)
- **Storage:** Auto-grows in 10 GB increments up to 64 TiB
- **Multi-AZ:** Always 3-AZ storage; add replicas in different AZs for compute HA
- **Neptune Serverless:** Auto-scales Neptune Capacity Units (NCUs); available (preview); good for variable graph workloads

---

### 🏢 Multi-AZ

Same model as Aurora:
- Storage: always 6 copies / 3 AZs (built-in, not optional)
- Compute HA: add at least 1 replica in a different AZ
- Failover: ~30 seconds (replica promotion); DNS CNAME flip
- Standby replicas serve reads (unlike RDS Multi-AZ standby)

---

### 📖 Read Replicas

| Feature | Neptune Replica |
|---------|----------------|
| Max replicas | 15 |
| Replication lag | Milliseconds (shared storage) |
| Cross-region | No native cross-region replicas (unlike Aurora Global DB) |
| Promotion | ~30 sec |
| Reads | Yes — serves Gremlin/SPARQL read queries |

> 🔑 **Exam key:** Neptune does NOT have cross-region replication (no equivalent to Aurora Global Database). For cross-region Neptune DR: snapshot + restore in other region.

---

### ⏱️ RPO / RTO Table

| Scenario | RPO | RTO |
|----------|-----|-----|
| AZ failure (with replica) | ~0 | ~30 sec |
| Single instance (no replica) | Up to last backup | 20-40 min |
| Region failure | Hours (last snapshot) | 30-60 min (restore) |
| PITR | Up to 35 days | 20-40 min |

---

### 💾 Backup & Restore

- **Automated backups:** Daily, 1–35 day retention; continuous backup to S3
- **PITR:** Restore to any second within retention window → new cluster
- **Manual snapshots:** Indefinite retention; cross-account sharing; cross-region copy
- **Neptune Streams:** Capture graph changes; retain 7 days; use for change-data-capture pipelines

---

### 🔄 Migration

| Source | Method |
|--------|--------|
| **Neo4j** | Export to CSV → Neptune Bulk Loader via S3 |
| **Relational (RDS)** | Transform rows to vertices/edges → CSV → Bulk Loader |
| **Amazon DMS** | Limited support; Neptune not a primary DMS target |
| **Neptune Bulk Loader** | Primary tool; loads CSV format from S3 |

**Neptune Bulk Loader:** Fast parallel S3-to-Neptune import. Format: `application/vnd.amazon.neptune-csv` (vertices file + edges file).

---

### 🔗 Integration

| Service | Purpose |
|---------|---------|
| **S3** | Bulk load data via Neptune Bulk Loader |
| **Lambda** | Query Neptune from serverless functions |
| **SageMaker** | Neptune ML — use graph structure features in ML models |
| **Kinesis** | Stream graph changes via Neptune Streams |
| **CloudWatch** | Cluster metrics |
| **IAM** | HTTP-based authentication |
| **Jupyter/SageMaker Notebooks** | Neptune Workbench for interactive graph exploration |

---

### ⚖️ Comparison

| Feature | Neptune | Neo4j | DynamoDB | RDS |
|---------|---------|-------|----------|-----|
| Data model | Graph (PG + RDF) | Property Graph | Key-value/document | Relational |
| Query language | Gremlin, SPARQL, openCypher | Cypher | DynamoDB API | SQL |
| Managed on AWS | Fully | Self-managed on EC2 | Fully | Fully |
| Relationship traversal | Native | Native | Limited (manual) | Joins (slow at scale) |
| Scale | Up to 64 TiB | Cluster-based | Unlimited | 64 TiB |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max storage | 64 TiB |
| Max replicas | 15 |
| Max nodes/edges | Billions (practical limit: storage) |
| Cross-region replication | Not natively supported |
| Neptune Streams retention | 7 days |
| Encryption change | Cannot change after cluster creation |
| PITR retention | Up to 35 days |

---

### 🏛️ Well-Architected Framework

| Pillar | Neptune Considerations |
|--------|------------------------|
| **Operational Excellence** | Use Neptune Workbench for query development; monitor query latency via CloudWatch; use Neptune Streams for change tracking |
| **Security** | Encrypt at creation with CMK; use VPC private subnets; enforce TLS; use IAM HTTP auth; enable CloudTrail |
| **Reliability** | Add replicas in different AZs; enable PITR; use automated snapshots; test failover |
| **Performance Efficiency** | Design graph schema for access patterns; use read replicas for read-heavy traversals; use Neptune ML for graph-enhanced ML |
| **Cost Optimization** | Right-size instances; use Neptune Serverless for variable graph workloads; use snapshots instead of running idle clusters for dev/test |
| **Sustainability** | Neptune Serverless scales to minimum when idle; shared storage avoids per-replica duplication |

---

### 📐 4 Exam Domains

**Domain 1 — Organizational Complexity:** Neptune in VPC; IAM auth for programmatic access; CloudTrail for audit; cross-account snapshot sharing for DR.

**Domain 2 — Design for New Solutions:** Social network (fraud detection, recommendations) → Neptune with property graph. Knowledge graph (semantics, ontologies) → Neptune with RDF/SPARQL. Use Neptune ML for graph neural network features.

**Domain 3 — Continuous Improvement:** Add replicas for read scaling; enable Neptune Streams for CDC pipelines; enable PITR.

**Domain 4 — Migration:** Neo4j/relational → Neptune via Bulk Loader. Transform relational data to graph model (vertices + edges CSVs) → load to Neptune.


---

## 🚀 Chapter 13: Amazon ElastiCache

### 📖 Overview

**Amazon ElastiCache** is AWS's fully managed in-memory caching service. It supports two engines: **Redis** (now also offered as **Valkey**) and **Memcached**. ElastiCache sits in front of a database to absorb read traffic, dramatically reducing latency (microseconds to low milliseconds) and database load.

> 🔑 **Exam key:** ElastiCache = in-memory cache. Use to offload repetitive reads from RDS/Aurora/DynamoDB. NOT a database — data can be lost if cluster fails (unless Redis persistence enabled). Use when latency must be sub-millisecond and the data fits in memory.

**Real-life analogy:** ElastiCache is like a whiteboard next to a librarian — frequently requested books (data) are written on the whiteboard for instant access instead of fetching from the shelves (database) every time.

**Scope:** **Regional** service. Individual nodes are AZ-scoped. No native cross-region replication.

**Open-source equivalents:** Redis (upstream), Memcached (upstream).

---

### 🏗️ Architecture

```mermaid
graph LR
    App["🌐 Application\n(Cache-aside pattern)"]
    EC["🚀 ElastiCache\n(Redis Cluster)"]
    DB["🗄️ Aurora/RDS\n(Source of truth)"]
    App -->|"1. Check cache"| EC
    EC -->|"2a. Cache HIT — return data"| App
    EC -->|"2b. Cache MISS"| App
    App -->|"3. Fetch from DB"| DB
    DB -->|"4. Return data"| App
    App -->|"5. Write to cache"| EC
```

---

### 🔧 Redis vs Memcached

| Feature | Redis / Valkey | Memcached |
|---------|---------------|-----------|
| **Data structures** | Strings, hashes, lists, sets, sorted sets, bitmaps, HyperLogLog, streams | Strings only |
| **Persistence** | RDB snapshots + AOF logs | None |
| **Replication** | Yes (async) | No |
| **Multi-AZ failover** | Yes (automatic) | No |
| **Pub/Sub** | Yes | No |
| **Lua scripting** | Yes | No |
| **Cluster mode** | Yes (sharding) | Yes (multi-node) |
| **Transactions** | Yes (MULTI/EXEC) | No |
| **Geo-spatial** | Yes | No |
| **Use case** | Sessions, leaderboards, queues, pub/sub, complex data | Simple cache, large datasets, multi-threaded |
| **Valkey** | Drop-in Redis 7.2 fork (AWS-managed) | N/A |

> 🔑 **Exam key:** Use Redis when you need persistence, replication, failover, or complex data structures. Use Memcached only for simple, pure caching with multi-threaded horizontal scaling and no HA requirements.

---

### 🔧 Redis Deployment Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **Single-node** | One Redis node, no HA | Dev/test |
| **Cluster Mode Disabled (Replication Group)** | 1 primary + up to 5 replicas; all data on each node | Simple HA, read scaling |
| **Cluster Mode Enabled (Redis Cluster)** | Data sharded across up to 500 shards; each shard: 1 primary + up to 5 replicas | Massive datasets, horizontal scaling |

> 🔑 **Exam key:** Cluster Mode Disabled = entire dataset on each node (limited by node memory). Cluster Mode Enabled = dataset sharded across nodes (scale beyond single-node memory limit).

---

### 🔒 Connectivity & Security

- Deployed in a **VPC** with a **Cache Subnet Group** (spans AZs)
- **Security Groups** control access; no public access by default
- **Encryption in-transit:** TLS enabled per cluster; clients must use TLS endpoint
- **Encryption at-rest:** AES-256 via KMS (Redis only, not Memcached)
- **AUTH token:** Redis password authentication; `AUTH <password>` command
- **RBAC (Redis 6+):** Role-based access control; user accounts with specific command permissions
- **Port:** 6379 (Redis default), 11211 (Memcached)

---

### 🔐 Encryption Deep-Dive

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **At-rest** | AES-256 via KMS | Redis only; enable at creation; CMK supported |
| **In-transit** | TLS 1.2+ | Enable at creation; client must use TLS |
| **AUTH** | Password token | Basic Redis AUTH |
| **RBAC** | IAM-like user/group model | Redis 6+; fine-grained command control |
| **Memcached** | No at-rest encryption | TLS in-transit only |

---

### 📈 Scalability & HA

**Redis Cluster Mode Disabled:**
- Add/remove replicas (read scaling)
- Vertical: change node type (requires cluster recreation for major changes)
- Failover: primary failure → replica promoted automatically (~30-60 sec)

**Redis Cluster Mode Enabled:**
- **Online resharding:** Add shards without downtime
- **Shard scaling:** Scale out (add shards) or scale in (remove shards)
- Up to 500 shards × 6 nodes = 3000 nodes per cluster

**Memcached:**
- Add/remove nodes (horizontal scaling)
- No replication, no failover
- Client-side consistent hashing for key distribution

---

### 🏢 Multi-AZ

**Redis Multi-AZ:**
- Primary in one AZ; replicas in other AZs
- Automatic failover: replica in another AZ promoted on primary failure
- DNS endpoint for primary updates automatically (~30-60 sec downtime)
- **Cluster Mode Enabled:** Each shard can have replicas in different AZs

**Memcached:**
- No Multi-AZ support; no replication; nodes are independent
- For HA: add nodes across AZs; application handles cache miss on node failure

---

### 📖 Read Replicas

| Feature | Redis Replica |
|---------|--------------|
| Max replicas per shard | 5 (Cluster Mode Disabled) or 5 per shard (Enabled) |
| Replication | Async |
| Cross-region | Not natively supported |
| Purpose | Read scaling + automatic failover source |
| Promotion | Automatic on primary failure |

---

### ⏱️ RPO / RTO Table

| Scenario | RPO | RTO |
|----------|-----|-----|
| Redis primary failure (with replica) | Data in cache at time of failure (no persistence) OR last RDB/AOF | ~30-60 sec (replica promotion) |
| Redis with AOF persistence | RPO = last AOF write (~1 sec) | 1-5 min (replay AOF) |
| Memcached node failure | All data on that node lost | Immediate (cache miss, rebuild from DB) |
| Full cluster failure | Depends on persistence config | Minutes (restore from RDB snapshot) |

> 🔑 **Exam key:** ElastiCache is a cache, not a database. Even with persistence, RPO/RTO are not as critical — the database is the source of truth. Cache loss = increased DB load, not data loss.

---

### 💾 Backup & Restore (Redis only)

- **RDB snapshots:** Point-in-time dump to S3; manual or automatic (daily)
- **AOF (Append-Only File):** Logs every write command; better durability; replay on restart
- **Backup retention:** 0–35 days
- **Restore:** Create new cluster from backup
- **Export to S3:** Export RDB file to S3 for analysis or cross-region restore

---

### 🔄 Migration

| Source | Method |
|--------|--------|
| **Self-managed Redis** | Redis `DUMP`/`RESTORE`, or Online Migration tool, or `SLAVEOF` replication |
| **ElastiCache Redis → ElastiCache Redis** | Backup + restore to new cluster |
| **Memcached → Redis** | Cold migration (data not persistent; restart with new cluster) |
| **AWS DMS** | Not a typical DMS use case; DMS is for relational migrations |

**Online Migration to ElastiCache:**
AWS provides an **Online Migration** feature for migrating self-managed Redis to ElastiCache — uses Redis replication to sync with minimal downtime.

---

### 🔗 Integration

| Service | Purpose |
|---------|---------|
| **RDS/Aurora** | Cache frequently read DB results (cache-aside pattern) |
| **DynamoDB + DAX** | DAX is DynamoDB-specific; ElastiCache Redis is general-purpose |
| **Lambda** | Session storage for serverless apps |
| **ElastiCache for Redis** | Session store, rate limiter, pub/sub, leaderboard |
| **CloudWatch** | Cache hit/miss ratio, evictions, memory usage metrics |
| **Secrets Manager** | Store Redis AUTH token |
| **VPC** | Private network isolation |

---

### ⚖️ Comparison

| Feature | ElastiCache Redis | ElastiCache Memcached | DynamoDB DAX | In-memory on EC2 |
|---------|------------------|----------------------|-------------|-----------------|
| Latency | Microseconds | Microseconds | Microseconds | Microseconds |
| Persistence | Yes (RDB/AOF) | No | No | No |
| Multi-AZ HA | Yes | No | Yes | Manual |
| Data structures | Rich | Strings only | DynamoDB types | Any |
| Max data | Limited by RAM | Limited by RAM | Configurable | Limited by RAM |
| Managed | Yes | Yes | Yes | No |
| Use with any DB | Yes | Yes | DynamoDB only | Yes |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max nodes per Memcached cluster | 40 |
| Max shards (Redis Cluster Mode Enabled) | 500 |
| Max replicas per shard | 5 |
| Max node memory | Depends on node type (up to ~340 GB for cache.r6g.16xlarge) |
| Cross-region replication | Not natively supported |
| Redis AUTH token length | 16-128 chars |
| Backup retention | 0-35 days |

---

### 🏛️ Well-Architected Framework

| Pillar | ElastiCache Considerations |
|--------|---------------------------|
| **Operational Excellence** | Monitor cache hit ratio (target >80%); set appropriate TTL; use CloudWatch alarms for evictions; use RBAC for operational access control |
| **Security** | Enable TLS + at-rest encryption; use RBAC (Redis 6+); deploy in private subnets; store AUTH token in Secrets Manager |
| **Reliability** | Use Redis Multi-AZ with automatic failover; design cache-aside pattern so DB is always the source of truth; test failover; set `maxmemory-policy` appropriately |
| **Performance Efficiency** | Choose right node size (memory-bound); use Cluster Mode Enabled for large datasets; monitor eviction rate; use connection pooling |
| **Cost Optimization** | Right-size nodes; use Reserved Nodes for production; Cluster Mode Enabled scales memory more efficiently than large single nodes |
| **Sustainability** | Right-size to avoid idle memory; use TTL to expire unused data (reduces memory waste); Graviton-based nodes available |

---

### 📐 4 Exam Domains

**Domain 1 — Organizational Complexity:** Deploy ElastiCache in private subnets; use RAM sharing is NOT supported for ElastiCache — deploy per account; use CloudWatch cross-account for monitoring.

**Domain 2 — Design for New Solutions:** Session store for stateless web apps → Redis; real-time leaderboard → Redis sorted sets; rate limiting → Redis atomic operations; pub/sub messaging → Redis Pub/Sub.

**Domain 3 — Continuous Improvement:** Enable Multi-AZ if single-node; increase replica count for read scaling; enable AOF for better cache durability; monitor hit ratio and increase cache size if low.

**Domain 4 — Migration:** Migrate self-managed Redis to ElastiCache with Online Migration (minimal downtime). Memcached → ElastiCache Memcached (simple endpoint swap). Consider replacing Memcached with Redis for HA requirements.


---

## 📄 Chapter 14: Amazon DocumentDB

### 📖 Overview

**Amazon DocumentDB** (with MongoDB compatibility) is AWS's fully managed document database service. It is designed for JSON document workloads with MongoDB-compatible APIs — applications using MongoDB drivers, tools, and queries can largely work with DocumentDB without code changes.

> 🔑 **Exam key:** DocumentDB is NOT MongoDB — it is a MongoDB-compatible API layer over a purpose-built AWS storage engine (Aurora-like distributed storage). Not all MongoDB features are supported. Check the compatibility matrix for advanced features.

**Real-life analogy:** DocumentDB is to MongoDB what Aurora is to MySQL/PostgreSQL — same API/wire protocol, different underlying storage engine optimized for AWS.

**Scope:** **Regional** service. Multi-AZ supported via cluster replicas.

**Open-source equivalent:** MongoDB.

---

### 🏗️ Architecture

DocumentDB uses the **same distributed storage architecture as Aurora** — 6 copies across 3 AZs, storage auto-grows, quorum-based.

```mermaid
graph TB
    subgraph "DocumentDB Cluster"
        P["✏️ Primary Instance\n(Reads + Writes)"]
        R1["📖 Replica 1\n(AZ-b)"]
        R2["📖 Replica 2\n(AZ-c)"]
    end
    subgraph "Distributed Storage"
        S1["💾 AZ-a (2 copies)"]
        S2["💾 AZ-b (2 copies)"]
        S3["💾 AZ-c (2 copies)"]
    end
    P --> S1 & S2 & S3
    R1 --> S2 & S3
    R2 --> S1 & S3
    App["🌐 MongoDB Driver"] -->|"Port 27017"| P
```

---

### 🔒 Connectivity & Security

- Deployed in a **VPC**; private subnets recommended; port **27017**
- **Security Groups** for access control
- **TLS in-transit:** Always enabled; cannot disable; use MongoDB TLS connection strings
- **At-rest encryption:** AES-256 via KMS; enable at creation
- **IAM Authentication:** Supported — IAM roles generate short-lived passwords
- **MongoDB connection string:** `mongodb://user:pass@cluster-endpoint:27017/?tls=true`

---

### 🔐 Encryption

| Layer | Details |
|-------|---------|
| At-rest | AES-256 KMS (CMK supported); enable at creation only |
| In-transit | TLS always enforced (cannot disable) |
| Snapshots | Inherit cluster key |

---

### 📈 Scalability & HA

- Up to **15 read replicas** per cluster (same as Aurora/Neptune)
- Storage auto-grows in 10 GB increments up to **64 TiB**
- Vertical scaling: change instance class
- No serverless option (unlike Aurora Serverless v2)

---

### 🏢 Multi-AZ

Same model as Aurora:
- Storage: always 6 copies / 3 AZs
- Compute HA: add replicas in different AZs
- Failover: ~30 sec (DNS flip to promoted replica)
- Replicas serve reads (unlike RDS standby)

---

### 📖 Read Replicas

- Up to 15 replicas; millisecond lag (shared storage); cross-region NOT natively supported
- Promotion: ~30 sec on primary failure

---

### ⏱️ RPO / RTO

| Scenario | RPO | RTO |
|----------|-----|-----|
| AZ failure (with replica) | ~0 | ~30 sec |
| Region failure | Hours (last snapshot) | 30-60 min |
| PITR | Up to 35 days | 20-40 min |

---

### 💾 Backup & Restore

- Automated backups: 1–35 day retention; PITR supported (restores to new cluster)
- Manual snapshots: indefinite retention; cross-account sharing; cross-region copy
- No export to S3 (unlike DynamoDB)

---

### 🔄 Migration

| Source | Method |
|--------|--------|
| **MongoDB (self-managed)** | `mongodump` / `mongorestore`; AWS DMS |
| **MongoDB Atlas** | AWS DMS with MongoDB source |
| **AWS DMS** | Supports MongoDB as source, DocumentDB as target; Full Load + CDC |

> 🔑 **Exam key:** DMS supports MongoDB → DocumentDB migration with CDC for minimal downtime. Use `mongodump`/`mongorestore` for offline migration of small datasets.

---

### ⚖️ Comparison

| Feature | DocumentDB | MongoDB Atlas | DynamoDB |
|---------|------------|--------------|----------|
| API | MongoDB 4.0/5.0 compat | Native MongoDB | DynamoDB API |
| Data model | Document (BSON/JSON) | Document | Key-value/document |
| Storage engine | Aurora-like distributed | WiredTiger | AWS proprietary |
| Managed | Fully (AWS) | Fully (Atlas SaaS) | Fully (AWS) |
| Serverless | No | Yes (Atlas Serverless) | Yes (On-Demand) |
| Cross-region | Snapshot copy only | Atlas Global Clusters | Global Tables |
| MongoDB feature compat | ~80-90% | 100% | N/A |

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max storage | 64 TiB |
| Max replicas | 15 |
| Max document size | 16 MB |
| Max collections per database | No hard limit |
| Transactions | MongoDB 4.0-style multi-document ACID |
| MongoDB version compatibility | 3.6, 4.0, 5.0 APIs |
| Not supported | Change Streams (limited), Atlas Search, MongoDB 6.0+ features |

---

### 🏛️ Well-Architected Framework

| Pillar | DocumentDB Considerations |
|--------|--------------------------|
| **Operational Excellence** | Monitor via CloudWatch; use Performance Insights (if available); use profiler for slow query analysis |
| **Security** | CMK encryption at creation; TLS always on; IAM auth; private subnets; CloudTrail |
| **Reliability** | Replicas in different AZs; PITR enabled; automated snapshots; test failover |
| **Performance Efficiency** | Index key query fields; use read replicas for read scaling; choose right instance class |
| **Cost Optimization** | Right-size instances; reserved instances for production; stop dev clusters when not in use |
| **Sustainability** | Shared storage avoids per-replica duplication; right-size compute |

---

### 📐 4 Exam Domains

**Domain 1:** Cross-account snapshot sharing for DR; VPC + SG controls; CloudTrail audit.

**Domain 2:** JSON document workloads with MongoDB compatibility → DocumentDB. Content management, catalogs, user profiles. MongoDB-compatible API means minimal code changes.

**Domain 3:** Migrate from single-instance MongoDB to DocumentDB cluster for HA; add replicas for read scaling; enable PITR.

**Domain 4:** Self-managed MongoDB → DocumentDB via DMS (Full Load + CDC). Minimal downtime migration for MongoDB-based applications moving to AWS.


---

## 📒 Chapter 15: Amazon QLDB

### 📖 Overview

**Amazon Quantum Ledger Database (QLDB)** is a fully managed ledger database that provides a **cryptographically verifiable, immutable transaction log**. Every change to data is recorded permanently with a cryptographic hash chain — you can prove that data has not been altered since it was written.

> 🔑 **Exam key:** QLDB = immutable ledger with cryptographic verification. Use when you need an audit trail that CANNOT be falsified — financial transactions, supply chain records, medical records. NOT a general-purpose database. NOT blockchain (no decentralized consensus).

**Real-life analogy:** QLDB is like a bank's accounting ledger — every transaction is written in permanent ink, each entry references the previous one, and the entire history can be mathematically verified. You can see any balance at any point in history.

**Scope:** **Regional** service. No Multi-AZ option (AWS manages HA internally). No cross-region replication natively.

**Comparison:** Unlike blockchain (Managed Blockchain), QLDB is **centralized** — owned by you, trusted by you. No distributed consensus. Much simpler and faster.

---

### 🏗️ Architecture

```mermaid
graph TB
    subgraph "QLDB Ledger"
        J["📋 Journal\n(Immutable append-only\ncryptographic hash chain)"]
        S["🗃️ Current State\n(Queryable via PartiQL)"]
        J -->|"Derived view"| S
    end
    App["🌐 Application\n(PartiQL queries)"] -->|"Reads + Writes"| S
    App -->|"Verify history"| J
    Export["📤 Export to S3\n(for analysis)"] --> J
```

**Key concepts:**
- **Journal:** Append-only, immutable sequence of all committed transactions; organized into blocks; each block has a hash of the previous block (blockchain-like chain)
- **Ledger:** The top-level QLDB resource; contains tables
- **Tables:** Store document data (JSON-like ION format)
- **Current state:** Queryable current snapshot of all tables; derived from journal
- **Digest:** Cryptographic proof (SHA-256 hash) of the journal at a point in time; used to verify integrity
- **Verification:** Prove any document revision has not been altered using the digest + hash chain

---

### 🔑 PartiQL Query Language

QLDB uses **PartiQL** — an SQL-compatible query language that works with semi-structured (JSON/ION) data.

```sql
-- Query current state
SELECT * FROM Vehicles WHERE VIN = 'KM8J3CA46JU622498'

-- Query history (all revisions)
SELECT * FROM history(Vehicles) AS h
WHERE h.metadata.id = 'doc-id-here'

-- Insert (creates new revision in journal)
INSERT INTO Vehicles VALUE {
    'VIN': 'KM8J3CA46JU622498',
    'Type': 'Sedan',
    'Year': 2024
}
```

---

### 🔒 Connectivity & Security

- QLDB is **not deployed in a VPC** — it is a public AWS endpoint service
- Access via **HTTPS endpoint** + IAM authentication (no VPC endpoint natively for most operations)
- **IAM policies** control all access (no database-level users)
- **Encryption at-rest:** AES-256 via KMS; AWS-managed key default; CMK supported
- **Encryption in-transit:** TLS enforced

> ⚠️ **QLDB is NOT inside your VPC** — this is a key exam gotcha. Access is via HTTPS public endpoint controlled by IAM. VPC Endpoints for QLDB are available in some regions (interface endpoint).

---

### 🔐 Encryption Deep-Dive

| Layer | Details |
|-------|---------|
| At-rest | AES-256 KMS; AWS-managed or CMK; set at ledger creation |
| In-transit | TLS enforced |
| Journal integrity | SHA-256 hash chain; independent of KMS |
| Verification | Digest + proof — mathematically verifiable without decrypting |

> 🔑 **Exam key:** QLDB's cryptographic integrity is separate from KMS encryption. Even if KMS key is compromised, the hash chain integrity verification tells you if data was tampered with. These are two different security properties.

---

### 📈 Scalability & HA

- **Serverless:** No capacity planning; scales automatically
- **HA:** AWS manages — QLDB stores journal across multiple AZs internally (not user-configurable)
- **No Multi-AZ configuration option** — AWS handles it transparently
- **No read replicas** — single endpoint; AWS scales internally

---

### 🏢 Multi-AZ

QLDB is **serverless and fully managed** — AWS handles AZ redundancy internally. There is no Multi-AZ toggle, no standby to configure, no replica to add. AWS SLA covers AZ-level resilience.

---

### 📖 Read Replicas

QLDB has **no user-configurable read replicas**. Scaling is handled automatically by AWS. For read-heavy workloads, export journal to S3 and query with Athena.

---

### ⏱️ RPO / RTO

| Scenario | RPO | RTO |
|----------|-----|-----|
| AZ failure | ~0 (AWS-managed) | Seconds–minutes (AWS managed) |
| Region failure | Last export to S3 | Manual rebuild from export |
| Accidental wrong write | N/A (immutable — cannot delete from journal) | N/A |

> 🔑 **Exam key:** You CANNOT delete data from the QLDB journal. You can drop a table (removes current state view) but all journal entries remain. Immutability is the core design guarantee.

---

### 💾 Backup & Restore

- **No traditional backup/restore** — the journal IS the backup (immutable, complete history)
- **Journal export to S3:** Export journal blocks to S3 in ION or JSON format; use Athena or Spark to analyze; use for cross-region DR
- **Ledger deletion:** Destroying the ledger destroys everything (ask for confirmation)

---

### 🔄 Migration

| Source | Method |
|--------|--------|
| **Relational DB (audit tables)** | Export historical data → import to QLDB tables; application layer migration |
| **Custom audit log** | Write new records to QLDB via application; backfill via bulk import |
| **Another ledger system** | Custom ETL via S3 |
| **AWS DMS** | Not typically used for QLDB; no native DMS support |

---

### 🔗 Integration

| Service | Purpose |
|---------|---------|
| **S3** | Journal export for analysis/DR |
| **Athena** | Query exported journal data |
| **Lambda** | Trigger Lambda on QLDB Streams (document changes) |
| **Kinesis Data Streams** | QLDB Streams → Kinesis → Lambda/Firehose |
| **IAM** | All authentication/authorization |
| **CloudTrail** | API audit |
| **CloudWatch** | Ledger metrics |

---

### ⚖️ Comparison

| Feature | QLDB | Managed Blockchain | RDS (audit tables) | DynamoDB + Streams |
|---------|------|-------------------|-------------------|-------------------|
| Immutability | Cryptographic guarantee | Distributed consensus | No (mutable) | No (events only) |
| Verification | Cryptographic digest | On-chain | No | No |
| Decentralized | No (centralized) | Yes | No | No |
| Query language | PartiQL (SQL-like) | Smart contracts | SQL | DynamoDB API |
| Performance | High (single-owner) | Low (consensus overhead) | High | High |
| Use case | Financial ledger, audit | Multi-party trustless | Compliance logging | Change events |
| Managed | Fully | Fully | Manually managed | Fully |

> 🔑 **Exam key:** QLDB vs Managed Blockchain: QLDB = centralized, trusted single-owner ledger (company owns and trusts their own data). Managed Blockchain = decentralized, multiple parties who don't trust each other. If you trust yourself = QLDB. If multiple organizations need to agree = Managed Blockchain.

---

### ⚠️ Limitations

| Limit | Value |
|-------|-------|
| Max ledger size | No documented limit |
| Max document size | 128 KB |
| Max tables per ledger | 20 |
| Transactions per second | Up to 8,000 (auto-scaled) |
| Cross-region replication | Not natively supported |
| Data deletion | Cannot delete journal entries |
| VPC | No native VPC deployment (public endpoint) |
| DMS support | Not a DMS source/target |

---

### 🏛️ Well-Architected Framework

| Pillar | QLDB Considerations |
|--------|---------------------|
| **Operational Excellence** | Use QLDB Streams to integrate with downstream systems; export journal to S3 for long-term retention; monitor via CloudWatch |
| **Security** | IAM-only authentication (no DB users); CMK for at-rest encryption; TLS enforced; CloudTrail for API audit; note public endpoint (no VPC) |
| **Reliability** | AWS manages AZ resilience; export journal to S3 for cross-region DR; test verification procedures |
| **Performance Efficiency** | Serverless — no capacity planning; use PartiQL indexes for query performance; export to S3+Athena for analytics (don't scan QLDB for analytics) |
| **Cost Optimization** | Pay per I/O + storage; export old journal to S3 (cheaper storage); use Athena for analytics on exports |
| **Sustainability** | Serverless scales to zero; no idle compute cost |

---

### 📐 4 Exam Domains

**Domain 1 — Organizational Complexity:** IAM controls all QLDB access — integrate with AWS Organizations SCPs to restrict ledger access. CloudTrail captures all API operations for cross-account governance.

**Domain 2 — Design for New Solutions:** Financial transactions (banking credits/debits), supply chain provenance, medical record audit trails, regulatory compliance ledgers → QLDB. Need cryptographic proof of data integrity → QLDB. Multiple untrusted parties → Managed Blockchain instead.

**Domain 3 — Continuous Improvement:** Enable QLDB Streams → Kinesis for real-time downstream processing. Export journal to S3 + Athena for analytics. Add CMK if using AWS-managed key.

**Domain 4 — Migration:** Legacy audit-log tables in RDS → QLDB. Write new audit events to QLDB; backfill historical data via bulk import. No DMS support — custom ETL required.

---


---

