# 📚 AWS SAP-C02 — Księga Nauki

> Generowane przez Claude | Aktualizowane na bieżąco | Egzamin: AWS Certified Solutions Architect – Professional

---

## 📋 Spis Treści

- [📚 AWS SAP-C02 — Księga Nauki](#-aws-sap-c02--księga-nauki)
  - [📋 Spis Treści](#-spis-treści)
  - [1. AWS Directory Services, Identity Federation, IAM Identity Center](#1-aws-directory-services-identity-federation-iam-identity-center)
    - [1.1 Przegląd i kontekst](#11-przegląd-i-kontekst)
    - [1.2 AWS Managed Microsoft AD](#12-aws-managed-microsoft-ad)
    - [1.3 Simple AD](#13-simple-ad)
    - [1.4 AD Connector](#14-ad-connector)
    - [1.5 Identity Federation](#15-identity-federation)
      - [SAML 2.0 Federation — przepływ](#saml-20-federation--przepływ)
      - [SAML Federation dla AWS Console](#saml-federation-dla-aws-console)
      - [Web Identity Federation (Cognito)](#web-identity-federation-cognito)
    - [1.6 IAM Identity Center (SSO)](#16-iam-identity-center-sso)
    - [1.7 Porównanie usług](#17-porównanie-usług)
    - [1.8 4 Domeny egzaminacyjne](#18-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization)
  - [2. RADIUS, Amazon Cognito i AWS STS](#2-radius-amazon-cognito-i-aws-sts)
    - [2.1 RADIUS — protokół uwierzytelniania sieciowego](#21-radius--protokół-uwierzytelniania-sieciowego)
    - [2.2 Amazon Cognito — przegląd](#22-amazon-cognito--przegląd)
    - [2.3 Cognito User Pools](#23-cognito-user-pools)
    - [2.4 Cognito Identity Pools](#24-cognito-identity-pools)
    - [2.5 User Pools vs Identity Pools — porównanie](#25-user-pools-vs-identity-pools--porównanie)
    - [2.6 AWS STS — Security Token Service](#26-aws-sts--security-token-service)
    - [2.7 STS API — porównanie wywołań](#27-sts-api--porównanie-wywołań)
    - [2.8 4 Domeny egzaminacyjne](#28-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-1)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-1)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-1)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-1)
  - [3. VPC, Subnets, Route Tables, Security Groups](#3-vpc-subnets-route-tables-security-groups)
    - [3.1 VPC — Virtual Private Cloud](#31-vpc--virtual-private-cloud)
    - [3.2 Subnets — podsieci](#32-subnets--podsieci)
    - [3.3 Route Tables](#33-route-tables)
    - [3.4 Internet Gateway i NAT](#34-internet-gateway-i-nat)
    - [3.5 Security Groups](#35-security-groups)
    - [3.6 Network ACL (NACL)](#36-network-acl-nacl)
    - [3.7 Security Group vs NACL — porównanie](#37-security-group-vs-nacl--porównanie)
    - [3.8 VPC Flow Logs](#38-vpc-flow-logs)
    - [3.9 4 Domeny egzaminacyjne](#39-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-2)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-2)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-2)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-2)
  - [4. Public Subnet, IGW, NAT i alternatywne metody dostępu do Internetu](#4-public-subnet-igw-nat-i-alternatywne-metody-dostępu-do-internetu)
    - [4.1 Public Subnet — czy każda maszyna musi mieć publiczne IP?](#41-public-subnet--czy-każda-maszyna-musi-mieć-publiczne-ip)
    - [4.2 Internet Gateway — szczegóły](#42-internet-gateway--szczegóły)
    - [4.3 NAT Gateway — szczegóły](#43-nat-gateway--szczegóły)
    - [4.4 NAT Instance (legacy)](#44-nat-instance-legacy)
    - [4.5 Inne rozwiązania dostępu do Internetu i usług AWS](#45-inne-rozwiązania-dostępu-do-internetu-i-usług-aws)
      - [AWS PrivateLink (VPC Interface Endpoint)](#aws-privatelink-vpc-interface-endpoint)
      - [VPC Gateway Endpoint](#vpc-gateway-endpoint)
      - [AWS VPN (Site-to-Site)](#aws-vpn-site-to-site)
      - [AWS Direct Connect](#aws-direct-connect)
      - [AWS Transit Gateway (TGW)](#aws-transit-gateway-tgw)
      - [AWS VPN CloudHub](#aws-vpn-cloudhub)
      - [AWS Client VPN](#aws-client-vpn)
    - [4.6 VPC Endpoints — Gateway i Interface](#46-vpc-endpoints--gateway-i-interface)
    - [4.7 Porównanie wszystkich rozwiązań](#47-porównanie-wszystkich-rozwiązań)
    - [4.8 4 Domeny egzaminacyjne](#48-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-3)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-3)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-3)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-3)
  - [5. VPC Router, DNS, Route Tables, Routing wewnątrz VPC i między VPC](#5-vpc-router-dns-route-tables-routing-wewnątrz-vpc-i-między-vpc)
    - [5.1 VPC Router — czym jest i jak działa](#51-vpc-router--czym-jest-i-jak-działa)
    - [5.2 VPC DNS Server (Route 53 Resolver)](#52-vpc-dns-server-route-53-resolver)
    - [5.3 Route Tables — Main vs Custom, Association](#53-route-tables--main-vs-custom-association)
    - [5.4 Local Route — fundament routingu VPC](#54-local-route--fundament-routingu-vpc)
    - [5.5 Routing wewnątrz VPC krok po kroku](#55-routing-wewnątrz-vpc-krok-po-kroku)
    - [5.6 VPC Peering — routing między VPC](#56-vpc-peering--routing-między-vpc)
    - [5.7 Cross-AZ i Cross-Region transfer — koszty](#57-cross-az-i-cross-region-transfer--koszty)
    - [5.8 VPC Router vs Internet Gateway — porównanie](#58-vpc-router-vs-internet-gateway--porównanie)
    - [5.9 4 Domeny egzaminacyjne](#59-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-4)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-4)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-4)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-4)
  - [6. Zaawansowane pytania o VPC](#6-zaawansowane-pytania-o-vpc)
    - [6.1 Ile Route Tables ma subnet?](#61-ile-route-tables-ma-subnet)
    - [6.2 Czy każda VM w public subnet musi mieć publiczny IP?](#62-czy-każda-vm-w-public-subnet-musi-mieć-publiczny-ip)
    - [6.3 Source/Destination Check w NAT Instance](#63-sourcedestination-check-w-nat-instance)
    - [6.4 VPN CloudHub](#64-vpn-cloudhub)
    - [6.5 Private Subnets — jak działają szczegółowo](#65-private-subnets--jak-działają-szczegółowo)
    - [6.6 Dual Stack — IPv4 + IPv6 w VPC](#66-dual-stack--ipv4--ipv6-w-vpc)
    - [6.7 Delegated Subnets — odpowiednik Azure w AWS](#67-delegated-subnets--odpowiednik-azure-w-aws)
      - [AWS VPC Subnet Reservations (dla AWS services):](#aws-vpc-subnet-reservations-dla-aws-services)
      - [Network Firewall Subnet — najsilniejszy odpowiednik:](#network-firewall-subnet--najsilniejszy-odpowiednik)
      - [AWS RAM (Resource Access Manager) — subnet sharing:](#aws-ram-resource-access-manager--subnet-sharing)
    - [6.8 PrivateLink i VPC Endpoints vs Azure Private Link i Service Endpoints](#68-privatelink-i-vpc-endpoints-vs-azure-private-link-i-service-endpoints)
    - [6.9 Czy subnet jest regionalny?](#69-czy-subnet-jest-regionalny)
    - [6.10 4 Domeny egzaminacyjne](#610-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-5)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-5)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-5)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-5)
  - [7. AWS ARM — Graviton i Architektura ARM w EC2](#7-aws-arm--graviton-i-architektura-arm-w-ec2)
    - [7.1 Co to jest ARM i Graviton?](#71-co-to-jest-arm-i-graviton)
      - [Analogia z życia codziennego](#analogia-z-życia-codziennego)
      - [Analogia Linux/Open-source](#analogia-linuxopen-source)
      - [Analogia Azure](#analogia-azure)
    - [7.2 Generacje Graviton](#72-generacje-graviton)
      - [Zasięg geograficzny](#zasięg-geograficzny)
    - [7.3 Typy instancji EC2 ARM (rodziny g-suffix)](#73-typy-instancji-ec2-arm-rodziny-g-suffix)
      - [Nomeklatura EC2 — jak czytać?](#nomeklatura-ec2--jak-czytać)
      - [Rozmiary instancji — co oznaczają?](#rozmiary-instancji--co-oznaczają)
    - [7.4 Graviton vs x86 — porównanie szczegółowe](#74-graviton-vs-x86--porównanie-szczegółowe)
      - [Koszt](#koszt)
      - [Wydajność](#wydajność)
      - [Co działa lepiej na Graviton?](#co-działa-lepiej-na-graviton)
      - [Co może nie działać lub wymaga pracy?](#co-może-nie-działać-lub-wymaga-pracy)
    - [7.5 Kiedy używać Graviton, kiedy x86?](#75-kiedy-używać-graviton-kiedy-x86)
      - [Tabela decyzyjna](#tabela-decyzyjna)
    - [7.6 Usługi AWS wspierające Graviton](#76-usługi-aws-wspierające-graviton)
      - [Lambda z Graviton2 — szczegóły](#lambda-z-graviton2--szczegóły)
      - [RDS na Graviton](#rds-na-graviton)
    - [7.7 Migracja z x86 na ARM — strategie](#77-migracja-z-x86-na-arm--strategie)
      - [Strategia 1: Lift \& Shift (kontenery)](#strategia-1-lift--shift-kontenery)
      - [Strategia 2: EC2 — zmiana AMI i instance type](#strategia-2-ec2--zmiana-ami-i-instance-type)
      - [Strategia 3: Lambda — zmiana architektury](#strategia-3-lambda--zmiana-architektury)
      - [Strategia 4: Testy kompatybilności](#strategia-4-testy-kompatybilności)
      - [Narzędzia pomocne przy migracji](#narzędzia-pomocne-przy-migracji)
    - [7.8 Bezpieczeństwo i niezawodność na Graviton](#78-bezpieczeństwo-i-niezawodność-na-graviton)
      - [Bezpieczeństwo wbudowane w Graviton](#bezpieczeństwo-wbudowane-w-graviton)
      - [Nitro System — kluczowy dla bezpieczeństwa](#nitro-system--kluczowy-dla-bezpieczeństwa)
      - [Niezawodność — Multi-AZ](#niezawodność--multi-az)
      - [Auto Scaling — Mixed Instance Policy (x86 + ARM)](#auto-scaling--mixed-instance-policy-x86--arm)
    - [7.9 Graviton w kontekście Well-Architected Framework](#79-graviton-w-kontekście-well-architected-framework)
    - [7.10 4 Domeny egzaminacyjne](#710-4-domeny-egzaminacyjne)
      - [🏢 Domain 1: Design Solutions for Organizational Complexity](#-domain-1-design-solutions-for-organizational-complexity)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-6)
      - [🔧 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-6)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-6)
  - [8. DHCP Option Sets, NACLs, Security Groups, VPC Peering, Elastic IP, NAT Gateway, Transit VPC, EC2 Router](#8-dhcp-option-sets-nacls-security-groups-vpc-peering-elastic-ip-nat-gateway-transit-vpc-ec2-router)
    - [8.1 DHCP Option Sets](#81-dhcp-option-sets)
    - [8.2 Network ACL (NACL) — szczegóły zaawansowane](#82-network-acl-nacl--szczegóły-zaawansowane)
    - [8.3 Security Groups — szczegóły zaawansowane](#83-security-groups--szczegóły-zaawansowane)
    - [8.4 NACL vs Security Group — porównanie i kontekst Azure/ARM](#84-nacl-vs-security-group--porównanie-i-kontekst-azurearm)
    - [8.5 VPC Peering — zaawansowane](#85-vpc-peering--zaawansowane)
    - [8.6 Elastic IP (EIP)](#86-elastic-ip-eip)
    - [8.7 NAT Gateway — szczegóły, koszty, security context](#87-nat-gateway--szczegóły-koszty-security-context)
    - [8.8 Private NAT Gateway](#88-private-nat-gateway)
    - [8.9 Transit VPC (legacy) i Transit Gateway](#89-transit-vpc-legacy-i-transit-gateway)
    - [8.10 EC2 jako router — jak EC2 routuje ruch](#810-ec2-jako-router--jak-ec2-routuje-ruch)
    - [8.11 ARM — Resource Access Manager, sharing VPC resources](#811-arm--resource-access-manager-sharing-vpc-resources)
    - [8.12 Well-Architected Framework](#812-well-architected-framework)
    - [8.13 4 Domeny egzaminacyjne](#813-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-6)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-7)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-7)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-7)
  - [9. Virtual Private Gateway, Transit VPC, VPC Endpoints, AWS PrivateLink, Interface Endpoints, Gateway Endpoints](#9-virtual-private-gateway-transit-vpc-vpc-endpoints-aws-privatelink-interface-endpoints-gateway-endpoints)
    - [9.1 Virtual Private Gateway (VGW)](#91-virtual-private-gateway-vgw)
    - [9.2 Transit VPC — architektura (szerzej)](#92-transit-vpc--architektura-szerzej)
    - [9.3 VPC Gateway Endpoint](#93-vpc-gateway-endpoint)
    - [9.4 Interface Endpoint (VPC Endpoint — Interface Type)](#94-interface-endpoint-vpc-endpoint--interface-type)
    - [9.5 Jak zbudowany jest AWS PrivateLink](#95-jak-zbudowany-jest-aws-privatelink)
    - [9.6 Gateway Endpoint vs Interface Endpoint — pełne porównanie](#96-gateway-endpoint-vs-interface-endpoint--pełne-porównanie)
    - [9.7 Porównanie z Azure: Private Endpoints i Service Endpoints](#97-porównanie-z-azure-private-endpoints-i-service-endpoints)
    - [9.8 Well-Architected Framework](#98-well-architected-framework)
    - [9.9 4 Domeny egzaminacyjne](#99-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-7)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-8)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-8)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-8)
  - [10. DNS (Regional/Zonal), Private DNS, Site-to-Site VPN, CGW, NAT-T, IPSec, Client VPN, CloudHub, BGP, VPN Failover](#10-dns-regionalzonal-private-dns-site-to-site-vpn-cgw-nat-t-ipsec-client-vpn-cloudhub-bgp-vpn-failover)
    - [10.1 Regional i Zonal DNS entries — Route 53 i VPC DNS](#101-regional-i-zonal-dns-entries--route-53-i-vpc-dns)
    - [10.2 Private DNS i Private DNS Resolution](#102-private-dns-i-private-dns-resolution)
    - [10.3 Protokoły VPN — podstawy](#103-protokoły-vpn--podstawy)
      - [IPSec — szczegółowo](#ipsec--szczegółowo)
      - [NAT-T (NAT Traversal)](#nat-t-nat-traversal)
      - [OpenVPN](#openvpn)
      - [WireGuard](#wireguard)
    - [10.4 Site-to-Site VPN — AWS](#104-site-to-site-vpn--aws)
    - [10.5 BGP i VPN — Route Propagation](#105-bgp-i-vpn--route-propagation)
    - [10.6 VPN Failover](#106-vpn-failover)
    - [10.7 VPN CloudHub](#107-vpn-cloudhub)
    - [10.8 Transitive Routing i VPN](#108-transitive-routing-i-vpn)
    - [10.9 AWS Client VPN](#109-aws-client-vpn)
    - [10.10 Porównanie rozwiązań VPN w AWS](#1010-porównanie-rozwiązań-vpn-w-aws)
    - [10.11 Well-Architected Framework](#1011-well-architected-framework)
    - [10.12 4 Domeny egzaminacyjne](#1012-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Design Solutions for Organizational Complexity](#️-domain-1-design-solutions-for-organizational-complexity-8)
      - [🆕 Domain 2: Design for New Solutions](#-domain-2-design-for-new-solutions-9)
      - [📈 Domain 3: Continuous Improvement for Existing Solutions](#-domain-3-continuous-improvement-for-existing-solutions-9)
      - [🚀 Domain 4: Accelerate Workload Migration and Modernization](#-domain-4-accelerate-workload-migration-and-modernization-9)
  - [11. Hyperplane, Direct Connect, DX Gateway, Transit Gateway Deep Dive, TGW vs Azure Virtual Hub](#11-hyperplane-direct-connect-dx-gateway-transit-gateway-deep-dive-tgw-vs-azure-virtual-hub)
    - [11.1 Hyperplane — AWS Internal Network Fabric](#111-hyperplane--aws-internal-network-fabric)
    - [11.2 Direct Connect — Fizyczna Infrastruktura](#112-direct-connect--fizyczna-infrastruktura)
      - [Last Mile](#last-mile)
      - [X-Connect (Cross-Connect)](#x-connect-cross-connect)
      - [DX Connection](#dx-connection)
    - [11.3 Virtual Interface (VIF)](#113-virtual-interface-vif)
    - [11.4 Direct Connect Gateway (DX Gateway)](#114-direct-connect-gateway-dx-gateway)
    - [11.5 Transit Gateway (TGW) — Deep Dive](#115-transit-gateway-tgw--deep-dive)
      - [TGW Attachment](#tgw-attachment)
    - [11.6 TGW Route Tables — Segmentacja](#116-tgw-route-tables--segmentacja)
    - [11.7 TGW Peering](#117-tgw-peering)
    - [11.8 TGW i Resource Access Manager (RAM)](#118-tgw-i-resource-access-manager-ram)
    - [11.9 Multicast w VPC (TGW)](#119-multicast-w-vpc-tgw)
    - [11.10 AWS Transit Gateway vs Azure Virtual WAN / Virtual Hub](#1110-aws-transit-gateway-vs-azure-virtual-wan--virtual-hub)
      - [Architektura porównawcza](#architektura-porównawcza)
      - [Szczegółowe porównanie Route Management](#szczegółowe-porównanie-route-management)
    - [11.11 Well-Architected Framework](#1111-well-architected-framework)
    - [11.12 4 Domeny egzaminacyjne](#1112-4-domeny-egzaminacyjne)
      - [🏗️ Domain 1: Organizational Complexity](#️-domain-1-organizational-complexity)
      - [🆕 Domain 2: New Solutions](#-domain-2-new-solutions)
      - [📈 Domain 3: Continuous Improvement](#-domain-3-continuous-improvement)
      - [🚀 Domain 4: Migration and Modernization](#-domain-4-migration-and-modernization)
  - [12. VPC — Architektura, VPC Peering i Rozwiązania VPN w AWS](#12-vpc--architektura-vpc-peering-i-rozwiązania-vpn-w-aws)
    - [12.1 VPC — Fundament Sieci AWS](#121-vpc--fundament-sieci-aws)
    - [Co to jest VPC?](#co-to-jest-vpc)
    - [Kluczowe właściwości VPC](#kluczowe-właściwości-vpc)
    - [Komponenty VPC — mapa zależności](#komponenty-vpc--mapa-zależności)
    - [Secondary CIDR Blocks](#secondary-cidr-blocks)
    - [12.2 VPC Peering — Połączenie Bezpośrednie między VPC](#122-vpc-peering--połączenie-bezpośrednie-między-vpc)
    - [Czym jest VPC Peering?](#czym-jest-vpc-peering)
    - [Kluczowe cechy i ograniczenia](#kluczowe-cechy-i-ograniczenia)
    - [Non-Transitive Routing — dlaczego VPC Peering nie jest "hub"](#non-transitive-routing--dlaczego-vpc-peering-nie-jest-hub)
    - [Konfiguracja VPC Peering krok po kroku](#konfiguracja-vpc-peering-krok-po-kroku)
    - [VPC Peering — Security Groups cross-VPC](#vpc-peering--security-groups-cross-vpc)
    - [Skalowanie: Problem N\*(N-1)/2](#skalowanie-problem-nn-12)
    - [VPC Peering vs Azure VNet Peering](#vpc-peering-vs-azure-vnet-peering)
    - [12.3 AWS Site-to-Site VPN](#123-aws-site-to-site-vpn)
    - [Czym jest Site-to-Site VPN?](#czym-jest-site-to-site-vpn)
    - [Architektura](#architektura)
    - [Komponenty](#komponenty)
    - [BGP vs Static Routing](#bgp-vs-static-routing)
    - [Route Propagation do VPC Route Table](#route-propagation-do-vpc-route-table)
    - [HA i Redundancja VPN](#ha-i-redundancja-vpn)
    - [VPN przez Transit Gateway zamiast VGW](#vpn-przez-transit-gateway-zamiast-vgw)
    - [Parametry IPsec](#parametry-ipsec)
    - [Koszty Site-to-Site VPN](#koszty-site-to-site-vpn)
    - [12.4 AWS Client VPN](#124-aws-client-vpn)
    - [Czym jest Client VPN?](#czym-jest-client-vpn)
    - [Architektura](#architektura-1)
    - [Konfiguracja](#konfiguracja)
    - [Metody Autentykacji](#metody-autentykacji)
    - [Split Tunnel vs Full Tunnel](#split-tunnel-vs-full-tunnel)
    - [Koszty Client VPN](#koszty-client-vpn)
    - [12.5 VPN CloudHub](#125-vpn-cloudhub)
    - [Czym jest VPN CloudHub?](#czym-jest-vpn-cloudhub)
    - [Architektura CloudHub](#architektura-cloudhub)
    - [Konfiguracja (VGW-based)](#konfiguracja-vgw-based)
    - [CloudHub vs Transit Gateway (TGW)](#cloudhub-vs-transit-gateway-tgw)
    - [DX SiteLink — "CloudHub przez Direct Connect"](#dx-sitelink--cloudhub-przez-direct-connect)
    - [12.6 VPN i Direct Connect — Wybór i Porównanie](#126-vpn-i-direct-connect--wybór-i-porównanie)
    - [Decision Tree — VPN vs DX](#decision-tree--vpn-vs-dx)
    - [Pełne Porównanie](#pełne-porównanie)
    - [12.7 Well-Architected Framework — VPC, Peering, VPN](#127-well-architected-framework--vpc-peering-vpn)
    - [12.8 4 Domeny Egzaminacyjne SAP-C02](#128-4-domeny-egzaminacyjne-sap-c02)
    - [Domena 1: Design Solutions for Organizational Complexity](#domena-1-design-solutions-for-organizational-complexity)
    - [Domena 2: Design for New Solutions](#domena-2-design-for-new-solutions)
    - [Domena 3: Continuous Improvement for Existing Solutions](#domena-3-continuous-improvement-for-existing-solutions)
    - [Domena 4: Accelerate Workload Migration and Modernization](#domena-4-accelerate-workload-migration-and-modernization)
  - [13. Amazon EC2 — Rodziny Instancji, Zasoby, Koszty i Modele Zakupu](#13-amazon-ec2--rodziny-instancji-zasoby-koszty-i-modele-zakupu)
    - [13.1 Co to jest EC2 i z czego się składa?](#131-co-to-jest-ec2-i-z-czego-się-składa)
    - [Zasoby składowe każdej instancji EC2](#zasoby-składowe-każdej-instancji-ec2)
    - [13.2 Nomenklatura Instancji EC2 — Jak Czytać Nazwę](#132-nomenklatura-instancji-ec2--jak-czytać-nazwę)
    - [Sufiksy — co oznaczają litery po generacji](#sufiksy--co-oznaczają-litery-po-generacji)
    - [Rozmiary — skala mocy](#rozmiary--skala-mocy)
    - [13.3 Rodziny Instancji EC2 — Przegląd](#133-rodziny-instancji-ec2--przegląd)
    - [🟢 General Purpose (t, m)](#-general-purpose-t-m)
      - [Rodzina T — Burstable Performance](#rodzina-t--burstable-performance)
      - [Rodzina M — General Purpose (produkcja)](#rodzina-m--general-purpose-produkcja)
    - [🔵 Compute Optimized (c)](#-compute-optimized-c)
    - [🔴 Memory Optimized (r, x, u, z)](#-memory-optimized-r-x-u-z)
      - [Rodzina R — Memory Optimized (most popular)](#rodzina-r--memory-optimized-most-popular)
      - [Rodzina X — High Memory](#rodzina-x--high-memory)
      - [Rodzina U — High Memory (bare metal)](#rodzina-u--high-memory-bare-metal)
      - [Rodzina Z — High Frequency](#rodzina-z--high-frequency)
    - [🟡 Storage Optimized (i, d, h, im, is)](#-storage-optimized-i-d-h-im-is)
      - [Rodzina I — NVMe SSD (wysoki IOPS)](#rodzina-i--nvme-ssd-wysoki-iops)
      - [Rodzina D — Dense HDD (wysoka przepustowość)](#rodzina-d--dense-hdd-wysoka-przepustowość)
      - [Rodzina H — High Throughput HDD](#rodzina-h--high-throughput-hdd)
      - [Rodzina IM/IS — Storage Optimized z Graviton](#rodzina-imis--storage-optimized-z-graviton)
    - [🟣 Accelerated Computing (p, g, trn, inf, f, vt)](#-accelerated-computing-p-g-trn-inf-f-vt)
      - [Rodzina P — GPU (Training ML)](#rodzina-p--gpu-training-ml)
      - [Rodzina G — GPU (Inference + Graphics)](#rodzina-g--gpu-inference--graphics)
      - [Rodzina Trn — AWS Trainium (Training)](#rodzina-trn--aws-trainium-training)
      - [Rodzina Inf — AWS Inferentia (Inference)](#rodzina-inf--aws-inferentia-inference)
      - [Rodzina F — FPGA](#rodzina-f--fpga)
      - [Rodzina VT — Video Transcoding](#rodzina-vt--video-transcoding)
    - [⚫ High Performance Computing (hpc)](#-high-performance-computing-hpc)
    - [13.4 Tabela Porównawcza Rodzin — Quick Reference](#134-tabela-porównawcza-rodzin--quick-reference)
    - [13.5 Modele Zakupu EC2 — Koszty i Optymalizacja](#135-modele-zakupu-ec2--koszty-i-optymalizacja)
    - [On-Demand](#on-demand)
    - [Reserved Instances (RI)](#reserved-instances-ri)
    - [Savings Plans](#savings-plans)
    - [Spot Instances](#spot-instances)
    - [Dedicated Hosts](#dedicated-hosts)
    - [Dedicated Instances](#dedicated-instances)
    - [Capacity Reservations](#capacity-reservations)
    - [13.6 Porównanie Wszystkich Modeli Cenowych](#136-porównanie-wszystkich-modeli-cenowych)
    - [13.7 Storage dla EC2](#137-storage-dla-ec2)
    - [EBS — Elastic Block Store](#ebs--elastic-block-store)
    - [Instance Store](#instance-store)
    - [EFS — Elastic File System](#efs--elastic-file-system)
    - [13.8 Sieć EC2 — ENI, Enhanced Networking, Placement Groups](#138-sieć-ec2--eni-enhanced-networking-placement-groups)
    - [ENI (Elastic Network Interface)](#eni-elastic-network-interface)
    - [Enhanced Networking (ENA/SR-IOV)](#enhanced-networking-enasr-iov)
    - [Placement Groups](#placement-groups)
    - [13.9 EC2 — Limity i Ograniczenia](#139-ec2--limity-i-ograniczenia)
    - [13.10 Well-Architected Framework — EC2](#1310-well-architected-framework--ec2)
    - [13.11 4 Domeny Egzaminacyjne SAP-C02](#1311-4-domeny-egzaminacyjne-sap-c02)
    - [Domena 1: Design Solutions for Organizational Complexity](#domena-1-design-solutions-for-organizational-complexity-1)
    - [Domena 2: Design for New Solutions](#domena-2-design-for-new-solutions-1)
    - [Domena 3: Continuous Improvement for Existing Solutions](#domena-3-continuous-improvement-for-existing-solutions-1)
    - [Domena 4: Accelerate Workload Migration and Modernization](#domena-4-accelerate-workload-migration-and-modernization-1)
  - [14. Elastic Load Balancing — ALB, NLB, GWLB, CLB](#14-elastic-load-balancing--alb-nlb-gwlb-clb)
    - [14.1 Co to jest ELB i po co istnieje?](#141-co-to-jest-elb-i-po-co-istnieje)
    - [Dlaczego ELB, a nie własny load balancer na EC2?](#dlaczego-elb-a-nie-własny-load-balancer-na-ec2)
    - [14.2 Cztery Typy Load Balancerów — Przegląd](#142-cztery-typy-load-balancerów--przegląd)
    - [14.3 ALB — Application Load Balancer](#143-alb--application-load-balancer)
    - [Architektura i działanie](#architektura-i-działanie)
    - [Komponenty ALB](#komponenty-alb)
    - [Target Types](#target-types)
    - [Weighted Target Groups (Traffic Shifting)](#weighted-target-groups-traffic-shifting)
    - [Sticky Sessions (Session Affinity)](#sticky-sessions-session-affinity)
    - [SSL/TLS Termination i SNI](#ssltls-termination-i-sni)
    - [Connection Draining / Deregistration Delay](#connection-draining--deregistration-delay)
    - [ALB Access Logs](#alb-access-logs)
    - [ALB — Headers i X-Forwarded](#alb--headers-i-x-forwarded)
    - [14.4 NLB — Network Load Balancer](#144-nlb--network-load-balancer)
    - [Architektura i działanie](#architektura-i-działanie-1)
    - [Kluczowe cechy NLB](#kluczowe-cechy-nlb)
    - [NLB — kiedy używać vs ALB](#nlb--kiedy-używać-vs-alb)
    - [NLB + PrivateLink](#nlb--privatelink)
    - [Cross-Zone Load Balancing](#cross-zone-load-balancing)
    - [14.5 GWLB — Gateway Load Balancer](#145-gwlb--gateway-load-balancer)
    - [Czym jest GWLB?](#czym-jest-gwlb)
    - [Architektura GWLB z GENEVE](#architektura-gwlb-z-geneve)
    - [GENEVE Protocol](#geneve-protocol)
    - [Kiedy używać GWLB](#kiedy-używać-gwlb)
    - [14.6 CLB — Classic Load Balancer (Legacy)](#146-clb--classic-load-balancer-legacy)
    - [14.7 Health Checks — Szczegóły](#147-health-checks--szczegóły)
    - [14.8 ELB + Auto Scaling Group — Integracja](#148-elb--auto-scaling-group--integracja)
    - [14.9 SSL/TLS — Tryby i Polityki](#149-ssltls--tryby-i-polityki)
    - [SSL Termination na ALB](#ssl-termination-na-alb)
    - [SSL Passthrough na NLB](#ssl-passthrough-na-nlb)
    - [SSL Bridging (Re-encryption)](#ssl-bridging-re-encryption)
    - [14.10 ELB — Zaawansowane Funkcje](#1410-elb--zaawansowane-funkcje)
    - [Idle Timeout](#idle-timeout)
    - [Deletion Protection](#deletion-protection)
    - [Slow Start Mode (ALB)](#slow-start-mode-alb)
    - [Request Routing Algorithms](#request-routing-algorithms)
    - [Lambda Target (ALB → Lambda)](#lambda-target-alb--lambda)
    - [ALB Authentication (Cognito/OIDC)](#alb-authentication-cognitooidc)
    - [14.11 ELB — Koszty](#1411-elb--koszty)
    - [ALB Pricing](#alb-pricing)
    - [NLB Pricing](#nlb-pricing)
    - [GWLB Pricing](#gwlb-pricing)
    - [14.12 ELB — Monitoring i Metryki](#1412-elb--monitoring-i-metryki)
    - [14.13 Well-Architected Framework — ELB](#1413-well-architected-framework--elb)
    - [14.14 4 Domeny Egzaminacyjne SAP-C02](#1414-4-domeny-egzaminacyjne-sap-c02)
    - [Domena 1: Design Solutions for Organizational Complexity](#domena-1-design-solutions-for-organizational-complexity-2)
    - [Domena 2: Design for New Solutions](#domena-2-design-for-new-solutions-2)
    - [Domena 3: Continuous Improvement for Existing Solutions](#domena-3-continuous-improvement-for-existing-solutions-2)
    - [Domena 4: Accelerate Workload Migration and Modernization](#domena-4-accelerate-workload-migration-and-modernization-2)
  - [15. HA, Redundancja i Skalowalność — EC2, ELB, ASG i Integracje](#15-ha-redundancja-i-skalowalność--ec2-elb-asg-i-integracje)
    - [15.1 Fundamenty HA i Redundancji w AWS](#151-fundamenty-ha-i-redundancji-w-aws)
    - [Hierarchia odporności na awarie](#hierarchia-odporności-na-awarie)
    - [Terminy](#terminy)
    - [15.2 Auto Scaling Group (ASG) — Fundament Skalowania EC2](#152-auto-scaling-group-asg--fundament-skalowania-ec2)
    - [Czym jest ASG?](#czym-jest-asg)
    - [Launch Template (nie Launch Configuration!)](#launch-template-nie-launch-configuration)
    - [Typy Health Check w ASG](#typy-health-check-w-asg)
    - [ASG Lifecycle Hooks](#asg-lifecycle-hooks)
    - [15.3 Polityki Skalowania ASG](#153-polityki-skalowania-asg)
    - [1. Simple Scaling (legacy, nie używaj)](#1-simple-scaling-legacy-nie-używaj)
    - [2. Step Scaling (zalecane dla reaktywnego)](#2-step-scaling-zalecane-dla-reaktywnego)
    - [3. Target Tracking Scaling (zalecane, najprostsze)](#3-target-tracking-scaling-zalecane-najprostsze)
    - [4. Scheduled Scaling](#4-scheduled-scaling)
    - [5. Predictive Scaling](#5-predictive-scaling)
    - [Porównanie polityk skalowania](#porównanie-polityk-skalowania)
    - [15.4 ASG — Mixed Instances Policy (Spot + On-Demand)](#154-asg--mixed-instances-policy-spot--on-demand)
    - [Strategie Spot w ASG](#strategie-spot-w-asg)
    - [15.5 Warmup, Cooldown i Instance Refresh](#155-warmup-cooldown-i-instance-refresh)
    - [Cooldown i Warmup](#cooldown-i-warmup)
    - [Instance Refresh — Rolling Deploy bez Downtime](#instance-refresh--rolling-deploy-bez-downtime)
    - [15.6 ELB HA — Jak ELB Zapewnia Wysoką Dostępność](#156-elb-ha--jak-elb-zapewnia-wysoką-dostępność)
    - [Multi-AZ ELB](#multi-az-elb)
    - [ELB nie jest Single Point of Failure](#elb-nie-jest-single-point-of-failure)
    - [Zonal Isolation — Awaria AZ](#zonal-isolation--awaria-az)
    - [15.7 ELB + EC2 — Pełna Integracja](#157-elb--ec2--pełna-integracja)
    - [Flow: Request od klienta do EC2](#flow-request-od-klienta-do-ec2)
    - [Security Group Chain — Zasada Least Privilege](#security-group-chain--zasada-least-privilege)
    - [EC2 Registration w Target Group](#ec2-registration-w-target-group)
    - [Port Override — Multiple Apps na Jednej EC2](#port-override--multiple-apps-na-jednej-ec2)
    - [15.8 ELB + ECS/Fargate — Kontenery](#158-elb--ecsfargate--kontenery)
    - [Dynamic Port Mapping (EC2 launch type)](#dynamic-port-mapping-ec2-launch-type)
    - [Fargate (target type = ip)](#fargate-target-type--ip)
    - [15.9 ELB + Lambda — Serverless Backend](#159-elb--lambda--serverless-backend)
    - [15.10 ELB + On-Premises (Hybrid) — Target Type IP](#1510-elb--on-premises-hybrid--target-type-ip)
    - [Wzorzec Migracji Stopniowej](#wzorzec-migracji-stopniowej)
    - [15.11 ELB + Multi-VPC (Cross-VPC)](#1511-elb--multi-vpc-cross-vpc)
    - [Metoda 1: VPC Peering + ALB IP Target](#metoda-1-vpc-peering--alb-ip-target)
    - [Metoda 2: PrivateLink (NLB + Interface Endpoint)](#metoda-2-privatelink-nlb--interface-endpoint)
    - [Metoda 3: ALB → NLB (Nested LB)](#metoda-3-alb--nlb-nested-lb)
    - [15.12 ELB + Global Accelerator — Ultra-HA Multi-Region](#1512-elb--global-accelerator--ultra-ha-multi-region)
    - [15.13 Wzorce Redundancji — Active-Active vs Active-Passive](#1513-wzorce-redundancji--active-active-vs-active-passive)
    - [Active-Active Multi-AZ](#active-active-multi-az)
    - [Active-Passive Multi-AZ](#active-passive-multi-az)
    - [Active-Active Multi-Region](#active-active-multi-region)
    - [Active-Passive Multi-Region (Pilot Light / Warm Standby)](#active-passive-multi-region-pilot-light--warm-standby)
    - [15.14 Termination Policies ASG — Która Instancja Usunąć](#1514-termination-policies-asg--która-instancja-usunąć)
    - [15.15 Scale-In Protection](#1515-scale-in-protection)
    - [15.16 Troubleshooting — Częste Problemy ASG + ELB](#1516-troubleshooting--częste-problemy-asg--elb)
    - [Problem 1: ASG zastępuje zdrowe instancje](#problem-1-asg-zastępuje-zdrowe-instancje)
    - [Problem 2: Scale-out nie nadąża za ruchem](#problem-2-scale-out-nie-nadąża-za-ruchem)
    - [Problem 3: NLB nie routuje do instancji w AZ](#problem-3-nlb-nie-routuje-do-instancji-w-az)
    - [15.17 Well-Architected Framework — HA i Skalowalność](#1517-well-architected-framework--ha-i-skalowalność)
    - [15.18 4 Domeny Egzaminacyjne SAP-C02](#1518-4-domeny-egzaminacyjne-sap-c02)
    - [Domena 1: Design Solutions for Organizational Complexity](#domena-1-design-solutions-for-organizational-complexity-3)
    - [Domena 2: Design for New Solutions](#domena-2-design-for-new-solutions-3)
    - [Domena 3: Continuous Improvement for Existing Solutions](#domena-3-continuous-improvement-for-existing-solutions-3)
    - [Domena 4: Accelerate Workload Migration and Modernization](#domena-4-accelerate-workload-migration-and-modernization-3)
    - [15.19 Warm Pools — Skrócenie Czasu Scale-Out](#1519-warm-pools--skrócenie-czasu-scale-out)
  - [16. TUN/TAP — Wirtualne Interfejsy Sieciowe, OpenVPN i AWS VPN](#16-tuntap--wirtualne-interfejsy-sieciowe-openvpn-i-aws-vpn)
    - [16.1 Czym są TUN i TAP?](#161-czym-są-tun-i-tap)
    - [16.2 Architektura Jądra — Jak TUN/TAP Istnieje w Systemie](#162-architektura-jądra--jak-tuntap-istnieje-w-systemie)
    - [16.3 TUN — Warstwa 3 (IP Tunnel)](#163-tun--warstwa-3-ip-tunnel)
    - [Jak działa TUN — krok po kroku](#jak-działa-tun--krok-po-kroku)
    - [TUN Interface — Właściwości](#tun-interface--właściwości)
    - [16.4 TAP — Warstwa 2 (Ethernet Bridge)](#164-tap--warstwa-2-ethernet-bridge)
    - [Jak działa TAP — krok po kroku](#jak-działa-tap--krok-po-kroku)
    - [TAP Interface — Właściwości](#tap-interface--właściwości)
    - [Ramka Ethernet przez TAP vs IP przez TUN](#ramka-ethernet-przez-tap-vs-ip-przez-tun)
    - [16.5 TUN vs TAP — Kiedy Co Używać](#165-tun-vs-tap--kiedy-co-używać)
    - [16.6 Tworzenie TUN/TAP — Konfiguracja Ręczna](#166-tworzenie-tuntap--konfiguracja-ręczna)
    - [Tworzymy TUN ręcznie (Linux)](#tworzymy-tun-ręcznie-linux)
    - [Tworzymy TAP ręcznie](#tworzymy-tap-ręcznie)
    - [/dev/net/tun — plik urządzenia](#devnettun--plik-urządzenia)
    - [16.7 OpenVPN — Jak Używa TUN/TAP](#167-openvpn--jak-używa-tuntap)
    - [Tryb TUN (--dev tun) — Remote Access VPN](#tryb-tun---dev-tun--remote-access-vpn)
    - [Routing po stronie klienta (co dzieje się z tabelą routingu)](#routing-po-stronie-klienta-co-dzieje-się-z-tabelą-routingu)
    - [Tryb TAP (--dev tap) — Bridge VPN](#tryb-tap---dev-tap--bridge-vpn)
    - [16.8 Viscosity — macOS/Windows TUN/TAP](#168-viscosity--macoswindows-tuntap)
    - [macOS — utun zamiast tun](#macos--utun-zamiast-tun)
    - [Windows — TAP-Windows / Wintun](#windows--tap-windows--wintun)
    - [Viscosity — co robi pod spodem](#viscosity--co-robi-pod-spodem)
    - [16.9 Szyfrowanie w TUN — Warstwy Ochrony OpenVPN](#169-szyfrowanie-w-tun--warstwy-ochrony-openvpn)
    - [16.10 MTU — Krytyczny Problem TUN/TAP](#1610-mtu--krytyczny-problem-tuntap)
    - [16.11 AWS Client VPN — TUN Pod Spodem](#1611-aws-client-vpn--tun-pod-spodem)
    - [Jak AWS Client VPN działa z TUN](#jak-aws-client-vpn-działa-z-tun)
    - [Client VPN .ovpn Plik — Anatomia](#client-vpn-ovpn-plik--anatomia)
    - [Co dzieje się z tun po połączeniu z Client VPN](#co-dzieje-się-z-tun-po-połączeniu-z-client-vpn)
    - [16.12 AWS Site-to-Site VPN — IPsec, nie OpenVPN](#1612-aws-site-to-site-vpn--ipsec-nie-openvpn)
    - [IPsec vs OpenVPN/TUN](#ipsec-vs-openvpntun)
    - [Jak IPsec tworzy wirtualny interfejs (Linux strongSwan/libreswan)](#jak-ipsec-tworzy-wirtualny-interfejs-linux-strongswanlibreswan)
    - [AWS VGW — Co dzieje się po stronie AWS](#aws-vgw--co-dzieje-się-po-stronie-aws)
    - [16.13 WireGuard — Nowoczesna Alternatywa dla TUN](#1613-wireguard--nowoczesna-alternatywa-dla-tun)
    - [WireGuard vs OpenVPN/TUN](#wireguard-vs-openvpntun)
    - [16.14 Diagnostyka TUN/TAP — Narzędzia](#1614-diagnostyka-tuntap--narzędzia)
    - [16.15 Bezpieczeństwo TUN/TAP — Zagrożenia i Ochrona](#1615-bezpieczeństwo-tuntap--zagrożenia-i-ochrona)
    - [16.16 TUN/TAP w Kontekście AWS — Mapa Powiązań](#1616-tuntap-w-kontekście-aws--mapa-powiązań)
    - [16.17 Well-Architected Framework — TUN/TAP i VPN](#1617-well-architected-framework--tuntap-i-vpn)
  - [17. Security Groups — Deep Dive: ENI, EC2, VPC, Reguły](#17-security-groups--deep-dive-eni-ec2-vpc-reguły)
    - [17.1 Czym Jest Security Group — Fundament](#171-czym-jest-security-group--fundament)
    - [17.2 ENI — Elastic Network Interface](#172-eni--elastic-network-interface)
    - [17.3 Jak SG Działa — Hypervisor Level](#173-jak-sg-działa--hypervisor-level)
    - [17.4 Stateful — Co To Naprawdę Oznacza](#174-stateful--co-to-naprawdę-oznacza)
    - [17.5 Reguły SG — Anatomia](#175-reguły-sg--anatomia)
    - [17.6 SG Source = Security Group ID — Magia AWS](#176-sg-source--security-group-id--magia-aws)
    - [Zamiast IP — używaj SG jako source/destination](#zamiast-ip--używaj-sg-jako-sourcedestination)
    - [Jak to działa pod spodem](#jak-to-działa-pod-spodem)
    - [Typowy schemat SG chaining (łańcuch SG)](#typowy-schemat-sg-chaining-łańcuch-sg)
    - [17.7 Self-Referencing Security Group](#177-self-referencing-security-group)
    - [17.8 Wiele SG na ENI — Jak Działa Union](#178-wiele-sg-na-eni--jak-działa-union)
    - [17.9 Default Security Group vs Custom Security Group](#179-default-security-group-vs-custom-security-group)
    - [17.10 SG na Poziomie VPC vs Subnet vs ENI](#1710-sg-na-poziomie-vpc-vs-subnet-vs-eni)
    - [Ilustracja: EC2 w tej samej subnecie](#ilustracja-ec2-w-tej-samej-subnecie)
    - [17.11 SG vs NACL — Kluczowe Różnice](#1711-sg-vs-nacl--kluczowe-różnice)
    - [NACL Stateless — Dlaczego Ephemeral Ports Ważne](#nacl-stateless--dlaczego-ephemeral-ports-ważne)
    - [17.12 Outbound Rules — Kiedy Ograniczać](#1712-outbound-rules--kiedy-ograniczać)
    - [17.13 Jak Dodać / Zmodyfikować SG — AWS CLI](#1713-jak-dodać--zmodyfikować-sg--aws-cli)
    - [17.14 SG i VPC Peering / Transit Gateway](#1714-sg-i-vpc-peering--transit-gateway)
    - [17.15 SG i Services — ELB, RDS, Lambda, ECS](#1715-sg-i-services--elb-rds-lambda-ecs)
    - [ALB / NLB Security Groups](#alb--nlb-security-groups)
    - [RDS Security Groups](#rds-security-groups)
    - [Lambda Security Groups](#lambda-security-groups)
    - [ECS Fargate Security Groups](#ecs-fargate-security-groups)
    - [17.16 Troubleshooting SG — Najczęstsze Problemy](#1716-troubleshooting-sg--najczęstsze-problemy)
    - [VPC Reachability Analyzer — automatyczna diagnostyka](#vpc-reachability-analyzer--automatyczna-diagnostyka)
    - [17.17 Limity Security Groups](#1717-limity-security-groups)
    - [17.18 Well-Architected Framework — Security Groups](#1718-well-architected-framework--security-groups)
    - [17.19 4 Domeny Egzaminacyjne SAP-C02](#1719-4-domeny-egzaminacyjne-sap-c02)
    - [17.20 VPC Flow Logs — Debug SG przez Logi](#1720-vpc-flow-logs--debug-sg-przez-logi)
  - [18. Launch Templates, AMI, Instance Types, Placement Groups — Deep Dive](#18-launch-templates-ami-instance-types-placement-groups--deep-dive)
    - [18.1 Launch Template — Czym Jest i Po Co](#181-launch-template--czym-jest-i-po-co)
    - [Launch Template vs Launch Configuration](#launch-template-vs-launch-configuration)
    - [Wersjonowanie Launch Template](#wersjonowanie-launch-template)
    - [User Data — Cloud-Init](#user-data--cloud-init)
    - [18.2 AMI — Amazon Machine Image](#182-ami--amazon-machine-image)
    - [Anatomia AMI](#anatomia-ami)
    - [Tworzenie AMI](#tworzenie-ami)
    - [AMI — Gdzie i Kiedy](#ami--gdzie-i-kiedy)
    - [AMI Sharing — Właściciele i Dostęp](#ami-sharing--właściciele-i-dostęp)
    - [AMI Encryption](#ami-encryption)
    - [18.3 AMI Catalog — Publiczne i Marketplace AMI](#183-ami-catalog--publiczne-i-marketplace-ami)
    - [EC2 Image Builder — Automatyczne AMI Pipeline](#ec2-image-builder--automatyczne-ami-pipeline)
    - [18.4 Typy Instancji EC2 — Szczegółowy Przegląd](#184-typy-instancji-ec2--szczegółowy-przegląd)
    - [Wszystkie Sufiksy Instancji](#wszystkie-sufiksy-instancji)
    - [Rodziny Instancji — Pełna Tabela](#rodziny-instancji--pełna-tabela)
    - [Graviton — AWS ARM Chip Szczegóły](#graviton--aws-arm-chip-szczegóły)
    - [18.5 Placement Groups — Rozmieszczenie Instancji](#185-placement-groups--rozmieszczenie-instancji)
    - [Cluster Placement Group](#cluster-placement-group)
    - [Spread Placement Group](#spread-placement-group)
    - [Partition Placement Group](#partition-placement-group)
    - [Placement Groups — Porównanie](#placement-groups--porównanie)
    - [18.6 Well-Architected Framework — Launch Templates, AMI, PG](#186-well-architected-framework--launch-templates-ami-pg)
  - [19. Modele Zakupu EC2 — Spot, Reserved Instances, Savings Plans, Dedicated, Capacity](#19-modele-zakupu-ec2--spot-reserved-instances-savings-plans-dedicated-capacity)
    - [19.1 Przegląd Modeli Zakupu — Big Picture](#191-przegląd-modeli-zakupu--big-picture)
    - [19.2 On-Demand — Baseline](#192-on-demand--baseline)
    - [19.3 Spot Instances — Deep Dive](#193-spot-instances--deep-dive)
    - [Czym Są Spot Instances](#czym-są-spot-instances)
    - [Spot Fleet vs EC2 Fleet](#spot-fleet-vs-ec2-fleet)
    - [Spot Request Types](#spot-request-types)
    - [Spot Blocks (deprecated 2021)](#spot-blocks-deprecated-2021)
    - [Resilient Architecture dla Spot](#resilient-architecture-dla-spot)
    - [19.4 Reserved Instances (RI) — Deep Dive](#194-reserved-instances-ri--deep-dive)
    - [Typy RI](#typy-ri)
    - [RI Zakres (Scope)](#ri-zakres-scope)
    - [RI Payment Options](#ri-payment-options)
    - [Jak RI jest Aplikowane](#jak-ri-jest-aplikowane)
    - [RI Marketplace](#ri-marketplace)
    - [19.5 Savings Plans — Elastyczna Alternatywa dla RI](#195-savings-plans--elastyczna-alternatywa-dla-ri)
    - [Typy Savings Plans](#typy-savings-plans)
    - [Jak Zakupić Savings Plans](#jak-zakupić-savings-plans)
    - [Savings Plans vs RI — Kiedy Co](#savings-plans-vs-ri--kiedy-co)
    - [19.6 Dedicated Hosts i Dedicated Instances](#196-dedicated-hosts-i-dedicated-instances)
    - [Dedicated Host](#dedicated-host)
    - [Dedicated Instance](#dedicated-instance)
    - [19.7 Capacity Reservations](#197-capacity-reservations)
    - [On-Demand Capacity Reservation](#on-demand-capacity-reservation)
    - [19.8 UltraServers (EC2 UltraCluster)](#198-ultraservers-ec2-ultracluster)
    - [19.9 EC2 Capacity Manager (AWS Capacity Manager)](#199-ec2-capacity-manager-aws-capacity-manager)
    - [19.10 Wybór Modelu — Decision Tree](#1910-wybór-modelu--decision-tree)
    - [19.11 Well-Architected Framework — Modele Zakupu](#1911-well-architected-framework--modele-zakupu)
  - [20. EBS Volumes, Snapshots, Lifecycle Manager, Instance Store](#20-ebs-volumes-snapshots-lifecycle-manager-instance-store)
    - [20.1 EBS — Elastic Block Store Przegląd](#201-ebs--elastic-block-store-przegląd)
    - [20.2 Typy Woluminów EBS — Szczegółowa Tabela](#202-typy-woluminów-ebs--szczegółowa-tabela)
    - [gp3 — General Purpose SSD (Standard)](#gp3--general-purpose-ssd-standard)
    - [io2 Block Express — Provisioned IOPS SSD](#io2-block-express--provisioned-iops-ssd)
    - [st1 i sc1 — HDD Volumes](#st1-i-sc1--hdd-volumes)
    - [20.3 EBS Multi-Attach](#203-ebs-multi-attach)
    - [20.4 EBS Szyfrowanie](#204-ebs-szyfrowanie)
    - [20.5 EBS Snapshots — Incremental Backup](#205-ebs-snapshots--incremental-backup)
    - [Fast Snapshot Restore (FSR)](#fast-snapshot-restore-fsr)
    - [Snapshot Cross-Region Copy](#snapshot-cross-region-copy)
    - [20.6 EBS Volume Modification (Hot Resize)](#206-ebs-volume-modification-hot-resize)
    - [20.7 EBS Wydajność — Limity i Bottlenecks](#207-ebs-wydajność--limity-i-bottlenecks)
    - [20.8 Amazon Data Lifecycle Manager (DLM)](#208-amazon-data-lifecycle-manager-dlm)
    - [Tworzenie DLM Policy](#tworzenie-dlm-policy)
    - [DLM Policy Typy](#dlm-policy-typy)
    - [DLM IAM Role](#dlm-iam-role)
    - [20.9 EBS Recycle Bin](#209-ebs-recycle-bin)
    - [20.10 Instance Store vs EBS — Kiedy Co](#2010-instance-store-vs-ebs--kiedy-co)
    - [20.11 Well-Architected Framework — EBS/Snapshots](#2011-well-architected-framework--ebssnapshots)
  - [21. Target Groups, Trust Stores, Auto Scaling Groups — Deep Dive](#21-target-groups-trust-stores-auto-scaling-groups--deep-dive)
    - [21.1 Target Groups — Czym Są](#211-target-groups--czym-są)
    - [21.2 Typy Targetów w Target Group](#212-typy-targetów-w-target-group)
    - [Target Type: IP — Szczegóły](#target-type-ip--szczegóły)
    - [Target Type: Lambda](#target-type-lambda)
    - [21.3 Health Checks w Target Group](#213-health-checks-w-target-group)
    - [21.4 Target Group Attributes — Konfiguracja](#214-target-group-attributes--konfiguracja)
    - [Slow Start Mode](#slow-start-mode)
    - [Weighted Target Groups](#weighted-target-groups)
    - [21.5 Trust Stores — Mutual TLS (mTLS) dla ALB](#215-trust-stores--mutual-tls-mtls-dla-alb)
    - [Czym Jest Mutual TLS](#czym-jest-mutual-tls)
    - [ALB Trust Store — Jak Działa](#alb-trust-store--jak-działa)
    - [Tworzenie Trust Store](#tworzenie-trust-store)
    - [Trust Store Certificate Revocation (OCSP/CRL)](#trust-store-certificate-revocation-ocspcrl)
    - [21.6 Auto Scaling Groups — Deep Dive](#216-auto-scaling-groups--deep-dive)
    - [Lifecycle Hooks — Szczegóły](#lifecycle-hooks--szczegóły)
    - [Warm Pools — Szczegóły](#warm-pools--szczegóły)
    - [Instance Refresh — Szczegóły](#instance-refresh--szczegóły)
    - [Termination Policies — Które Instancje Usunąć](#termination-policies--które-instancje-usunąć)
    - [Scaling Policies — Głębsze Szczegóły](#scaling-policies--głębsze-szczegóły)
    - [21.7 ASG Mixed Instances — Advanced](#217-asg-mixed-instances--advanced)
    - [21.8 ASG Health Checks — Typy](#218-asg-health-checks--typy)
    - [21.9 ASG Scaling Cooldown i Warmup](#219-asg-scaling-cooldown-i-warmup)
    - [21.10 Well-Architected Framework — TG, Trust Stores, ASG](#2110-well-architected-framework--tg-trust-stores-asg)
  - [22. AWS DataSync i AWS Transfer Family](#22-aws-datasync-i-aws-transfer-family)
    - [22.1 AWS DataSync — Przegląd](#221-aws-datasync--przegląd)
    - [Kiedy używać DataSync?](#kiedy-używać-datasync)
    - [Jak działa DataSync?](#jak-działa-datasync)
    - [Kluczowe cechy DataSync](#kluczowe-cechy-datasync)
    - [22.2 DataSync Agent](#222-datasync-agent)
    - [Gdzie wdrożyć agenta?](#gdzie-wdrożyć-agenta)
    - [Komendy AWS CLI — DataSync Agent](#komendy-aws-cli--datasync-agent)
    - [Agent — ważne szczegóły](#agent--ważne-szczegóły)
    - [22.3 DataSync — Źródła i Cele](#223-datasync--źródła-i-cele)
    - [Obsługiwane lokalizacje źródłowe (Source Locations)](#obsługiwane-lokalizacje-źródłowe-source-locations)
    - [Obsługiwane lokalizacje docelowe (Destination Locations)](#obsługiwane-lokalizacje-docelowe-destination-locations)
    - [Komendy — tworzenie lokalizacji](#komendy--tworzenie-lokalizacji)
    - [22.4 DataSync — Konfiguracja i Funkcje](#224-datasync--konfiguracja-i-funkcje)
    - [Task — podstawowy obiekt DataSync](#task--podstawowy-obiekt-datasync)
    - [Opcje Task — kluczowe parametry](#opcje-task--kluczowe-parametry)
    - [Bandwidth Throttling](#bandwidth-throttling)
    - [DataSync — Filtrowanie plików](#datasync--filtrowanie-plików)
    - [Diagram: DataSync Task Lifecycle](#diagram-datasync-task-lifecycle)
    - [22.5 AWS Transfer Family — Przegląd](#225-aws-transfer-family--przegląd)
    - [Kluczowe przypadki użycia](#kluczowe-przypadki-użycia)
    - [Transfer Family — architektura](#transfer-family--architektura)
    - [22.6 Transfer Family — Protokoły](#226-transfer-family--protokoły)
    - [Porównanie protokołów](#porównanie-protokołów)
    - [SFTP — Szczegóły](#sftp--szczegóły)
    - [AS2 — Applicability Statement 2](#as2--applicability-statement-2)
    - [22.7 Transfer Family — Autentykacja i Endpointy](#227-transfer-family--autentykacja-i-endpointy)
    - [Metody autentykacji](#metody-autentykacji-1)
    - [Custom Identity Provider (Lambda)](#custom-identity-provider-lambda)
    - [Typy endpointów](#typy-endpointów)
    - [Logical Directories — mapowanie katalogów](#logical-directories--mapowanie-katalogów)
    - [Workflow — przetwarzanie po uploadzie](#workflow--przetwarzanie-po-uploadzie)
    - [22.8 Porównanie Usług Transferu Danych](#228-porównanie-usług-transferu-danych)
    - [Główna tabela porównawcza](#główna-tabela-porównawcza)
    - [Decision Tree — którą usługę wybrać?](#decision-tree--którą-usługę-wybrać)
    - [DataSync vs Transfer Family — kluczowa różnica](#datasync-vs-transfer-family--kluczowa-różnica)
    - [22.9 Azure i Open-Source Odpowiedniki](#229-azure-i-open-source-odpowiedniki)
    - [Azure Odpowiedniki](#azure-odpowiedniki)
    - [Open-Source Odpowiedniki](#open-source-odpowiedniki)
    - [22.10 Well-Architected Framework](#2210-well-architected-framework)
    - [22.11 4 Domeny Egzaminacyjne SAP-C02](#2211-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization)
    - [22.12 Reliability \& Security](#2212-reliability--security)
    - [DataSync — Security](#datasync--security)
    - [Transfer Family — Security](#transfer-family--security)
    - [Reliability](#reliability)
    - [Limity (Limits)](#limity-limits)
  - [23. AWS Storage Gateway](#23-aws-storage-gateway)
    - [23.1 Przegląd i Kontekst](#231-przegląd-i-kontekst)
    - [23.2 S3 File Gateway](#232-s3-file-gateway)
    - [Opis](#opis)
    - [Kluczowe cechy](#kluczowe-cechy)
    - [Use cases](#use-cases)
    - [AWS CLI](#aws-cli)
    - [23.3 FSx File Gateway](#233-fsx-file-gateway)
    - [Opis](#opis-1)
    - [Kluczowe cechy](#kluczowe-cechy-1)
    - [Use cases](#use-cases-1)
    - [23.4 Volume Gateway](#234-volume-gateway)
    - [Opis](#opis-2)
    - [Cached Mode (Tryb z Pamięcią Podręczną)](#cached-mode-tryb-z-pamięcią-podręczną)
    - [Stored Mode (Tryb z Pełnym Przechowywaniem)](#stored-mode-tryb-z-pełnym-przechowywaniem)
    - [Porównanie trybów Volume Gateway](#porównanie-trybów-volume-gateway)
    - [AWS CLI](#aws-cli-1)
    - [23.5 Tape Gateway (VTL — Virtual Tape Library)](#235-tape-gateway-vtl--virtual-tape-library)
    - [Opis](#opis-3)
    - [Kluczowe pojęcia](#kluczowe-pojęcia)
    - [Lifecycle taśmy](#lifecycle-taśmy)
    - [Kompatybilność oprogramowania](#kompatybilność-oprogramowania)
    - [AWS CLI](#aws-cli-2)
    - [23.6 Pełne Porównanie — Wszystkie Typy](#236-pełne-porównanie--wszystkie-typy)
    - [23.7 Opcje Wdrożenia](#237-opcje-wdrożenia)
    - [23.8 Przepustowość i Latencja](#238-przepustowość-i-latencja)
    - [23.9 Storage Gateway vs DataSync](#239-storage-gateway-vs-datasync)
    - [23.10 Bezpieczeństwo i Niezawodność](#2310-bezpieczeństwo-i-niezawodność)
    - [Szyfrowanie](#szyfrowanie)
    - [IAM i dostęp](#iam-i-dostęp)
    - [Wysoka dostępność](#wysoka-dostępność)
    - [23.11 Limity i Ograniczenia](#2311-limity-i-ograniczenia)
    - [23.12 Well-Architected Framework](#2312-well-architected-framework)
    - [23.13 4 Domeny Egzaminacyjne SAP-C02](#2313-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-1)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-1)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-1)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-1)
  - [24. Amazon EBS — Elastic Block Store](#24-amazon-ebs--elastic-block-store)
    - [24.1 Przegląd i Kontekst](#241-przegląd-i-kontekst)
    - [🧠 Czym jest EBS?](#-czym-jest-ebs)
    - [🌍 Gdzie regionalny/zonalny?](#-gdzie-regionalnyzonalny)
    - [☁️ Porównanie z Azure](#️-porównanie-z-azure)
    - [24.2 Typy Woluminów EBS](#242-typy-woluminów-ebs)
    - [📊 Tabela porównawcza wszystkich typów](#-tabela-porównawcza-wszystkich-typów)
    - [🔍 gp2 vs gp3 — kluczowe różnice](#-gp2-vs-gp3--kluczowe-różnice)
    - [🔍 io1 vs io2](#-io1-vs-io2)
    - [24.3 EBS Multi-Attach](#243-ebs-multi-attach)
    - [24.4 EBS Snapshots](#244-ebs-snapshots)
    - [Jak działają snapshots](#jak-działają-snapshots)
    - [Operacje na snapshotach](#operacje-na-snapshotach)
    - [Fast Snapshot Restore (FSR)](#fast-snapshot-restore-fsr-1)
    - [Snapshot Archive Tier](#snapshot-archive-tier)
    - [24.5 EBS Encryption](#245-ebs-encryption)
    - [Co jest szyfrowane?](#co-jest-szyfrowane)
    - [Jak działa](#jak-działa)
    - [Jak zaszyfrować istniejący niezaszyfrowany wolumin](#jak-zaszyfrować-istniejący-niezaszyfrowany-wolumin)
    - [24.6 EBS vs Instance Store](#246-ebs-vs-instance-store)
    - [24.7 EBS Performance](#247-ebs-performance)
    - [IOPS vs Throughput](#iops-vs-throughput)
    - [Queue Depth](#queue-depth)
    - [Nitro Instances](#nitro-instances)
    - [24.8 EBS Data Lifecycle Manager (DLM)](#248-ebs-data-lifecycle-manager-dlm)
    - [24.9 EBS Elastic Volumes](#249-ebs-elastic-volumes)
    - [24.10 Ograniczenia EBS](#2410-ograniczenia-ebs)
    - [24.11 Reliability \& Security](#2411-reliability--security)
    - [Reliability](#reliability-1)
    - [Security](#security)
    - [24.12 Well-Architected Framework](#2412-well-architected-framework)
    - [24.13 4 Domeny Egzaminacyjne SAP-C02](#2413-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-2)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-2)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-2)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-2)
  - [25. Amazon EFS — Elastic File System](#25-amazon-efs--elastic-file-system)
    - [25.1 Przegląd i kontekst](#251-przegląd-i-kontekst)
    - [25.2 EFS vs EBS vs S3 — Porównanie](#252-efs-vs-ebs-vs-s3--porównanie)
    - [25.3 EFS Performance Modes](#253-efs-performance-modes)
    - [25.4 EFS Throughput Modes](#254-efs-throughput-modes)
    - [25.5 EFS Storage Classes i Lifecycle Management](#255-efs-storage-classes-i-lifecycle-management)
    - [Klasy storage](#klasy-storage)
    - [Lifecycle Management](#lifecycle-management)
    - [25.6 EFS Access Points](#256-efs-access-points)
    - [25.7 EFS Security](#257-efs-security)
    - [Warstwy bezpieczeństwa](#warstwy-bezpieczeństwa)
    - [25.8 EFS z Lambda, ECS, EKS](#258-efs-z-lambda-ecs-eks)
    - [EFS + Lambda](#efs--lambda)
    - [EFS + ECS/Fargate](#efs--ecsfargate)
    - [EFS + EKS](#efs--eks)
    - [25.9 EFS Cross-Region i Backup](#259-efs-cross-region-i-backup)
    - [Cross-Region](#cross-region)
    - [AWS Backup + EFS](#aws-backup--efs)
    - [25.10 Porównanie EFS vs Azure Files vs GCP Filestore](#2510-porównanie-efs-vs-azure-files-vs-gcp-filestore)
    - [25.11 Ograniczenia EFS](#2511-ograniczenia-efs)
    - [25.12 Well-Architected Framework — EFS](#2512-well-architected-framework--efs)
    - [25.13 4 Domeny Egzaminacyjne SAP-C02](#2513-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-3)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-3)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-3)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-3)
    - [25.14 Reliability i Security — Deep Dive](#2514-reliability-i-security--deep-dive)
    - [Reliability](#reliability-2)
    - [Security Checklist](#security-checklist)
  - [26. Amazon FSx — Managed File Systems](#26-amazon-fsx--managed-file-systems)
    - [26.1 Przegląd FSx](#261-przegląd-fsx)
    - [26.2 FSx for Windows File Server](#262-fsx-for-windows-file-server)
    - [🪟 Opis](#-opis)
    - [Architektura](#architektura-2)
    - [Kluczowe cechy](#kluczowe-cechy-2)
    - [Single-AZ vs Multi-AZ](#single-az-vs-multi-az)
    - [DFS (Distributed File System)](#dfs-distributed-file-system)
    - [AWS CLI — przykłady](#aws-cli--przykłady)
    - [Ograniczenia](#ograniczenia)
    - [26.3 FSx for Lustre](#263-fsx-for-lustre)
    - [⚡ Opis](#-opis-1)
    - [Architektura + S3 integracja](#architektura--s3-integracja)
    - [Scratch vs Persistent](#scratch-vs-persistent)
    - [Kluczowe cechy](#kluczowe-cechy-3)
    - [AWS CLI — przykłady](#aws-cli--przykłady-1)
    - [Ograniczenia](#ograniczenia-1)
    - [26.4 FSx for NetApp ONTAP](#264-fsx-for-netapp-ontap)
    - [🐧 Opis](#-opis-2)
    - [Architektura](#architektura-3)
    - [Kluczowe cechy](#kluczowe-cechy-4)
    - [Storage tiering](#storage-tiering)
    - [Migracja z on-prem ONTAP](#migracja-z-on-prem-ontap)
    - [26.5 FSx for OpenZFS](#265-fsx-for-openzfs)
    - [🐡 Opis](#-opis-3)
    - [Kluczowe cechy](#kluczowe-cechy-5)
    - [ZFS — zero-copy clone](#zfs--zero-copy-clone)
    - [AWS CLI](#aws-cli-3)
    - [26.6 Tabela porównawcza](#266-tabela-porównawcza)
    - [26.7 FSx vs EFS](#267-fsx-vs-efs)
    - [26.8 Bezpieczeństwo i niezawodność](#268-bezpieczeństwo-i-niezawodność)
    - [Szyfrowanie](#szyfrowanie-1)
    - [Dostęp sieciowy](#dostęp-sieciowy)
    - [IAM + Resource Policies](#iam--resource-policies)
    - [Backup](#backup)
    - [HA i failover](#ha-i-failover)
    - [26.9 Well-Architected Framework](#269-well-architected-framework)
    - [26.10 4 Domeny egzaminacyjne SAP-C02](#2610-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-4)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-4)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-4)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-4)
  - [27. Amazon S3 — Simple Storage Service](#27-amazon-s3--simple-storage-service)
    - [27.1 Przegląd i Kontekst](#271-przegląd-i-kontekst)
    - [27.2 S3 Storage Classes](#272-s3-storage-classes)
    - [27.3 S3 Lifecycle Policies](#273-s3-lifecycle-policies)
    - [27.4 S3 Versioning](#274-s3-versioning)
    - [27.5 S3 Replication](#275-s3-replication)
    - [27.6 S3 Object Lock — WORM](#276-s3-object-lock--worm)
    - [27.7 S3 Encryption](#277-s3-encryption)
    - [27.8 S3 Access Control](#278-s3-access-control)
    - [27.9 S3 Presigned URLs](#279-s3-presigned-urls)
    - [27.10 S3 Event Notifications](#2710-s3-event-notifications)
    - [27.11 S3 Performance](#2711-s3-performance)
    - [27.12 S3 Requester Pays](#2712-s3-requester-pays)
    - [27.13 S3 Static Website Hosting](#2713-s3-static-website-hosting)
    - [27.14 S3 Storage Lens](#2714-s3-storage-lens)
    - [27.15 S3 Batch Operations](#2715-s3-batch-operations)
    - [27.16 Integracje z innymi serwisami AWS](#2716-integracje-z-innymi-serwisami-aws)
    - [27.17 Reliability i Security](#2717-reliability-i-security)
    - [27.18 Well-Architected Framework — S3](#2718-well-architected-framework--s3)
    - [27.19 4 Domeny Egzaminacyjne SAP-C02](#2719-4-domeny-egzaminacyjne-sap-c02)
    - [Design Solutions for Organizational Complexity](#design-solutions-for-organizational-complexity)
    - [Design for New Solutions](#design-for-new-solutions)
    - [Continuous Improvement for Existing Solutions](#continuous-improvement-for-existing-solutions)
    - [Accelerate Workload Migration and Modernization](#accelerate-workload-migration-and-modernization)
  - [28. AWS Backup — Centralne Zarządzanie Kopiami Zapasowymi](#28-aws-backup--centralne-zarządzanie-kopiami-zapasowymi)
    - [28.1 Czym jest AWS Backup?](#281-czym-jest-aws-backup)
    - [28.2 Obsługiwane Usługi AWS](#282-obsługiwane-usługi-aws)
    - [28.3 Backup Plans — Szczegóły](#283-backup-plans--szczegóły)
    - [Backup Rule — parametry:](#backup-rule--parametry)
    - [28.4 Backup Vaults](#284-backup-vaults)
    - [28.5 Backup Selection — Jak Wybierać Zasoby](#285-backup-selection--jak-wybierać-zasoby)
    - [28.6 Cross-Region i Cross-Account Backup](#286-cross-region-i-cross-account-backup)
    - [Cross-Region — Copy Rule](#cross-region--copy-rule)
    - [Cross-Account — AWS Organizations](#cross-account--aws-organizations)
    - [28.7 AWS Backup Vault Lock — WORM](#287-aws-backup-vault-lock--worm)
    - [28.8 AWS Backup Audit Manager](#288-aws-backup-audit-manager)
    - [28.9 AWS Backup vs Native Snapshots](#289-aws-backup-vs-native-snapshots)
    - [28.10 Restore](#2810-restore)
    - [28.11 Backup dla VMware — AWS Backup Gateway](#2811-backup-dla-vmware--aws-backup-gateway)
    - [28.12 Pricing](#2812-pricing)
    - [28.13 Integracja z AWS Organizations](#2813-integracja-z-aws-organizations)
    - [28.14 Well-Architected Framework](#2814-well-architected-framework)
    - [28.15 4 Domeny Egzaminacyjne SAP-C02](#2815-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-5)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-5)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-5)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-5)
    - [28.16 Reliability i Security](#2816-reliability-i-security)
    - [Reliability](#reliability-3)
    - [Security](#security-1)
    - [Ograniczenia](#ograniczenia-2)
  - [29. AWS Snow Family — Snowcone, Snowball Edge, Snowmobile](#29-aws-snow-family--snowcone-snowball-edge-snowmobile)
    - [29.1 Przegląd i Kontekst](#291-przegląd-i-kontekst)
    - [29.2 AWS Snowcone](#292-aws-snowcone)
    - [29.3 AWS Snowball Edge](#293-aws-snowball-edge)
    - [Storage Optimized vs Compute Optimized](#storage-optimized-vs-compute-optimized)
    - [29.4 AWS Snowmobile](#294-aws-snowmobile)
    - [29.5 Tabela Porównawcza — Snow Family](#295-tabela-porównawcza--snow-family)
    - [29.6 Edge Computing na Snow](#296-edge-computing-na-snow)
    - [29.7 Proces Transferu Danych](#297-proces-transferu-danych)
    - [29.8 OpsHub](#298-opshub)
    - [29.9 Szyfrowanie i Bezpieczeństwo](#299-szyfrowanie-i-bezpieczeństwo)
    - [29.10 Snow vs Direct Connect vs DataSync — Decision Tree](#2910-snow-vs-direct-connect-vs-datasync--decision-tree)
    - [29.11 Azure — Data Box Family](#2911-azure--data-box-family)
    - [29.12 Well-Architected Framework](#2912-well-architected-framework)
    - [29.13 4 Domeny Egzaminacyjne SAP-C02](#2913-4-domeny-egzaminacyjne-sap-c02)
    - [Domain 1: Design Solutions for Organizational Complexity](#domain-1-design-solutions-for-organizational-complexity-6)
    - [Domain 2: Design for New Solutions](#domain-2-design-for-new-solutions-6)
    - [Domain 3: Continuous Improvement for Existing Solutions](#domain-3-continuous-improvement-for-existing-solutions-6)
    - [Domain 4: Accelerate Workload Migration and Modernization](#domain-4-accelerate-workload-migration-and-modernization-6)
  - [30. Amazon SageMaker i AI/ML w AWS](#30-amazon-sagemaker-i-aiml-w-aws)
    - [30.1 Przegląd i Kontekst](#301-przegląd-i-kontekst)
    - [30.2 Amazon SageMaker — Architektura i Składniki](#302-amazon-sagemaker--architektura-i-składniki)
    - [Główne składniki](#główne-składniki)
    - [30.3 SageMaker Studio — Centrum Operacyjne](#303-sagemaker-studio--centrum-operacyjne)
    - [Konfiguracja Studio](#konfiguracja-studio)
    - [Typy przestrzeni w Studio](#typy-przestrzeni-w-studio)
    - [Izolacja i sieć](#izolacja-i-sieć)
    - [30.4 SageMaker Training — Trenowanie Modeli](#304-sagemaker-training--trenowanie-modeli)
    - [Jak działa Training Job](#jak-działa-training-job)
    - [Tryby dystrybucji danych](#tryby-dystrybucji-danych)
    - [Typy instancji ML](#typy-instancji-ml)
    - [Distributed Training Strategies](#distributed-training-strategies)
    - [30.5 SageMaker Endpoints — Inference](#305-sagemaker-endpoints--inference)
    - [Real-Time Inference](#real-time-inference)
    - [Typy Inference](#typy-inference)
    - [A/B Testing — Multi-Variant Endpoint](#ab-testing--multi-variant-endpoint)
    - [Auto Scaling Endpointów](#auto-scaling-endpointów)
    - [30.6 SageMaker Pipelines — MLOps i CI/CD dla ML](#306-sagemaker-pipelines--mlops-i-cicd-dla-ml)
    - [Pipeline vs Step Functions vs MWAA (Airflow)](#pipeline-vs-step-functions-vs-mwaa-airflow)
    - [30.7 SageMaker Feature Store](#307-sagemaker-feature-store)
    - [30.8 SageMaker Model Monitor — Monitorowanie Dryftu](#308-sagemaker-model-monitor--monitorowanie-dryftu)
    - [Typy monitoringu](#typy-monitoringu)
    - [30.9 SageMaker Clarify — Bias i Explainability](#309-sagemaker-clarify--bias-i-explainability)
    - [30.10 Amazon Bedrock — Generative AI](#3010-amazon-bedrock--generative-ai)
    - [Dostępne modele (2024/2025)](#dostępne-modele-20242025)
    - [Bedrock — Kluczowe Funkcje](#bedrock--kluczowe-funkcje)
    - [Bedrock Knowledge Bases — RAG Architecture](#bedrock-knowledge-bases--rag-architecture)
    - [Bedrock Guardrails](#bedrock-guardrails)
    - [Bedrock vs SageMaker vs Third-party](#bedrock-vs-sagemaker-vs-third-party)
    - [30.11 AI Services (Pre-built) — Gotowe Modele AWS](#3011-ai-services-pre-built--gotowe-modele-aws)
    - [Amazon Rekognition — Computer Vision](#amazon-rekognition--computer-vision)
    - [Amazon Comprehend — NLP](#amazon-comprehend--nlp)
    - [Amazon Textract — Document Analysis](#amazon-textract--document-analysis)
    - [Amazon Polly — Text-to-Speech](#amazon-polly--text-to-speech)
    - [Amazon Lex — Konwersacyjne AI (Chatbot)](#amazon-lex--konwersacyjne-ai-chatbot)
    - [Amazon Kendra — Intelligent Search](#amazon-kendra--intelligent-search)
    - [Amazon Forecast — Prognozowanie Szeregów Czasowych](#amazon-forecast--prognozowanie-szeregów-czasowych)
    - [Amazon Personalize — Rekomendacje](#amazon-personalize--rekomendacje)
    - [Amazon Translate — Tłumaczenie](#amazon-translate--tłumaczenie)
    - [Amazon Transcribe — Speech-to-Text](#amazon-transcribe--speech-to-text)
    - [30.12 AWS Panorama — Edge Computer Vision](#3012-aws-panorama--edge-computer-vision)
    - [30.13 Amazon Lookout — Anomaly Detection](#3013-amazon-lookout--anomaly-detection)
    - [30.14 Amazon CodeGuru — AI Code Review](#3014-amazon-codeguru--ai-code-review)
    - [30.15 AWS DeepLearning AMIs i Containers](#3015-aws-deeplearning-amis-i-containers)
    - [30.16 AWS Trainium i Inferentia — Custom ML Chips](#3016-aws-trainium-i-inferentia--custom-ml-chips)
    - [AWS Trainium](#aws-trainium)
    - [AWS Inferentia](#aws-inferentia)
    - [Porównanie chipów ML](#porównanie-chipów-ml)
    - [30.17 SageMaker vs Azure ML vs Google Vertex AI](#3017-sagemaker-vs-azure-ml-vs-google-vertex-ai)
    - [30.18 Bezpieczeństwo SageMaker i AI](#3018-bezpieczeństwo-sagemaker-i-ai)
    - [Sieć i izolacja](#sieć-i-izolacja)
    - [IAM dla SageMaker](#iam-dla-sagemaker)
    - [Szyfrowanie](#szyfrowanie-2)
    - [Compliance i audyt](#compliance-i-audyt)
    - [30.19 Well-Architected Framework — AI/ML](#3019-well-architected-framework--aiml)
    - [30.20 4 Domeny Egzaminacyjne SAP-C02](#3020-4-domeny-egzaminacyjne-sap-c02)
    - [Domena 1: Design Solutions for Organizational Complexity](#domena-1-design-solutions-for-organizational-complexity-4)
    - [Domena 2: Design for New Solutions](#domena-2-design-for-new-solutions-4)
    - [Domena 3: Continuous Improvement for Existing Solutions](#domena-3-continuous-improvement-for-existing-solutions-4)
    - [Domena 4: Accelerate Workload Migration and Modernization](#domena-4-accelerate-workload-migration-and-modernization-4)

## 1. AWS Directory Services, Identity Federation, IAM Identity Center

### 1.1 Przegląd i kontekst

> **Kontekst:** Każda firma ma użytkowników. Zarządzanie tymi użytkownikami to katalog (directory). W świecie on-premises dominuje **Microsoft Active Directory (AD)**. AWS oferuje kilka usług, które albo replikują AD w chmurze, albo łączą się z istniejącym AD, albo zastępują go uproszczoną wersją.

```
Użytkownicy → uwierzytelnianie → katalog → autoryzacja → zasoby
```

**Kluczowe pytanie egzaminacyjne:** Czy firma ma już AD on-premises? Czy potrzebuje pełnego AD w AWS? Czy wystarczy prosty LDAP? Odpowiedź determinuje wybór usługi.

**Odpowiedniki:**
| AWS | On-premises / Open Source | Azure |
|-----|--------------------------|-------|
| AWS Managed Microsoft AD | Windows Server Active Directory | Azure Active Directory Domain Services (AAD DS) |
| Simple AD | Samba 4 / OpenLDAP | brak bezpośredniego odpowiednika |
| AD Connector | AD proxy / ADFS Proxy | Azure AD Application Proxy |
| IAM Identity Center | ADFS + MIM / Okta | Azure Active Directory (Entra ID) |

---

### 1.2 AWS Managed Microsoft AD

**🔵 Co to jest?**
Zarządzana implementacja **Microsoft Active Directory** uruchomiona bezpośrednio w AWS (Region). AWS zarządza infrastrukturą (DC, replikacja, patching). Ty zarządzasz użytkownikami, grupami, GPO, Trust.

**Architektura:**
```mermaid
graph TD
    A[On-premises AD] -- Forest Trust --> B[AWS Managed Microsoft AD]
    B --> C[EC2 Windows joined to domain]
    B --> D[RDS SQL Server / Oracle]
    B --> E[WorkSpaces]
    B --> F[IAM Identity Center]
    B --> G[Amazon Chime / Connect]
```

**Tiery:**

| Feature | Standard Edition | Enterprise Edition |
|---------|-----------------|-------------------|
| Obiekty (users/groups/computers) | do 30 000 | do 500 000 |
| Przeznaczenie | małe/średnie firmy | duże enterprise |
| Multi-Region replication | ❌ | ✅ |
| Cena (Region us-east-1) | ~$0.10/h per DC | ~$0.35/h per DC |

**Kiedy używać:**
- ✅ Potrzebujesz pełnego AD w AWS (GPO, Kerberos, LDAP, DNS)
- ✅ Chcesz domain-join EC2, RDS, WorkSpaces
- ✅ Budujesz Trust z on-premises AD (obydwa katalogi żyją niezależnie)
- ✅ Potrzebujesz MFA z RADIUS

**Kiedy NIE używać:**
- ❌ Masz istniejące AD on-prem i chcesz go tylko rozszerzyć → użyj **AD Connector**
- ❌ Potrzebujesz prostego LDAP bez pełnego AD → użyj **Simple AD**

**Bezpieczeństwo:**
- Domain Admins i Enterprise Admins — AWS zarządza, **ty nie masz dostępu** do tych kont
- Możesz tworzyć własnych adminów w dedykowanej OU
- Szyfrowanie ruchu LDAPS (port 636) — włącz certyfikat
- Multi-AZ domyślnie (2 Domain Controllers w różnych AZ)
- Logi do CloudWatch Logs (Security Event logs)
- Integracja z AWS CloudTrail

**Niezawodność:**
- AWS automatycznie wykrywa i zastępuje uszkodzone DC
- Snapshot automatyczny co 24h (retencja 5 dni)
- Multi-AZ — brak single point of failure

**Integracja z innymi serwisami:**

```
AWS Managed Microsoft AD
├── EC2 (domain join)
├── RDS (Windows Auth dla SQL Server, Oracle, PostgreSQL)
├── WorkSpaces / AppStream 2.0
├── IAM Identity Center (identity source)
├── Amazon Chime / Connect / QuickSight
└── FSx for Windows File Server (wymagane!)
```

> ⚠️ **Egzamin:** `FSx for Windows File Server` **wymaga** AWS Managed Microsoft AD lub trust z on-prem AD. Simple AD i AD Connector **nie działają** z FSx for Windows.

---

### 1.3 Simple AD

**🟢 Co to jest?**
Lekka, tania, zarządzana usługa katalogowa oparta na **Samba 4** — kompatybilna z podzbiorem API Microsoft AD. Nie jest prawdziwym AD.

**Architektura:**
```mermaid
graph TD
    A[Simple AD] --> B[EC2 Linux / Windows]
    A --> C[WorkSpaces basic]
    A --> D[Aplikacje LDAP-compatible]
```

**Tiery:**

| Feature | Small | Large |
|---------|-------|-------|
| Użytkownicy | do 500 | do 5000 |
| Cena (us-east-1) | ~$0.05/h | ~$0.15/h |

**Co NIE działa z Simple AD:**
- ❌ Trust z Microsoft AD (on-prem lub AWS Managed)
- ❌ MFA (RADIUS)
- ❌ FSx for Windows File Server
- ❌ RDS SQL Server Windows Authentication
- ❌ PowerShell ADWS (AD Web Services)
- ❌ Schema extensions

**Kiedy używać:**
- ✅ Tania, prosta usługa LDAP/Kerberos dla małych aplikacji
- ✅ Nie masz on-premises AD i nie potrzebujesz pełnego AD
- ✅ Podstawowy domain-join dla EC2 (Linux + Windows)

**Linux analogy:** Samba 4 w trybie Active Directory Domain Controller — dokładnie to napędza Simple AD.

---

### 1.4 AD Connector

**🟡 Co to jest?**
**Proxy** — nie przechowuje żadnych danych katalogowych. Przekierowuje zapytania uwierzytelniania i autoryzacji do **istniejącego on-premises Active Directory** przez VPN lub Direct Connect.

**Architektura:**
```mermaid
graph TD
    A[AWS Services\nEC2 / WorkSpaces / Console] --> B[AD Connector]
    B -- VPN / Direct Connect --> C[On-premises\nActive Directory]
    C -- response --> B
    B -- auth result --> A
```

**Tiery:**

| Feature | Small | Large |
|---------|-------|-------|
| Concurrent auth | do 500 | do 5000 |

**Kiedy używać:**
- ✅ Masz AD on-premises i chcesz używać go do logowania do AWS Console / WorkSpaces
- ✅ Nie chcesz synchronizować/replikować danych katalogowych do AWS
- ✅ Compliance wymaga, żeby dane użytkowników zostały on-premises
- ✅ Domain-join EC2 do on-premises AD

**Kiedy NIE używać:**
- ❌ VPN/Direct Connect niedostępny → wszystko przestaje działać
- ❌ Chcesz AWS-native AD (nie masz on-prem) → AWS Managed Microsoft AD
- ❌ FSx for Windows → nie wspierany

**Ograniczenia:**
- Wymaga działającego VPN lub Direct Connect (single point of failure jeszcze przed AWS)
- Nie przechowuje kont → nie działa offline
- Nie możesz robić domain-join jeśli on-prem AD niedostępny

> ⚠️ **Egzamin:** AD Connector = "bridge" do on-prem AD. Żadne dane NIE są przechowywane w AWS.

---

### 1.5 Identity Federation

**🔴 Co to jest?**
Mechanizm umożliwiający **zewnętrznym tożsamościom** (spoza AWS IAM) dostęp do zasobów AWS **bez tworzenia kont IAM**. Firma używa własnego IdP (Identity Provider), AWS mu ufa.

**Protokoły federacji:**

| Protokół | Opis | Przykłady IdP |
|----------|------|---------------|
| **SAML 2.0** | XML-based, enterprise SSO | ADFS, Okta, Azure AD, PingFederate |
| **OpenID Connect (OIDC)** | Token-based (JWT), web/mobile | Google, Facebook, Amazon Cognito |
| **Web Identity** | OIDC dla aplikacji mobilnych | Amazon, Google, Facebook |
| **AWS SSO / IAM Identity Center** | Managed federation w AWS | AD, External IdP |

#### SAML 2.0 Federation — przepływ

```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant IdP as IdP (ADFS/Okta)
    participant STS as AWS STS
    participant S3 as AWS Resource

    U->>IdP: 1. Zaloguj się
    IdP-->>U: 2. SAML Assertion (XML)
    U->>STS: 3. AssumeRoleWithSAML(SAML assertion)
    STS-->>U: 4. Temporary credentials (15min - 12h)
    U->>S3: 5. Dostęp z temp credentials
```

**Kluczowe API:**

| API Call | Protokół | Kto wywołuje |
|----------|----------|-------------|
| `AssumeRoleWithSAML` | SAML 2.0 | Aplikacja po otrzymaniu SAML assertion |
| `AssumeRoleWithWebIdentity` | OIDC/Web Identity | Aplikacja mobilna/web |
| `AssumeRole` | IAM → IAM | Konto AWS do konta AWS |
| `GetFederationToken` | - | Legacy, dla proxy apps |

#### SAML Federation dla AWS Console

```mermaid
sequenceDiagram
    participant U as User
    participant IdP as ADFS
    participant STS as AWS STS
    participant CON as AWS Console

    U->>IdP: Logowanie przez portal ADFS
    IdP-->>U: SAML Assertion
    U->>STS: POST /saml (endpoint: signin.aws.amazon.com/saml)
    STS-->>CON: Redirect z temporary credentials
    CON-->>U: Dostęp do konsoli
```

> ⚠️ **Egzamin:** URL do konsoli przez SAML to `https://signin.aws.amazon.com/saml`

#### Web Identity Federation (Cognito)

```mermaid
graph LR
    A[Mobile App] --> B[Google/Facebook/Amazon]
    B --> C[Token OIDC]
    C --> D[Amazon Cognito]
    D --> E[AWS STS]
    E --> F[Temp Credentials]
    F --> G[S3/DynamoDB/API GW]
```

> ⚠️ **Egzamin:** Dla aplikacji mobilnych zawsze preferuj **Amazon Cognito** zamiast bezpośredniego `AssumeRoleWithWebIdentity`. Cognito dodaje: user pools, synchronizację danych, offline access.

**Custom Identity Broker:**
Gdy IdP nie obsługuje SAML/OIDC — budujesz własny broker który:
1. Uwierzytelnia użytkownika
2. Wywołuje `GetFederationToken` lub `AssumeRole`
3. Zwraca tymczasowe credentials

---

### 1.6 IAM Identity Center (SSO)

**🟣 Co to jest?**
Centralny serwis do zarządzania **dostępem SSO** do wielu kont AWS i aplikacji biznesowych (Salesforce, Office 365, itp.). Następca AWS Single Sign-On (AWS SSO).

**Architektura:**
```mermaid
graph TD
    A[IAM Identity Center] --> B[Identity Source]
    B --> B1[Built-in Identity Store]
    B --> B2[AWS Managed Microsoft AD]
    B --> B3[External IdP via SAML 2.0\nOkta / Azure AD / ADFS]

    A --> C[Permission Sets]
    C --> D[AWS Account 1]
    C --> E[AWS Account 2]
    C --> F[AWS Account N]

    A --> G[Application Assignments]
    G --> H[Salesforce]
    G --> I[Office 365]
    G --> J[Custom SAML Apps]
```

**Kluczowe pojęcia:**

| Pojęcie | Opis |
|---------|------|
| **Permission Set** | Zestaw uprawnień (jak rola IAM) przypisany użytkownikowi/grupie do konta |
| **Assignment** | Powiązanie: user/group + permission set + account |
| **Identity Store** | Wbudowany katalog użytkowników (jeśli nie używasz zewnętrznego) |
| **SCIM** | Protokół automatycznej synchronizacji użytkowników z zewnętrznego IdP |
| **MFA** | Wymuszanie MFA na poziomie IAM Identity Center |

**Identity Sources — porównanie:**

| Źródło | Sync użytkowników | Trust | Uwagi |
|--------|-------------------|-------|-------|
| Identity Center directory | Wbudowany | - | Najprostsze, brak integracji z AD |
| AWS Managed Microsoft AD | Automatyczny | Kerberos | Najlepsza integracja AD w AWS |
| External IdP (Okta, Azure AD) | SCIM 2.0 | SAML 2.0 | Dla istniejących enterprise IdP |

**Przepływ logowania:**
```mermaid
sequenceDiagram
    participant U as User
    participant IC as IAM Identity Center Portal
    participant IdP as External IdP (Okta)
    participant ACC as AWS Account

    U->>IC: Otwiera access portal (awsapps.com/start)
    IC->>IdP: SAML AuthnRequest
    IdP-->>U: Prośba o logowanie
    U->>IdP: Credentials + MFA
    IdP-->>IC: SAML Assertion
    IC-->>U: Lista kont i ról
    U->>ACC: Klikam rolę → temp credentials via STS
    ACC-->>U: Dostęp do konsoli / CLI / SDK
```

**CLI Integration:**
```bash
# Konfiguracja profilu SSO
aws configure sso
# SSO start URL: https://your-domain.awsapps.com/start
# SSO Region: eu-west-1

# Logowanie
aws sso login --profile my-dev-account

# Użycie profilu
aws s3 ls --profile my-dev-account
```

**Kiedy używać IAM Identity Center:**
- ✅ Multi-account AWS Organizations — zarządzasz wieloma kontami
- ✅ SSO do aplikacji SaaS (Salesforce, Box, Dropbox)
- ✅ Centralizacja dostępu zamiast per-konto IAM users
- ✅ Audyt: kto ma dostęp do czego w całej organizacji

**Bezpieczeństwo:**
- Temporary credentials (STS) — nie ma long-term keys dla użytkowników
- MFA enforcement — per user lub per account
- CloudTrail logi wszystkich logowań
- Access Reviews przez AWS IAM Access Analyzer

---

### 1.7 Porównanie usług

```mermaid
flowchart TD
    Q1{Masz AD on-premises?} -->|Tak| Q2
    Q1 -->|Nie| Q3

    Q2{Dane mogą trafić do AWS?} -->|Nie - compliance| AD_CONN[AD Connector\nProxy do on-prem]
    Q2 -->|Tak| Q4

    Q4{Potrzebujesz Trust\noba katalogi niezależne?} -->|Tak| MMAD[AWS Managed\nMicrosoft AD\n+ Trust]
    Q4 -->|Nie, chcesz przenieść AD do AWS| MMAD

    Q3{Potrzebujesz pełnego AD\nGPO, Kerberos, LDAP?} -->|Tak| MMAD2[AWS Managed\nMicrosoft AD]
    Q3 -->|Nie, wystarczy prosty LDAP| Q5

    Q5{Skala?} -->|< 500-5000 users| SIMPLE[Simple AD\nSamba 4]
    Q5 -->|SSO do wielu kont AWS| SSO[IAM Identity Center]
```

**Tabela porównawcza:**

| Feature | AWS Managed Microsoft AD | Simple AD | AD Connector | IAM Identity Center |
|---------|--------------------------|-----------|--------------|---------------------|
| Prawdziwe Microsoft AD | ✅ | ❌ (Samba 4) | ❌ (proxy) | ❌ (federation) |
| Przechowuje dane w AWS | ✅ | ✅ | ❌ | ✅ (identity store) |
| Trust z on-prem AD | ✅ | ❌ | ✅ (przekazuje) | ✅ (przez SAML/AD) |
| FSx for Windows | ✅ | ❌ | ❌ | ❌ |
| RDS Windows Auth | ✅ | ❌ | ❌ | ❌ |
| WorkSpaces | ✅ | ✅ | ✅ | ✅ |
| MFA (RADIUS) | ✅ | ❌ | ✅ | ✅ |
| GPO | ✅ | ❌ | ❌ | ❌ |
| SSO do aplikacji SaaS | ❌ (wymaga IC) | ❌ | ❌ | ✅ |
| Multi-account SSO | ❌ (wymaga IC) | ❌ | ❌ | ✅ |
| Wymaga VPN/DX do działania | ❌ | ❌ | ✅ | ❌ |
| Cena (relative) | $$$ | $ | $$ | $ |

---

### 1.8 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Firma ma 500 kont AWS w Organizations, ADFS on-premises, i chce centralnego SSO.

**Rozwiązanie:**
```
ADFS on-prem → IAM Identity Center (External IdP via SAML) → Permission Sets → 500 kont
```
- SCIM 2.0 do automatycznej synchronizacji grup z ADFS
- Permission Sets mapują grupy AD na role IAM per konto
- Access Portal daje użytkownikom self-service widok kont

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Nowa aplikacja multi-tenant na EKS. Użytkownicy logują się przez Google/Facebook.

**Rozwiązanie:**
```
Mobile/Web App → Amazon Cognito (User Pools + Identity Pools) → STS → AWS Resources
```
- Cognito User Pools: federacja z Google/Facebook
- Cognito Identity Pools: mapowanie na IAM Role
- Nie używaj bezpośrednio `AssumeRoleWithWebIdentity` → Cognito jako best practice

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma ma AWS Managed Microsoft AD. WorkSpaces działają. Chcą dodać MFA.

**Rozwiązanie:**
- Dodaj RADIUS server (np. AWS MFA lub zewnętrzny)
- Skonfiguruj MFA w AWS Managed Microsoft AD settings
- Alternatywnie: migruj do IAM Identity Center (ma wbudowane MFA TOTP/WebAuthn)

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Migracja 2000 Windows serwerów. Wszystkie są domain-joined do on-prem AD.

**Strategia:**
```
Faza 1: AD Connector → szybkie domain-join w AWS, dane zostają on-prem
Faza 2: AWS Managed Microsoft AD + Trust → stopniowa migracja katalogów
Faza 3: IAM Identity Center → SSO dla wszystkich kont AWS
```

---

## 2. RADIUS, Amazon Cognito i AWS STS

### 2.1 RADIUS — protokół uwierzytelniania sieciowego

**🔵 Co to jest?**
**RADIUS** (Remote Authentication Dial-In User Service) to protokół sieciowy z 1991 roku (RFC 2865) służący do **centralnego uwierzytelniania, autoryzacji i rozliczania (AAA)** dla użytkowników łączących się z siecią. Działa w modelu klient-serwer na UDP port **1812** (auth) i **1813** (accounting).

**Analogia z życia:** RADIUS to jak portier w dużej firmie. Ktokolwiek chce wejść (VPN, WiFi, VoIP, AWS MFA) — pyta portiera. Portier sprawdza listę w swoim systemie i mówi: "wpuść" lub "nie wpuść".

**Architektura RADIUS:**
```mermaid
graph LR
    U[Użytkownik] -->|credentials| C[RADIUS Client\nNAS - Network Access Server\nnp. VPN Gateway, WiFi AP]
    C -->|Access-Request UDP 1812| R[RADIUS Server\nnp. FreeRADIUS, NPS, Cisco ISE]
    R -->|sprawdza| DB[(User DB\nAD / LDAP)]
    R -->|Access-Accept / Access-Reject / Access-Challenge| C
    C -->|wpuść / odrzuć| U
```

**AAA w RADIUS:**

| Litera | Funkcja | Opis |
|--------|---------|------|
| **A**uthentication | Uwierzytelnianie | Kto jesteś? (login/hasło, OTP, cert) |
| **A**uthorization | Autoryzacja | Co możesz robić? (VLAN, ACL, bandwidth) |
| **A**ccounting | Rozliczanie | Co zrobiłeś? (logi sesji, czas, transfer) |

**Kody odpowiedzi RADIUS:**
- `Access-Accept` — uwierzytelnianie OK
- `Access-Reject` — odmowa
- `Access-Challenge` — wymagany dodatkowy krok (np. OTP) — **to właśnie MFA!**

**RADIUS w AWS — gdzie jest używany:**

| Serwis AWS | Rola RADIUS |
|-----------|-------------|
| **AWS Managed Microsoft AD** | MFA dla domain-joined usług (WorkSpaces, console) |
| **AD Connector** | MFA przekazywane do on-prem RADIUS |
| **AWS Client VPN** | Uwierzytelnianie użytkowników VPN |
| **Amazon WorkSpaces** | MFA przez RADIUS serwer |

**Jak AWS Managed Microsoft AD używa RADIUS (MFA):**
```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant WS as WorkSpaces / App
    participant AD as AWS Managed AD
    participant R as RADIUS Server (on-prem lub EC2)
    participant MFA as MFA App (Google Auth / RSA Token)

    U->>WS: Login: jan.kowalski / hasło
    WS->>AD: Sprawdź credentials
    AD->>R: Access-Request (username)
    R-->>U: Access-Challenge: podaj OTP
    U->>MFA: Sprawdź kod TOTP
    MFA-->>U: 123456
    U->>R: OTP = 123456
    R-->>AD: Access-Accept
    AD-->>WS: Uwierzytelniony
    WS-->>U: Dostęp
```

**RADIUS vs inne protokoły MFA:**

| Protokół | Transport | MFA typ | Używany w AWS |
|----------|-----------|---------|---------------|
| **RADIUS** | UDP, stary, prosty | OTP, token, TOTP | AD, AD Connector, Client VPN |
| **SAML 2.0** | HTTPS/XML | Dowolny (przez IdP) | IAM Identity Center, federacja |
| **FIDO2/WebAuthn** | HTTPS | Hardware key, biometria | IAM Identity Center |
| **TOTP (Virtual MFA)** | App-based | TOTP RFC 6238 | IAM Identity Center, IAM Users |

**Open-source / Linux odpowiednik:** `FreeRADIUS` — najpopularniejszy open-source RADIUS server. AWS nie oferuje zarządzanego RADIUS — używasz FreeRADIUS na EC2 lub on-prem NPS (Windows Network Policy Server).

> ⚠️ **Egzamin:** Gdy widzisz "MFA dla WorkSpaces / AWS Managed Microsoft AD" → myśl RADIUS. IAM Identity Center ma wbudowane MFA i **nie potrzebuje** RADIUS.

---

### 2.2 Amazon Cognito — przegląd

**🟣 Co to jest?**
Amazon Cognito to zarządzana usługa **uwierzytelniania i autoryzacji dla aplikacji web i mobilnych**. Składa się z dwóch niezależnych komponentów z różnymi zadaniami:

```
Amazon Cognito
├── User Pools      → KTO jesteś? (uwierzytelnianie, zarządzanie użytkownikami)
└── Identity Pools  → CO możesz robić? (autoryzacja, tymczasowe credentials AWS)
```

**Odpowiedniki:**

| AWS | Azure | Open-source / Self-hosted |
|-----|-------|--------------------------|
| Cognito User Pools | Azure AD B2C | Keycloak, Auth0 (SaaS) |
| Cognito Identity Pools | Azure Managed Identity | AWS STS bezpośrednio |

---

### 2.3 Cognito User Pools

**🔵 Co to jest?**
User Pool to **katalog użytkowników** + **serwer uwierzytelniania** dla Twojej aplikacji. Zarządza rejestracją, logowaniem, hasłami, MFA, weryfikacją email/telefonu. Po udanym logowaniu zwraca **JWT tokeny**.

**Tokeny JWT zwracane przez User Pool:**

| Token | Zawartość | Ważność | Użycie |
|-------|-----------|---------|--------|
| **ID Token** | Tożsamość użytkownika (name, email, custom attrs) | 1h domyślnie | Backend: kto to jest |
| **Access Token** | Uprawnienia (scopes, groups) | 1h domyślnie | Wywołanie chronionych API |
| **Refresh Token** | Odświeżenie Access/ID Token | 30 dni domyślnie | Nowe tokeny bez re-logowania |

**Architektura User Pool:**
```mermaid
graph TD
    A[App: web / mobile] --> B[Cognito User Pool]
    B --> C{Źródło tożsamości}
    C --> C1[Wbudowany katalog\nCognito native users]
    C --> C2[Federacja: Google / Facebook\nAmazon / Apple OIDC]
    C --> C3[Enterprise: SAML 2.0\nADFS / Okta / Azure AD]
    B -->|JWT Tokens| A
    A -->|ID Token w header| D[API Gateway / ALB]
    D -->|weryfikuje JWT| E[Backend Lambda / ECS]
```

**Kluczowe features User Pool:**

| Feature | Opis |
|---------|------|
| **User directory** | Przechowuje username, email, phone, custom attributes |
| **Sign-up / Sign-in** | Hosted UI lub custom UI przez Amplify/SDK |
| **MFA** | TOTP (Google Authenticator), SMS |
| **Password policy** | Min length, complexity, expiry |
| **Account recovery** | Email / SMS |
| **Lambda Triggers** | Pre/post auth hooks, custom messages, migration |
| **Advanced Security** | Risk-based auth, adaptive MFA, compromised credentials |
| **Groups** | Mapowanie użytkowników na role (przez ID Token `cognito:groups`) |

**Lambda Triggers (ważne na egzamin):**

| Trigger | Kiedy odpala | Przykład użycia |
|---------|-------------|-----------------|
| Pre Sign-Up | Przed rejestracją | Blokuj domenę email |
| Post Confirmation | Po potwierdzeniu konta | Dodaj do DynamoDB |
| Pre Authentication | Przed logowaniem | Blokuj według IP |
| Post Authentication | Po logowaniu | Log do CloudWatch |
| Pre Token Generation | Przed wydaniem tokenu | Dodaj custom claims do JWT |
| User Migration | Logowanie nieistniejącego usera | Migruj z legacy systemu |
| Custom Message | Weryfikacja email/SMS | Własna treść wiadomości |

**User Pool + API Gateway:**
```mermaid
sequenceDiagram
    participant App as Mobile App
    participant UP as Cognito User Pool
    participant APIGW as API Gateway
    participant Lambda as Lambda

    App->>UP: login(email, password)
    UP-->>App: ID Token + Access Token (JWT)
    App->>APIGW: GET /api/data\nAuthorization: Bearer <AccessToken>
    APIGW->>UP: Weryfikuj JWT (public key)
    UP-->>APIGW: Valid / Invalid
    APIGW->>Lambda: Przekaż request
    Lambda-->>App: Response
```

**User Pool + ALB (Application Load Balancer):**
ALB ma wbudowaną integrację z Cognito — może wymagać logowania przed dostępem do backendu. Bez pisania kodu!

```
Browser → ALB → Redirect do Cognito Hosted UI → Login → ALB przekazuje JWT → Target (ECS/EC2)
```

---

### 2.4 Cognito Identity Pools

**🟢 Co to jest?**
Identity Pool (zwany też **Federated Identity**) daje uwierzytelnionym (lub anonimowym) użytkownikom **tymczasowe credentials AWS (STS)** do bezpośredniego dostępu do serwisów AWS (S3, DynamoDB, API GW itp.).

**Kluczowa różnica:** User Pool mówi "kto to jest". Identity Pool mówi "co może robić w AWS".

**Architektura Identity Pool:**
```mermaid
graph TD
    A[App] --> B{Źródło tożsamości\ndla Identity Pool}
    B --> B1[Cognito User Pool\nJWT token]
    B --> B2[Google / Facebook / Amazon\nOIDC token]
    B --> B3[SAML 2.0 token]
    B --> B4[Developer Authenticated\nwłasny backend]
    B --> B5[Unauthenticated\nAnonymous access]

    B --> C[Cognito Identity Pool]
    C --> D[AWS STS\nAssumeRoleWithWebIdentity]
    D --> E[Temporary Credentials\nAccess Key + Secret + Token]
    E --> F[AWS Resources\nS3 / DynamoDB / API GW]
```

**Role w Identity Pool:**

| Rola | Kiedy przypisana |
|------|-----------------|
| **Authenticated Role** | Użytkownik zalogowany (przez User Pool lub external IdP) |
| **Unauthenticated Role** | Anonimowy gość (opcjonalne, zawęź uprawnienia!) |
| **Role Mapping (Rules)** | Różne role na podstawie claims w tokenie (np. admins → admin-role) |

**Przykład: Role Mapping na podstawie grupy Cognito:**
```json
// Jeśli cognito:groups zawiera "admins" → przypisz AdminRole
// Jeśli cognito:groups zawiera "readers" → przypisz ReadOnlyRole
```

**Przykładowa polityka IAM dla Authenticated Role (per-user S3):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject", "s3:PutObject"],
    "Resource": "arn:aws:s3:::my-app-bucket/${cognito-identity.amazonaws.com:sub}/*"
  }]
}
```
> `${cognito-identity.amazonaws.com:sub}` — dynamicznie podstawia Identity ID użytkownika. Każdy user widzi tylko swój "folder" w S3.

---

### 2.5 User Pools vs Identity Pools — porównanie

```mermaid
graph LR
    subgraph "User Pool"
        UP1[Zarządza użytkownikami]
        UP2[Logowanie / rejestracja]
        UP3[MFA, hasła]
        UP4[Zwraca JWT]
    end

    subgraph "Identity Pool"
        IP1[Federuje tożsamości]
        IP2[Mapuje na IAM Role]
        IP3[Wywołuje STS]
        IP4[Zwraca AWS Credentials]
    end

    UP4 -->|JWT jako input| IP1
```

**Tabela porównawcza:**

| Aspekt | User Pool | Identity Pool |
|--------|-----------|---------------|
| **Cel** | Uwierzytelnianie użytkowników aplikacji | Autoryzacja dostępu do AWS |
| **Co zwraca** | JWT (ID/Access/Refresh Token) | Tymczasowe credentials AWS (STS) |
| **Protokół** | OIDC / OAuth 2.0 | AWS STS (AssumeRoleWithWebIdentity) |
| **Przechowuje użytkowników** | ✅ Własny katalog | ❌ Tylko Identity ID (UUID) |
| **Federacja zewnętrzna** | Google, FB, SAML, OIDC | User Pool, Google, FB, SAML, Custom |
| **Dostęp anonimowy** | ❌ | ✅ (Unauthenticated role) |
| **Integracja API GW/ALB** | ✅ bezpośrednio | ❌ (przez STS credentials) |
| **Odpowiednik Azure** | Azure AD B2C | Azure Managed Identity |

**Typowy pattern — oba razem:**
```mermaid
sequenceDiagram
    participant App as Mobile App
    participant UP as Cognito User Pool
    participant IP as Cognito Identity Pool
    participant STS as AWS STS
    participant S3 as Amazon S3

    App->>UP: 1. Login (email/password)
    UP-->>App: 2. JWT ID Token
    App->>IP: 3. GetCredentialsForIdentity(ID Token)
    IP->>STS: 4. AssumeRoleWithWebIdentity
    STS-->>IP: 5. Temp credentials (15min-1h)
    IP-->>App: 6. AccessKey + SecretKey + SessionToken
    App->>S3: 7. GetObject z temp credentials
```

---

### 2.6 AWS STS — Security Token Service

**🔴 Co to jest?**
AWS Security Token Service to **globalny serwis** wydający **tymczasowe, ograniczone credentials** dla AWS. Każde tymczasowe poświadczenie to trójka:
- `AccessKeyId` (ASIA...)
- `SecretAccessKey`
- `SessionToken` (wymagany razem z kluczem)

**Kluczowa zasada:** STS **nie przechowuje tożsamości** — on tylko wydaje tymczasowe credentials na podstawie zaufania (trust policy).

**Architektura STS:**
```mermaid
graph TD
    STS[AWS STS\nglobalny endpoint\nsts.amazonaws.com]
    STS --> R1[AssumeRole\nIAM → IAM cross-account]
    STS --> R2[AssumeRoleWithSAML\nSAML 2.0 federation]
    STS --> R3[AssumeRoleWithWebIdentity\nOIDC / Cognito]
    STS --> R4[GetSessionToken\nIAM User + MFA]
    STS --> R5[GetFederationToken\nlegacy proxy apps]
    STS --> R6[DecodeAuthorizationMessage\ndebug denied requests]
```

**Jak działają tymczasowe credentials:**
```mermaid
sequenceDiagram
    participant C as Caller (user/app/service)
    participant STS as AWS STS
    participant R as IAM Role
    participant S3 as AWS Resource

    C->>R: Sprawdź Trust Policy — czy C może AssumeRole?
    R-->>C: Trust Policy pozwala
    C->>STS: AssumeRole(RoleArn, SessionName, Duration)
    STS->>R: Weryfikacja Trust Policy
    STS-->>C: AccessKeyId + SecretKey + SessionToken
    C->>S3: API call z tymczasowymi credentials
    S3->>IAM: Weryfikuj uprawnienia (Permission Policy roli)
    IAM-->>S3: Allow / Deny
    S3-->>C: Response
```

**Endpoint STS — regionalne vs globalne:**

| Typ | Endpoint | Uwagi |
|-----|----------|-------|
| Global | `sts.amazonaws.com` | us-east-1, starszy |
| Regional | `sts.<region>.amazonaws.com` | Zalecane — wyższy availability, niższe latency |

> ⚠️ **Egzamin:** Używaj **regionalnych endpointów STS** dla lepszej dostępności. Jeśli us-east-1 ma problemy, globalny endpoint może nie działać.

---

### 2.7 STS API — porównanie wywołań

| API | Caller | Protokół | Max duration | Kiedy użyć |
|-----|--------|----------|-------------|------------|
| **AssumeRole** | IAM User, Role, Service | IAM Trust | 15min – 12h | Cross-account, service delegation |
| **AssumeRoleWithSAML** | Zewnętrzny user (SAML) | SAML 2.0 | 15min – 12h | ADFS, Okta → AWS |
| **AssumeRoleWithWebIdentity** | Zewnętrzny user (OIDC) | OIDC/JWT | 15min – 12h | Cognito, Google, mobile apps |
| **GetSessionToken** | IAM User | IAM + MFA | 15min – 36h | IAM User z MFA, wymuszenie MFA |
| **GetFederationToken** | IAM User | IAM | 15min – 36h | Legacy proxy app, custombrokery |
| **DecodeAuthorizationMessage** | Dowolny | - | - | Debug "Access Denied" |

**AssumeRole — cross-account:**
```mermaid
graph LR
    subgraph "Account A (Dev)"
        IAM_USER[IAM User / Role]
    end
    subgraph "Account B (Prod)"
        ROLE[IAM Role\nTrust Policy: Account A]
        S3[S3 Bucket]
    end

    IAM_USER -->|AssumeRole| ROLE
    ROLE -->|Temp Credentials| IAM_USER
    IAM_USER -->|Dostęp| S3
```

**Trust Policy dla cross-account AssumeRole:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::111122223333:root"
    },
    "Action": "sts:AssumeRole",
    "Condition": {
      "BoolIfExists": {
        "aws:MultiFactorAuthPresent": "true"
      }
    }
  }]
}
```

**GetSessionToken — wymuszanie MFA dla IAM Users:**
```bash
# Pobierz tymczasowe credentials z MFA
aws sts get-session-token \
  --serial-number arn:aws:iam::123456789:mfa/jan.kowalski \
  --token-code 123456 \
  --duration-seconds 3600
# Zwraca: AccessKeyId, SecretAccessKey, SessionToken, Expiration
```

**AssumeRole z External ID (ochrona przed confused deputy):**
```json
{
  "Condition": {
    "StringEquals": {
      "sts:ExternalId": "unique-customer-id-12345"
    }
  }
}
```
> **Confused Deputy Problem:** Bez External ID złośliwa firma mogłaby użyć Twojego AWS konta do ataku przez współdzielonego AWS partnera. External ID to secret znany tylko Tobie i partnerowi.

**Session Policies — zawężanie uprawnień w locie:**
```bash
# AssumeRole z dodatkową session policy (zawęża uprawnienia roli)
aws sts assume-role \
  --role-arn arn:aws:iam::123456789:role/AdminRole \
  --role-session-name my-session \
  --policy '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":"s3:*","Resource":"*"}]}'
# Wynikowe uprawnienia = AdminRole AND s3:* (intersection, nie union)
```

**STS Condition Keys (ważne na egzamin):**

| Condition Key | Znaczenie |
|--------------|-----------|
| `aws:MultiFactorAuthPresent` | Czy użyto MFA |
| `aws:MultiFactorAuthAge` | Ile sekund temu MFA było użyte |
| `sts:ExternalId` | External ID dla cross-account |
| `sts:RoleSessionName` | Nazwa sesji (audyt w CloudTrail) |
| `aws:RequestedRegion` | Region API call |

---

### 2.8 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Korporacja ma 50 000 pracowników w Azure AD. Chce dostępu do AWS bez tworzenia IAM users.

**Rozwiązanie:**
```
Azure AD (IdP SAML 2.0) → IAM Identity Center → Permission Sets → AWS Accounts
```
Lub dla starszego podejścia:
```
Azure AD → SAML Assertion → STS AssumeRoleWithSAML → Temp Credentials → AWS
```
SCIM 2.0 synchronizuje grupy z Azure AD do IAM Identity Center.

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Aplikacja mobilna (iOS/Android) z 1M użytkowników. Login Google/Facebook. Dane użytkownika w DynamoDB, media w S3.

**Rozwiązanie:**
```
App → Cognito User Pool (Google/Facebook federation)
    → JWT Token
    → Cognito Identity Pool
    → STS (per-user IAM Role)
    → S3: bucket/cognito-identity-id/*
    → DynamoDB: tylko wiersze gdzie userId = cognito:sub
```
Policy DynamoDB z variable substitution:
```json
"Condition": {"ForAllValues:StringEquals": {"dynamodb:LeadingKeys": ["${cognito-identity.amazonaws.com:sub}"]}}
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma używa direct `AssumeRoleWithWebIdentity` z Google w aplikacji mobilnej. Chcą dodać rejestrację, MFA, offline sync.

**Rozwiązanie:** Migracja do Cognito User Pools + Identity Pools:
- User Pools: rejestracja, MFA, password reset, session management
- Identity Pools: zachowanie mapowania na AWS role
- Brak zmian w backendzie S3/DynamoDB — tylko credentials source zmieniony

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Legacy app ma własną bazę użytkowników (MySQL). Migracja do cloud-native auth bez wymuszania re-rejestracji.

**Rozwiązanie:** Cognito User Pool z **User Migration Lambda Trigger**:
```mermaid
graph TD
    U[Użytkownik loguje się\npierwszy raz w Cognito] --> T[Lambda: User Migration Trigger]
    T --> MySQL[(Stara baza MySQL)]
    MySQL -->|user istnieje, hasło OK| T
    T -->|utwórz usera w Cognito| Cognito[Cognito User Pool]
    Cognito --> U
```
Zero downtime migration — użytkownicy migrują "lazy" przy pierwszym logowaniu.

---

## 3. VPC, Subnets, Route Tables, Security Groups

### 3.1 VPC — Virtual Private Cloud

**🔵 Co to jest?**
VPC to **logicznie izolowana sieć wirtualna** w AWS, w której uruchamiasz zasoby (EC2, RDS, Lambda w VPC itd.). Pełna kontrola nad przestrzenią adresową, routingiem, bramami i regułami bezpieczeństwa.

**Analogia:** VPC to Twoje własne centrum danych w AWS — masz własną sieć, własne podsieci, własne firewalle — ale bez fizycznego hardware.

**Odpowiedniki:**

| AWS | Azure | GCP | On-prem |
|-----|-------|-----|---------|
| VPC | Virtual Network (VNet) | VPC Network | LAN / VLAN segment |
| Subnet | Subnet | Subnet | VLAN |
| Security Group | NSG (Network Security Group) | Firewall Rules | Stateful Firewall |
| NACL | NSG na poziomie subnetu | VPC Firewall (stateless rules) | ACL na routerze |
| IGW | Internet Gateway (wbudowany) | Cloud Router | Router brzegowy |
| NAT Gateway | NAT Gateway | Cloud NAT | NAT Router |

**Limity VPC (ważne na egzamin):**

| Zasób | Limit domyślny | Max (po zwiększeniu) |
|-------|---------------|---------------------|
| VPC per Region | 5 | 100+ |
| Subnets per VPC | 200 | 200 |
| IPv4 CIDR blocks per VPC | 5 | 5 |
| IPv6 CIDR blocks per VPC | 1 | 1 |
| Route Tables per VPC | 200 | 200 |
| Security Groups per VPC | 2500 | 2500 |
| Rules per Security Group | 60 inbound + 60 outbound | 1000 |
| Security Groups per ENI | 5 | 16 |

**Default VPC:**
Każde konto AWS ma **Default VPC** per Region (tworzony automatycznie):
- CIDR: `172.31.0.0/16`
- Default subnet per AZ: `/20` (np. `172.31.0.0/20`, `172.31.16.0/20`, ...)
- Internet Gateway podłączony
- Wszystkie subnety są publiczne (route do IGW)
- Default Security Group, Default NACL, Default Route Table

> ⚠️ **Egzamin:** Nigdy nie używaj Default VPC w produkcji. Stwórz własne VPC z przemyślanym CIDR.

**Projektowanie CIDR:**

```
RFC 1918 — prywatne zakresy IP:
10.0.0.0/8      → 16 777 214 hostów  (duże enterprise)
172.16.0.0/12   → 1 048 574 hostów   (medium)
192.168.0.0/16  → 65 534 hostów      (małe sieci)

Zalecenia AWS:
- Min VPC CIDR: /28 (14 adresów)
- Max VPC CIDR: /16 (65 536 adresów)
- Planuj na przyszłość — za małe CIDR = ból
- Unikaj nakładania się z on-prem i innymi VPC (VPC Peering, TGW)
```

**Architektura typowego VPC (3-tier):**
```mermaid
graph TB
    subgraph VPC ["VPC: 10.0.0.0/16"]
        subgraph AZ1 ["AZ: eu-west-1a"]
            PUB1["Public Subnet\n10.0.1.0/24"]
            PRV1["Private Subnet\n10.0.3.0/24"]
            DATA1["Data Subnet\n10.0.5.0/24"]
        end
        subgraph AZ2 ["AZ: eu-west-1b"]
            PUB2["Public Subnet\n10.0.2.0/24"]
            PRV2["Private Subnet\n10.0.4.0/24"]
            DATA2["Data Subnet\n10.0.6.0/24"]
        end
        IGW["Internet Gateway"]
        NAT1["NAT Gateway\n(AZ1)"]
        NAT2["NAT Gateway\n(AZ2)"]
    end
    INET((Internet)) --> IGW
    IGW --> PUB1
    IGW --> PUB2
    PUB1 --> NAT1
    PUB2 --> NAT2
    NAT1 --> PRV1
    NAT2 --> PRV2
    PRV1 --> DATA1
    PRV2 --> DATA2
```

---

### 3.2 Subnets — podsieci

**🟢 Co to jest?**
Subnet to **podział CIDR VPC na mniejsze bloki** przypisane do konkretnej **Availability Zone**. Zasoby (EC2, RDS itd.) żyją w subnetach, nie w VPC.

**Kluczowa zasada:** Subnet = 1 AZ. Jeden AZ może mieć wiele subnetów.

**Zarezerwowane adresy IP (każdy subnet):**
AWS rezerwuje **5 adresów** w każdym subnecie:

| Adres | Przykład dla 10.0.1.0/24 | Opis |
|-------|--------------------------|------|
| Sieciowy | 10.0.1.0 | Adres sieci |
| Router VPC | 10.0.1.1 | AWS VPC Router |
| DNS | 10.0.1.2 | Amazon DNS (Route 53 Resolver) |
| Przyszłe | 10.0.1.3 | Zarezerwowany przez AWS |
| Broadcast | 10.0.1.255 | Broadcast (AWS go nie używa, ale rezerwuje) |

> ⚠️ **Egzamin:** `/28` (16 adresów) – 5 zarezerwowanych = **11 użytkowych**. To minimum dla większości zasobów.

**Typy subnetów:**

| Typ | Definicja | Route do IGW | Przykład użytku |
|-----|-----------|-------------|-----------------|
| **Public** | Ma route `0.0.0.0/0 → IGW` | ✅ | ALB, Bastion, NAT GW |
| **Private** | Brak route do IGW | ❌ | EC2 app servers, ECS tasks |
| **Isolated** | Brak route do IGW i NAT | ❌ | RDS, ElastiCache (tylko wewnętrzna komunikacja) |

> ⚠️ **Ważne:** Subnet nie jest "publiczny" przez samą nazwę — jest publiczny gdy Route Table ma route `0.0.0.0/0 → IGW`. To Route Table decyduje!

**Auto-assign Public IP:**
- Ustawienie na poziomie subnetu
- Jeśli włączone → EC2 w tym subnecie dostaje publiczne IP automatycznie
- Public subnet = route do IGW + auto-assign public IP = typowa konfiguracja

**IPv6 w subnetach:**
- VPC może mieć `/56` IPv6 CIDR (z AWS-provided lub BYOIP)
- Subnet dostaje `/64` IPv6 CIDR
- IPv6 adresy są z natury publiczne (nie ma NAT dla IPv6)
- Subnet dual-stack = ma oba IPv4 i IPv6

---

### 3.3 Route Tables

**🟡 Co to jest?**
Route Table to **zbiór reguł routingu** (routes) określających dokąd idzie ruch z subnetu. Każdy subnet musi być powiązany z dokładnie jedną Route Table.

**Anatomia Route Table:**

| Destination | Target | Opis |
|-------------|--------|------|
| `10.0.0.0/16` | `local` | Ruch wewnątrz VPC — zawsze obecny, nieusuwalna |
| `0.0.0.0/0` | `igw-xxx` | Ruch internetowy → Internet Gateway |
| `0.0.0.0/0` | `nat-xxx` | Ruch internetowy → NAT Gateway |
| `10.1.0.0/16` | `vpcx-xxx` | Ruch do VPC Peering |
| `10.2.0.0/16` | `tgw-xxx` | Ruch do Transit Gateway |
| `192.168.0.0/16` | `vgw-xxx` | Ruch do VPN / Direct Connect |

**Priorytet routingu — Longest Prefix Match:**
AWS (jak każdy router) wybiera **najbardziej specyficzną trasę**:
```
Pakiet do 10.0.1.50:
- 10.0.0.0/16 → local    (/16 - mniej specyficzny)
- 10.0.1.0/24 → tgw-xxx  (/24 - bardziej specyficzny) ← WYGRYWA
```

**Main Route Table vs Custom:**

| Typ | Opis |
|-----|------|
| **Main Route Table** | Domyślna dla nowych subnetów bez explict association. Modyfikuj ostrożnie! |
| **Custom Route Table** | Tworzysz ręcznie i przypisujesz do wybranych subnetów. Best practice. |

**Typowe Route Tables (3-tier):**
```
Public Route Table:
  10.0.0.0/16  → local
  0.0.0.0/0    → igw-xxx
→ Przypisany do: Public Subnet AZ1, Public Subnet AZ2

Private Route Table AZ1:
  10.0.0.0/16  → local
  0.0.0.0/0    → nat-xxx-az1   ← NAT GW w AZ1
→ Przypisany do: Private Subnet AZ1

Private Route Table AZ2:
  10.0.0.0/16  → local
  0.0.0.0/0    → nat-xxx-az2   ← NAT GW w AZ2 (HA!)
→ Przypisany do: Private Subnet AZ2

Data Route Table:
  10.0.0.0/16  → local
  (brak 0.0.0.0/0)
→ Przypisany do: Data Subnet AZ1, Data Subnet AZ2
```

> ⚠️ **Egzamin:** Jeśli Private Subnet AZ1 i AZ2 mają tę **samą** Route Table z jednym NAT GW → Single Point of Failure + cross-AZ data transfer costs. Best practice: osobna Route Table per AZ, każda z własnym NAT GW.

---

### 3.4 Internet Gateway i NAT

**Internet Gateway (IGW):**
- Poziomowo skalowana, redundantna, HA brama do Internetu
- **1 IGW per VPC** (limit twardy)
- Obsługuje IPv4 (NAT 1:1 dla publicznych IP) i IPv6
- Bezstanowy dla celów routingu (sam nie filtruje)
- Nie ma przepustowości limitu — "unlimited"

**NAT Gateway:**
Daje prywatnym subnetom możliwość wychodzącego dostępu do Internetu (egress-only), blokując przychodzące połączenia z zewnątrz.

| Feature | NAT Gateway | NAT Instance (EC2) |
|---------|-------------|-------------------|
| Zarządzanie | AWS managed | Ty zarządzasz |
| Availability | Multi-AZ (jeśli 1 per AZ) | Single EC2 — SPOF |
| Przepustowość | 5 Gbps → 45 Gbps (auto-scale) | Zależy od typu EC2 |
| Koszt | ~$0.045/h + $0.045/GB | Koszt EC2 + transfer |
| Security Groups | ❌ Nie obsługuje | ✅ Obsługuje |
| Port forwarding | ❌ | ✅ |
| Bastion host | ❌ | ✅ (podwójna rola) |

**Typy NAT Gateway:**

| Typ | Opis | Kiedy użyć |
|-----|------|------------|
| **Public** | EIP, ruch wychodzący do Internetu | Prywatne subnety potrzebują Internetu |
| **Private** | Brak EIP, ruch między VPC przez TGW | Centralized NAT w hub-and-spoke |

**Egress-Only Internet Gateway:**
- Tylko dla **IPv6**
- Pozwala IPv6 na ruch wychodzący (egress) bez ruchu przychodzącego z Internetu
- Odpowiednik NAT GW ale dla IPv6 (bo IPv6 jest publiczne, nie ma NAT 1:many)

---

### 3.5 Security Groups

**🔴 Co to jest?**
Security Group (SG) to **stateful firewall na poziomie zasobu** (ENI — Elastic Network Interface). Działa jako wirtualna zapora przypisana bezpośrednio do EC2, RDS, Lambda, ALB itp.

**Kluczowe cechy:**

| Cecha | Wartość |
|-------|---------|
| **Stateful** | ✅ Return traffic automatycznie dozwolony |
| **Poziom** | ENI (Elastic Network Interface) — nie subnet |
| **Default behavior** | Deny all inbound, Allow all outbound |
| **Reguły** | Tylko Allow (nie ma Deny) |
| **Ocena** | Wszystkie reguły oceniane razem (union) |
| **Chainable** | ✅ SG jako source/destination innej SG |

**Anatomia reguły Security Group:**
```
Inbound Rule:
  Type:       Custom TCP
  Protocol:   TCP
  Port Range: 8080
  Source:     sg-0123456789abcdef  ← inna Security Group!
  Description: Allow from ALB

Outbound Rule:
  Type:       HTTPS
  Protocol:   TCP
  Port Range: 443
  Destination: 0.0.0.0/0
```

**Security Group Chaining (best practice):**
```mermaid
graph LR
    I((Internet)) -->|443| ALB_SG[ALB\nsg-alb]
    ALB_SG -->|8080\nSource: sg-alb| APP_SG[App Server\nsg-app]
    APP_SG -->|5432\nSource: sg-app| DB_SG[RDS PostgreSQL\nsg-db]
```
Reguła w `sg-app`: `Allow TCP 8080 from sg-alb` — tylko ruch z ALB dozwolony, nie z całego Internetu. Gdy dodajesz nowy ALB → automatycznie dostaje ten sam SG → dostęp bez zmiany reguł.

**Stateful — przykład:**
```
EC2 wysyła request do 8.8.8.8:53 (DNS):
  Outbound: Allow UDP 53 → 0.0.0.0/0 ✅
  Return traffic (8.8.8.8:53 → EC2): automatycznie dozwolone (stateful)
  Nawet jeśli brak reguły Inbound dla UDP ephemeral ports!
```

**Ważne defaults:**
- Nowy SG (custom): **Deny all inbound, Allow all outbound**
- Default SG (tworzony z VPC): Allow all inbound **from same SG**, Allow all outbound
- EC2 bez custom SG → dostaje Default SG

**Security Group References — kiedy używać IP vs SG:**

| Sytuacja | Użyj |
|----------|------|
| Dostęp z konkretnego IP (on-prem, biuro) | CIDR (np. `203.0.113.0/24`) |
| Dostęp z innego zasobu AWS w tym samym VPC | Security Group ID |
| Dostęp z innego VPC (Peering) | Security Group ID (działa cross-VPC przez Peering) |
| Dostęp z Internetu | `0.0.0.0/0` |

---

### 3.6 Network ACL (NACL)

**🟠 Co to jest?**
Network ACL to **stateless firewall na poziomie subnetu**. Kontroluje ruch wchodzący i wychodzący z całego subnetu.

**Kluczowe cechy:**

| Cecha | Wartość |
|-------|---------|
| **Stateless** | ❌ Return traffic NIE jest automatyczny — potrzeba obu reguł |
| **Poziom** | Subnet |
| **Default behavior** | Allow all (default NACL), Deny all (nowy custom NACL) |
| **Reguły** | Allow i Deny |
| **Ocena** | Numerycznie po kolei (1, 100, 200...) — FIRST MATCH wins |
| **Zakres** | 1 NACL → wiele subnetów, 1 subnet → 1 NACL |

**Anatomia reguły NACL:**
```
Rule #100 — Inbound:
  Type:     HTTPS
  Protocol: TCP (6)
  Port:     443
  Source:   0.0.0.0/0
  Action:   ALLOW

Rule #200 — Inbound:
  Type:     Custom TCP
  Protocol: TCP (6)
  Port:     1024-65535  ← ephemeral ports (return traffic!)
  Source:   0.0.0.0/0
  Action:   ALLOW

Rule #*   — Inbound:
  (Default deny — wszystko inne)
  Action:   DENY
```

**Ephemeral Ports — kluczowe dla NACL:**
```
Klient (np. przeglądarka) otwiera połączenie TCP:
  - Docelowy port: 443 (HTTPS)
  - Źródłowy (ephemeral) port: np. 49152-65535 (losowy)

W NACL musi być reguła:
  Inbound:  Allow TCP 443 from 0.0.0.0/0      (żądanie klienta)
  Outbound: Allow TCP 1024-65535 to 0.0.0.0/0 (odpowiedź — ephemeral port klienta)
```

---

### 3.7 Security Group vs NACL — porównanie

```mermaid
graph TB
    INET((Internet)) -->|ruch| NACL1[NACL Inbound\n— subnet border]
    NACL1 --> SG1[Security Group Inbound\n— ENI border]
    SG1 --> EC2[EC2 Instance]
    EC2 --> SG2[Security Group Outbound\n— ENI border]
    SG2 --> NACL2[NACL Outbound\n— subnet border]
    NACL2 --> INET
```

**Tabela porównawcza:**

| Cecha | Security Group | Network ACL |
|-------|---------------|-------------|
| **Poziom** | ENI / zasób | Subnet |
| **Stateful** | ✅ Tak | ❌ Nie |
| **Reguły Allow** | ✅ | ✅ |
| **Reguły Deny** | ❌ | ✅ |
| **Kolejność reguł** | Wszystkie razem | Numeryczna (first match) |
| **Default nowy** | Deny all in, Allow all out | Deny all (in + out) |
| **Default VPC** | Allow same-SG, Allow all out | Allow all (in + out) |
| **Stosowany do** | EC2, RDS, ALB, Lambda... | Wszystko w subnecie |
| **Ephemeral ports** | Automatyczne (stateful) | Musisz dodać ręcznie |
| **Analogia Linux** | `iptables` stateful (conntrack) | `iptables` stateless (bez conntrack) |
| **Analogia Azure** | NSG na NIC | NSG na subnet |

**Kiedy używać NACL:**
- Dodatkowa warstwa obrony (defense in depth)
- **Blokowanie konkretnych IP** (Security Group nie ma Deny!) — np. blokowanie złośliwego IP
- Zgodność z regulacjami wymagającymi stateless firewall
- Szybkie blokowanie subsieci podczas incydentu

> ⚠️ **Egzamin:** Chcesz zablokować konkretny IP atakujący EC2? → **NACL** (ma Deny). Security Group nie ma Deny — możesz tylko nie dodawać Allow.

---

### 3.8 VPC Flow Logs

**Co to jest?**
Przechwytuje metadane ruchu sieciowego w VPC (nie payload — tylko nagłówki). Używane do audytu, troubleshootingu, threat detection.

**Poziomy zbierania:**
- VPC level (wszystkie ENI w VPC)
- Subnet level (wszystkie ENI w subnecie)
- ENI level (konkretny interfejs)

**Format rekordu Flow Log:**
```
version account-id interface-id srcaddr dstaddr srcport dstport protocol packets bytes windowstart windowend action flow-direction log-status
2 123456789012 eni-abc123 10.0.1.5 10.0.2.10 12345 443 6 10 4500 1609459200 1609459260 ACCEPT INGRESS OK
```

**Destynacje:**
- CloudWatch Logs (query z Logs Insights)
- S3 (Athena queries, długoterminowe przechowywanie)
- Kinesis Data Firehose (real-time processing)

**Pola custom (V5+):**
`vpc-id`, `subnet-id`, `instance-id`, `tcp-flags`, `pkt-srcaddr`, `pkt-dstaddr`, `region`, `az-id`, `sublocation-type`

> ⚠️ **Egzamin:** Flow Logs **nie przechwytują**: DNS queries (port 53 do Route 53 Resolver, 169.254.169.253), DHCP traffic, metadata traffic (169.254.169.254), Windows license activation traffic.

---

### 3.9 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** 50 kont AWS, każde ma własne VPC. Centralne usługi bezpieczeństwa (firewall, DNS, logging) muszą być dostępne dla wszystkich.

**Rozwiązanie — Hub-and-Spoke z Transit Gateway:**
```mermaid
graph TB
    HUB["Hub VPC\n(Security/Shared Services)\n10.0.0.0/16"] --> TGW[Transit Gateway]
    TGW --> S1["Spoke VPC 1\nDev - 10.1.0.0/16"]
    TGW --> S2["Spoke VPC 2\nProd - 10.2.0.0/16"]
    TGW --> S3["Spoke VPC 3\nTest - 10.3.0.0/16"]
    HUB --> FW[AWS Network Firewall\nCentralized Egress]
    HUB --> DNS[Route 53 Resolver\nCentralized DNS]
```
Kluczowe: CIDR każdego VPC musi być **unikalny** — brak nakładania się!

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Aplikacja 3-tier (ALB → ECS → RDS). Projektowanie VPC od zera.

**Best practice:**
```
VPC: 10.0.0.0/16

Public Subnets  (ALB):  10.0.1.0/24, 10.0.2.0/24   (po 1 per AZ)
Private Subnets (ECS):  10.0.11.0/24, 10.0.12.0/24
Data Subnets    (RDS):  10.0.21.0/24, 10.0.22.0/24

Security Groups:
  sg-alb  → Allow 443 from 0.0.0.0/0
  sg-ecs  → Allow 8080 from sg-alb
  sg-rds  → Allow 5432 from sg-ecs

NACL Data Subnet: Deny all inbound except from Private Subnet CIDRs
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** EC2 w private subnet nie może połączyć się z S3. Troubleshooting.

**Diagnostyka:**
```bash
# Sprawdź Flow Logs — szukaj REJECT dla ruchu do S3
# Sprawdź Route Table — czy jest trasa 0.0.0.0/0 → NAT GW (lub VPC Endpoint!)
# Sprawdź Security Group — czy outbound 443 dozwolony
# Sprawdź NACL — czy outbound 443 i inbound ephemeral ports dozwolone

# Rozwiązanie preferowane: VPC Gateway Endpoint dla S3 (bez NAT, darmowy!)
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-xxx \
  --service-name com.amazonaws.eu-west-1.s3 \
  --route-table-ids rtb-xxx
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Migracja 200 serwerów on-prem. Sieć on-prem: `192.168.0.0/16`. Projektuj VPC.

**Kluczowe decyzje:**
- VPC CIDR nie może być `192.168.0.0/16` — koliduje z on-prem
- Użyj `10.0.0.0/16` lub `172.16.0.0/16`
- VPN/Direct Connect → VGW (Virtual Private Gateway) lub TGW
- Route Tables: dodaj route `192.168.0.0/16 → vgw-xxx`
- Security Groups: rozszerz reguły o CIDR on-prem dla migrowanych usług

---

## 4. Public Subnet, IGW, NAT i alternatywne metody dostępu do Internetu

### 4.1 Public Subnet — czy każda maszyna musi mieć publiczne IP?

**Krótka odpowiedź: NIE.**

Public subnet definiuje Route Table (route do IGW), nie IP maszyny. Te dwie rzeczy są **niezależne**.

```
Public Subnet = Route Table z 0.0.0.0/0 → IGW
                                    ≠
Publiczne IP  = Elastic IP lub auto-assign Public IP na instancji
```

**Macierz możliwości:**

| Subnet | Publiczne IP | Rezultat | Typowy użytek |
|--------|-------------|----------|---------------|
| Public (route → IGW) | ✅ Tak | ✅ Dwukierunkowy dostęp Internet ↔ EC2 | Web serwer, Bastion, ALB |
| Public (route → IGW) | ❌ Nie | ✅ Wychodzący do Internetu przez IGW, ❌ brak przychodzącego z Internetu | EC2 który inicjuje połączenia outbound, nie przyjmuje inbound |
| Private (brak route IGW) | ✅ Tak (EIP) | ❌ Bezużyteczne — nie ma trasy przez IGW, IP "wisi" | Błąd projektowy, nie rób tego |
| Private (brak route IGW) | ❌ Nie | ✅ Izolowana od Internetu | App servers, RDS, ElastiCache |

> ⚠️ **Egzamin:** EC2 w public subnet **bez publicznego IP** może inicjować połączenia outbound przez IGW (działa jak prywatna maszyna z dostępem do Internetu). Internet **nie może** nawiązać połączenia do niej (brak publicznego IP = brak celu).

**Auto-assign Public IP — gdzie się konfiguruje:**

Trzy poziomy, każdy może nadpisać poprzedni:

```
1. Subnet-level setting (domyślne dla nowych instancji w tym subnecie)
   aws ec2 modify-subnet-attribute --subnet-id subnet-xxx \
     --map-public-ip-on-launch

2. EC2 launch — można nadpisać ustawienie subnetu (enable/disable)

3. Elastic IP (EIP) — statyczny publiczny IP, przypisany ręcznie do ENI
```

**Elastic IP (EIP) vs Auto-assigned Public IP:**

| Cecha | Auto-assigned Public IP | Elastic IP (EIP) |
|-------|------------------------|-----------------|
| Trwałość | Zmienia się przy stop/start | ✅ Stały, przypisany do konta |
| Koszt | Darmowy (gdy przypisany) | Bezpłatny gdy **przypisany do running instancji**, $0.005/h gdy nieprzypisany |
| Limit | Nieograniczony | 5 per Region (soft limit) |
| Re-assign | ❌ Nie można przenieść | ✅ Można odpiąć i przypiąć do innej ENI |
| DNS | Dynamiczny | Stały (ptr record) |
| Failover | ❌ | ✅ Przenieś EIP na inną instancję |

**Kiedy NIE dawać publicznego IP (nawet w public subnet):**
- EC2 za ALB — ALB ma publiczne IP, EC2 nie potrzebuje
- EC2 w Auto Scaling Group za ALB — to samo
- Bastion Host tylko do zarządzania wewnętrznego
- Każda maszyna gdzie ruch wychodzi tylko przez NAT GW lub VPC Endpoint

---

### 4.2 Internet Gateway — szczegóły

**Co to dokładnie robi IGW?**

IGW pełni **dwie funkcje** jednocześnie:
1. **Router** — jest celem w Route Table (`0.0.0.0/0 → igw-xxx`)
2. **NAT 1:1** — tłumaczy prywatne IP instancji na jej publiczne IP (i odwrotnie)

**Jak działa NAT 1:1 w IGW:**
```
EC2: prywatne IP 10.0.1.50, publiczne IP 54.1.2.3

Pakiet wychodzący:
  EC2 wysyła:     src=10.0.1.50  dst=8.8.8.8
  IGW transluje:  src=54.1.2.3   dst=8.8.8.8   ← podmienia src na publiczne IP
  Pakiet w Internecie widoczny jako: 54.1.2.3 → 8.8.8.8

Pakiet przychodzący:
  Internet wysyła: src=8.8.8.8   dst=54.1.2.3
  IGW transluje:   src=8.8.8.8   dst=10.0.1.50 ← podmienia dst na prywatne IP
  EC2 widzi:       src=8.8.8.8   dst=10.0.1.50
```

> ⚠️ **Ważne:** EC2 **nie wie** o swoim publicznym IP — nie widzi go na interfejsie sieciowym (`ip addr show`). Widzi tylko prywatne IP. Translacja odbywa się w IGW, poza instancją.

**IGW — cechy techniczne:**

| Cecha | Wartość |
|-------|---------|
| Availability | Redundantny, HA z definicji (AWS managed) |
| Limit per VPC | **1** (twardy limit) |
| Przepustowość | Brak limitu — skaluje się automatycznie |
| Koszt | **Darmowy** (płacisz tylko za transfer danych) |
| IPv6 | ✅ Obsługuje (bez NAT — IPv6 jest end-to-end) |
| Odłączanie | Można odpiąć od VPC i przypiąć do innego |

**Warunki żeby IGW działał dla instancji:**
```
1. IGW podłączony do VPC
2. Route Table subnetu ma: 0.0.0.0/0 → igw-xxx
3. Instancja ma publiczne IP (EIP lub auto-assign)
4. Security Group pozwala na ruch
5. NACL pozwala na ruch
```
Brak **któregokolwiek** z powyższych → brak dostępu z/do Internetu.

---

### 4.3 NAT Gateway — szczegóły

**Cel:** Prywatne zasoby (EC2, Lambda, ECS) mogą **inicjować** połączenia do Internetu (download packages, call external APIs), ale Internet **nie może inicjować** połączeń do nich.

**Jak działa NAT Gateway (PAT — Port Address Translation):**
```
Prywatna EC2: 10.0.3.50

Pakiet wychodzący:
  EC2 wysyła:       src=10.0.3.50:52341  dst=8.8.8.8:443
  NAT GW transluje: src=52.18.100.1:50001  dst=8.8.8.8:443
  (NAT GW EIP: 52.18.100.1, nowy port: 50001)

Pakiet powrotny:
  Internet wysyła:    src=8.8.8.8:443    dst=52.18.100.1:50001
  NAT GW transluje:   src=8.8.8.8:443    dst=10.0.3.50:52341
  EC2 dostaje odpowiedź

Wiele EC2 za jednym NAT GW → wiele prywatnych IP → 1 publiczny EIP
(różnicowane przez port — PAT/NAPT)
```

**NAT Gateway — przepływ w VPC:**
```mermaid
graph LR
    EC2["EC2\n10.0.3.50\nPrivate Subnet"] -->|"0.0.0.0/0\nRoute Table"| NATGW["NAT Gateway\nEIP: 52.18.100.1\nPublic Subnet"]
    NATGW -->|"0.0.0.0/0\nRoute Table"| IGW["Internet\nGateway"]
    IGW --> INET((Internet))
```

**Kluczowe:** NAT Gateway **sam musi być w public subnet** i mieć route do IGW. Instancje w private subnet mają route do NAT GW.

**NAT Gateway — parametry techniczne:**

| Parametr | Wartość |
|----------|---------|
| Przepustowość | 5 Gbps → auto-scale do 45 Gbps |
| Concurrent connections | 55 000 per destination IP:port |
| Porty źródłowe | 1024–65535 |
| Availability | Per-AZ (1 NAT GW = 1 AZ) |
| Elastic IP | Wymagany (1 per NAT GW) |
| Protokoły | TCP, UDP, ICMP |
| Security Groups | ❌ Nie można przypisać |
| Zarządzanie | AWS managed (patching, scaling) |
| Koszt | ~$0.045/h + $0.045/GB processed |

**Connection tracking w NAT Gateway:**
NAT GW śledzi 55 000 równoczesnych połączeń **per destination IP + port**. Jeśli przekroczysz → packet loss. Rozwiązanie: rozkładaj ruch na wiele NAT GW lub wiele destination IPs.

**NAT Gateway Public vs Private (ponownie, więcej detali):**

```mermaid
graph TB
    subgraph "Public NAT Gateway"
        EIP["Elastic IP"] --> NPUB["NAT GW\n(public subnet)"]
        NPUB --> IGW["IGW"] --> INET((Internet))
        PRIV["Private EC2"] --> NPUB
    end

    subgraph "Private NAT Gateway"
        NPRIV["NAT GW\n(private subnet)\nBrak EIP"]
        VPC1["VPC A\n10.1.0.0/16"] --> NPRIV
        NPRIV --> TGW["Transit Gateway"]
        TGW --> VPC2["VPC B\n10.1.0.0/16\nTen sam CIDR!"]
    end
```

**Private NAT GW** rozwiązuje problem komunikacji między VPC z **nakładającymi się CIDR** (overlapping) — jedyny natywny sposób bez PrivateLink.

---

### 4.4 NAT Instance (legacy)

EC2 z własnym procesem NAT. AWS nie tworzy już nowych — historyczne, ale może pojawić się na egzaminie.

**Jak uruchomić:**
```bash
# Wyłącz Source/Destination Check (OBOWIĄZKOWE!)
aws ec2 modify-instance-attribute \
  --instance-id i-xxx \
  --no-source-dest-check
# Bez tego Linux NAT nie działa — EC2 odrzuca pakiety nie adresowane do siebie
```

**NAT Instance vs NAT Gateway:**

| Feature | NAT Gateway | NAT Instance |
|---------|-------------|-------------|
| Managed | ✅ AWS | ❌ Ty |
| HA | ✅ (per-AZ) | ❌ Single EC2 — SPOF |
| Bandwidth | 5→45 Gbps auto | Zależy od EC2 type |
| Security Group | ❌ | ✅ |
| Port forwarding | ❌ | ✅ |
| Bastion host | ❌ | ✅ (może być jednocześnie) |
| Cost | Per hour + GB | EC2 cost + transfer |
| Source/dest check | N/A | Wyłączyć obowiązkowo |

> ⚠️ **Egzamin:** NAT Instance pojawia się w pytaniach jako "zła odpowiedź" lub pytanie "co trzeba zrobić żeby NAT Instance działał" → wyłączyć Source/Destination Check.

---

### 4.5 Inne rozwiązania dostępu do Internetu i usług AWS

#### AWS PrivateLink (VPC Interface Endpoint)

**Co to jest:** Prywatne połączenie do usług AWS (lub własnych usług) **bez przechodzenia przez Internet**. Ruch zostaje w sieci AWS.

```mermaid
graph LR
    EC2["EC2\nPrivate Subnet"] --> ENI["ENI z prywatnym IP\n(Interface Endpoint)"]
    ENI --> PL["AWS PrivateLink"]
    PL --> SVC["AWS Service\n(np. S3, SQS, SNS...)"]
    style PL fill:#f96,color:#000
```

Tworzy **ENI w Twoim subnecie** z prywatnym IP → DNS przekierowuje zapytania do usługi na ten ENI.

#### VPC Gateway Endpoint

**Tylko dla S3 i DynamoDB.** Darmowy. Nie tworzy ENI — dodaje wpis w Route Table.

```
Route Table po dodaniu Gateway Endpoint:
  10.0.0.0/16          → local
  0.0.0.0/0            → nat-xxx
  pl-xxxxx (S3 prefix) → vpce-xxx  ← nowy wpis
```

Ruch do S3 → omija NAT GW → oszczędność kosztów.

#### AWS VPN (Site-to-Site)

Połączenie szyfrowane przez Internet między on-prem a VPC. Używa IPsec.

```
On-prem Router ←—IPsec VPN—→ Virtual Private Gateway (VGW) ←→ VPC
```

- Przepustowość: do 1.25 Gbps per tunnel (2 tunele per connection dla HA)
- Koszt: ~$0.05/h per connection
- Szybkość wdrożenia: minuty (vs Direct Connect = tygodnie/miesiące)

#### AWS Direct Connect

Dedykowane fizyczne łącze między on-prem a AWS. Nie przez Internet.

```
On-prem ←—dedykowane łącze—→ AWS Direct Connect Location ←→ VPC
```

- Przepustowość: 1 Gbps, 10 Gbps, 100 Gbps (hosted: 50 Mbps – 10 Gbps)
- Latency: przewidywalne, niskie
- Koszt: wyższy niż VPN, ale niższy data transfer
- Wdrożenie: tygodnie do miesięcy

#### AWS Transit Gateway (TGW)

Centralny hub routingu między wieloma VPC, VPN, Direct Connect. Hub-and-spoke.

```
VPC 1 ─┐
VPC 2 ─┤
VPC 3 ─┼─→ Transit Gateway ←─ On-prem (VPN/DX)
VPN   ─┤
DX    ─┘
```

- Zastępuje mesh peeringu VPC (N*(N-1)/2 peeringów → N połączeń do TGW)
- Routing między VPC przez TGW Route Tables
- Wspiera multicast

#### AWS VPN CloudHub

Łączenie wielu on-prem lokalizacji przez **jeden VGW** (Virtual Private Gateway). Każda lokalizacja może komunikować się z innymi przez AWS.

```
Biuro Warsaw ─┐
Biuro Berlin ─┼─→ Virtual Private Gateway (AWS) → VPC
Biuro Paris  ─┘
(+ Warsaw ↔ Berlin przez AWS, nie bezpośrednio)
```

Tanie rozwiązanie HA dla połączeń multi-site (nie zastąpi MPLS ale tańszy backup).

#### AWS Client VPN

VPN dla **użytkowników** (nie site-to-site). OpenVPN-based.

```
Laptop pracownika ←—OpenVPN—→ Client VPN Endpoint (w VPC) ←→ zasoby w VPC
```

- Uwierzytelnianie: Cognito User Pool, Active Directory (RADIUS/AD), certyfikaty
- Split tunnel: tylko VPC traffic przez VPN, reszta bezpośrednio
- Koszt: per subnet association + per connection-hour

---

### 4.6 VPC Endpoints — Gateway i Interface

**Dwa typy endpoint, fundamentalnie różne:**

| Cecha | Gateway Endpoint | Interface Endpoint (PrivateLink) |
|-------|-----------------|----------------------------------|
| Usługi | **Tylko S3 i DynamoDB** | 100+ usług AWS + własne |
| Implementacja | Wpis w Route Table | ENI z prywatnym IP w subnecie |
| DNS | Bez zmian | Prywatny DNS (opcja) |
| Koszt | **Darmowy** | ~$0.01/h per AZ + $0.01/GB |
| Security Group | ❌ | ✅ (na ENI) |
| Dostęp cross-account | ❌ | ✅ |
| Dostęp cross-region | ❌ | ❌ (per-region) |
| On-prem dostęp (przez DX/VPN) | ❌ | ✅ |

**Kiedy Gateway Endpoint (S3):**
```bash
# EC2 w private subnet chce pisać do S3 — bez Gateway Endpoint:
#   EC2 → NAT GW ($0.045/GB) → IGW → S3

# Z Gateway Endpoint:
#   EC2 → Gateway Endpoint (DARMOWY) → S3
# Oszczędność: 100% kosztów NAT dla ruchu S3

aws ec2 create-vpc-endpoint \
  --vpc-id vpc-xxx \
  --service-name com.amazonaws.eu-west-1.s3 \
  --route-table-ids rtb-private-az1 rtb-private-az2
```

**Endpoint Policy (kontrola dostępu):**
```json
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-company-bucket/*"
  }]
}
```
Możesz ograniczyć przez endpoint policy: tylko konkretny bucket, tylko konkretne akcje.

**Interface Endpoint + Private DNS:**
```
Bez Private DNS:
  EC2 rozwiązuje sqs.eu-west-1.amazonaws.com → publiczny IP AWS
  Ruch przez Internet (lub przez NAT GW)

Z Private DNS (enabled):
  EC2 rozwiązuje sqs.eu-west-1.amazonaws.com → prywatny IP ENI w Twoim VPC
  Ruch przez PrivateLink, nie przez Internet
  Zero zmian w kodzie aplikacji!
```

**PrivateLink dla własnych usług:**
```mermaid
graph LR
    subgraph "Consumer VPC"
        C_EC2["EC2"] --> C_ENI["Interface Endpoint\nENI: 10.0.1.100"]
    end
    subgraph "Provider VPC"
        NLB["Network Load Balancer"] --> P_EC2["Your Service\n(EC2/ECS/Lambda)"]
    end
    C_ENI -->|AWS PrivateLink| NLB
```
Możesz udostępniać własne usługi innym kontom AWS przez PrivateLink (AWS Marketplace model).

---

### 4.7 Porównanie wszystkich rozwiązań

**Mapa decyzyjna:**
```mermaid
flowchart TD
    Q1{Kto potrzebuje\ndostępu do czego?}

    Q1 -->|EC2 → Internet\n(outbound only)| Q2
    Q1 -->|Internet → EC2\n(inbound)| IGW_PUB["IGW + Public IP\nna instancji"]
    Q1 -->|EC2 → usługa AWS\nS3 lub DynamoDB| GW_EP["VPC Gateway Endpoint\nDARMOWY"]
    Q1 -->|EC2 → inna usługa AWS\nnp. SQS, SSM, ECR| IF_EP["VPC Interface Endpoint\nPrivateLink"]
    Q1 -->|On-prem → VPC| Q3
    Q1 -->|Wiele VPC → central services| TGW["Transit Gateway"]
    Q1 -->|Użytkownicy → VPC| CVPN["Client VPN\nOpenVPN"]

    Q2 -->|Prosta, zarządzana,\nprodukcja| NATGW["NAT Gateway\n$0.045/h per AZ"]
    Q2 -->|Legacy / port-forwarding\n/ bastion| NATI["NAT Instance\n(nie polecane)"]

    Q3 -->|Szybko, przez Internet,\ntymczasowe| VPN["Site-to-Site VPN\n$0.05/h"]
    Q3 -->|Dedykowane łącze,\nniska latency, duży ruch| DX["Direct Connect\n1/10/100 Gbps"]
    Q3 -->|Wiele lokalizacji,\nzamiast pełnego MPLS| CLOUD_HUB["VPN CloudHub"]
```

**Tabela kosztów i cech (ruch wychodzący):**

| Rozwiązanie | Koszt godzinowy | Koszt danych | Internet traffic | Prywatność |
|-------------|----------------|-------------|-----------------|-----------|
| NAT Gateway | $0.045/h per AZ | $0.045/GB | ✅ | ❌ przez Internet |
| NAT Instance | EC2 cost | transfer cost | ✅ | ❌ przez Internet |
| Gateway Endpoint (S3/DDB) | **DARMOWY** | **DARMOWY** | ❌ AWS sieć | ✅ |
| Interface Endpoint | $0.01/h per AZ | $0.01/GB | ❌ AWS sieć | ✅ |
| Direct Connect | port fee | $0.02/GB (DX) | ❌ dedykowane | ✅✅ |
| Site-to-Site VPN | $0.05/h | standard | ✅ przez Internet | ✅ (szyfrowane) |

---

### 4.8 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** 200 kont AWS, każde ma zasoby w private subnet które muszą komunikować się z S3 i wywoływać zewnętrzne API (internet).

**Rozwiązanie:**
```
S3 / DynamoDB: Gateway Endpoint per VPC (darmowy, zero Internet)
Zewnętrzne API: Centralized NAT Gateway w Shared Services VPC przez Transit Gateway
  → oszczędność: 1 NAT GW w hub zamiast 200 NAT GW w spoke VPC
```

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Aplikacja na ECS Fargate (private subnet). Musi pobierać images z ECR, logi do CloudWatch, sekrety z Secrets Manager. Żadnego NAT GW.

**Rozwiązanie — VPC Interface Endpoints:**
```bash
# Wymagane endpoints dla Fargate bez NAT:
com.amazonaws.region.ecr.api        # ECR API
com.amazonaws.region.ecr.dkr        # ECR Docker registry
com.amazonaws.region.logs           # CloudWatch Logs
com.amazonaws.region.secretsmanager # Secrets Manager
com.amazonaws.region.s3             # S3 (Gateway Endpoint - darmowy)
com.amazonaws.region.sts            # STS (dla IAM roles)
```
Fargate bez NAT GW → Interface Endpoints → w pełni prywatne, tańsze przy dużym ruchu do AWS.

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma płaci $5000/miesiąc za NAT Gateway (głównie ruch do S3). Jak obniżyć koszty?

**Rozwiązanie:**
```
Analiza: VPC Flow Logs → CloudWatch Logs Insights
Sprawdź ile ruchu NAT GW idzie do S3:

fields @timestamp, srcAddr, dstAddr, bytes
| filter dstAddr like /s3/
| stats sum(bytes) by dstAddr

Rozwiązanie: Dodaj Gateway Endpoint dla S3
→ Ruch S3 omija NAT GW → oszczędność 100% dla tego ruchu
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Firma migruje z on-prem. Tymczasowo potrzebuje dostępu do zasobów AWS z on-prem. Docelowo Direct Connect (ale wymaga 3 miesięcy).

**Strategia dwuetapowa:**
```
Etap 1 (natychmiast): Site-to-Site VPN przez Internet
  On-prem router → VPN → VGW → VPC
  Koszt: $0.05/h, wdrożenie: godziny

Etap 2 (za 3 miesiące): Direct Connect
  On-prem → DX Location → AWS
  Opcja: VPN jako backup dla DX (Active/Passive)
  
Best practice: DX primary + VPN backup w tym samym VGW
```

---

## 5. VPC Router, DNS, Route Tables, Routing wewnątrz VPC i między VPC

### 5.1 VPC Router — czym jest i jak działa

**Co to jest?**
VPC Router to **wirtualny router zaimplementowany przez hypervisor AWS** — nie jest osobnym zasobem który widzisz w konsoli. Istnieje automatycznie w każdym VPC, jest niewidzialny, niezarządzalny, zawsze aktywny.

**Adres IP VPC Routera:**
Zawsze `VPC_CIDR_base + 1`. Przykłady:
```
VPC: 10.0.0.0/16   → Router IP: 10.0.0.1
VPC: 172.31.0.0/16 → Router IP: 172.31.0.1
VPC: 192.168.0.0/24 → Router IP: 192.168.0.1
```
Ten adres jest **zarezerwowany w każdym subnecie** jako `.1` — to adres routera dla instancji w tym subnecie (default gateway).

**Co robi VPC Router:**
1. **Routuje ruch między subnetami** w VPC (używając Local Route)
2. **Routuje ruch do zewnętrznych targetów** (IGW, NAT GW, TGW, VGW, Peering) według Route Table
3. **Odpowiada na ARP** — tłumaczy IP na MAC w obrębie VPC
4. **Wymusza reguły Route Table** — wybiera target według longest prefix match
5. **Przekazuje DNS queries** do VPC DNS (gdy `enableDnsSupport = true`)

**Jak EC2 "widzi" router:**
```bash
# Na EC2 Linux w VPC 10.0.0.0/16, subnet 10.0.1.0/24
ip route show
# default via 10.0.1.1 dev eth0    ← VPC Router = default gateway
# 10.0.1.0/24 dev eth0 proto kernel ← lokalny subnet

# Router odpowie na ping
ping 10.0.1.1   # ✅ zawsze działa
```

**VPC Router vs fizyczny router:**

| Cecha | VPC Router | Fizyczny Router (Cisco/Juniper) |
|-------|------------|--------------------------------|
| Konfiguracja | Przez Route Tables w AWS Console/API | CLI (IOS, JunOS) |
| Widoczność | Niewidzialny (brak zasobu) | Fizyczne urządzenie |
| HA | ✅ Zawsze (AWS managed) | Wymaga HSRP/VRRP |
| BGP | ❌ Bezpośrednio | ✅ |
| Firewall | ❌ (SG/NACL osobno) | Często wbudowany |
| Przepustowość | Brak limitu | Zależy od modelu |
| Koszt | **Darmowy** | Licencja + hardware |

**Odpowiedniki:**

| AWS VPC Router | Azure | GCP | On-prem |
|---------------|-------|-----|---------|
| VPC Router (implicit) | Implicit system route w VNet | Implicit VPC routes | Default Gateway / Core Switch L3 |

---

### 5.2 VPC DNS Server (Route 53 Resolver)

**Adres DNS:**
Zawsze `VPC_CIDR_base + 2`. Lub zawsze dostępny pod `169.254.169.253`.
```
VPC: 10.0.0.0/16   → DNS: 10.0.0.2
VPC: 172.31.0.0/16 → DNS: 172.31.0.2
Zawsze też:         → DNS: 169.254.169.253
```

**Dwa ustawienia VPC które to kontrolują:**

| Ustawienie VPC | Default | Opis |
|----------------|---------|------|
| `enableDnsSupport` | ✅ True | Włącza DNS resolver w VPC (10.x.x.2 / 169.254.169.253) |
| `enableDnsHostnames` | ❌ False (custom VPC) / ✅ True (Default VPC) | EC2 dostają publiczne DNS hostname (np. `ec2-1-2-3-4.compute-1.amazonaws.com`) |

> ⚠️ **Egzamin:** Żeby EC2 dostała publiczne DNS hostname → **oba** ustawienia muszą być `true`. Tylko `enableDnsHostnames=true` bez `enableDnsSupport=true` → nic nie działa.

**Co robi VPC DNS:**
- Rozwiązuje prywatne DNS hostnames instancji (`ip-10-0-1-50.eu-west-1.compute.internal`)
- Rozwiązuje publiczne DNS (`google.com`, `s3.amazonaws.com`)
- Obsługuje **Route 53 Private Hosted Zones** przypisanych do VPC
- Obsługuje **Interface Endpoint private DNS** (gdy włączone)
- Forward/Resolve przez **Route 53 Resolver** (inbound/outbound endpoints dla on-prem integracji)

**DNS dla instancji:**
```
Prywatny hostname:  ip-10-0-1-50.eu-west-1.compute.internal
  → Resolves to: 10.0.1.50 (prywatne IP)

Publiczny hostname: ec2-54-1-2-3.eu-west-1.compute.amazonaws.com
  → Resolves to: 54.1.2.3 (publiczne IP) z zewnątrz
  → Resolves to: 10.0.1.50 (prywatne IP!) z wewnątrz VPC  ← Split-horizon DNS
```

**Split-horizon DNS** — ta sama nazwa, różne IP zależnie od skąd pytasz:
```mermaid
graph LR
    EXT((Internet)) -->|"ec2-54-1-2-3.compute.amazonaws.com"| PUB_DNS["Publiczny DNS\n→ 54.1.2.3 (publiczny IP)"]
    INT["EC2 wewnątrz VPC"] -->|"ec2-54-1-2-3.compute.amazonaws.com"| VPC_DNS["VPC DNS\n→ 10.0.1.50 (prywatny IP!)"]
```
To kluczowe — komunikacja wewnątrz VPC przez prywatne IP nawet gdy używasz publicznej nazwy. Zero kosztów cross-region/Internet, niskie latency.

---

### 5.3 Route Tables — Main vs Custom, Association

**Main Route Table:**
- Tworzona automatycznie razem z VPC
- Każda nowa subnet **bez jawnego przypisania** dostaje Main Route Table
- Można ją modyfikować, ale **nie można usunąć**
- Domyślna zawartość: tylko `local` route

> ⚠️ **Best practice egzaminacyjny:** Zostaw Main Route Table z samym `local` (domyślna, bezpieczna). Twórz **Custom Route Tables** dla każdego tieru. Dlaczego? Nowa subnet bez przypisania dostaje Main — jeśli Main ma route do IGW → nowa subnet staje się przypadkowo publiczna.

**Custom Route Table:**
- Tworzysz ręcznie
- Przypisujesz explicitnie do subnetów
- Możesz usunąć (jeśli nie ma przypisanych subnetów)
- Best practice: osobna Route Table per tier per AZ

**Route Table Association:**

```mermaid
graph TD
    subgraph VPC
        MAIN["Main Route Table\n10.0.0.0/16 → local\n(tylko local)"]
        PUB_RT["Custom: Public RT\n10.0.0.0/16 → local\n0.0.0.0/0 → igw-xxx"]
        PRV_RT_AZ1["Custom: Private RT AZ1\n10.0.0.0/16 → local\n0.0.0.0/0 → nat-az1"]
        PRV_RT_AZ2["Custom: Private RT AZ2\n10.0.0.0/16 → local\n0.0.0.0/0 → nat-az2"]

        PUB1["Public Subnet AZ1\n10.0.1.0/24"] -->|"Associated with"| PUB_RT
        PUB2["Public Subnet AZ2\n10.0.2.0/24"] -->|"Associated with"| PUB_RT
        PRV1["Private Subnet AZ1\n10.0.11.0/24"] -->|"Associated with"| PRV_RT_AZ1
        PRV2["Private Subnet AZ2\n10.0.12.0/24"] -->|"Associated with"| PRV_RT_AZ2
        DATA1["Data Subnet AZ1\n10.0.21.0/24"] -->|"No explicit → uses"| MAIN
        DATA2["Data Subnet AZ2\n10.0.22.0/24"] -->|"No explicit → uses"| MAIN
    end
```

**Reguły asocjacji:**
- 1 subnet = dokładnie 1 Route Table w danym momencie
- 1 Route Table = wiele subnetów (many-to-one)
- Zmiana asocjacji działa natychmiast (nowe połączenia) — istniejące połączenia nie są zrywane

---

### 5.4 Local Route — fundament routingu VPC

**Local Route:**
```
Destination: 10.0.0.0/16   Target: local
```

- **Automatycznie tworzona** gdy tworzysz VPC
- **Nieusuwalna i nieedytowalna**
- Pokrywa **cały CIDR VPC** (primary i secondary bloki)
- Kieruje cały ruch wewnątrz VPC przez VPC Router

**Co oznacza target `local`:**
`local` = VPC Router. Ruch do dowolnego IP w CIDR VPC → VPC Router → dostarcza do odpowiedniej ENI.

**Local Route i secondary CIDRs:**
Gdy dodasz secondary CIDR do VPC, automatycznie pojawia się nowa Local Route:
```
Route Table po dodaniu secondary CIDR:
  10.0.0.0/16  → local   ← primary
  100.64.0.0/10 → local  ← secondary (np. dla Private NAT)
  0.0.0.0/0    → nat-xxx
```

**Czy można "obejść" Local Route?**
TAK — poprzez **longer prefix match**:
```
Route Table:
  10.0.0.0/16   → local        (/16)
  10.0.1.0/24   → tgw-xxx      (/24) ← bardziej specyficzny → wygrywa

Pakiet do 10.0.1.50 → idzie do TGW, nie przez local!
```
Użyteczne np. w centralized firewall architecture — wymuszasz ruch przez Inspection VPC.

---

### 5.5 Routing wewnątrz VPC krok po kroku

**Scenariusz:** EC2-A w `10.0.1.50` (Private Subnet AZ1) → EC2-B w `10.0.11.20` (Private Subnet AZ2). Ten sam VPC.

```mermaid
sequenceDiagram
    participant A as EC2-A\n10.0.1.50
    participant RT_A as Route Table\n(Private AZ1)
    participant ROUTER as VPC Router\n10.0.1.1 / 10.0.11.1
    participant RT_B as Route Table\n(Private AZ2)
    participant B as EC2-B\n10.0.11.20

    A->>RT_A: Pakiet do 10.0.11.20\nGdzie iść?
    RT_A-->>A: 10.0.0.0/16 → local\n(VPC Router)
    A->>ROUTER: Wyślij pakiet\nsrc=10.0.1.50 dst=10.0.11.20
    ROUTER->>RT_B: Sprawdź Route Table\ndla subnetu 10.0.11.0/24
    RT_B-->>ROUTER: 10.0.0.0/16 → local
    ROUTER->>B: Dostarcz pakiet\ndo ENI 10.0.11.20
```

**Kluczowy punkt:** Ruch między subnetami w tym samym VPC przechodzi przez **VPC Router**, **nie przez IGW**. IGW jest tylko dla Internetu.

**Routing — pełna ścieżka wychodzącego pakietu:**
```
EC2 (private subnet) chce połączyć się z api.external.com:443

1. EC2: DNS query → 169.254.169.253 (VPC DNS)
   VPC DNS → zwraca publiczny IP: 93.184.216.34

2. EC2: Pakiet src=10.0.3.50 dst=93.184.216.34
   Sprawdza swoją routing table (kernel):
   default via 10.0.3.1 (VPC Router) → wysyła do VPC Router

3. VPC Router: sprawdza Route Table subnetu:
   10.0.0.0/16 → local        (nie pasuje — dst poza VPC)
   0.0.0.0/0   → nat-xxx      (pasuje! → NAT Gateway)
   Forwwarduje do NAT GW

4. NAT Gateway: translacja src=10.0.3.50:52341 → EIP:50001
   Forwwarduje przez swoją Route Table:
   0.0.0.0/0 → igw-xxx

5. IGW: translacja src=EIP:50001 (NAT GW EIP, nie EC2 IP)
   Pakiet wychodzi do Internetu

6. Odpowiedź wraca tej samej ścieżki w odwrotnej kolejności
```

**Routing wewnątrz subnetu (ten sam subnet):**
```
EC2-A: 10.0.1.50 → EC2-B: 10.0.1.60 (ten sam subnet)

EC2-A sprawdza: czy 10.0.1.60 w tej samej sieci? (10.0.1.0/24)
→ TAK → ARP broadcast (kto ma 10.0.1.60?)
→ VPC Router odpowiada z MAC EC2-B (VPC Router obsługuje ARP proxy)
→ Pakiet wysyłany bezpośrednio do MAC EC2-B przez hypervisor
→ Nie przechodzi przez Route Table dla rutingu!
```

> ⚠️ **Egzamin:** Ruch w tym samym subnecie nie używa Route Table (używa ARP/L2). Route Table używana tylko dla ruchu między subnetami i na zewnątrz.

---

### 5.6 VPC Peering — routing między VPC

**Co to jest VPC Peering:**
Prywatne połączenie między dwoma VPC — ruch przechodzi przez **wewnętrzną sieć AWS**, nie przez Internet. Działa jak bridge — oba VPC "widzą się" jak w jednej sieci z perspektywy routingu.

**Wymagania:**
- CIDR obu VPC **nie mogą się nakładać** (brak overlapping)
- **Nie jest transitive** — A↔B i B↔C NIE oznacza A↔C (kluczowe!)
- Działa cross-account i cross-region (Inter-Region VPC Peering)
- Max: 125 aktywnych peeringów per VPC (soft limit)

**Konfiguracja routingu dla Peering:**

Po stworzeniu Peering Connection — **samo nie działa!** Musisz dodać routes w **obu VPC ręcznie**:

```
VPC A (10.0.0.0/16) — Route Table:
  10.0.0.0/16  → local
  10.1.0.0/16  → pcx-xxx   ← route do VPC B przez Peering Connection
  0.0.0.0/0    → igw-xxx

VPC B (10.1.0.0/16) — Route Table:
  10.1.0.0/16  → local
  10.0.0.0/16  → pcx-xxx   ← route do VPC A przez Peering Connection
  0.0.0.0/0    → igw-xxx
```

**Dlaczego Peering nie jest transitive:**
```mermaid
graph LR
    A["VPC A\n10.0.0.0/16"] <-->|"pcx-AB"| B["VPC B\n10.1.0.0/16"]
    B <-->|"pcx-BC"| C["VPC C\n10.2.0.0/16"]
    A -.->|"❌ NIE DZIAŁA\nbez pcx-AC"| C

    style A fill:#4472C4,color:#fff
    style B fill:#70AD47,color:#fff
    style C fill:#ED7D31,color:#fff
```

Pakiet z VPC A do VPC C: Route Table A nie ma route do `10.2.0.0/16` → pakiet odrzucony. Nawet jeśli VPC B ma peering do obu — VPC B **nie może routować tranzytowo** (AWS blokuje).

**Rozwiązanie dla transitive routing: Transit Gateway**
```
VPC A ─┐
VPC B ─┼─→ TGW → routuje między wszystkimi
VPC C ─┘
```

**Inter-Region VPC Peering:**
- Działa jak normalny Peering ale między Regionami
- Ruch przez **AWS global backbone** (nie Internet)
- Szyfrowany AES-256 automatycznie
- Wyższy koszt transferu danych (cross-region rate)
- Nie obsługuje IPv6 (tylko IPv4)
- Nie obsługuje Jumbo Frames

**Security Group Reference przez Peering:**
W tym samym regionie możesz użyć SG ID z peer VPC jako source/destination w Security Group rules:
```json
{
  "IpPermissions": [{
    "FromPort": 5432,
    "ToPort": 5432,
    "IpProtocol": "tcp",
    "UserIdGroupPairs": [{
      "GroupId": "sg-xxxxx",
      "UserId": "111122223333",    ← account ID właściciela peer VPC
      "VpcPeeringConnectionId": "pcx-xxxxx"
    }]
  }]
}
```

---

### 5.7 Cross-AZ i Cross-Region transfer — koszty

**To jeden z najważniejszych aspektów kosztowych AWS!**

**Koszty transferu danych:**

| Typ transferu | Koszt (us-east-1 przykład) |
|---------------|---------------------------|
| W tym samym AZ (private IP) | **$0.00** |
| Między AZ (private IP) | **$0.01/GB** (każdy kierunek) |
| Między AZ (public/EIP) | **$0.01/GB** (każdy kierunek) |
| VPC Peering — ten sam Region | **$0.01/GB** (każdy kierunek) |
| VPC Peering — cross-Region | Od $0.01 do $0.08/GB (zależy od regionów) |
| Internet egress (do Internetu) | $0.09/GB (pierwsze 10 TB) |
| Internet ingress (z Internetu) | **$0.00** |
| NAT Gateway processing | $0.045/GB (+ cross-AZ jeśli dotyczy) |
| Transfer do S3 (przez Gateway EP) | **$0.00** |
| Transfer między S3 a EC2 (ten sam Region) | **$0.00** |

> ⚠️ **Egzamin i real life:** Cross-AZ transfer kumuluje się szybko! Load Balancer → EC2 w innym AZ = podwójny cross-AZ koszt (LB → EC2 i EC2 → LB). Dlatego ALB używa sticky sessions lub zone-aware routing.

**Przykład kumulacji kosztów:**
```
Aplikacja: ALB (eu-west-1a) → EC2 (eu-west-1b) → RDS (eu-west-1a)

Request: 1 KB in, 100 KB response
  ALB → EC2: cross-AZ $0.01/GB
  EC2 → RDS: cross-AZ $0.01/GB
  RDS → EC2: cross-AZ $0.01/GB  
  EC2 → ALB: cross-AZ $0.01/GB

Całkowity cross-AZ: 4 x $0.01/GB = $0.04/GB dodatkowych kosztów
Przy 1 TB miesięcznie: $40 tylko za cross-AZ (+ normalny koszt transferu)
```

**Jak uniknąć cross-AZ kosztów:**
```
1. Deploy symetrycznie — EC2 i RDS w tej samej AZ gdzie ALB
2. ALB cross-zone load balancing — rozważ wyłączenie (domyślnie ON)
   Wyłączenie = każdy ALB node rozsyła tylko do EC2 w swojej AZ
3. NAT GW per AZ (zamiast jednego) — EC2 w AZ1 używa NAT GW w AZ1
4. ElastiCache w tej samej AZ co EC2 które go używają
5. VPC Peering — używaj private IP (nie public) → tańszy transfer
```

**AWS Free Tier — transfer danych:**
- EC2 → Internet: 100 GB/miesiąc za darmo (pierwsze 12 miesięcy)
- EC2 ↔ S3/Glacier ten sam region: **zawsze darmowy**
- CloudFront → Internet: 1 TB/miesiąc za darmo

---

### 5.8 VPC Router vs Internet Gateway — porównanie

Fundamentalne pytanie: **czym się różnią?**

```mermaid
graph LR
    subgraph VPC ["VPC 10.0.0.0/16"]
        EC2_PUB["EC2 Public\n10.0.1.50\nEIP: 54.1.2.3"]
        EC2_PRV["EC2 Private\n10.0.11.20"]
        ROUTER["VPC Router\n(implicit)\nTarget: local"]
        EC2_PUB <-->|"Internal routing\nL3 inside VPC"| ROUTER
        EC2_PRV <-->|"Internal routing"| ROUTER
    end
    IGW["Internet Gateway\nNAT 1:1\nTarget: igw-xxx"]
    INET((Internet\n8.8.8.8))
    ROUTER -->|"Route: 0.0.0.0/0 → igw-xxx"| IGW
    IGW <-->|"54.1.2.3 ↔ 8.8.8.8"| INET
```

**Tabela porównawcza:**

| Cecha | VPC Router | Internet Gateway |
|-------|------------|-----------------|
| **Rola** | Routuje ruch **wewnątrz VPC** i do zewnętrznych targetów | Brama **między VPC a Internetem** |
| **Widoczność** | Niewidzialny (implicit) | Widoczny zasób w konsoli |
| **Adres IP** | `VPC_CIDR+1` (każdy subnet) | Brak — nie ma IP w VPC |
| **Zarządzanie** | Automatyczne przez AWS | Tworzysz i przypisujesz do VPC |
| **Route Table** | Jest **targetem** wpisu `local` | Jest **targetem** wpisu `0.0.0.0/0` |
| **Limit** | 1 per VPC (implicit) | 1 per VPC (twardy) |
| **NAT** | ❌ Nie robi NAT | ✅ NAT 1:1 (prywatne ↔ publiczne IP) |
| **Przekraczanie granic VPC** | ❌ Tylko wewnątrz VPC | ✅ Do Internetu |
| **Koszt** | **Darmowy** | **Darmowy** (płacisz transfer) |
| **HA** | ✅ Zawsze | ✅ Zawsze |
| **Protokoły** | IPv4, IPv6, routing layer 3 | IPv4 (NAT), IPv6 (bez NAT) |
| **Analogia** | Core Switch L3 w DC | Border Router / Edge Firewall |

**Kluczowa różnica w przepływie pakietu:**
```
Pakiet EC2-A → EC2-B (różne subnety, ten sam VPC):
  EC2-A → [VPC Router] → EC2-B
  IGW: nie uczestniczy w ogóle

Pakiet EC2-A → google.com:
  EC2-A → [VPC Router] → [IGW] → Internet
  VPC Router: przekazuje do IGW (bo Route Table mówi 0.0.0.0/0 → IGW)
  IGW: robi NAT i wysyła dalej
```

**IGW nie jest routerem** — nie routuje między subnetami. Jest wyłącznie bramą wejścia/wyjścia do Internetu z translacją adresów.

---

### 5.9 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** 3 VPC (Dev, Staging, Prod) muszą komunikować się wzajemnie. Wszystkie w tym samym regionie, różne CIDRy.

**VPC Peering — problemy z mesh:**
```
3 VPC → 3 peering connections (A-B, B-C, A-C) — OK
10 VPC → 45 connections — trudne do zarządzania
50 VPC → 1225 connections — niemożliwe

Rozwiązanie powyżej ~5 VPC: Transit Gateway
```

**VPC Peering design:**
- Dev: `10.1.0.0/16` ↔ Staging: `10.2.0.0/16` ↔ Prod: `10.3.0.0/16`
- Każdy VPC ma explicit route do każdego peer VPC
- SG rules referują SG ID z peer VPC (same region)
- CloudWatch VPC Flow Logs do audytu cross-VPC ruchu

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Mikroserwisy na ECS w 3 AZ. Minimalizuj koszty cross-AZ.

**Strategia:**
```
1. Service Mesh (App Mesh / Istio) z zone-aware routing
   → Preferuj komunikację serwis→serwis w tej samej AZ

2. ALB: Cross-zone LB wyłączony → ruch trafia tylko do EC2 w tej samej AZ co ALB node

3. ElastiCache: odczyt z replica w tej samej AZ
   aws elasticache create-replication-group --preferred-cache-cluster-azs eu-west-1a eu-west-1b eu-west-1c

4. RDS: read replicas per AZ dla read-heavy workloads
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Aplikacja ma wysokie koszty transferu danych. VPC Peering między 2 VPC, ruch 10 TB/miesiąc.

**Diagnoza Route Table:**
```bash
# Sprawdź czy używasz prywatnych IP (0.01/GB) vs publicznych (0.09/GB)
# przez Peering musi być prywatny IP → trasa przez pcx-xxx

# VPC A route table:
aws ec2 describe-route-tables --filters Name=vpc-id,Values=vpc-A

# Powinno być:
# 10.1.0.0/16 → pcx-xxx  (prywatne IP, $0.01/GB)
# NIE powinno być ruchu przez IGW → internet → do VPC B ($0.09/GB)
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Migracja do AWS. On-prem używa DNS dla service discovery. Chcesz rozwiązywania on-prem nazw z VPC i vice versa.

**Rozwiązanie: Route 53 Resolver (Inbound + Outbound Endpoints):**
```mermaid
graph LR
    ONPREM["On-prem DNS\n192.168.1.10"] -->|"Conditional forwarder\ndla *.aws.company.com"| IN_EP["Route 53 Resolver\nInbound Endpoint\n(ENI w VPC)"]
    IN_EP --> R53["Route 53\nPrivate Hosted Zone\naws.company.com"]

    VPC_EC2["EC2 w VPC"] -->|"Conditional forward\ndla *.onprem.company.com"| OUT_EP["Route 53 Resolver\nOutbound Endpoint\n(ENI w VPC)"]
    OUT_EP -->|"Forward rules"| ONPREM
```

---

## 6. Zaawansowane pytania o VPC

### 6.1 Ile Route Tables ma subnet?

**Dokładnie jedna** — zawsze, w każdej chwili.

```
Subnet → Route Table: N:1 (wiele subnetów → jedna RT)
Route Table → Subnet: 1:N (jedna RT → wiele subnetów)
```

Jeśli nie przypisujesz explicitnie → subnet automatycznie używa **Main Route Table** VPC. Zmiana asocjacji podmienia Route Table natychmiast dla nowych połączeń.

```mermaid
graph LR
    S1["Subnet A\n10.0.1.0/24"] --> RT1["Custom RT: Public\n0.0.0.0/0 → igw"]
    S2["Subnet B\n10.0.2.0/24"] --> RT1
    S3["Subnet C\n10.0.11.0/24"] --> RT2["Custom RT: Private-AZ1\n0.0.0.0/0 → nat-az1"]
    S4["Subnet D\n10.0.21.0/24"] --> MAIN["Main RT\n(tylko local)"]
    S5["Subnet E\n10.0.22.0/24"] --> MAIN
```

---

### 6.2 Czy każda VM w public subnet musi mieć publiczny IP?

**NIE.** Szczegółowe wyjaśnienie i wszystkie kombinacje:

**Definicja "public subnet":** subnet którego Route Table ma `0.0.0.0/0 → IGW`. To jedyna definicja.

**4 kombinacje EC2 w public subnet:**

| Konfiguracja | Inicjuje outbound | Przyjmuje inbound | Typowy użytek |
|-------------|-------------------|-------------------|---------------|
| Public subnet + Publiczne IP (EIP/auto) | ✅ przez IGW | ✅ z Internetu | Web serwer, Bastion |
| Public subnet + **brak** publicznego IP | ❌ brak src IP | ❌ | ❌ Bezużyteczne — EC2 nie ma jak wyjść, Internet nie zna jej adresu |
| Private subnet + Publiczne IP | ❌ brak route IGW | ❌ | ❌ Błąd projektowy — EIP "wisi" |
| Private subnet + NAT GW | ✅ przez NAT GW | ❌ | App servers, workers |

> ⚠️ **Doprecyzowanie z poprzedniego rozdziału:** EC2 w public subnet BEZ publicznego IP nie może komunikować się z Internetem w żadnym kierunku. Wcześniej napisałem "może inicjować outbound" — to było nieprecyzyjne. IGW robi NAT 1:1 na podstawie publicznego IP instancji. Bez publicznego IP → IGW nie ma na co podmienić source → pakiet odrzucony.

**Kiedy EC2 w public subnet NIE potrzebuje publicznego IP:**
```
ALB (public IP) → target group → EC2 (prywatne IP, public subnet)
                                       ↑
                            ALB komunikuje się przez prywatne IP
                            EC2 nie musi być widoczna z Internetu
                            EC2 nie musi mieć publicznego IP
```
To najpowszechniejszy wzorzec — EC2 za ALB, w public subnet, bez publicznych IP.

**Auto-assign Public IP — hierarchia nadpisania:**
```
1. Subnet default (modify-subnet-attribute --map-public-ip-on-launch)
2. EC2 launch wizard — nadpisuje ustawienie subnetu
3. EIP — niezależne, przypisywane ręcznie do ENI
```

---

### 6.3 Source/Destination Check w NAT Instance

**Co to jest Source/Destination Check:**
EC2 domyślnie sprawdza każdy pakiet: "czy jestem źródłem (src) lub celem (dst) tego pakietu?". Jeśli nie → **odrzuca pakiet**.

To normalne zachowanie hosta sieciowego — chroni przed przypadkowym routowaniem obcego ruchu.

```
Normalna EC2 (Source/Destination Check ON):
  Pakiet: src=10.0.3.50, dst=8.8.8.8
  EC2 sprawdza: czy mój IP to 10.0.3.50? TAK → przyjmuje
  
  Pakiet: src=10.0.2.30, dst=8.8.8.8  (obcy pakiet!)
  EC2 sprawdza: czy mój IP to 10.0.2.30? NIE. Czy mój IP to 8.8.8.8? NIE.
  EC2 odrzuca → DROP
```

**Problem z NAT Instance:**
NAT Instance jest **pośrednikiem** — przyjmuje pakiety od innych EC2 i je forwarduje. Z definicji jest ani źródłem ani celem tych pakietów.

```
EC2-private (10.0.3.50) → NAT Instance (10.0.1.10) → Internet (8.8.8.8)

Pakiet od EC2-private do NAT Instance:
  src=10.0.3.50, dst=8.8.8.8
  
  NAT Instance widzi: src=10.0.3.50 (nie mój IP), dst=8.8.8.8 (nie mój IP)
  Source/Destination Check ON → DROP! ← NIE ZADZIAŁA

  Source/Destination Check OFF → akceptuje i forwarduje ← DZIAŁA
```

**Jak wyłączyć:**
```bash
# AWS CLI
aws ec2 modify-instance-attribute \
  --instance-id i-0123456789abcdef \
  --no-source-dest-check

# Lub w konsoli: EC2 → Actions → Networking → Change Source/Destination Check
```

**Inne przypadki gdzie wyłączasz Source/Destination Check:**
- NAT Instance (klasyczny przypadek)
- Własny software router/firewall na EC2 (np. pfSense, VyOS)
- Network appliance (IDS/IPS) w inline mode
- GRE tunnel endpoint
- Każda EC2 która forwarduje ruch nie swój

**Azure equivalent:** W Azure NIE ma tego problemu — IP Forwarding na NIC to osobne ustawienie (Enable IP forwarding on NIC), nie Source/Destination Check. AWS i Azure rozwiązują to samo inaczej.

---

### 6.4 VPN CloudHub

**Co to jest:**
Wzorzec architektoniczny (nie osobna usługa!) używający **jednego Virtual Private Gateway (VGW)** do łączenia wielu lokalizacji on-premises ze sobą i z VPC. Każda lokalizacja może komunikować się z innymi przez AWS sieć jako "hub".

```mermaid
graph TD
    VGW["Virtual Private Gateway\n(w VPC AWS)"]

    WA["Warszawa\nCustomer Gateway\n192.168.1.0/24"] -->|"IPsec VPN Tunnel"| VGW
    BE["Berlin\nCustomer Gateway\n192.168.2.0/24"] -->|"IPsec VPN Tunnel"| VGW
    PA["Paryż\nCustomer Gateway\n192.168.3.0/24"] -->|"IPsec VPN Tunnel"| VGW

    VGW -->|"Local route"| VPC["VPC\n10.0.0.0/16"]

    WA -.->|"Przez VGW (hub)"| BE
    WA -.->|"Przez VGW (hub)"| PA
    BE -.->|"Przez VGW (hub)"| PA
```

**Jak działa routing:**
VGW używa **BGP** — każdy Customer Gateway (biuro) ogłasza swój CIDR. VGW redistribuuje te trasy do wszystkich innych Customer Gateways. Biura "widzą się" nawzajem przez AWS jako hub.

**Wymagania:**
- Każda lokalizacja musi mieć unikalny ASN (BGP Autonomous System Number)
- Każda lokalizacja musi mieć unikalny CIDR (non-overlapping)
- Dynamiczny routing (BGP) — nie statyczny
- Każde biuro: 1 VPN connection (2 tunele) do tego samego VGW

**VPN CloudHub vs Transit Gateway:**

| Cecha | VPN CloudHub (VGW) | Transit Gateway |
|-------|-------------------|-----------------|
| VPC connections | 1 (VGW należy do 1 VPC) | Wiele VPC |
| Max VPN connections | 10 (per VGW) | 5000 |
| Transitive routing VPC→VPC | ❌ | ✅ |
| Bandwidth per VPN | 1.25 Gbps (2 tunele ECMP) | 1.25 Gbps per tunnel |
| Koszt | $0.05/h per VPN connection | $0.05/h per attachment + $0.02/GB |
| Złożoność | Prosta | Duża skala |

**Kiedy CloudHub zamiast TGW:**
- Mała firma z kilkoma biurami
- Potrzebujesz tylko site-to-site między biurami + VPC
- Nie masz wielu VPC do połączenia
- Oszczędność kosztu TGW

> ⚠️ **Egzamin:** AWS VPN CloudHub to **wzorzec architektury** (Hub-and-Spoke VPN), nie osobna usługa. Pytania mogą dotyczyć "jak połączyć wiele biur z VPC i między sobą tanio" → CloudHub przez VGW z BGP.

---

### 6.5 Private Subnets — jak działają szczegółowo

**Definicja techniczna:**
Subnet bez route `0.0.0.0/0 → IGW` w swojej Route Table. Zasoby nie są bezpośrednio dostępne z Internetu.

**Co private subnet MA:**
```
Route Table private subnet:
  10.0.0.0/16  → local              (obowiązkowe, auto)
  0.0.0.0/0    → nat-xxx            (opcjonalne — dla outbound Internet)
  10.1.0.0/16  → pcx-xxx            (opcjonalne — VPC Peering)
  10.2.0.0/16  → tgw-xxx            (opcjonalne — Transit Gateway)
  pl-xxxxxxxx  → vpce-xxx           (opcjonalne — Gateway Endpoint S3/DDB)
```

**Warstwy private subnets (3 kategorie):**

| Typ | Route Table | Dostęp outbound | Dostęp inbound | Przykład |
|-----|-------------|-----------------|----------------|---------|
| **Private + NAT** | `0.0.0.0/0 → nat` | ✅ Internet (NAT) | ❌ | App servers |
| **Private isolated** | tylko `local` | ❌ | ❌ | RDS, ElastiCache |
| **Private + VPC Endpoints** | `local` + endpoint routes | ✅ AWS services | ❌ | Lambda, ECS bez NAT |

**Jak zasób w private subnet komunikuje się z innymi AWS services:**
```
Opcja A — NAT Gateway:
  EC2 → Route: 0.0.0.0/0 → NAT GW → IGW → Internet → AWS API endpoint
  Koszt: $0.045/h + $0.045/GB processing

Opcja B — VPC Interface Endpoint (PrivateLink):
  EC2 → Route: local → ENI endpoint (prywatny IP) → AWS service backbone
  Koszt: $0.01/h + $0.01/GB
  Ruch nie wychodzi z sieci AWS

Opcja C — VPC Gateway Endpoint (S3/DynamoDB):
  EC2 → Route: pl-xxx → Gateway Endpoint → S3/DynamoDB
  Koszt: DARMOWY
  Ruch nie wychodzi z sieci AWS
```

**Private subnet a Instance Metadata:**
EC2 zawsze może dotrzeć do `169.254.169.254` (link-local) niezależnie od Route Table — to ruch L2 obsługiwany przez hypervisor, nie przechodzi przez VPC Router.

---

### 6.6 Dual Stack — IPv4 + IPv6 w VPC

**Co to jest Dual Stack:**
VPC i subnety mają **jednocześnie** adresy IPv4 i IPv6. Zasoby (EC2) mogą komunikować się przez oba protokoły.

**Konfiguracja Dual Stack krok po kroku:**

```
1. VPC: przypisz IPv6 CIDR
   - AWS-provided: /56 automatycznie (np. 2600:1f18:xxx::/56)
   - BYOIP: własny publiczny IPv6 prefix
   - Amazon-provided IPv6 CIDR: zawsze /56

2. Subnet: przypisz /64 z VPC /56
   VPC /56 = 256 subnetów /64
   np. 2600:1f18:xxx:0001::/64 dla subnet 1
       2600:1f18:xxx:0002::/64 dla subnet 2

3. IGW: automatycznie obsługuje IPv6 (bez NAT — IPv6 jest end-to-end)

4. Route Table: dodaj ::/0 → igw-xxx
   (analogicznie do 0.0.0.0/0 dla IPv4)

5. EC2: przypisz IPv6 address (auto lub ręcznie)
   Security Group: dodaj reguły dla IPv6 osobno!
```

**IPv4 vs IPv6 w AWS:**

| Cecha | IPv4 | IPv6 |
|-------|------|------|
| Adresy prywatne | ✅ RFC 1918 (10.x, 172.16.x, 192.168.x) | ❌ Wszystkie IPv6 są publicznie routowalne |
| NAT | ✅ NAT GW / IGW NAT 1:1 | ❌ Brak NAT (unnecesary — dość adresów) |
| Prywatne subnety outbound | NAT Gateway | **Egress-Only IGW** |
| Format | `10.0.1.50` | `2600:1f18:xxx:0001::50` |
| Security Groups | Osobne reguły IPv4 | Osobne reguły IPv6 |
| DNS | A record | AAAA record |

**Egress-Only Internet Gateway (EIGW):**
Odpowiednik NAT GW dla IPv6 — pozwala na wychodzący ruch IPv6 bez przychodzącego z Internetu:
```
Route Table private subnet (dual stack):
  10.0.0.0/16  → local         (IPv4 internal)
  ::/0          → eigw-xxx      (IPv6 outbound only — Egress-Only IGW)
  0.0.0.0/0    → nat-xxx       (IPv4 outbound przez NAT GW)
```

**Dual Stack — które usługi AWS wspierają:**

| Usługa | Dual Stack | Uwagi |
|--------|-----------|-------|
| EC2 | ✅ | Przypisz IPv6 address do ENI |
| ALB | ✅ | `dualstack` DNS name (`dualstack.alb-xxx.eu-west-1.elb.amazonaws.com`) |
| NLB | ✅ | IPv6 target groups |
| EKS | ✅ | IPv6 mode dla pods |
| Lambda | ✅ | W VPC z dual stack subnets |
| RDS | ✅ | Dual stack DB instance |
| ECS Fargate | ✅ | IPv6 w dual stack subnets |
| S3 | ✅ | `s3.dualstack.<region>.amazonaws.com` |
| CloudFront | ✅ | Automatycznie dual stack |
| API Gateway | ✅ | IPv6 support |
| NAT Gateway | ❌ | NAT GW jest tylko IPv4. IPv6 → Egress-Only IGW |
| VPC Peering (cross-region) | ❌ | Inter-region peering tylko IPv4 |
| Site-to-Site VPN | ❌ | Tylko IPv4 |

**Security Groups z Dual Stack — pułapka:**
```
Reguła SG dla IPv4:   Allow TCP 443 from 0.0.0.0/0
Reguła SG dla IPv6:   Allow TCP 443 from ::/0      ← osobna reguła!

Bez reguły IPv6 → IPv6 ruch blokowany mimo IPv4 reguły
```

**ALB Dual Stack przykład:**
```bash
# ALB z dual stack
aws elbv2 create-load-balancer \
  --name my-alb \
  --subnets subnet-xxx1 subnet-xxx2 \
  --ip-address-type dualstack   # lub "ipv4" (default)

# DNS name: dualstack.my-alb-xxx.eu-west-1.elb.amazonaws.com
# A record → IPv4
# AAAA record → IPv6
```

---

### 6.7 Delegated Subnets — odpowiednik Azure w AWS

**Azure Delegated Subnets — przypomnienie:**
W Azure możesz "delegować" subnet do konkretnej usługi (np. `Microsoft.Web/serverFarms`, `Microsoft.Sql/managedInstances`). Usługa dostaje wyłączne prawa do zarządzania siecią w tym subnecie.

**AWS — czy istnieje coś takiego?**
Nie ma **bezpośredniego odpowiednika** delegated subnets, ale AWS ma kilka mechanizmów zbliżonych:

#### AWS VPC Subnet Reservations (dla AWS services):

Niektóre usługi AWS **wymagają dedykowanych subnetów** i faktycznie przejmują zarządzanie siecią w nich:

| Usługa AWS | Wymaganie subnetu | Analogia do Azure Delegated |
|-----------|-------------------|-----------------------------|
| **AWS EKS** (managed node groups) | Dedykowane subnety tagowane `kubernetes.io/role/internal-elb` | Częściowe |
| **AWS EKS** (Load Balancer Controller) | Tagi na subnetach dla ALB/NLB | Częściowe |
| **AWS RDS** (Subnet Group) | Minimum 2 subnety w różnych AZ | Logiczne grupowanie |
| **AWS ElastiCache** (Subnet Group) | Minimum 2 subnety | Logiczne grupowanie |
| **AWS MSK** | Dedykowane subnety | Częściowe |
| **AWS Network Firewall** | Dedykowany subnet per AZ | Najbliższy analogii |

#### Network Firewall Subnet — najsilniejszy odpowiednik:
```mermaid
graph TD
    PUB["Public Subnet\n(IGW)"] --> FW_SUBNET["Firewall Subnet\n(dedykowany AWS Network Firewall)"]
    FW_SUBNET --> PRV["Private Subnet\n(App servers)"]
```
Firewall Subnet jest "zarezerwowany" dla Network Firewall Endpoint — inne zasoby nie powinny tam być. Funkcjonalnie zbliżone do delegacji.

#### AWS RAM (Resource Access Manager) — subnet sharing:
Zamiast delegacji, AWS używa **subnet sharing**:
```
Account A (Owner) → VPC Subnet → Share through AWS RAM → Account B (Participant)
Account B deploys EC2 w subnecie Account A
Account A zarządza routingiem/NACL, Account B zarządza EC2/SG
```

```bash
# Owner udostępnia subnet
aws ram create-resource-share \
  --name "shared-subnet" \
  --resource-arns arn:aws:ec2:region:account-a:subnet/subnet-xxx \
  --principals "111122223333"   # account B

# Account B może teraz deploy resources w tym subnecie
```

**Porównanie Azure vs AWS:**

| Mechanizm | Azure | AWS |
|-----------|-------|-----|
| Delegacja do managed service | Delegated Subnet (np. SQL MI) | Dedykowany subnet per service + tagging (EKS, NW Firewall) |
| Udostępnianie subnet między kontami | VNet Peering + RBAC | AWS RAM subnet sharing |
| Serwis zarządza ENI w subnecie | Delegated Subnet | AWS Managed prefixes (np. EKS, RDS managed ENI) |
| Wymagane tagi na subnetach | Label + delegation | AWS tags (k8s, elb controller) |

**Kluczowa różnica:** Azure delegacja to formalny mechanizm platformy z API. AWS używa **konwencji tagowania + dedykowanych subnetów** bez formalnej delegacji. Efekt podobny, mechanizm różny.

---

### 6.8 PrivateLink i VPC Endpoints vs Azure Private Link i Service Endpoints

**Najpierw strona Azure (dla kontekstu):**

| Azure | Opis |
|-------|------|
| **Service Endpoint** | Bezpośrednia trasa sieciowa do usługi Azure z subnetu. Tańszy. Usługa widzi Twój VNet IP. Nie tworzy prywatnego IP. |
| **Private Endpoint** | Prywatny IP (NIC) w Twoim subnecie dla usługi Azure. Usługa dostępna przez prywatny IP. Działa przez DNS. Droższy. |

**Strona AWS:**

| AWS | Azure Odpowiednik | Opis |
|-----|------------------|------|
| **VPC Gateway Endpoint** | Service Endpoint | Trasa w Route Table do S3/DynamoDB. Brak ENI. Darmowy. |
| **VPC Interface Endpoint (PrivateLink)** | Private Endpoint | ENI z prywatnym IP w Twoim subnecie. DNS. Płatny. |

**Szczegółowe porównanie:**

```mermaid
graph TB
    subgraph "Azure"
        SE["Service Endpoint\nRoute table extension\nBrak private IP\nTańszy"] 
        PE["Private Endpoint\nNIC w subnecie\nPrivate IP\nDroższy"]
    end
    subgraph "AWS"
        GE["Gateway Endpoint\nRoute Table entry\nBrak ENI\nDARMOWY\nTylko S3 + DynamoDB"]
        IE["Interface Endpoint\nENI w subnecie\nPrivate IP\n$0.01/h + $0.01/GB\n100+ services"]
    end
    SE -.->|"Odpowiednik"| GE
    PE -.->|"Odpowiednik"| IE
```

**Kluczowe różnice AWS vs Azure:**

| Cecha | AWS Gateway Endpoint | Azure Service Endpoint |
|-------|---------------------|----------------------|
| Tworzenie prywatnego IP | ❌ | ❌ |
| Koszt | **Darmowy** | Darmowy |
| Usługi | Tylko S3 i DynamoDB | Wiele usług Azure |
| On-prem dostęp | ❌ | ❌ |
| Dostęp cross-account | ❌ | ❌ |
| Implementacja | Route Table entry | Route Table extension |

| Cecha | AWS Interface Endpoint | Azure Private Endpoint |
|-------|----------------------|----------------------|
| Tworzenie prywatnego IP | ✅ ENI | ✅ NIC |
| Koszt | $0.01/h per AZ + $0.01/GB | $0.01/h + $0.01/GB |
| On-prem dostęp (przez DX/VPN) | ✅ | ✅ |
| Cross-account | ✅ | ✅ (cross-tenant) |
| Własne usługi (nie tylko cloud provider) | ✅ (Endpoint Service za NLB) | ✅ (Private Link Service za LB) |
| Private DNS | ✅ (opcja) | ✅ (Private DNS Zone) |

**AWS PrivateLink dla własnych usług:**
```mermaid
graph LR
    subgraph "Provider Account (dostawca usługi)"
        SVC["Twoja usługa\n(EC2/ECS/Lambda)"] --> NLB["Network Load Balancer"]
        NLB --> EPS["VPC Endpoint Service\n(PrivateLink)"]
    end
    subgraph "Consumer Account (odbiorca)"
        ENI["Interface Endpoint\nENI: 10.0.1.100"] --> APP["Aplikacja"]
    end
    EPS <-->|"AWS PrivateLink backbone"| ENI
```

**Azure Private Link Service** — dokładnie ten sam wzorzec:
```
Azure: Your Service → Azure Load Balancer → Private Link Service → Private Endpoint (NIC w VNet klienta)
AWS:   Your Service → NLB               → VPC Endpoint Service → Interface Endpoint (ENI w VPC klienta)
```

**Endpoint Policies (AWS) vs Service Endpoint Policies (Azure):**
Oba pozwalają ograniczyć dostęp przez endpoint do konkretnych zasobów (konkretnych bucketów S3, konkretnych storage accounts).

**Kiedy Gateway vs Interface Endpoint:**
```
S3 lub DynamoDB → zawsze Gateway Endpoint (darmowy!)
Inne usługi AWS → Interface Endpoint
On-prem dostęp przez DX → Interface Endpoint (Gateway nie działa on-prem)
Własna usługa dla innych kont → Interface Endpoint (PrivateLink)
```

---

### 6.9 Czy subnet jest regionalny?

**NIE — subnet jest zasobem AZ-level (Availability Zone).**

```
Region: eu-west-1
  ├── AZ: eu-west-1a
  │   ├── subnet-aaa (10.0.1.0/24)
  │   └── subnet-bbb (10.0.11.0/24)
  ├── AZ: eu-west-1b
  │   ├── subnet-ccc (10.0.2.0/24)
  │   └── subnet-ddd (10.0.12.0/24)
  └── AZ: eu-west-1c
      ├── subnet-eee (10.0.3.0/24)
      └── subnet-fff (10.0.13.0/24)

VPC: eu-west-1 (REGIONALNY — obejmuje wszystkie AZ w regionie)
Subnet: eu-west-1a lub eu-west-1b lub eu-west-1c (AZ-LEVEL)
```

**VPC jest regionalny, subnet jest AZ-level:**

| Zasób | Zakres | Uwagi |
|-------|--------|-------|
| **VPC** | **Region** | Obejmuje wszystkie AZ w regionie |
| **Subnet** | **AZ** | Dokładnie jedna AZ |
| Route Table | Region (VPC-scoped) | Jeden VPC, przypisywana do subnetów w różnych AZ |
| Security Group | Region (VPC-scoped) | Jeden VPC, używana przez zasoby w dowolnym subnecie VPC |
| NACL | Region (VPC-scoped) | Jeden VPC, przypisywana do subnetów w różnych AZ |
| IGW | Region (VPC-scoped) | Jeden VPC |
| NAT Gateway | **AZ** | Jeden NAT GW w jednym subnecie (jednej AZ) |
| VPC Endpoint (Interface) | AZ-level ENI | Tworzysz per-AZ dla HA |

**Konsekwencje AZ-scope subnetu:**
```
EC2 w subnet eu-west-1a → fizycznie w eu-west-1a
Jeśli eu-west-1a ma awarię → EC2 niedostępna
Rozwiązanie: deploy w wielu subnetach w różnych AZ + ALB/ASG

NAT GW w subnet eu-west-1a → obsługuje tylko subnety z route do niego
Jeśli eu-west-1a ma awarię → NAT GW niedostępny
Rozwiązanie: NAT GW per AZ
```

**Azure porównanie:**

| AWS | Azure |
|-----|-------|
| VPC (Regional) | VNet (Regional) |
| Subnet (AZ-level) | Subnet (**Regional** — to różnica!) |

> ⚠️ **Kluczowa różnica AWS vs Azure:** Azure subnety są **regionalne** — rozciągają się na wszystkie AZ w regionie. AWS subnety są **AZ-level**. Musisz tworzyć osobny subnet per AZ w AWS dla HA. W Azure jeden subnet obsługuje wiele AZ automatycznie.

---

### 6.10 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Firma ma 5 biur (Warszawa, Berlin, Paryż, Londyn, Madryt) i VPC w AWS. Każde biuro musi komunikować się z VPC i z innymi biurami. Budżet ograniczony.

**Rozwiązanie VPN CloudHub:**
```
Virtual Private Gateway w VPC
  ← VPN Connection 1: Warszawa (BGP AS 65001, 192.168.1.0/24)
  ← VPN Connection 2: Berlin   (BGP AS 65002, 192.168.2.0/24)
  ← VPN Connection 3: Paryż    (BGP AS 65003, 192.168.3.0/24)
  ← VPN Connection 4: Londyn   (BGP AS 65004, 192.168.4.0/24)
  ← VPN Connection 5: Madryt   (BGP AS 65005, 192.168.5.0/24)

Koszt: 5 × $0.05/h = $0.25/h vs Transit Gateway $0.05/h TGW + 5 × $0.05/h attachments
```

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Nowa aplikacja SaaS — EKS (IPv6-native), ALB dual stack, RDS dual stack. Projekt sieci.

**Dual Stack VPC design:**
```
VPC:
  IPv4: 10.0.0.0/16
  IPv6: 2600:1f18:abc::/56 (AWS-provided)

Subnety per AZ (3 AZ):
  Public:  /24 IPv4 + /64 IPv6 → ALB (dualstack ip-address-type)
  Private: /24 IPv4 + /64 IPv6 → EKS nodes
  Data:    /24 IPv4 + /64 IPv6 → RDS

Route Tables:
  Public:  0.0.0.0/0 → igw, ::/0 → igw
  Private: 0.0.0.0/0 → nat, ::/0 → eigw (Egress-Only IGW)
  Data:    (tylko local IPv4 + IPv6)

EKS: --ip-family ipv6 (pods dostają IPv6 z subnet pool)
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma migruje z Azure. Mają Private Endpoints dla Azure SQL i Azure Storage. Jak odwzorować w AWS?

```
Azure Private Endpoint (SQL) → AWS Interface Endpoint + PrivateLink dla RDS Proxy
Azure Private Endpoint (Storage) → AWS Gateway Endpoint dla S3 (darmowy!)
Azure Service Endpoint (Storage) → AWS Gateway Endpoint dla S3
Azure Private DNS Zone → AWS Route 53 Private Hosted Zone
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Shared Services VPC udostępnia wewnętrzne API innym 50 VPC (różne konta). Bez VPC Peering mesh.

**Rozwiązanie: PrivateLink Endpoint Service:**
```
Shared Services VPC:
  Internal API (ECS) → NLB → VPC Endpoint Service

50 Spoke VPCs (różne konta):
  Interface Endpoint → wskazuje na Endpoint Service
  Private DNS: api.internal.company.com → prywatny IP ENI

Zalety vs Peering mesh:
  - Nie wymaga non-overlapping CIDRs
  - Nie daje dostępu do całego VPC (tylko API)
  - Skaluje bez zarządzania 1225 peeringów
  - Cross-account działa natywnie
```

---

## 7. AWS ARM — Graviton i Architektura ARM w EC2

> 💡 **Kontekst egzaminacyjny:** Na egzaminie SAP-C02 Graviton pojawia się głównie w pytaniach o optymalizację kosztów, wydajność i migrację workloadów. Kluczowe: kiedy wybrać Graviton zamiast x86, jak migrować, jakie usługi go wspierają.

---

### 7.1 Co to jest ARM i Graviton?

**ARM** (Advanced RISC Machines) to architektura procesorów oparta na uproszczonym zestawie instrukcji (RISC — Reduced Instruction Set Computer). Procesory ARM dominują w urządzeniach mobilnych (iPhone, Android) i coraz bardziej w serwerach.

**AWS Graviton** to seria procesorów ARM zaprojektowanych przez AWS (silicon własny, podobnie jak Apple M1/M2). Graviton działa wyłącznie w środowisku AWS — nie ma odpowiednika on-premises ani w innych chmurach.

#### Analogia z życia codziennego
- **x86 (Intel/AMD)** = silnik V8 w samochodzie — duża moc, duże zużycie paliwa
- **ARM (Graviton)** = silnik hybrydowy — podobna lub wyższa wydajność na watt, niższe koszty operacyjne

#### Analogia Linux/Open-source
- ARM na serwerach Linux to nie nowość — Raspberry Pi, AWS A1 (pierwszy ARM w EC2 z 2018)
- Porównaj: x86-64 (AMD64) vs AArch64 (ARM64) — różne ISA (Instruction Set Architecture)
- Kod kompilowany dla x86 **nie działa** na ARM bez rekompilacji lub emulacji

#### Analogia Azure
| AWS | Azure |
|-----|-------|
| Graviton (ARM64) | Ampere Altra (ARM64) — seria Dpsv5, Dplsv5 |
| Własny chip | Chip Ampere (zewnętrzny producent) |
| Głębsza integracja z usługami | Dostępne, ale mniej zintegrowane |

---

### 7.2 Generacje Graviton

| Generacja | Rok | Architektura | Poprawa vs poprzedni | Zastosowanie |
|-----------|-----|-------------|---------------------|--------------|
| **Graviton1** | 2018 | ARMv8, 16 cores, 64-bit | baseline | A1 instances (legacy) |
| **Graviton2** | 2020 | ARMv8.2, do 64 vCPU | 7x wydajność vs G1, 40% lepszy niż x86 (koszt/wydajność) | t4g, m6g, c6g, r6g, x2gd |
| **Graviton3** | 2022 | ARMv8.2+, do 64 vCPU | 25% szybszy vs G2, 2x FP, 2x krypto, 3x ML | c7g, m7g, r7g, hpc7g |
| **Graviton3E** | 2022 | HPC-optimized | 35% wyższa wydajność wektorowa vs G3 | hpc7g (HPC workloads) |
| **Graviton4** | 2024 | ARMv9, do 96 vCPU | 30% szybszy vs G3, 50% więcej RAM | r8g, c8g, m8g |

> 🎯 **Exam tip:** Graviton2 = pierwszy mainstream. Graviton3 = dodane ulepszone wsparcie ML i krypto. Graviton4 = ARMv9, największy skok.

#### Zasięg geograficzny
- Graviton2: większość regionów AWS
- Graviton3: dostępny w głównych regionach (us-east-1, eu-west-1, ap-southeast-1 itd.)
- Graviton4: stopniowe rollout od 2024

> ⚠️ **Zawsze sprawdź dostępność w docelowym regionie** — nie wszystkie generacje we wszystkich regionach!

---

### 7.3 Typy instancji EC2 ARM (rodziny g-suffix)

Instancje ARM w EC2 mają sufiks **`g`** w nazwie rodziny:

| Typ | Rodzina ARM | Odpowiednik x86 | Zastosowanie |
|-----|-------------|-----------------|--------------|
| `t4g` | Graviton2 | `t3` | Dev/test, low traffic, burstable |
| `m6g`, `m7g`, `m8g` | G2/G3/G4 | `m6i`, `m7i` | General purpose — web servers, app servers |
| `c6g`, `c7g`, `c8g` | G2/G3/G4 | `c6i`, `c7i` | Compute-optimized — HPC, batch, encoding |
| `r6g`, `r7g`, `r8g` | G2/G3/G4 | `r6i`, `r7i` | Memory-optimized — in-memory DB, caching |
| `x2gd` | G2 | `x2idn` | Memory-intensive — SAP HANA, large DB |
| `im4gn`, `is4gen` | G2 | `i4i` | Storage-optimized — NVMe SSD, DB |
| `hpc7g` | G3E | brak ARM-HPC x86-equiv | HPC — MPI, tightly-coupled compute |

#### Nomeklatura EC2 — jak czytać?
```
c  7  g  .  2xlarge
│  │  │     └─ rozmiar
│  │  └──────── g = Graviton (ARM)
│  └─────────── 7 = generacja (7 = Graviton3)
└────────────── c = compute-optimized
```

#### Rozmiary instancji — co oznaczają?
| Rozmiar | vCPU | RAM (przykład m7g) |
|---------|------|---------------------|
| `.medium` | 1 | 4 GB |
| `.large` | 2 | 8 GB |
| `.xlarge` | 4 | 16 GB |
| `.2xlarge` | 8 | 32 GB |
| `.4xlarge` | 16 | 64 GB |
| `.8xlarge` | 32 | 128 GB |
| `.12xlarge` | 48 | 192 GB |
| `.16xlarge` | 64 | 256 GB |
| `.metal` | 64 | 256 GB (bare metal, brak hypervisor) |

---

### 7.4 Graviton vs x86 — porównanie szczegółowe

#### Koszt
| Metryka | Graviton (ARM) | x86 (Intel/AMD) |
|---------|---------------|-----------------|
| Cena za instancję | ~20% taniej | Baseline |
| Koszt/wydajność | Do 40% lepszy | Baseline |
| Spot prices | Proporcjonalnie niższe | Baseline |
| Reserved/Savings Plans | Proporcjonalnie niższe | Baseline |

#### Wydajność

```mermaid
graph LR
    A[Graviton3 c7g.xlarge] -->|25% szybszy single-thread| B[Graviton2 c6g.xlarge]
    B -->|40% lepszy perf/cost| C[x86 c5.xlarge]
    A -->|porównywalna lub lepsza| D[x86 c6i.xlarge]
    
    style A fill:#ff9900,color:#000
    style B fill:#ffb84d,color:#000
```

#### Co działa lepiej na Graviton?
- ✅ Web servers (nginx, Apache) — CPU-bound, kompilowane dla ARM
- ✅ Microservices (Go, Rust, Java, Python, Node.js)
- ✅ Kontenery (ECS, EKS) — większość obrazów ma multi-arch support
- ✅ Bazy danych (MySQL, PostgreSQL, Redis) — natywne pakiety ARM
- ✅ Media encoding (FFmpeg z ARM NEON)
- ✅ ML inference (szczególnie Graviton3 z instrukcjami bfloat16)

#### Co może nie działać lub wymaga pracy?
- ⚠️ Stary software bez ARM build (legacy Java JARs z natywnym JNI)
- ⚠️ Aplikacje C/C++ z x86-specific intrinsics (SSE2, AVX)
- ⚠️ Gotowe binaries bez ARM variant
- ⚠️ Windows workloads — **Graviton nie obsługuje Windows!**
- ❌ GPU workloads — Graviton nie ma GPU, używaj p4/p5 (NVIDIA)

> 🎯 **Exam tip:** Graviton **nie wspiera Windows**. Pytania o Windows EC2 → wybieraj x86.

---

### 7.5 Kiedy używać Graviton, kiedy x86?

```mermaid
flowchart TD
    A[Nowy workload lub migracja] --> B{Windows?}
    B -->|TAK| C[❌ x86 obowiązkowo\nm5, c5, r5...]
    B -->|NIE| D{Masz gotowe binaries\nbez ARM support?}
    D -->|TAK| E{Możliwość\nrekompilacji?}
    E -->|NIE| C
    E -->|TAK| F[✅ Graviton — rekompiluj i migruj]
    D -->|NIE - open source\nlub własny kod| G{Potrzebujesz GPU?}
    G -->|TAK| H[❌ x86 z GPU\np3/p4/p5/g5]
    G -->|NIE| I[✅ Graviton preferowany\noszczędność ~20-40%]
```

#### Tabela decyzyjna

| Scenariusz | Rekomendacja | Dlaczego |
|------------|-------------|----------|
| Web/API servers (Linux) | ✅ Graviton | 20-40% oszczędność, pełna kompatybilność |
| Kontenery multi-arch | ✅ Graviton | Większość images ma `linux/arm64` |
| Windows Server | ❌ x86 | Graviton nie wspiera Windows |
| GPU ML training | ❌ x86 (p4, p5) | Graviton bez GPU |
| Legacy .NET Framework | ❌ x86 | .NET Framework = Windows only |
| .NET Core / .NET 5+ | ✅ Graviton | Cross-platform, ARM support |
| SAP HANA | ✅ x2gd (Graviton) | Certyfikowany, zoptymalizowany |
| HPC/MPI | ✅ hpc7g (G3E) | 35% wyższa wydajność wektorowa |
| Lambda (cost optimization) | ✅ Graviton2 | ~20% taniej, 19% szybciej |
| RDS MySQL/PostgreSQL | ✅ Graviton | Dostępne, niższe koszty |

---

### 7.6 Usługi AWS wspierające Graviton

Graviton to nie tylko EC2 — wiele managed services korzysta z ARM pod spodem:

| Usługa | Graviton Support | Szczegóły |
|--------|-----------------|-----------|
| **EC2** | ✅ Pełne | Wszystkie rodziny g-suffix |
| **Lambda** | ✅ arm64 | `architecture: arm64` w konfiguracji |
| **ECS/EKS** | ✅ | Task/Node definition — wybór architektury |
| **RDS** | ✅ | MySQL, PostgreSQL, MariaDB na Graviton |
| **Aurora** | ✅ | Aurora MySQL i PostgreSQL |
| **ElastiCache** | ✅ | Redis na Graviton (cache.r7g) |
| **OpenSearch** | ✅ | or7g node types |
| **EMR** | ✅ | m6g, c6g, r6g node types |
| **Fargate** | ✅ | `cpuArchitecture: ARM64` |

#### Lambda z Graviton2 — szczegóły
```yaml
# AWS SAM / CloudFormation
MyFunction:
  Type: AWS::Lambda::Function
  Properties:
    Architectures:
      - arm64          # Graviton2
    Runtime: python3.11
    # ~20% tańszy, ~19% szybszy cold start
```

> 🎯 **Exam tip:** Lambda z `arm64` (Graviton) jest tańsza i szybsza niż domyślna `x86_64`. Pytania o optymalizację kosztów Lambda → rozważ zmianę architektury.

#### RDS na Graviton
```bash
# Tworzenie RDS na Graviton
aws rds create-db-instance \
  --db-instance-class db.m6g.large \    # g = Graviton
  --engine mysql \
  --engine-version 8.0
```

---

### 7.7 Migracja z x86 na ARM — strategie

#### Strategia 1: Lift & Shift (kontenery)

Najłatwiejsze podejście dla kontenerów — rebuild multi-arch image:

```bash
# Budowanie multi-arch Docker image
docker buildx create --use
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t myapp:latest \
  --push .

# Sprawdzenie dostępnych architektur
docker manifest inspect myapp:latest
```

#### Strategia 2: EC2 — zmiana AMI i instance type

```bash
# Krok 1: Znajdź ARM AMI (Amazon Linux 2 ARM)
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=amzn2-ami-hvm-*-arm64-gp2" \
  --query 'Images[0].ImageId'

# Krok 2: Launch nowej instancji ARM
aws ec2 run-instances \
  --image-id ami-xxx-arm64 \
  --instance-type m6g.large \
  --key-name mykey

# Krok 3: Test aplikacji na ARM
# Krok 4: Blue/green switch (Route 53 lub ALB)
```

#### Strategia 3: Lambda — zmiana architektury

```bash
aws lambda update-function-configuration \
  --function-name my-function \
  --architectures arm64
# UWAGA: wymaga rebuild kodu dla arm64 jeśli masz native dependencies!
```

#### Strategia 4: Testy kompatybilności

```mermaid
flowchart LR
    A[Analiza zależności] --> B[Build ARM version]
    B --> C[Testy jednostkowe]
    C --> D[Test na instancji ARM - dev]
    D --> E{Testy OK?}
    E -->|NIE - błędy native| F[Identyfikuj native deps]
    F --> G[Znajdź ARM-compatible alternatives]
    G --> B
    E -->|TAK| H[Load test na ARM]
    H --> I[Blue/Green deploy na ARM prod]
    I --> J[Monitor CloudWatch]
    J --> K{Problemy?}
    K -->|TAK| L[Rollback do x86]
    K -->|NIE| M[✅ Migracja kompletna]
```

#### Narzędzia pomocne przy migracji
| Narzędzie | Zastosowanie |
|-----------|-------------|
| **AWS Graviton Ready Program** | Lista software certyfikowanego dla Graviton |
| **QEMU** | Emulacja ARM na x86 (wolniejsza, ale do testów) |
| **Docker Buildx** | Multi-arch image build |
| **AWS CodeBuild** | CI/CD z ARM build environment |
| **Porting Advisor for Graviton** | AWS tool — skanuje kod C/C++/Fortran pod ARM compatibility |

---

### 7.8 Bezpieczeństwo i niezawodność na Graviton

#### Bezpieczeństwo wbudowane w Graviton

| Feature | Graviton2+ | Opis |
|---------|-----------|------|
| **Pointer Authentication** | Graviton3+ (ARMv8.5) | Chroni przed ROP (Return-Oriented Programming) attacks |
| **Memory Tagging (MTE)** | Graviton4 (ARMv9) | Wykrywa use-after-free, buffer overflows w hardware |
| **Nitro System** | Wszystkie | Izolacja hypervisor, dedykowany hardware security |
| **ARM TrustZone** | Wbudowany | Hardware security boundary |
| **Encrypted memory** | Graviton2+ | Automatyczne szyfrowanie RAM |

#### Nitro System — kluczowy dla bezpieczeństwa
Wszystkie instancje Graviton działają na **AWS Nitro System**:
- Dedykowane karty Nitro dla sieci i storage (nie współdzielone z CPU hosta)
- Hypervisor zredukowany do minimum — mniejsza attack surface
- Weryfikacja kryptograficzna firmware

> 🎯 **Exam tip:** Nitro = lepsza izolacja, wyższa wydajność sieci i storage, obowiązkowe dla bare metal.

#### Niezawodność — Multi-AZ

```mermaid
graph TB
    subgraph AZ-1[Availability Zone 1]
        G1[m7g.xlarge\nGraviton3]
        G2[m7g.xlarge\nGraviton3]
    end
    subgraph AZ-2[Availability Zone 2]
        G3[m7g.xlarge\nGraviton3]
        G4[m7g.xlarge\nGraviton3]
    end
    ALB[Application Load Balancer] --> G1
    ALB --> G2
    ALB --> G3
    ALB --> G4
    ASG[Auto Scaling Group\nMixed Instance Policy] --> G1
    ASG --> G3
```

#### Auto Scaling — Mixed Instance Policy (x86 + ARM)

```json
{
  "MixedInstancesPolicy": {
    "LaunchTemplate": {
      "LaunchTemplateSpecification": {
        "LaunchTemplateId": "lt-xxx",
        "Version": "$Latest"
      },
      "Overrides": [
        {"InstanceType": "m7g.xlarge"},
        {"InstanceType": "m7i.xlarge"},
        {"InstanceType": "m6g.xlarge"}
      ]
    },
    "InstancesDistribution": {
      "OnDemandPercentageAboveBaseCapacity": 20,
      "SpotAllocationStrategy": "price-capacity-optimized"
    }
  }
}
```

> 💡 Mixed Instance Policy z ARM + x86 = resilience + cost optimization. ALB nie "widzi" różnicy — balansuje na poziomie IP.

---

### 7.9 Graviton w kontekście Well-Architected Framework

| Filar | Zastosowanie Graviton | Praktyczne kroki |
|-------|----------------------|-----------------|
| **⚙️ Operational Excellence** | Łatwiejsze zarządzanie przez managed services (RDS, Lambda arm64) | Używaj Systems Manager dla fleet management; CloudWatch metrics identyczne dla ARM i x86 |
| **🔒 Security** | Graviton3/4 — pointer authentication, memory tagging; Nitro isolation | Włącz IMDSv2 obowiązkowo; Graviton nie zmienia modelu security (IAM, SG, NACL — te same zasady) |
| **🔄 Reliability** | Brak specyficznych ograniczeń ARM dla HA | Multi-AZ deployment; Mixed Instance Policy (ARM+x86) dla większej puli Spot; Auto Scaling — te same mechanizmy |
| **⚡ Performance Efficiency** | 20-40% lepszy perf/cost; Graviton3 2x FP, 3x ML; Graviton4 50% więcej RAM | Benchmarkuj swoją aplikację (nie zakładaj poprawy — mierz); używaj Compute Optimizer do rekomendacji |
| **💰 Cost Optimization** | ~20% niższe ceny On-Demand; proporcjonalne Spot/Reserved oszczędności; Lambda arm64 tańsza | AWS Compute Optimizer analizuje i sugeruje migrację; Savings Plans działają cross-architecture |
| **🌱 Sustainability** | ARM = niższe zużycie energii na operację obliczeniową | Graviton zużywa 60% mniej energii vs porównywalne x86 — mniejszy carbon footprint |

---

### 7.10 4 Domeny egzaminacyjne

#### 🏢 Domain 1: Design Solutions for Organizational Complexity

**Multi-account Graviton strategy:**
- Organizacje z wieloma kontami mogą standaryzować na Graviton przez AWS Organizations SCP
- Przykład SCP wymuszający użycie Graviton dla nowych zasobów (cost governance)
- AWS Compute Optimizer działa cross-account w Organizations

**Heterogeneous fleet management:**
- AWS Systems Manager Fleet Manager obsługuje ARM i x86 jednakowo
- Patch Manager — ARM instances używają tych samych Patch Baselines
- Instance Types w OUs: możliwość ograniczenia do Graviton przez Service Control Policies

#### 🆕 Domain 2: Design for New Solutions

**Greenfield architecture na Graviton:**
- Nowe aplikacje containerized → ECS/EKS z `ARM64` node groups
- Serverless → Lambda `arm64` jako default
- Database tier → RDS/Aurora na Graviton (db.m7g, db.r7g)
- Caching → ElastiCache r7g

**Compute Optimizer — automatyczna rekomendacja:**
```bash
# Pobierz rekomendacje Compute Optimizer
aws compute-optimizer get-ec2-instance-recommendations \
  --account-ids 123456789012

# Wynik zawiera:
# - currentInstanceType: m5.xlarge
# - recommendedInstanceType: m7g.xlarge  ← Graviton suggestion
# - estimatedMonthlySavings: $45.20
```

#### 🔧 Domain 3: Continuous Improvement for Existing Solutions

**Incremental migration strategy:**
1. Compute Optimizer → identyfikuj instancje kandydatki
2. Dev/Test environment → pierwsza migracja
3. A/B testing: Route 53 weighted routing (10% ARM, 90% x86)
4. Monitoring: CloudWatch, X-Ray — brak różnic w telemetrii
5. Full cutover

**Performance monitoring:**
```bash
# CloudWatch metrics — identyczne dla ARM i x86
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-xxx-graviton
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Containers modernization:**
- Lift & shift: rebuild Docker images jako multi-arch
- `docker buildx` + ECR multi-arch manifest
- ECS task definition: `runtimePlatform: {cpuArchitecture: ARM64}`
- EKS: node groups z `m7g` instances

**Database modernization:**
- On-premises MySQL → RDS MySQL na db.m6g (Graviton) — oszczędność + managed
- Self-managed Redis → ElastiCache Redis na cache.r7g

---

## 8. DHCP Option Sets, NACLs, Security Groups, VPC Peering, Elastic IP, NAT Gateway, Transit VPC, EC2 Router

### 8.1 DHCP Option Sets

**🔵 Co to jest?**
**DHCP Option Set** to zestaw parametrów przekazywanych instancjom EC2 przez protokół DHCP przy starcie (lub odświeżeniu dzierżawy IP). VPC ma zawsze dokładnie jeden aktywny DHCP Option Set.

> **Analogia Linux/on-prem:** Jak `/etc/dhcpd.conf` — serwer DHCP odpowiada nie tylko adresem IP, ale też domeną DNS, serwerami DNS, NTP itd.

**Parametry DHCP Option Set:**

| Opcja DHCP | Wartość domyślna AWS | Opis |
|-----------|---------------------|------|
| `domain-name-servers` | `AmazonProvidedDNS` | Serwery DNS (max 4). AmazonProvidedDNS = VPC+2 (np. 10.0.0.2 lub 169.254.169.253) |
| `domain-name` | Region-specific (np. `ec2.internal` w us-east-1) | Sufiks domeny dla EC2 DNS |
| `ntp-servers` | (brak) | Adresy serwerów NTP (max 4) |
| `netbios-name-servers` | (brak) | Dla Windows WINS/NetBIOS resolution |
| `netbios-node-type` | (brak) | Typ NetBIOS (2 = p-node, rekomendowany) |

**Jak działa w AWS:**

```mermaid
sequenceDiagram
    participant EC2 as EC2 Instance (start)
    participant DHCP as VPC DHCP Server
    participant DNS as Route 53 Resolver (AmazonProvidedDNS)

    EC2->>DHCP: DHCP Discover
    DHCP-->>EC2: DHCP Offer (IP, subnet mask, gateway, DNS, domain)
    EC2->>DHCP: DHCP Request
    DHCP-->>EC2: DHCP ACK
    Note over EC2: Konfiguracja: IP, GW, DNS serwer
    EC2->>DNS: Resolve hostname
    DNS-->>EC2: IP
```

**Zmiana DHCP Option Set:**
- Jeden VPC → jeden DHCP Option Set naraz
- Można stworzyć nowy i przypisać do VPC — stary jest odłączony
- Zmiana jest **natychmiastowa** dla VPC, ale instancje aplikują ją dopiero po **wygaśnięciu dzierżawy DHCP** (domyślnie co godzinę) lub ręcznym `dhclient -r && dhclient`
- **Nie można edytować** istniejącego DHCP Option Set — trzeba stworzyć nowy

**Kiedy zmieniać DHCP Option Set:**
- ✅ Własne serwery DNS (on-prem AD, custom resolver) zamiast AmazonProvidedDNS
- ✅ Custom domain suffix dla EC2 hostname resolution
- ✅ Własny NTP (np. synchronizacja z on-prem time servers)
- ✅ Windows domain-joined środowiska (NetBIOS/WINS)

**Custom DNS z własnym AD:**
```
Scenariusz: AWS Managed Microsoft AD → instancje muszą używać DC jako DNS
DHCP Option Set: domain-name-servers = 10.0.1.10, 10.0.1.11 (IP DC)
                 domain-name = corp.example.com
```

**Ograniczenia:**
- Nie możesz wyłączyć DHCP w VPC (zawsze aktywny)
- Nie możesz ustawiać opcji per-subnet (tylko per-VPC)
- Opcja `domain-name-servers = AmazonProvidedDNS` wymagana dla poprawnego działania Route 53 Resolver

**Zasięg:** Regionalny (per VPC)

**Azure odpowiednik:** Azure VNet DNS Settings (Custom DNS Servers) — podobna koncepcja, ale konfiguracja na poziomie VNet lub NIC bezpośrednio.

> ⚠️ **Egzamin:** Jeśli EC2 nie może rozwiązać hostnamów AD po domain-join → prawdopodobnie DHCP Option Set nie wskazuje na DC jako DNS. Zawsze wskaż IP Domain Controllerów.

---

### 8.2 Network ACL (NACL) — szczegóły zaawansowane

> Podstawy NACL omówione w sekcji 3.6. Tutaj rozszerzenie o zaawansowane przypadki.

**NACL a ARM/Azure — pełne porównanie:**

| Cecha | AWS NACL | Azure NSG (subnet) | Linux iptables (stateless) |
|-------|----------|-------------------|---------------------------|
| Poziom | Subnet | Subnet | Interface/Chain |
| Stateful | ❌ | ✅ (stanowy!) | Zależy od conntrack |
| Reguły Allow+Deny | ✅ | ✅ | ✅ |
| Kolejność | Numeryczna | Priorytet | Top-to-bottom |
| Default nowy | Deny all | Deny all | Zależy od policy |
| First match | ✅ | ✅ | ✅ |

> **Kluczowa różnica AWS vs Azure:** Azure NSG jest **stateful** (jak Security Group AWS) nawet gdy przypisane do subnetu! AWS NACL jest **stateless** — to fundamentalna różnica wymagająca pamiętania o ephemeral ports.

**NACL Best Practices:**
```
Rule numbering convention:
100  — Allow ruch aplikacyjny (HTTPS, SSH od bastion)
200  — Allow ephemeral ports (1024-65535) dla return traffic
900  — Allow inne specyficzne reguły
*    — DENY all (domyślna ostatnia reguła)

Zostaw luki (100, 200, nie 101, 102) — łatwiej wstawić reguły między istniejące
```

**NACL w scenariuszu defense-in-depth:**
```mermaid
graph LR
    INET((Internet)) 
    INET -->|TCP 443| NACL_PUB["NACL Public Subnet\n✅ Allow 443 inbound\n✅ Allow 1024-65535 outbound\n❌ Deny 22 from 0.0.0.0/0"]
    NACL_PUB --> SG_ALB["SG ALB\n✅ Allow 443 from 0.0.0.0/0"]
    SG_ALB --> ALB[ALB]
    ALB --> NACL_PRV["NACL Private Subnet\n✅ Allow 8080 from public subnet CIDR\n✅ Allow 1024-65535 outbound\n❌ Deny direct Internet access"]
    NACL_PRV --> SG_APP["SG App\n✅ Allow 8080 from sg-alb"]
    SG_APP --> EC2[EC2]
```

**Kiedy NACL > Security Group:**
- Blokowanie IP w czasie ataku (DDoS mitigation): Security Group nie ma Deny → NACL jest jedynym miejscem szybkiego bloku
- Isolation całego subnetu w czasie incydentu
- Zgodność z compliance (PCI-DSS, HIPAA) wymagająca stateless firewall na poziomie sieci

**NACL a VPC Peering:**
Przy VPC Peering — NACL jest respektowane po obu stronach! Ruch z Peering przechodzi przez NACL subnetu docelowego. Musisz otworzyć NACL w obu VPC.

**Zasięg:** Regionalny (per VPC, per subnet)

---

### 8.3 Security Groups — szczegóły zaawansowane

> Podstawy SG omówione w sekcji 3.5. Tutaj rozszerzenie o zaawansowane case'y.

**Security Group a ARM/Azure — pełne porównanie:**

| Cecha | AWS Security Group | Azure NSG (NIC) | Linux iptables stateful |
|-------|-------------------|-----------------|------------------------|
| Poziom | ENI (Elastic Network Interface) | NIC | Interface |
| Stateful | ✅ | ✅ | Zależy od conntrack |
| Allow only | ✅ (brak Deny) | ✅+❌ (Allow+Deny) | ✅+❌ |
| Priorytet | Wszystkie razem (union) | Numeryczny | Top-to-bottom |
| Cross-resource reference | ✅ SG ID jako source | ✅ ASG reference | ❌ |
| Default wychodząca | Allow all | Zależy od reguł | Zależy |

**Security Group Referencing — zaawansowane:**

```
Cross-VPC SG reference (przez VPC Peering):
  VPC A (10.0.0.0/16) i VPC B (10.1.0.0/16) połączone Peeringiem
  
  SG w VPC B może mieć regułę: Allow TCP 5432 from sg-xxxxxx (SG z VPC A)
  → Traffic z EC2 w VPC A (z tym SG) będzie dozwolony do RDS w VPC B
  → Nie trzeba hardkodować CIDR — dynamicznie śledzi EC2 w source SG
```

**Security Group dla ECS/EKS:**
```
ECS Task Security Group → każdy task ma własny ENI (awsvpc mode)
  → Możesz mieć per-task SG zamiast per-EC2-host SG
  → Izolacja między taskami na tym samym hoście
  
EKS Pod Security Group (pod-level SG):
  → Feature: Security groups for pods
  → Każdy pod AWS VPC CNI dostaje ENI → SG per pod możliwe
```

**Security Group limits (ważne na egzamin):**

| Limit | Wartość domyślna | Max |
|-------|-----------------|-----|
| SG per VPC | 2500 | 2500 |
| Rules per SG (inbound) | 60 | 1000 |
| Rules per SG (outbound) | 60 | 1000 |
| SG per ENI | 5 | 16 |

**Security Group dla NAT Gateway:**
> ⚠️ NAT Gateway **NIE ma Security Group** — jest zarządzany przez AWS i nie eksponuje ENI do przypisania SG. Kontrola przez NACL lub przez SG instancji źródłowych.

**Security Group dla PrivateLink endpoints (Interface Endpoints):**
Interface VPC Endpoint (PrivateLink) **ma Security Group** — kontrolujesz ruch do endpointu. Gateway Endpoint (S3/DynamoDB) nie ma SG.

**Zasięg:** Regionalny (per VPC, przypisany do zasobów)

---

### 8.4 NACL vs Security Group — porównanie i kontekst Azure/ARM

**Pełna tabela:**

| Cecha | Security Group | NACL | Azure NSG (NIC) | Azure NSG (Subnet) |
|-------|---------------|------|-----------------|-------------------|
| **Poziom** | ENI/Zasób | Subnet | NIC | Subnet |
| **Stateful** | ✅ | ❌ | ✅ | ✅ |
| **Deny rules** | ❌ | ✅ | ✅ | ✅ |
| **Kolejność** | Wszystkie razem | Numeryczna | Priorytet | Priorytet |
| **Ephemeral ports** | Auto | Manual | Auto | Auto |
| **Default nowy** | Deny in, Allow out | Deny all | Deny all | Deny all |
| **Zasięg** | Zasób | Subnet | NIC | Subnet |
| **Linux analog** | iptables stateful | iptables stateless | iptables stateful | iptables stateless |

```mermaid
flowchart LR
    INET((Internet)) -->|ruch wchodzący| NACL_IN["🔒 NACL Inbound\n(stateless, subnet)"]
    NACL_IN --> SG_IN["🔒 SG Inbound\n(stateful, ENI)"]
    SG_IN --> EC2[EC2/RDS/Lambda]
    EC2 --> SG_OUT["🔒 SG Outbound\n(stateful, ENI)"]
    SG_OUT --> NACL_OUT["🔒 NACL Outbound\n(stateless, subnet)"]
    NACL_OUT --> INET
```

**Kiedy używać NACL, kiedy SG:**

| Cel | Użyj |
|-----|------|
| Blokowanie złośliwego IP | NACL (Deny) |
| Izolacja subnetu (incydent) | NACL |
| Mikrosegmentacja między zasobami | Security Group |
| Layer 4 firewall per zasób | Security Group |
| Compliance (stateless firewall layer) | NACL |
| Szybkie reguły dla developmentu | Security Group |
| Automatyczne blokowanie po trigger (Lambda → NACL API) | NACL |

**Defense in depth — obie warstwy:**
```
Ruch MUSI przejść przez NACL (subnet) ORAZ SG (ENI).
Obie warstwy muszą przepuszczać — jedna blokuje = ruch odrzucony.
```

---

### 8.5 VPC Peering — zaawansowane

**🔵 Co to jest?**
VPC Peering to **bezpośrednie połączenie sieciowe** między dwoma VPC (to samo konto, różne konta, różne regiony), przez które ruch przechodzi infrastrukturą AWS (nie przez Internet).

> **Analogia:** Jak bezpośredni kabel Ethernet między dwoma sieciami — pakiety trafiają bezpośrednio, nie przez router brzegowy.

**Odpowiedniki:**

| AWS | Azure | GCP |
|-----|-------|-----|
| VPC Peering | VNet Peering | VPC Peering |
| Transit Gateway | Virtual WAN / Hub VNet | Cloud Router + VPN/Interconnect |

**Kluczowe cechy:**

| Cecha | Wartość |
|-------|---------|
| Transitive routing | ❌ Brak (fundamentalne ograniczenie!) |
| Cross-account | ✅ Tak |
| Cross-region | ✅ Tak (Inter-Region VPC Peering) |
| Overlapping CIDR | ❌ Niedozwolone |
| Szyfrowanie | ✅ Ruch szyfrowany (jak cały ruch w AWS backbone) |
| Przepustowość | Brak sztywnego limitu — ograniczony przez EC2 bandwidth |
| Koszt | Data transfer charges (cross-AZ, cross-region) |

**Non-transitive routing — kluczowe ograniczenie:**
```mermaid
graph LR
    A["VPC A\n10.0.0.0/16"] <-->|Peering| B["VPC B\n10.1.0.0/16"]
    B <-->|Peering| C["VPC C\n10.2.0.0/16"]
    A -.->|❌ NIE DZIAŁA| C
    Note["A nie może rozmawiać z C\nprzez B (non-transitive)\nPotrzeba osobnego Peering A-C\nlub Transit Gateway"]
```

**Wymagane kroki do VPC Peering:**
1. Stwórz Peering Connection (requester VPC → accepter VPC)
2. Accepter **akceptuje** (manualnie lub automatycznie jeśli same konto)
3. Zaktualizuj **Route Tables po obu stronach** (bez tego ruch nie dotrze!)
4. Zaktualizuj **Security Groups** (dodaj CIDR lub SG reference drugiego VPC)
5. Opcjonalnie: zaktualizuj **NACL** (jeśli masz niestandardowe)

**Route Tables dla Peering:**
```
VPC A Route Table:
  10.0.0.0/16  → local
  10.1.0.0/16  → pcx-xxxxxx  ← route do VPC B przez Peering

VPC B Route Table:
  10.1.0.0/16  → local
  10.0.0.0/16  → pcx-xxxxxx  ← route do VPC A przez Peering
```

> ⚠️ **Egzamin:** Bez aktualizacji Route Tables — Peering istnieje ale ruch NIE płynie. To najczęstszy błąd konfiguracji.

**VPC Peering vs Transit Gateway — kiedy co:**

| Kryterium | VPC Peering | Transit Gateway |
|-----------|-------------|-----------------|
| Liczba połączeń | N*(N-1)/2 (mesh) | N połączeń do TGW |
| Koszt przy <10 VPC | Tańszy | Droższy ($0.05/h per attachment) |
| Koszt przy >10 VPC | Eksploduje (mesh) | TGW tańszy |
| Transitive routing | ❌ | ✅ |
| Cross-account/region | ✅ | ✅ |
| Centralized routing policy | ❌ | ✅ |
| Shared services VPC | Możliwe z mesh | Prostsze przez TGW |

**VPC Peering — bezpieczeństwo:**
- Szyfrowanie w tranzycie: ✅ AWS szyfruje ruch w backbone
- Izolacja: każde Peering connection jest oddzielne, non-transitive
- IAM kontrola: kto może tworzyć/akceptować peering
- Security Groups: nie otwierają się automatycznie → wymaga ręcznej konfiguracji
- Flow Logs: logują ruch przez Peering (tagowany źródłem i celem)

**VPC Peering cross-region:**
- Ruch przechodzi przez AWS global backbone (nie Internet)
- Wyższe latency niż intra-region
- Data transfer costs: cross-region peering = $0.02/GB (zależy od par regionów)
- Obsługuje IPv4 i IPv6

**Zasięg:** Globalny (cross-region), ale connection jest resource w konkretnym regionie

**Limity:**
- 125 active VPC Peering connections per VPC (domyślnie)
- Można zwiększyć przez Service Quotas

---

### 8.6 Elastic IP (EIP)

**🟢 Co to jest?**
Elastic IP to **statyczny publiczny adres IPv4**, który możesz przypisać do zasobu AWS i zatrzymać nawet gdy zasób jest wyłączony. IP należy do Twojego konta, nie do instancji.

> **Analogia:** Jak wynajęcie stałego numeru telefonu — numer jest Twój niezależnie od tego, który aparat (instancja) go używa.

**Odpowiedniki:**

| AWS | Azure | GCP | On-prem |
|-----|-------|-----|---------|
| Elastic IP | Static Public IP / Public IP (Standard SKU) | Static External IP | Stały adres publiczny od ISP |

**Kluczowe cechy:**

| Cecha | Wartość |
|-------|---------|
| Protokół | IPv4 only (brak IPv6 EIP) |
| Regionalny | ✅ (per Region) |
| Przenoszalny | ✅ Między EC2, NAT GW, Network Interface |
| Limit | 5 per Region (soft limit, zwiększalny) |
| BYOIP | ✅ Możesz przynieść własne bloki IP |

**Koszty EIP — ważne na egzamin:**

| Stan | Koszt |
|------|-------|
| EIP przypisany do działającej instancji | **$0.00** |
| EIP przypisany do zatrzymanej instancji | **$0.005/h** |
| EIP **nieprzypisany** (idle) | **$0.005/h** |
| Dodatkowe EIP na jednej instancji | $0.005/h za każdy |

> ⚠️ **Egzamin:** Płacisz za EIP gdy **NIE jest używany** — idle EIP = marnotrawstwo. Zwolnij nieużywane EIP. AWS karze za "marnowanie" publicznych IP.

**Typowe zastosowania:**

| Użycie | Opis |
|--------|------|
| **Failover** | Przenieś EIP z uszkodzonej instancji na standby w kilka sekund |
| **NAT Gateway** | Każdy public NAT GW wymaga EIP |
| **EC2 z whitelisted IP** | Firewall zewnętrzny akceptuje tylko konkretne IP — EIP daje stabilność |
| **Bastion Host** | Stały IP do SSH |
| **NLB** | Network Load Balancer może mieć EIP per AZ |

**EIP a IGW:**
```
EC2 private IP: 10.0.1.50
EC2 public IP (EIP): 52.10.20.30

Gdy pakiet wychodzi:
  Źródło: 10.0.1.50 → IGW → transluje na 52.10.20.30

Gdy pakiet wraca do 52.10.20.30:
  IGW transluje 52.10.20.30 → 10.0.1.50 → EC2

IGW robi NAT 1:1 dla EIP (nie NAT 1:many jak NAT Gateway)
```

**BYOIP (Bring Your Own IP):**
- Możesz przynieść własny zakres IP (min /24 dla IPv4, /48 dla IPv6)
- Wymagany ROA (Route Origin Authorization) przez Regional Internet Registry
- Po importacji używasz jak regularny EIP, ale IP należy do Twojej organizacji
- Przydatne: firma ma swoje IP w allowlistach klientów — zachowujesz stary zakres przy migracji do AWS

**EIP a bezpieczeństwo:**
- EIP nie filtruje ruchu — filtruje Security Group i NACL
- DDOS protection: AWS Shield Basic chroni EIP automatycznie (gratis)
- Nie przypisuj EIP bezpośrednio do prywatnych EC2 — używaj Load Balancer + prywatne EC2

**Zasięg:** Regionalny (nie możesz przenieść EIP między regionami)

---

### 8.7 NAT Gateway — szczegóły, koszty, security context

**🔵 Co to jest?**
NAT Gateway to zarządzana usługa AWS umożliwiająca instancjom w **prywatnych subnetach** inicjowanie **wychodzących** połączeń do Internetu lub innych usług, blokując jednocześnie **przychodzące** połączenia z zewnątrz.

> **Analogia on-prem:** Router z PAT (Port Address Translation) / NAT overload — wiele wewnętrznych IP wychodzi pod jednym publicznym.

**Odpowiedniki:**

| AWS | Azure | GCP | Linux |
|-----|-------|-----|-------|
| Public NAT Gateway | NAT Gateway (VNet level) | Cloud NAT | `iptables -t nat MASQUERADE` / `nftables` |
| Private NAT Gateway | Brak bezpośredniego odpowiednika | Brak | Routing między sieci bez NATu |

**Typy NAT Gateway — szczegółowe porównanie:**

| Cecha | Public NAT Gateway | Private NAT Gateway |
|-------|-------------------|---------------------|
| Elastic IP | ✅ Wymagany | ❌ Brak |
| Internet access | ✅ Tak | ❌ Nie |
| Deployment | Public subnet | Dowolny subnet (też private) |
| Use case | EC2 → Internet | EC2 w VPC → inne VPC przez TGW (z overlapping CIDR) |
| Docelowy ruch | Internet (0.0.0.0/0) | Inne VPC/on-prem przez TGW |
| Cena | $0.045/h + $0.045/GB | $0.045/h + $0.045/GB |

**Public NAT Gateway — przepływ:**
```mermaid
graph LR
    EC2["EC2\n10.0.2.50\n(private subnet)"] 
    NATGW["NAT GW\nPrivate IP: 10.0.1.100\nEIP: 52.10.20.30"]
    IGW[Internet Gateway]
    INET((Internet\ngoogle.com))

    EC2 -->|"src: 10.0.2.50:12345\ndst: 8.8.8.8:443"| NATGW
    NATGW -->|"src: 52.10.20.30:PORT\ndst: 8.8.8.8:443"| IGW
    IGW --> INET
```

**NAT Gateway — koszty (szczegóły):**

| Komponent | Cena (us-east-1) |
|-----------|-----------------|
| Godzina działania | $0.045/h (≈ $32/mc) |
| Data processing | $0.045 per GB przetworzonych |

> **Optymalizacja kosztów:** Zamiast NAT GW dla dostępu do S3/DynamoDB → użyj **VPC Gateway Endpoint** (darmowy). Dla innych serwisów AWS → **Interface Endpoint** ($0.01/h + data, ale często tańszy niż NAT + data).

**High Availability NAT Gateway:**
```
Best practice: 1 NAT GW per AZ
  
  AZ1: Private Subnet → Route Table → NAT GW AZ1 → IGW
  AZ2: Private Subnet → Route Table → NAT GW AZ2 → IGW
  
  Jeśli AZ1 fail → EC2 w AZ2 nadal ma dostęp przez własny NAT GW
  Jeśli 1 NAT GW dla obu AZ: AZ1 fail → EC2 AZ2 ruch przez cross-AZ → 
  wyższe koszty + brak HA
```

**NAT Gateway w kontekście Security Group:**
> ⚠️ NAT Gateway **NIE posiada Security Group!** To ważna różnica vs NAT Instance.

- Nie możesz filtrować ruchu przez NAT GW na poziomie SG
- Filtrowanie przez: Security Group **instancji źródłowej** + NACL subnetu

**NAT Gateway skalowanie:**
- Start: 5 Gbps
- Auto-scale do: 45 Gbps (per NAT GW)
- Powyżej 45 Gbps: rozstaw ruch na wiele NAT GW (przez różne subnety lub ECMP)

**NAT Gateway vs NAT Instance:**

| Cecha | NAT Gateway | NAT Instance (EC2) |
|-------|-------------|-------------------|
| Zarządzanie | Fully managed | Ty (patching, monitoring) |
| HA | Multi-AZ (1 per AZ best practice) | Single instance SPOF (lub HA z własnym skryptem) |
| Przepustowość | 5-45 Gbps auto-scale | Zależy od EC2 type (max ~25 Gbps dla r5n) |
| Security Group | ❌ | ✅ |
| Port forwarding | ❌ | ✅ (iptables DNAT) |
| Bastion host | ❌ | ✅ (dual role) |
| Koszt (małe) | Wyższy | Niższy (małe t3.nano) |
| Koszt (duże) | Niższy (no EC2 mgmt) | Wyższy |
| Source/Dest Check | N/A (managed) | **Musi być wyłączony!** |
| Use case | Produkcja, zarządzana | Legacy, lab, port forwarding |

**Zasięg:** Zonal — jeden NAT GW jest w jednym AZ! (Nie myć z "wysoką dostępnością" — HA przez duplikację per AZ)

---

### 8.8 Private NAT Gateway

**🟡 Co to jest?**
Private NAT Gateway to NAT Gateway **bez Elastic IP**, wdrożony w **dowolnym subnecie** (zazwyczaj prywatnym). Służy do translacji adresów **między prywatnymi sieciami** — nie daje dostępu do Internetu.

**Kiedy używać Private NAT Gateway:**
```
Scenariusz 1 — Overlapping CIDR między VPC przez Transit Gateway:
  VPC A: 10.0.0.0/16
  VPC B: 10.0.0.0/16  (taki sam CIDR!)
  
  Normalnie VPC Peering niemożliwy (overlapping CIDR)
  
  Rozwiązanie:
  VPC A → Private NAT GW → TGW → VPC B
  EC2 w VPC A (10.0.1.50) → SNAT na prywatny IP NAT GW → TGW → VPC B
  VPC B widzi ruch jako przychodzący z IP NAT GW (unikalny) nie z 10.0.0.0/16
```

```mermaid
graph LR
    subgraph VPC_A["VPC A: 10.0.0.0/16"]
        EC2A["EC2\n10.0.1.50"]
        PNATGW["Private NAT GW\n100.64.0.1"]
    end
    TGW[Transit Gateway]
    subgraph VPC_B["VPC B: 10.0.0.0/16 (overlapping!)"]
        EC2B["EC2\n10.0.1.100"]
    end

    EC2A -->|src: 10.0.1.50| PNATGW
    PNATGW -->|src: 100.64.0.1 → SNAT| TGW
    TGW --> EC2B
```

**Scenariusz 2 — Centralized egress przez hub VPC:**
```
Spoke VPC (private EC2) → TGW → Hub VPC → Private NAT GW → Public NAT GW → Internet

Korzyść: wszystkie Spoke VPC wychodzą przez jeden zestaw EIP (centralized NAT)
Kontrola: Hub VPC ma centralized firewalling (AWS Network Firewall)
```

**Kluczowa różnica Public vs Private NAT GW:**

| | Public NAT GW | Private NAT GW |
|-|--------------|----------------|
| EIP | ✅ Wymagany | ❌ Brak |
| Internet | ✅ Tak | ❌ Nie |
| Placement | **Public subnet** | Dowolny subnet |
| Cel | Prywatne EC2 → Internet | Ruch między prywatnymi VPC (overlapping CIDR) |
| Routing | → IGW | → TGW lub inne prywatne destination |

**Zasięg:** Zonal (jak wszystkie NAT Gateways)

---

### 8.9 Transit VPC (legacy) i Transit Gateway

**🔴 Transit VPC (Legacy):**
Stary wzorzec (przed Transit Gateway, 2018+) gdzie dedykowane **EC2 z VPN software** (np. Cisco, Palo Alto) działały jako centralny hub routingu między VPC.

```mermaid
graph TB
    subgraph HUB["Hub VPC (Transit VPC)"]
        EC2_ROUTER["EC2 router\n(Cisco CSR 1000v)\nz VPN software"]
    end
    EC2_ROUTER <-->|VPN Tunnel| VPC1[Spoke VPC 1]
    EC2_ROUTER <-->|VPN Tunnel| VPC2[Spoke VPC 2]
    EC2_ROUTER <-->|VPN Tunnel| ONPREM[On-premises]
```

**Problemy Transit VPC (dlaczego zastąpiony przez TGW):**
- ❌ Zarządzanie EC2 (patching, HA, monitoring)
- ❌ Przepustowość ograniczona przez EC2 bandwidth
- ❌ Skalowanie trudne
- ❌ Brak support od AWS (rozwiązanie partner/custom)
- ❌ Wyższy koszt (EC2 + VPN licenses)
- ❌ Single point of failure bez własnej HA architektury

**🟢 Transit Gateway (TGW):**
Managed hub dla połączeń VPC, VPN i Direct Connect. Zastępuje Transit VPC i mesh VPC Peering.

```mermaid
graph TB
    TGW["AWS Transit Gateway\n(managed hub)"]
    TGW <-->|TGW Attachment| VPC1["VPC 1\nDev"]
    TGW <-->|TGW Attachment| VPC2["VPC 2\nProd"]
    TGW <-->|TGW Attachment| VPC3["VPC 3\nShared Services"]
    TGW <-->|VPN Attachment| ONPREM[On-premises\nVPN]
    TGW <-->|DX Attachment| DX[Direct Connect\nGateway]
    TGW <-->|Peering Attachment| TGW2["TGW w innym\nRegionie"]
```

**TGW — kluczowe cechy:**

| Cecha | Wartość |
|-------|---------|
| Max VPC attachments | 5000 per TGW |
| Routing | ✅ Transitive (pełny hub-and-spoke) |
| Cross-account | ✅ Przez AWS RAM |
| Cross-region | ✅ TGW Peering |
| Route Tables | ✅ Wiele (segmentacja) |
| Multicast | ✅ (opcjonalny) |
| Bandwidth per VPC attachment | Max 50 Gbps |
| Cena | $0.05/h per attachment + $0.02/GB |

**TGW Route Tables — segmentacja:**
```
TGW Route Table "Production":
  Attachment: VPC-Prod, VPC-Shared-Services
  Route: 10.1.0.0/16 → VPC-Prod attachment
  Route: 10.0.0.0/16 → VPC-Shared-Services attachment

TGW Route Table "Development":
  Attachment: VPC-Dev
  Route: 10.2.0.0/16 → VPC-Dev attachment
  Route: 10.0.0.0/16 → VPC-Shared-Services attachment
  
→ Dev nie może komunikować się z Prod (różne Route Tables)
→ Oba mogą korzystać z Shared Services
```

**TGW vs VPC Peering — kiedy co:**

| Kryterium | VPC Peering | Transit Gateway |
|-----------|-------------|-----------------|
| Do 10 VPC | ✅ Tańszy | ❌ Droższy |
| Powyżej 10 VPC | ❌ Mesh = O(n²) | ✅ TGW skaluje liniowo |
| Transitive routing | ❌ | ✅ |
| Centralized security | ❌ | ✅ |
| Cross-account | ✅ Tak | ✅ Tak (przez RAM) |
| Latency | Niższy | Nieco wyższy |
| Overlapping CIDR | ❌ Niemożliwy | ✅ Możliwy przez Private NAT GW |

**Azure odpowiednik TGW:** Azure Virtual WAN (vWAN) — managed hub-and-spoke connectivity. Podobna koncepcja: centralny hub, spoke VNets, on-prem connections.

**TGW a bezpieczeństwo:**
- TGW sam nie filtruje ruchu (nie jest firewallem)
- Dla centralized inspection: AWS Network Firewall w hub VPC + TGW routing przez firewall
- Flow Logs: TGW obsługuje Flow Logs
- CloudTrail loguje zmiany konfiguracji TGW

**Zasięg:** Regionalny (ale TGW Peering cross-region)

---

### 8.10 EC2 jako router — jak EC2 routuje ruch

**🟠 Co to jest?**
EC2 instancja może działać jako **software router** gdy:
1. **Source/Destination Check jest wyłączony** (kluczowy warunek!)
2. Skonfigurowano routing software (Linux IP routing, VPN software, NGFW)
3. Route Tables VPC kierują ruch przez EC2

**Source/Destination Check:**
```
Domyślnie: AWS sprawdza czy pakiet ma jako SRC lub DST adres IP tej instancji
           Jeśli nie → pakiet odrzucony (EC2 nie jest routerem)

Dla NAT/Router/VPN:
           Musi być WYŁĄCZONY → EC2 akceptuje i forwarduje cudze pakiety
```

```bash
# Wyłącz Source/Destination Check przez AWS CLI
aws ec2 modify-instance-attribute \
  --instance-id i-xxxxxxxxxxxx \
  --no-source-dest-check

# Lub przez EC2 Console: Actions → Networking → Change Source/Dest. Check
```

**Dlaczego EC2 jako router (use cases):**

| Przypadek użycia | Opis |
|-----------------|------|
| **NAT Instance** | Legacy — zanim NAT Gateway istniał. EC2 z `iptables MASQUERADE` |
| **VPN Concentrator** | Cisco CSR 1000v, Juniper vSRX, Palo Alto VM-Series |
| **Network Firewall / NGFW** | Check Point CloudGuard, Fortinet FortiGate, Palo Alto |
| **SD-WAN** | Cisco Viptela, VMware VeloCloud jako EC2 |
| **Transit VPC** | Hub router (legacy, zastąpiony przez TGW) |
| **Packet inspection** | IDS/IPS na EC2 (Snort/Suricata) |

**Linux routing na EC2:**
```bash
# Włącz IP forwarding (kernel)
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p

# NAT masquerade (PAT):
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Statyczna trasa forwarding:
ip route add 10.1.0.0/16 via 10.0.1.1
```

**EC2 router w VPC — konfiguracja Route Table:**
```
Private Subnet Route Table:
  10.0.0.0/16  → local
  0.0.0.0/0    → i-xxxxxxxx (EC2 NAT instance!)  ← target jest ID instancji!
  
Ważne: cel w Route Table może być:
  igw-xxx     (Internet Gateway)
  nat-xxx     (NAT Gateway)
  i-xxx       (EC2 instance — NAT instance/router)
  pcx-xxx     (VPC Peering)
  tgw-xxx     (Transit Gateway)
  vgw-xxx     (Virtual Private Gateway)
  eigw-xxx    (Egress-Only Internet Gateway — IPv6)
  vpce-xxx    (VPC Endpoint — Gateway type)
```

**Multi-ENI EC2 Router:**
```mermaid
graph LR
    subgraph VPC
        subgraph Public_Subnet
            EC2_ROUTER["EC2 Router\nENI1: 10.0.1.10 (eth0) - public\nENI2: 10.0.2.10 (eth1) - private\nSource/Dest Check: OFF"]
        end
        subgraph Private_Subnet
            EC2_PRIV["EC2 Private\n10.0.2.50"]
        end
    end
    EC2_PRIV -->|route: 0.0.0.0/0 → i-router| EC2_ROUTER
    EC2_ROUTER -->|NAT/routing| IGW[IGW] --> INET((Internet))
```

**Kiedy używać EC2 jako router vs NAT GW/TGW:**

| Kiedy EC2 | Kiedy NAT GW / TGW |
|-----------|-------------------|
| Port forwarding (DNAT) | Standardowy NAT outbound |
| Deep packet inspection | Proste routing |
| VPN termination (IKEv2, IPSec) | AWS-native managed VPN |
| 3rd party NGFW (firewall features) | Prosty egress |
| SD-WAN edge | Skalowalny hub-and-spoke |
| Laboratorium/POC | Produkcja |

**Zasięg:** EC2 jest zonal (AZ-specific)

---

### 8.11 ARM — Resource Access Manager, sharing VPC resources

**🟣 Co to jest ARM?**
**AWS Resource Access Manager (RAM)** to serwis umożliwiający **udostępnianie zasobów AWS** między kontami w obrębie **AWS Organizations** lub z wybranymi kontami/OU.

> **Uwaga:** RAM (Resource Access Manager) ≠ ARM (architektura procesorów). Tu mówimy o RAM = AWS Resource Access Manager.

> **Azure odpowiednik:** Azure Lighthouse (dla cross-tenant management), Azure RBAC cross-subscription. Brak bezpośredniego 1:1 — Azure Shared Resources w Hub VNet jest bliżej architekturalnie.

**Zasoby które możesz udostępniać przez RAM:**

| Zasób | Opis |
|-------|------|
| **VPC Subnets** | ⭐ Najważniejsze dla egzaminu! Udostępnianie subnetów między kontami |
| Transit Gateway | TGW w jednym koncie, attachments z innych kont |
| Route 53 Resolver Rules | Centralne reguły DNS dla całej organizacji |
| License Manager Configurations | Zarządzanie licencjami |
| AWS Network Firewall Policies | Centralne polityki firewall |
| AWS Resource Groups | Grupowanie zasobów cross-account |
| Capacity Reservations | Rezerwacja EC2 capacity |
| Prefix Lists | Listy CIDR dla Security Groups |

**VPC Subnet Sharing — kluczowy scenariusz egzaminacyjny:**

```mermaid
graph TB
    subgraph ACC_NET["Konto Network (centralne)"]
        VPC["VPC: 10.0.0.0/16"]
        SUB1["Subnet A: 10.0.1.0/24\n(udostępniony do Account-Dev)"]
        SUB2["Subnet B: 10.0.2.0/24\n(udostępniony do Account-Prod)"]
        SUB3["Subnet C: 10.0.3.0/24\n(tylko dla Account-Network)"]
        VPC --> SUB1
        VPC --> SUB2
        VPC --> SUB3
    end
    
    subgraph ACC_DEV["Konto Dev"]
        EC2_DEV["EC2 dev\ndeploy do Subnet A"]
    end
    
    subgraph ACC_PROD["Konto Prod"]
        EC2_PROD["EC2 prod\ndeploy do Subnet B"]
    end
    
    SUB1 -->|RAM share| EC2_DEV
    SUB2 -->|RAM share| EC2_PROD
```

**Jak działa VPC Subnet Sharing:**
1. Konto Network tworzy VPC, subnety, Route Tables, NAT GW, IGW
2. RAM udostępnia subnety innym kontom (w Organizations lub konkretnym account ID)
3. Konta "uczestniczące" (participant) mogą deploywać EC2, RDS, ECS do tych subnetów
4. Zasoby w tych subnetach widoczne przez Network Account jako "shared" resources

**Co może/nie może robić konto uczestniczące:**

| Może | Nie może |
|------|---------|
| Deploy EC2, RDS, ECS/EKS do shared subnetu | Modyfikować VPC/subnet/Route Table/NACL |
| Tworzyć Security Groups **w swoim koncie** dla zasobów w shared subnecie | Zarządzać Security Groups z Network konta |
| Widzieć własne zasoby w shared subnecie | Widzieć zasoby innych kont w tym samym subnecie |
| Tworzyć Interface VPC Endpoints | Modyfikować główne zasoby sieciowe |

**Korzyści VPC Subnet Sharing vs VPC Peering:**

| Aspekt | VPC Subnet Sharing | VPC Peering |
|--------|-------------------|-------------|
| VPC count | Jedno wspólne VPC | Wiele VPC |
| Routing | Automatyczny (local) | Wymaga Route Tables |
| Security Groups | Cross-account reference możliwy | Cross-account SG reference możliwy |
| Izolacja kont | ✅ (konty widzą tylko swoje zasoby) | ✅ |
| Centralizacja sieci | ✅ Sieć w jednym koncie | ❌ Sieć per konto |
| Compliance | Jeden VPC = łatwiejszy audyt | Wiele VPC = więcej pracy |
| Koszt | Brak opłat za transfer w obrębie VPC | Data transfer charges cross-AZ |

**Zasięg:** RAM jest usługą globalną (działa cross-region dla niektórych zasobów, ale VPC subnet sharing jest regionalny)

**RAM a Organizations:**
```bash
# Włącz sharing z Organizations (exec w management account)
aws ram enable-sharing-with-aws-organization

# Utwórz resource share
aws ram create-resource-share \
  --name "network-subnets-share" \
  --resource-arns "arn:aws:ec2:eu-west-1:111122223333:subnet/subnet-xxx" \
  --principals "arn:aws:organizations::111122223333:ou/o-xxx/ou-xxx-yyy"
```

---

### 8.12 Well-Architected Framework

| Filar | DHCP Options | NACL/SG | VPC Peering | EIP | NAT GW | Private NAT | TGW | EC2 Router | RAM/Subnet Share |
|-------|-------------|---------|-------------|-----|--------|-------------|-----|------------|-----------------|
| **Operational Excellence** | Automatyczna konfiguracja DNS/NTP przez DHCP eliminuje ręczną konfigurację | SG + NACL jako code (Terraform/CDK) — IaC zarządzanie | Peering connections dokumentowane w diagram | EIP inventory — monitoruj idle EIP w Cost Explorer | Managed — brak operacyjnego overhead | Centralized NAT pattern — prostsze zarządzanie | Centralized routing policy — jeden punkt konfiguracji | Wymaga manual patching, monitoring OS | Centralne VPC w Network Account — jeden team zarządza siecią |
| **Security** | Kieruj instancje do własnego DNS (AD) przez DHCP — zapobiega DNS hijacking | Defense in depth: NACL (subnet) + SG (ENI). NACL = Deny dla blokowania IP atakujących | Peering nie jest transitive — naturalna izolacja. Security Groups cross-VPC | EIP = stały punkt ataku — minimalizuj ekspozycję. Shield Basic automatyczny | NAT GW blokuje inbound z Internetu — jedynie egress | Prywatna komunikacja bez Internetu — zwiększa izolację | Segmentacja przez TGW Route Tables (prod vs dev separation) | Source/Dest Check OFF = ryzyko if misconfigured. Patch EC2 router | Izolacja kont — uczestnik widzi tylko swoje zasoby w shared subnecie |
| **Reliability** | DHCP Option Set zmiana propaguje się po wygaśnięciu lease — plan change mgmt | NACL bez stanowości — incydentalna modyfikacja może zerwać ruch. Test przed wdrożeniem | Non-transitive = no blast radius dla awarii. Cross-region Peering przez AWS backbone | EIP failover pattern: przenieś EIP z uszkodzonej instancji na standby | 1 NAT GW per AZ eliminuje cross-AZ SPOF | Private NAT + TGW: odporność na awarie pojedynczego VPC | TGW HA przez AWS — multi-AZ managed. VPC attachments odporne | Single EC2 router = SPOF. HA: Auto Scaling Group + Route Table Lambda failover | Network Account centralne — ich stabilność krytyczna dla wszystkich kont |
| **Performance Efficiency** | Własny NTP przez DHCP = zsynchronizowany czas = prawidłowe timestampy | SG union ocena — unikaj zbyt wielu SG per ENI (max 16) | Intra-region Peering: niskie latency. Cross-region: AWS backbone ale wyższe niż intra | EIP eliminuje DNS TTL czekanie przy failover (natychmiastowe) | 5-45 Gbps auto-scale. VPC Gateway Endpoint zamiast NAT dla S3/DynamoDB (szybciej, taniej) | Prywatny NAT: eliminuje roundtrip przez Internet dla VPC-to-VPC | 50 Gbps per attachment. Bandwidth agregacja przez TGW | EC2 bandwidth ograniczony przez instance type | Brak cross-VPC data transfer kosztów — zasoby w jednym VPC/subnet |
| **Cost Optimization** | Brak dodatkowych opłat za DHCP Option Set | NACL i SG bezpłatne (wbudowane w VPC) | Koszt: data transfer. Cross-AZ = $0.01/GB. Cross-region = $0.02/GB per GB | Idle EIP = $0.005/h. Monitoruj i zwalniaj nieużywane. EIP attached to running = free | Głowna pozycja kosztowa VPC. S3/DynamoDB → Gateway Endpoint (free). NAT GW per AZ vs koszt cross-AZ | Private NAT GW koszt = Public NAT GW koszt — rozważ czy potrzebny | $0.05/h per attachment. Alternatywa dla <10 VPC: VPC Peering tańszy | EC2 koszt + oprogramowanie routera (licencje Cisco/Palo Alto drogie!) | Jeden VPC = jeden NAT GW, jeden IGW — dzielony między konta = tańsze |
| **Sustainability** | Brak wpływu | Brak wpływu | Ruch przez AWS backbone nie przez Internet = niższe carbon footprint niż przez ISP | Minimalizuj idle EIP = mniej zmarnowanych zasobów | Managed service = zoptymalizowane przez AWS pod kątem energii | Eliminuje Internet roundtrip = mniejsze zużycie energii trasy | AWS managed = energooszczędny hardware vs EC2 router fleet | EC2 router = zużycie compute nawet przy minimalnym ruchu | Jeden VPC = mniej duplikowanej infrastruktury = mniej zasobów |

---

### 8.13 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Firma ma 50 kont AWS w Organizations. Centralny team sieciowy chce zarządzać całą infrastrukturą sieciową z jednego miejsca. Każdy team aplikacyjny ma własne konto. Jak zaprojektować sieć?

**Rozwiązanie — Network Account Pattern:**
```
Management Account (Organizations):
  └── Network Account:
        - 1 centralne VPC per region (10.0.0.0/8)
        - Subnety per team przez RAM
        - Transit Gateway (cross-account przez RAM)
        - Route 53 Resolver (centralne DNS)
        - AWS Network Firewall (centralized inspection)
        
  └── Dev Account:
        - Używa shared subnet z Network Account
        - Deployuje EC2/ECS do shared subnetu
        - Własny VPC opcjonalnie dla specjalnych potrzeb
        
  └── Prod Account:
        - Osobne shared subnety (izolacja od Dev)
        - TGW attachment przez RAM
```

**DHCP Option Set:** Network Account → DHCP Option Set wskazuje na Route 53 Resolver w Network Account → wszystkie instancje (we wszystkich kontach) używają centralnego DNS.

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Nowa aplikacja 3-tier w jednym koncie. Wymaga: izolacji warstw, blokowania konkretnych IP, dostępu do S3 bez Internetu, HA NAT Gateway.

**Rozwiązanie:**
```
VPC: 10.0.0.0/16
  
  Public Subnets (ALB + NAT GW per AZ):
    AZ1: 10.0.1.0/24  → rt-public (0.0.0.0/0 → igw)
    AZ2: 10.0.2.0/24  → rt-public
    
  Private Subnets (App):
    AZ1: 10.0.11.0/24 → rt-private-az1 (0.0.0.0/0 → nat-gw-az1)
    AZ2: 10.0.12.0/24 → rt-private-az2 (0.0.0.0/0 → nat-gw-az2)
    
  Data Subnets (RDS):
    AZ1: 10.0.21.0/24 → rt-data (brak 0.0.0.0/0)
    AZ2: 10.0.22.0/24 → rt-data

Security:
  sg-alb → Allow 443 from 0.0.0.0/0
  sg-app → Allow 8080 from sg-alb
  sg-rds → Allow 5432 from sg-app
  
  NACL Data Subnet:
    Rule 100: Allow TCP 5432 from 10.0.11.0/24 (private subnet AZ1) INBOUND
    Rule 110: Allow TCP 5432 from 10.0.12.0/24 (private subnet AZ2) INBOUND
    Rule 200: Allow TCP 1024-65535 to 10.0.11.0/24 OUTBOUND (ephemeral)
    Rule 210: Allow TCP 1024-65535 to 10.0.12.0/24 OUTBOUND (ephemeral)
    Rule 900: Deny all (explicit)
    
S3 Access:
  VPC Gateway Endpoint dla S3 → dodaje route w rt-private-az1 i rt-private-az2
  → EC2 komunikuje się z S3 bez NAT GW, bez Internetu, bezpłatnie
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma ma 20 VPC połączonych mesh Peeringiem. Koszty management i data transfer rosną. VPC dodawane raz na miesiąc.

**Rozwiązanie — migracja do Transit Gateway:**
```
Faza 1: Deploy Transit Gateway
  - TGW w koncie Network (lub centralne konto)
  - Udostępnij przez RAM do wszystkich kont

Faza 2: TGW attachments
  - Każdy VPC dostaje TGW attachment
  - TGW Route Table: trasy do wszystkich VPC

Faza 3: Aktualizacja Route Tables w VPC
  - Stare: 10.x.0.0/16 → pcx-xxx (Peering)
  - Nowe: 10.x.0.0/16 → tgw-xxx (Transit Gateway)
  - Lub: 0.0.0.0/0 → tgw-xxx (all traffic through TGW) + specific routes

Faza 4: Usuń stare Peering connections
  - Monitoruj Flow Logs — brak ruchu przez stare Peering → bezpieczne usunięcie

Korzyść: O(N²) Peering connections → O(N) TGW attachments
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Duża firma migruje 500 serwerów z on-prem. Ma istniejący AD (dla DNS), własny zakres IP 10.10.0.0/16 (on-prem). Potrzeba szybkiej, bezpiecznej migracji z minimalną zmianą konfiguracji serwerów.

**Rozwiązanie:**
```
1. Sieć:
   - VPC: 10.20.0.0/16 (inna pula niż on-prem 10.10.0.0/16)
   - Direct Connect lub VPN do on-prem
   - TGW: VPC + DX attachment

2. DNS:
   - DHCP Option Set: domain-name-servers = DC IPs (on-prem AD)
   - Route 53 Resolver Outbound Rules: *.corp.local → on-prem DNS
   - Serwery w AWS automatycznie rozwiązują on-prem nazwy

3. IP management:
   - Jeśli serwery mają hardkodowane IP → EIP Mapping (EIP = on-prem IP mapping)
   - Lub: DHCP reservation → IP per hostname consistency

4. Security:
   - Migration phase: NACL relaxed (allow all from on-prem CIDR)
   - Security Groups: Allow from 10.10.0.0/16 (on-prem) dla migrowanych serwisów
   - Post-migration: zaostrzenie NACL i SG

5. NAT Gateway:
   - Public NAT GW per AZ → EC2 dostają dostęp do Windows Update, vendor repos
   - VPC Endpoints dla S3 (backupy, AMI storage)
```

---

## 9. Virtual Private Gateway, Transit VPC, VPC Endpoints, AWS PrivateLink, Interface Endpoints, Gateway Endpoints

### 9.1 Virtual Private Gateway (VGW)

**🔵 Co to jest?**
**Virtual Private Gateway (VGW)** to brama VPN po stronie AWS — "koncentrator VPN" zarządzany przez AWS, przyłączony do VPC. Umożliwia połączenie VPC z on-premises przez **Site-to-Site VPN** lub **Direct Connect**.

> **Analogia:** VGW to router brzegowy Twojego VPC — ten sam, który mówi "ruch do sieci firmowej idzie tędy".

**Odpowiedniki:**

| AWS | Azure | GCP | On-prem |
|-----|-------|-----|---------|
| Virtual Private Gateway | VPN Gateway / ExpressRoute Gateway | Cloud VPN Gateway / Interconnect | Router brzegowy z IPSec |
| Site-to-Site VPN + VGW | VPN Connection | Cloud VPN | IPSec tunnel |
| Direct Connect + VGW | ExpressRoute | Cloud Interconnect | MPLS / leased line |

**Architektura VGW:**

```mermaid
graph LR
    subgraph ON_PREM["On-premises"]
        CGW["Customer Gateway\n(router/firewall Cisco, pfSense...)"]
    end
    
    subgraph AWS_VPC["VPC (AWS)"]
        VGW["Virtual Private Gateway\n(AWS-managed VPN termination)"]
        SUBNET["Private Subnet\n10.0.1.0/24"]
        EC2["EC2 Instance"]
    end
    
    CGW <-->|"IPSec VPN Tunnel\n(2 tunele dla HA)"| VGW
    VGW --> SUBNET --> EC2
```

**Kluczowe cechy VGW:**

| Cecha | Wartość |
|-------|---------|
| Przyłączenie | 1 VGW per VPC |
| VPN tunnele | 2 per connection (HA automatycznie) |
| Przepustowość VPN | Do 1.25 Gbps per tunnel (łącznie do ~2.5 Gbps z ECMP) |
| Protokół | IKEv1 i IKEv2 |
| Routing VPN | Static lub BGP (dynamic) |
| Direct Connect | Tak — VGW jako attachment point |
| Multi-region | ❌ (per region — użyj TGW dla multi-region) |

**Route propagation — ważne:**
```
Static routing: dodajesz ręcznie trasy w Route Table
  Route Table: 192.168.0.0/16 → vgw-xxx

Dynamic routing (BGP): włącz "Route Propagation" w Route Table
  VGW automatycznie wstrzykuje trasy z BGP do Route Table
  
aws ec2 enable-vgw-route-propagation \
  --route-table-id rtb-xxx \
  --gateway-id vgw-xxx
```

**Site-to-Site VPN przez VGW:**
```mermaid
sequenceDiagram
    participant CGW as Customer Gateway (on-prem)
    participant VGW as Virtual Private Gateway (AWS)
    participant EC2 as EC2 w VPC

    Note over CGW,VGW: Dwa tunele IPSec (Primary + Backup)
    CGW->>VGW: IKE Phase 1 (ISAKMP SA)
    VGW-->>CGW: IKE Phase 1 Response
    CGW->>VGW: IKE Phase 2 (IPSec SA)
    VGW-->>CGW: Tunnel UP
    CGW->>EC2: Pakiet 192.168.1.50 → 10.0.1.100 (through tunnel)
    EC2-->>CGW: Response
```

**VGW vs Transit Gateway dla VPN:**

| Cecha | VGW | Transit Gateway |
|-------|-----|-----------------|
| VPN connections per unit | 10 | 5000 |
| ECMP (load balance tunele) | ❌ | ✅ |
| Transitive routing | ❌ | ✅ |
| Multi-VPC VPN | Jeden VPC per VGW | Wiele VPC przez jeden TGW |
| Accelerated VPN (AWS Global Accelerator) | ✅ | ✅ |
| Cena | Brak opłaty za VGW, płacisz za VPN connection ($0.05/h) | $0.05/h per attachment + VPN |

> ⚠️ **Egzamin:** VGW to stary pattern dla jednego VPC. Dla wielu VPC, transitive routing lub SD-WAN → **Transit Gateway**. VPN CloudHub: wiele CGW → jeden VGW = hub-and-spoke dla wielu site.

**VPN CloudHub (pattern z VGW):**
```mermaid
graph TB
    VGW["Virtual Private Gateway\n(VPN CloudHub Hub)"]
    VGW <-->|VPN Tunnel| SITE1["Site 1\nWarszawa\n192.168.1.0/24"]
    VGW <-->|VPN Tunnel| SITE2["Site 2\nKraków\n192.168.2.0/24"]
    VGW <-->|VPN Tunnel| SITE3["Site 3\nWrocław\n192.168.3.0/24"]
    Note["Lokalizacje mogą się komunikować\nprzez VGW jak przez hub\n(site1 ↔ site2 przez VGW)"]
```

**Direct Connect + VGW:**
Direct Connect (DX) wymaga **Virtual Interface (VIF)**:
- **Private VIF** → do VGW → dostęp do prywatnych IP VPC
- **Transit VIF** → do TGW → dostęp do wielu VPC
- **Public VIF** → do publicznych serwisów AWS (S3, SES itd.)

**Zasięg:** Regionalny (VGW jest w jednym regionie, przypisany do jednego VPC)

---

### 9.2 Transit VPC — architektura (szerzej)

> Podstawy omówione w sekcji 8.9. Tutaj pełny kontekst architektoniczny i porównanie z TGW.

**Pełna architektura Transit VPC:**
```mermaid
graph TB
    subgraph TRANSIT_VPC["Transit VPC (Hub)\n10.0.0.0/16"]
        VPN1["EC2 VPN Router AZ1\n(Cisco CSR 1000v)\nSource/Dest Check OFF"]
        VPN2["EC2 VPN Router AZ2\n(HA failover)"]
        VGW_HUB["VGW Hub"]
    end
    
    subgraph SPOKE1["Spoke VPC 1 Dev\n10.1.0.0/16"]
        VGW1["VGW 1"] 
    end
    subgraph SPOKE2["Spoke VPC 2 Prod\n10.2.0.0/16"]
        VGW2["VGW 2"]
    end
    subgraph ON_PREM["On-premises"]
        CGW_ONPREM["Customer Gateway"]
    end
    
    VPN1 <-->|"VPN Tunnel"| VGW1
    VPN1 <-->|"VPN Tunnel"| VGW2
    VGW_HUB <-->|"VPN Tunnel"| CGW_ONPREM
    VPN2 -.->|HA failover| VPN1
```

**Dlaczego Transit VPC jest legacy (podsumowanie):**

| Problem Transit VPC | Rozwiązanie w TGW |
|--------------------|------------------|
| EC2 VPN router = SPOF + zarządzanie | AWS-managed, HA wbudowane |
| Przepustowość EC2-limited | 50 Gbps per attachment |
| VPN tunele między każdym Spoke i Hub | Bezpośrednie VPC attachments |
| Kosztowne licencje (Cisco CSR ~$0.35/h) | Brak kosztów oprogramowania |
| Brak centralnej Route Table | TGW Route Tables |
| Własny HA scripting (Lambda failover) | AWS-managed HA |

---

### 9.3 VPC Gateway Endpoint

**🟢 Co to jest?**
Gateway Endpoint to **target w Route Table** — specjalny typ endpointu, który kieruje ruch z VPC bezpośrednio do serwisów AWS przez **sieć AWS backbone**, bez NAT Gateway, bez Internetu.

> **Ważne:** Gateway Endpoint istnieje tylko dla **dwóch serwisów**: Amazon S3 i Amazon DynamoDB.

**Jak działa:**
```mermaid
graph LR
    EC2["EC2 Private Subnet\n10.0.1.50"]
    VPCE_GW["VPC Gateway Endpoint\n(prefix list target w Route Table)"]
    S3["Amazon S3\n(regional)"]

    EC2 -->|"Route Table:\npl-xxx → vpce-xxx"| VPCE_GW
    VPCE_GW -->|"AWS backbone\n(nie Internet)"| S3
```

**Route Table z Gateway Endpoint:**
```
Before (bez Gateway Endpoint):
  10.0.0.0/16  → local
  0.0.0.0/0    → nat-gw-xxx    ← S3 ruch szedł przez NAT GW!

After (z Gateway Endpoint):
  10.0.0.0/16  → local
  0.0.0.0/0    → nat-gw-xxx
  pl-xxxxxxxx  → vpce-xxxxxxxx  ← prefix list S3 → Gateway Endpoint
  (automatycznie dodana przez AWS przy tworzeniu endpoint)
```

**Kluczowe cechy Gateway Endpoint:**

| Cecha | Wartość |
|-------|---------|
| Serwisy | **S3** i **DynamoDB** (tylko te dwa!) |
| Koszt | **Bezpłatny** — $0.00 |
| NAT Gateway pominięty | ✅ S3/DynamoDB przez backbone, nie przez NAT |
| Dostęp z on-prem (przez VPN/DX) | ❌ Tylko z VPC |
| DNS | Nie zmienia DNS — używa public S3/DynamoDB DNS |
| Polityka endpointu | ✅ Możesz ograniczyć dostęp policy |
| Route Table association | Per Route Table (nie per subnet) |
| IPv6 | ❌ Tylko IPv4 |
| Cross-region | ❌ Regional only |

**Endpoint Policy (kontrola dostępu):**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": ["s3:GetObject", "s3:PutObject"],
    "Resource": "arn:aws:s3:::my-company-bucket/*"
  }]
}
```
> Endpoint Policy ogranicza jakie zasoby S3/DynamoDB są dostępne przez ten endpoint. Domyślnie: Full Access.

**Bucket Policy — wymuszaj dostęp tylko przez Endpoint:**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"],
    "Condition": {
      "StringNotEquals": {
        "aws:SourceVpce": "vpce-xxxxxxxxxxxxxxxx"
      }
    }
  }]
}
```
> To wymusza, że S3 bucket jest dostępny **WYŁĄCZNIE** przez VPC Endpoint — nie przez Internet, nie przez inne konta bez endpointu.

**Kiedy Gateway Endpoint:**
- ✅ EC2 w private subnet potrzebuje S3/DynamoDB
- ✅ Chcesz uniknąć NAT GW kosztu dla S3 traffic (może być duże oszczędności!)
- ✅ Compliance — ruch do S3 nie może iść przez Internet ani NAT
- ✅ Zwiększenie bezpieczeństwa — S3 dostępny tylko z VPC

**Zasięg:** Regionalny (endpoint dla S3 regional, nie cross-region)

---

### 9.4 Interface Endpoint (VPC Endpoint — Interface Type)

**🔵 Co to jest?**
Interface Endpoint to **Elastic Network Interface (ENI) z prywatnym IP** wdrożona w Twoim subnecie, która reprezentuje serwis AWS lub serwis partnera. Oparty na technologii **AWS PrivateLink**.

> **Analogia:** Zamiast dzwonić do S3 przez Internet, AWS wstawia "lokalny numer telefonu" (ENI z prywatnym IP) bezpośrednio w Twojej sieci. Dzwonisz do lokalnego numeru — AWS robi resztę.

**Jak działa — szczegółowo:**
```mermaid
graph LR
    subgraph VPC["Twoje VPC"]
        EC2["EC2\n10.0.1.50"]
        ENI["Interface Endpoint\nENI: 10.0.1.200\n(vpce-xxxxxxxx)"]
    end
    
    EC2 -->|"Resolve: secretsmanager.eu-west-1.amazonaws.com\n→ 10.0.1.200 (private DNS)"| ENI
    ENI -->|"AWS PrivateLink\n(wewnętrzna sieć AWS)"| SM["AWS Secrets Manager\n(service endpoint)"]
```

**DNS dla Interface Endpoint:**
Gdy tworzysz Interface Endpoint z **Private DNS enabled**:
```
Bez Private DNS:
  secretsmanager.eu-west-1.amazonaws.com → 52.x.x.x (publiczne IP)
  
Z Private DNS enabled:
  secretsmanager.eu-west-1.amazonaws.com → 10.0.1.200 (prywatne IP ENI!)
  
EC2 używa tej samej nazwy DNS — transparentne przekierowanie do ENI
```

**Kluczowe cechy Interface Endpoint:**

| Cecha | Wartość |
|-------|---------|
| Technologia | AWS PrivateLink |
| Serwisy | 100+ serwisów AWS + serwisy partnerów + własne serwisy |
| Koszt | **$0.01/h per AZ + $0.01/GB** |
| Deployment | ENI w subnecie (zazwyczaj per AZ dla HA) |
| DNS | Private DNS — transparentne, lub endpoint-specific DNS |
| Dostęp z on-prem | ✅ Przez VPN/Direct Connect → Route → ENI IP |
| Security Group | ✅ Możesz przypisać SG do ENI endpointu |
| IPv6 | ✅ Dual-stack możliwy |
| Cross-region | ❌ Regional (ale endpoint service może być cross-region przez DR) |

**Serwisy wspierające Interface Endpoint (przykłady):**
```
EC2 API, SSM, SSM Messages, EC2 Messages
Secrets Manager
KMS (Key Management Service)
ECR API, ECR Docker
CloudWatch Logs, CloudWatch Monitoring
SQS, SNS
API Gateway (Private)
Lambda (invocation)
STS
CodeBuild, CodeCommit, CodeDeploy
ECS
Kinesis
S3 (Interface, nie Gateway — różny typ!)
...i 100+ więcej
```

**Interface Endpoint — koszty vs NAT Gateway:**
```
Przykład: EC2 w private subnet → 100 GB/mc do Secrets Manager

Przez NAT GW:
  $0.045/h (NAT GW) + $0.045 x 100 GB = $0.045 x 720h + $4.50 = $32.40 + $4.50 = $36.90/mc

Przez Interface Endpoint (1 AZ):
  $0.01/h x 720h + $0.01 x 100 GB = $7.20 + $1.00 = $8.20/mc

Oszczędność: ~$28/mc per serwis. Rośnie z liczbą serwisów i ruchem.
```

**Interface Endpoint Security Group:**
```
SG dla Interface Endpoint:
  Inbound: Allow HTTPS (443) from 10.0.0.0/16 (VPC CIDR)
  → Tylko instancje z VPC mogą używać endpointu
  → Blokuj konkretne subnety jeśli potrzebujesz izolacji
```

**Dostęp z on-premises przez Interface Endpoint:**
```mermaid
graph LR
    ONPREM["On-premises\nServer: 192.168.1.50"]
    VPN["VPN/Direct Connect"]
    VPC["VPC"]
    ENI["Interface Endpoint\nENI: 10.0.1.200"]
    SSM["AWS SSM Service"]

    ONPREM -->|"Route: 10.0.1.200 via VPN"| VPN
    VPN --> VPC
    VPC --> ENI
    ENI -->|PrivateLink| SSM
    
    Note["On-prem serwer może używać SSM/Secrets Manager\nbez dostępu do Internetu!\nWymaga: Route do IP ENI przez VPN/DX"]
```

**Zasięg:** Regionalny, AZ-specific (ENI jest w konkretnym subnecie/AZ)

---

### 9.5 Jak zbudowany jest AWS PrivateLink

**🔴 Co to jest AWS PrivateLink?**
AWS PrivateLink to technologia umożliwiająca **prywatne połączenie** między VPC (konsumentów) a serwisami (w VPC producentów lub serwisami AWS) przez sieć AWS, **bez przechodzenia przez Internet, bez VPC Peering, bez IGW, bez NAT**.

> **Klucz:** PrivateLink działa na poziomie **serwisu** (nie VPC-do-VPC). Konsument widzi tylko ENI, nie cały VPC producenta.

**Architektura PrivateLink — jak zbudowany:**

```mermaid
graph TB
    subgraph CONSUMER_VPC["Consumer VPC (Twój VPC)"]
        EC2_C["EC2 Consumer"]
        ENI_ENDPOINT["Interface Endpoint\nENI: 10.0.1.100\n(vpce-xxx)"]
    end

    subgraph AWS_NETWORK["AWS Network Infrastructure"]
        PLLINK["PrivateLink\n(AWS-managed NLB proxy layer)"]
    end

    subgraph PRODUCER_VPC["Producer VPC (AWS Service VPC lub partner)"]
        NLB["Network Load Balancer\n(endpoint service)"]
        EC2_P1["Service EC2/ECS\nAZ1"]
        EC2_P2["Service EC2/ECS\nAZ2"]
        NLB --> EC2_P1
        NLB --> EC2_P2
    end

    EC2_C -->|"prywatny IP"| ENI_ENDPOINT
    ENI_ENDPOINT <-->|"PrivateLink tunnel\n(nie peering, nie IGW)"| PLLINK
    PLLINK <-->|"do NLB"| NLB
```

**Techniczne komponenty PrivateLink:**

| Komponent | Rola |
|-----------|------|
| **Endpoint Service** | Producent rejestruje NLB jako "Endpoint Service" — to co udostępnia |
| **Interface Endpoint (VPC Endpoint)** | Konsument tworzy ENI w swoim VPC — to co odbiera |
| **NLB (Network Load Balancer)** | Po stronie producenta — odbiera ruch z PrivateLink |
| **Hyperplane** | Wewnętrzna platforma AWS — obsługuje mapping ENI → NLB |

**Przepływ ruchu przez PrivateLink:**
```
1. EC2 Consumer (10.0.1.50) wywołuje API endpoint
2. DNS: secretsmanager.eu-west-1.amazonaws.com → 10.0.1.100 (ENI IP — private DNS)
3. TCP połączenie do 10.0.1.100:443
4. Hyperplane intercepts → przekazuje do NLB w Service VPC
5. NLB → target (AWS Service backend lub partner service)
6. Response wraca tą samą ścieżką
```

**Kluczowe właściwości PrivateLink:**
- ❌ Nie wymaga VPC Peering (nie widać sieci producenta)
- ❌ Nie wymaga IGW, NAT, publicznych IP
- ✅ Unidirectional — konsument inicjuje, producent nie "widzi" sieci konsumenta
- ✅ Działa z **overlapping CIDR** (bo nie ma routingu między VPC)
- ✅ Skalowalny — jeden Endpoint Service obsługuje tysiące konsumentów
- ✅ AWS serwisy AWS używają PrivateLink jako infrastruktury dla Interface Endpoints

**Własny Endpoint Service (producer side):**
```bash
# 1. Utwórz NLB w swoim VPC (Service VPC)
# 2. Utwórz Endpoint Service z NLB jako backend

aws ec2 create-vpc-endpoint-service-configuration \
  --network-load-balancer-arns arn:aws:elasticloadbalancing:...:loadbalancer/net/my-nlb/xxx \
  --acceptance-required \   # Konsument musi czekać na akceptację
  --private-dns-name api.myservice.com

# 3. Whitelist kont lub organizations które mogą się podłączyć
aws ec2 modify-vpc-endpoint-service-permissions \
  --service-id vpce-svc-xxx \
  --add-allowed-principals '["arn:aws:iam::111122223333:root"]'

# Konsument w swoim koncie:
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-yyy \
  --service-name com.amazonaws.vpce.eu-west-1.vpce-svc-xxx \
  --vpc-endpoint-type Interface \
  --subnet-ids subnet-aaa subnet-bbb \
  --security-group-ids sg-ccc
```

**PrivateLink vs VPC Peering — różnica fundamentalna:**

| Aspekt | PrivateLink (Interface Endpoint) | VPC Peering |
|--------|----------------------------------|-------------|
| Widoczność sieci | Tylko serwis (ENI IP) | Cała sieć drugiego VPC |
| Overlapping CIDR | ✅ Działa | ❌ Niemożliwe |
| Transitive | ❌ (nie trzeba) | ❌ |
| Initiation | Unidirectional (konsument → producent) | Bidirectional |
| Skala | 1 Endpoint Service → ∞ konsumentów | 1 Peering → 1 para VPC |
| Use case | Udostępnianie serwisu | Połączenie sieci |

---

### 9.6 Gateway Endpoint vs Interface Endpoint — pełne porównanie

```mermaid
graph TB
    subgraph GW["Gateway Endpoint"]
        GW1["Tylko S3 i DynamoDB"]
        GW2["Target w Route Table\n(prefix list)"]
        GW3["Bezpłatny"]
        GW4["Tylko z VPC\n(nie z on-prem)"]
        GW5["DNS bez zmian\n(używa public DNS)"]
    end
    
    subgraph IF["Interface Endpoint (PrivateLink)"]
        IF1["100+ serwisów AWS\n+ partner services"]
        IF2["ENI w subnecie\n(prywatny IP)"]
        IF3["$0.01/h + $0.01/GB"]
        IF4["Dostępny z on-prem\n(przez VPN/DX)"]
        IF5["Private DNS\n(transparentne)"]
    end
```

**Tabela porównawcza:**

| Cecha | Gateway Endpoint | Interface Endpoint |
|-------|-----------------|-------------------|
| **Serwisy** | S3, DynamoDB | 100+ serwisów AWS + partnerzy |
| **Typ** | Route Table target | ENI z prywatnym IP |
| **Koszt** | **$0.00** | $0.01/h/AZ + $0.01/GB |
| **Dostęp z on-prem** | ❌ | ✅ Przez VPN/DX → ENI IP |
| **DNS** | Bez zmian (public DNS) | Private DNS (public → private) |
| **Security Group** | ❌ | ✅ |
| **Endpoint Policy** | ✅ | ✅ |
| **Technologia** | Route injection | PrivateLink / ENI |
| **Deployment** | Per Route Table | Per subnet / AZ |
| **HA** | Zarządzany przez AWS | Deploy per AZ (1 ENI per AZ) |
| **IPv6** | ❌ | ✅ |

**S3 ma oba typy — kiedy który:**

| Gateway Endpoint dla S3 | Interface Endpoint dla S3 |
|------------------------|--------------------------|
| Dostęp tylko z VPC | Dostęp z on-prem przez VPN/DX |
| Bezpłatny | $0.01/h + data |
| Brak SG kontroli na endpoincie | SG na ENI endpointu |
| Nie wymaga DNS zmian | Private DNS |
| Prostszy w konfiguracji | Bardziej elastyczny |

> ⚠️ **Egzamin:** Pytanie "EC2 musi dotrzeć do S3 bez Internetu, bez NAT, jak najtaniej" → **Gateway Endpoint** (darmowy). Pytanie "Serwer on-prem musi dotrzeć do S3 przez VPN bez Internetu" → **Interface Endpoint** dla S3 (bo Gateway Endpoint nie obsługuje on-prem).

---

### 9.7 Porównanie z Azure: Private Endpoints i Service Endpoints

**Azure Service Endpoint (odpowiednik Gateway Endpoint):**

| | AWS Gateway Endpoint | Azure Service Endpoint |
|-|---------------------|----------------------|
| **Mechanizm** | Route Table target (prefix list) | Rozszerzenie subnetu do serwisu Azure |
| **Serwisy** | S3, DynamoDB (2 serwisy) | Storage, SQL, CosmosDB, Key Vault, Service Bus, Event Hub i inne |
| **Koszt** | Bezpłatny | Bezpłatny |
| **Dostęp z on-prem** | ❌ | ❌ (tylko z VNet) |
| **DNS** | Public DNS (bez zmian) | Public DNS (bez zmian) |
| **IP używany** | Prefix list (bloki AWS S3/DynamoDB) | Public IP serwisu (filtrowane) |
| **Izolacja** | Ruch przez AWS backbone | Ruch przez Microsoft backbone |
| **Eksponowane IP** | Publiczne IP serwisu AWS | Publiczne IP serwisu Azure |
| **Ograniczenie dostępu** | Endpoint Policy | VNet/Subnet rules na serwisie |

> **Kluczowa podobieństwo:** Oba darmowe, oba kierują ruch przez backbone providera, oba nie dają dostępu z on-prem, oba nie tworzą prywatnego IP w sieci.

**Azure Private Endpoint (odpowiednik Interface Endpoint):**

| | AWS Interface Endpoint | Azure Private Endpoint |
|-|----------------------|----------------------|
| **Mechanizm** | ENI z prywatnym IP w subnecie | NIC z prywatnym IP w VNet |
| **Technologia** | AWS PrivateLink | Azure Private Link |
| **Serwisy** | 100+ serwisów AWS + partnerzy | Azure Storage, SQL, CosmosDB, Key Vault, i 100+ |
| **Koszt** | $0.01/h/AZ + $0.01/GB | ~$0.01/h + $0.01/GB (podobne) |
| **Dostęp z on-prem** | ✅ Przez VPN/DX → ENI IP | ✅ Przez ExpressRoute/VPN → NIC IP |
| **DNS** | Private DNS (opcjonalne) | Private DNS Zone (opcjonalne) |
| **Security Group** | ✅ (na ENI endpointu) | ❌ (NSG nie działa na Private Endpoint NIC) |
| **Widoczność sieci** | Tylko ENI IP, nie sieć producenta | Tylko NIC IP, nie sieć producenta |
| **Kierunkowość** | Unidirectional | Unidirectional |
| **Overlapping CIDR** | ✅ | ✅ |

**Pełna tabela porównawcza AWS vs Azure:**

```mermaid
graph LR
    subgraph AWS["AWS"]
        GW_E["Gateway Endpoint\nS3 + DynamoDB\nBezpłatny\nRoute Table target"]
        IF_E["Interface Endpoint\n100+ serwisów\n$0.01/h ENI\nPrivateLink"]
    end
    subgraph AZURE["Azure"]
        SVC_E["Service Endpoint\nStorage+SQL+inne\nBezpłatny\nSubnet extension"]
        PRIV_E["Private Endpoint\n100+ serwisów\n~$0.01/h NIC\nPrivate Link"]
    end
    
    GW_E <-.->|"analogia"| SVC_E
    IF_E <-.->|"analogia"| PRIV_E
```

**Różnice Azure vs AWS — co warto zapamiętać:**

| Aspekt | AWS | Azure |
|--------|-----|-------|
| Service Endpoint — co blokuje | Endpoint Policy (na endpoincie) | Firewall rules na serwisie |
| Private Endpoint — SG | ✅ (na ENI) | ❌ (NSG disabled na PE NIC) |
| Private DNS zarządzanie | Route 53 Private Hosted Zone lub automatyczne | Azure Private DNS Zone (automatyczna integracja) |
| Endpoint dla własnych serwisów | ✅ Endpoint Service + NLB | ✅ Private Link Service + Azure Load Balancer |
| Akceptacja połączenia | Ręczna lub auto-approval | Auto lub manual approval |

**Własny serwis z PrivateLink (AWS) vs Private Link Service (Azure):**

| | AWS Endpoint Service | Azure Private Link Service |
|-|---------------------|--------------------------|
| Backend | NLB (Network Load Balancer) | Azure Standard Load Balancer |
| Konsument | Tworzy Interface Endpoint | Tworzy Private Endpoint |
| DNS własna domena | ✅ (Private DNS Name) | ✅ (Custom DNS) |
| Approval | Manual lub auto-accept | Manual lub auto-accept |
| Scope | Regional | Regional |

---

### 9.8 Well-Architected Framework

| Filar | VGW | Gateway Endpoint | Interface Endpoint / PrivateLink | Transit VPC vs TGW |
|-------|-----|-----------------|----------------------------------|-------------------|
| **Operational Excellence** | BGP Route Propagation automatyzuje routing | IaC (Terraform/CDK): endpoint + route table update = atomiczne | Private DNS eliminuje zmianę kodu — przezroczysty endpoint | TGW centralizuje routing: jeden punkt zarządzania zamiast wielu EC2 routerów |
| **Security** | VPN szyfrowany (IPSec) + BGP kontrola tras | Ruch S3/DynamoDB nie opuszcza AWS backbone. Bucket policy: `aws:SourceVpce` | Ruch nie przez Internet. SG na ENI = dodatkowy firewall. Unidirectional — serwis nie widzi sieci konsumenta | TGW Route Table segmentacja = prod/dev izolacja. Transit VPC miał EC2 jako SPOF security |
| **Reliability** | 2 tunele IPSec automatycznie (HA). BFD dla szybszego failover | Endpoint zarządzany przez AWS (HA). Route Table propagacja automatyczna | Deploy ENI per AZ dla HA. AWS zarządza Hyperplane (HA wbudowane) | TGW: AWS-managed HA. Transit VPC: własne skrypty failover = ryzyko |
| **Performance Efficiency** | 1.25 Gbps per VPN tunnel. ECMP przez TGW dla wyższej przepustowości | Zero dodatkowych hopów vs NAT GW. Niższe latency dla S3 (nie przez NAT) | ENI w Twoim subnecie = lokalny hop. PrivateLink: AWS-optimized path | TGW: 50 Gbps per attachment. Transit VPC: ograniczony przez EC2 type |
| **Cost Optimization** | $0.05/h per VPN connection. Accelerated VPN droższe ale lepsze latency | **Bezpłatny** — eliminuje NAT GW koszty dla S3/DynamoDB | $0.01/h/AZ — często tańsze niż NAT GW przy dużym ruchu do wielu serwisów | TGW: O(N) cost. Transit VPC: O(N) EC2 + licencje VPN software |
| **Sustainability** | VPN przez AWS Global: nie przez publiczny Internet | Mniej ruchu przez NAT GW = mniej skoków sieciowych = efektywniej | Bezpośrednia ścieżka do serwisu = krótsze trasy = mniejsze zużycie energii | TGW managed hardware = zoptymalizowane przez AWS energetycznie vs fleet EC2 routerów |

---

### 9.9 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Firma ma 30 kont AWS, każde z EC2. Wszystkie muszą używać Secrets Manager i KMS bez dostępu do Internetu. Centralne zarządzanie. Jak najefektywniej?

**Rozwiązanie:**
```
Option A: Interface Endpoint per konto per serwis
  → 30 kont × 2 serwisy = 60 endpointów
  → Koszt: 60 × $0.01/h × 720h = $432/mc + data
  
Option B (lepsza): Centralne VPC z Interface Endpoints + TGW
  Network Account:
    → 1 Interface Endpoint dla Secrets Manager
    → 1 Interface Endpoint dla KMS
    → TGW z attachments do wszystkich 30 kont
    
  Spoke VPC (każde konto):
    → Route: serwis IP → TGW → Network VPC → Interface Endpoint
    → 2 endpointy zamiast 60
    
  Oszczędność: ~58 endpointów × $0.01/h = ~$417/mc mniej
  Trade-off: TGW koszty, centralne DNS, single point of management (akceptowalne)
```

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Microservice w ECS (Fargate) musi bezpiecznie komunikować się z: S3 (duże dane), SQS (kolejki), Secrets Manager (credentials). Brak NAT Gateway w projekcie (koszt). Jak?

**Rozwiązanie:**
```
S3 (duże dane):     VPC Gateway Endpoint → bezpłatny, S3 przez backbone
SQS (kolejki):      Interface Endpoint dla SQS ($0.01/h) 
Secrets Manager:    Interface Endpoint dla Secrets Manager ($0.01/h)

Konfiguracja:
  Private Subnet z ECS → Route Table: pl-xxx → vpce-gateway (S3)
  Private Subnet z ECS → ENI Interface Endpoint SQS (Private DNS)
  Private Subnet z ECS → ENI Interface Endpoint SM (Private DNS)
  
  Security Group na Interface Endpoints: Allow 443 from ECS Task SG only
  
Zero NAT Gateway, zero Internet traffic, security compliant
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma używa Transit VPC (EC2 Cisco CSR) jako hub. Koszty $0.35/h per EC2 × 2 (HA) = $504/mc. EC2 wymagają patchowania, team sieciowy narzeka. Jak ulepszyć?

**Plan migracji Transit VPC → TGW:**
```
Faza 1: Deploy Transit Gateway
  - Utwórz TGW w Network Account
  - Udostępnij przez RAM do wszystkich kont

Faza 2: Migracja VPC attachments
  - Każde Spoke VPC: Utwórz TGW attachment
  - TGW Route Table: dodaj trasy do wszystkich VPC
  - Spoke VPC Route Tables: zmień trasy z VPN endpoint → tgw-xxx
  - Testuj komunikację VPC-to-VPC

Faza 3: Migracja on-prem VPN
  - Utwórz Site-to-Site VPN na TGW (zamiast na VGW w Transit VPC)
  - Customer Gateway (on-prem) → TGW VPN attachment
  - Testuj, następnie usuń VPN z Transit VPC EC2

Faza 4: Cleanup
  - Wyłącz Transit VPC EC2 (po okresie obserwacji)
  - Usuń VGW z Transit VPC
  - Zakończ licencje Cisco CSR

Oszczędność: $504/mc EC2 + licencje → $72/mc TGW (4 attachments × $0.05/h × 720h)
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Firma on-prem ma 200 serwerów które używają SQL Server i Windows plików. Migracja do AWS (EC2 + RDS). Serwery on-prem muszą NADAL działać przez 12 miesięcy. Jak zapewnić łączność i bezpieczny dostęp do serwisów AWS?

```
Architektura hybrydowa:
  On-prem → Direct Connect (lub VPN) → TGW → VPC
  
  TGW attachments:
    - VPC Production
    - VPC Shared Services  
    - DX Transit VIF (Direct Connect Gateway)
    
  Serwisy AWS przez Interface Endpoints:
    - SSM (Session Manager zamiast SSH/RDP — bezpieczniejsze)
    - CloudWatch (metryki serwerów migrowanych)
    - S3 (backupy, snapshots) przez Gateway Endpoint
    
  On-prem serwery → DX → TGW → VPC → Interface Endpoint ENI IP
    → On-prem serwery mogą używać SSM/CW/S3 bez Internetu
    
  DNS:
    - Route 53 Resolver Outbound: *.corp.local → on-prem DC
    - Route 53 Resolver Inbound: on-prem → Route 53 → AWS internal names
    - DHCP Option Set: wskazuje na Route 53 Resolver (centralny)
```

---

## 10. DNS (Regional/Zonal), Private DNS, Site-to-Site VPN, CGW, NAT-T, IPSec, Client VPN, CloudHub, BGP, VPN Failover

### 10.1 Regional i Zonal DNS entries — Route 53 i VPC DNS

**🔵 Co to jest?**
AWS ma dwie kategorie wpisów DNS dla zasobów: **regionalne** (jeden rekord, dostępny z całego regionu) i **zonalne** (AZ-specific, pozwala na routing ruchu do konkretnej Availability Zone).

**Regionalne DNS entries:**
```
Przykłady:
  EC2:          ec2-52-10-20-30.eu-west-1.compute.amazonaws.com
  RDS:          mydb.cluster-xxxx.eu-west-1.rds.amazonaws.com
  ALB:          my-alb-1234567890.eu-west-1.elb.amazonaws.com
  S3:           my-bucket.s3.eu-west-1.amazonaws.com
  
Cechy:
  → Resolwuje na IP zasobu w regionie
  → Jeden rekord dla całego regionu
  → ALB/NLB: resolwuje na wiele IP (Multi-Value, różne AZ)
```

**Zonalne (AZ-specific) DNS entries:**
```
Przykłady:
  Interface Endpoint per-AZ DNS:
    vpce-xxx-eu-west-1a.secretsmanager.eu-west-1.vpce.amazonaws.com → 10.0.1.100
    vpce-xxx-eu-west-1b.secretsmanager.eu-west-1.vpce.amazonaws.com → 10.0.2.100
    vpce-xxx-eu-west-1c.secretsmanager.eu-west-1.vpce.amazonaws.com → 10.0.3.100
  
  ALB Zonal DNS (od 2023 — zonal shift):
    eu-west-1a.my-alb-1234.eu-west-1.elb.amazonaws.com → IP w AZ1
  
Cechy:
  → Resolwuje na IP zasobu w konkretnym AZ
  → Przydatne dla AZ-affinity (niższe latency, brak cross-AZ charges)
  → Interface Endpoints zawsze generują zarówno regional jak i zonal DNS
```

**AZ-affinity pattern:**
```mermaid
graph LR
    EC2_AZ1["EC2 w AZ1"] -->|"Zonal DNS:\nvpce-xxx-eu-west-1a.*"| ENI_AZ1["Interface Endpoint ENI\nw AZ1 (10.0.1.100)"]
    EC2_AZ2["EC2 w AZ2"] -->|"Zonal DNS:\nvpce-xxx-eu-west-1b.*"| ENI_AZ2["Interface Endpoint ENI\nw AZ2 (10.0.2.100)"]
    
    Note["EC2 w AZ1 trafia do ENI w AZ1\n= brak cross-AZ transfer charges\n= niższe latency"]
```

**EC2 hostname i DNS:**
```
EC2 z publicznym IP (public subnet):
  Public DNS:   ec2-52-10-20-30.compute-1.amazonaws.com  (us-east-1)
                ec2-52-10-20-30.eu-west-1.compute.amazonaws.com  (inne regiony)
  Resolve:      z Internetu → 52.10.20.30 (public IP)
                z wewnątrz VPC → 10.0.1.50 (private IP!)  ← split-horizon DNS

EC2 bez publicznego IP (private subnet):
  Private DNS:  ip-10-0-1-50.eu-west-1.compute.internal
  Resolve:      zawsze → 10.0.1.50

Split-horizon = ta sama nazwa DNS, różna odpowiedź zależnie od skąd pytasz
```

**DNS dla VPC — konfiguracja:**
```
VPC DNS Settings (dwa ustawienia):
  enableDnsHostnames: ✅ → EC2 dostają DNS hostname
  enableDnsSupport:   ✅ → VPC ma DNS resolver (AmazonProvidedDNS)

Domyślnie:
  Default VPC: oba włączone
  Custom VPC:  enableDnsSupport = true, enableDnsHostnames = false
  
Dla Interface Endpoints (Private DNS):
  Oba MUSZĄ być włączone!
```

---

### 10.2 Private DNS i Private DNS Resolution

**🟢 Private DNS — co to jest?**
Private DNS to mechanizm rozwiązywania nazw DNS **tylko wewnątrz VPC** — zasoby prywatne (bez publicznego IP) mogą mieć swoje DNS nazwy, niewidoczne z Internetu.

**Route 53 Private Hosted Zone (PHZ):**
```mermaid
graph TB
    R53_PHZ["Route 53\nPrivate Hosted Zone\ncorp.internal"]
    R53_PHZ -->|"A record"| DB["db.corp.internal → 10.0.1.100"]
    R53_PHZ -->|"CNAME record"| API["api.corp.internal → internal-alb.eu-west-1.elb.amazonaws.com"]
    R53_PHZ -->|"A record"| APP["app.corp.internal → 10.0.2.50"]
    
    VPC_A["VPC A\n(associated)"] -->|"resolve corp.internal"| R53_PHZ
    VPC_B["VPC B\n(associated)"] -->|"resolve corp.internal"| R53_PHZ
    INET((Internet)) -.->|"❌ nie widzi corp.internal"| R53_PHZ
```

**Kluczowe cechy Private Hosted Zone:**

| Cecha | Wartość |
|-------|---------|
| Widoczność | Tylko z powiązanych VPC |
| Powiązanie | Wiele VPC per PHZ (cross-account możliwe) |
| Split-horizon | ✅ Możesz mieć PHZ i PHZ o tej samej nazwie (ale różne zasoby per VPC) |
| Overlapping public | ✅ PHZ `example.com` nadpisuje publiczne `example.com` dla powiązanych VPC |
| Cena | $0.50/mc per hosted zone + $0.40/mln queries |

**Split-horizon DNS (split-view):**
```
Public Hosted Zone: example.com
  api.example.com → 52.10.20.30 (publiczny ALB)
  
Private Hosted Zone: example.com (associated z VPC)
  api.example.com → 10.0.1.100 (wewnętrzny ALB)
  
Z VPC: api.example.com → 10.0.1.100 (prywatny)
Z Internetu: api.example.com → 52.10.20.30 (publiczny)
Kod nie musi wiedzieć w jakim środowisku działa!
```

**Private DNS dla Interface Endpoints:**
```
Gdy tworzysz Interface Endpoint z "Enable Private DNS":
  AWS automatycznie:
    1. Tworzy/aktualizuje Route 53 PHZ dla serwisu w Twoim VPC
    2. Dodaje A record: secretsmanager.eu-west-1.amazonaws.com → 10.0.1.200 (ENI IP)
    
EC2 w VPC:
  resolve secretsmanager.eu-west-1.amazonaws.com → 10.0.1.200 (ENI!)
  Nie zmienia kodu, nie zmienia SDK konfiguracji
  
Wymagania:
  VPC enableDnsHostnames = true
  VPC enableDnsSupport = true
```

**Route 53 Resolver — jak działa prywatne resolution:**
```mermaid
graph LR
    EC2["EC2\n10.0.1.50"]
    R53R["Route 53 Resolver\n10.0.0.2 lub 169.254.169.253"]
    PHZ["Private Hosted Zone\ncorp.internal"]
    PUB["Public DNS\n(Route 53 Public)"]
    
    EC2 -->|"1. resolve: db.corp.internal"| R53R
    R53R -->|"2. sprawdź PHZ"| PHZ
    PHZ -->|"3. znaleziono: 10.0.1.100"| R53R
    R53R -->|"4. odpowiedź: 10.0.1.100"| EC2
    
    EC2 -->|"resolve: google.com"| R53R
    R53R -->|"brak w PHZ"| PUB
    PUB -->|"142.250.x.x"| R53R
```

**Route 53 Resolver Endpoints (hybrid DNS):**
```
Inbound Endpoint:
  IP w VPC → on-prem DNS może forwardować do AWS
  On-prem: DNS conditional forwarder: *.corp.aws → Inbound Endpoint IP
  
Outbound Endpoint:
  EC2 w VPC → Route 53 Resolver → Forwarding Rules → on-prem DNS
  Reguła: *.corp.local → on-prem DNS 192.168.1.10

Razem:
  AWS EC2 → resolve corp.local → Outbound Endpoint → on-prem DNS
  On-prem → resolve aws.corp.internal → Inbound Endpoint → Route 53 PHZ
```

**Private DNS a VPN/Direct Connect:**
```
Problem: on-prem serwer chce resolve aws.corp.internal (PHZ)
  
Rozwiązanie A: Route 53 Resolver Inbound Endpoint
  On-prem DNS conditional forwarder: aws.corp.internal → 10.0.0.5 (Inbound Endpoint IP)
  Ruch przez VPN/DX → Inbound Endpoint IP → Route 53 Resolver → PHZ → odpowiedź

Rozwiązanie B: DNS forwarder EC2 w VPC (legacy)
  On-prem DNS → EC2 BIND/Unbound w VPC → Route 53 Resolver (169.254.169.253) → PHZ
  Stary wzorzec, Route 53 Resolver Endpoints są lepsze
```

---

### 10.3 Protokoły VPN — podstawy

Przed omówieniem AWS VPN — fundament protokołów.

**Rodzaje tuneli VPN:**

```mermaid
graph TB
    VPN["Protokoły VPN"]
    VPN --> IPSEC["IPSec\n(Internet Protocol Security)\nWarstwa 3 — szyfruje IP pakiety"]
    VPN --> SSL_TLS["SSL/TLS\n(OpenVPN, SSTP)\nWarstwa 4/7 — przez TCP/UDP"]
    VPN --> WG["WireGuard\nNowoczesny, lekki\nUDP, Curve25519"]
    VPN --> MPLS["MPLS L2/L3 VPN\nPrzewodnik MPLS\n(Direct Connect level)"]
```

#### IPSec — szczegółowo

**IPSec** to zestaw protokołów warstwy 3 zapewniający: szyfrowanie, integralność i autentykację pakietów IP.

**Dwie fazy IPSec (IKE):**

```mermaid
sequenceDiagram
    participant A as Peer A (CGW on-prem)
    participant B as Peer B (VGW AWS)

    Note over A,B: FAZA 1 — IKE (Internet Key Exchange)
    A->>B: Propozycja: algorytmy (AES, SHA, DH group)
    B-->>A: Akceptacja algorytmów
    A->>B: Wymiana kluczy Diffie-Hellman
    B-->>A: DH public key
    Note over A,B: Shared secret obliczony, ISAKMP SA ustanowiony
    A->>B: Identyfikacja (pre-shared key lub certyfikat)
    B-->>A: Identyfikacja potwierdzona
    Note over A,B: IKE SA (ISAKMP SA) ustanowiony — szyfrowany kanał sterujący

    Note over A,B: FAZA 2 — IPSec SA
    A->>B: Propozycja IPSec (ESP, AES-256, SHA-256)
    B-->>A: Akceptacja
    Note over A,B: IPSec SA ustanowiony — szyfrowany tunel danych
    A->>B: Dane przez tunel (ESP encrypted)
```

**IKEv1 vs IKEv2:**

| Cecha | IKEv1 | IKEv2 |
|-------|-------|-------|
| Złożoność | Wyższy — 6 lub 3 pakiety w Main/Aggressive mode | Prostszy — 4 pakiety |
| Certyfikaty | Bardziej ograniczone | Lepsze wsparcie |
| MOBIKE | ❌ | ✅ — zmiana IP bez rekonekcji (mobilne) |
| EAP (Extended Authentication) | ❌ | ✅ — ważne dla Client VPN |
| Dead Peer Detection | Dodatkowy RFC | Wbudowany |
| AWS obsługa | ✅ | ✅ (preferowany) |

**IPSec Tryby:**

| Tryb | Opis | Użycie |
|------|------|--------|
| **Transport Mode** | Szyfruje tylko payload (dane), zachowuje oryginalny nagłówek IP | Host-to-host, rzadkie |
| **Tunnel Mode** | Szyfruje cały pakiet IP + nowy nagłówek IP | **Site-to-site VPN** — AWS używa tego |

**ESP (Encapsulating Security Payload):**
```
Pakiet IPSec ESP (Tunnel Mode):
  
  [Zewnętrzny IP nagłówek] [ESP Header] [Zaszyfrowany oryginalny IP] [ESP Trailer] [ESP Auth]
  
  Zewnętrzny IP: Public IP CGW → Public IP VGW (widziany w sieci)
  ESP Header:    SPI (Security Parameter Index), numer sekwencji
  Zaszyfrowane:  Oryginalny pakiet 10.0.1.50 → 192.168.1.100 (prywatne IPs)
  ESP Auth:      Integralność pakietu (HMAC)
  
  Algorytmy szyfrowania ESP: AES-128, AES-256, AES-128-GCM, AES-256-GCM (AWS)
  Algorytmy integralności: SHA-1, SHA-256, SHA-384, SHA-512
```

**AH (Authentication Header) — różnica od ESP:**
- AH: tylko autentykacja i integralność, **bez szyfrowania**
- ESP: autentykacja + szyfrowanie
- AWS używa **ESP** (nie AH) dla Site-to-Site VPN

#### NAT-T (NAT Traversal)

**Problem:** IPSec ESP pakiety nie mają portów TCP/UDP — firewall/NAT nie wie jak je zmapować.

```
Normalny IPSec ESP:
  [IP header: src=203.0.113.1, dst=52.10.20.1] [ESP payload]
  NAT router: "brak portu → nie wiem jak translować" → odrzuca!

NAT-T (UDP 4500):
  [IP header: src=203.0.113.1, dst=52.10.20.1] [UDP: 4500→4500] [ESP payload]
  NAT router: "UDP 4500 → mogę translować" → przepuszcza
  
NAT-T opakuje ESP w UDP port 4500 → NAT może translować
```

**NAT-T w AWS Site-to-Site VPN:**
```
Scenariusz: On-prem CGW za NAT (typowe!)
  On-prem LAN → NAT Router (publiczny IP) → Internet → VGW

  IKE: UDP 500 (Key exchange)
  Wykrycie NAT: IKE Exchange → oba peery wykrywają że jest NAT pomiędzy
  Przełączenie: UDP 500 → UDP 4500 (NAT-T)
  Dane: ESP opakowane w UDP 4500

AWS VGW automatycznie obsługuje NAT-T — nie wymaga konfiguracji po stronie AWS.
CGW musi obsługiwać NAT-T (wszystkie nowoczesne urządzenia: Cisco, Juniper, pfSense, StrongSwan).
```

**Porty które muszą być otwarte dla VPN:**

| Port | Protokół | Cel |
|------|----------|-----|
| UDP 500 | IKE | Key exchange (faza 1 i 2) |
| UDP 4500 | NAT-T | IPSec ESP przez NAT |
| Protocol 50 | ESP | Dane (bez NAT-T — bezpośredni IPSec) |

> ⚠️ **Egzamin:** Jeśli CGW jest za NAT → **NAT-T wymagane** (UDP 4500). AWS VGW to obsługuje automatycznie.

#### OpenVPN

**OpenVPN** to SSL/TLS-based VPN:
```
Protokół: TLS (nad TCP lub UDP)
Port: UDP 1194 (domyślny), TCP 443 (przez firewalle)
Szyfrowanie: OpenSSL (AES-256-GCM, ChaCha20-Poly1305)
Auth: Certyfikaty X.509, hasło, MFA (przez plugin)
```

**AWS Client VPN używa OpenVPN protocol** — klienci używają standardowego klienta OpenVPN lub AWS VPN Client.

**OpenVPN vs IPSec:**

| Cecha | OpenVPN | IPSec (IKEv2) |
|-------|---------|---------------|
| Warstwa | 4/7 (SSL/TLS) | 3 (Network) |
| Firewall traversal | ✅ Łatwy (TCP 443) | Trudniejszy (ESP, UDP 500/4500) |
| Performance | Niższy (SSL overhead) | Wyższy (kernel implementation) |
| Compatibility | Wymaga oprogramowania | Wbudowany w OS (Windows, macOS, iOS, Android) |
| Certyfikaty | ✅ | ✅ |
| Use case | Client VPN, remote access | Site-to-site, enterprise |
| AWS użycie | Client VPN | Site-to-Site VPN (VGW/TGW) |

#### WireGuard

Nowoczesny protokół VPN (2020), **nie używany natywnie przez AWS** ale dostępny jako EC2 solution:
```
Protokół: UDP (własny)
Szyfrowanie: ChaCha20-Poly1305, Curve25519
Performance: ~2x szybszy niż OpenVPN, comparable z IPSec
Kod: ~4000 linii (vs ~400 000 IPSec/OpenVPN) — prostszy audyt
Brak: Dynamicznego IP dla peera (nie idealny dla roaming)
```

---

### 10.4 Site-to-Site VPN — AWS

**🔵 Co to jest?**
AWS Site-to-Site VPN łączy sieć **on-premises** z VPC przez **zaszyfrowany tunel IPSec** przez Internet publiczny (lub AWS Global Accelerator).

**Komponenty:**

| Komponent | Opis |
|-----------|------|
| **Virtual Private Gateway (VGW)** | AWS-side VPN termination, przypisany do VPC |
| **Transit Gateway (TGW)** | Nowszy: AWS-side dla multi-VPC VPN |
| **Customer Gateway (CGW)** | Reprezentacja Twojego on-prem urządzenia w AWS |
| **VPN Connection** | Logiczny link VGW↔CGW z 2 tunelami |
| **Customer Gateway Device** | Fizyczne/wirtualne urządzenie on-prem (Cisco, pfSense, StrongSwan...) |

**Architektura pełna:**

```mermaid
graph LR
    subgraph ON_PREM["On-premises DC"]
        LAN["Sieć LAN\n192.168.0.0/16"]
        CGW_DEV["Customer Gateway Device\n(Cisco ASA / pfSense / StrongSwan)\nPublic IP: 203.0.113.1"]
    end

    subgraph AWS_REGION["AWS Region"]
        subgraph VPC["VPC 10.0.0.0/16"]
            EC2["EC2"]
            SUBNET["Private Subnet"]
        end
        VGW["Virtual Private Gateway\n(VGW)\nvpn-endpoint: 52.10.20.1 (T1)\nvpn-endpoint: 52.10.21.1 (T2)"]
    end

    CGW["Customer Gateway\n(zasób AWS)\nIP: 203.0.113.1"]

    LAN --> CGW_DEV
    CGW_DEV <-->|"Tunel 1 (Primary)\nIPSec AES-256"| VGW
    CGW_DEV <-.->|"Tunel 2 (Backup)\nIPSec AES-256"| VGW
    CGW -.->|"reprezentuje"| CGW_DEV
    VGW --> VPC
    SUBNET --> EC2
```

**Customer Gateway (CGW) — obiekt w AWS:**
```bash
# Tworzenie CGW w AWS (reprezentacja on-prem urządzenia)
aws ec2 create-customer-gateway \
  --type ipsec.1 \
  --public-ip 203.0.113.1 \          # Publiczny IP on-prem urządzenia
  --bgp-asn 65000 \                  # BGP ASN on-prem (dla dynamic routing)
  --device-name "cisco-asa-warsaw"

# Tworzenie VPN Connection (VGW)
aws ec2 create-vpn-connection \
  --type ipsec.1 \
  --customer-gateway-id cgw-xxx \
  --vpn-gateway-id vgw-xxx \         # Lub --transit-gateway-id tgw-xxx
  --options '{"StaticRoutesOnly": false}'  # false = BGP dynamic routing
```

**Konfiguracja tunelu VPN:**
```
Tunel 1 parameters (z konfiguracji pobranej z AWS):
  Outside IP (AWS side): 52.10.20.1
  Outside IP (CGW side): 203.0.113.1
  Inside CIDR: 169.254.x.x/30 (link-local dla BGP session)
  Pre-shared key: xxxxxxxxxxxxx (losowy, 32+ znaki)
  IKE version: IKEv2
  Phase 1: AES-256-GCM, SHA-256, DH Group 20
  Phase 2: AES-256-GCM, SHA-256, PFS Group 20
  
Tunel 2 parameters:
  Outside IP (AWS side): 52.10.21.1   ← Inny endpoint AWS dla HA
  Outside IP (CGW side): 203.0.113.1  ← Ten sam CGW
  Inside CIDR: 169.254.y.y/30
  Pre-shared key: yyyyyyyyyyyyy       ← Inny klucz
```

**AWS pobiera konfigurację dla konkretnego urządzenia:**
```bash
aws ec2 describe-vpn-connections \
  --vpn-connection-ids vpn-xxx \
  --query 'VpnConnections[0].CustomerGatewayConfiguration'
# Zwraca XML z pełną konfiguracją dla: Cisco, Juniper, Fortinet, pfSense, StrongSwan, Palo Alto itd.
```

---

### 10.5 BGP i VPN — Route Propagation

**BGP (Border Gateway Protocol)** to protokół routingu używany między autonomicznymi systemami (AS). W kontekście AWS VPN:

**Terminologia BGP:**

| Term | Opis |
|------|------|
| **ASN** (Autonomous System Number) | Numer identyfikujący sieć w BGP. Private: 64512-65534. AWS VGW domyślnie: 64512 |
| **IBGP** | BGP wewnątrz jednego AS |
| **EBGP** | BGP między różnymi AS |
| **BGP Peer** | Partner BGP (CGW ↔ VGW) |
| **BGP Advertisement** | Ogłaszanie tras przez BGP |
| **BGP Attribute** | Właściwości trasy (LOCAL_PREF, AS_PATH, MED, NEXT_HOP) |

**Jak BGP działa z VPN:**
```mermaid
sequenceDiagram
    participant CGW as CGW On-prem (ASN 65000)
    participant VGW as VGW AWS (ASN 64512)

    Note over CGW,VGW: BGP Session przez tunel VPN (TCP 179)
    CGW->>VGW: BGP OPEN (ASN 65000)
    VGW-->>CGW: BGP OPEN (ASN 64512)
    Note over CGW,VGW: BGP Session ESTABLISHED

    CGW->>VGW: BGP UPDATE: 192.168.0.0/16 (on-prem sieć)
    VGW->>CGW: BGP UPDATE: 10.0.0.0/16 (VPC CIDR)

    Note over CGW,VGW: Routing tables zaktualizowane
    Note over VGW: Route Table VPC (jeśli Route Propagation ON)
    Note over VGW: 192.168.0.0/16 → vgw-xxx  (auto-wstrzyknięte!)
```

**Route Propagation — konfiguracja:**
```bash
# Włącz Route Propagation w Route Table
aws ec2 enable-vgw-route-propagation \
  --route-table-id rtb-xxx \
  --gateway-id vgw-xxx

# Efekt: VGW automatycznie wstrzykuje trasy z BGP do Route Table
# Route Table po włączeniu:
#   10.0.0.0/16       → local
#   192.168.0.0/16    → vgw-xxx  ← AUTOMATYCZNIE z BGP!
#   0.0.0.0/0         → nat-gw
```

**Static vs Dynamic (BGP) routing:**

| Aspekt | Static Routing | Dynamic BGP |
|--------|---------------|-------------|
| Konfiguracja | Ręczne trasy w AWS | Automatyczna wymiana tras |
| Skala | Małe sieci | Duże sieci, wiele site |
| Failover | Ręczny | Automatyczny (BGP failover) |
| Precyzja | Dokładne trasy | Automatyczne, ale większa złożoność |
| Użycie | Simple setups, S2S VPN z 1 lokalizacją | Enterprise, CloudHub, HA setups |
| AWS support | Tak (StaticRoutesOnly: true) | Tak (domyślny z BGP ASN) |

**BGP ASN w AWS:**
```
VGW domyślny ASN: 64512 (konfigurowalne przy tworzeniu VGW: 64512-65534 lub public ASN)
TGW ASN: 64512 (konfigurowalne)
CGW ASN: Twój on-prem AS (np. 65000 — private range)

AWS BGP session (link-local):
  Inside tunnel CIDR: 169.254.x.x/30
  AWS side: 169.254.x.1
  CGW side: 169.254.x.2
  BGP session TCP 179 przez tunel (nie przez publiczny Internet)
```

**BGP Attributes dla failover:**
```
LOCAL_PREF: Wyższy = preferowana trasa (dla wychodzącego ruchu z AS)
  Tunel 1: LOCAL_PREF 200 (preferred)
  Tunel 2: LOCAL_PREF 100 (backup)
  
AS_PATH Prepending: Dłuższy AS_PATH = mniej preferowany
  Tunel 1: AS_PATH 65000 (shorter = preferred)
  Tunel 2: AS_PATH 65000 65000 65000 (longer = less preferred)
  
MED (Multi-Exit Discriminator): Mniejszy = preferowany
  AWS używa MED by wskazać preferencję tunelu dla ruchu z CGW
```

---

### 10.6 VPN Failover

**Problem:** Site-to-Site VPN ma 2 tunele (Primary i Backup). Co się dzieje przy awarii?

**Scenariusz 1 — Failover między 2 tunelami tego samego VPN:**
```mermaid
graph LR
    CGW["CGW On-prem\n203.0.113.1"]
    VGW["VGW AWS"]
    
    CGW <-->|"Tunel 1 (Primary)\nBGP: LOCAL_PREF 200\nSTATUS: UP"| VGW
    CGW <-.->|"Tunel 2 (Backup)\nBGP: LOCAL_PREF 100\nSTATUS: UP (standby)"| VGW
    
    Note["Tunel 1 fail:\n  BGP withdrawal trasy z Tunelu 1\n  Ruch failover na Tunel 2\n  Czas: ~60 sekund (DPD detection + BGP)"]
```

**Scenariusz 2 — Failover między dwoma VPN connections (dual-ISP):**
```mermaid
graph TB
    subgraph ON_PREM["On-premises (Dual ISP)"]
        CGW1_DEV["CGW Device\nISP1 IP: 203.0.113.1"]
        CGW2_DEV["CGW Device (lub drugi port)\nISP2 IP: 198.51.100.1"]
    end
    VGW["VGW / TGW\n(AWS)"]
    
    CGW1["CGW1 (ISP1)\nin AWS"]
    CGW2["CGW2 (ISP2)\nin AWS"]
    
    CGW1_DEV <-->|"VPN Connection 1\nPrimary (LOCAL_PREF 200)"| VGW
    CGW2_DEV <-->|"VPN Connection 2\nBackup (LOCAL_PREF 100)"| VGW
    
    Note["ISP1 fail:\n  VPN Connection 1 tunele down\n  BGP withdrawal\n  Ruch failover na VPN Connection 2 (ISP2)\n  Pełna HA na poziomie ISP"]
```

**Czas failover:**
```
DPD (Dead Peer Detection):
  Wykrycie awarii peera: 10-30 sekund (konfigurowalne)
  
BGP failover:
  Withdrawal trasy + ponowna selekcja: 10-60 sekund
  
Całkowity failover: ~30-90 sekund
  
Dla szybszego failover: BFD (Bidirectional Forwarding Detection)
  BFD detection: <1 sekunda
  Dostępne przez Direct Connect (nie przez VPN)
```

**VPN + Direct Connect HA (złoty standard):**
```mermaid
graph LR
    ON_PREM["On-premises"]
    TGW["Transit Gateway"]
    
    ON_PREM <-->|"Primary:\nDirect Connect\n(Private VIF)\nNiska latency, wysoka przepustowość"| TGW
    ON_PREM <-.->|"Backup:\nSite-to-Site VPN\n(wyższe latency, niższa bandwidth)\nAutomatic failover przez BGP"| TGW
    
    Note["DX fail → BGP wycofuje trasy DX\n→ ruch failover na VPN backup\nSame BGP attributes different metrics"]
```

```
BGP konfiguracja dla DX primary + VPN backup:
  Direct Connect Virtual Interface: LOCAL_PREF 200 (lub wyższy MED)
  VPN Connection: LOCAL_PREF 100 (niższy = backup)
  
  Gdy DX fail: BGP nie ogłasza tras przez DX → VPN staje się aktywny
```

---

### 10.7 VPN CloudHub

**🟡 Co to jest?**
VPN CloudHub to wzorzec (nie osobna usługa) gdzie **wiele Customer Gateways** łączy się do jednego VGW, a VGW pełni rolę **centralnego hub** zarówno dla komunikacji z VPC jak i **między lokalizacjami on-prem**.

```mermaid
graph TB
    VGW["Virtual Private Gateway\n(VPN CloudHub Hub)\nASN: 64512"]
    
    VGW <-->|"VPN + BGP\nASN 65001"| WARSAW["Lokalizacja Warszawa\nCGW1: 203.0.113.1\nSieć: 192.168.1.0/24"]
    VGW <-->|"VPN + BGP\nASN 65002"| KRAKOW["Lokalizacja Kraków\nCGW2: 198.51.100.1\nSieć: 192.168.2.0/24"]
    VGW <-->|"VPN + BGP\nASN 65003"| WROCLAW["Lokalizacja Wrocław\nCGW3: 203.0.114.1\nSieć: 192.168.3.0/24"]
    
    VGW --> VPC["VPC AWS\n10.0.0.0/16"]
    
    Note1["Warszawa może kommunikować z Krakowem\nprzez VGW (hub)\nBGP routuje: 192.168.2.0/24 via VGW"]
```

**Jak CloudHub umożliwia komunikację site-to-site:**
```
BGP routing przez VGW:
  CGW1 (Warszawa) → VGW: "moja sieć 192.168.1.0/24"
  CGW2 (Kraków)   → VGW: "moja sieć 192.168.2.0/24"
  CGW3 (Wrocław)  → VGW: "moja sieć 192.168.3.0/24"
  
VGW re-advertisuje trasy do wszystkich CGW:
  CGW1 uczy się: 192.168.2.0/24 i 192.168.3.0/24 przez VGW
  CGW2 uczy się: 192.168.1.0/24 i 192.168.3.0/24 przez VGW
  CGW3 uczy się: 192.168.1.0/24 i 192.168.2.0/24 przez VGW
  
Warszawa → Kraków:
  192.168.1.50 → tunel VPN → VGW → tunel VPN → 192.168.2.100
```

**Wymagania CloudHub:**
- ✅ BGP (dynamic routing) — nie działa z static routing
- ✅ Każdy CGW musi mieć unikalne BGP ASN
- ✅ Każda lokalizacja musi mieć unikalne prefiksy CIDR (nie overlapping)
- ✅ Route Propagation włączona w VPC Route Tables

**Ograniczenia CloudHub:**
- ❌ Max 10 VPN connections per VGW
- ❌ Przepustowość ograniczona do 1.25 Gbps per tunel
- ❌ Całkowity ruch między site przez VGW = jeden punkt ruchu
- ✅ Dla większej skali → **Transit Gateway**

**CloudHub koszt:**
```
N lokalizacji × 2 tunele × $0.05/h = N × $0.10/h

5 lokalizacji × $0.10/h × 720h = $360/mc + data charges
```

---

### 10.8 Transitive Routing i VPN

**Problem transitive routing z VGW:**
```mermaid
graph LR
    VPC_A["VPC A\n10.0.0.0/16"] <-->|"VPC Peering"| VPC_B["VPC B\n10.1.0.0/16"]
    VPC_B <-->|"VGW + VPN"| ONPREM["On-premises\n192.168.0.0/16"]
    
    VPC_A -.->|"❌ NIE MOŻE\nkomunikować się z On-prem\nprzez VPC B"| ONPREM
    
    Note["VPC Peering jest non-transitive\nruch z VPC A nie może 'przejść przez' VPC B\ndo on-prem przez VGW VPC B"]
```

**Rozwiązanie transitive routing:**

```
Option 1: Osobny VGW dla każdego VPC (kosztowne)
  VPC A ma własny VGW + VPN do on-prem
  VPC B ma własny VGW + VPN do on-prem
  Dwa osobne VPN connections

Option 2: Transit Gateway (preferowane)
  TGW: VPC A + VPC B + VPN attachment
  TGW: transitive — A może komunikować się z on-prem przez TGW VPN
  
Option 3: AWS PrivateLink (tylko dla konkretnych serwisów, nie routing)
```

**TGW rozwiązuje transitive routing:**
```mermaid
graph TB
    TGW["Transit Gateway"]
    
    TGW <-->|"VPC attachment"| VPC_A["VPC A\n10.0.0.0/16"]
    TGW <-->|"VPC attachment"| VPC_B["VPC B\n10.1.0.0/16"]
    TGW <-->|"VPN attachment\n(lub DX Transit VIF)"| ONPREM["On-premises\n192.168.0.0/16"]
    
    Note["VPC A ↔ TGW ↔ VPN ↔ On-prem ✅\nVPC B ↔ TGW ↔ VPN ↔ On-prem ✅\nVPC A ↔ TGW ↔ VPC B ✅\nWszystko przez jeden TGW"]
```

---

### 10.9 AWS Client VPN

**🔴 Co to jest?**
AWS Client VPN to zarządzana usługa **remote access VPN** — umożliwia indywidualnym użytkownikom (pracownicy, laptopy) bezpieczne połączenie z VPC i on-premises z dowolnego miejsca.

> **Analogia:** Corporate VPN dla pracownika zdalnego — jak Cisco AnyConnect, Palo Alto GlobalProtect, ale zarządzany przez AWS.

**Odpowiedniki:**

| AWS | Azure | On-prem |
|-----|-------|---------|
| AWS Client VPN | Azure VPN Gateway (Point-to-Site) | Cisco AnyConnect, OpenVPN Access Server, Palo Alto GlobalProtect |

**Protokół:** OpenVPN (TCP 443 lub UDP 1194) — klient może używać:
- AWS VPN Client (darmowy)
- OpenVPN Connect Client
- Tunnelblick (macOS)
- OpenVPN GUI (Windows)

**Architektura:**
```mermaid
graph LR
    subgraph LAPTOPS["Klienci (laptopy, mobiles)"]
        L1["Pracownik Paryż\nOpenVPN client"]
        L2["Pracownik Warszawa\nAWS VPN Client"]
        L3["Pracownik Tokyo\nOpenVPN client"]
    end
    
    CVPN["AWS Client VPN Endpoint\n(zarządzany przez AWS)\ndeployment w subnecie VPC"]
    
    subgraph VPC["VPC"]
        PRIV["Private Resources\n(EC2, RDS, ECS)"]
    end
    
    ONPREM["On-premises\n(przez split-tunnel lub full-tunnel)"]
    
    L1 -->|"TLS/OpenVPN UDP 443"| CVPN
    L2 -->|"TLS/OpenVPN UDP 443"| CVPN
    L3 -->|"TLS/OpenVPN UDP 443"| CVPN
    CVPN --> PRIV
    CVPN -.->|"Jeśli split tunnel OFF:\nCały ruch przez VPN"| ONPREM
```

**Metody uwierzytelniania (Client VPN):**

| Metoda | Opis | Kiedy |
|--------|------|-------|
| **Mutual TLS (certyfikaty)** | Klient + serwer mają certyfikaty X.509 | Proste, bez integracji AD |
| **Active Directory** | Integracja z AWS Managed Microsoft AD lub AD Connector | Enterprise, istniejący AD |
| **SAML 2.0 (Federated)** | Przez Cognito, Okta, Azure AD | MFA, nowoczesny IdP |
| **Kombinacja** | Certyfikat serwera + AD/SAML | Defense in depth |

**Kluczowe cechy Client VPN:**

| Cecha | Wartość |
|-------|---------|
| Protokół | OpenVPN |
| Port | UDP 443 (lub TCP 443, UDP 1194) |
| Maks. concurrent connections | 20 000 |
| Client CIDR | Zakres IP przydzielany klientom (poza VPC CIDR!) |
| Split tunnel | ✅ Tylko ruch do VPC przez VPN, reszta przez lokalny ISP |
| Full tunnel | ✅ Cały ruch przez VPN (prywatność, compliance) |
| Autoryzacja | Authorization rules (CIDR + AD grup) |
| Logs | CloudWatch Logs (połączenia, disconnect) |
| RADIUS | ❌ (nie bezpośrednio — przez AD) |

**Split Tunnel vs Full Tunnel:**
```
Split Tunnel (domyślny tryb):
  Ruch do 10.0.0.0/16 (VPC) → przez VPN
  Ruch do google.com, youtube.com → przez lokalny ISP
  
  Korzyść: Mniej ruchu przez VPN = lepsza wydajność, mniejszy koszt
  Ryzyko: Internet ruch nie jest chroniony przez firmowy firewall

Full Tunnel (split tunnel OFF):
  Cały ruch → przez VPN → przez VPC → przez NAT GW → Internet
  
  Korzyść: Pełna widoczność i kontrola ruchu (DLP, proxy)
  Ryzyko: Wyższy koszt NAT GW, wyższe latency dla użytkownika
```

**Authorization Rules:**
```
Reguła 1: 10.0.0.0/16 → Allow dla grupy AD "VPN-All-Employees"
Reguła 2: 10.0.1.0/24 → Allow dla grupy AD "VPN-Admins" (private admin subnet)
Reguła 3: 192.168.0.0/16 → Allow dla grupy AD "VPN-OnPrem-Users" (on-prem sieć)

Pracownik z grupy "VPN-All-Employees" ma dostęp do 10.0.0.0/16 ale NIE do admin subnetu
Admin z grupy "VPN-Admins" ma dostęp do wszystkiego (jest też w "VPN-All-Employees")
```

**Client VPN koszty:**
```
Endpoint association: $0.10/h per subnet association
Connection: $0.05/h per active connection

Przykład: 100 pracowników × 8h/day × 22 dni = 17 600 godzin × $0.05 = $880/mc
          + 2 subnety × $0.10 × 720h = $144/mc
          Razem: ~$1024/mc
```

**Client VPN + SAML (MFA):**
```mermaid
sequenceDiagram
    participant USER as Pracownik (laptop)
    participant CLIENT as AWS VPN Client
    participant CVPN as Client VPN Endpoint
    participant IDP as IdP (Okta/Azure AD)

    USER->>CLIENT: Połącz z VPN
    CLIENT->>CVPN: TLS connection initiation
    CVPN->>USER: Redirect do IdP (SAML)
    USER->>IDP: Login + MFA (TOTP/Push)
    IDP-->>CVPN: SAML Assertion
    CVPN-->>CLIENT: VPN Connected + IP z Client CIDR
    CLIENT-->>USER: Połączono 🔒
```

---

### 10.10 Porównanie rozwiązań VPN w AWS

**Tabela porównawcza wszystkich opcji:**

| Kryterium | Site-to-Site VPN (VGW) | Site-to-Site VPN (TGW) | Client VPN | Direct Connect |
|-----------|----------------------|----------------------|------------|----------------|
| **Użytkownik** | Sieć on-prem → VPC | Sieć on-prem → wiele VPC | Pracownik zdalny | Sieć on-prem → VPC |
| **Protokół** | IPSec (IKEv2) | IPSec (IKEv2) | OpenVPN/TLS | Fizyczna linia |
| **Przepustowość** | 1.25 Gbps/tunel | 1.25 Gbps/tunel (ECMP→5 Gbps) | Do 10 Gbps per endpoint | 1/10/100 Gbps |
| **Latency** | Variable (Internet) | Variable (Internet) | Variable | Consistent, niski |
| **HA** | 2 tunele auto | 2 tunele auto + ECMP | Multi-AZ deployment | Dual DX + VPN backup |
| **Transitive** | ❌ | ✅ | ❌ (per VPC) | ❌ (VGW) / ✅ (TGW) |
| **Koszt setup** | Niski | Niski | Niski | Wysoki ($$$) |
| **Koszt/mc** | ~$36 (1 connection) | ~$36 + TGW | ~$72 + $0.05/connection/h | $100s-$1000s |
| **Routing** | Static lub BGP | Static lub BGP | OpenVPN (push routes) | BGP |
| **Use case** | Jedno VPC + on-prem | Multi-VPC + on-prem | Remote workers | Enterprise, high bandwidth |

**Drzewo decyzyjne:**
```mermaid
flowchart TD
    Q1{Kto się łączy?} -->|"Pracownik zdalny\n(laptop/mobile)"| CVPN["Client VPN\n(OpenVPN)"]
    Q1 -->|"Sieć on-prem\n(office/DC)"| Q2

    Q2{Ile VPC?} -->|"1 VPC"| Q3
    Q2 -->|"Wiele VPC lub\nfuture growth"| Q4

    Q3{Wymagana przepustowość?} -->|"<1.25 Gbps\nzmienny latency OK"| VGW_VPN["Site-to-Site VPN\n(VGW)\n$36/mc"]
    Q3 -->|">1 Gbps\nstały latency\ncritical workload"| DX["Direct Connect\n+ VPN backup"]

    Q4{Transitive routing?} -->|"Tak (wiele VPC\ndo on-prem)"| TGW_VPN["Site-to-Site VPN\n(TGW)\n$36 + TGW koszty"]
    Q4 -->|"Nie (proste połączenie)"| VGW_VPN

    DX --> DX_COST{Budget?}
    DX_COST -->|"Enterprise"| DX_FULL["Direct Connect\nPrimary + VPN Backup"]
    DX_COST -->|"Cost-sensitive"| VGW_VPN
```

---

### 10.11 Well-Architected Framework

| Filar | Private DNS / PHZ | Site-to-Site VPN | BGP/Route Propagation | Client VPN | VPN Failover |
|-------|-------------------|-----------------|----------------------|------------|--------------|
| **Operational Excellence** | Split-horizon eliminuje konfigurację per-środowisko. IaC: Route 53 PHZ jako Terraform resource | AWS generuje konfigurację CGW dla każdego urządzenia — mniej ręcznej pracy | Route Propagation = zero ręcznych tras. BGP automatycznie uczy się zmieniającej się topologii | Centralne zarządzanie przez AWS Console/API. CloudWatch Logs dla audit | DPD i BGP automatycznie wykrywają i przełączają — brak manualnej interwencji |
| **Security** | PHZ nie eksponuje wewnętrznych nazw do Internetu. `SourceVpce` w Bucket Policy. Route 53 Resolver Firewall: blokuj malware domains | IPSec ESP szyfruje dane w tranzycie. IKEv2 + certyfikaty > pre-shared key. Ograniczaj CGW do konkretnych IP w NACL | Validate BGP prefixes (prefix-list/route-map) — zapobiega prefix hijacking. Ogranicz co CGW może ogłaszać | MFA przez SAML/AD + certyfikat serwera. Authorization Rules = least privilege. Logs wszystkich połączeń | Dual ISP + dual VPN = eliminate ISP SPOF. Monitor tunel status w CloudWatch |
| **Reliability** | PHZ multi-VPC association. Route 53 Resolver: multi-AZ wbudowane (managed). Inbound/Outbound Endpoints per AZ | 2 tunele per VPN connection. Dual ISP pattern: VPN Connection 1 (ISP1) + VPN Connection 2 (ISP2) | BGP automatic failover: DX primary + VPN backup. RPO/RTO: failover ~30-90s (lub <1s z BFD na DX) | Multi-AZ: deploy Client VPN endpoint w wielu subnetach. AWS zarządza HA endpointu | BFD na DX dla sub-second failover. DPD na VPN. Monitor BGP state w CloudWatch |
| **Performance Efficiency** | Route 53 Resolver: <1ms latency DNS w VPC. Zonal DNS dla Interface Endpoints = brak cross-AZ latency | ECMP przez TGW: agreguj 4 tunele = 5 Gbps. Accelerated VPN przez Global Accelerator dla lepszej trasy | BGP AS_PATH optimization: krótszy path = szybszy routing. MED optimization dla asymmetric routing | Split Tunnel: tylko VPC ruch przez VPN = user experience ↑. Full Tunnel: +latency ale security ↑ | Accelerated VPN + TGW dla enterprise workloads |
| **Cost Optimization** | Route 53 PHZ: $0.50/mc. Resolver Endpoints: $0.125/h per endpoint. Nie twórz duplikatów PHZ | VPN: $36/mc per connection. TGW adds cost but reduces Peering mesh. Evaluate DX ROI przy >TB/mc traffic | BGP = bez dodatkowego kosztu vs static. Static routing dla prostych setupów (mniej zarządzania) | $0.05/h per connection: optimize przez session limits, disconnect idle clients. Split tunnel = mniej NAT GW costs | Dual VPN ($72/mc) vs DX ($100s) — VPN jako koszt-efektywny backup |
| **Sustainability** | DNS w VPC = krótkie ścieżki, brak roundtrip. Resolwowanie nazw lokalnie zamiast przez Internet | IPSec szyfrowanie na sprzęcie (CGW) = dedykowany hardware, efektywny. Accelerated VPN: krótsze trasy przez AWS edge | BGP dynamic failover: nie utrzymuj aktywnych połączeń jeśli niepotrzebne | Split tunnel: mniej ruchu przez VPN = mniejsze zużycie energii sieci. Connection timeout dla idle = less compute | Active/passive VPN: tylko 1 aktywny tunel przy normalnej pracy = mniej energii |

---

### 10.12 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Design Solutions for Organizational Complexity

**Scenariusz:** Firma globalna ma 8 biur: Warszawa, Kraków, Berlin, Paryż, Londyn, Nowy Jork, Tokio, Sydney. Każde biuro musi komunikować się z centralnym VPC AWS i ze sobą. Jak zaprojektować?

```
Rozwiązanie: TGW + Site-to-Site VPN (CloudHub przez TGW)

Transit Gateway (TGW):
  Region: eu-west-1 (centralny)
  
  Per biuro: 1 VPN Connection do TGW
    - Warszawa: CGW1 (ISP główny) + CGW1b (ISP backup) = 2 VPN Connections
    - Kraków: CGW2 + CGW2b
    - ...8 biur × 2 ISP = 16 VPN Connections
    
  TGW Route Table "Biura":
    192.168.1.0/24 → vpn-attachment-warsaw
    192.168.2.0/24 → vpn-attachment-krakow
    ...
    
  TGW Route Table "VPC":
    10.0.0.0/16 → vpc-attachment
    
  Route Propagation: BGP → każde biuro ogłasza swoje prefiksy
  
Komunikacja biuro-biuro przez TGW (transitive routing):
  Warszawa → TGW → Kraków ✅
  Berlin → TGW → Tokio ✅

DNS:
  DHCP Option Set → AmazonProvidedDNS
  Route 53 Resolver Outbound: *.corp.local → on-prem DNS biur
  Route 53 Resolver Inbound: biura → AWS internal names
  
Client VPN: pracownicy zdalni → Client VPN Endpoint → TGW → biura lub VPC
```

#### 🆕 Domain 2: Design for New Solutions

**Scenariusz:** Nowy startup, 20 developerów zdalnych, jeden VPC AWS. Budget tight. Jak zapewnić bezpieczny zdalny dostęp bez on-prem infrastruktury?

```
Rozwiązanie: Client VPN + Cognito/SAML MFA

AWS Client VPN Endpoint:
  Deployment: 2 subnety (2 AZ) w VPC
  Autentykacja: SAML 2.0 → Okta (lub Cognito User Pool)
  MFA: Okta Push / TOTP (przez SAML)
  
  Client CIDR: 172.16.0.0/22 (poza VPC CIDR)
  Split Tunnel: ON (tylko VPC ruch przez VPN)
  
  Authorization Rules:
    10.0.0.0/16 → ALL (wszyscy developerzy mają dostęp do VPC)
    10.0.1.0/24 → grupa "admins" (admin subnet tylko dla adminów)
  
  DNS: Push przez VPN: corp.internal → Route 53 Resolver
  
Koszty:
  20 dev × 8h/day × 22dni = 3520h × $0.05 = $176/mc
  + 2 subnet associations × $0.10 × 720h = $144/mc
  Razem: ~$320/mc (vs Bastion Host: EC2 + zarządzanie)
```

#### 📈 Domain 3: Continuous Improvement for Existing Solutions

**Scenariusz:** Firma ma jeden Site-to-Site VPN (VGW, static routing). Narzekają na: brak failover, ręczne zarządzanie tras, nowy VPC wymaga nowego VPN.

**Migracja:**
```
Faza 1: Static → BGP
  - Zaktualizuj VPN Connection: StaticRoutesOnly = false
  - CGW: skonfiguruj BGP session (ASN, neighbor IP)
  - Włącz Route Propagation w Route Tables
  - Test: czy trasy pojawiły się automatycznie?

Faza 2: Dodaj drugi VPN Connection (HA)
  - CGW2 przez drugi ISP (lub ten sam CGW ale inny IP)
  - VPN Connection 2 do VGW
  - BGP: CGW2 ogłasza te same prefiksy z niższym LOCAL_PREF
  - Test: wyłącz Tunel 1 → czy ruch failover na Tunel 2?

Faza 3: VGW → TGW
  - Utwórz TGW
  - Przenieś VPN Connection z VGW na TGW
  - Zaktualizuj Route Tables: vgw-xxx → tgw-xxx
  - TGW umożliwia dodawanie nowych VPC bez nowych VPN
  
Faza 4: DPD i monitoring
  - CloudWatch alarm: VPN TunnelState < 1 → SNS alert
  - CGW: DPD interval 10s, timeout 30s
```

#### 🚀 Domain 4: Accelerate Workload Migration and Modernization

**Scenariusz:** Firma migruje z on-prem do AWS w 18 miesięcy. Na razie: 500 serwerów on-prem, 100 już w AWS. Potrzeba: bidirectional connectivity (on-prem ↔ AWS), przyszły full cutover.

```
Faza 1 (migracja — rok 1):
  Direct Connect 1 Gbps (primary dla batch transfer/migracji)
  + Site-to-Site VPN (backup)
  
  TGW:
    VPC Prod + VPC Dev + VPC Migration + DX Transit VIF + VPN
    
  DNS hybrid:
    Route 53 Inbound Endpoint: on-prem → resolve AWS names
    Route 53 Outbound Endpoint: AWS → resolve on-prem names
    
Faza 2 (stabilizacja — rok 1-2):
  Zwiększ DX do 10 Gbps (coraz więcej workloadów w AWS)
  Client VPN dla pracowników (off bastion host)
  
Faza 3 (cutover — rok 2):
  Wszystko w AWS
  VPN + DX: zredukuj do DX only (tańsze per GB)
  lub: całkowita rezygnacja z DX po migracji (jeśli nie ma on-prem)
  Client VPN: nadal dla zdalnych pracowników

BGP communities dla kontroli:
  On-prem BGP advertises:
    192.168.0.0/16 (całość) → VPN
    192.168.0.0/16 (całość) → DX
  AWS preferuje DX (wyższy LOCAL_PREF)
  Migracja serwisów: stopniowo zawęź ogłaszane prefiksy z on-prem
```

---

## 11. Hyperplane, Direct Connect, DX Gateway, Transit Gateway Deep Dive, TGW vs Azure Virtual Hub

### 11.1 Hyperplane — AWS Internal Network Fabric

**Co to jest Hyperplane?**
Hyperplane to **wewnętrzna sieć wirtualna AWS** — stateful forwarding engine który stoi za wieloma managed services. Nie jest konfigurowany bezpośrednio przez klientów — to architektura wewnętrzna AWS.

**Gdzie Hyperplane działa:**
```
Usługi zbudowane na Hyperplane:
  → Interface VPC Endpoints (PrivateLink)
  → NAT Gateway
  → Network Load Balancer (NLB)
  → AWS Transit Gateway (częściowo)
  → Security Groups (stateful tracking)
  → VPC Flow Logs aggregation

Cechy Hyperplane:
  → Horizontal scaling: nodes bez centralnego SPOF
  → Consistent low latency (single-digit ms)
  → Stateful connection tracking (SYN/ACK/FIN)
  → Zonal deployment: węzły per AZ dla HA
```

**Dlaczego ważny dla egzaminu:**
> Pytania o "jak skaluje NLB/NAT GW/PrivateLink" → odpowiedź: Hyperplane umożliwia auto-scale bez downtime, bez NAT state loss. NLB może skalować do milionów req/sec właśnie przez Hyperplane.

---

### 11.2 Direct Connect — Fizyczna Infrastruktura

**🔵 Co to jest Direct Connect?**
AWS Direct Connect to **dedykowane, prywatne połączenie sieciowe** między Twoim data center (lub biurem) a AWS — przez fizyczną linię, bez publicznego Internetu.

```mermaid
graph LR
    subgraph YOUR_DC["Twoje Data Center"]
        ROUTER["Twój Router\n(Cisco/Juniper/etc.)"]
    end
    
    subgraph LAST_MILE["Last Mile (Telco)"]
        FIBER["Światłowód\nod Telco/Partner"]
    end
    
    subgraph DX_LOC["DX Location (Colocation)"]
        XCROSS["X-Connect\n(patch panel połączenie\nw DX cage)"]
        AWS_DX["AWS Direct Connect\nRouter / Port"]
    end
    
    subgraph AWS["AWS Region"]
        VGW["VGW / DX GW / TGW"]
    end
    
    ROUTER -->|"Dedykowana linia\n1G/10G/100G"| FIBER
    FIBER -->|"Last Mile\ndo DX Location"| XCROSS
    XCROSS -->|"X-Connect w cage"| AWS_DX
    AWS_DX -->|"AWS backbone"| VGW
```

#### Last Mile

**Last Mile** = połączenie od Twojego obiektu do Direct Connect Location (DX Location). Zwykle realizowane przez:
- Telco (Orange, T-Systems, Comarch w Polsce)
- Network Service Provider (NTT, Equinix, Colt, Lumen)
- Własny kabel (jeśli jesteś w tym samym colo co DX Location)

```
Twoja odpowiedzialność: Last Mile (zamówić u providera)
AWS odpowiedzialność: DX Location port + AWS backbone

Last Mile może mieć latency kilka-kilkadziesiąt ms (zależy od odległości)
DX backbone: <1ms (AWS-managed fiber)
```

#### X-Connect (Cross-Connect)

**X-Connect** (Cross-Connect) = fizyczne połączenie patch kablem w DX Location między Twoim sprzętem (lub providera) a portem AWS.

```
DX Location (np. Equinix AM3 Amsterdam):
  AWS cage:          AWS DX router, port 10G
  Twój cage:         Twój router lub partner router
  X-Connect:         Kabel łączący oba cage w tym samym budynku
  
Zamówienie X-Connect:
  Option A: Sam masz sprzęt w DX Location → zamów X-Connect u operatora colo
  Option B: Używasz DX Partner (np. Colt) → partner zamawia X-Connect w Twoim imieniu
```

#### DX Connection

**DX Connection** = zasób AWS reprezentujący fizyczne połączenie 1:1 do portu w DX Location.

```bash
# Zamówienie DX Connection
aws directconnect create-connection \
  --connection-name "warsaw-to-aws-eu-west-1" \
  --bandwidth "10Gbps" \                    # 1Gbps, 10Gbps, 100Gbps
  --location "EqAM3" \                      # Equinix Amsterdam
  --provider-name "Colt"                    # opcjonalnie: przez partnera

# Stany DX Connection:
# requested → pending → available → deleted

# Po zamówieniu AWS daje:
#   LOA-CFA (Letter of Authorization - Connecting Facility Assignment)
#   → przekazujesz do operatora colo i providera Last Mile
```

**DX Bandwidth options:**

| Bandwidth | Typ | Uwagi |
|-----------|-----|-------|
| 1 Gbps | Dedicated | Direct od AWS |
| 10 Gbps | Dedicated | Direct od AWS |
| 100 Gbps | Dedicated | Direct od AWS (wybrane DX Locations) |
| 50/100/200/300/400/500 Mbps | Hosted | Przez DX Partner |
| 1/2/5/10 Gbps | Hosted | Przez DX Partner |

**Dedicated vs Hosted Connection:**

| Cecha | Dedicated | Hosted |
|-------|-----------|--------|
| Zamówienie | Bezpośrednio u AWS | Przez APN Partner |
| LOA-CFA | Od AWS bezpośrednio | Partner zarządza |
| Czas instalacji | 4-12 tygodni | Zależy od partnera |
| Vlan granularność | 1 link = wiele VIFs | Hosted VIF = 1 VIF per connection |
| Min bandwidth | 1 Gbps | 50 Mbps |
| Shared physical port | ❌ (dedykowany) | ✅ (Partner multipleksuje) |

---

### 11.3 Virtual Interface (VIF)

**Virtual Interface (VIF)** = logiczne połączenie BGP nad fizycznym DX Connection. Jeden DX Connection może mieć **wiele VIF** (każdy to osobny VLAN + BGP session).

**Typy VIF:**

```mermaid
graph TB
    DX_CON["DX Connection\n10 Gbps\n(fizyczny port)"]
    
    DX_CON -->|"VLAN 100"| PVIF["Private VIF\nDostęp do VPC przez VGW\nBGP: prywatne prefiksy"]
    DX_CON -->|"VLAN 200"| PUBVIF["Public VIF\nDostęp do publicznych\nAWS endpointów (S3, SQS...)\nBGP: publiczne AWS prefiksy"]
    DX_CON -->|"VLAN 300"| TVIF["Transit VIF\nDostęp do TGW → wiele VPC\nBGP: przez DX Gateway"]
    
    PVIF --> VGW["VGW\n(per VPC)"]
    PUBVIF --> AWS_PUB["AWS Public Services\n(bez Internet)"]
    TVIF --> DXGW["DX Gateway\n→ TGW → wiele VPC"]
```

**Private VIF:**
```
Łączy on-prem z jednym VPC przez VGW.
BGP: ogłaszasz prywatne IP on-prem, AWS ogłasza VPC CIDR.
VLAN: 802.1Q tag (1-4094).
MTU: 1500 (domyślny) lub 9001 (Jumbo Frames - włącz per VIF).
BGP ASN: on-prem własny (65000) ↔ VGW ASN (64512).
Limit: 1 Private VIF per VGW (nie można podłączyć wielu Private VIF do tego samego VGW).
```

**Public VIF:**
```
Dostęp do publicznych AWS endpointów (S3, SQS, SNS, API GW, EC2 public IP...) przez DX
zamiast przez Internet.
BGP: AWS ogłasza publiczne prefiksy (~50 000 CIDR) → On-prem router uczy się ich → ruch do S3 
przez DX zamiast Internet.
Użycie: batch transfer do S3 z on-prem z przewidywalną przepustowością.
```

**Transit VIF:**
```
Łączy DX Connection z Direct Connect Gateway (DX GW) → TGW → wiele VPC.
Wymagany dla: multi-VPC, multi-region, multi-account z DX.
Limit: 1 Transit VIF per DX Connection (per DX GW).
BGP: bardziej złożone — przez DX GW jako intermediate.
```

**Porównanie VIF:**

| Cecha | Private VIF | Public VIF | Transit VIF |
|-------|-------------|------------|-------------|
| Cel | Jeden VPC | Publiczne AWS serwisy | Wiele VPC/regionów |
| Przez | VGW | - (bezpośrednio) | DX Gateway → TGW |
| BGP prefix | Prywatne IP | Publiczne AWS | Prywatne IP |
| Jumbo Frames | ✅ 9001 MTU | ❌ | ✅ 9001 MTU |
| Ile per DX con. | Wiele | Wiele | 1 per DX GW |
| HA | 2 DX Connections | 2 DX Connections | 2 DX Connections |

---

### 11.4 Direct Connect Gateway (DX Gateway)

**🟡 Co to jest?**
DX Gateway to **globalny** (nie regionalny) zasób AWS umożliwiający połączenie jednego DX Connection z **wieloma VPC w różnych regionach AWS** lub z **Transit Gateway**.

```mermaid
graph TB
    subgraph ON_PREM["On-premises DC"]
        ROUTER["Router on-prem\nASN 65000"]
    end
    
    DX_CON["DX Connection\n10 Gbps\nEU-WEST-1 DX Location"]
    TVIF["Transit VIF\nVLAN 300"]
    
    DXGW["Direct Connect Gateway\n(GLOBALNY - nie regionalny)\nASN 64512"]
    
    TGW_EU["TGW eu-west-1"]
    TGW_US["TGW us-east-1"]
    TGW_AP["TGW ap-southeast-1"]
    
    VPC_A["VPC A\neu-west-1"]
    VPC_B["VPC B\neu-west-1"]
    VPC_C["VPC C\nus-east-1"]
    VPC_D["VPC D\nap-southeast-1"]
    
    ROUTER --> DX_CON --> TVIF --> DXGW
    
    DXGW <-->|"DX GW Association"| TGW_EU
    DXGW <-->|"DX GW Association"| TGW_US
    DXGW <-->|"DX GW Association"| TGW_AP
    
    TGW_EU --> VPC_A
    TGW_EU --> VPC_B
    TGW_US --> VPC_C
    TGW_AP --> VPC_D
    
    ON_PREM -.->|"przez DX GW\ndo wszystkich VPC\nbez osobnych połączeń"| VPC_D
```

**Kluczowe cechy DX Gateway:**

| Cecha | Wartość |
|-------|---------|
| Scope | **Global** (nie regionalny) — jeden DX GW, wiele regionów |
| Maks. VGW associations | 10 per DX GW |
| Maks. TGW associations | 3 per DX GW |
| ASN | 64512 (konfigurowalne) |
| Cross-account | ✅ — DX GW w jednym koncie, VGW/TGW w innych |
| Koszt | Brak dodatkowego kosztu za DX GW (płacisz za VIF i dane) |

**DX Gateway z Private VIF (nie Transit VIF):**
```bash
# Utwórz DX Gateway
aws directconnect create-direct-connect-gateway \
  --direct-connect-gateway-name "my-dxgw" \
  --amazon-side-asn 64512

# Asocjuj VGW z DX Gateway
aws directconnect create-direct-connect-gateway-association \
  --direct-connect-gateway-id dxgw-xxx \
  --gateway-id vgw-xxx                  # VGW w dowolnym regionie

# Teraz Private VIF może używać DX GW zamiast bezpośrednio VGW
# → On-prem → DX → DX GW → VGW VPC-A (eu-west-1) + VGW VPC-B (us-east-1)
```

**Ograniczenia DX Gateway:**
- ❌ **Brak komunikacji VPC-to-VPC przez DX GW** — DX GW służy TYLKO do on-prem ↔ VPC (nie VPC ↔ VPC)
- ❌ Nie można asocjować overlapping CIDR VPC do tego samego DX GW
- ❌ Ruch VPC A → VPC B przez DX GW → on-prem → DX GW → VPC B jest możliwy ale nieefektywny (trombone routing)

---

### 11.5 Transit Gateway (TGW) — Deep Dive

**🔴 Co to jest?**
AWS Transit Gateway to **regionalny, zarządzany router** który działa jako centralny hub dla połączeń między VPC, VPN, Direct Connect i innymi TGW (peering).

> **Analogia:** TGW to jak **zarządzany L3 switch/router** w centrum data center — wszystko podłącza się do niego, on zarządza routingiem.

**Bez TGW (mesh peering — O(n²)):**
```
5 VPC: 5×4/2 = 10 VPC Peering connections
10 VPC: 10×9/2 = 45 VPC Peering connections
→ każda para potrzebuje osobnego routingu, Security Groups, zarządzania
```

**Z TGW (hub-and-spoke — O(n)):**
```
5 VPC: 5 TGW attachments
10 VPC: 10 TGW attachments
100 VPC: 100 TGW attachments
→ jeden punkt zarządzania routingiem
```

#### TGW Attachment

**Attachment** = podłączenie zasobu do TGW.

**Typy Attachments:**

| Typ | Opis | Limit per TGW |
|-----|------|---------------|
| **VPC Attachment** | Podłączenie VPC (subnety per AZ) | 5000 total |
| **VPN Attachment** | Site-to-Site VPN (IPSec) | 5000 total |
| **DX Gateway Attachment** | Direct Connect przez DX GW | 20 per region |
| **TGW Peering Attachment** | TGW-to-TGW (intra lub inter-region) | 50 per TGW |
| **Connect Attachment** | SD-WAN/appliance przez GRE + BGP | - |

**VPC Attachment — szczegóły:**
```bash
# Utwórz VPC Attachment
aws ec2 create-transit-gateway-vpc-attachment \
  --transit-gateway-id tgw-xxx \
  --vpc-id vpc-xxx \
  --subnet-ids subnet-aaa subnet-bbb subnet-ccc \  # po 1 subnet per AZ
  --options "ApplianceModeSupport=enable"          # dla sticky routing przez appliance
  
# Subnet wymagania:
#   Jeden subnet per AZ (TGW ENI umieszczone w tym subnecie)
#   Subnet musi mieć IP do alokacji dla TGW ENI
#   /28 = minimum (14 IP do alokacji)
```

**TGW Routing — jak działa:**
```mermaid
graph TB
    TGW["Transit Gateway"]
    
    subgraph ATTACH["Attachments"]
        VPC_A["VPC A\n10.0.0.0/16"]
        VPC_B["VPC B\n10.1.0.0/16"]
        VPC_C["VPC C\n10.2.0.0/16"]
        VPN["VPN\n192.168.0.0/16"]
    end
    
    TGW <-->|"attachment-aaa"| VPC_A
    TGW <-->|"attachment-bbb"| VPC_B
    TGW <-->|"attachment-ccc"| VPC_C
    TGW <-->|"attachment-vpn"| VPN
    
    TGW_RT["TGW Route Table\n(default)\n10.0.0.0/16 → attachment-aaa\n10.1.0.0/16 → attachment-bbb\n10.2.0.0/16 → attachment-ccc\n192.168.0.0/16 → attachment-vpn"]
    TGW --> TGW_RT
```

---

### 11.6 TGW Route Tables — Segmentacja

**TGW Route Table** = tabela routingu dla TGW (analogia do VPC Route Table, ale dla attachments).

**Każdy attachment ma dwie relacje z Route Table:**
1. **Association** — "z której RT sprawdzam routing gdy pakiet przychodzi z tego attachmentu?"
2. **Propagation** — "do której RT propagować (ogłaszać) moje trasy?"

```
Domyślnie: 1 RT dla wszystkich (pełna komunikacja)

Zaawansowanie: wiele RT = segmentacja (np. Prod nie widzi Dev)
```

**Scenariusz: Segmentacja Prod/Dev/Shared:**
```
TGW Route Tables:
  RT-Prod:
    Associations: VPC-Prod, VPN-Prod
    Propagations przyjęte od: VPC-Prod, VPN-Prod, VPC-Shared
    Routes:
      10.0.0.0/16 → VPC-Prod
      192.168.1.0/24 → VPN-Prod
      10.2.0.0/16 → VPC-Shared
    (brak VPC-Dev → Prod nie może komunikować się z Dev)

  RT-Dev:
    Associations: VPC-Dev, VPN-Dev
    Propagations przyjęte od: VPC-Dev, VPN-Dev, VPC-Shared
    Routes:
      10.1.0.0/16 → VPC-Dev
      192.168.2.0/24 → VPN-Dev
      10.2.0.0/16 → VPC-Shared
    (brak VPC-Prod → Dev nie może komunikować się z Prod)

  RT-Shared:
    Associations: VPC-Shared
    Propagations przyjęte od: VPC-Shared
    Routes:
      10.2.0.0/16 → VPC-Shared
    (Shared nie wie o Prod/Dev → Shared nie może inicjować do Prod/Dev)
    (Ale może odpowiadać → bo routing jest per-packet, nie per-session w TGW)
```

**Diagram segmentacji:**
```mermaid
graph TB
    TGW["Transit Gateway"]
    
    VPC_PROD["VPC Prod\n10.0.0.0/16"] <-->|"attachment\nassoc: RT-Prod\nprop → RT-Prod"| TGW
    VPC_DEV["VPC Dev\n10.1.0.0/16"] <-->|"attachment\nassoc: RT-Dev\nprop → RT-Dev"| TGW
    VPC_SHARED["VPC Shared\n10.2.0.0/16"] <-->|"attachment\nassoc: RT-Shared\nprop → RT-Prod + RT-Dev"| TGW
    
    RT_PROD["RT-Prod\n10.0.0.0/16 local\n10.2.0.0/16 → Shared\n❌ no Dev route"]
    RT_DEV["RT-Dev\n10.1.0.0/16 local\n10.2.0.0/16 → Shared\n❌ no Prod route"]
    RT_SHARED["RT-Shared\n10.2.0.0/16 local"]
    
    TGW --- RT_PROD
    TGW --- RT_DEV
    TGW --- RT_SHARED
```

**Statyczne trasy w TGW RT:**
```bash
# Dodaj statyczną trasę do TGW RT
aws ec2 create-transit-gateway-route \
  --transit-gateway-route-table-id tgw-rtb-xxx \
  --destination-cidr-block 0.0.0.0/0 \
  --transit-gateway-attachment-id tgw-attach-firewall  # ruch przez firewall VPC

# Blackhole route (odrzuć ruch)
aws ec2 create-transit-gateway-route \
  --transit-gateway-route-table-id tgw-rtb-xxx \
  --destination-cidr-block 10.99.0.0/16 \
  --blackhole
```

**Inspection VPC pattern (centralized firewall):**
```mermaid
graph TB
    VPC_A["VPC A"] -->|"ruch do VPC B"| TGW
    VPC_B["VPC B"] -->|"ruch do VPC A"| TGW
    TGW -->|"statyczna trasa 0.0.0.0/0\n→ Inspection VPC"| VPC_INSPECT["Inspection VPC\n(AWS Network Firewall\nlub Palo Alto NGFW)"]
    VPC_INSPECT -->|"po inspekcji:\nforward lub drop"| TGW
    
    Note["TGW RT:
    0.0.0.0/0 → inspection-attachment
    10.0.0.0/8 → inspection-attachment
    Appliance Mode: ON (same ENI dla request/response)"]
```

---

### 11.7 TGW Peering

**TGW Peering** = połączenie dwóch TGW (intra-region lub inter-region).

```mermaid
graph LR
    subgraph EU["Region eu-west-1"]
        TGW_EU["TGW EU\ntgw-aaa"]
        VPC_EU1["VPC A\n10.0.0.0/16"]
        VPC_EU2["VPC B\n10.1.0.0/16"]
        TGW_EU --- VPC_EU1
        TGW_EU --- VPC_EU2
    end
    
    subgraph US["Region us-east-1"]
        TGW_US["TGW US\ntgw-bbb"]
        VPC_US1["VPC C\n172.16.0.0/16"]
        VPC_US2["VPC D\n172.17.0.0/16"]
        TGW_US --- VPC_US1
        TGW_US --- VPC_US2
    end
    
    TGW_EU <-->|"TGW Peering Attachment\n(AWS backbone — nie publiczny Internet)"| TGW_US
    
    Note["Trasy statyczne (nie BGP!) w TGW RT:
    EU RT: 172.16.0.0/14 → peering-attachment-tgw-us
    US RT: 10.0.0.0/8 → peering-attachment-tgw-eu"]
```

**Kluczowe cechy TGW Peering:**

| Cecha | Wartość |
|-------|---------|
| Routing | **Statyczne trasy** (nie BGP) — ręczne dodawanie tras |
| Encryption | ✅ Dane szyfrowane przez AWS backbone |
| Bandwidth | Brak limitu (AWS backbone) |
| Latency | Zależy od odległości regionów |
| Cross-account | ✅ — RAM do share TGW |
| Transitivity | ❌ — TGW A ↔ TGW B ↔ TGW C, ale A nie widzi C (przez B) |
| Koszt | $0.05/GB (inter-region data transfer) + TGW attachment hour |

```bash
# Utwórz TGW Peering Attachment (inicjator)
aws ec2 create-transit-gateway-peering-attachment \
  --transit-gateway-id tgw-aaa \              # lokalny TGW (eu-west-1)
  --peer-transit-gateway-id tgw-bbb \         # zdalny TGW (us-east-1)
  --peer-region us-east-1

# Akceptacja po stronie zdalnej
aws ec2 accept-transit-gateway-peering-attachment \
  --transit-gateway-attachment-id tgw-attach-yyy \
  --region us-east-1

# Dodaj statyczne trasy (manualne!)
aws ec2 create-transit-gateway-route \
  --transit-gateway-route-table-id tgw-rtb-eu \
  --destination-cidr-block 172.16.0.0/14 \    # sieci US
  --transit-gateway-attachment-id tgw-attach-peering
```

---

### 11.8 TGW i Resource Access Manager (RAM)

**Problem:** TGW i VPC w różnych kontach AWS.
**Rozwiązanie:** AWS RAM (Resource Access Manager) — share TGW cross-account.

```mermaid
graph TB
    subgraph NETWORK_ACCOUNT["Network Account (111111111)"]
        TGW["Transit Gateway\ntgw-central"]
        RAM_SHARE["RAM Share\nResource: TGW\nPrincipal: OU lub konto"]
    end
    
    subgraph APP_ACCOUNT_A["App Account A (222222222)"]
        VPC_A["VPC A"]
        ATTACH_A["TGW Attachment\n(tworzy w swoim koncie)"]
    end
    
    subgraph APP_ACCOUNT_B["App Account B (333333333)"]
        VPC_B["VPC B"]
        ATTACH_B["TGW Attachment\n(tworzy w swoim koncie)"]
    end
    
    TGW -->|"RAM Share"| RAM_SHARE
    RAM_SHARE -->|"Principal: konto 222"| APP_ACCOUNT_A
    RAM_SHARE -->|"Principal: konto 333"| APP_ACCOUNT_B
    
    VPC_A --> ATTACH_A -->|"attach do TGW (konto 111)"| TGW
    VPC_B --> ATTACH_B -->|"attach do TGW (konto 111)"| TGW
```

**Jak działa RAM + TGW:**
```bash
# Network Account: utwórz RAM Share dla TGW
aws ram create-resource-share \
  --name "tgw-central-share" \
  --resource-arns arn:aws:ec2:eu-west-1:111111111:transit-gateway/tgw-central \
  --principals "arn:aws:organizations::root:ou/o-xxx/ou-xxx-yyy"  # cały OU
  # lub: "222222222"  # konkretne konto

# App Account A: utwórz VPC Attachment do TGW (który jest w innym koncie)
aws ec2 create-transit-gateway-vpc-attachment \
  --transit-gateway-id tgw-central \           # ID TGW z Network Account
  --vpc-id vpc-app-a \
  --subnet-ids subnet-aaa subnet-bbb
# Attachment tworzony w App Account A, ale rejestrowany w TGW sieci Network Account

# Network Account: zaakceptuj attachment (jeśli nie auto-accept)
aws ec2 accept-transit-gateway-vpc-attachment \
  --transit-gateway-attachment-id tgw-attach-zzz
```

**Co można shareować przez RAM dla networking:**

| Zasób | RAM Share | Cross-account |
|-------|-----------|---------------|
| Transit Gateway | ✅ | ✅ |
| VPC Subnety | ✅ (VPC Sharing) | ✅ (w org) |
| Route 53 Resolver Rules | ✅ | ✅ |
| License Manager configs | ✅ | ✅ |
| VPN Connections | ❌ | ❌ (nie przez RAM) |

**TGW RAM centralized model — AWS Landing Zone pattern:**
```
Organizacja AWS:
  Network Account: TGW + DX Connection + VPN
  Shared Services Account: VPC (AD, DNS, monitoring)
  Prod Account: VPC Prod
  Dev Account: VPC Dev
  
Wszyscy attachują się do TGW w Network Account przez RAM
Network Account zarządza centralnie TGW RT (segmentacja)
Zero mesh peering, zero duplikacji VPN/DX connections
```

---

### 11.9 Multicast w VPC (TGW)

**Multicast** = wysłanie pakietu do **grupy** odbiorców jednocześnie (1-to-many) bez duplikowania ruchu na źródle.

```
Unicast:   Źródło → każdy odbiorca osobno (N kopii)
Broadcast: Źródło → wszyscy w sieci (Layer 2, ograniczone)
Multicast: Źródło → tylko zainteresowani (joined) odbiorcy przez IGMP
```

**AWS VPC natywnie NIE obsługuje multicast** — ale **TGW obsługuje Multicast** dla VPC Attachments.

```mermaid
graph TB
    TGW["Transit Gateway\n(Multicast Domain)"]
    
    SOURCE["EC2 Source\n(Multicast Sender)\n10.0.1.50\nGrupa: 239.1.1.1"]
    
    R1["EC2 Receiver 1\n10.1.1.50\nIGMP Join: 239.1.1.1"]
    R2["EC2 Receiver 2\n10.2.1.50\nIGMP Join: 239.1.1.1"]
    R3["EC2 Receiver 3\n10.3.1.50\nBez IGMP → nie otrzyma"]
    
    SOURCE -->|"Multicast\n239.1.1.1"| TGW
    TGW -->|"kopia do R1"| R1
    TGW -->|"kopia do R2"| R2
    TGW -.->|"❌ R3 nie joinował"| R3
```

**Konfiguracja Multicast w TGW:**
```bash
# 1. Utwórz TGW z obsługą multicast (musi być enabled przy tworzeniu!)
aws ec2 create-transit-gateway \
  --options "MulticastSupport=enable"

# 2. Utwórz Multicast Domain
aws ec2 create-transit-gateway-multicast-domain \
  --transit-gateway-id tgw-xxx \
  --options "Igmpv2Support=enable,StaticSourcesConfig=disable"

# 3. Asocjuj VPC Attachment z Multicast Domain
aws ec2 associate-transit-gateway-multicast-domain \
  --transit-gateway-multicast-domain-id tgw-mcast-domain-xxx \
  --transit-gateway-attachment-id tgw-attach-yyy \
  --subnet-ids subnet-zzz

# 4. Zarejestruj source i grupy (opcjonalnie — przez IGMP lub statycznie)
aws ec2 register-transit-gateway-multicast-group-sources \
  --transit-gateway-multicast-domain-id tgw-mcast-domain-xxx \
  --group-ip-address 239.1.1.1 \
  --network-interface-ids eni-source-xxx
```

**Przypadki użycia multicast:**
- Live video streaming (media broadcasting) — 1 stream do N odbiorców
- Financial data feeds — tick data do wielu trading engines jednocześnie
- Cache invalidation — broadcast do N node cache
- IPTV, telepresence

**Ograniczenia TGW Multicast:**
- ❌ Nie działa między regionami (TGW Peering nie przenosi multicast)
- ❌ Tylko wewnątrz TGW Multicast Domain (nie do Internet)
- ❌ IGMPv2 (nie v3 snooping full)
- ✅ IPv4 tylko (nie IPv6 multicast)

---

### 11.10 AWS Transit Gateway vs Azure Virtual WAN / Virtual Hub

**Azure odpowiedniki:**

| AWS | Azure |
|-----|-------|
| Transit Gateway | Azure Virtual WAN (vWAN) + Virtual Hub |
| TGW Attachment (VPC) | Hub VNet Connection |
| TGW Attachment (VPN) | S2S VPN Gateway w Virtual Hub |
| TGW Attachment (DX) | ExpressRoute Gateway w Virtual Hub |
| TGW Peering | Virtual WAN Hub-to-Hub (globalny) |
| TGW Route Table | Virtual Hub Route Table |
| TGW Route Propagation | Route Propagation w Azure Hub |
| RAM Share TGW | Azure VNet Peering (inny mechanizm) |

#### Architektura porównawcza

**AWS TGW:**
```mermaid
graph TB
    TGW["Transit Gateway\n(Regionalny)\nRouter-like, pełna kontrola RT"]
    
    TGW <-->|"VPC Attachment"| VPC_A["VPC A"]
    TGW <-->|"VPC Attachment"| VPC_B["VPC B"]
    TGW <-->|"VPN Attachment"| VPN["Site-to-Site VPN"]
    TGW <-->|"DX GW Attachment"| DXGW["DX Gateway → DX"]
    TGW <-->|"Peering Attachment"| TGW_US["TGW us-east-1"]
    
    style TGW fill:#FF9900,color:#000
```

**Azure Virtual WAN:**
```mermaid
graph TB
    VWAN["Azure Virtual WAN\n(Globalny kontener)"]
    
    HUB_EU["Virtual Hub\neu-west (Amsterdam)\nManaged hub - auto-routing"]
    HUB_US["Virtual Hub\neastus (Virginia)"]
    
    VWAN --> HUB_EU
    VWAN --> HUB_US
    
    HUB_EU <-->|"VNet Connection"| VNET_A["VNet A"]
    HUB_EU <-->|"VNet Connection"| VNET_B["VNet B"]
    HUB_EU <-->|"S2S VPN GW"| VPN["On-prem VPN"]
    HUB_EU <-->|"ER GW"| ER["ExpressRoute"]
    
    HUB_EU <-->|"Hub-to-Hub\n(auto przez VWAN backbone)"| HUB_US
    
    style VWAN fill:#0078D4,color:#fff
    style HUB_EU fill:#0078D4,color:#fff
    style HUB_US fill:#0078D4,color:#fff
```

#### Szczegółowe porównanie Route Management

| Aspekt | AWS Transit Gateway | Azure Virtual Hub |
|--------|--------------------|--------------------|
| **Route Propagation** | Explicit: włącz propagację per attachment do wybranych RT | Automatyczna: BGP z connected resources (branch, VNet) propaguje do Hub RT |
| **Route Tables** | Pełna kontrola: twórz ile chcesz RT, decydujesz co do czego propaguje | Predefiniowane: `defaultRouteTable`, `noneRouteTable`; od v2 custom RTs dostępne (Standard tier) |
| **Segmentacja** | Pełna elastyczność: per-RT association + propagation | Azure vWAN Basic: brak segmentacji; Standard: custom RT z labels |
| **Static Routes** | Ręczne statyczne trasy per RT | Statyczne trasy per RT (Azure Standard vWAN) |
| **BGP** | BGP dla VPN/DX dynamicznie, peering statyczny | BGP dla VPN/ER dynamicznie, VNet connections statyczne |
| **Routing between Hubs** | TGW Peering: statyczne (inter-region) | Hub-to-Hub: automatyczne przez VWAN backbone (globalne) |
| **Default Route** | Manualny: dodaj `0.0.0.0/0 → inspection-attach` | `Propagate Default Route`: można propagować do VNet Connections |
| **Transitive Routing** | ✅ Pełne (po skonfigurowaniu RT) | ✅ Automatyczne w ramach VWAN |
| **Third-party NVA** | Przez Inspection VPC + Appliance Mode | NVA można wdrożyć bezpośrednio w Virtual Hub (od 2022) |

**Route Propagation — jak działa krok po kroku:**

```
AWS TGW Route Propagation:
─────────────────────────
1. VPC Attachment (VPC 10.0.0.0/16) PROPAGATES TO → RT-Default
   Effect: RT-Default uczy się: 10.0.0.0/16 → vpc-attach-aaa

2. VPN Attachment PROPAGATES TO → RT-Default (BGP dynamicznie)
   Effect: RT-Default uczy się: 192.168.0.0/16 → vpn-attach-bbb (z BGP)

3. EC2 w VPC A wysyła pakiet do 192.168.1.100:
   VPC RT: 0.0.0.0/0 → tgw-xxx
   TGW RT-Default: 192.168.0.0/16 → vpn-attach-bbb
   → pakiet do on-prem ✅

Wymagane kroki: 
   → Enable Route Propagation (aws ec2 enable-transit-gateway-route-table-propagation)
   → Lub statyczna trasa
```

```
Azure Virtual Hub Route Propagation:
─────────────────────────────────────
1. VNet Connection skonfigurowana → automatycznie propaguje VNet prefix do Hub RT
   Brak ręcznego "enable propagation"

2. Branch (VPN/ER) → BGP → automatycznie propaguje on-prem prefixes do Hub RT

3. Hub RT "defaultRouteTable":
   10.0.0.0/16 → VNet-A Connection  (auto-propagated)
   192.168.0.0/16 → VPN GW          (auto-propagated przez BGP)

4. VM w VNet A wysyła do 192.168.1.100:
   Efektywne trasy z Hub → 192.168.0.0/16 → VPN GW → on-prem
   
Mniej ręcznej pracy, mniej kontroli granularnej
```

**Segmentacja sieci:**

```
AWS TGW (pełna kontrola):
  RT-Prod: propaguje od VPC-Prod, VPC-Shared
  RT-Dev:  propaguje od VPC-Dev, VPC-Shared
  RT-Shared: propaguje od VPC-Shared
  → Prod i Dev odizolowane, oba mają dostęp do Shared
  → Możliwe: wiele RT, dowolna topologia
  → Cena za elastyczność: ręczna konfiguracja

Azure vWAN Standard (custom RT):
  RT-Prod: labeled "Prod" — VNet-Prod + VNet-Shared propaguje
  RT-Dev:  labeled "Dev" — VNet-Dev + VNet-Shared propaguje
  → Podobna segmentacja, ale mniej granularna
  → Labels upraszczają (zamiast per-attachment config)
  → Branches (VPN/ER): domyślnie do wszystkich RT
```

**Hub-to-Hub routing:**

```
AWS TGW (inter-region):
  → TGW Peering Attachment (ręczne)
  → Statyczne trasy w obu TGW RT
  → Brak BGP przez peering
  → Wymaga: 2 TGW + peering + 4 statyczne trasy (min)
  
Azure vWAN (globalne):
  → Dwa Virtual Hubs w jednym VWAN → automatyczne Hub-to-Hub
  → Brak ręcznej konfiguracji inter-hub
  → Globalny VWAN backbone (Microsoft global network)
  → VNet w EU automatycznie widzi VNet w US przez VWAN
```

**Kiedy wybrać TGW vs vWAN:**

| Scenariusz | AWS TGW | Azure vWAN |
|-----------|---------|------------|
| Pełna kontrola routingu | ✅ | ⚠️ (Standard tier) |
| Auto-routing, mało ops | ⚠️ Wymaga konfiguracji | ✅ (auto) |
| Multi-region | Peering (ręczne) | Automatyczne (VWAN) |
| NVA (firewall) w hub | Inspection VPC | Bezpośrednio w hub |
| Multicast | ✅ (TGW) | ❌ (brak w vWAN) |
| SD-WAN integration | Connect Attachment | SD-WAN partner w vWAN |
| Koszt per GB | $0.02/GB attachment | €0.02/GB |
| Open-source equiv. | VyOS/FRR router | - |

---

### 11.11 Well-Architected Framework

| Filar | Direct Connect | DX Gateway | Transit Gateway | TGW RAM | Multicast |
|-------|---------------|------------|-----------------|---------|-----------|
| **Operational Excellence** | AWS generuje konfigurację VIF (LOA-CFA). CloudWatch DX metrics (ConnectionState, ConnectionBpsIngress). Automated failover z BGP. | Jeden DX GW = zarządza wieloma VPC/regionami. Mniej zasobów do monitorowania. | Centralne zarządzanie routingiem vs mesh Peering. TGW Network Manager: visualizacja całej sieci. Route Analyzer: debug routingu. | Jeden TGW w Network Account = wszyscy używają przez RAM. Mniej duplikacji konfiguracji. | TGW Multicast Domain: monitoruj przez TGW metrics. IGMP membership tracking. |
| **Security** | Dedykowana linia — dane nie przez publiczny Internet. MACsec (Layer 2 encryption) dostępny na 10/100G DX. Combine z IPSec dla end-to-end encryption. | Scentralizuj DX w Network Account (separation of duties). DX GW nie eksponuje VPC CIDR cross-account. | TGW RT segmentacja = network-level isolation (Prod/Dev). Inspection VPC: scentralizuj IDS/IPS/NGFW. Appliance Mode: ensure traffic inspected. | RAM + SCPs (Service Control Policies) = kontroluj kto może attachować do TGW. Automatyczny accept vs manual accept per attachment. | Multicast Domain: izoluj — różne aplikacje w różnych domenach. |
| **Reliability** | HA: 2 DX Connections (różni DX Locations) + VPN backup. LAG (Link Aggregation Group): 2-4 połączenia = redundancja + bandwidth agregacja. BFD: sub-second failure detection. | DX GW Global: SPOF mitigation — jeden DX GW zarządza wszystkimi regionami. HA przez wiele DX Connections asocjowanych z tym samym DX GW. | TGW: managed service, AWS zarządza HA. Multi-AZ: subnety per AZ w VPC attachment. Nie ma SPoF w jednym AZ. | RAM: TGW HA wbudowane (nie zależy od account sharing). Attachment HA = subnet per AZ w każdym VPC. | Multicast inherently reliable (sender nie wie o receivers). TGW zarządza replikacją. |
| **Performance Efficiency** | 1/10/100 Gbps dedykowane — przewidywalna przepustowość. Consistent latency (nie zależy od Internet congestion). Jumbo Frames (9001 MTU) przez Private/Transit VIF = mniej overhead. | DX GW nie dodaje latency (pass-through routing). Transit VIF = jeden VIF dla wielu VPC (zamiast N Private VIF). | ECMP przez TGW VPN: 4 tunnele = 5 Gbps. TGW Peering przez AWS backbone (szybsze niż Internet). Connect Attachment (GRE): do 5 Gbps per appliance. | RAM Share: zero copy TGW — nie tworzy nowego TGW per account (efektywność zasobów). | Multicast vs unicast: N receivers, 1 kopia na sieci = dramatyczna redukcja bandwidth przy dużej skali. |
| **Cost Optimization** | DX vs VPN: breakeven przy ~1.5 TB/mc data transfer (DX $0.02/GB vs Internet $0.09/GB). Hosted DX (od 50 Mbps) dla mniejszych workloadów. LAG: 2×1G zamiast 1×10G (cena). | Brak dodatkowego kosztu za DX GW. Oszczędność: 1 DX Connection obsługuje N VPC przez DX GW + Transit VIF (zamiast N Private VIF = N×$). | TGW koszty: $0.05/h per attachment + $0.02/GB. Eliminate mesh Peering (ale Peering jest tańsze dla 2-3 VPC). ROI przy >5 VPC. | Jeden TGW w org = brak duplikacji (oszczędność na attachment hours). | Multicast eliminuje N-krotne bandwidth (finansowe dane: 1 stream do 100 consumers = 100x oszczędność). |
| **Sustainability** | DX niższy carbon footprint niż VPN przez Internet (krótsze ścieżki, dedykowany sprzęt). Jumbo Frames = mniej pakietów = mniej CPU. | Konsolidacja połączeń przez DX GW zmniejsza liczbę aktywnych połączeń. | TGW hub-and-spoke = optymalne ścieżki routingu (nie przez Internet). Inspection centralization = mniej redundantnych appliances. | Shared TGW: mniej instancji TGW w całej organizacji. | Multicast: drastycznie mniej ruchu sieciowego dla broadcast use cases. |

---

### 11.12 4 Domeny egzaminacyjne

#### 🏗️ Domain 1: Organizational Complexity

**Scenariusz:** Enterprise 200 kont, 50 VPC, 5 regionów, 3 DC on-prem, 2 x 10G Direct Connect (różne lokalizacje), wymagana segmentacja Prod/Dev/Shared.

```
Architektura:
  Network Account (centralne zarządzanie):
    → TGW per region (5 TGW)
    → TGW Peering między regionami (mesh między 5 = 10 peering connections)
    → DX Gateway (globalny) → oba DX Connections → TGW każdego regionu
    → RAM: share każdy TGW do odpowiednich OU w AWS Organizations
    
  Per TGW: 3 Route Tables (Prod / Dev / Shared):
    → Segmentacja przez RT Association + Propagation
    → Shared propaguje do Prod RT i Dev RT (ale nie vice versa)
    
  DNS:
    → Route 53 Resolver Inbound/Outbound Endpoints w Shared VPC
    → PHZ dla corp.internal → shared VPC (centralny DNS)
    
  DX HA:
    → DX Connection 1: Equinix Amsterdam
    → DX Connection 2: Interxion Amsterdam  
    → Oba przez DX GW → TGW eu-west-1
    → VPN Backup: Site-to-Site VPN przez Internet
    
  Koszt miesięczny (oszacowanie):
    5 TGW × 100 attachments × $0.05 × 720h = $18 000
    10 TGW Peering × $0.05 × 720h = $360
    DX: 2 × 10G $1620/mc + data
```

#### 🆕 Domain 2: New Solutions

**Scenariusz:** Startup, 5 VPC (Prod, Staging, Dev, Security, Shared), jeden region, bez on-prem.

```
Rozwiązanie: TGW + RAM (jeśli multi-account) lub VPC Peering (jeśli single-account)

Analiza:
  5 VPC single-account: 
    Peering: 5×4/2 = 10 connections — zarządzalne
    TGW: 5 attachments + koszty $0.05/h = $36/mc minimum
    
  Decyzja: dla <5 VPC single-account → VPC Peering może być tańsze
  Dla multi-account lub przyszły wzrost → TGW od razu

TGW Setup:
  RT-Prod:     Shared propaguje tu, Prod propaguje tu, brak Dev/Staging
  RT-Staging:  Shared propaguje tu, Staging propaguje tu
  RT-Dev:      Shared propaguje tu, Dev propaguje tu
  RT-Security: wszystkie propagują (Security obserwuje cały ruch)
  
Inspection:
  Security VPC → AWS Network Firewall
  TGW RT: 0.0.0.0/0 → Security VPC Attachment (dla inter-VPC traffic)
```

#### 📈 Domain 3: Continuous Improvement

**Scenariusz:** Firma ma mesh VPC Peering (10 VPC = 45 connections), on-prem przez VPN per-VPC (10 VPN connections). Zarządzanie staje się niemożliwe.

```
Migracja do TGW:
  Faza 1: Utwórz TGW, default RT
  Faza 2: Jeden po jednym — VPC Attachment do TGW
    Test: Ruch przez TGW działa
    Parallel run: Peering + TGW jednocześnie (dwa ścieżki)
  Faza 3: Usuń VPN connections per VPC → jeden TGW VPN Attachment
  Faza 4: Usuń VPC Peering (po potwierdzeniu ruch przez TGW)
  
Kalkulacja:
  Przed: 45 Peering (bezpłatne ale $0.01/GB) + 10 VPN ($360/mc)
  Po: 10 Attachments × $36 = $360/mc + $0.02/GB przez TGW
  
  Przy <10 TB/mc: podobny koszt
  Przy >10 TB/mc: porównaj GB charges
  Benefit: drastycznie prostsze zarządzanie = mniej błędów = wartość nienamierzalna
```

#### 🚀 Domain 4: Migration and Modernization

**Scenariusz:** Migracja z on-prem do AWS. Aktualnie: VPN 1 Gbps (nasycony), DC colocated z Equinix AM3.

```
Faza 1 (migracja aktywna):
  Direct Connect 10G: Equinix AM3 → eu-west-1
    X-Connect w AM3 (Equinix: $30-100/mc)
    Last Mile: Equinix connect (cross-connect do AWS cage)
    Private VIF → DX GW → TGW → VPC Migration
    
  Parallel: VPN jako backup (zachowaj na czas DX provisioning 4-12 tyg)

Faza 2 (migracja zakończona):
  DX 10G Primary (BGP LOCAL_PREF 200)
  VPN Backup (BGP LOCAL_PREF 100)
  TGW: VPC Prod + VPC Migration + VPC Dev
  
Faza 3 (post-migracja):
  Oceń czy potrzebujesz DX po migracji:
    Jeśli tylko AWS: DX kosztuje $1600+/mc → może nie opłacać się
    Jeśli hybrid (workloady on-prem nadal): zachowaj DX
  
  Optymalizacja:
    10G → 1G DX (jeśli ruch maleje)
    VPN jako jedyne połączenie (jeśli <1 GB/mc z on-prem)
```

---

## 12. VPC — Architektura, VPC Peering i Rozwiązania VPN w AWS

> **Zakres:** Kompletny przegląd VPC jako fundament sieci AWS, mechanizmy połączeń VPC-to-VPC (Peering), oraz wszystkie opcje VPN: Site-to-Site VPN, Client VPN, VPN CloudHub, i ich integracja z Transit Gateway.

---

### 12.1 VPC — Fundament Sieci AWS

### Co to jest VPC?

**Amazon VPC (Virtual Private Cloud)** to logicznie izolowana sieć wirtualna w chmurze AWS. Możesz o niej myśleć jak o własnym centrum danych w AWS — masz pełną kontrolę nad przestrzenią adresową IP, podsieciami, tabelami routingu, bramami sieciowymi i ustawieniami bezpieczeństwa.

**Analogia z życia:** VPC to jak wydzielona działka w parku technologicznym. Masz ogrodzony teren (izolacja sieciowa), możesz budować budynki (EC2, RDS), układać wewnętrzne drogi (routing), stawiać bramy wjazdowe (IGW, VGW) i systemy alarmowe (Security Groups, NACL).

**Odpowiednik w Azure:** Azure Virtual Network (VNet). Różnice:
- AWS VPC: Regional resource, ale subnety są **Availability Zone-specific**
- Azure VNet: Regional resource, subnety są **regionalne** (nie AZ-specific)
- AWS: explict ENI model (każdy zasób dostaje Elastic Network Interface)
- Azure: NIC-based model — podobny, ale z innymi ograniczeniami

### Kluczowe właściwości VPC

| Właściwość | Wartość | Uwaga |
|---|---|---|
| **Zakres** | Regionalny | Obejmuje wszystkie AZ w regionie |
| **Subnety** | Zonal (per-AZ) | Jeden subnet = jedna AZ |
| **Max VPC per region** | 5 (soft limit) | Można zwiększyć do 100+ |
| **Max subnets per VPC** | 200 (soft limit) | |
| **CIDR block** | /16 – /28 | Minimum /28 = 16 IP (11 użytecznych) |
| **Reserved IPs per subnet** | 5 | .0 (network), .1 (router), .2 (DNS), .3 (future), .255 (broadcast) |
| **Domyślny DNS** | VPC CIDR + 2 | np. `10.0.0.2` dla `10.0.0.0/16` |
| **IPv6** | /56 per VPC, /64 per subnet | AWS przydziela, nie możesz wybrać |

### Komponenty VPC — mapa zależności

```mermaid
graph TB
    subgraph VPC["VPC 10.0.0.0/16 (Regional)"]
        subgraph AZ1["Availability Zone A"]
            PubA["Public Subnet\n10.0.1.0/24"]
            PrivA["Private Subnet\n10.0.2.0/24"]
        end
        subgraph AZ2["Availability Zone B"]
            PubB["Public Subnet\n10.0.3.0/24"]
            PrivB["Private Subnet\n10.0.4.0/24"]
        end
        
        ROUTER["VPC Router\n(implicit, każdy subnet)"]
        RT_PUB["Route Table: Public\n0.0.0.0/0 → IGW"]
        RT_PRIV["Route Table: Private\n0.0.0.0/0 → NAT GW"]
        
        IGW["Internet Gateway"]
        NATGW["NAT Gateway\n(w Public Subnet)"]
        VGW["Virtual Private Gateway\n(dla VPN/DX)"]
        TGWA["TGW Attachment"]
    end
    
    IGW --> INTERNET["Internet"]
    VGW --> ONPREM["On-premises"]
    TGWA --> TGW["Transit Gateway"]
    
    PubA & PubB --> RT_PUB
    PrivA & PrivB --> RT_PRIV
    RT_PUB --> IGW
    RT_PRIV --> NATGW
    NATGW --> IGW
```

### Secondary CIDR Blocks

VPC może mieć wiele bloków CIDR:
- **Primary CIDR:** ustalony przy tworzeniu VPC, nie można zmienić
- **Secondary CIDRs:** do 5 dodatkowych bloków IPv4, do 5 bloków IPv6

```bash
# Dodaj secondary CIDR do VPC
aws ec2 associate-vpc-cidr-block \
  --vpc-id vpc-12345678 \
  --cidr-block 100.64.0.0/16

# Kiedy przydatne?
# - VPC zbliża się do wyczerpania przestrzeni IP
# - Migracja do nowego CIDR (overlay)
# - Obejście dla overlapping CIDR z Private NAT Gateway
```

> ⚠️ **Egzamin:** Nie można USUNĄĆ primary CIDR. Secondary CIDR można usunąć (jeśli brak zasobów w tej przestrzeni). Secondary CIDR musi być z tych samych RFC 1918 lub innych dozwolonych zakresów.

---

### 12.2 VPC Peering — Połączenie Bezpośrednie między VPC

### Czym jest VPC Peering?

**VPC Peering** to prywatne połączenie sieciowe między dwoma VPC — ruch przepływa przez **wewnętrzną sieć AWS** (nie przez Internet). Oba VPC zachowują swoją izolację; połączenie jest punktowe (1:1).

**Analogia:** VPC Peering to jak prywatna kładka łącząca dwa ogrodzony parki technologiczne. Mieszkańcy obu parków mogą się odwiedzać, ale nikt z zewnątrz nie widzi tej kładki.

**Odpowiednik Azure:** Azure VNet Peering (bardzo zbliżony model, ale różnice w transitivity i cenach).

### Kluczowe cechy i ograniczenia

```
VPC A ←──── Peering ────→ VPC B
       (prywatna sieć AWS, bez IGW)
```

| Cecha | Wartość |
|---|---|
| **Typ połączenia** | Point-to-point, non-transitive |
| **Cross-account** | ✅ Tak |
| **Cross-region** | ✅ Tak (Inter-Region Peering) |
| **Szyfrowanie** | ✅ Automatyczne (AES-256 dla cross-region) |
| **Bandwidth** | Brak limitu (ograniczony przez EC2 instance type) |
| **Overlapping CIDR** | ❌ Niemożliwe — CIDR muszą być unikalne |
| **Transitive routing** | ❌ Niemożliwe (patrz poniżej) |
| **Latency** | Bardzo niska (przez AWS backbone) |
| **Koszt** | Brak opłat za setup/hour; DATA TRANSFER: $0.01/GB (same region), cross-region — jak standard inter-region |

### Non-Transitive Routing — dlaczego VPC Peering nie jest "hub"

```
    VPC A ←──── Peering ────→ VPC B
                                  ↕ Peering
                              VPC C

❌ VPC A NIE może komunikować się z VPC C przez VPC B
   (Packet: A→B→C jest blokowany przez AWS)

✅ Rozwiązanie: osobny Peering A↔C  LUB  Transit Gateway
```

**Dlaczego?** AWS blokuje "packet forwarding" przez Peering na poziomie hypervisora — bezpieczeństwo i uproszczenie topologii. VPC B nie może być "routerem" dla A↔C.

### Konfiguracja VPC Peering krok po kroku

```bash
# Krok 1: Utwórz Peering Connection (z konta właściciela VPC A)
aws ec2 create-vpc-peering-connection \
  --vpc-id vpc-aaa111 \         # Requester VPC
  --peer-vpc-id vpc-bbb222 \    # Accepter VPC
  --peer-owner-id 123456789012 \ # Inne konto (opcjonalne)
  --peer-region eu-west-1        # Inny region (opcjonalne)

# Krok 2: Akceptuj z konta właściciela VPC B
aws ec2 accept-vpc-peering-connection \
  --vpc-peering-connection-id pcx-12345678

# Krok 3: Dodaj Route w VPC A (Route Table)
aws ec2 create-route \
  --route-table-id rtb-aaaa1111 \
  --destination-cidr-block 10.1.0.0/16 \   # CIDR VPC B
  --vpc-peering-connection-id pcx-12345678

# Krok 4: Dodaj Route w VPC B (Route Table) — OBUSTRONNIE!
aws ec2 create-route \
  --route-table-id rtb-bbbb2222 \
  --destination-cidr-block 10.0.0.0/16 \   # CIDR VPC A
  --vpc-peering-connection-id pcx-12345678

# Krok 5: Zaktualizuj Security Groups w obu VPC
# (Zezwól na ruch z CIDR drugiego VPC lub referencja do SG)
```

> ⚠️ **Egzamin:** Route tables trzeba zaktualizować **w OBU VPC**. Peering bez tras = brak komunikacji mimo aktywnego Peering.

### VPC Peering — Security Groups cross-VPC

```bash
# Security Group w VPC A może referencjonować SG z VPC B (przez Peering)
# Wymaga: Peering aktywny, Same region
aws ec2 authorize-security-group-ingress \
  --group-id sg-aaa111 \
  --ip-permissions '[{
    "IpProtocol": "tcp",
    "FromPort": 443,
    "ToPort": 443,
    "UserIdGroupPairs": [{
      "GroupId": "sg-bbb222",
      "VpcId": "vpc-bbb222",
      "VpcPeeringConnectionId": "pcx-12345678",
      "UserId": "123456789012"
    }]
  }]'

# ❌ Cross-region Peering: SG reference NIE działa — użyj CIDR
```

### Skalowanie: Problem N*(N-1)/2

```
Liczba VPC  | Peerings potrzebnych (full mesh)
4           | 6
10          | 45
50          | 1225
100         | 4950

→ Dla >4-5 VPC: Transit Gateway jest lepszym rozwiązaniem
```

### VPC Peering vs Azure VNet Peering

| Cecha | AWS VPC Peering | Azure VNet Peering |
|---|---|---|
| **Transitive** | ❌ | ❌ (domyślnie, ale można z NVA) |
| **Cross-region** | ✅ | ✅ (Global VNet Peering) |
| **Cross-tenant** | ✅ | ✅ |
| **Gateway Transit** | ❌ | ✅ (UseRemoteGateways) |
| **Koszt data** | $0.01/GB (same-region) | Zależy od regionu, porównywalne |
| **DNS** | Ręczna konfiguracja PHZ | Automatyczna opcja |
| **SG/NSG reference** | ✅ (same region) | ❌ |

> 🔵 **Azure różnica:** Azure VNet Peering ma opcję `UseRemoteGateways` — pozwala na VNet spoke używać VPN GW z VNet hub (transitive!). AWS VPC Peering tego nie ma — potrzebujesz TGW.

---

### 12.3 AWS Site-to-Site VPN

### Czym jest Site-to-Site VPN?

Połączenie **IPsec VPN** między siecią on-premises (lub inną lokalizacją) a AWS VPC. Ruch przechodzi przez publiczny Internet, ale jest **szyfrowany**.

**Analogia:** Bezpieczny tunel w szybie między Twoim biurem a parkiem technologicznym AWS. Z zewnątrz widać tylko zaszyfrowany ruch, treść jest niewidoczna.

**Odpowiednik Azure:** Azure VPN Gateway (Site-to-Site).

### Architektura

```
On-premises                    AWS
┌─────────────────┐            ┌──────────────────────────┐
│  Customer       │            │ Virtual Private Gateway  │
│  Gateway (CGW)  │            │ (VGW) lub TGW            │
│  Router/FW      │◄──IPsec───►│                          │
│  BGP lub Static │   VPN      │  VPC                     │
└─────────────────┘  Tunnel    └──────────────────────────┘
  - Twój publiczny IP            - 2 tunele (HA)
  - Twoje urządzenie VPN         - Dwa publiczne IP (AWS)
```

### Komponenty

**Customer Gateway (CGW):**
- AWS resource reprezentujący Twoje urządzenie VPN on-prem
- Wymagane: publiczny IP CGW, opcjonalnie ASN dla BGP

```bash
aws ec2 create-customer-gateway \
  --type ipsec.1 \
  --public-ip 203.0.113.1 \      # Twój publiczny IP on-prem
  --bgp-asn 65000                # Twój ASN dla BGP (lub 65000 domyślnie)
```

**Virtual Private Gateway (VGW):**
- Attachuje się do VPC
- Terminuje VPN tunnele
- Ma dwa endpointy (HA) w różnych AZ

```bash
aws ec2 create-vpn-gateway \
  --type ipsec.1 \
  --amazon-side-asn 64512        # AWS ASN

aws ec2 attach-vpn-gateway \
  --vpc-id vpc-12345678 \
  --vpn-gateway-id vgw-12345678
```

**VPN Connection:**
- Łączy CGW z VGW (lub TGW)
- **ZAWSZE 2 tunele** (dwa publiczne IP po stronie AWS) dla HA
- Każdy tunel: inne IP, inne klucze

```bash
aws ec2 create-vpn-connection \
  --type ipsec.1 \
  --customer-gateway-id cgw-12345678 \
  --vpn-gateway-id vgw-12345678 \
  --options '{"StaticRoutesOnly": false}' # false = BGP

# Download konfiguracji dla Cisco/Juniper/Palo Alto etc.
aws ec2 describe-vpn-connections \
  --vpn-connection-ids vpn-12345678 \
  --query 'VpnConnections[0].CustomerGatewayConfiguration'
```

### BGP vs Static Routing

| | BGP (Dynamic) | Static |
|---|---|---|
| **Konfiguracja** | Bardziej złożona | Prostsze |
| **Failover** | Automatyczny | Ręczny |
| **Scalability** | ✅ Dobre | ❌ Ograniczone |
| **Route propagation** | ✅ Automatyczna | ❌ Ręczna |
| **Wymagania** | BGP-capable router | Dowolny router |
| **Zalecane dla SAP-C02** | Tak (best practice) | Legacy/simple |

> ⚠️ **Egzamin:** BGP używa **eBGP** (External BGP). AWS ASN: 7224 (default) lub dowolny 64512-65534 (private ASN). CGW ASN: dowolny, najczęściej 65000.

### Route Propagation do VPC Route Table

```bash
# Włącz Route Propagation — VGW automatycznie wstrzykuje trasy BGP do RT
aws ec2 enable-vgw-route-propagation \
  --route-table-id rtb-12345678 \
  --gateway-id vgw-12345678

# Bez tego: musisz ręcznie dodać statyczne trasy do RT
```

### HA i Redundancja VPN

```
Pattern 1: Single CGW, 2 tunnele (basic HA)
  CGW ─── Tunnel 1 (AWS IP #1) ─── VGW
  CGW ─── Tunnel 2 (AWS IP #2) ─── VGW
  
  Odporność na: awaria jednego AWS endpoint
  NIE odporny na: awaria CGW, awaria łącza internetowego

Pattern 2: 2x CGW + VGW (redundant CGW)
  CGW-1 ─── Tunnels 1,2 ─── VGW
  CGW-2 ─── Tunnels 3,4 ─── VGW
  
  Odporność na: awaria urządzenia CGW lub łącza

Pattern 3: VPN + Direct Connect (best practice)
  CGW ─── VPN ─── VGW   (backup)
  On-prem ─── DX ─── VGW (primary)
  
  BGP LOCAL_PREF lub MED kontroluje failover
```

### VPN przez Transit Gateway zamiast VGW

```bash
# Nowy VPN Connection do TGW (zalecane dla enterprise)
aws ec2 create-vpn-connection \
  --type ipsec.1 \
  --customer-gateway-id cgw-12345678 \
  --transit-gateway-id tgw-12345678 \
  --options '{"StaticRoutesOnly": false}'

# Zalety TGW vs VGW:
# - ECMP: wiele VPN do TGW = load balancing (max 1.25 Gbps per tunel → wiele tuneli)
# - Jeden punkt centralny dla wielu VPC
# - VGW: max ~1.25 Gbps per VPN Connection
```

### Parametry IPsec

```
Phase 1 (IKE — konfiguracja tunelu):
  - IKEv1 lub IKEv2 (rekomendowane IKEv2)
  - Encryption: AES-128, AES-256
  - Integrity: SHA-1, SHA-256
  - DH Group: 2, 14, 15, 16, 17, 18, 22, 23, 24
  - Lifetime: 28800s (8h)

Phase 2 (IPsec — szyfrowanie danych):
  - Encryption: AES-128, AES-256, AES-128-GCM, AES-256-GCM
  - Integrity: SHA-1, SHA-256, SHA-384, SHA-512
  - PFS: Groups 2, 5, 14-24
  - Lifetime: 3600s (1h)
  
DPD (Dead Peer Detection): dostępne
```

### Koszty Site-to-Site VPN

| Składnik | Koszt (eu-west-1) |
|---|---|
| VPN Connection | **$0.05/h** ≈ $36/mc |
| Data Transfer In | $0.00 |
| Data Transfer Out | Standard EC2 rates |
| VGW hour | $0.05/h (jeśli nie TGW) |

---

### 12.4 AWS Client VPN

### Czym jest Client VPN?

**AWS Client VPN** to zarządzany VPN oparty na **OpenVPN** dla użytkowników końcowych (laptopy, urządzenia mobilne). Umożliwia zdalne połączenie do zasobów AWS lub on-premises.

**Analogia:** Client VPN to jak odznaka z kartą dostępu dla pracownika — każdy pracownik ma swoją kartę i może wejść do chronionego obiektu (VPC) z dowolnego miejsca na świecie.

**Odpowiednik Azure:** Azure Point-to-Site VPN Gateway.

### Architektura

```
Laptop pracownika
    │
    │ OpenVPN Client
    │ (AWS-provided lub compatible OpenVPN)
    ▼
Client VPN Endpoint
(ENI w wybranych subnetach VPC)
    │
    ├──→ VPC (bezpośredni dostęp do EC2, RDS, etc.)
    ├──→ On-premises (przez VPN lub DX — jeśli skonfigurowane)
    └──→ Internet (jeśli full tunnel — opcja)
```

### Konfiguracja

```bash
# 1. Utwórz Client VPN Endpoint
aws ec2 create-client-vpn-endpoint \
  --client-cidr-block 192.168.100.0/22 \   # Zakres IP dla klientów
  --server-certificate-arn arn:aws:acm:... \
  --authentication-options '[{
    "Type": "certificate-authentication",
    "MutualAuthentication": {
      "ClientRootCertificateChainArn": "arn:aws:acm:..."
    }
  }]' \
  --connection-log-options '{"Enabled": true, "CloudwatchLogGroup": "/aws/client-vpn"}' \
  --split-tunnel                             # Split tunnel mode

# 2. Associate z subnetem VPC
aws ec2 associate-client-vpn-target-network \
  --client-vpn-endpoint-id cvpn-endpoint-... \
  --subnet-id subnet-12345678

# 3. Dodaj Authorization Rule
aws ec2 authorize-client-vpn-ingress \
  --client-vpn-endpoint-id cvpn-endpoint-... \
  --target-network-cidr 10.0.0.0/16 \      # VPC CIDR
  --authorize-all-groups                    # Wszyscy użytkownicy
```

### Metody Autentykacji

| Metoda | Opis | Kiedy używać |
|---|---|---|
| **Mutual TLS (certyfikaty)** | Klient + serwer certyfikat X.509 | Najprostsza, bez zewnętrznego IdP |
| **Active Directory (AWS Managed AD)** | Username/Password przez AD | Integracja korporacyjna |
| **SAML 2.0 (SSO)** | IAM Identity Center, Okta, Azure AD | SSO z MFA |
| **Kombination** | np. SAML + certyfikat | Wysoki poziom bezpieczeństwa |

### Split Tunnel vs Full Tunnel

```
Split Tunnel (zalecane):
  - Tylko ruch do VPC CIDR → przez VPN
  - Ruch do Internetu → bezpośrednio z laptop
  - Zalety: mniejsza latency, mniej kosztów, mniej obciążenia

Full Tunnel:
  - CAŁY ruch → przez VPN → przez VPC
  - Ruch do Internetu: laptop → VPN → VPC → NAT GW → Internet
  - Zalety: pełna inspekcja ruchu (compliance), blokada direct Internet
  - Wady: wyższe koszty, wyższa latency
```

### Koszty Client VPN

| Składnik | Koszt |
|---|---|
| Endpoint association (per subnet/h) | $0.10/h ≈ $72/mc per subnet |
| Active connections | $0.05/h per połączenie |
| Data Transfer | Standardowe stawki EC2 |

> ⚠️ Koszty Client VPN mogą być znaczne przy wielu użytkownikach. Dla >200 użytkowników rozważ third-party VPN lub architektury alternatywne.

---

### 12.5 VPN CloudHub

### Czym jest VPN CloudHub?

**AWS VPN CloudHub** to architektura (nie osobna usługa!) która pozwala na połączenie **wielu lokalizacji on-premises** przez wspólny **Virtual Private Gateway** (VGW) lub **Transit Gateway**. Lokalizacje mogą komunikować się ze sobą **przez AWS backbone**.

**Analogia:** CloudHub to jak centrum logistyczne — każde biuro wysyła paczki do centrum (AWS), centrum przekierowuje do właściwego adresata. Biura nie muszą mieć bezpośrednich połączeń między sobą.

### Architektura CloudHub

```
Biuro Warsaw ─── VPN ──┐
                        │
Biuro Berlin ─── VPN ──┼──── Virtual Private Gateway ──── VPC
                        │         (hub)
Biuro London ─── VPN ──┘

Komunikacja Warsaw → Berlin:
Warsaw → AWS VGW → Berlin (przez AWS backbone, przez Internet)
```

### Konfiguracja (VGW-based)

```bash
# Każde biuro dostaje własny CGW
aws ec2 create-customer-gateway --public-ip 203.0.113.10 --bgp-asn 65001  # Warsaw
aws ec2 create-customer-gateway --public-ip 203.0.113.20 --bgp-asn 65002  # Berlin
aws ec2 create-customer-gateway --public-ip 203.0.113.30 --bgp-asn 65003  # London

# VPN Connection dla każdego biura → ten sam VGW
aws ec2 create-vpn-connection --customer-gateway-id cgw-warsaw --vpn-gateway-id vgw-hub
aws ec2 create-vpn-connection --customer-gateway-id cgw-berlin --vpn-gateway-id vgw-hub
aws ec2 create-vpn-connection --customer-gateway-id cgw-london --vpn-gateway-id vgw-hub

# Każde biuro ogłasza przez BGP swoje prefiksy
# VGW redystrybuuje trasy między CGW → site-to-site routing
```

> ⚠️ **Wymaganie:** BGP jest **konieczny** dla CloudHub (dynamiczne ogłaszanie tras). Statyczne VPN nie obsługuje CloudHub routing między lokalizacjami.

### CloudHub vs Transit Gateway (TGW)

| Cecha | VPN CloudHub (VGW) | TGW CloudHub |
|---|---|---|
| **Max lokalizacje** | 10 CGW per VGW | 5000 attachments per TGW |
| **Bandwidth** | ~1.25 Gbps per tunel | ECMP — wiele tuneli |
| **VPC routing** | 1 VPC (per VGW) | Wiele VPC |
| **Segmentacja** | Brak | TGW RT segmentacja |
| **Koszt** | $0.05/h per VPN | $0.05/h per attachment |
| **Złożoność** | Prosta | Bardziej złożona |
| **Zalecane** | ≤10 lokalizacji, 1 VPC | Enterprise, >10 lokalizacji |

### DX SiteLink — "CloudHub przez Direct Connect"

```
DX SiteLink to routing między on-prem lokalizacjami przez AWS backbone 
przez DX Connections (nie przez Internet).

Lokalizacja A ── DX ──┐
                       ├── DX Gateway ── sieć AWS backbone
Lokalizacja B ── DX ──┘

A → B: przez AWS backbone (niskie latency, wysoka bandwidth)
vs CloudHub: przez Internet (wyższe latency, zmienna bandwidth)
```

---

### 12.6 VPN i Direct Connect — Wybór i Porównanie

### Decision Tree — VPN vs DX

```mermaid
flowchart TD
    START["Potrzebujesz\npołączenia on-prem → AWS?"] --> Q1{"Wymagana\nbandwidth?"}
    
    Q1 -->|"< 1.25 Gbps\nlub nieregularne"| Q2{"Czas\nwdrożenia?"}
    Q1 -->|"> 1 Gbps\nlub konsystentne"| DX["Direct Connect\n(1G/10G/100G)"]
    
    Q2 -->|"Szybko\n(dni/tygodnie)"| VPN["Site-to-Site VPN\n$0.05/h\nGotowe w godzinach"]
    Q2 -->|"Wolno\n(miesiące OK)"| DX
    
    DX --> Q3{"HA requirement?"}
    Q3 -->|"Critical"| DXHA["DX + VPN Backup\n(best practice)"]
    Q3 -->|"Standard"| Q4{"Ruch między\nregionami?"}
    Q4 -->|"Tak"| DXSL["DX SiteLink"]
    Q4 -->|"Nie"| DX2["Single DX"]
    
    VPN --> Q5{"Wielu użytkowników\nzdalnych?"}
    Q5 -->|"Tak"| CVPN["Client VPN\n(OpenVPN)"]
    Q5 -->|"Nie"| VPN2["Site-to-Site VPN"]
```

### Pełne Porównanie

| | Site-to-Site VPN | Client VPN | VPN CloudHub | Direct Connect |
|---|---|---|---|---|
| **Use case** | Biuro/DC → VPC | Użytkownik → VPC | Wiele biur → VPC | Biuro/DC → VPC |
| **Szyfrowanie** | ✅ IPsec | ✅ TLS/OpenVPN | ✅ IPsec | ❌ (opcjonalnie MACsec) |
| **Bandwidth** | ~1.25 Gbps/tunel | 10 Mbps/użytkownik | ~1.25 Gbps/tunel | 1G/10G/100G |
| **Latency** | Zmienne (Internet) | Zmienne (Internet) | Zmienne (Internet) | Przewidywalne, niskie |
| **Setup time** | Godziny | Godziny | Godziny | Tygodnie-miesiące |
| **Koszt** | $0.05/h | $0.05/h/user + $0.10/subnet | $0.05/h per tunel | $1600+/mc (10G) |
| **HA** | 2 tunele built-in | Multi-AZ | 2 tunele per CGW | Wymaga 2x DX |
| **BGP** | Opcjonalny | N/A | Wymagany | Wymagany |

---

### 12.7 Well-Architected Framework — VPC, Peering, VPN

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | VPC Flow Logs dla visibility, CloudWatch alarms na VPN tunnel state, centralny monitoring przez TGW Network Manager |
| **Security** | Security Groups + NACL layered defense, VPC Peering SG reference (nie CIDR gdzie możliwe), szyfrowanie VPN end-to-end, PrivateLink zamiast Peering dla 3rd party services |
| **Reliability** | VPN: zawsze 2 tunele (built-in HA), DX + VPN backup, Multi-AZ VPC subnets, Client VPN: multi-subnet association |
| **Performance Efficiency** | VPC Peering dla niskiej latency VPC-to-VPC (vs TGW overhead), Direct Connect dla consistent throughput, ECMP na TGW VPN dla bandwidth agregacji |
| **Cost Optimization** | VPN vs DX breakeven (~23 TB/mc), Client VPN split-tunnel (mniej danych przez VPN), VPC Peering free (same-region, brak /h), CloudHub dla wielu biur (1 VGW = oszczędność) |
| **Sustainability** | Konsolidacja ruchu przez TGW (mniej duplikacji), right-sizing Client VPN (brak idle endpoint hours) |

---

### 12.8 4 Domeny Egzaminacyjne SAP-C02

### Domena 1: Design Solutions for Organizational Complexity

- **Multi-account VPC sharing:** RAM share VPC subnets między kontami (nie VPC Peering potrzebny)
- **TGW + VPC Peering trade-off:** Peering dla prostych 2-VPC, TGW dla enterprise multi-VPC
- **VPN CloudHub dla multi-site:** Jeden VGW zamiast mesh VPN między biurami
- **Centralized security:** Inspection VPC przez TGW, Network Firewall

### Domena 2: Design for New Solutions

- **Wybór VPN vs DX:** Bandwidth, latency, cost, setup time
- **Client VPN authentication:** SAML+MFA dla zero-trust remote access
- **VPC CIDR planning:** Nie nakładać się (Peering, TGW), zostawić miejsce
- **Overlapping CIDR workaround:** Private NAT GW + Secondary CIDR + TGW

### Domena 3: Continuous Improvement for Existing Solutions

- **Peering → TGW migration:** Gdy topologia rośnie powyżej 4-5 VPC
- **Static VPN → BGP:** Automatyczny failover, route propagation
- **Single VPN → DX + VPN backup:** Gdy SLA wymaga wyższej dostępności
- **Route Propagation:** Automatyzacja zamiast manualnych tras w RT

### Domena 4: Accelerate Workload Migration and Modernization

- **VPN jako pierwszy krok:** Szybkie połączenie on-prem → AWS podczas migracji
- **DX po migracji:** Gdy ruch stabilny i duży, przejście na DX
- **Client VPN dla adminów:** Zdalny dostęp do zasobów migrowanych
- **VPC Peering dla lift-and-shift:** Istniejące aplikacje cross-VPC bez refaktoru

---

## 13. Amazon EC2 — Rodziny Instancji, Zasoby, Koszty i Modele Zakupu

> **Zakres:** Pełny przegląd typów instancji EC2, ich zasobów (CPU/RAM/Storage/Network), modeli cenowych (On-Demand, Reserved, Spot, Savings Plans, Dedicated), oraz kiedy używać której rodziny dla konkretnego workload.

---

### 13.1 Co to jest EC2 i z czego się składa?

**Amazon EC2 (Elastic Compute Cloud)** to usługa wirtualnych maszyn (VM) w AWS. Każda instancja to izolowany serwer wirtualny działający na fizycznym hoście w centrum danych AWS.

**Analogia z życia:** EC2 to jak wynajem biurka w centrum coworkingowym — wynajmujesz określoną ilość zasobów (biurko = vCPU, przestrzeń = RAM, szafka = storage) na wybrany czas, płacąc za faktyczne użycie.

**Odpowiednik Azure:** Azure Virtual Machines (VMs). Odpowiednik Linux/Open-source: KVM/QEMU, VMware ESXi.

### Zasoby składowe każdej instancji EC2

```
┌─────────────────────────────────────────────────────┐
│                  Instancja EC2                       │
│                                                     │
│  ┌───────────┐  ┌───────────┐  ┌─────────────────┐ │
│  │  vCPU     │  │  RAM      │  │  Network        │ │
│  │ (virtual  │  │ (GiB)     │  │ Bandwidth       │ │
│  │  core)    │  │           │  │ (Gbps)          │ │
│  └───────────┘  └───────────┘  └─────────────────┘ │
│                                                     │
│  ┌───────────┐  ┌───────────┐  ┌─────────────────┐ │
│  │  Storage  │  │  GPU/FPGA │  │  ENA / EFA      │ │
│  │ (EBS/NVMe │  │ (opcjonal)│  │  (sieć wys.     │ │
│  │  /S3)     │  │           │  │  wydajności)    │ │
│  └───────────┘  └───────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────┘
```

| Zasób | Opis | Uwaga |
|---|---|---|
| **vCPU** | Virtual CPU — fragment fizycznego rdzenia (lub cały rdzeń na dedicated) | 1 vCPU ≈ 1 hardware thread (Intel HT / AMD SMT) |
| **RAM** | Pamięć operacyjna (GiB) | Nie można zmienić bez zatrzymania instancji |
| **Network bandwidth** | Przepustowość sieci (Gbps) | Baseline + burst dla mniejszych; gwarantowane dla większych |
| **EBS bandwidth** | Przepustowość dla dysków EBS | Wyrażona w MB/s i IOPS |
| **Local NVMe SSD** | Lokalny fizyczny dysk (Instance Store) | Efemeryczny — dane tracone po stop/terminate |
| **GPU** | NVIDIA/AMD/AWS Trainium/Inferentia | Dla ML, renderingu, HPC |
| **FPGA** | Field-Programmable Gate Array | Dla custom hardware acceleration |
| **ENA** | Elastic Network Adapter — high-performance networking | Wymagany dla Enhanced Networking |
| **EFA** | Elastic Fabric Adapter — MPI/HPC networking | Dla klastrów HPC (low-latency, high bandwidth) |

---

### 13.2 Nomenklatura Instancji EC2 — Jak Czytać Nazwę

```
m  7  g  .  2  x  l  a  r  g  e
│  │  │     │  │  │  │  │  │  │
│  │  │     │  │  │  │  │  └──┴─ rozmiar: nano/micro/small/medium/large/xlarge/2xlarge...
│  │  │     │  │  │  │  └──────── a = AMD EPYC
│  │  │     │  │  │  └─────────── r = burst (nie w tym przypadku — to część rozmiaru)
│  │  │     │  │  └────────────── x = extra large modifier
│  │  │     │  └───────────────── 2 = mnożnik rozmiaru
│  │  │     └──────────────────── . = separator
│  │  └────────────────────────── g = Graviton (AWS ARM)
│  └───────────────────────────── 7 = generacja (7. generacja)
└──────────────────────────────── m = rodzina (General Purpose)
```

### Sufiksy — co oznaczają litery po generacji

| Sufiks | Znaczenie | Przykład |
|---|---|---|
| `a` | AMD EPYC processor | `m6a`, `c6a` |
| `g` | AWS Graviton (ARM) | `m7g`, `c7g`, `r7g` |
| `i` | Intel processor (explicit) | `m6i`, `c6i` |
| `n` | Higher network bandwidth | `m5n`, `c5n` |
| `d` | Local NVMe SSD (instance store) | `m5d`, `r5d`, `c5d` |
| `z` | High frequency (Intel, 4 GHz+) | `z1d` |
| `e` | Extra storage or memory | `r5e` (extra RAM) |
| `b` | Block storage optimized | `im4gn`, `is4gen` |
| `q` | Qualcomm inference | (rzadkie) |
| `flex` | Flex instance (burstable) | (nowość) |

### Rozmiary — skala mocy

```
nano → micro → small → medium → large → xlarge → 2xlarge → 4xlarge → 8xlarge → 12xlarge → 16xlarge → 24xlarge → 32xlarge → 48xlarge → metal
```

Każde podwojenie `xlarge` = 2× vCPU + 2× RAM (generalnie).

---

### 13.3 Rodziny Instancji EC2 — Przegląd

### 🟢 General Purpose (t, m)

**Zbalansowane CPU/RAM** — do większości standardowych workloadów.

#### Rodzina T — Burstable Performance

```
T3, T3a, T4g (Graviton), T2 (legacy)
Przykłady: t3.micro, t3.small, t3.medium, t3.large, t3.xlarge, t3.2xlarge
```

**Mechanizm CPU Credits:**

```
CPU Credits accumulate gdy idle → spend gdy potrzebujesz burst
                                        
Baseline: t3.micro = 10% CPU, t3.large = 30% CPU
Burst:    do 100% CPU (dopóki masz kredyty)
Earn:     t3.micro zarabia 6 kredytów/h przy 0% użycia
Spend:    1 kredyt = 1 vCPU × 1 min @ 100%

Tryby:
  Standard:    kredyty przepadają po 7 dniach nieużycia
  Unlimited:   nieograniczony burst → opłata za surplus credits ($0.05/vCPU-h ponad baseline)
```

```bash
# Sprawdź CPU credit balance
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUCreditBalance \
  --dimensions Name=InstanceId,Value=i-12345678 \
  --period 3600 --statistics Average \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-02T00:00:00Z
```

**Kiedy używać T:** Dev/test, małe webapps, idle-with-spikes workloady, microservices. **Kiedy nie:** Sustained high CPU (CI/CD heavy builds, video encoding) — użyj M lub C.

#### Rodzina M — General Purpose (produkcja)

```
M5, M5a, M5n, M6i, M6a, M6g (Graviton), M7i, M7a, M7g
Przykłady: m6i.large (2 vCPU, 8 GiB), m6i.4xlarge (16 vCPU, 64 GiB)
```

Proporcja CPU:RAM = **1:4** (1 vCPU = 4 GiB RAM).

**Kiedy używać M:** Web serwery, aplikacje enterprise (SAP, Oracle), bazy danych średnich rozmiarów, gaming, dev/prod balanced.

---

### 🔵 Compute Optimized (c)

**Wysoki CPU, mało RAM** — proporcja CPU:RAM = **1:2**.

```
C5, C5a, C5n, C6i, C6a, C6g (Graviton), C7i, C7a, C7g
Przykłady: c6i.xlarge (4 vCPU, 8 GiB), c6i.8xlarge (32 vCPU, 64 GiB)
```

**Cechy szczególne:**
- Najwyższy CPU clock speed w standardowych rodzinach
- `C5n`: zwiększony network bandwidth (do 100 Gbps)
- `C7g`: Graviton3 — najlepsza wydajność na watt

**Kiedy używać C:** Batch processing, web serving (nginx, Apache), scientific computing, gaming serwery, HFT (High-Frequency Trading), media transcoding. **NIE dla:** baz danych z dużym RAM, ML inference (użyj G/Inf).

---

### 🔴 Memory Optimized (r, x, u, z)

**Dużo RAM** — dla in-memory baz danych i cache.

#### Rodzina R — Memory Optimized (most popular)

```
R5, R5a, R5b (EBS optimized), R6i, R6a, R6g, R7i, R7a, R7g
Proporcja: 1 vCPU : 8 GiB RAM
Przykłady: r6i.xlarge (4 vCPU, 32 GiB), r6i.32xlarge (128 vCPU, 1024 GiB)
```

#### Rodzina X — High Memory

```
X1, X1e, X2idn, X2iedn, X2iezn
Proporcja: nawet 1:32 (x1e.xlarge: 4 vCPU, 122 GiB RAM!)
Max RAM: x2iedn.metal = 128 vCPU, 4096 GiB (4 TB!) RAM
```

**Kiedy używać X:** SAP HANA (In-Memory), Apache Spark, in-memory analytics, massive databases.

#### Rodzina U — High Memory (bare metal)

```
u-6tb1.metal (448 vCPU, 6 TB RAM)
u-9tb1.metal (448 vCPU, 9 TB RAM)
u-12tb1.metal (448 vCPU, 12 TB RAM)
u-18tb1.metal (896 vCPU, 18 TB RAM)
u-24tb1.metal (896 vCPU, 24 TB RAM)
```

> ⚠️ **Egzamin:** SAP HANA wymaga certyfikowanych instancji (X1, X2, U rodziny). U-series to dedykowane bare-metal dla SAP HANA na AWS.

#### Rodzina Z — High Frequency

```
z1d.large → z1d.metal
1 vCPU : 4 GiB RAM, ale CPU @4 GHz+ (najwyższy zegar w EC2)
```

**Kiedy używać Z:** Elektroniczne symulacje (EDA), CAD, single-threaded workloady z premium na zegar.

---

### 🟡 Storage Optimized (i, d, h, im, is)

**Wysoki IOPS lub wysoka przepustowość lokalnego storage.**

#### Rodzina I — NVMe SSD (wysoki IOPS)

```
I3, I3en, I4i, I4g (Graviton)
i3.large: 2 vCPU, 15.25 GiB, 1× 475 GB NVMe SSD, 425K IOPS
i3en.24xlarge: 96 vCPU, 768 GiB, 8× 7500 GB NVMe SSD, 4.2M IOPS
```

**Kiedy używać I:** NoSQL bazy danych (Cassandra, MongoDB), Elasticsearch, in-memory cache (Redis), high IOPS transactional databases.

#### Rodzina D — Dense HDD (wysoka przepustowość)

```
D2, D3, D3en
d3.xlarge: 4 vCPU, 32 GiB, 3× 2 TB HDD
d3.8xlarge: 32 vCPU, 256 GiB, 24× 2 TB HDD = 48 TB local storage!
```

**Kiedy używać D:** Hadoop/MapReduce, data warehousing, distributed file systems (Lustre), log processing.

#### Rodzina H — High Throughput HDD

```
H1: duże dyski HDD, throughput-focused
```

#### Rodzina IM/IS — Storage Optimized z Graviton

```
im4gn, is4gen — Graviton2 + NVMe SSD
Dobry stosunek ceny do wydajności storage
```

---

### 🟣 Accelerated Computing (p, g, trn, inf, f, vt)

**GPU, FPGA, AWS custom chips** — dla ML, AI, grafiki, wideo.

#### Rodzina P — GPU (Training ML)

```
P2 (NVIDIA K80, legacy)
P3 (NVIDIA V100): p3.2xlarge (1 GPU), p3.16xlarge (8× V100)
P4d (NVIDIA A100): p4d.24xlarge (8× A100 40GB, 400 Gbps NVLink+EFA)
P5 (NVIDIA H100): p5.48xlarge (8× H100 80GB, 3200 Gbps NVLink+EFA)
```

**Kiedy używać P:** Deep learning training, scientific HPC, molecular dynamics.

#### Rodzina G — GPU (Inference + Graphics)

```
G3 (NVIDIA M60, legacy — wirtualizacja grafiki)
G4dn (NVIDIA T4): g4dn.xlarge (1× T4), g4dn.12xlarge (4× T4)
G4ad (AMD Radeon Pro V520): tańsza alternatywa
G5 (NVIDIA A10G): g5.xlarge (1× A10G), g5.48xlarge (8× A10G)
G6 (NVIDIA L4): najnowsza, cost-effective inference
```

**Kiedy używać G:** ML inference, game streaming (GameLift), 3D visualization, video transcoding.

#### Rodzina Trn — AWS Trainium (Training)

```
trn1.2xlarge (2× Trainium chips)
trn1.32xlarge (16× Trainium chips + 800 Gbps EFA)
trn1n.32xlarge (16× Trainium + higher network)
```

**AWS Trainium** to custom ASIC zoptymalizowany pod deep learning training. Do 50% tańszy niż P3 dla tego samego workloadu.

#### Rodzina Inf — AWS Inferentia (Inference)

```
inf1.xlarge (1× Inferentia chip)
inf1.6xlarge (4× Inferentia)
inf1.24xlarge (16× Inferentia)
inf2 (Inferentia2): inf2.xlarge → inf2.48xlarge
```

**AWS Inferentia** = custom ASIC dla inference. Do 70% taniej niż GPU instancje dla inference workloadów.

#### Rodzina F — FPGA

```
f1.2xlarge (1× FPGA Xilinx VU9P)
f1.16xlarge (8× FPGA)
```

**Kiedy używać F:** Genomics acceleration, financial analytics, video processing, custom hardware logic.

#### Rodzina VT — Video Transcoding

```
vt1.3xlarge, vt1.6xlarge, vt1.24xlarge
Xilinx Alveo U30 media accelerators
```

**Kiedy używać VT:** Live video transcoding (H.264, H.265), media workflows.

---

### ⚫ High Performance Computing (hpc)

```
hpc6a.48xlarge (96 vCPU AMD EPYC, 384 GiB, 100 Gbps EFA)
hpc7g.16xlarge (64 vCPU Graviton3E, 128 GiB, 200 Gbps EFA)
```

**Kiedy używać HPC:** CFD (Computational Fluid Dynamics), weather modeling, molecular dynamics, seismic analysis.

---

### 13.4 Tabela Porównawcza Rodzin — Quick Reference

| Rodzina | CPU:RAM | Przypadek użycia | Przykład |
|---|---|---|---|
| **t** (burstable) | 1:2–1:4 | Dev/test, low-traffic web | t3.micro |
| **m** (general) | 1:4 | Web serwery, app serwery | m6i.xlarge |
| **c** (compute) | 1:2 | Batch, HPC light, web | c6i.xlarge |
| **r** (memory) | 1:8 | Redis, Memcached, in-mem DB | r6i.xlarge |
| **x** (high mem) | 1:16–1:32 | SAP HANA, Apache Spark | x2iedn.xlarge |
| **u** (ultra mem) | 1:16 | SAP HANA 6-24 TB | u-6tb1.metal |
| **z** (high freq) | 1:4 @4GHz | EDA, single-thread perf | z1d.xlarge |
| **i** (NVMe SSD) | 1:4+NVMe | Cassandra, Elasticsearch | i3.xlarge |
| **d** (dense HDD) | 1:4+HDD | Hadoop, data warehouse | d3.xlarge |
| **p** (GPU train) | 1:4+GPU | DL training | p4d.24xlarge |
| **g** (GPU infer) | 1:4+GPU | ML inference, gaming | g5.xlarge |
| **trn** (Trainium) | custom | DL training (AWS chip) | trn1.2xlarge |
| **inf** (Inferentia) | custom | DL inference (AWS chip) | inf2.xlarge |
| **f** (FPGA) | 1:4+FPGA | Custom acceleration | f1.2xlarge |
| **hpc** (HPC) | 1:4+EFA | CFD, molecular dynamics | hpc7g.16xlarge |

---

### 13.5 Modele Zakupu EC2 — Koszty i Optymalizacja

### On-Demand

**Płacisz za sekundę** (minimum 60 sekund) bez zobowiązań. Najwyższe ceny, maksymalna elastyczność.

```bash
# Cena On-Demand (eu-west-1, Linux)
aws pricing get-products \
  --service-code AmazonEC2 \
  --filters \
    "Type=TERM_MATCH,Field=instanceType,Value=m6i.xlarge" \
    "Type=TERM_MATCH,Field=location,Value=EU (Ireland)" \
    "Type=TERM_MATCH,Field=tenancy,Value=Shared" \
    "Type=TERM_MATCH,Field=operatingSystem,Value=Linux" \
    "Type=TERM_MATCH,Field=preInstalledSw,Value=NA" \
  --query 'PriceList[0]'

# Przykładowe ceny (eu-west-1, Linux On-Demand):
# t3.micro:    $0.0116/h   ≈ $8/mc
# m6i.xlarge:  $0.214/h    ≈ $155/mc
# c6i.4xlarge: $0.736/h    ≈ $532/mc
# r6i.2xlarge: $0.672/h    ≈ $485/mc
# p3.2xlarge:  $3.823/h    ≈ $2754/mc
```

**Kiedy używać:** Nieregularne workloady, dev/test, krótkie eksperymenty, baseline dla porównania.

---

### Reserved Instances (RI)

**Zobowiązanie 1 lub 3 lata** w zamian za rabat 30–72%.

```
Typy RI:
  Standard RI:     do 72% taniej, MOŻNA sprzedać na Marketplace
  Convertible RI:  do 66% taniej, MOŻNA zmienić rodzinę/OS/tenancy
  
Opcje płatności:
  All Upfront:     Najwyższy rabat (płacisz wszystko z góry)
  Partial Upfront: Średni rabat (część z góry + miesięczna rata)
  No Upfront:      Najniższy rabat (tylko miesięczna rata)
```

| | 1 rok All Upfront | 1 rok No Upfront | 3 lata All Upfront |
|---|---|---|---|
| **Rabat vs On-Demand** | ~40% | ~35% | ~60-72% |
| **Elastyczność** | Niska | Średnia | Najniższa |
| **Gdy opłacalne** | Przewidywalny workload | Cashflow ważny | Długoterminowy, stabilny |

```bash
# Kup Reserved Instance
aws ec2 purchase-reserved-instances-offering \
  --reserved-instances-offering-id abc123 \
  --instance-count 2

# Sprawdź dostępne oferty RI
aws ec2 describe-reserved-instances-offerings \
  --instance-type m6i.xlarge \
  --product-description "Linux/UNIX" \
  --offering-class standard \
  --filters "Name=duration,Values=31536000"  # 1 rok w sekundach
```

> ⚠️ **Egzamin:** RI **nie przypisuje się** do konkretnej instancji — AWS automatycznie stosuje rabat do pasującej uruchomionej instancji. Standard RI można sprzedać na **Reserved Instance Marketplace**. Convertible RI — nie można sprzedać, ale można zmienić.

---

### Savings Plans

**Nowszy i elastyczniejszy zamiennik RI** — zobowiązujesz się do wydatku w dolarach/godzinę ($X/h), nie do konkretnego type.

```
Compute Savings Plans:
  - Najszersze pokrycie: EC2 dowolna rodzina/region/OS + Fargate + Lambda
  - Rabat: do 66%
  - Najlepsza elastyczność

EC2 Instance Savings Plans:
  - Konkretna rodzina + region (np. M w eu-west-1)
  - Rabat: do 72%
  - Możesz zmieniać rozmiar, OS, AZ wewnątrz rodziny

SageMaker Savings Plans:
  - Tylko dla SageMaker
```

```bash
# Rekomendacje Savings Plans (AWS Cost Explorer)
aws ce get-savings-plans-purchase-recommendation \
  --savings-plans-type COMPUTE_SP \
  --term-in-years ONE_YEAR \
  --payment-option ALL_UPFRONT \
  --lookback-period-in-days THIRTY_DAYS
```

**Compute Savings Plans vs Reserved Instances:**

| | Savings Plans (Compute) | Reserved Instances (Standard) |
|---|---|---|
| **Pokrycie** | EC2 + Fargate + Lambda | Tylko EC2 |
| **Elastyczność** | Dowolna rodzina/region | Konkretny typ/region |
| **Rabat** | do 66% | do 72% |
| **Zmiana** | Automatyczna (zmień workload) | Wymaga sprzedaży/exchange |
| **Zalecane** | Nowe projekty, dynamic | Legacy, przewidywalne |

---

### Spot Instances

**Nieużywana pojemność AWS** sprzedawana ze zniżką **60–90% vs On-Demand**. AWS może przerwać instancję z 2-minutowym ostrzeżeniem.

```bash
# Sprawdź aktualne ceny Spot
aws ec2 describe-spot-price-history \
  --instance-types c6i.xlarge m6i.xlarge r6i.xlarge \
  --product-descriptions "Linux/UNIX" \
  --start-time 2024-01-01T00:00:00Z \
  --query 'SpotPriceHistory[*].{Type:InstanceType,Price:SpotPrice,AZ:AvailabilityZone}'

# Launch Spot Instance
aws ec2 run-instances \
  --image-id ami-12345678 \
  --instance-type c6i.xlarge \
  --instance-market-options '{"MarketType":"spot","SpotOptions":{"MaxPrice":"0.10","SpotInstanceType":"one-time"}}'
```

**Typy Spot:**

| Typ | Opis |
|---|---|
| **One-time** | Uruchamia się gdy cena ≤ max, kończy gdy cena przekroczy lub gdy terminacja |
| **Persistent** | Automatycznie się restartuje gdy znowu dostępna |

**Spot Interruption (2-min warning):**

```bash
# Zapis na instancji — sprawdź czy będzie terminated
curl -s http://169.254.169.254/latest/meta-data/spot/termination-time
# Jeśli odpowiedź = 404: brak zaplanowanej terminacji
# Jeśli odpowiedź = timestamp: za ~2 min terminacja

# CloudWatch Event dla Spot Interruption
aws events put-rule \
  --name "SpotInterruptionRule" \
  --event-pattern '{"source":["aws.ec2"],"detail-type":["EC2 Spot Instance Interruption Warning"]}'
```

**Spot Fleet i EC2 Fleet:**

```bash
# Spot Fleet — wiele typów instancji dla elastyczności
aws ec2 request-spot-fleet \
  --spot-fleet-request-config '{
    "IamFleetRole": "arn:aws:iam::123456789012:role/aws-ec2-spot-fleet-role",
    "AllocationStrategy": "capacityOptimized",
    "TargetCapacity": 10,
    "LaunchSpecifications": [
      {"InstanceType": "c6i.xlarge", "ImageId": "ami-xxx"},
      {"InstanceType": "c5.xlarge", "ImageId": "ami-xxx"},
      {"InstanceType": "c5a.xlarge", "ImageId": "ami-xxx"}
    ]
  }'

# Strategie alokacji:
# lowestPrice:         najtańsze (ryzyko interruption)
# capacityOptimized:   największa dostępna pojemność (mniej interruptions)
# priceCapacityOptimized: balans ceny i pojemności (zalecane)
# diversified:         rozłóż po pulach
```

**Kiedy używać Spot:** Batch processing, big data (EMR), CI/CD workers, fault-tolerant distributed apps, ML training (Spot + checkpointing), rendering.

**Kiedy NIE używać Spot:** Bazy danych, stany sesji, krytyczne serwisy produkcyjne bez HA.

---

### Dedicated Hosts

**Fizyczny serwer wyłącznie dla Ciebie** — pełna kontrola nad socket/core placement.

```bash
# Allocate Dedicated Host
aws ec2 allocate-hosts \
  --availability-zone eu-west-1a \
  --instance-type m6i.xlarge \
  --quantity 1 \
  --auto-placement on

# On-Demand: $4.624/h dla m6i Dedicated Host (eu-west-1)
# Mieści np. 32× m6i.xlarge lub 1× m6i.metal
```

**Kiedy używać Dedicated Hosts:**
- **BYOL (Bring Your Own License)** — Windows Server, SQL Server, RHEL licencjonowane per-socket/per-core
- Compliance (PCI DSS, HIPAA) wymagający fizycznej izolacji
- Regulatory requirements

---

### Dedicated Instances

**Instancje na dedykowanym hardwarze** (shared tylko z Twoim kontem), ale bez kontroli nad socket placement.

| | Dedicated Host | Dedicated Instance |
|---|---|---|
| **Fizyczna izolacja** | ✅ Per host | ✅ Per account |
| **Socket/core visibility** | ✅ Pełna | ❌ |
| **BYOL support** | ✅ | ❌ (nie wszystkie licencje) |
| **Koszt** | Hourly per host | Dodatkowa opłata per-instance + $2/h per region fee |
| **Kontrola placement** | ✅ | ❌ |

---

### Capacity Reservations

**Zarezerwowana pojemność** w konkretnej AZ — bez zobowiązania cenowego (płacisz On-Demand rate).

```bash
# Zarezerwuj pojemność — gwarantuje dostępność instancji
aws ec2 create-capacity-reservation \
  --availability-zone eu-west-1a \
  --instance-type m6i.xlarge \
  --instance-platform Linux/UNIX \
  --instance-count 10 \
  --instance-match-criteria targeted  # tylko explicit requests

# Łącz z Savings Plans lub RI dla rabatu + pojemności
```

**Kiedy:** Disaster recovery, gwarantowana pojemność dla krytycznych aplikacji podczas AWS capacity crunches.

---

### 13.6 Porównanie Wszystkich Modeli Cenowych

```mermaid
graph LR
    START["Jaki workload?"] --> Q1{"Przewidywalny\nczas pracy?"}
    
    Q1 -->|Tak, ciągły| Q2{"Elastyczność\nważna?"}
    Q1 -->|Nie, sporadyczny| Q3{"Fault\ntolerant?"}
    
    Q2 -->|Elastyczność| SP["Savings Plans\nCompute\ndo 66% rabat"]
    Q2 -->|Stabilny typ| RI["Reserved Instances\nStandard\ndo 72% rabat"]
    
    Q3 -->|Tak| SPOT["Spot Instances\ndo 90% rabat"]
    Q3 -->|Nie| OD["On-Demand\npłać za sekundę"]
    
    RI --> BYOL{"BYOL lub\nCompliance?"}
    BYOL -->|Tak| DH["Dedicated Host\npłać za host"]
    BYOL -->|Nie| RI
```

| Model | Rabat | Zobowiązanie | Przerwanie | Najlepszy dla |
|---|---|---|---|---|
| **On-Demand** | 0% | Brak | Nie | Dev/test, nieregularne |
| **Savings Plans** | 66% | 1 lub 3 lata ($) | Nie | Elastyczny steady-state |
| **RI Standard** | 72% | 1 lub 3 lata (typ) | Nie | Stabilny, znany typ |
| **RI Convertible** | 66% | 1 lub 3 lata | Nie | Stabilny, zmienne typy |
| **Spot** | 90% | Brak | ✅ 2 min warning | Fault-tolerant, batch |
| **Dedicated Host** | ~30-40% (RI) | Opcjonalne | Nie | BYOL, compliance |
| **Capacity Reservation** | 0% | Brak | Nie | Guaranteed capacity |

---

### 13.7 Storage dla EC2

### EBS — Elastic Block Store

**Sieciowy dysk blokowy** — persystentny, niezależny od czasu życia instancji.

| Typ EBS | Użycie | Max IOPS | Max throughput | Cena |
|---|---|---|---|---|
| **gp3** (General Purpose SSD) | Domyślny, boot, apps | 16 000 | 1 000 MB/s | $0.08/GB-mc |
| **gp2** (legacy) | Stary domyślny | 16 000 | 250 MB/s | $0.10/GB-mc |
| **io1** (Provisioned IOPS SSD) | Bazy danych | 64 000 | 1 000 MB/s | $0.125/GB + $0.065/IOPS |
| **io2 Block Express** | Mission-critical DB | 256 000 | 4 000 MB/s | $0.125/GB + $0.065/IOPS |
| **st1** (Throughput HDD) | Big data, log | 500 | 500 MB/s | $0.045/GB-mc |
| **sc1** (Cold HDD) | Archiwum, rzadki dostęp | 250 | 250 MB/s | $0.025/GB-mc |

> ⚠️ **Egzamin:** `gp3` oddzielnie ustawia IOPS i throughput (niezależnie od rozmiaru). `gp2` IOPS = 3× GB (max 16000). `io2 Block Express` obsługuje multi-attach (wiele EC2 → jeden wolumen).

### Instance Store

**Fizyczny dysk NVMe SSD** wbudowany w host. Efemeryczny.

```
Zalety:   Bardzo niski latency (brak sieci), bezpłatny (wliczony w cenę instancji)
Wady:     Dane tracone przy: stop, terminate, awaria hosta
Użycie:   Cache, tymczasowe dane, swap, Hadoop HDFS tmp
```

### EFS — Elastic File System

**Sieciowy shared filesystem** (NFS) — wiele EC2 może montować ten sam filesystem.

```bash
# Mount EFS na EC2
sudo mount -t efs -o tls fs-12345678:/ /mnt/efs
```

| | EBS | EFS | S3 |
|---|---|---|---|
| **Typ** | Block | File (NFS) | Object |
| **Dostęp** | 1 EC2 (io2: multi-attach) | Wiele EC2 | HTTP API |
| **Scope** | AZ | Regional | Global |
| **Persystencja** | ✅ | ✅ | ✅ |
| **Cena** | $0.08-0.125/GB | $0.30/GB | $0.023/GB |

---

### 13.8 Sieć EC2 — ENI, Enhanced Networking, Placement Groups

### ENI (Elastic Network Interface)

```bash
# EC2 może mieć wiele ENI (np. dla multi-homed routing)
aws ec2 create-network-interface \
  --subnet-id subnet-12345678 \
  --groups sg-12345678 \
  --description "Secondary ENI"

aws ec2 attach-network-interface \
  --instance-id i-12345678 \
  --network-interface-id eni-12345678 \
  --device-index 1
```

### Enhanced Networking (ENA/SR-IOV)

- **ENA (Elastic Network Adapter):** Do 100 Gbps, niski latency, low CPU overhead — domyślnie na wszystkich nowszych instancjach
- **SR-IOV:** Hardware virtualization dla sieci — mniej overhead vs software emulation
- **EFA (Elastic Fabric Adapter):** MPI-capable network dla HPC klastrów, RDMA

### Placement Groups

```
Cluster Placement Group:
  - Wszystkie instancje w jednej AZ, blisko siebie (niski latency)
  - Do 100 Gbps między instancjami
  - Ryzyko: awaria AZ = wszystkie instancje
  - Użycie: HPC, ML distributed training, low-latency apps

Spread Placement Group:
  - Każda instancja na INNYM rack (różne zasilanie i sieć)
  - Max 7 instancji per AZ per group
  - Maksymalna odporność na hardware failure
  - Użycie: HA dla małych grup (np. ZooKeeper, Kafka brokers)

Partition Placement Group:
  - Instancje w partycjach (każda partycja = osobny rack)
  - Do 7 partycji per AZ
  - Skalowanie przy zachowaniu izolacji fault domains
  - Użycie: Hadoop, Cassandra, Kafka (duże clustry)
```

```bash
# Utwórz Cluster Placement Group
aws ec2 create-placement-group \
  --group-name MyHPCCluster \
  --strategy cluster

# Launch instancji w PG
aws ec2 run-instances \
  --placement '{"GroupName":"MyHPCCluster"}' \
  --instance-type c6in.32xlarge \
  --image-id ami-12345678
```

---

### 13.9 EC2 — Limity i Ograniczenia

| Limit | Wartość domyślna | Notatka |
|---|---|---|
| **Running On-Demand vCPU** (Standard) | 32 vCPU | Soft limit — można zwiększyć |
| **Running On-Demand vCPU** (per family) | Osobny limit per A/C/D/F/G/I/M/P/R/T/X | Sprawdź Service Quotas |
| **Spot Requests** | 640 vCPU | Per region, per type |
| **EBS volumes per instance** | 40 | |
| **ENI per instance** | Zależy od typu (np. m6i.xlarge = 4) | |
| **Elastic IPs** | 5 per region | |
| **Max EBS volume size** | 64 TiB (io2 Block Express) | |
| **AMI per region** | 1000 private AMI | |

```bash
# Sprawdź aktualne limity
aws service-quotas list-service-quotas \
  --service-code ec2 \
  --query 'Quotas[?contains(QuotaName, `vCPU`)].{Name:QuotaName,Value:Value}'
```

---

### 13.10 Well-Architected Framework — EC2

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | AMI golden images (standard baseline), SSM Session Manager zamiast SSH, CloudWatch agent dla metryk, EC2 Instance Connect, automatyzacja przez Launch Templates |
| **Security** | IMDSv2 (nie IMDSv1) dla metadata access, Security Groups least-privilege, IAM Instance Profile (nie access keys na instancji), EBS encryption domyślne, VPC private subnets dla EC2 |
| **Reliability** | Multi-AZ deployment (ASG), Spot Fleet z wieloma typami, Placement Groups dla HA (Spread), EBS Multi-Attach dla shared storage, Capacity Reservations dla krytycznych workloadów |
| **Performance Efficiency** | Graviton3 (do 40% lepszy stosunek ceny do wydajności), Instance Store dla ephemeral cache, Enhanced Networking (ENA), właściwa rodzina instancji (C dla CPU, R dla RAM, I dla IOPS), ECMP dla sieciowych workloadów |
| **Cost Optimization** | Savings Plans lub RI dla steady-state, Spot dla batch/fault-tolerant, T3 Unlimited tylko gdy potrzebny burst (nie domyślnie), AWS Compute Optimizer (zalecenia typów), Graviton gdzie kompatybilny |
| **Sustainability** | Graviton (lepszy PUE), right-sizing (Compute Optimizer), Spot zmniejsza waste (używa idle capacity), konsolidacja małych instancji w duże (gdzie możliwe) |

---

### 13.11 4 Domeny Egzaminacyjne SAP-C02

### Domena 1: Design Solutions for Organizational Complexity

- **Multi-account EC2 governance:** Service Control Policies (SCP) limitujące typy instancji, AWS Config rules dla `ec2-instance-no-public-ip`, Tagging policies dla kosztów
- **Reserved Instance sharing:** RI zakupione w AWS Organizations Management Account automatycznie shareowane do member accounts
- **Savings Plans**: zarządzanie z management account, apply do całej organizacji

### Domena 2: Design for New Solutions

- **Wybór rodziny instancji:** T dla dev/test, M dla produkcja general, C dla CPU-intensive, R dla in-memory DB, G/Trn/Inf dla AI/ML
- **Graviton3 jako default:** 40% taniejszy per performance, ale sprawdź kompatybilność oprogramowania (ARM vs x86)
- **EFA dla HPC/ML:** Gdy potrzebujesz >100 Gbps między instancjami lub MPI
- **Placement Groups:** Cluster dla latency, Spread dla HA

### Domena 3: Continuous Improvement for Existing Solutions

- **Compute Optimizer:** Automatyczne zalecenia right-sizing (na podstawie 14-dniowych metryk CloudWatch)
- **Migration t2 → t3:** t3 tańszy, lepszy, Unlimited stabilniejszy, brak opłaty za ENA
- **gp2 → gp3:** gp3 tańszy ($0.08 vs $0.10/GB) i lepszy (niezależny IOPS config)
- **Spot migration pattern:** Użyj ASG Mixed Instances Policy (On-Demand baseline + Spot dla scale)

### Domena 4: Accelerate Workload Migration and Modernization

- **AWS Migration Hub + AWS Application Migration Service (MGN):** Lift-and-shift EC2 z on-prem
- **Instance type matching:** Użyj `vmimport/export` lub MGN — automatycznie dobiera EC2 type
- **Modernization:** Lift to EC2 → refactor do ECS/Fargate lub Lambda (gdzie możliwe)
- **Graviton migration:** Kompiluj aplikację dla ARM, testuj kompatybilność, oszczędź ~40%

---

## 14. Elastic Load Balancing — ALB, NLB, GWLB, CLB

> **Zakres:** Kompletny przegląd czterech typów load balancerów AWS — Application (ALB), Network (NLB), Gateway (GWLB) i Classic (CLB legacy). Architektura, algorytmy, sticky sessions, SSL/TLS offloading, health checks, integracje, koszty.

---

### 14.1 Co to jest ELB i po co istnieje?

**Elastic Load Balancing (ELB)** to zarządzana usługa AWS rozdzielająca ruch przychodzący między wiele celów (EC2, kontenery, IP, Lambda). AWS zarządza całą infrastrukturą load balancera — HA, skalowanie, patche.

**Analogia z życia:** ELB to jak recepcja w dużym hotelu z wieloma liniami — recepcjonista (ELB) kieruje każdego gościa (request) do wolnego pokoju (serwera). Gdy jeden pokój jest zajęty (unhealthy), gość automatycznie trafia do innego.

**Odpowiednik Azure:** Azure Load Balancer (L4, odpowiednik NLB) + Azure Application Gateway (L7, odpowiednik ALB) + Azure Gateway Load Balancer (GWLB).

**Odpowiednik open-source:** HAProxy, nginx, Envoy, Traefik.

### Dlaczego ELB, a nie własny load balancer na EC2?

| Cecha | ELB (managed) | Własny HAProxy/nginx |
|---|---|---|
| **HA** | ✅ Wbudowane, multi-AZ | Ręczna konfiguracja |
| **Skalowanie** | ✅ Automatyczne | Ręczne |
| **SSL termination** | ✅ Wbudowane (ACM) | Ręczna konfiguracja |
| **Health checks** | ✅ Wbudowane | Ręczna konfiguracja |
| **Integracje AWS** | ✅ Auto Scaling, ECS, WAF | Brak |
| **Koszt** | $/h + LCU | EC2 (wyższy baseline) |
| **Patche/maintencance** | ✅ AWS zarządza | Ręczna |
| **Kiedy własny** | — | Specyficzne funcje, legacy |

---

### 14.2 Cztery Typy Load Balancerów — Przegląd

```mermaid
graph TD
    subgraph ELB["Elastic Load Balancing — Typy"]
        ALB["🔵 ALB\nApplication Load Balancer\nLayer 7 (HTTP/HTTPS/gRPC)\nContent-based routing"]
        NLB["🟢 NLB\nNetwork Load Balancer\nLayer 4 (TCP/UDP/TLS)\nUltra-low latency"]
        GWLB["🟡 GWLB\nGateway Load Balancer\nLayer 3 (IP packets)\nVirtual appliances"]
        CLB["⚫ CLB\nClassic Load Balancer\nLayer 4+7 (legacy)\nNIE używaj w nowych projektach"]
    end
```

| | ALB | NLB | GWLB | CLB |
|---|---|---|---|---|
| **Warstwa OSI** | L7 (aplikacja) | L4 (transport) | L3 (sieć) | L4+L7 (stary) |
| **Protokoły** | HTTP, HTTPS, gRPC, WebSocket | TCP, UDP, TLS | IP | HTTP, HTTPS, TCP |
| **Routing** | Content-based (path, host, header) | IP:Port | GENEVE tunnel | Basic |
| **Latency** | ~ms (wyższy) | ~µs (bardzo niski) | ~µs | ms |
| **Static IP** | ❌ (DNS name) | ✅ (per-AZ EIP) | ❌ | ❌ |
| **SSL offload** | ✅ | ✅ (TLS termination) | ❌ | ✅ |
| **WAF integracja** | ✅ | ❌ | ❌ | ❌ |
| **Lambda target** | ✅ | ❌ | ❌ | ❌ |
| **IP target** | ✅ | ✅ | ✅ | ❌ |
| **Zakres** | Regional | Regional | Regional | Regional |
| **Status** | ✅ Aktywny | ✅ Aktywny | ✅ Aktywny | ⚠️ Legacy |

---

### 14.3 ALB — Application Load Balancer

### Architektura i działanie

ALB operuje na **warstwie 7** — rozumie HTTP/HTTPS. Może routować ruch na podstawie zawartości requestu (URL path, hostname, headers, query strings, metody HTTP).

```
Klient
  │
  ▼
ALB (jeden DNS name, multi-AZ)
  │
  ├── Listener: :443 (HTTPS)
  │     │
  │     ├── Rule 1: path=/api/*  → Target Group: API Servers (EC2)
  │     ├── Rule 2: path=/images/* → Target Group: Static Servers (EC2)
  │     ├── Rule 3: host=mobile.example.com → Target Group: Mobile App
  │     └── Default: → Target Group: Web Servers (EC2)
  │
  └── Listener: :80 (HTTP)
        └── Rule: Redirect → https://#{host}/#{path}
```

### Komponenty ALB

**Listener:**
- Sprawdza przychodzące połączenia (port + protokół)
- Ocenia reguły (rules) w kolejności priorytetu
- Może być HTTP lub HTTPS

**Rules (reguły routingu):**
```
Warunki (AND logic):
  - host-header: mobile.example.com
  - path-pattern: /api/*
  - http-header: X-Custom-Header = value
  - http-request-method: POST
  - query-string: version=2
  - source-ip: 10.0.0.0/8

Akcje:
  - forward → Target Group (z wagami)
  - redirect → inny URL (301/302)
  - fixed-response → zwróć statyczną odpowiedź (200/404/503 + body)
  - authenticate-cognito → wymagaj logowania przez Cognito
  - authenticate-oidc → wymagaj logowania przez OIDC provider
```

```bash
# Utwórz ALB
aws elbv2 create-load-balancer \
  --name my-alb \
  --subnets subnet-aaa subnet-bbb \    # min 2 różne AZ
  --security-groups sg-12345678 \
  --scheme internet-facing \           # lub internal
  --type application

# Utwórz Target Group
aws elbv2 create-target-group \
  --name my-tg \
  --protocol HTTP \
  --port 80 \
  --vpc-id vpc-12345678 \
  --target-type instance \             # instance | ip | lambda | alb
  --health-check-path /health \
  --health-check-interval-seconds 30

# Dodaj regułę routingu
aws elbv2 create-rule \
  --listener-arn arn:aws:elasticloadbalancing:... \
  --priority 10 \
  --conditions '[{"Field":"path-pattern","Values":["/api/*"]}]' \
  --actions '[{"Type":"forward","TargetGroupArn":"arn:..."}]'
```

### Target Types

| Target Type | Opis | Kiedy |
|---|---|---|
| **instance** | EC2 instancje po ID | Tradycyjne EC2 apps |
| **ip** | Dowolny IP (VPC, on-prem przez DX/VPN) | Kontenery (ECS), on-prem, multi-VPC |
| **lambda** | AWS Lambda function | Serverless backend |
| **alb** | Inny ALB (nested) | Multi-tenant, micro-frontend isolation |

> ⚠️ **Egzamin:** Typ `ip` pozwala targetować **prywatne IP** — EC2 w innym VPC (przez Peering), on-prem serwery (przez DX/VPN). Używane w ECS gdy kontener ma dynamiczne IP.

### Weighted Target Groups (Traffic Shifting)

```bash
# Blue/Green lub Canary deployment przez wagi
aws elbv2 modify-rule \
  --rule-arn arn:... \
  --actions '[{
    "Type": "forward",
    "ForwardConfig": {
      "TargetGroups": [
        {"TargetGroupArn": "arn:...blue", "Weight": 90},
        {"TargetGroupArn": "arn:...green", "Weight": 10}
      ],
      "TargetGroupStickinessConfig": {"Enabled": false}
    }
  }]'

# 90% → stara wersja (blue), 10% → nowa wersja (green)
# Stopniowo zmieniaj wagi: 80/20 → 50/50 → 0/100
```

### Sticky Sessions (Session Affinity)

```
Bez sticky sessions:
  User request 1 → Server A
  User request 2 → Server B  ← różny serwer, utrata sesji!

Ze sticky sessions (duration-based):
  User request 1 → Server A  ← cookie AWSALB ustawiony
  User request 2 → Server A  ← cookie = ta sama instancja
  User request 3 → Server A  ← przez X sekund (max 7 dni)

Ze sticky sessions (application-based):
  Twoja aplikacja kontroluje cookie → ALB czyta twój cookie
```

```bash
aws elbv2 modify-target-group-attributes \
  --target-group-arn arn:... \
  --attributes \
    Key=stickiness.enabled,Value=true \
    Key=stickiness.type,Value=lb_cookie \   # lub app_cookie
    Key=stickiness.lb_cookie.duration_seconds,Value=86400
```

> ⚠️ **Egzamin:** Sticky sessions = mniej equal distribution, potencjalnie nieoptymalne balansowanie. Lepiej: trzymaj sesję w zewnętrznym storage (Redis/DynamoDB) i nie używaj sticky.

### SSL/TLS Termination i SNI

```
ALB z SNI (Server Name Indication):
  - Jeden ALB może obsługiwać WIELE certyfikatów SSL
  - TLS rozróżnia który certyfikat na podstawie hostname (SNI)
  - Max 25 certyfikatów per listener (można zwiększyć)

Certyfikaty: z AWS ACM (bezpłatne, auto-renew) lub importowane

Polityki SSL (security policy):
  ELBSecurityPolicy-TLS13-1-2-2021-06  (TLS 1.3 + 1.2, rekomendowane)
  ELBSecurityPolicy-2016-08            (TLS 1.0-1.2, legacy)
```

```bash
# Dodaj certyfikat do Listener
aws elbv2 add-listener-certificates \
  --listener-arn arn:... \
  --certificates CertificateArn=arn:aws:acm:eu-west-1:123:certificate/abc123
```

### Connection Draining / Deregistration Delay

```
Gdy instancja wyrejestrowana z TG (scale-in, deploy):
  - ALB przestaje wysyłać NOWE requesty do tej instancji
  - Ale czeka na dokończenie IN-FLIGHT requestów
  - Domyślnie: 300 sekund (5 minut)
  - Zakres: 1-3600s (ustaw niski dla szybkiego deploymentu)

aws elbv2 modify-target-group-attributes \
  --attributes Key=deregistration_delay.timeout_seconds,Value=30
```

### ALB Access Logs

```bash
# Włącz Access Logs → S3
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn arn:... \
  --attributes \
    Key=access_logs.s3.enabled,Value=true \
    Key=access_logs.s3.bucket,Value=my-alb-logs \
    Key=access_logs.s3.prefix,Value=alb

# Format logów: timestamp, elb, client:port, target:port, 
# request_processing_time, target_processing_time, 
# response_processing_time, elb_status_code, target_status_code,
# received_bytes, sent_bytes, request, user_agent, ssl_cipher...
```

### ALB — Headers i X-Forwarded

```
ALB dodaje do requestu:
  X-Forwarded-For: <original-client-IP>    ← prawdziwy IP klienta
  X-Forwarded-Proto: https                 ← oryginalny protokół
  X-Forwarded-Port: 443                    ← oryginalny port

Twoja aplikacja może sprawdzić X-Forwarded-For zamiast source IP
(bo source IP do EC2 = IP ALB node, nie klienta)
```

---

### 14.4 NLB — Network Load Balancer

### Architektura i działanie

NLB operuje na **warstwie 4** — widzi tylko TCP/UDP/TLS, nie rozumie HTTP. Ultraszybki (mikrosekundowe latency), zachowuje source IP klienta.

```
Klient (IP: 1.2.3.4)
  │
  ▼
NLB (Static IP per AZ — Elastic IP)
  │
  ├── Listener: :443 (TLS)
  │     └── Target Group: TLS / TCP 443 → EC2 backends
  │
  ├── Listener: :53 (UDP)
  │     └── Target Group: UDP 53 → DNS servers
  │
  └── Listener: :80 (TCP)
        └── Target Group: TCP 80 → HTTP servers

EC2 widzi: source IP = 1.2.3.4 (zachowane!)
```

### Kluczowe cechy NLB

**Static IP per AZ:**
```bash
# NLB automatycznie dostaje Elastic IP per AZ
# → niezmienne IP nawet przy skalowaniu
# Przydatne: firewalle on-prem które whitelist-ują po IP

aws elbv2 create-load-balancer \
  --name my-nlb \
  --type network \
  --subnets subnet-aaa subnet-bbb \
  --subnet-mappings \
    SubnetId=subnet-aaa,AllocationId=eipalloc-11111 \  # własny EIP
    SubnetId=subnet-bbb,AllocationId=eipalloc-22222
```

**Zachowanie source IP klienta:**
```
NLB TCP/UDP: source IP = prawdziwy IP klienta (nie NLB IP!)
NLB TLS termination: source IP zachowane
ALB: source IP = IP ALB node (musisz użyć X-Forwarded-For)

Ważne: Security Groups na EC2 muszą zezwalać na ruch z IP klientów!
(nie z IP NLB — bo NLB nie ma własnego SG dla L4 flow)
```

**Zonal DNS:**
```
NLB DNS: my-nlb-123456.eu-west-1.elb.amazonaws.com
  → rozkłada po AZ

Zonal DNS (per-AZ): eu-west-1a.my-nlb-123456.eu-west-1.elb.amazonaws.com
  → zawsze trafia do konkretnej AZ (przydatne dla latency-sensitive)
```

### NLB — kiedy używać vs ALB

| Potrzebujesz | Wybierz |
|---|---|
| HTTP/HTTPS routing (path, header) | ALB |
| Static IP (Elastic IP) | NLB |
| Ultra-low latency (µs) | NLB |
| TCP/UDP (nie HTTP) | NLB |
| Source IP preservation | NLB |
| WebSockets (long-lived) | NLB lub ALB |
| gRPC | ALB |
| Lambda target | ALB |
| WAF integration | ALB |
| On-prem firewall whitelist | NLB (static IP) |
| Gaming (UDP) | NLB |
| IoT (MQTT/TCP) | NLB |
| PrivateLink (VPC endpoint service) | NLB |

### NLB + PrivateLink

```
NLB jest wymagany dla VPC Endpoint Services (PrivateLink):
  
Dostawca VPC:
  EC2/ECS → NLB (endpoint service)
  
Klient VPC:
  Interface Endpoint (ENI) → PrivateLink → NLB → EC2/ECS
  
Klient widzi ENI w swoim VPC — nie wie nic o VPC dostawcy
Works: cross-account, cross-VPC, overlapping CIDR OK
```

### Cross-Zone Load Balancing

```
Bez Cross-Zone (domyślne dla NLB):
  AZ-A: 3 instancje → 50% ruchu (po 16.7% każda)
  AZ-B: 1 instancja → 50% ruchu (50% na jedną instancję!)
  
  → Nierówne obciążenie

Z Cross-Zone (domyślne dla ALB):
  AZ-A: 3 instancje → 37.5% łącznie (12.5% każda)
  AZ-B: 1 instancja → 12.5%
  
  → Równe obciążenie wszystkich instancji

NLB: Cross-Zone = domyślnie OFF (dodatkowy koszt $0.01/GB cross-AZ)
ALB: Cross-Zone = domyślnie ON (wliczone w cenę)
```

```bash
aws elbv2 modify-load-balancer-attributes \
  --load-balancer-arn arn:... \
  --attributes Key=load_balancing.cross_zone.enabled,Value=true
```

---

### 14.5 GWLB — Gateway Load Balancer

### Czym jest GWLB?

**Gateway Load Balancer** to specjalizowany load balancer dla **wirtualnych urządzeń sieciowych** (NVA — Network Virtual Appliance): firewalle, IDS/IPS, DPI (Deep Packet Inspection). Działa na warstwie 3 — widzi surowe pakiety IP.

**Analogia:** GWLB to jak punkt kontrolny celny — każda ciężarówka (pakiet) przechodzi przez kontrolę (firewall/IDS), a po przejściu jedzie dalej do celu.

**Odpowiednik Azure:** Azure Gateway Load Balancer.

### Architektura GWLB z GENEVE

```
Klient → Internet
  │
  ▼
IGW (Internet Gateway)
  │
  ▼  ← Ingress Route Table: 0.0.0.0/0 → GWLB Endpoint
GWLB Endpoint (VPC)
  │  GENEVE tunnel (port 6081, zachowuje oryginalne pakiety)
  ▼
GWLB → Fleet wirtualnych firewalli (EC2: Palo Alto, Checkpoint, Fortinet)
  │  
  ▼  ← po inspekcji: approve lub drop
GWLB Endpoint
  │
  ▼
Docelowy zasób (EC2, ALB)

Ruch powrotny (odpowiedź):
  EC2 → GWLB Endpoint → GWLB → Firewall (inspekcja) → GWLB Endpoint → IGW → Klient
```

### GENEVE Protocol

```
GWLB używa GENEVE (Generic Network Virtualization Encapsulation):
  - Encapsuluje oryginalne pakiety IP
  - Port: UDP 6081
  - Zachowuje: source IP, destination IP, wszystkie nagłówki
  - Firewall widzi oryginalny pakiet (nie enkapsulowany)
  - Po inspekcji: firewall wysyła pakiet z powrotem do GWLB

Ważne: Firewall MUSI obsługiwać GENEVE/GWLB
(Palo Alto VM-Series, Check Point CloudGuard, Fortinet FortiGate, etc.)
```

### Kiedy używać GWLB

- Wymagana inspekcja WSZYSTKIEGO ruchu wchodzącego/wychodzącego
- Regulacje: NGFW (Next-Generation Firewall) per compliance
- IDS/IPS (Intrusion Detection/Prevention)
- Centralny punkt inspekcji w hub-and-spoke (TGW + Inspection VPC)
- Vendor-specific firewall (nie AWS Network Firewall)

---

### 14.6 CLB — Classic Load Balancer (Legacy)

**NIE używaj w nowych projektach.** CLB to stary load balancer z EC2-Classic era.

```
CLB obsługuje:
  - HTTP/HTTPS (L7 — ale bardzo ograniczone reguły)
  - TCP/SSL (L4)
  
Braki vs ALB/NLB:
  - Brak content-based routing
  - Brak target groups
  - Brak SNI (tylko jeden certyfikat per CLB)
  - Brak Lambda targets
  - Brak weighted routing
  
Kiedy zobaczysz CLB na egzaminie:
  → To jest odpowiedź BŁĘDNA (chyba że pytanie wprost o legacy)
  → Migracja: CLB → ALB lub NLB przez Load Balancer Migration Tool
```

---

### 14.7 Health Checks — Szczegóły

```
ALB Health Check:
  Protokół: HTTP/HTTPS
  Path: /health (konfigurowalne)
  Kod sukcesu: 200 (konfigurowalne, np. 200-299)
  
NLB Health Check:
  Protokół: TCP/HTTP/HTTPS
  TCP: tylko sprawdza czy port otwarty (3-way handshake)
  HTTP: sprawdza status code
  
Parametry:
  HealthCheckIntervalSeconds: 5-300s (default 30)
  HealthyThresholdCount: 2-10 (default 3 = po 3 pass = healthy)
  UnhealthyThresholdCount: 2-10 (default 3 = po 3 fail = unhealthy)
  HealthCheckTimeoutSeconds: 2-120s (default 5)
```

```bash
aws elbv2 modify-target-group \
  --target-group-arn arn:... \
  --health-check-path /health \
  --health-check-interval-seconds 10 \
  --healthy-threshold-count 2 \
  --unhealthy-threshold-count 3 \
  --matcher HttpCode=200-299
```

---

### 14.8 ELB + Auto Scaling Group — Integracja

```
ASG automatycznie:
  1. Uruchamia nową EC2 (scale-out)
  2. Rejestruje ją w Target Group
  3. ELB health check → healthy
  4. ALB zaczyna wysyłać ruch

Scale-in:
  1. ASG wybiera instancję do terminacji
  2. Wyrejestrowuje z TG (Deregistration Delay!)
  3. ALB drainuje połączenia (connection draining)
  4. Po drain: terminuje instancję

Warm-up period (ASG cooldown):
  Nowa instancja potrzebuje czasu na start
  Instance Warmup: czas zanim ASG liczy metryki (np. 300s)
  → nie wysyła ruchu przez ten czas? NIE — ALB wysyła gdy health check pass
  → ASG nie skaluje ponownie przez warmup czas
```

---

### 14.9 SSL/TLS — Tryby i Polityki

### SSL Termination na ALB

```
Klient ──HTTPS──► ALB (terminacja SSL) ──HTTP──► EC2
                    ↑ certyfikat ACM

Zalety: EC2 nie musi obsługiwać SSL (CPU savings), centralny cert mgmt
Wady: ruch ALB→EC2 jest nieszyfrowany (w VPC, akceptowalne)
Dla compliance wymagającego E2E: użyj HTTPS na TG też
```

### SSL Passthrough na NLB

```
Klient ──TLS──► NLB (TCP passthrough) ──TLS──► EC2
                ↑ NLB NIE terminuje SSL

EC2 terminuje TLS, widzi prawdziwy IP klienta
Używaj gdy: E2E encryption wymagane, lub EC2 potrzebuje client certificate
```

### SSL Bridging (Re-encryption)

```
Klient ──HTTPS──► ALB (terminacja SSL, inspekcja) ──HTTPS──► EC2
                    ↑ ALB re-szyfruje do backend

Używaj gdy: compliance wymaga E2E + ALB musi widzieć headers (L7 routing)
```

---

### 14.10 ELB — Zaawansowane Funkcje

### Idle Timeout

```bash
# Domyślnie: 60 sekund idle → connection closed
# Dla long-lived connections (WebSocket, streaming): zwiększ
aws elbv2 modify-load-balancer-attributes \
  --attributes Key=idle_timeout.timeout_seconds,Value=4000
```

### Deletion Protection

```bash
aws elbv2 modify-load-balancer-attributes \
  --attributes Key=deletion_protection.enabled,Value=true
```

### Slow Start Mode (ALB)

```bash
# Nowe instancje w TG dostają mały % ruchu przez X sekund
# Pozwala na "rozgrzanie" aplikacji (cache, JVM JIT)
aws elbv2 modify-target-group-attributes \
  --attributes Key=slow_start.duration_seconds,Value=300
# 300 sekund: od 0% → 100% stopniowo
```

### Request Routing Algorithms

```
ALB Target Group:
  Round robin (default): kolejno do każdej instancji
  Least outstanding requests: instancja z najmniej aktywnych requestów
    → lepsze dla zmiennych czasów odpowiedzi (niektóre requesty długie)

aws elbv2 modify-target-group-attributes \
  --attributes Key=load_balancing.algorithm.type,Value=least_outstanding_requests
```

### Lambda Target (ALB → Lambda)

```bash
# ALB może wywołać Lambda bezpośrednio
aws lambda add-permission \
  --function-name my-function \
  --statement-id AllowELBInvoke \
  --action lambda:InvokeFunction \
  --principal elasticloadbalancing.amazonaws.com \
  --source-arn arn:aws:elasticloadbalancing:...

# Lambda receives event:
# {
#   "requestContext": {"elb": {...}},
#   "httpMethod": "GET",
#   "path": "/api/users",
#   "headers": {...},
#   "body": null
# }
# Lambda musi zwrócić: statusCode, headers, body
```

### ALB Authentication (Cognito/OIDC)

```bash
# ALB wymaga logowania PRZED przekazaniem do backend
aws elbv2 create-rule \
  --actions '[{
    "Type": "authenticate-cognito",
    "Order": 1,
    "AuthenticateCognitoConfig": {
      "UserPoolArn": "arn:aws:cognito-idp:...",
      "UserPoolClientId": "abc123",
      "UserPoolDomain": "myapp.auth.eu-west-1.amazoncognito.com",
      "OnUnauthenticatedRequest": "authenticate"
    }
  }, {
    "Type": "forward",
    "Order": 2,
    "TargetGroupArn": "arn:..."
  }]'

# Użytkownik niezalogowany → redirect do Cognito login page
# Po zalogowaniu → ALB forward z JWT headers do backend
```

---

### 14.11 ELB — Koszty

### ALB Pricing

```
Opłata:  $0.008/h (ALB hour) + $0.008/LCU-h

LCU (Load Balancer Capacity Unit) = MAX z:
  - Nowe połączenia:     25/s  = 1 LCU
  - Aktywne połączenia:  3000 concurrent = 1 LCU
  - Bandwidth:           1 GB/h = 1 LCU
  - Reguły (Rules):      1000 evaluated rules/s = 1 LCU

Przykład:
  ALB hour: $0.008 × 730h/mc = $5.84/mc
  + LCU: zależy od ruchu
  
Typowy mały ALB: ~$20-50/mc
```

### NLB Pricing

```
Opłata: $0.008/h (NLB hour) + $0.006/NLCU-h

NLCU (Network Load Balancer Capacity Unit) = MAX z:
  - Nowe TCP/UDP flows:  800/s = 1 NLCU
  - Aktywne TCP flows:   100000 concurrent = 1 NLCU  
  - Bandwidth:           1 GB/h = 1 NLCU
  
Cross-Zone Load Balancing: $0.01/GB (jeśli włączone)
```

### GWLB Pricing

```
Opłata: $0.008/h + $0.004/GWLCU-h

Endpoint: $0.0035/h per endpoint + $0.004/GB
```

---

### 14.12 ELB — Monitoring i Metryki

```bash
# Kluczowe metryki CloudWatch:

# ALB:
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApplicationELB \
  --metric-name RequestCount \
  --dimensions Name=LoadBalancer,Value=app/my-alb/50dc6c495c0c9188

# Ważne metryki ALB:
# RequestCount           — liczba requestów
# TargetResponseTime     — czas odpowiedzi backendu (p50, p95, p99!)
# HTTPCode_Target_5XX    — błędy 5xx z backendu
# HTTPCode_ELB_5XX       — błędy 5xx z samego ELB (timeout, no healthy)
# HealthyHostCount       — liczba zdrowych instancji w TG
# UnhealthyHostCount     — liczba niezdrowych
# ActiveConnectionCount  — aktywne połączenia

# NLB:
# ActiveFlowCount        — aktywne TCP flows
# NewFlowCount           — nowe flows/s
# ProcessedBytes         — dane przetworzone
# HealthyHostCount
```

---

### 14.13 Well-Architected Framework — ELB

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Access logs do S3 → Athena dla analizy, CloudWatch dashboards (TargetResponseTime p99, 5XX rates), AWS X-Ray tracing dla distributed tracing przez ALB |
| **Security** | HTTPS only (redirect HTTP→HTTPS), TLS 1.2/1.3 policy, AWS WAF na ALB (OWASP rules, rate limiting, IP blocking), ALB authentication (Cognito/OIDC) dla wewnętrznych apps, Deletion Protection |
| **Reliability** | Multi-AZ subnety (min 2), Cross-Zone Load Balancing (ALB domyślnie), health checks odpowiednio czułe (interval, threshold), Connection Draining dla zero-downtime deploy, ASG + TG integration |
| **Performance Efficiency** | NLB dla ultra-low latency TCP/UDP, ALB Least Outstanding Requests zamiast Round Robin (zmienne czasy odpowiedzi), Slow Start Mode dla warm-up, keep-alive connections, HTTP/2 na ALB |
| **Cost Optimization** | Right-size: NLB tańszy per połączenie niż ALB dla prostego TCP, Idle Timeout tuning (unikaj zbyt długich), wyłącz Cross-Zone w NLB jeśli nie potrzebne (oszczędność na transfer), jeden ALB z regułami zamiast wielu ALB |
| **Sustainability** | Konsolidacja: jeden ALB dla wielu aplikacji (path routing), nie twórz oddzielnych LB per aplikacja, wyłącz CLB (legacy, mniej efektywny) |

---

### 14.14 4 Domeny Egzaminacyjne SAP-C02

### Domena 1: Design Solutions for Organizational Complexity

- **NLB + PrivateLink** dla ekspozycji usług cross-account bez VPC Peering
- **ALB + Cognito** dla centralnego auth w multi-tenant aplikacjach
- **Shared ALB** (multi-tenant path routing) vs oddzielne ALB per tenant — cost vs isolation
- **WAF na ALB** dla centralnej ochrony aplikacji w organizacji

### Domena 2: Design for New Solutions

- **Wybór ALB vs NLB**: HTTP routing → ALB, static IP / UDP / PrivateLink → NLB
- **Weighted Target Groups** dla Blue/Green i Canary deploymentu
- **GWLB** gdy wymagana inspekcja przez third-party firewall (compliance, NGFW)
- **Lambda targets** dla event-driven backends za ALB

### Domena 3: Continuous Improvement for Existing Solutions

- **CLB → ALB migration**: Load Balancer Migration Tool, zero-downtime via DNS cutover
- **HTTP → HTTPS**: Listener redirect rule (HTTP 301 → HTTPS)
- **gp2 TG → Least Outstanding Requests** gdy response time niejednorodny
- **Cross-Zone tuning** dla NLB: włącz gdy nierówna liczba instancji per AZ

### Domena 4: Accelerate Workload Migration and Modernization

- **ALB + ECS/Fargate**: target type `ip` dla dynamicznych portów kontenerów
- **ALB + Lambda**: serverless migration bez zmiany klienta (ten sam DNS)
- **NLB static IP**: wymagane gdy on-prem firewall whitelist per IP (nie DNS)
- **GWLB dla brownfield**: wstaw inspekcję między istniejące zasoby bez refaktoru

---

## 15. HA, Redundancja i Skalowalność — EC2, ELB, ASG i Integracje

> **Zakres:** Pełny przegląd mechanizmów wysokiej dostępności (HA), redundancji i skalowania dla EC2 i ELB. Auto Scaling Groups (ASG), Launch Templates, polityki skalowania, integracje ELB z różnymi typami endpointów (EC2, ECS, Lambda, on-prem, multi-VPC). Failure modes i recovery patterns.

---

### 15.1 Fundamenty HA i Redundancji w AWS

### Hierarchia odporności na awarie

```
Poziom odporności (rosnąco):

Single EC2 w jednej AZ
  └─ Awaria instancji = downtime

Multi-EC2 w jednej AZ + ELB
  └─ Awaria instancji = ELB przenosi ruch; awaria AZ = downtime

Multi-EC2 w wielu AZ + ELB (Cross-Zone)
  └─ Awaria instancji = OK; awaria AZ = OK; awaria regionu = downtime

Multi-Region (Active-Active lub Active-Passive)
  └─ Awaria regionu = failover; awaria globalna = bardzo rzadka
```

**Analogia z życia:** Redundancja to jak rezerwa w wojsku. Jeden żołnierz (single EC2) pada — misja niewykonana. Pluton (multi-EC2 w AZ) — jeden pada, reszta działa. Kilka plutonów w różnych lokalizacjach (multi-AZ) — lokalna katastrofa nie zatrzymuje misji. Kilka baz w różnych krajach (multi-region) — nawet kraj w chaosie nie zatrzymuje operacji.

### Terminy

| Termin | Definicja |
|---|---|
| **HA (High Availability)** | System działa mimo awarii komponentów (mierzone jako % uptime: 99.9% = 8.76h/rok downtime) |
| **Fault Tolerance** | System działa BEZ degradacji mimo awarii (droższe niż HA — wymaga nadmiarowości 1:1) |
| **Redundancja** | Duplikacja komponentów dla HA/FT |
| **RPO (Recovery Point Objective)** | Maksymalna akceptowalna utrata danych (np. 1h = możemy stracić max 1h danych) |
| **RTO (Recovery Time Objective)** | Maksymalny akceptowalny czas przywrócenia (np. 15min = system musi działać w 15 min po awarii) |
| **Scale-out (horizontal)** | Dodaj więcej instancji — preferowane w AWS |
| **Scale-up (vertical)** | Większa instancja — wymaga restartu, nie dynamiczne |

---

### 15.2 Auto Scaling Group (ASG) — Fundament Skalowania EC2

### Czym jest ASG?

**ASG (Auto Scaling Group)** to mechanizm automatycznego zarządzania flotą EC2 — dodaje instancje gdy potrzeba więcej mocy, usuwa gdy obciążenie spada, zastępuje niedziałające instancje.

**Analogia:** ASG to jak dyspozytor taksówek. Gdy więcej zamówień → wysyła więcej kierowców. Gdy mało zamówień → odsyła część do domu. Gdy kierowca zgłasza awarię → wysyła zastępstwo.

```
┌─────────────────────────────────────────────────────┐
│               Auto Scaling Group                    │
│                                                     │
│  MinSize: 2 ──────┐                                │
│  DesiredCapacity: 4 ─── aktualnie uruchomione      │
│  MaxSize: 10 ─────┘                                │
│                                                     │
│  [EC2] [EC2] [EC2] [EC2]   ← Desired=4            │
│   AZ-a  AZ-a  AZ-b  AZ-b  ← Multi-AZ rozkład     │
└─────────────────────────────────────────────────────┘
```

### Launch Template (nie Launch Configuration!)

```bash
# Launch Template — nowoczesny sposób definiowania EC2 konfiguracji
aws ec2 create-launch-template \
  --launch-template-name my-app-lt \
  --version-description "v1" \
  --launch-template-data '{
    "ImageId": "ami-12345678",
    "InstanceType": "m6i.xlarge",
    "SecurityGroupIds": ["sg-abc123"],
    "IamInstanceProfile": {"Name": "MyAppRole"},
    "UserData": "IyEvYmluL2Jhc2gK...",
    "MetadataOptions": {"HttpTokens": "required"},
    "BlockDeviceMappings": [{
      "DeviceName": "/dev/xvda",
      "Ebs": {"VolumeSize": 30, "VolumeType": "gp3", "Encrypted": true}
    }],
    "TagSpecifications": [{
      "ResourceType": "instance",
      "Tags": [{"Key": "App", "Value": "MyApp"}]
    }]
  }'

# Utwórz ASG z Launch Template
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-app-lt,Version='$Latest' \
  --min-size 2 \
  --desired-capacity 4 \
  --max-size 10 \
  --vpc-zone-identifier "subnet-aaa,subnet-bbb,subnet-ccc" \  # Multi-AZ!
  --target-group-arns "arn:aws:elasticloadbalancing:..." \
  --health-check-type ELB \        # ELB sprawdza zdrowie (nie tylko EC2 status)
  --health-check-grace-period 300  # 5 min na start aplikacji
```

### Typy Health Check w ASG

```
EC2 Health Check (domyślny):
  Sprawdza: EC2 status checks (hardware, hypervisor, OS)
  Nie sprawdza: czy aplikacja odpowiada na port 80

ELB Health Check (REKOMENDOWANE z ALB/NLB):
  Sprawdza: EC2 status + ELB health check (HTTP 200 na /health)
  Gdy ELB uzna instancję za Unhealthy:
    → ASG terminuje instancję i uruchamia nową (auto-replacement!)
  
  aws autoscaling update-auto-scaling-group \
    --auto-scaling-group-name my-asg \
    --health-check-type ELB \
    --health-check-grace-period 300
```

> ⚠️ **Egzamin:** `health-check-grace-period` = czas po launch zanim ASG zacznie sprawdzać health. Jeśli za krótki → ASG terminuje zdrowe instancje które się jeszcze uruchamiają. Ustaw ≥ czasu startowania aplikacji.

### ASG Lifecycle Hooks

```
Normalny flow:
  Launch: Pending → InService
  Terminate: Terminating → Terminated

Z Lifecycle Hooks:
  Launch Hook:
    Pending → Pending:Wait (np. 1h) → Pending:Proceed → InService
              ↑ Tutaj: konfiguruj instancję, pobierz config z S3,
                rejestruj w CM DB, warm-up cache

  Terminate Hook:
    Terminating → Terminating:Wait (np. 1h) → Terminating:Proceed → Terminated
                  ↑ Tutaj: graceful shutdown, flush logi, deregister z discovery

aws autoscaling put-lifecycle-hook \
  --auto-scaling-group-name my-asg \
  --lifecycle-hook-name my-launch-hook \
  --lifecycle-transition autoscaling:EC2_INSTANCE_LAUNCHING \
  --default-result CONTINUE \
  --heartbeat-timeout 300 \
  --notification-target-arn arn:aws:sns:...   # SNS → Lambda → konfiguracja
```

---

### 15.3 Polityki Skalowania ASG

### 1. Simple Scaling (legacy, nie używaj)

```
IF CPU > 70% → dodaj 2 instancje
WAIT 300s cooldown
IF CPU > 70% → dodaj 2 instancje
...

Problem: cooldown = opóźnienie; może nie nadążać za ruchem
```

### 2. Step Scaling (zalecane dla reaktywnego)

```
IF CPU 70-80% → dodaj 1 instancję
IF CPU 80-90% → dodaj 3 instancje
IF CPU >90%   → dodaj 5 instancji
IF CPU <30%   → usuń 2 instancje
IF CPU <20%   → usuń 4 instancje

Bez cooldown (każde przekroczenie progu → natychmiastowa akcja)
Lepiej reaguje na nagłe skoki
```

```bash
aws autoscaling put-scaling-policy \
  --policy-name step-scale-out \
  --auto-scaling-group-name my-asg \
  --policy-type StepScaling \
  --adjustment-type ChangeInCapacity \
  --step-adjustments \
    'MetricIntervalLowerBound=0,MetricIntervalUpperBound=10,ScalingAdjustment=1' \
    'MetricIntervalLowerBound=10,MetricIntervalUpperBound=20,ScalingAdjustment=3' \
    'MetricIntervalLowerBound=20,ScalingAdjustment=5'
```

### 3. Target Tracking Scaling (zalecane, najprostsze)

```
Utrzymuj: CPU = 50% (lub inne metryki)
ASG automatycznie oblicza ile instancji potrzeba

Przykład:
  4 instancje, CPU=80% → ASG liczy: chcę 50%, mam 80%
  Potrzebuję: 4 × (80/50) = 6.4 → zaokrągl = 7 instancji
  Dodaj 3 instancje
```

```bash
aws autoscaling put-scaling-policy \
  --policy-name target-tracking-cpu \
  --auto-scaling-group-name my-asg \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration '{
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "ASGAverageCPUUtilization"
    },
    "TargetValue": 50.0,
    "ScaleInCooldown": 300,
    "ScaleOutCooldown": 60
  }'

# Inne predefined metryki:
# ALBRequestCountPerTarget  ← NAJWAŻNIEJSZA dla web apps
# ASGAverageNetworkIn
# ASGAverageNetworkOut

# Custom metric (np. SQS queue depth):
aws autoscaling put-scaling-policy \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration '{
    "CustomizedMetricSpecification": {
      "MetricName": "ApproximateNumberOfMessagesVisible",
      "Namespace": "AWS/SQS",
      "Dimensions": [{"Name": "QueueName", "Value": "my-queue"}],
      "Statistic": "Average"
    },
    "TargetValue": 10
  }'
```

### 4. Scheduled Scaling

```bash
# Wiesz KIEDY będzie ruch (np. poniedziałek 9:00, przerwa 12:00)
aws autoscaling put-scheduled-update-group-action \
  --auto-scaling-group-name my-asg \
  --scheduled-action-name scale-out-morning \
  --recurrence "0 8 * * MON-FRI" \   # Cron: pn-pt 8:00 UTC
  --min-size 5 \
  --desired-capacity 10 \
  --max-size 20

aws autoscaling put-scheduled-update-group-action \
  --scheduled-action-name scale-in-night \
  --recurrence "0 20 * * *" \         # Każdy dzień 20:00 UTC
  --desired-capacity 2
```

### 5. Predictive Scaling

```
ML-based: AWS analizuje historyczne metryki i przewiduje load
Pre-skaluje PRZED spodziewanym szczytem (nie reaktywnie)

Tryby:
  ForecastOnly:      tylko prognoza, nie skaluje
  ForecastAndScale:  prognoza + automatyczne skalowanie

Wymaganie: ≥24h historii metryk (najlepiej ≥2 tygodnie)
```

```bash
aws autoscaling put-scaling-policy \
  --policy-name predictive-scaling \
  --policy-type PredictiveScaling \
  --predictive-scaling-configuration '{
    "MetricSpecifications": [{
      "TargetValue": 50,
      "PredefinedMetricPairSpecification": {
        "PredefinedMetricType": "ASGCPUUtilization"
      }
    }],
    "Mode": "ForecastAndScale",
    "SchedulingBufferTime": 300
  }'
# SchedulingBufferTime=300: skaluj 5 min PRZED prognozowanym szczytem
```

### Porównanie polityk skalowania

| Polityka | Reakcja | Złożoność | Najlepszy dla |
|---|---|---|---|
| **Simple** | Cooldown = opóźnienie | Prosta | Legacy, nie używaj |
| **Step** | Natychmiastowa | Średnia | Reaktywne na duże skoki |
| **Target Tracking** | Automatyczna kalkulacja | Niska | Większość przypadków |
| **Scheduled** | Pre-emptive | Niska | Znane wzorce (business hours) |
| **Predictive** | Pre-emptive (ML) | Niska | Regularne wzorce tygodniowe |

---

### 15.4 ASG — Mixed Instances Policy (Spot + On-Demand)

```bash
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name mixed-asg \
  --mixed-instances-policy '{
    "LaunchTemplate": {
      "LaunchTemplateSpecification": {
        "LaunchTemplateName": "my-app-lt",
        "Version": "$Latest"
      },
      "Overrides": [
        {"InstanceType": "m6i.xlarge"},
        {"InstanceType": "m5.xlarge"},
        {"InstanceType": "m5a.xlarge"},
        {"InstanceType": "c6i.xlarge"},
        {"InstanceType": "c5.xlarge"}
      ]
    },
    "InstancesDistribution": {
      "OnDemandBaseCapacity": 2,
      "OnDemandPercentageAboveBaseCapacity": 30,
      "SpotAllocationStrategy": "price-capacity-optimized"
    }
  }' \
  --min-size 2 \
  --desired-capacity 6 \
  --max-size 20 \
  --vpc-zone-identifier "subnet-a,subnet-b,subnet-c"

# Wynik przy Desired=6:
# 2 On-Demand (base) + 4 instancje (30% OD = 1.2 → 2 OD, 70% Spot = 2 Spot)
# Faktycznie: 2 base OD + 2 OD + 2 Spot = 4 OD + 2 Spot
```

### Strategie Spot w ASG

| Strategia | Priorytet | Najlepszy dla |
|---|---|---|
| `lowest-price` | Najtańsze pule | Koszty (ryzyko interruption) |
| `capacity-optimized` | Największa pojemność | Stabilność (mniej interruptions) |
| `price-capacity-optimized` | Balans ceny i pojemności | **Rekomendowane** |
| `diversified` | Rozkłada po pulach | Resilience dla Spot Fleet |

---

### 15.5 Warmup, Cooldown i Instance Refresh

### Cooldown i Warmup

```
Scale-Out Cooldown (default 300s):
  Po scale-out: czekaj X sekund zanim kolejna scale-out akcja
  Cel: dać czas nowym instancjom na uruchomienie i stabilizację
  
Scale-In Cooldown (default 300s):
  Po scale-in: czekaj X sekund zanim kolejna scale-in akcja
  
Instance Warmup (Target Tracking / Step Scaling):
  Nowa instancja nie wliczana do metryk przez X sekund
  Zapobiega "zbyt szybkiemu" scale-in zanim instancja się rozgrzeje
  
  Przykład bez warmup:
    ASG skaluje do 6, 2 nowe instancje uruchamiają się
    CPU nagle spada (2 nowe instancje "idle")
    ASG myśli: za dużo → scale-in → terminuje 2 instancje
    Błąd! Instancje się uruchamiały, nie były zbędne
```

### Instance Refresh — Rolling Deploy bez Downtime

```bash
# Zaktualizuj AMI/konfigurację bez downtime przez postupowe zastępowanie
aws autoscaling start-instance-refresh \
  --auto-scaling-group-name my-asg \
  --preferences '{
    "MinHealthyPercentage": 90,
    "InstanceWarmup": 300,
    "CheckpointPercentages": [20, 50, 100],
    "CheckpointDelay": 600
  }'

# Działanie:
# 1. Terminuje 10% instancji (MinHealthy=90% → max 10% w remoncie)
# 2. Uruchamia nowe z nową konfiguracją
# 3. Czeka na health check
# 4. Kontynuuje do 20% → checkpoint (czeka 600s) → 50% → 100%
# 
# Rollback: aws autoscaling cancel-instance-refresh --asg my-asg
```

---

### 15.6 ELB HA — Jak ELB Zapewnia Wysoką Dostępność

### Multi-AZ ELB

```
ALB/NLB wymagają minimum 2 subnety w różnych AZ:

ALB:
  eu-west-1a: ALB node (auto-skaluje)
  eu-west-1b: ALB node (auto-skaluje)
  eu-west-1c: ALB node (auto-skaluje)
  
  → Jeden DNS name: my-alb-xxx.eu-west-1.elb.amazonaws.com
  → DNS rozwiązuje do IP wszystkich node'ów (Round Robin DNS)
  → Awaria jednej AZ: DNS usuwa IP tej AZ automatycznie

NLB:
  eu-west-1a: NLB node z Elastic IP #1
  eu-west-1b: NLB node z Elastic IP #2
  
  → Każda AZ: osobny statyczny IP
  → Awaria AZ: IP tej AZ nadal istnieje, ale nie odpowiada
    (klient musi mieć retry/failover logic)
```

### ELB nie jest Single Point of Failure

```
AWS SLA dla ALB/NLB: 99.99% availability
AWS zarządza:
  - Skalowaniem ELB nodes (horizontal auto-scaling)
  - HA między AZ (redundantne ELB nodes)
  - Patchowaniem i aktualizacjami
  
Ty zarządzasz:
  - Registrowanymi targetami (EC2, ECS, Lambda)
  - Security Groups (dla ALB)
  - Regułami routingu
  - Certyfikatami SSL
```

### Zonal Isolation — Awaria AZ

```
Scenariusz: eu-west-1b niedostępna

ALB z Cross-Zone ON:
  Przed awarią: 33% ruchu per AZ
  Po awarii eu-west-1b:
    - ALB node w 1b niedostępny
    - DNS usuwa IP 1b automatycznie
    - Pozostałe ALB nodes (1a, 1c) obsługują 100% ruchu
    - Ruch routowany do zdrowych EC2 w 1a i 1c (Cross-Zone!)
    
NLB z Cross-Zone OFF:
  Przed awarią: 33% do 1a, 33% do 1b, 33% do 1c
  Po awarii eu-west-1b:
    - NLB node 1b niedostępny
    - Klienci na 1b EIP nie dostają odpowiedzi
    - Klienci na 1a/1c EIP: OK
    - TYLKO 67% ruchu dociera (klienci celujący w 1b IP tracą połączenie)
    
  Z Cross-Zone ON: podobnie do ALB — wszystkie node routują wszędzie
```

---

### 15.7 ELB + EC2 — Pełna Integracja

### Flow: Request od klienta do EC2

```
1. Klient → DNS query: api.example.com
2. Route 53 → ALIAS/CNAME → ALB DNS name
3. ALB DNS → IP ALB node (najbliższy/round robin)
4. Klient → TCP connect → ALB node:443
5. ALB node:
   a. TLS handshake (terminacja SSL)
   b. Sprawdza Listener rules (path, host, header)
   c. Wybiera Target Group
   d. Wybiera zdrowy target (Round Robin lub LOR)
   e. Ustanawia nowe TCP połączenie → EC2:80
   f. Forward HTTP request z X-Forwarded-For
6. EC2 przetwarza request
7. EC2 → ALB node → Klient
8. ALB drainuje połączenie (idle timeout lub Connection close)
```

### Security Group Chain — Zasada Least Privilege

```
Internet ─────► ALB (SG: allow 443 from 0.0.0.0/0)
                  │
                  └─► EC2 (SG: allow 80 from SG-ALB only)
                          │
                          └─► RDS (SG: allow 5432 from SG-EC2 only)

Konfiguracja:

# ALB Security Group
aws ec2 authorize-security-group-ingress \
  --group-id sg-alb \
  --protocol tcp --port 443 --cidr 0.0.0.0/0

# EC2 Security Group — tylko z SG ALB (nie CIDR!)
aws ec2 authorize-security-group-ingress \
  --group-id sg-ec2 \
  --protocol tcp --port 80 \
  --source-group sg-alb    # Referencja do SG, nie IP!

# Zalety SG reference vs CIDR:
# - ALB może zmieniać IP (skalowanie) → SG zawsze aktualne
# - Czytelniejsze reguły
# - Mniejsze ryzyko błędu
```

### EC2 Registration w Target Group

```bash
# Ręczna rejestracja (bez ASG)
aws elbv2 register-targets \
  --target-group-arn arn:... \
  --targets \
    Id=i-11111111,Port=80 \
    Id=i-22222222,Port=80

# Wyrejestrowanie
aws elbv2 deregister-targets \
  --target-group-arn arn:... \
  --targets Id=i-11111111

# Sprawdź health status
aws elbv2 describe-target-health \
  --target-group-arn arn:... \
  --query 'TargetHealthDescriptions[*].{
    ID:Target.Id,
    Port:Target.Port,
    State:TargetHealth.State,
    Reason:TargetHealth.Reason
  }'

# Stany: healthy | unhealthy | initial | draining | unused | unavailable
```

### Port Override — Multiple Apps na Jednej EC2

```bash
# Jedna EC2 działa jako target dla DWÓCH różnych Target Groups
# na różnych portach (np. app A: 8080, app B: 8081)

# TG dla App A: port 8080
aws elbv2 register-targets \
  --target-group-arn arn:tg-app-a \
  --targets Id=i-12345678,Port=8080

# TG dla App B: port 8081 (ta sama instancja!)
aws elbv2 register-targets \
  --target-group-arn arn:tg-app-b \
  --targets Id=i-12345678,Port=8081

# ALB rules:
# /app-a/* → TG-App-A (port 8080)
# /app-b/* → TG-App-B (port 8081)
```

---

### 15.8 ELB + ECS/Fargate — Kontenery

### Dynamic Port Mapping (EC2 launch type)

```
Problem: wiele kontenerów na jednej EC2, każdy musi mieć unikalny port

ECS z EC2 launch type (dynamic port mapping):
  Container A: host port = 0 (auto-assign) → dostaje np. 32768
  Container B: host port = 0 (auto-assign) → dostaje np. 32769
  
  ECS automatycznie rejestruje w TG: EC2_IP:32768 i EC2_IP:32769
  ALB routuje do właściwego portu
  
  Target Group: type=instance, port=0 (override per target)
```

### Fargate (target type = ip)

```
Fargate: każdy task dostaje własne ENI z prywatnym IP
  
Task A: 10.0.1.5:8080
Task B: 10.0.1.6:8080  (inny IP, ten sam port!)
  
Target Group: type=ip
  ECS rejestruje: 10.0.1.5:8080, 10.0.1.6:8080
  
Zalety: brak konfliktu portów, izolacja na poziomie ENI
```

```bash
# ECS Service z ALB integracja
aws ecs create-service \
  --cluster my-cluster \
  --service-name my-service \
  --task-definition my-task:3 \
  --desired-count 3 \
  --launch-type FARGATE \
  --load-balancers '[{
    "targetGroupArn": "arn:aws:elasticloadbalancing:...",
    "containerName": "my-container",
    "containerPort": 8080
  }]' \
  --network-configuration '{
    "awsvpcConfiguration": {
      "subnets": ["subnet-aaa", "subnet-bbb"],
      "securityGroups": ["sg-12345678"],
      "assignPublicIp": "DISABLED"
    }
  }'
```

---

### 15.9 ELB + Lambda — Serverless Backend

```
ALB → Lambda flow:
  1. ALB otrzymuje HTTP request
  2. ALB wywołuje Lambda synchronicznie (RequestResponse)
  3. ALB transformuje HTTP → Lambda event (JSON)
  4. Lambda przetwarza → zwraca JSON response
  5. ALB transformuje JSON → HTTP response → klient

Lambda event format:
{
  "requestContext": {"elb": {"targetGroupArn": "..."}},
  "httpMethod": "POST",
  "path": "/api/users",
  "queryStringParameters": {"page": "1"},
  "headers": {"content-type": "application/json", "x-forwarded-for": "1.2.3.4"},
  "body": "{\"name\": \"John\"}",
  "isBase64Encoded": false
}

Lambda response format:
{
  "statusCode": 200,
  "headers": {"Content-Type": "application/json"},
  "body": "{\"id\": 123}",
  "isBase64Encoded": false
}
```

```bash
# Konfiguracja Lambda target w Target Group
aws elbv2 create-target-group \
  --name lambda-tg \
  --target-type lambda

aws elbv2 register-targets \
  --target-group-arn arn:tg-lambda \
  --targets Id=arn:aws:lambda:eu-west-1:123:function:my-function

# Lambda permissions dla ALB
aws lambda add-permission \
  --function-name my-function \
  --statement-id alb-invoke \
  --action lambda:InvokeFunction \
  --principal elasticloadbalancing.amazonaws.com \
  --source-arn arn:aws:elasticloadbalancing:...
```

**Ograniczenia Lambda za ALB:**
- Timeout: max 15 min (Lambda limit)
- Payload: max 1 MB request body
- Concurrency: Lambda concurrency limits (burst + provisioned)
- Multi-value headers: wymagają `MultiValueHeaders=true` na TG

---

### 15.10 ELB + On-Premises (Hybrid) — Target Type IP

```
Scenariusz: Migracja stopniowa — część serwerów na AWS, część on-prem

ALB z Target Type=IP:
  Może targetować dowolny prywatny IP osiągalny z VPC

  On-prem servers (przez DX/VPN):
    10.100.1.5:8080  (on-prem serwer 1)
    10.100.1.6:8080  (on-prem serwer 2)
  
  AWS servers:
    10.0.1.100:8080  (EC2 instance 1)
    10.0.1.101:8080  (EC2 instance 2)
  
  TG łączy oba → ALB routuje do wszystkich!
```

```bash
# Rejestruj on-prem IP w Target Group
aws elbv2 register-targets \
  --target-group-arn arn:tg-hybrid \
  --targets \
    Id=10.100.1.5,Port=8080,AvailabilityZone=all \   # on-prem: AZ=all
    Id=10.100.1.6,Port=8080,AvailabilityZone=all \
    Id=10.0.1.100,Port=8080 \    # EC2: AZ=eu-west-1a (auto-detect)
    Id=10.0.1.101,Port=8080

# AvailabilityZone=all → dla IP poza VPC (on-prem, inny VPC)
# ALB wysyła ruch do tych IP niezależnie od AZ
```

### Wzorzec Migracji Stopniowej

```
Faza 1: 100% on-prem w TG
  On-prem(1) w=50%, On-prem(2) w=50%

Faza 2: Canary — 10% na AWS
  On-prem(1) w=45%, On-prem(2) w=45%, EC2(1) w=10%

Faza 3: Coraz więcej na AWS
  On-prem w=30%, EC2 w=70%

Faza 4: 100% AWS
  EC2(1) w=50%, EC2(2) w=50%
  Wyrejestruj on-prem z TG

Zaleta: Jeden DNS, zero downtime, rollback w sekundy (zmień wagi)
```

---

### 15.11 ELB + Multi-VPC (Cross-VPC)

### Metoda 1: VPC Peering + ALB IP Target

```
VPC A (ALB) ←── VPC Peering ──→ VPC B (EC2 10.1.0.5)
  
ALB w VPC A:
  TG type=ip → target: 10.1.0.5 (VPC B, przez Peering)
  AvailabilityZone=all
  
Wymagania:
  - VPC Peering aktywny
  - Route Tables zaktualizowane
  - SG EC2 w VPC B: zezwól ruch z CIDR VPC A (ALB subnet)
```

### Metoda 2: PrivateLink (NLB + Interface Endpoint)

```
VPC A (konsument)                VPC B (dostawca)
  Interface Endpoint  ←──────────  NLB (port 443)
  ENI: 10.0.1.5:443              EC2 fleet
  
  ALB (VPC A) → NLB endpoint → NLB → EC2

Zalety PrivateLink vs Peering:
  - Działa z overlapping CIDR
  - Dostawca nie widzi całej sieci konsumenta
  - Cross-account, cross-region
  - No transitive routing issues
```

### Metoda 3: ALB → NLB (Nested LB)

```
Cel: Static IP + L7 routing

Internet ─► NLB (static Elastic IP) ─► ALB (L7 routing) ─► EC2
              └ stały IP dla on-prem      └ path/host routing
                firewall whitelist

Konfiguracja:
  NLB Target Group: type=alb, target=ARN of ALB
  NLB Listener: TCP 443 → TG-ALB
```

---

### 15.12 ELB + Global Accelerator — Ultra-HA Multi-Region

```
Route 53 Latency Routing → Global Accelerator → Regional ALB

AWS Global Accelerator:
  - 2 statyczne Anycast IP (globalne, nigdy się nie zmieniają)
  - Ruch wchodzi do najbliższego AWS Edge Location
  - Przez AWS backbone (nie Internet) → regional endpoint (ALB)
  - Automatyczny failover między regionami (30s)

Bez GA:
  Klient (Tokyo) → Internet → eu-west-1 ALB
  Latency: wysoka, niestabilna

Z GA:
  Klient (Tokyo) → Tokyo Edge (AWS) → AWS backbone → eu-west-1 ALB
  Latency: niższa, stabilna (AWS backbone vs Internet)

Kiedy GA zamiast CloudFront:
  - Non-HTTP: TCP/UDP apps (gaming, VoIP) — CF tylko HTTP
  - Static IP wymagany globalnie — CF ma DNS tylko
  - Multi-region active-active TCP — CF ma edge caching, GA ma routing
```

```bash
# Utwórz Global Accelerator z ALB endpoint
aws globalaccelerator create-accelerator \
  --name my-accelerator \
  --ip-address-type IPV4

aws globalaccelerator create-listener \
  --accelerator-arn arn:... \
  --protocol TCP \
  --port-ranges '[{"FromPort":443,"ToPort":443}]'

aws globalaccelerator create-endpoint-group \
  --listener-arn arn:... \
  --endpoint-group-region eu-west-1 \
  --endpoint-configurations '[{
    "EndpointId": "arn:aws:elasticloadbalancing:...", # ALB ARN
    "Weight": 100,
    "ClientIPPreservationEnabled": true
  }]' \
  --traffic-dial-percentage 100   # 0-100: ile % ruchu GA wysyła do tej grupy
```

---

### 15.13 Wzorce Redundancji — Active-Active vs Active-Passive

### Active-Active Multi-AZ

```
Route 53 (lub ALB)
  │
  ├── AZ-a: EC2 fleet (ASG) ← obsługuje ruch
  └── AZ-b: EC2 fleet (ASG) ← obsługuje ruch

Oba AZ aktywne jednocześnie:
  + Pełne wykorzystanie zasobów
  + Automatyczny failover (ALB Cross-Zone)
  + Lepsza latency (ruch do nearest AZ)
  - Wymaga stateless aplikacji (lub shared session store)
  - Synchronizacja danych między AZ
```

### Active-Passive Multi-AZ

```
Route 53 (failover routing)
  │
  ├── AZ-a: EC2 (Primary, aktywny) ← CAŁY ruch
  └── AZ-b: EC2 (Standby, idle)   ← włącza się gdy Primary pada

Kiedy używać:
  - Stateful aplikacje których nie można łatwo rozpraszać
  - Bazy danych (RDS Multi-AZ: primary write + standby)
  - Legacy apps z global state

Wady:
  - Zasoby Standby są "zmarnowane"
  - Wyższy failover time (Route 53 health check TTL)
```

### Active-Active Multi-Region

```
Route 53 (Latency-based routing):
  Klient EU → eu-west-1 ALB → EC2 EU
  Klient US → us-east-1 ALB → EC2 US
  Klient AP → ap-southeast-1 ALB → EC2 AP

Dane: DynamoDB Global Tables / Aurora Global Database / S3 replication

Najwyższy poziom HA:
  + Niższe latency globalnie
  + Pełna redundancja per region
  - Najdroższe
  - Skomplikowana synchronizacja danych (eventual consistency)
  - Wymaga stateless lub globally consistent DB
```

### Active-Passive Multi-Region (Pilot Light / Warm Standby)

```
Pilot Light:
  Primary region: full traffic
  DR region: minimalna infrastruktura (tylko krytyczne komponenty)
  Recovery: auto-scale DR region gdy Primary niedostępny
  RTO: ~10-30 min

Warm Standby:
  Primary region: full traffic
  DR region: mała ale działająca replika (scale-up przy failover)
  Recovery: szybszy niż Pilot Light (zasoby już działają)
  RTO: ~5-15 min
  
Multi-Site (Active-Active):
  Oba regiony pełen ruch
  RTO: ~0 (natychmiastowe)
```

---

### 15.14 Termination Policies ASG — Która Instancja Usunąć

```
Gdy ASG scale-in (usuwa instancje), algorytm:

1. Wybierz AZ z największą liczbą instancji
2. W tej AZ zastosuj Termination Policy:

   Default policy (kolejność):
   a. Instancje ze starego Launch Template/Configuration
   b. Instancja z najkrótszym remaining billing hour
      (EC2 rozlicza per-sekundę, ale ta logika to legacy)
   c. Najstarsza instancja
   
   Inne polityki:
   OldestInstance:          usuwa najstarsze
   NewestInstance:          usuwa najnowsze (testing)
   OldestLaunchTemplate:    usuwa z najstarszym LT (promuje nowe)
   ClosestToNextInstanceHour: optymalizacja kosztów (legacy)
   AllocationStrategy:      dla Mixed (usuwa najpierw OD lub Spot)

aws autoscaling update-auto-scaling-group \
  --termination-policies "OldestLaunchTemplate" "Default"
```

---

### 15.15 Scale-In Protection

```bash
# Chroń konkretną instancję przed scale-in (np. przetwarzanie długiego jobu)
aws autoscaling set-instance-protection \
  --instance-ids i-12345678 \
  --auto-scaling-group-name my-asg \
  --protected-from-scale-in

# Zdejmij ochronę po zakończeniu jobu
aws autoscaling set-instance-protection \
  --instance-ids i-12345678 \
  --auto-scaling-group-name my-asg \
  --no-protected-from-scale-in

# Gdy cała flota Spot jest chroniona: ASG nie może scale-in
# → może prowadzić do kosztów powyżej MaxSize!
# Zawsze zdejmuj ochronę po zakończeniu zadania
```

---

### 15.16 Troubleshooting — Częste Problemy ASG + ELB

### Problem 1: ASG zastępuje zdrowe instancje

```
Symptom: EC2 terminowane mimo działającej aplikacji
Diagnoza: 
  aws autoscaling describe-auto-scaling-instances
  → sprawdź "HealthStatus": "Unhealthy"
  
Przyczyny:
  1. health-check-grace-period za krótki → ustaw ≥ czas startowania
  2. ELB health check za czuły → zwiększ threshold/interval
  3. Aplikacja nie zwraca 200 na /health (np. zwraca 404)
  4. Security Group blokuje HC port
```

### Problem 2: Scale-out nie nadąża za ruchem

```
Symptom: 503 Service Unavailable, mimo że ASG skaluje
Diagnoza:
  1. ASG w trakcie skalowania (check scaling activity)
  2. New instancje: Initial → czeka na health check (60-120s)
  3. ELB drainuje stare instancje (Deregistration Delay 300s)
  
Rozwiązanie:
  - Zmniejsz health check grace period (jeśli app startuje szybciej)
  - Zmniejsz Deregistration Delay (np. 30s)
  - Predictive Scaling zamiast reaktywnego
  - Scheduled Scaling dla znanych szczytów
  - Pre-warm ALB (kontakt z AWS Support)
```

### Problem 3: NLB nie routuje do instancji w AZ

```
Symptom: część requestów timeout, część OK
Diagnoza:
  aws elbv2 describe-target-health --target-group-arn arn:...
  → instancje w danej AZ: "unused" lub "unavailable"
  
Przyczyny:
  1. Cross-Zone OFF + brak instancji w danej AZ
  2. Security Group EC2: brak reguły dla klientów (NLB zachowuje source IP)
  3. Instancje zarejestrowane ale nie ma subnet NLB w tej AZ
  
Rozwiązanie:
  - Dodaj subnet NLB w każdej AZ gdzie masz instancje
  - Lub włącz Cross-Zone Load Balancing
  - SG EC2: `0.0.0.0/0 : port` lub konkretne CIDR klientów
```

---

### 15.17 Well-Architected Framework — HA i Skalowalność

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Instance Refresh dla rolling deploy (nie ręczny), CloudWatch Alarms na ASG (ScalingActivity, FailedActions), EventBridge dla lifecycle hook events, Lifecycle Hooks dla graceful shutdown i config |
| **Security** | SG chain (ALB → EC2 → RDS), Scale-In Protection dla długich jobów, IMDSv2 wymagane w Launch Template, EBS encryption w LT, Instance Profile (nie access keys) |
| **Reliability** | Min 2 AZ zawsze, Multi-AZ ASG subnets, ELB health check type (nie EC2-only), Deregistration Delay dostosowany, health-check-grace-period ≥ app startup, Lifecycle Hooks dla graceful operations |
| **Performance Efficiency** | Target Tracking (ALBRequestCountPerTarget) dla web apps, Predictive Scaling dla regularnych wzorców, Warm Pools (pre-initialized instances), Mixed Instances Policy dla dywersyfikacji, Slow Start Mode dla JVM |
| **Cost Optimization** | Mixed Instances (Spot + On-Demand), Scale-In agresywne (krótki cooldown), Scheduled Scaling (noc/weekend), Savings Plans dla baseline On-Demand, Spot dla stateless web/app tier |
| **Sustainability** | Graviton w Launch Template, right-sizing przez Compute Optimizer przed skalowaniem, agresywne scale-in w nocy (nie idle zasobów), Spot reduces idle physical capacity |

---

### 15.18 4 Domeny Egzaminacyjne SAP-C02

### Domena 1: Design Solutions for Organizational Complexity

- **Multi-account HA:** ASG + ALB per account, wspólny Route 53 (Latency routing), shared TGW dla connectivity
- **Centralne zarządzanie ASG:** AWS Systems Manager → Parameter Store dla konfiguracji, Config dla compliance
- **Cross-account ELB monitoring:** CloudWatch cross-account metrics sharing

### Domena 2: Design for New Solutions

- **Wybór strategii HA:** Active-Active (stateless) vs Active-Passive (stateful/DB)
- **RPO/RTO mapping:** Pilot Light (RTO 30min), Warm Standby (RTO 5min), Multi-Site/Active-Active (RTO ~0)
- **Hybrydowe TG:** IP targets dla stopniowej migracji on-prem → AWS
- **Predictive + Target Tracking combo:** Predictive pre-skaluje, Target Tracking reaguje na odchylenia

### Domena 3: Continuous Improvement for Existing Solutions

- **Simple → Target Tracking:** eliminacja cooldown problemów
- **LC → LT migration:** unlock Mixed Instances, version control
- **EC2 health check → ELB health check:** auto-replacement niedziałających instancji
- **Instance Refresh:** zastąp ręczny rolling deploy

### Domena 4: Accelerate Workload Migration and Modernization

- **Lift-and-shift:** ASG zastępuje ręcznie zarządzane VM on-prem
- **Hybryid TG (IP targets):** stopniowa migracja bez cutover
- **EC2 → ECS migration:** zmień target type instance → ip, ECS zarządza rejestracją
- **EC2 → Lambda:** dodaj Lambda target do istniejącego ALB (zero klient-side changes)

---

### 15.19 Warm Pools — Skrócenie Czasu Scale-Out

```
Problem: nowa EC2 startuje 3-5 min (OS boot + app init) → powolny scale-out

Warm Pool: pula pre-initialized instancji czeka w stanie Stopped lub Running
  Gdy ASG potrzebuje scale-out → instancja z Warm Pool (już gotowa!)
  Czas scale-out: sekundy zamiast minut

Stany instancji w Warm Pool:
  Stopped (default): EC2 zatrzymane (płacisz za EBS, NIE za EC2 hour)
  Running:           EC2 działa (płacisz za EC2, ale najszybszy start)
  Hibernated:        EC2 z zapisanym RAM (najszybszy start, wymaga Hibernate support)
```

```bash
aws autoscaling put-warm-pool \
  --auto-scaling-group-name my-asg \
  --pool-state Stopped \        # Stopped | Running | Hibernated
  --min-size 2 \                # Zawsze trzymaj 2 w Warm Pool
  --max-group-prepared-capacity 5   # Max łącznie (InService + WarmPool)

# Lifecycle Hook dla Warm Pool (konfiguracja instancji w WP):
aws autoscaling put-lifecycle-hook \
  --auto-scaling-group-name my-asg \
  --lifecycle-hook-name WarmPoolInit \
  --lifecycle-transition autoscaling:EC2_INSTANCE_LAUNCHING \
  --default-result CONTINUE
```

---

## 16. TUN/TAP — Wirtualne Interfejsy Sieciowe, OpenVPN i AWS VPN

> **Zakres:** Deep-dive w mechanizmy TUN i TAP — wirtualne interfejsy sieciowe jądra Linux. Jak działają, jak się różnią, jak OpenVPN / Viscosity je używają, jak to się ma do AWS Client VPN i Site-to-Site VPN. Diagramy przepływu pakietów, konfiguracje, troubleshooting.

---

### 16.1 Czym są TUN i TAP?

**TUN** i **TAP** to **wirtualne interfejsy sieciowe** implementowane jako sterowniki jądra Linux (i Windows/macOS przez odpowiedniki). Nie reprezentują fizycznego sprzętu — są w całości programowe.

**Kluczowa różnica już na starcie:**

```
TUN  =  Tunnel  =  działa na warstwie 3 (IP)   — widzi pakiety IP
TAP  =  Tap     =  działa na warstwie 2 (Ethernet) — widzi ramki Ethernet
```

**Analogia z życia:**
- **TUN** to jak rura wodociągowa łącząca dwa miasta — przesyła wodę (pakiety IP) bez znajomości tego co jest na końcu ulicy (Ethernet)
- **TAP** to jak kabel sieciowy między dwoma przełącznikami — przesyła surowe ramki Ethernet razem z nagłówkami MAC, ARP, VLAN

---

### 16.2 Architektura Jądra — Jak TUN/TAP Istnieje w Systemie

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER SPACE                               │
│                                                                 │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────────────┐  │
│  │ Aplikacja│    │   OpenVPN    │    │  Twoja Aplikacja     │  │
│  │ (nginx,  │    │  (czyta i    │    │  (np. przeglądarka)  │  │
│  │  sshd)   │    │   pisze do   │    │                      │  │
│  └──────────┘    │  /dev/net/tun│    └──────────────────────┘  │
│                  └──────┬───────┘                              │
└─────────────────────────│───────────────────────────────────────┘
                          │ read()/write() — pakiety IP
                          │ (lub ramki Ethernet dla TAP)
┌─────────────────────────│───────────────────────────────────────┐
│                   KERNEL SPACE                                  │
│                          │                                      │
│  ┌───────────────────────▼───────────────────────────────────┐  │
│  │              TUN/TAP Driver (/dev/net/tun)                │  │
│  │                                                           │  │
│  │   Tworzy wirtualny interfejs: tun0, tap0, utun0 (macOS)  │  │
│  └──────────────────────┬────────────────────────────────────┘  │
│                         │                                       │
│  ┌──────────────────────▼────────────────────────────────────┐  │
│  │                  Network Stack                            │  │
│  │   Routing Table   │   iptables/nftables  │   Sockets     │  │
│  └──────────────────────┬────────────────────────────────────┘  │
│                         │                                       │
│  ┌──────────────────────▼────────────────────────────────────┐  │
│  │             Fizyczny NIC Driver (eth0, ens3)               │  │
└──┴───────────────────────────────────────────────────────────┴──┘
                          │
                    Fizyczna sieć
```

**Kluczowy mechanizm:** Gdy jądro chce wysłać pakiet przez `tun0`, zamiast przekazać go do fizycznego NIC, **wkłada pakiet do kolejki** z której czyta userspace-owy proces (OpenVPN). OpenVPN szyfruje pakiet i wysyła przez **prawdziwy** interfejs (eth0) jako UDP/TCP datagram do serwera VPN.

---

### 16.3 TUN — Warstwa 3 (IP Tunnel)

### Jak działa TUN — krok po kroku

```
Scenariusz: Laptop (10.8.0.2) → przez VPN → serwer (192.168.1.10)

STRONA KLIENTA:
═══════════════

Krok 1: Aplikacja wysyła pakiet
  ping 192.168.1.10
  Kernel: "192.168.1.0/24 via tun0" (routing table)
  Kernel dostarcza pakiet do tun0

Krok 2: TUN driver → OpenVPN
  Pakiet IP wchodzi do kolejki tun0
  OpenVPN process: read(/dev/net/tun) → dostaje raw IP packet:

  ┌─────────────────────────────────────────────────────┐
  │  IP Header  │  ICMP Header  │  Payload              │
  │  Src:10.8.0.2│             │  (echo request)        │
  │  Dst:192.168.1.10          │                         │
  └─────────────────────────────────────────────────────┘

Krok 3: OpenVPN szyfruje (AES-256-GCM + HMAC)
  Dodaje OpenVPN header (peer ID, packet ID, timestamp)
  Wynik:
  ┌──────┬────────────────────────────────────────────┐
  │OpenVPN│  Zaszyfrowany (cały oryginalny IP pakiet) │
  │Header │  + MAC (authentication tag)               │
  └──────┴────────────────────────────────────────────┘

Krok 4: Wysłanie przez fizyczną sieć
  OpenVPN: write(UDP socket) → eth0 → Internet
  
  ┌──────────┬────────┬──────────┬──────────────────────┐
  │Ethernet  │  IP    │   UDP    │ OpenVPN+Encrypted IP  │
  │Header    │ Header │  Header  │ Payload               │
  │src: MACa │src:1.2.3.4│ :1194│                       │
  │dst: MACb │dst:5.6.7.8│      │                       │
  └──────────┴────────┴──────────┴──────────────────────┘

STRONA SERWERA:
═══════════════

Krok 5: Serwer OpenVPN odbiera UDP
  read(UDP socket) → dostaje zaszyfrowany payload
  Odszyfrowuje → oryginalny pakiet IP (src:10.8.0.2, dst:192.168.1.10)

Krok 6: write(/dev/net/tun) → tun0
  Jądro serwera: routing table → "192.168.1.0/24 via eth1"
  Pakiet wędruje do 192.168.1.10
```

### TUN Interface — Właściwości

```
tun0 po konfiguracji OpenVPN:
  IP:           10.8.0.1/24 (lub point-to-point)
  MTU:          1500 (domyślnie) → często zmniejszone do 1400-1450
                (musi zmieścić się w UDP datagram razem z encapsulation)
  Flags:        UP POINTOPOINT RUNNING NOARP MULTICAST
  Brak MAC:     TUN = L3 = nie ma adresu MAC!
  
ip addr show tun0:
  3: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc ...
      link/none                         ← brak MAC!
      inet 10.8.0.1 peer 10.8.0.2/32   ← point-to-point lub
      inet 10.8.0.1/24                  ← subnet mode
```

---

### 16.4 TAP — Warstwa 2 (Ethernet Bridge)

### Jak działa TAP — krok po kroku

```
Scenariusz: Klient (PC w biurze) przez VPN staje się częścią sieci biura
           Klient dostaje adres z DHCP serwera biurowego!

TAP działa jak WIRTUALNY KABEL SIECIOWY:

LAPTOP (w domu)              BIURO (serwer VPN)
══════════════               ════════════════
tap0                         tap0
 │                            │
 │  ←── Zaszyfrowana          │
 │       ramka Ethernet ───►  │
 │       (przez Internet)     │
 │                            │
 └── bridge (br0) ────────────┘── eth0 (fizyczna sieć biura)
                                     │
                              DHCP Server biura
                              Serwery biurowe
                              Drukarki biurowe

Klient dostaje IP 192.168.1.105 od DHCP serwera biura!
Ping do drukarki 192.168.1.50 → TAP działa jak kabel biurowy
ARP działa! Klient może "widzieć" wszystkie urządzenia L2!
```

### TAP Interface — Właściwości

```
tap0 po konfiguracji:
  ip addr show tap0:
  4: tap0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc ...
      link/ether 2e:a4:6c:bb:cc:dd brd ff:ff:ff:ff:ff:ff   ← MA MAC!
      inet 192.168.1.105/24                                  ← z DHCP biura

Kluczowe: TAP MA adres MAC → może uczestniczyć w ARP, spanning tree itd.
TAP można dodać do bridge:
  ip link set tap0 master br0
  → TAP staje się portem wirtualnego switcha
```

### Ramka Ethernet przez TAP vs IP przez TUN

```
TUN – co OpenVPN czyta:
┌─────────────┬─────────────┬──────────┐
│  IP Header  │  TCP Header │  Payload │
│  20 bytes   │  20 bytes   │  dane    │
└─────────────┴─────────────┴──────────┘
(brak Ethernet header — kernel go strippuje dla tun)

TAP – co OpenVPN czyta:
┌────────────────┬─────────────┬─────────────┬──────────┐
│ Ethernet Header│  IP Header  │  TCP Header │  Payload │
│  14 bytes      │  20 bytes   │  20 bytes   │  dane    │
│ dst MAC|src MAC│             │             │          │
│ EtherType      │             │             │          │
└────────────────┴─────────────┴─────────────┴──────────┘
(pełna ramka Ethernet — OpenVPN szyfruje całą)
```

---

### 16.5 TUN vs TAP — Kiedy Co Używać

```
┌─────────────────────────────────────────────────────────────────┐
│                    TUN vs TAP Porównanie                        │
├────────────────┬────────────────────────┬───────────────────────┤
│ Cecha          │ TUN (L3)               │ TAP (L2)              │
├────────────────┼────────────────────────┼───────────────────────┤
│ Warstwa OSI    │ 3 (Network — IP)       │ 2 (Data Link—Ethernet)│
│ Co przesyła    │ Pakiety IP             │ Ramki Ethernet        │
│ MAC address    │ ❌ Brak                │ ✅ Ma MAC             │
│ ARP support    │ ❌ Nie                 │ ✅ Tak                │
│ DHCP (L2)      │ ❌ Tylko statyczne IP  │ ✅ Pełny DHCP         │
│ Broadcast      │ ❌ Nie                 │ ✅ Tak                │
│ Bridge/Switch  │ ❌ Nie można bridgować │ ✅ Może być mostkowane │
│ Overhead       │ Niższy (brak 14B ETH) │ Wyższy (+14B ETH hdr) │
│ MTU efficiency │ Lepsze                 │ Nieco gorsze          │
│ IPv6 support   │ ✅ Tak                 │ ✅ Tak                │
│ Non-IP traffic │ ❌ Tylko IP            │ ✅ IPX, AppleTalk etc │
│ Typowe użycie  │ Remote access VPN,     │ Site-to-Site bridge,  │
│                │ Point-to-Point         │ VM networking, legacy │
│ OpenVPN mode   │ --dev tun (default)    │ --dev tap             │
│ AWS Client VPN │ ✅ Używa TUN           │ ❌ Nie używa          │
├────────────────┴────────────────────────┴───────────────────────┤
│ Kiedy TAP:                                                       │
│  - Potrzebujesz L2 bridge (VM migracja, legacy apps)           │
│  - NetBIOS/SMB broadcasting (Windows file shares przez VPN)    │
│  - Protokoły non-IP (rzadkie, legacy)                          │
│  - Emulacja fizycznej sieci LAN przez VPN                      │
│                                                                 │
│ Kiedy TUN (domyślnie):                                          │
│  - Większość przypadków remote access VPN                      │
│  - AWS Client VPN, WireGuard                                   │
│  - Lepsza wydajność i prostota                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### 16.6 Tworzenie TUN/TAP — Konfiguracja Ręczna

### Tworzymy TUN ręcznie (Linux)

```bash
# Załaduj moduł jądra (zwykle załadowany automatycznie)
sudo modprobe tun

# Utwórz TUN interface
sudo ip tuntap add dev tun0 mode tun user $(whoami)

# Lub z iproute2 (nowszy sposób):
sudo ip link add tun0 type tun
sudo ip tuntap add mode tun dev tun0

# Przypisz IP i uruchom
sudo ip addr add 10.8.0.1/24 dev tun0
sudo ip link set tun0 up

# Sprawdź
ip addr show tun0
ip link show tun0

# Usuń
sudo ip tuntap del dev tun0 mode tun
# lub
sudo ip link del tun0
```

### Tworzymy TAP ręcznie

```bash
# Utwórz TAP interface
sudo ip tuntap add dev tap0 mode tap user $(whoami)

# Przypisz IP
sudo ip addr add 192.168.100.1/24 dev tap0
sudo ip link set tap0 up

# Dodaj do bridge (opcjonalne — emulacja switcha):
sudo ip link add br0 type bridge
sudo ip link set tap0 master br0
sudo ip link set eth1 master br0      # dodaj fizyczny NIC do bridge
sudo ip link set br0 up
sudo ip addr add 192.168.1.1/24 dev br0  # IP na bridge, nie na tap0!

# Sprawdź bridge
bridge link show
```

### /dev/net/tun — plik urządzenia

```bash
# /dev/net/tun to jeden plik dla WSZYSTKICH TUN/TAP interfejsów
ls -la /dev/net/tun
# crw-rw-rw- 1 root root 10, 200 /dev/net/tun

# Otwarcie /dev/net/tun + ioctl(TUNSETIFF) tworzy nowy interfejs
# Każdy proces który otwiera /dev/net/tun dostaje własny deskryptor
# Deskryptor = jeden koniec "rury" do jądra

# OpenVPN robi mniej więcej:
# int fd = open("/dev/net/tun", O_RDWR);
# struct ifreq ifr = {.ifr_flags = IFF_TUN};
# strcpy(ifr.ifr_name, "tun0");
# ioctl(fd, TUNSETIFF, &ifr);
# → teraz read(fd) pobiera pakiety IP, write(fd) wysyła do stosu
```

---

### 16.7 OpenVPN — Jak Używa TUN/TAP

### Tryb TUN (--dev tun) — Remote Access VPN

```bash
# Typowa konfiguracja serwera OpenVPN (tun mode):
# /etc/openvpn/server.conf

port 1194
proto udp                    # UDP: szybsze, ale może być blokowane
dev tun                      # ← TUN mode!
ca ca.crt
cert server.crt
key server.key
dh dh2048.pem
tls-auth ta.key 0            # Dodatkowe HMAC na pakiety handshake

# VPN Subnet (nie część sieci biurowej)
server 10.8.0.0 255.255.255.0   # ← Klienci dostają 10.8.0.x
                                 # Serwer: 10.8.0.1, Klienci: 10.8.0.2+

# Routing — powiedz klientom żeby routowali 192.168.1.0/24 przez VPN
push "route 192.168.1.0 255.255.255.0"
push "dhcp-option DNS 192.168.1.1"

# Opcjonalnie: cały ruch przez VPN (full tunnel):
# push "redirect-gateway def1 bypass-dhcp"

keepalive 10 120             # Ping co 10s, restart po 120s braku odpowiedzi
cipher AES-256-GCM           # Szyfrowanie
auth SHA256                  # HMAC algorithm
compress lz4-v2              # Kompresja (ostrożnie z VORACLE attack)
max-clients 100
persist-key
persist-tun
```

### Routing po stronie klienta (co dzieje się z tabelą routingu)

```bash
# PRZED połączeniem VPN:
ip route show
# default via 192.168.0.1 dev eth0   ← cały ruch przez router domowy
# 192.168.0.0/24 dev eth0            ← sieć lokalna

# PO połączeniu OpenVPN (tun, split tunnel):
ip route show
# default via 192.168.0.1 dev eth0       ← Internet nadal przez domowy router
# 10.8.0.0/24 dev tun0                   ← VPN subnet przez tun0
# 192.168.1.0/24 via 10.8.0.1 dev tun0  ← BIURO przez VPN ← TO JEST CLUE
# 5.6.7.8/32 via 192.168.0.1 dev eth0   ← serwer VPN przez lokalny router

# FULL TUNNEL (push "redirect-gateway def1"):
ip route show
# 0.0.0.0/1 via 10.8.0.5 dev tun0        ← pierwsza połowa Internetu przez VPN
# 128.0.0.0/1 via 10.8.0.5 dev tun0      ← druga połowa Internetu przez VPN
# 5.6.7.8/32 via 192.168.0.1 dev eth0    ← TYLKO serwer VPN przez lokalny router
# default via 192.168.0.1 dev eth0        ← oryginalne default (nadpisane przez 0/1, 128/1)
```

> ⚠️ **Dlaczego dwie trasy 0.0.0.0/1 i 128.0.0.0/1 zamiast 0.0.0.0/0?**
> Bo `/0` (default route) jest nadpisywane przez bardziej specyficzne `/1` trasy.
> Trasa do serwera VPN (5.6.7.8/32) jest bardziej specyficzna niż `/1` więc nie idzie przez VPN (pętla!).

### Tryb TAP (--dev tap) — Bridge VPN

```bash
# /etc/openvpn/server-tap.conf
port 1194
proto udp
dev tap                         # ← TAP mode!
ca ca.crt
cert server.crt
key server.key
dh dh2048.pem

# W trybie TAP: używamy bridge zamiast server directive
# server 10.8.0.0... NIE używamy
# Zamiast tego konfigurujemy bridge w OS:

up "/etc/openvpn/bridge-start.sh"
down "/etc/openvpn/bridge-stop.sh"
```

```bash
# bridge-start.sh:
#!/bin/bash
BR="br0"
TAP="tap0"
ETH="eth0"
BRIDGE_IP="192.168.1.1"
BRIDGE_NETMASK="255.255.255.0"

# Utwórz bridge
brctl addbr $BR

# Dodaj tap0 i eth0 do bridge
ip link set $TAP promisc on
ip link set $ETH promisc on
brctl addif $BR $TAP
brctl addif $BR $ETH

# Nadaj IP bridgowi (nie eth0/tap0!)
ip addr add $BRIDGE_IP/$BRIDGE_NETMASK dev $BR
ip link set $BR up

# Klienci VPN dostają IP z DHCP serwera biura (przez bridge)!
```

---

### 16.8 Viscosity — macOS/Windows TUN/TAP

**Viscosity** to graficzny klient OpenVPN dla macOS i Windows. Pod spodem używa tych samych mechanizmów TUN/TAP.

### macOS — utun zamiast tun

```
macOS nie ma /dev/net/tun jak Linux.
macOS używa własnego mechanizmu: utun (user-space tunnel).

Dostępne przez: Network Extension framework (nowszy)
                lub TunnelKit / TunTap dla macOS (starszy)

Viscosity na macOS:
  Tworzy utun0, utun1, utun2...
  /var/log/viscosity.log:
  "Interface: utun2"
  "IP Address: 10.8.0.4"
  
Sprawdź w macOS:
  ifconfig utun0
  netstat -rn -f inet   ← routing table
  
# Na macOS routing działa identycznie jak Linux tun
# utun0 pojawia się jak zwykły interface:
# utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
#     inet 10.8.0.4 --> 10.8.0.5 netmask 0xffffffff
```

### Windows — TAP-Windows / Wintun

```
Windows OpenVPN klient używa:
  TAP-Windows6 (starszy):   wirtualny adapter widoczny w Device Manager
                            "TAP-Windows Adapter V9"
  
  Wintun (nowszy):          ring-buffer driver, szybszy
                            WireGuard używa Wintun

Viscosity na Windows:
  Instaluje TAP-Windows6 adapter
  Pojawia się w Network Connections jako:
    "TAP-Windows Adapter V9 #2"
  
  Przydzielony IP np. 10.8.0.4
  Routing jak na Linux/macOS

Sprawdź Windows:
  ipconfig /all             ← zobaczysz TAP adapter
  route print               ← routing table
  Get-NetAdapter -Name "TAP*"  ← PowerShell
```

### Viscosity — co robi pod spodem

```
Viscosity UI → OpenVPN proces → TUN/TAP driver → szyfrowanie → UDP → VPN serwer

Viscosity nie jest osobnym protokołem VPN!
Jest to GUI wrapper dla OpenVPN.
Może importować .ovpn pliki konfiguracyjne.
Obsługuje:
  - Profile zarządzania (wiele VPN)
  - DNS leak protection (wymusza DNS przez VPN)
  - IPv6 leak protection
  - Kill switch (blokada Internetu gdy VPN rozłączony)

Kill Switch działa przez:
  iptables/pf (firewall) — blokada non-VPN ruchu
  lub route manipulation — usuń default route gdy VPN padnie
```

---

### 16.9 Szyfrowanie w TUN — Warstwy Ochrony OpenVPN

```
Warstwa 1: TLS Control Channel (konfiguracja, wymiana kluczy)
  Protokół: TLS 1.2/1.3
  Cel: uzgodnienie klucza sesji, certyfikaty peer-to-peer
  Klucze: PKI (CA, Server cert, Client cert)
  
  ┌──────────────────────────────────────────────────────┐
  │                 TLS Handshake                        │
  │  Klient ──ServerHello──► Serwer                      │
  │  Klient ◄──Certificate── Serwer                      │
  │  Klient ──ClientCert───► Serwer  (mutual TLS)        │
  │  Klient ◄──SessionKey─── Serwer                      │
  │         Klucz sesji uzgodniony!                      │
  └──────────────────────────────────────────────────────┘

Warstwa 2: Data Channel (dane użytkownika przez tun0)
  Szyfrowanie: AES-256-GCM (recommended) lub AES-256-CBC
  HMAC: SHA256 / SHA512 (dla CBC) lub wbudowany w GCM
  
  Każdy pakiet IP z tun0:
  ┌───────────┬──────────────────────────────────────────┐
  │ OpenVPN   │  AES-256-GCM zaszyfrowany IP pakiet      │
  │ Packet    │  + 16-byte Authentication Tag (AEAD)     │
  │ Header    │  (zapobiega tampering)                   │
  │ (peer_id, │                                          │
  │  packet_id│                                          │
  │  timestamp│                                          │
  └───────────┴──────────────────────────────────────────┘

Warstwa 3: tls-auth / tls-crypt (Pre-TLS HMAC)
  tls-auth:   HMAC na pakiety kontrolne PRZED TLS handshake
              Chroni przed DDoS na TLS endpoint
  tls-crypt:  Szyfruje + HMAC pakiety kontrolne
              Silniejsze — ukrywa fakt że to OpenVPN
```

---

### 16.10 MTU — Krytyczny Problem TUN/TAP

```
Ethernet MTU: 1500 bytes (standard)

Problem z TUN/OpenVPN:
  Oryginalny pakiet IP:        1500 bytes
  + OpenVPN header:              29 bytes
  + UDP header:                   8 bytes
  + Outer IP header:             20 bytes
  + Ethernet header:             14 bytes
                              ──────────
  TOTAL:                       1571 bytes  ← za duże dla Ethernet!

Wynik: fragmentacja lub dropped packets

Rozwiązanie — zmniejsz MTU:

# Opcja 1: tun-mtu w OpenVPN config
tun-mtu 1400           # Wewnętrzny pakiet max 1400 bytes
fragment 1300          # Fragmentuj na poziomie OpenVPN jeśli >1300
mssfix 1300            # TCP MSS clamp (ważne!)

# Opcja 2: na interfejsie OS
ip link set tun0 mtu 1400

# mssfix — dlaczego ważne:
# TCP negocjuje MSS (Max Segment Size) w SYN handshake
# Jeśli MSS za duże → TCP segmenty będą za duże → fragmentacja na UDP
# mssfix przepisuje MSS w TCP SYN pakietach wychodzących przez tun
# mssfix = tun-mtu - 40 (IP+TCP headers) = 1400 - 40 = 1360 (typowo)

# Diagnostyka MTU:
ping -M do -s 1400 192.168.1.1    # Linux: -M do = don't fragment
ping -D -s 1400 192.168.1.1       # macOS
# jeśli "Frag needed" lub timeout → MTU za duże
```

---

### 16.11 AWS Client VPN — TUN Pod Spodem

**AWS Client VPN** to zarządzany OpenVPN-compatible service. Klient AWS VPN Client (lub dowolny klient OpenVPN) tworzy TUN interface na laptopie.

### Jak AWS Client VPN działa z TUN

```
Laptop pracownika
  │
  │  AWS VPN Client (lub OpenVPN klient):
  │  1. TLS handshake do AWS Client VPN Endpoint
  │  2. Mutual TLS (certyfikat klienta) lub SAML auth
  │  3. OpenVPN protokół
  │  4. Tworzy tun0 / utun0 (macOS)
  │
  ├──tun0: 10.0.64.2/22  ← z puli CIDR Client VPN (10.0.64.0/22)
  │
  │ OpenVPN UDP lub TCP → Internet → AWS Edge
  │                                    │
  ▼                                    ▼
AWS Client VPN Endpoint          ENI w subnecie VPC (eu-west-1a)
  10.0.64.1 (gateway)              10.0.1.x
                                      │
                                      └──► VPC (10.0.0.0/16)
                                              EC2, RDS, etc.

Routing po stronie klienta (split tunnel):
  10.0.0.0/16 → tun0       ← tylko VPC ruch przez VPN
  0.0.0.0/0 → eth0         ← Internet bezpośrednio (split tunnel)
```

### Client VPN .ovpn Plik — Anatomia

```openvpn
# Fragment pliku .ovpn pobranego z AWS Console

client
dev tun                    ← TUN mode (zawsze dla Client VPN)
proto udp                  ← UDP 443 lub TCP 443
remote cvpn-endpoint-xxx.prod.clientvpn.eu-west-1.amazonaws.com 443
remote-random-hostname     ← randomizuje hostname (dla DNS-based LB)
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-GCM
verb 3

<ca>
-----BEGIN CERTIFICATE-----
MIIDxx...  (AWS Certificate Authority)
-----END CERTIFICATE-----
</ca>

<cert>
-----BEGIN CERTIFICATE-----
MIIDxx...  (Client certificate — dla mutual TLS)
-----END CERTIFICATE-----
</cert>

<key>
-----BEGIN PRIVATE KEY-----
MIIExx...  (Client private key)
-----END PRIVATE KEY-----
</key>

<tls-auth>
# Static key pre-shared — tls-auth ta.key
-----BEGIN OpenVPN Static key V1-----
xxxxxxxx...
-----END OpenVPN Static key V1-----
</tls-auth>
key-direction 1
```

### Co dzieje się z tun po połączeniu z Client VPN

```bash
# Po połączeniu AWS Client VPN (macOS):
ifconfig utun4
# utun4: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
#     inet 10.0.64.5 --> 10.0.64.1 netmask 0xffffffff

netstat -rn -f inet | grep utun4
# 10.0.0.0/16   10.0.64.1    UGSc    utun4   ← VPC przez VPN
# 10.0.64.0/22  10.0.64.1    UGSc    utun4   ← Client VPN subnet

# Linux:
ip route show
# 10.0.0.0/16 via 10.0.64.1 dev tun0    ← VPC traffic przez VPN
# 10.0.64.0/22 dev tun0                 ← Client VPN subnet
```

---

### 16.12 AWS Site-to-Site VPN — IPsec, nie OpenVPN

Site-to-Site VPN nie używa TUN/TAP w tradycyjnym sensie — używa **IPsec** który jest implementowany głębiej w stosie sieciowym.

### IPsec vs OpenVPN/TUN

```
OpenVPN/TUN:
  Działa w userspace (OpenVPN daemon)
  Protokół aplikacji (UDP/TCP)
  Tworzy wirtualny interfejs tun0
  Elastyczne (dowolny port, HTTP-like)

IPsec (Site-to-Site VPN AWS):
  Działa w kernel space (wbudowany w jądro Linux/Windows)
  Protokoły IP: ESP (Encapsulating Security Payload, proto 50)
                AH (Authentication Header, proto 51)
                IKE (UDP port 500, 4500)
  Nie tworzy tun0! Działa "transparentnie" w stosie
  Bardziej standardowy, obsługiwany przez sprzęt (Cisco, Juniper)
```

### Jak IPsec tworzy wirtualny interfejs (Linux strongSwan/libreswan)

```bash
# IPsec też tworzy wirtualne interfejsy — ale inaczej niż TUN:

# strongSwan / XFRM (Linux kernel IPsec):
ip xfrm state list     ← security associations (SA)
ip xfrm policy list    ← security policies (SP)

# Interfejs VTI (Virtual Tunnel Interface) — dla IPsec:
ip link add vti0 type vti local 1.2.3.4 remote 5.6.7.8 key 0
ip addr add 169.254.x.x/30 dev vti0
ip link set vti0 up

# Routing przez VTI:
ip route add 10.0.0.0/16 dev vti0

# Na AWS CGW (on-premises router):
# Cisco IOS:
interface Tunnel1
  ip address 169.254.x.x 255.255.255.252
  ip virtual-reassembly
  tunnel source GigabitEthernet0/0
  tunnel mode ipsec ipv4
  tunnel destination 52.x.x.x  ← AWS VGW public IP
  tunnel protection ipsec profile AWS-VPN-Profile

# Palo Alto / Fortinet: GUI + CLI podobnie
```

### AWS VGW — Co dzieje się po stronie AWS

```
On-prem Router (CGW)
  │
  │ IPsec ESP (proto 50) zaszyfrowane pakiety
  │ IKEv2 (UDP 500/4500) key exchange
  │
  ▼
AWS VGW (Virtual Private Gateway) — zarządzane przez AWS
  │ Dwa tunele (różne publiczne IP, HA)
  │
  ▼
VPC Router
  │
  ▼
Route Table: 10.100.0.0/16 → vgw-xxxx (lub tgw-xxxx)
  │
  ▼
EC2 w private subnet
```

---

### 16.13 WireGuard — Nowoczesna Alternatywa dla TUN

**WireGuard** to nowoczesny protokół VPN, dużo prostszy od OpenVPN, wbudowany w jądro Linux od 5.6.

```bash
# WireGuard ZAWSZE używa TUN (nie TAP)
# Kernel module: wireguard.ko

# Utwórz WireGuard interface:
ip link add wg0 type wireguard

# Konfiguracja:
wg setconf wg0 /etc/wireguard/wg0.conf

# /etc/wireguard/wg0.conf (serwer):
[Interface]
Address = 10.200.0.1/24
ListenPort = 51820
PrivateKey = <base64 private key>

[Peer]
PublicKey = <client public key>
AllowedIPs = 10.200.0.2/32    ← klient może używać tego IP

# Uruchom:
wg-quick up wg0

# Sprawdź:
wg show
ip addr show wg0
# wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue
#   inet 10.200.0.1/24 scope global wg0

# WireGuard używa UDP (domyślnie 51820)
# Klucze: Curve25519 (ECDH), ChaCha20-Poly1305 szyfrowanie
# Dużo prostszy config niż OpenVPN (brak PKI per se)
```

### WireGuard vs OpenVPN/TUN

```
┌─────────────────┬─────────────────────┬─────────────────────┐
│ Cecha           │ OpenVPN/TUN         │ WireGuard/TUN        │
├─────────────────┼─────────────────────┼─────────────────────┤
│ Kernel/User     │ Userspace daemon    │ Kernel space (5.6+) │
│ Kod (LOC)       │ ~70,000            │ ~4,000               │
│ Konfiguracja    │ Złożona (.conf)     │ Prosta (key-based)  │
│ PKI             │ Wymagane (CA, cert) │ Nie wymagane        │
│ Protokół        │ UDP lub TCP         │ UDP only            │
│ Szyfrowanie     │ AES-256-GCM         │ ChaCha20-Poly1305   │
│ Latency         │ Wyższe (userspace)  │ Niższe (kernel)     │
│ Throughput      │ ~200 Mbps / core    │ ~1 Gbps / core      │
│ Mobilność       │ Dobra               │ Doskonała (roaming) │
│ AWS Client VPN  │ ✅ Używany          │ ❌ Nie obsługiwany  │
│ AWS S2S VPN     │ ❌                  │ ❌ (używa IPsec)    │
│ Interface       │ tun0                │ wg0                 │
└─────────────────┴─────────────────────┴─────────────────────┘
```

---

### 16.14 Diagnostyka TUN/TAP — Narzędzia

```bash
# ═══════════════════════════════════════════════
# DIAGNOSTYKA INTERFEJSÓW
# ═══════════════════════════════════════════════

# Lista wszystkich interfejsów (w tym tun/tap):
ip link show
ip addr show
ifconfig -a          # starsze systemy

# Tylko TUN/TAP interfejsy:
ip link show type tun
ip link show type tap

# Statystyki ruchu:
ip -s link show tun0
ethtool -S tun0      # nie zawsze dostępne dla virtual

# ═══════════════════════════════════════════════
# DIAGNOSTYKA ROUTINGU
# ═══════════════════════════════════════════════

# Pełna routing table:
ip route show table all
ip route show
netstat -rn

# Czy pakiet idzie przez tun0?
ip route get 192.168.1.10
# 192.168.1.10 via 10.8.0.1 dev tun0 src 10.8.0.2

# Sprawdź czy trasa istnieje:
ip route show 192.168.1.0/24

# ═══════════════════════════════════════════════
# DIAGNOSTYKA VPN POŁĄCZENIA
# ═══════════════════════════════════════════════

# OpenVPN logi (zwiększ verbosity w config: verb 9):
journalctl -u openvpn@server -f
tail -f /var/log/openvpn/openvpn.log

# Czy OpenVPN nasłuchuje na porcie?
ss -lunp | grep 1194     # UDP 1194
ss -ltnp | grep 1194     # TCP 1194

# Czy tunel przesyła dane?
watch -n1 'ip -s link show tun0'
# TX/RX bytes rosnące = dane przepływają

# tcpdump na tun0 (pakiety przed szyfrowaniem):
tcpdump -i tun0 -n

# tcpdump na eth0 (zaszyfrowany ruch UDP):
tcpdump -i eth0 udp port 1194 -n

# ═══════════════════════════════════════════════
# MTU DIAGNOSTYKA
# ═══════════════════════════════════════════════

# Test MTU path discovery:
ping -M do -s 1400 10.8.0.1    # DF bit set, 1400 bytes payload
ping -M do -s 1450 10.8.0.1    # zwiększaj do pojawienia się błędu
# "ping: local error: message too long, mtu=1430" → MTU = 1430

# Lub przez tracepath:
tracepath 192.168.1.10          # pokazuje MTU per hop

# ═══════════════════════════════════════════════
# DEBUGGING TAP BRIDGE
# ═══════════════════════════════════════════════

# Sprawdź bridge:
bridge link show
brctl show

# ARP table przez bridge:
arp -n
ip neigh show

# Czy MAC adresy przepływają?
tcpdump -i tap0 arp    # ARP na tap interfejsie
tcpdump -i br0 -en    # ramki Ethernet na bridge (-e = show MAC)
```

---

### 16.15 Bezpieczeństwo TUN/TAP — Zagrożenia i Ochrona

```
Zagrożenie 1: DNS Leak
  Problem: System DNS resolver używa eth0 zamiast tun0
  Sprawdź: https://dnsleaktest.com
  
  Rozwiązanie (OpenVPN):
    push "dhcp-option DNS 10.8.0.1"     ← wymuś DNS przez VPN
    script-security 2
    up /etc/openvpn/update-resolv-conf  ← skrypt aktualizuje /etc/resolv.conf
    down /etc/openvpn/update-resolv-conf
  
  Lub: systemd-resolved z DNS over VPN:
    [Resolve]
    DNS=10.8.0.1
    Domains=~.                          ← wszystkie domeny przez VPN DNS

Zagrożenie 2: IPv6 Leak
  Problem: VPN tuneluje tylko IPv4, IPv6 idzie bezpośrednio
  
  Rozwiązanie:
    push "block-outside-dns"
    ifconfig-ipv6 fd00::/8
    push "route-ipv6 ::/0"             ← full IPv6 tunnel
    lub: wyłącz IPv6 na interfejsie
    sysctl -w net.ipv6.conf.eth0.disable_ipv6=1

Zagrożenie 3: Kill Switch brak
  Problem: VPN rozłączone → ruch przez Internet bez szyfrowania
  
  Rozwiązanie (iptables):
    # Zezwól tylko ruch przez VPN:
    iptables -F
    iptables -P INPUT DROP
    iptables -P OUTPUT DROP
    iptables -P FORWARD DROP
    
    # Zezwól loopback:
    iptables -A INPUT -i lo -j ACCEPT
    iptables -A OUTPUT -o lo -j ACCEPT
    
    # Zezwól VPN server connection (przed tunelem):
    iptables -A OUTPUT -o eth0 -d <vpn_server_ip> -p udp --dport 1194 -j ACCEPT
    iptables -A INPUT -i eth0 -s <vpn_server_ip> -p udp --sport 1194 -j ACCEPT
    
    # Zezwól cały ruch przez tun:
    iptables -A INPUT -i tun0 -j ACCEPT
    iptables -A OUTPUT -o tun0 -j ACCEPT
    
    # VPN rozłączone → iptables blokuje → Kill Switch działa

Zagrożenie 4: TUN/TAP privilege escalation
  Problem: /dev/net/tun dostępny dla normalnych userów
  
  Rozwiązanie:
    # Ogranicz dostęp:
    chown root:vpngroup /dev/net/tun
    chmod 660 /dev/net/tun
    
    # OpenVPN --user/--group:
    user nobody
    group nogroup          ← drop privilegies po inicjalizacji
```

---

### 16.16 TUN/TAP w Kontekście AWS — Mapa Powiązań

```
┌─────────────────────────────────────────────────────────────────┐
│              VPN Solutions i TUN/TAP w AWS                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AWS Client VPN (zarządzany OpenVPN)                           │
│    Klient: tun0 / utun0 (macOS) / TAP-Windows (Windows)        │
│    Serwer: Zarządzany przez AWS (nie widać TUN po stronie AWS)  │
│    Protokół: OpenVPN (UDP 443 lub TCP 443)                      │
│    Szyfrowanie: AES-256-GCM                                     │
│                                                                 │
│  AWS Site-to-Site VPN (IPsec)                                  │
│    Klient (CGW): IPsec VTI lub policy-based                    │
│    Cisco/Juniper/Palo Alto: własny IPsec driver                 │
│    Linux (strongSwan): ip xfrm, opcjonalnie VTI interface       │
│    Protokół: IKEv2 + ESP (nie OpenVPN, nie TUN)                │
│                                                                 │
│  Własny OpenVPN na EC2                                         │
│    EC2 serwer: tun0 lub tap0                                    │
│    Klienci: tun0 / utun0                                       │
│    Pełna kontrola (dobry dla BYOL OpenVPN Access Server)        │
│                                                                 │
│  AWS Direct Connect                                            │
│    Warstwa fizyczna/L2 — bez TUN/TAP                           │
│    Dedykowane łącze fizyczne                                    │
│                                                                 │
│  Transit Gateway (VPN attachment)                              │
│    IPsec tunele do TGW — jak S2S VPN                           │
│    Nie TUN/TAP — kernel IPsec lub VTI po stronie CGW           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

Kiedy uruchamiasz własny OpenVPN na EC2:
  
  aws ec2 run-instances --instance-type t3.small  ← EC2 jako VPN serwer
  
  Na EC2 (Linux):
    modprobe tun
    tun0 tworzony przez OpenVPN
    
  Ważne dla EC2 jako VPN router:
    # Włącz IP forwarding (wymagane!):
    echo 1 > /proc/sys/net/ipv4/ip_forward
    sysctl -w net.ipv4.ip_forward=1
    
    # Wyłącz Source/Destination Check na EC2:
    aws ec2 modify-instance-attribute \
      --instance-id i-12345678 \
      --source-dest-check '{"Value": false}'
    
    # Bez tego: EC2 odrzuca pakiety gdzie src/dst != własne IP
    # (EC2 hypervisor sprawdza i droppuje)
    
    # NAT dla klientów VPN (maskarada):
    iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
```

---

### 16.17 Well-Architected Framework — TUN/TAP i VPN

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Centralizuj logi OpenVPN (CloudWatch Logs Agent), monitoring tun0 stats przez CloudWatch custom metrics, automate certificate rotation (ACM + Lambda), Infrastructure as Code dla VPN config |
| **Security** | TLS mutual auth (certyfikaty, nie tylko hasła), tls-crypt (ukryj że to OpenVPN), Kill Switch dla klientów, DNS leak protection, IMDSv2 dla EC2 VPN servera, minimal privilege (nobody/nogroup), wyłącz Source/Dest Check tylko gdy wymagane |
| **Reliability** | AWS Client VPN: multi-AZ (wiele subnet associations), OpenVPN własny: ASG za NLB, keepalive + restart on failure, certificate expiry monitoring |
| **Performance Efficiency** | TUN > TAP (mniejszy overhead, brak L2), AES-256-GCM (hardware AES-NI), UDP > TCP dla VPN (brak TCP-over-TCP problem), odpowiednie MTU/mssfix, WireGuard dla wyższej przepustowości |
| **Cost Optimization** | AWS Client VPN vs własny EC2 VPN: Client VPN $0.10/h/subnet + $0.05/h/user vs EC2 t3.small ~$15/mc + data transfer. Dla <50 users: EC2 tańszy. Dla >100: Client VPN łatwiejszy w zarządzaniu |
| **Sustainability** | Graviton EC2 dla VPN servera (lepsza efektywność energetyczna), prawidłowe MTU = mniej fragmentacji = mniej retransmisji |

---

## 17. Security Groups — Deep Dive: ENI, EC2, VPC, Reguły

> **Zakres:** Pełny deep-dive Security Groups w AWS. Jak działają na poziomie ENI, EC2, VPC. Stateful vs stateless. Reguły inbound/outbound. Self-referencing. Chaining SG. Różnice od NACL. Troubleshooting. 60 Q&A.

---

### 17.1 Czym Jest Security Group — Fundament

**Security Group (SG)** to wirtualny, **stanowy (stateful)** firewall działający na poziomie **ENI (Elastic Network Interface)**. Nie jest to firewall sieciowy — jest to firewall **na poziomie interfejsu sieciowego konkretnej instancji**.

```
┌─────────────────────────────────────────────────────────────────┐
│                         VPC                                     │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Subnet 10.0.1.0/24                    │  │
│  │                                                          │  │
│  │   ┌─────────────────────────────────────────────────┐   │  │
│  │   │                   EC2 Instance                  │   │  │
│  │   │                                                 │   │  │
│  │   │   ┌──────────────────────────────────────────┐  │   │  │
│  │   │   │              ENI (eth0)                   │  │   │  │
│  │   │   │              10.0.1.15                    │  │   │  │
│  │   │   │                   │                       │  │   │  │
│  │   │   │            ┌──────▼──────┐                │  │   │  │
│  │   │   │            │ Security    │  ← TUTAJ działa│  │   │  │
│  │   │   │            │ Group       │    SG! Na ENI  │  │   │  │
│  │   │   │            │ (sg-abc123) │                │  │   │  │
│  │   │   │            └─────────────┘                │  │   │  │
│  │   │   └──────────────────────────────────────────┘  │   │  │
│  │   └─────────────────────────────────────────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

**Kluczowe fakty:**
- SG przypisany do **ENI**, nie do EC2 bezpośrednio (EC2 ma ENI → ENI ma SG)
- Jeden ENI może mieć do **5 SG** jednocześnie
- Jedna EC2 może mieć wiele ENI (eth0, eth1...) — każde z osobnym SG
- SG to **nie jest sieć** — nie blokuje na poziomie routera/switcha
- SG działa w **hypervisorze** — ruch jest filtrowany zanim dotrze do VM

---

### 17.2 ENI — Elastic Network Interface

ENI to wirtualna karta sieciowa. Zrozumienie ENI = zrozumienie SG.

```
┌──────────────────────────────────────────────────────────────┐
│                    EC2 Instance                              │
│                                                              │
│  ┌────────────────────────┐  ┌────────────────────────────┐  │
│  │    ENI Primary (eth0)  │  │    ENI Secondary (eth1)    │  │
│  │                        │  │                            │  │
│  │  Private IP: 10.0.1.15 │  │  Private IP: 10.0.2.50    │  │
│  │  Public IP: 54.x.x.x  │  │  Private IP: 10.0.2.51    │  │
│  │  EIP: (opcjonalnie)    │  │  (wiele private IP!)       │  │
│  │  MAC: aa:bb:cc:dd:ee:ff│  │  MAC: 11:22:33:44:55:66   │  │
│  │                        │  │                            │  │
│  │  SG: sg-web (port 80)  │  │  SG: sg-admin (port 22)   │  │
│  │  SG: sg-https (443)    │  │                            │  │
│  └────────────────────────┘  └────────────────────────────┘  │
│         Subnet A                      Subnet B               │
└──────────────────────────────────────────────────────────────┘
```

**ENI właściwości:**
- Każde ENI ma osobny **MAC address** (stały, nawet przez restart)
- ENI primary (eth0) — nie można odłączyć od działającej instancji
- ENI secondary (eth1+) — można **przenosić między instancjami** (hot-attach/detach)
- Przeniesienie ENI = przeniesienie wszystkich jego SG, EIP, prywatnych IP
- ENI może być w **innym subnecie** niż primary ENI (ale tym samym AZ!)

**Przenoszenie ENI — use case:**
```
Scenariusz: EC2 padło, trzeba szybko przenieść ruch na nową instancję

EC2-Old (ip: 10.0.1.15, EIP: 54.x.x.x, SG: sg-web)
  ↓ detach eth1 (z EIP i SG)
EC2-New (attach eth1)
  → EC2-New natychmiast dostaje IP 10.0.1.15, EIP, SG
  → Zero zmian DNS, Security Groups, whitelist
```

---

### 17.3 Jak SG Działa — Hypervisor Level

Ruch nigdy nie dociera do EC2 zanim SG go nie przeanalizuje:

```
Ruch przychodzący (Inbound):

Internet / inna EC2
        │
        │ pakiet TCP: src=10.0.2.5:54321 dst=10.0.1.15:443
        │
        ▼
┌───────────────────────────────────────────────────┐
│               AWS Hypervisor / VPC SDN             │
│                                                   │
│  1. Lookup ENI dla IP 10.0.1.15                   │
│  2. Pobierz wszystkie SG przypisane do tego ENI   │
│  3. Sprawdź każdy SG — czy któryś ZEZWALA?        │
│                                                   │
│  SG-1: port 443 from 0.0.0.0/0  → MATCH ✅       │
│                                                   │
│  Decyzja: ALLOW                                   │
└───────────────────────────────────────────────────┘
        │
        ▼
    EC2 (10.0.1.15) otrzymuje pakiet

────────────────────────────────────────────────────

Ruch wychodzący (Outbound) odpowiedź (stateful!):

EC2 wysyła odpowiedź TCP ACK
        │
        ▼
┌───────────────────────────────────────────────────┐
│               AWS Hypervisor / VPC SDN             │
│                                                   │
│  STATEFUL: to jest odpowiedź na dozwolone         │
│  połączenie przychodzące                          │
│  → AUTOMATYCZNIE DOZWOLONE                        │
│  → Nie sprawdza Outbound Rules!                   │
│                                                   │
└───────────────────────────────────────────────────┘
        │
        ▼
    Odpowiedź dociera do klienta
```

---

### 17.4 Stateful — Co To Naprawdę Oznacza

**Stateful** = SG śledzi **stan połączeń** (connection tracking).

```
Connection Tracking Table (w hypervisorze):

┌────────────────────┬──────────┬──────────┬──────────┬────────┐
│ Source             │ Src Port │ Dest     │Dst Port  │ State  │
├────────────────────┼──────────┼──────────┼──────────┼────────┤
│ 203.0.113.5        │ 54321    │10.0.1.15 │ 443      │ESTAB.  │
│ 10.0.2.20          │ 33445    │10.0.1.15 │ 3306     │ESTAB.  │
│ 10.0.1.10          │ 45678    │10.0.1.15 │ 80       │SYN_SENT│
└────────────────────┴──────────┴──────────┴──────────┴────────┘

Gdy EC2 odpowiada na 203.0.113.5:54321:
  → Hypervisor: "To jest odpowiedź na tracked connection"
  → Nie sprawdza outbound rules
  → Pakiet przechodzi

Gdy EC2 INICJUJE połączenie do 10.0.2.100:5432:
  → Hypervisor: "Nowe połączenie — sprawdź Outbound Rules"
  → Outbound rule: port 5432 to 10.0.2.0/24 → MATCH ✅
  → Odpowiedź od 10.0.2.100 → tracked → przechodzi auto
```

**Praktyczna konsekwencja:**
```
Inbound: port 443 from 0.0.0.0/0
Outbound: (domyślnie ALL open)

→ HTTPS odpowiedzi: działają (stateful — odpowiedź na inbound)
→ EC2 inicjuje połączenie do S3: działa (outbound ALL)
→ EC2 inicjuje połączenie do innego EC2 port 8080: 
    outbound EC2-A: ALL → OK
    inbound EC2-B: brak reguły dla 8080 → BLOCKED ❌
```

---

### 17.5 Reguły SG — Anatomia

Każda reguła SG składa się z:

```
┌──────────────┬───────────────────────────────────────────────┐
│ Pole         │ Opis i wartości                               │
├──────────────┼───────────────────────────────────────────────┤
│ Type         │ Custom TCP/UDP/ICMP lub predefiniowane (SSH,   │
│              │ HTTP, HTTPS, RDP, MySQL, PostgreSQL...)       │
│ Protocol     │ TCP / UDP / ICMP / ICMPv6 / All (wszystko)   │
│ Port Range   │ Pojedynczy port (443) lub zakres (8000-9000)  │
│ Source/Dest  │ IP CIDR (10.0.0.0/8) lub SG ID (sg-abc123)  │
│              │ lub własny SG (self-referencing)              │
│ Description  │ Opcjonalny opis (max 255 znaków)             │
└──────────────┴───────────────────────────────────────────────┘
```

**Source/Destination — możliwości:**

```
Inbound rules — Source:
  ┌────────────────────────────────────────────────────────┐
  │  0.0.0.0/0         → cały Internet (IPv4)             │
  │  ::/0              → cały Internet (IPv6)             │
  │  10.0.1.0/24       → konkretna podsieć                │
  │  10.0.2.5/32       → konkretny IP (jeden host)        │
  │  sg-0abc123        → inny Security Group              │
  │  sg-self (własne)  → self-referencing                 │
  └────────────────────────────────────────────────────────┘

Outbound rules — Destination:
  (identyczne opcje jak Source)
```

---

### 17.6 SG Source = Security Group ID — Magia AWS

To najważniejsza i najczęściej używana funkcja SG.

### Zamiast IP — używaj SG jako source/destination

```
Tradycyjne podejście (IP-based):
  SG-RDS inbound: port 3306 from 10.0.1.0/24
  
  Problem: co gdy EC2 dostanie nowy IP po restart?
           co gdy dodam nowe EC2 w innym subnecie?
           co gdy ASG skaluje do innego zakresu IP?

Podejście AWS (SG-based):
  SG-RDS inbound: port 3306 from sg-webapp
  
  Znaczenie: "Zezwól na port 3306 od KAŻDEGO ENI który ma przypisany sg-webapp"
  
  Nowe EC2 → przypisz sg-webapp → automatycznie dostęp do RDS ✅
  Usuń EC2 → SG reguła niezmieniona, ale EC2 już nie istnieje ✅
  ASG skalowanie → nowe instancje mają sg-webapp → dostęp auto ✅
```

### Jak to działa pod spodem

```
EC2-App (sg-webapp, IP: 10.0.1.25) → MySQL (RDS, sg-rds)

SG-RDS inbound: 
  port 3306  source: sg-webapp

Hypervisor otrzymuje pakiet:
  src: 10.0.1.25:45123  dst: 10.0.2.100:3306

Sprawdza: "Który SG jest przypisany do ENI z IP 10.0.1.25?"
Odpowiedź: "sg-webapp"
Porównuje z regułą: "3306 from sg-webapp" → MATCH ✅

EC2-Hacker (sg-unknown, IP: 10.0.3.50) → MySQL:
  src: 10.0.3.50:12345  dst: 10.0.2.100:3306
  
ENI 10.0.3.50 ma sg-unknown ≠ sg-webapp → NO MATCH ❌ → BLOCKED
```

### Typowy schemat SG chaining (łańcuch SG)

```
Internet
   │
   │ port 443
   ▼
┌──────────────────┐
│   ALB            │  SG-ALB: inbound 443 from 0.0.0.0/0
│   sg-alb         │         outbound 8080 to sg-webapp
└──────────────────┘
   │
   │ port 8080 (tylko z sg-alb!)
   ▼
┌──────────────────┐
│   EC2 App        │  SG-App: inbound 8080 from sg-alb
│   sg-webapp      │         outbound 3306 to sg-rds
└──────────────────┘
   │
   │ port 3306 (tylko z sg-webapp!)
   ▼
┌──────────────────┐
│   RDS MySQL      │  SG-RDS: inbound 3306 from sg-webapp
│   sg-rds         │
└──────────────────┘

Hacker z Internetu nie może:
  → Bezpośrednio trafić do EC2 (tylko ALB może, przez sg-alb)
  → Bezpośrednio trafić do RDS (tylko EC2-App może, przez sg-webapp)
```

---

### 17.7 Self-Referencing Security Group

SG może zezwolić na ruch od instancji które **mają ten sam SG**.

```
SG-Cluster:
  Inbound: ALL traffic  source: sg-cluster (self!)
  
┌─────────────────────────────────────────────────────┐
│                 VPC                                 │
│                                                     │
│  EC2-Node1 ──────────────────► EC2-Node2           │
│  [sg-cluster]   ANY PORT       [sg-cluster]  ✅    │
│                                                     │
│  EC2-Node1 ──────────────────► EC2-Node3           │
│  [sg-cluster]                  [sg-cluster]  ✅    │
│                                                     │
│  EC2-External ────────────────► EC2-Node1          │
│  [sg-other]                    [sg-cluster]  ❌    │
│                                                     │
└─────────────────────────────────────────────────────┘

Use cases:
  - Klaster baz danych (MongoDB replica set, Cassandra)
  - Klaster aplikacji (Elasticsearch, Kafka)
  - ECS tasks w tym samym cluster
  - Kubernetes worker nodes
```

---

### 17.8 Wiele SG na ENI — Jak Działa Union

ENI może mieć do **5 SG**. Reguły są łączone przez **OR (union)**.

```
EC2 ma przypisane:
  SG-Web:   inbound port 80, 443 from 0.0.0.0/0
  SG-Admin: inbound port 22 from 10.0.0.0/8
  SG-Mon:   inbound port 9100 from sg-prometheus

Efektywne reguły = UNION wszystkich SG:
  port 80   from 0.0.0.0/0       (z SG-Web)
  port 443  from 0.0.0.0/0       (z SG-Web)
  port 22   from 10.0.0.0/8      (z SG-Admin)
  port 9100 from sg-prometheus   (z SG-Mon)

Pytanie: czy mogę w SG-Admin zablokować port 80?
Odpowiedź: NIE! SG nie ma reguł DENY — tylko ALLOW.
           Nie możesz "cofnąć" reguły z innego SG.
           Jedyna opcja: usuń SG-Web lub zmodyfikuj jego reguły.
```

**SG = TYLKO ALLOW, NIGDY DENY** — to fundamentalna różnica od tradycyjnych firewalli.

---

### 17.9 Default Security Group vs Custom Security Group

```
┌─────────────────────────────────────────────────────────────────┐
│              Default SG (tworzony automatycznie z VPC)          │
├───────────────────────┬─────────────────────────────────────────┤
│ Inbound               │ ALL traffic  from  default-sg (self)    │
│ Outbound              │ ALL traffic  to    0.0.0.0/0            │
├───────────────────────┴─────────────────────────────────────────┤
│ Efekt: instancje z default SG mogą się komunikować wzajemnie    │
│ (self-referencing), ale nie z innymi SG. Internet wychodzący OK.│
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│              Nowo Utworzony Custom SG                           │
├───────────────────────┬─────────────────────────────────────────┤
│ Inbound               │ (puste — wszystko zablokowane)          │
│ Outbound              │ ALL traffic  to    0.0.0.0/0 (default) │
├───────────────────────┴─────────────────────────────────────────┤
│ Efekt: nic nie może wejść do EC2. EC2 może inicjować dowolne    │
│ połączenia wychodzące.                                          │
└─────────────────────────────────────────────────────────────────┘

Best Practice: NIE używaj Default SG dla workloadów produkcyjnych.
Twórz dedykowane SG per warstwa (web, app, db, mgmt).
```

---

### 17.10 SG na Poziomie VPC vs Subnet vs ENI

Częste zamieszanie: gdzie "siedzi" SG?

```
VPC Level:
  SG jest ZASOBEM VPC — tworzony w konkretnym VPC
  Jeden SG nie może być przypisany do ENI w innym VPC!
  
  VPC-A: sg-web-a (tylko dla EC2 w VPC-A)
  VPC-B: sg-web-b (tylko dla EC2 w VPC-B)
  
  Po VPC Peering: sg-web-a NIE może być source w SG w VPC-B
  (Reguły SG cross-VPC: musisz użyć CIDR IP, nie SG ID)

Subnet Level:
  SG nie działa na poziomie subnetu — to jest NACL (Network ACL)
  SG ignoruje granice subnetów!
  
  EC2 w subnet-A → EC2 w subnet-B (ten sam VPC):
  Routing: automatyczne w VPC (local route)
  Filtrowanie: tylko SG obu EC2 (nie NACL subnetu, chyba że ruch 
               przez granicę subnetu → NACL też sprawdza)

ENI Level:
  SG działa TUTAJ. Każde ENI = osobny zestaw SG.
  EC2 z 2x ENI może mieć różne SG na każdym interfejsie.
```

### Ilustracja: EC2 w tej samej subnecie

```
Subnet 10.0.1.0/24
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  EC2-A                          EC2-B                   │
│  eth0: 10.0.1.10                eth0: 10.0.1.20         │
│  SG: sg-open                    SG: sg-empty            │
│    inbound: ALL from 0.0.0.0/0    inbound: (puste)      │
│                                                         │
│                                                         │
│  EC2-A ──── SSH port 22 ────► EC2-B                    │
│                                                         │
│  Routing: 10.0.1.20 → local route → bezpośrednio       │
│  SG EC2-B: brak reguły dla port 22 → BLOCKED ❌        │
│                                                         │
│  Fizyczna sieć "widzi" pakiet, ale hypervisor go        │
│  droppuje przed dostarczeniem do EC2-B!                │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Fizyczna sieć = nieistotna dla SG.** Sąsiedztwo w subnecie nie daje żadnych przywilejów.

---

### 17.11 SG vs NACL — Kluczowe Różnice

```
┌──────────────────────┬────────────────────────┬──────────────────────┐
│ Cecha                │ Security Group         │ Network ACL (NACL)   │
├──────────────────────┼────────────────────────┼──────────────────────┤
│ Poziom               │ ENI (instancja)        │ Subnet               │
│ Stan                 │ Stateful               │ Stateless            │
│ Reguły               │ Tylko ALLOW            │ ALLOW i DENY         │
│ Ocena reguł          │ Union wszystkich SG    │ Numerycznie (100, 200│
│                      │ (wszystkie sprawdzane) │ pierwsza pasująca)   │
│ Ephemeral ports      │ Auto (stateful)        │ Musisz dodać outbound│
│                      │                        │ 1024-65535 ręcznie   │
│ Zasięg               │ Ten sam VPC            │ Ten sam subnet       │
│ Domyślna akcja       │ Implicit deny (brak reguły)│ Explicit deny rule│
│ Kto zarządza         │ DevOps / App teams     │ Network / Security   │
│ Typowe użycie        │ Per-instance granular  │ Subnet-level broad   │
│                      │ control                │ deny/allow           │
│ Source jako SG       │ ✅ Tak                  │ ❌ Tylko CIDR        │
└──────────────────────┴────────────────────────┴──────────────────────┘
```

### NACL Stateless — Dlaczego Ephemeral Ports Ważne

```
Klient → EC2 port 443:
  
NACL Inbound:
  100: ALLOW TCP 443 from 0.0.0.0/0  ✅ (żądanie przechodzi)

Odpowiedź EC2 → Klient:
  Klient: port SOURCE był np. 54321 (ephemeral: 1024-65535)
  EC2 odpowiada: dst=203.0.113.5:54321

NACL Outbound (stateless! nie pamięta że 54321 to odpowiedź!):
  Sprawdza: "czy mam regułę dla TCP dst port 54321?"
  Bez reguły: BLOCKED ❌

Fix dla NACL:
  Outbound: ALLOW TCP 1024-65535 to 0.0.0.0/0  (ephemeral range)

SG (stateful): to jest AUTOMATYCZNIE obsługiwane, bez dodatkowych reguł.
```

---

### 17.12 Outbound Rules — Kiedy Ograniczać

Domyślnie outbound SG = `ALL traffic to 0.0.0.0/0` — wszystko wychodzi.

**Kiedy warto ograniczyć outbound:**

```
Scenariusz: EC2 z danymi wrażliwymi (PCI DSS, HIPAA)

SG-Database outbound:
  ❌ Usuń ALL 0.0.0.0/0
  ✅ Dodaj:
     port 5432 to sg-webapp (odpowiedź na DB connections)
     port 443 to 0.0.0.0/0 (AWS API calls: S3, CloudWatch, SSM)
     port 53 to VPC CIDR (DNS — Route53 Resolver)

Efekt:
  EC2 DB nie może zainicjować połączenia do Internetu
  Nawet jeśli malware zainfekuje EC2, nie "zadzwoni do domu"
  Data exfiltration przez wychodzące połączenie = niemożliwe

Ale uwaga: stateful! Odpowiedzi na INBOUND connections 
           przechodzą automatycznie niezależnie od outbound rules.
```

---

### 17.13 Jak Dodać / Zmodyfikować SG — AWS CLI

```bash
# ═══════════════════════════════════════════════════════════
# TWORZENIE SG
# ═══════════════════════════════════════════════════════════

# Utwórz SG w VPC:
aws ec2 create-security-group \
  --group-name sg-webapp \
  --description "Web Application Security Group" \
  --vpc-id vpc-12345678

# Output: {"GroupId": "sg-0abc123def456789"}

# ═══════════════════════════════════════════════════════════
# DODAWANIE REGUŁ INBOUND
# ═══════════════════════════════════════════════════════════

# Dodaj port 443 z Internetu:
aws ec2 authorize-security-group-ingress \
  --group-id sg-0abc123def456789 \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0

# Dodaj port 3306 z konkretnego SG:
aws ec2 authorize-security-group-ingress \
  --group-id sg-0rds111111111111 \
  --protocol tcp \
  --port 3306 \
  --source-group sg-0abc123def456789

# Dodaj ICMP (ping) z VPC CIDR:
aws ec2 authorize-security-group-ingress \
  --group-id sg-0abc123def456789 \
  --protocol icmp \
  --port -1 \
  --cidr 10.0.0.0/16

# Dodaj zakres portów:
aws ec2 authorize-security-group-ingress \
  --group-id sg-0abc123def456789 \
  --ip-permissions '[{
    "IpProtocol": "tcp",
    "FromPort": 8000,
    "ToPort": 9000,
    "IpRanges": [{"CidrIp": "10.0.0.0/8", "Description": "Internal apps"}]
  }]'

# ═══════════════════════════════════════════════════════════
# SELF-REFERENCING (self-referencing SG)
# ═══════════════════════════════════════════════════════════

# Zezwól na cały ruch między instancjami z tym samym SG:
aws ec2 authorize-security-group-ingress \
  --group-id sg-0cluster111111111 \
  --ip-permissions '[{
    "IpProtocol": "-1",
    "UserIdGroupPairs": [{
      "GroupId": "sg-0cluster111111111",
      "Description": "Allow all traffic within cluster SG"
    }]
  }]'

# ═══════════════════════════════════════════════════════════
# PRZYPISYWANIE SG DO EC2 / ENI
# ═══════════════════════════════════════════════════════════

# Przypisz SG do ENI (zastępuje istniejące!):
aws ec2 modify-network-interface-attribute \
  --network-interface-id eni-12345678 \
  --groups sg-0abc123def456789 sg-0def456789abc123

# Dodaj SG do istniejącej EC2 (przez modify-instance-attribute):
# Uwaga: to ZASTĘPUJE wszystkie SG! Podaj kompletną listę:
aws ec2 modify-instance-attribute \
  --instance-id i-12345678 \
  --groups sg-0abc123def456789 sg-0existing111111

# ═══════════════════════════════════════════════════════════
# SPRAWDZANIE / USUWANIE
# ═══════════════════════════════════════════════════════════

# Opisz SG:
aws ec2 describe-security-groups \
  --group-ids sg-0abc123def456789

# Usuń regułę inbound:
aws ec2 revoke-security-group-ingress \
  --group-id sg-0abc123def456789 \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0

# Usuń SG (musi być odłączony od wszystkich ENI):
aws ec2 delete-security-group \
  --group-id sg-0abc123def456789
```

---

### 17.14 SG i VPC Peering / Transit Gateway

Cross-VPC: SG ID jako source **nie działa** między różnymi VPC (nawet przez peering).

```
VPC-A (10.0.0.0/16)          VPC-B (10.1.0.0/16)
  EC2-A [sg-a]     ←── Peering ───►  EC2-B [sg-b]

SG-B inbound:
  ❌ port 443 from sg-a      ← NIE DZIAŁA dla cross-VPC peering
  ✅ port 443 from 10.0.0.0/16  ← Działa (CIDR VPC-A)
  ✅ port 443 from 10.0.1.5/32  ← Działa (konkretny IP)

Wyjątek: VPC Peering między VPC w TYM SAMYM koncie i tym samym regionie:
  → Można używać SG ID jako source (cross-VPC SG reference)
  aws ec2 authorize-security-group-ingress \
    --group-id sg-0inVPCB \
    --ip-permissions '[{
      "IpProtocol": "tcp",
      "FromPort": 443,
      "ToPort": 443,
      "UserIdGroupPairs": [{
        "UserId": "123456789012",
        "GroupId": "sg-0inVPCA"    ← SG z innego VPC (same account, same region)
      }]
    }]'

Transit Gateway:
  Ruch przez TGW: SG jako source NIE działa (zawsze CIDR)
```

---

### 17.15 SG i Services — ELB, RDS, Lambda, ECS

### ALB / NLB Security Groups

```
ALB ma własny SG!

ALB SG (sg-alb):
  Inbound: 443 from 0.0.0.0/0, 80 from 0.0.0.0/0

EC2 Target SG (sg-ec2):
  Inbound: 8080 from sg-alb   ← tylko ALB może dotrzeć do EC2

Ale: NLB nie ma SG!
  NLB to L4 — nie ma własnego ENI w tradycyjnym sensie
  Ruch przez NLB: source IP = CLIENT IP (nie NLB IP)
  EC2 za NLB musi mieć regułę dla client IP range (lub 0.0.0.0/0)
  
  Wyjątek: NLB z Security Group (nowa funkcja 2023+):
    NLB dostał własne SG → możliwe sg-nlb jako source w ec2 SG
```

### RDS Security Groups

```
RDS instance ma własne ENI → własny SG

SG-RDS:
  Inbound: 5432 from sg-webapp (EC2 App)
  Inbound: 5432 from sg-bastion (jump host)
  Outbound: (domyślnie ALL — możesz zostawić lub ograniczyć)

Multi-AZ RDS: oba ENI (primary i standby) mają ten sam SG
Read Replica: osobne ENI, może mieć inny SG (ale często ten sam)
```

### Lambda Security Groups

```
Lambda w VPC:
  Lambda dostaje ENI (VPC-connected Lambda)
  ENI ma SG jak każde inne

SG-Lambda:
  Outbound: 5432 to sg-rds    (Lambda → RDS)
  Outbound: 443 to 0.0.0.0/0  (Lambda → AWS API, S3 etc.)

SG-RDS:
  Inbound: 5432 from sg-lambda  ← zezwól Lambda

Uwaga: Lambda spoza VPC (domyślna) → nie ma SG → używa NAT/IGW przez AWS managed infra
```

### ECS Fargate Security Groups

```
Fargate task: każdy task dostaje własne ENI (awsvpc network mode)
  → Każdy task = osobny SG assignment
  → Możliwe granularne kontrole per-task

ECS EC2 launch type:
  Instancje EC2 mają SG
  Tasks współdzielą ENI instancji → SG instancji
  Dynamiczne port mapping → SG musi zezwolić na zakres portów
  lub użyj sg-alb jako source dla całego zakresu

SG-ECS-Fargate:
  Inbound: 8080 from sg-alb
  Outbound: 5432 to sg-rds
           443 to 0.0.0.0/0 (AWS API)
```

---

### 17.16 Troubleshooting SG — Najczęstsze Problemy

```
Problem 1: Połączenie odmówione mimo "poprawnych" reguł SG

Checklist:
  □ Czy SG jest przypisany do WŁAŚCIWEGO ENI?
    aws ec2 describe-network-interfaces --filters Name=group-id,Values=sg-xxx
  □ Czy sprawdziłeś OUTBOUND rules na EC2 INICJUJĄCEJ połączenie?
  □ Czy NACL subnetu nie blokuje? (stateless! sprawdź oba kierunki)
  □ Czy routing table ma trasę? (SG nie pomoże bez routingu)
  □ Czy aplikacja nasłuchuje na właściwym porcie/interfejsie?
    ss -tlnp lub netstat -tlnp na EC2

Problem 2: Działa z jednego IP ale nie z drugiego

  □ Sprawdź Source w regule — CIDR za wąskie?
  □ Czy second EC2 ma inny SG? Reguła dotyczy konkretnego sg-id?

Problem 3: EC2 nie może połączyć się z Internetem (outbound)

  □ Outbound SG: czy jest reguła dla 443/0.0.0.0/0?
  □ IGW przypisany do VPC?
  □ Route Table: default route → IGW lub NAT GW?
  □ Public IP / EIP przypisany?
  □ NACL: outbound i ephemeral ports (1024-65535)?

Problem 4: Security Group changes nie działają natychmiast

  → Nieprawda! SG changes są NATYCHMIASTOWE (bez restartu instancji)
  → Sprawdź czy zmieniony właściwy SG (może jest kilka o podobnych nazwach)
```

### VPC Reachability Analyzer — automatyczna diagnostyka

```bash
# Utwórz analizę ścieżki sieciowej:
aws ec2 create-network-insights-path \
  --source eni-0abc123 \
  --destination eni-0def456 \
  --protocol tcp \
  --destination-port 443

# Uruchom analizę:
aws ec2 start-network-insights-analysis \
  --network-insights-path-id nip-12345678

# Sprawdź wyniki:
aws ec2 describe-network-insights-analyses \
  --network-insights-analysis-ids nia-12345678

# Output wskaże dokładnie który SG / NACL / Route blokuje
```

---

### 17.17 Limity Security Groups

```
┌──────────────────────────────────────────────────────────────────┐
│ Limit                              │ Wartość domyślna            │
├──────────────────────────────────────────────────────────────────┤
│ SG per VPC                         │ 2,500                       │
│ SG per ENI                         │ 5 (max 16 po zwiększeniu)   │
│ Reguły inbound per SG              │ 60 (max 1,000 po zwiększeniu│
│ Reguły outbound per SG             │ 60 (max 1,000 po zwiększeniu│
│ SG w grupie (group references)     │ 5 per reguła               │
│ Reguły × SG per ENI (combined)    │ 300 (5 SG × 60 reguł)      │
└──────────────────────────────────────────────────────────────────┘

Uwaga: więcej reguł = więcej zasobów hypervisora = potencjalnie
wyższe latency przy inicjowaniu połączeń (minimalne, ale istnieje).
```

---

### 17.18 Well-Architected Framework — Security Groups

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Taguj SG czytelnie (Name, Environment, Owner), używaj AWS Config rules (np. `restricted-ssh` sprawdza czy port 22 nie otwarty na 0.0.0.0/0), automatyzuj przez CloudFormation/Terraform, audytuj przez Security Hub |
| **Security** | Zasada najmniejszego przywileju (tylko wymagane porty i źródła), SG chaining (ALB→App→DB), self-referencing dla klastrów, ogranicz outbound dla wrażliwych workloadów, usuń regułę `0.0.0.0/0` dla SSH/RDP, użyj SSM Session Manager zamiast bastiona |
| **Reliability** | SG changes są natychmiastowe i nie wymagają restartu. SG jako "żywa" konfiguracja — zmiana SG może naprawić lub zepsuć produkcję w sekundę. Testuj zmiany SG na staging przed prod |
| **Performance Efficiency** | Mniej reguł = szybsza ewaluacja. Konsoliduj reguły. Unikaj overlapping CIDR ranges. VPC Reachability Analyzer do debugowania bez wpływu na ruch |
| **Cost Optimization** | SG są bezpłatne — nie ma opłat per SG ani per reguła. Koszty pośrednie: złe SG → debugging time → engineer hours |
| **Sustainability** | Regularnie czyść nieużywane SG i reguły (AWS Config, Trusted Advisor), mniej overhead hypervisora |

---

### 17.19 4 Domeny Egzaminacyjne SAP-C02

| Domena | Zastosowanie SG |
|---|---|
| **Design for New Solutions** | Projektuj SG per warstwa (web/app/db/mgmt). SG chaining. Self-referencing dla klastrów. Decyzja: kiedy SG vs NACL vs WAF |
| **Organizational Complexity** | Cross-account SG reference przez Resource Access Manager (RAM). SCP nie może zarządzać SG — to IAM. SG per workload team, centralne security team audytuje |
| **Continuous Improvement** | AWS Config rules dla compliance SG. Security Hub findings dla SG misconfigurations. GuardDuty wykrywa anomalie w ruchu sieciowym |
| **Migration & Modernization** | Lift-and-shift: mapuj on-prem firewall rules na SG. Monitoruj traffic patterns przed migracją (VPC Flow Logs) żeby zbudować właściwe SG |

---

### 17.20 VPC Flow Logs — Debug SG przez Logi

```bash
# Włącz Flow Logs dla ENI (debug SG):
aws ec2 create-flow-logs \
  --resource-type NetworkInterface \
  --resource-ids eni-12345678 \
  --traffic-type ALL \
  --log-destination-type cloud-watch-logs \
  --log-destination arn:aws:logs:eu-west-1:123456789:log-group:/vpc/flow-logs

# Format logu (domyślny):
# version account-id interface-id srcaddr dstaddr srcport dstport protocol packets bytes start end action log-status
# 2 123456789 eni-abc 10.0.1.5 10.0.2.10 54321 3306 6 10 1200 ... REJECT OK
#                                                                    ^^^^^^
#                                                            REJECT = SG lub NACL zablokowało

# Szukaj REJECT dla konkretnego IP:
# CloudWatch Logs Insights:
# fields @timestamp, srcAddr, dstAddr, srcPort, dstPort, action
# | filter action = "REJECT" and dstAddr = "10.0.2.10"
# | sort @timestamp desc
# | limit 50
```

---

## 18. Launch Templates, AMI, Instance Types, Placement Groups — Deep Dive

> **Zakres:** Launch Templates (vs Launch Configurations), AMI (tworzenie, katalog, sharing, szyfrowanie), typy instancji EC2 (szczegółowe), Placement Groups (Cluster/Spread/Partition). Fundamenty uruchamiania EC2 workloadów.

---

### 18.1 Launch Template — Czym Jest i Po Co

**Launch Template (LT)** to szablon konfiguracji instancji EC2 — zbiera wszystkie parametry uruchamiania w jednym miejscu z obsługą wersjonowania.

```
Launch Template zawiera (opcjonalne — możesz pominąć co chcesz):
┌────────────────────────────────────────────────────────────────┐
│  AMI ID               instance-id lub AMI ARN                 │
│  Instance Type        t3.medium, m6g.large...                 │
│  Key Pair             dla SSH                                  │
│  Security Groups      sg-xxx, sg-yyy                          │
│  Subnet               lub pozostaw dla ASG                    │
│  IAM Instance Profile arn:aws:iam::xxx:instance-profile/MyIP  │
│  User Data            bash script (cloud-init)                │
│  Storage (EBS)        typ, rozmiar, encrypted, delete-on-term │
│  Network Interface    ENI settings, public IP assignment      │
│  Tags                 Name, Env, Team...                      │
│  Placement Group      cluster/spread/partition                │
│  Tenancy              default/dedicated/host                  │
│  Shutdown Behavior    stop/terminate                          │
│  Hibernation          enable/disable                          │
│  Monitoring           detailed monitoring on/off              │
│  Metadata Options     IMDSv2 required, hop limit              │
│  License Config       BYOL tracking                          │
└────────────────────────────────────────────────────────────────┘
```

### Launch Template vs Launch Configuration

```
┌───────────────────────┬────────────────────────┬────────────────────┐
│ Cecha                 │ Launch Template (LT)   │ Launch Config (LC) │
├───────────────────────┼────────────────────────┼────────────────────┤
│ Status                │ ✅ Aktywny, zalecany    │ ❌ Legacy, deprecated│
│ Wersjonowanie         │ ✅ Tak (v1, v2, $Latest)│ ❌ Nie             │
│ Spot + On-Demand mix  │ ✅ Mixed Instances      │ ❌ Nie             │
│ T2/T3 Unlimited       │ ✅ Tak                  │ ❌ Ograniczone     │
│ Elastic GPU           │ ✅ Tak                  │ ❌ Nie             │
│ Capacity Reservation  │ ✅ Tak                  │ ❌ Nie             │
│ IMDSv2 config         │ ✅ Tak                  │ ✅ Tak             │
│ Partial parameters    │ ✅ Tak (merge z bazą)   │ ❌ Wszystko lub nic│
│ Dedykowane Hosty      │ ✅ Tak                  │ ❌ Nie             │
│ ASG support           │ ✅ Tak                  │ ✅ Tak (legacy)    │
│ EC2 Fleet             │ ✅ Tak                  │ ❌ Nie             │
└───────────────────────┴────────────────────────┴────────────────────┘

Migracja LC → LT:
aws ec2 create-launch-template-from-launch-config \  (nie istnieje bezpośrednio)
Ręcznie: pobierz LC parametry, utwórz LT z tymi samymi wartościami
```

### Wersjonowanie Launch Template

```bash
# Utwórz LT:
aws ec2 create-launch-template \
  --launch-template-name "web-server-lt" \
  --version-description "Initial version" \
  --launch-template-data '{
    "ImageId": "ami-0abcdef1234567890",
    "InstanceType": "t3.medium",
    "SecurityGroupIds": ["sg-0abc123"],
    "UserData": "'"$(base64 -w0 user-data.sh)"'",
    "TagSpecifications": [{
      "ResourceType": "instance",
      "Tags": [{"Key": "Name", "Value": "WebServer"}]
    }],
    "MetadataOptions": {
      "HttpTokens": "required",
      "HttpPutResponseHopLimit": 1
    }
  }'

# Output: LaunchTemplateId: lt-0abc123, Version: 1

# Utwórz nową wersję (zmień tylko AMI):
aws ec2 create-launch-template-version \
  --launch-template-id lt-0abc123 \
  --source-version 1 \
  --version-description "Updated AMI" \
  --launch-template-data '{"ImageId": "ami-0newami1234567890"}'
# → Version: 2 (dziedziczy wszystko z v1, zmienia tylko AMI)

# Ustaw default version:
aws ec2 modify-launch-template \
  --launch-template-id lt-0abc123 \
  --default-version 2

# Wersje specjalne:
# $Latest  → zawsze najnowsza wersja
# $Default → ustawiona domyślna wersja
# 1, 2, 3  → konkretna wersja

# Sprawdź wersje:
aws ec2 describe-launch-template-versions \
  --launch-template-id lt-0abc123
```

### User Data — Cloud-Init

```bash
# User Data = skrypt wykonywany PRZY PIERWSZYM URUCHOMIENIU instancji
# Base64 encoded w Launch Template

#!/bin/bash
# /user-data.sh

# Zaktualizuj system:
yum update -y

# Zainstaluj aplikację:
yum install -y nginx

# Konfiguruj z S3:
aws s3 cp s3://my-config-bucket/nginx.conf /etc/nginx/nginx.conf

# Uruchom serwis:
systemctl enable nginx
systemctl start nginx

# Wyślij sygnał do cfn-signal (dla CloudFormation):
/opt/aws/bin/cfn-signal -e $? --stack MyStack --region eu-west-1

# Logi User Data:
# /var/log/cloud-init.log
# /var/log/cloud-init-output.log

# Sprawdź z działającej instancji (IMDSv2):
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/user-data
```

---

### 18.2 AMI — Amazon Machine Image

**AMI** to template do uruchamiania EC2 — zawiera OS, skonfigurowane oprogramowanie, dane i polityki startowe.

### Anatomia AMI

```
AMI = kilka komponentów:
┌──────────────────────────────────────────────────────────────────┐
│  AMI Metadata:                                                   │
│    AMI ID:          ami-0abcdef1234567890                        │
│    Name:            "MyApp-v1.2.3-2026-10-27"                   │
│    Architecture:    x86_64 / arm64                              │
│    Virtualization:  hvm (Hardware Virtual Machine — standard)    │
│                     pv (Paravirtual — legacy, nie używaj)        │
│    Root Device:     ebs (standard) lub instance-store (legacy)  │
│    State:           available / pending / failed                 │
│    Ownership:       self / amazon / marketplace / others         │
│                                                                  │
│  Snapshots EBS (per volume):                                     │
│    /dev/xvda → snap-0abc123 (root volume, 20GB gp3)             │
│    /dev/xvdb → snap-0def456 (data volume, 100GB gp3)            │
│                                                                  │
│  Block Device Mapping:                                           │
│    Mapuje snapshoty na urządzenia przy starcie instancji         │
└──────────────────────────────────────────────────────────────────┘
```

### Tworzenie AMI

```bash
# Metoda 1: Z działającej instancji (najczęstsza):
aws ec2 create-image \
  --instance-id i-0abc123 \
  --name "MyApp-v1.2.3-$(date +%Y%m%d)" \
  --description "Production AMI for MyApp" \
  --no-reboot   # ← bez restartu instancji (filesystem może być niespójny)
                # bez --no-reboot: instancja jest zatrzymana → AMI → restart

# Output: ImageId: ami-0newami1234567890

# Metoda 2: Ze snapshotu EBS (dla przypadku root volume):
aws ec2 register-image \
  --name "MyApp-from-snapshot" \
  --root-device-name "/dev/xvda" \
  --block-device-mappings '[{
    "DeviceName": "/dev/xvda",
    "Ebs": {
      "SnapshotId": "snap-0abc123",
      "VolumeType": "gp3",
      "DeleteOnTermination": true
    }
  }]' \
  --virtualization-type hvm \
  --architecture x86_64

# Metoda 3: EC2 Image Builder (automatyczne AMI pipeline):
# Managed service — definiujesz recipes, components, pipelines
# Buduje, testuje i dystrybuuje AMI automatycznie
```

### AMI — Gdzie i Kiedy

```
AMI jest regionalna! ami-xxx w eu-west-1 NIE DZIAŁA w us-east-1.

Kopiowanie AMI między regionami:
aws ec2 copy-image \
  --source-region eu-west-1 \
  --source-image-id ami-0abc123 \
  --region us-east-1 \
  --name "MyApp-v1.2.3-us-east-1" \
  --encrypted                    # ← opcjonalnie zaszyfruj w docelowym regionie
  --kms-key-id alias/my-key      # ← opcjonalnie własny KMS key

Kopiowanie tworzy nowe snapshoty w docelowym regionie.
Koszt: storage nowych snapshotów + transfer (nieduże).

AMI w Multi-Region DR:
  Każdy region ma własną kopię AMI
  Automation: EventBridge + Lambda po każdym nowym AMI build → copy-image do DR regionu
```

### AMI Sharing — Właściciele i Dostęp

```
AMI owner: konto które stworzyło AMI

Sharing AMI z innymi kontami:
aws ec2 modify-image-attribute \
  --image-id ami-0abc123 \
  --launch-permission '{"Add": [{"UserId": "123456789012"}]}'

Sharing z całą AWS Organization:
aws ec2 modify-image-attribute \
  --image-id ami-0abc123 \
  --launch-permission '{"Add": [{"OrganizationArn": "arn:aws:organizations::xxx:organization/o-xxx"}]}'

Public AMI:
aws ec2 modify-image-attribute \
  --image-id ami-0abc123 \
  --launch-permission '{"Add": [{"Group": "all"}]}'  # ← ostrożnie!

Zasady dostępu:
  Owner konta: może używać AMI do uruchamiania instancji
  Shared konto: może używać do uruchamiania, NIE może modyfikować
  Shared konto: może zrobić kopię (copy-image) i wtedy modyfikować swoją kopię
```

### AMI Encryption

```
AMI z EBS (standard):
  Snapshoty w AMI mogą być zaszyfrowane lub nie
  Instancja uruchomiona z niezaszyfrowanego AMI: root volume NIEzaszyfrowany
  (chyba że w Launch Template ustawisz encrypted=true → tworzy zaszyfrowaną kopię)

Wymuś szyfrowanie w całym koncie (Account-level default encryption):
  aws ec2 enable-ebs-encryption-by-default
  → wszystkie nowe woluminy, w tym z AMI, będą zaszyfrowane

Kopiowanie AMI z szyfrowaniem:
  aws ec2 copy-image \
    --source-image-id ami-unencrypted \
    --encrypted \
    --kms-key-id arn:aws:kms:us-east-1:xxx:key/yyy
  → Tworzy nowe zaszyfrowane snapshoty

Sharing zaszyfrowanych AMI:
  AMI z CMK-encrypted snapshot → musisz udostępnić też klucz KMS!
  Recipient musi mieć dostęp do klucza KMS przez Key Policy
```

---

### 18.3 AMI Catalog — Publiczne i Marketplace AMI

```
Typy AMI w AWS Console / CLI:

1. AWS AMI (właściciel: amazon):
   Amazon Linux 2023 (AL2023)
   Amazon Linux 2 (AL2)
   Ubuntu 22.04/24.04
   RHEL 8/9
   Windows Server 2019/2022
   macOS (tylko dla Mac instances)
   
   Pobierz najnowszy AMI ID (zmienia się z każdym patchem!):
   aws ec2 describe-images \
     --owners amazon \
     --filters \
       "Name=name,Values=al2023-ami-*" \
       "Name=architecture,Values=x86_64" \
       "Name=state,Values=available" \
     --query 'sort_by(Images, &CreationDate)[-1].ImageId' \
     --output text
   # → ami-0latest1234567890

2. AWS Marketplace AMI:
   Komercyjne obrazy (Cisco, Palo Alto, Fortinet, Zscaler)
   BYOL lub license included
   Dodatkowe opłaty za licencję (na godzinę lub roczna)
   Wymaga akceptacji warunków przed uruchomieniem

3. Community AMI:
   Publiczne AMI tworzone przez użytkowników
   Ostrożnie: brak weryfikacji bezpieczeństwa
   NIGDY nie uruchamiaj w prod bez audytu!

4. Private AMI (twoje własne):
   AMI z twojego konta
   Shared AMI z innych kont (wymagają explicit sharing)

SSM Parameter Store — canonical AMI IDs:
  aws ssm get-parameter \
    --name /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 \
    --query 'Parameter.Value' \
    --output text
  # Zawsze aktualny AMI ID bez szukania przez describe-images
```

### EC2 Image Builder — Automatyczne AMI Pipeline

```
EC2 Image Builder = managed service do automatycznego budowania AMI

Komponenty:
  Recipe        = definicja AMI (base AMI + components)
  Components    = scripts/commands do konfiguracji (AWS lub własne)
  Pipeline      = schedule + recipe + distribution
  Infrastructure= EC2 instance używana do budowania (tymczasowa)
  Distribution  = które regiony i konta dostają AMI
  
Workflow:
  1. Uruchom EC2 build instance (z base AMI)
  2. Zainstaluj komponenty (yum update, install nginx, harden OS...)
  3. Utwórz AMI ze zbudowanej instancji
  4. Uruchom testy (EC2 Test Instance z nowym AMI)
  5. Dystrybuuj do regionów/kont (copy-image)
  6. Terminated build/test instances
  
STIG Hardened AMI:
  AWS dostarcza STIG-compliant AMI przez Image Builder
  Defence in Depth: OS hardened per DISA STIG
```

---

### 18.4 Typy Instancji EC2 — Szczegółowy Przegląd

Budowa nazwy instancji (rozwinięcie z Rozdziału 10):

```
  c  7  g  n  . x l a r g e
  │  │  │  │    │ │
  │  │  │  │    │ └─ Rozmiar: nano, micro, small, medium, large,
  │  │  │  │    │             xlarge, 2xlarge, ... 112xlarge
  │  │  │  │    └─── Dodatkowe atrybuty (patrz niżej)
  │  │  │  └──────── n = Enhanced Networking (100 Gbps ENA Express)
  │  │  └─────────── g = Graviton (AWS ARM chip)
  │  └────────────── 7 = Generacja
  └───────────────── c = Rodzina (Compute optimized)
```

### Wszystkie Sufiksy Instancji

```
┌──────┬──────────────────────────────────────────────────────────┐
│ Suf. │ Znaczenie                                                │
├──────┼──────────────────────────────────────────────────────────┤
│  a   │ AMD EPYC processor (tańsze ~10%)                         │
│  b   │ Block storage optimized (więcej EBS throughput)          │
│  d   │ Local NVMe SSD (instance store) dołączony                │
│  e   │ Extra storage lub memory (extended)                      │
│  g   │ AWS Graviton (ARM64 — własny chip AWS)                   │
│  i   │ Intel processor (czasem explicit)                        │
│  n   │ Network optimized (wyższy bandwidth, ENA Express)        │
│  p   │ Processor feature (AES, AVX-512...)                      │
│  q   │ Qualcomm (tylko niektóre)                                │
│  s   │ Shadow — mniejszy rozmiar (1/4 next size)               │
│  z   │ High frequency CPU (Intel Xeon Scalable 4.0+ GHz)       │
│  flex│ Flexible (EC2 Flex Instances — nowe)                     │
└──────┴──────────────────────────────────────────────────────────┘
```

### Rodziny Instancji — Pełna Tabela

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Rodzina     │ Typy          │ vCPU:RAM ratio │ Use Case                     │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ T (Burst)   │ t2,t3,t3a,    │ Kredyty CPU    │ Dev/test, web, light prod    │
│             │ t4g           │ zmiennie       │ Tani; spike handling         │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ M (General) │ m5,m5a,m5d,   │ 1:4 (vCPU:GiB)│ Standardowy workload         │
│             │ m6g,m6i,m7g,  │                │ Aplikacje, mid-size DB       │
│             │ m7i,m8g       │                │ Default gdy wątpisz          │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ C (Compute) │ c5,c5n,c5d,   │ 1:2            │ CPU-bound: HPC, batch,       │
│             │ c6g,c6i,c7g,  │                │ gaming, encoding, ML infer.  │
│             │ c7i,c8g       │                │                              │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ R (Memory)  │ r5,r5b,r5d,   │ 1:8 lub 1:16  │ In-memory DB (Redis, Memcache│
│             │ r6g,r6i,r7g,  │                │ SAP HANA, big data analytics │
│             │ r7i,r8g       │                │                              │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ X (Extreme) │ x1e,x2gd,     │ 1:32+          │ SAP HANA, Oracle in-memory  │
│ Memory)     │ x2idn,x2iedn  │                │ TB-scale RAM                 │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ U (Ultra    │ u-6tb1, u-9tb1│ 1:128 (6-24 TB)│ Largest in-memory DB         │
│ High Mem)   │ u-12tb1       │                │ SAP HANA scale-up only       │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ I (Storage) │ i3,i3en,i4g,  │ High NVMe IOPS │ NoSQL, Elasticsearch, OLTP  │
│             │ i4i,is4gen    │                │ High random I/O              │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ D (Dense)   │ d2,d3,d3en    │ HDD sequential │ Hadoop, DWH, data lake (raw) │
│ Storage)    │               │                │ Cheap sequential throughput  │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ H (High     │ h1             │ HDD + storage  │ MapReduce, HDFS, S3-like     │
│ Disk)       │               │                │ workloads on-instance         │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ P (GPU)     │ p3,p4d,p4de,  │ GPU: Nvidia V100│ ML training, HPC, rendering  │
│ Parallel)   │ p5            │ A100, H100     │ Deep learning                │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ G (GPU      │ g4dn,g4ad,    │ GPU: T4, V520, │ ML inference, GPU gaming,    │
│ Graphics)   │ g5,g5g,g6     │ A10G           │ Remote desktop, video encode │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ Trn         │ trn1,trn2     │ AWS Trainium   │ ML training (AWS chip)       │
│ (Training)  │               │                │ Cheaper than P5 for training │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ Inf         │ inf1,inf2     │ AWS Inferentia │ ML inference (AWS chip)      │
│ (Inference) │               │                │ 40x throughput vs GPU        │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ VT          │ vt1           │ FPGA Video     │ Live video transcoding        │
│ (Video)     │               │                │                              │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ F (FPGA)    │ f1            │ Xilinx FPGA    │ Custom hardware acceleration  │
│             │               │                │ Genomics, financial           │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ HPC         │ hpc6a,hpc7g,  │ EFA 100 Gbps+  │ Tightly coupled HPC clusters │
│             │ hpc7a         │                │ MPI workloads                │
├─────────────┼───────────────┼────────────────┼──────────────────────────────┤
│ Mac         │ mac1,mac2,    │ macOS only      │ iOS/macOS app development    │
│             │ mac2-m2pro    │                │ Xcode builds, TestFlight     │
└─────────────┴───────────────┴────────────────┴──────────────────────────────┘
```

### Graviton — AWS ARM Chip Szczegóły

```
Graviton = AWS własny chip ARM64:
  Graviton 2 (M6g, C6g, R6g...): do 40% lepszy performance/$ vs x86
  Graviton 3 (M7g, C7g, R7g...): 25% szybszy niż Graviton 2
  Graviton 4 (M8g, C8g, R8g...): najnowszy, 30% szybszy niż Graviton 3
  
Wymagania dla Graviton:
  Software skompilowany dla ARM64 (aarch64)
  Większość managed services obsługuje automatycznie:
    Amazon Linux 2023: natywnie ARM64
    Ubuntu 22.04+: ARM64 wersja
    Docker images: --platform linux/arm64
    Java (Corretto): ARM64 JVM
    Python/Node/Go: multiarch
  
  Nie obsługuje Graviton:
    32-bit aplikacje
    Windows (brak ARM Windows w EC2)
    Niektóre legacy x86 binaries
  
Koszt porównanie (m7g.xlarge vs m7i.xlarge):
  m7g.xlarge (Graviton3): $0.1632/h
  m7i.xlarge (Intel):     $0.2016/h
  → Graviton ~19% tańszy + często szybszy
```

---

### 18.5 Placement Groups — Rozmieszczenie Instancji

**Placement Group** kontroluje jak EC2 instancje są rozmieszczone na fizycznym sprzęcie AWS.

### Cluster Placement Group

```
Cel: Minimalna latency, maksymalny throughput (HPC, MPI, ML)

Wszystkie instancje na jednym "rack" (lub kilka sąsiadujących rack):
  
  ┌─────────────────────────────────────────────────┐
  │           JEDEN RACK / ZONA SIECIOWA            │
  │                                                 │
  │  ┌─────┐  ┌─────┐  ┌─────┐  ┌─────┐           │
  │  │ EC2 │  │ EC2 │  │ EC2 │  │ EC2 │           │
  │  │ (1) │  │ (2) │  │ (3) │  │ (4) │  ...      │
  │  └──┬──┘  └──┬──┘  └──┬──┘  └──┬──┘           │
  │     └────────┴────────┴────────┘               │
  │          Enhanced Networking (EFA/ENA)          │
  │          10 / 25 / 100 Gbps                    │
  │          Latency: ~10-50 microseconds           │
  └─────────────────────────────────────────────────┘
  
  + Niskie latency (fizyczna bliskość)
  + Enhanced Networking: 10-100 Gbps między instancjami
  + Wymaga: Enhanced Networking (ENA) lub EFA
  - Jeden punkt awarii (wszystkie w jednym AZ!)
  - Jeśli rack padnie → cały klaster offline
  - Capacity risk: może brakować miejsca na rack dla dużego klastra
  
Tworzenie:
  aws ec2 create-placement-group \
    --group-name "hpc-cluster-1" \
    --strategy cluster
    
Use cases: Hadoop, MPI jobs, ML training (multi-node), Kafka

EFA (Elastic Fabric Adapter) — dla Cluster PG:
  Low-latency, high-bandwidth interconnect
  Omija OS network stack (RDMA-like)
  Wymagane dla: hpc6a, hpc7g, p4d, trn1
```

### Spread Placement Group

```
Cel: Maksymalna redundancja (krytyczne aplikacje)

Każda instancja na INNYM fizycznym rack (każdy rack = osobny zasilacz, sieć):

  AZ us-east-1a:
  ┌──────────┐  ┌──────────┐  ┌──────────┐
  │ Rack #1  │  │ Rack #2  │  │ Rack #3  │
  │ ┌──────┐ │  │ ┌──────┐ │  │ ┌──────┐ │
  │ │ EC2  │ │  │ │ EC2  │ │  │ │ EC2  │ │
  │ │ (1)  │ │  │ │ (2)  │ │  │ │ (3)  │ │
  │ └──────┘ │  │ └──────┘ │  │ └──────┘ │
  └──────────┘  └──────────┘  └──────────┘
  
  + Rack awaria = tylko 1 instancja offline
  + Różne AZ możliwe (spread działa cross-AZ)
  - LIMIT: 7 instancji per AZ per placement group!
  - Nie nadaje się dla dużych klastrów
  
Tworzenie:
  aws ec2 create-placement-group \
    --group-name "critical-app-spread" \
    --strategy spread

Use cases:
  Zookeeper (3 lub 5 nodes) — nie może paść > 1 naraz
  Primary/Secondary DB pair
  Aplikacje wymagające izolacji hardware
  Max 7 instancji → wtedy Partition PG zamiast
```

### Partition Placement Group

```
Cel: Duże klastry z izolacją "grup awarii" (failure domains)

Instancje dzielone na N partycji, każda na osobnych rack:

  ┌─────────────────────────────────────────────────────┐
  │              AZ us-east-1a                          │
  │                                                     │
  │  Partition 1    Partition 2    Partition 3           │
  │  ┌──────────┐  ┌──────────┐  ┌──────────┐          │
  │  │ Rack A   │  │ Rack B   │  │ Rack C   │          │
  │  │ EC2 (1)  │  │ EC2 (5)  │  │ EC2 (9)  │          │
  │  │ EC2 (2)  │  │ EC2 (6)  │  │ EC2 (10) │          │
  │  │ EC2 (3)  │  │ EC2 (7)  │  │ EC2 (11) │          │
  │  │ EC2 (4)  │  │ EC2 (8)  │  │ EC2 (12) │          │
  │  └──────────┘  └──────────┘  └──────────┘          │
  │   (osobny zasilacz, sieć, switch per partycja)      │
  └─────────────────────────────────────────────────────┘
  
  + Do 7 partycji per AZ
  + Każda partycja nie współdzieli sprzętu z innymi
  + Aplikacja "wie" do której partycji należy instancja (metadata)
  + Setki instancji w grupie (w przeciwieństwie do Spread max 7/AZ)
  - Partycja awarii = wszystkie EC2 w partycji offline
  
Dostęp do numeru partycji (w instancji):
  curl http://169.254.169.254/latest/meta-data/placement/partition-number
  → 2  (ta instancja jest w partycji 2)

Tworzenie:
  aws ec2 create-placement-group \
    --group-name "kafka-partition-pg" \
    --strategy partition \
    --partition-count 3  # 3 partycje = 3 failure domains

Use cases:
  Apache Kafka (replikacja między partycjami)
  Apache Cassandra (rack-aware replication)
  HDFS (block replication across racks)
  Elasticsearch (shard replicas across partitions)
```

### Placement Groups — Porównanie

```
┌──────────────────┬───────────────────┬───────────────────┬───────────────────┐
│ Cecha            │ Cluster           │ Spread            │ Partition         │
├──────────────────┼───────────────────┼───────────────────┼───────────────────┤
│ Cel              │ Niska latency     │ Max izolacja      │ Duże klastry HA   │
│ Maks instancji   │ Brak limitu (rack)│ 7 per AZ         │ Setki (7 part/AZ) │
│ Failure domain   │ Cały cluster      │ Jedna instancja   │ Cała partycja     │
│ Multi-AZ         │ ❌ (jedna AZ)      │ ✅ (cross-AZ)     │ ✅ (per AZ)       │
│ Latency          │ ~microseconds      │ Normalna          │ Normalna          │
│ Networking       │ 10-100 Gbps        │ Standardowe       │ Standardowe       │
│ Typowy use case  │ HPC, ML training  │ 3-7 kryt. serwers.│ Kafka, Cassandra  │
│ Koszt            │ Brak extra        │ Brak extra        │ Brak extra        │
│ Instancje typy   │ Wymaga ENA/HVM    │ Dowolne HVM       │ Dowolne HVM       │
└──────────────────┴───────────────────┴───────────────────┴───────────────────┘

Dodawanie instancji do Placement Group:
  W Launch Template: --placement '{"GroupName": "my-cluster-pg"}'
  lub przy uruchamianiu:
  aws ec2 run-instances --placement '{"GroupName": "my-cluster-pg"}'
  
  Uwaga: istniejąca instancja nie może być przeniesiona do PG
         (musi być zatrzymana i uruchomiona ponownie ze specyfikacją PG)
```

---

### 18.6 Well-Architected Framework — Launch Templates, AMI, PG

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Launch Template versioning = infrastruktura jako kod. AMI baking w Image Builder pipeline. Tagi AMI (wersja, build date, application). AMI lifecycle policy. |
| **Security** | IMDSv2 wymagane w LT (`HttpTokens: required`). Zaszyfrowane AMI (KMS). Minimalne prawa IAM Instance Profile. User Data bez sekretów (użyj SSM Parameter Store / Secrets Manager). |
| **Reliability** | AMI per region dla DR. Spread PG dla krytycznych aplikacji. Nie polegaj na jeden AMI — bake Application Layer, nie tylko OS. AMI golden image strategy. |
| **Performance Efficiency** | Graviton (arm64) dla kompatybilnych workloadów — 20-40% lepszy perf/$. Cluster PG dla HPC/ML. Właściwa rodzina instancji (C dla CPU, R dla RAM). Bake AMI z pre-installed dependencies = faster startup. |
| **Cost Optimization** | Graviton oszczędność 20-40%. Launch Template z Spot support. AMI deregistration po starzeniu (nie trzymaj 100 starych AMI — snapshoty kosztują). gp3 zamiast gp2 w AMI EBS. |
| **Sustainability** | Graviton = mniejszy ślad węglowy. Właściwy rozmiar instancji. Konsolidacja workloadów na większą instancję vs wiele małych. |

---

## 19. Modele Zakupu EC2 — Spot, Reserved Instances, Savings Plans, Dedicated, Capacity

> **Zakres:** Wszystkie modele zakupu EC2: On-Demand, Spot (Fleet, strategie, interruptions), Reserved Instances (Standard/Convertible), Savings Plans (Compute/EC2/SageMaker), Dedicated Hosts/Instances, Capacity Reservations, nowe UltraServers, Capacity Manager. Optymalizacja kosztów.

---

### 19.1 Przegląd Modeli Zakupu — Big Picture

```
┌────────────────────────────────────────────────────────────────────────┐
│                    MODELE ZAKUPU EC2                                   │
├──────────────────┬─────────────────┬──────────────────────────────────┤
│ Model            │ Discount vs OD  │ Use Case                         │
├──────────────────┼─────────────────┼──────────────────────────────────┤
│ On-Demand        │ 0% (baseline)   │ Unpredictable, short-term        │
│ Spot             │ 60-90%          │ Fault-tolerant, batch, ML train  │
│ Savings Plans    │ do 66%          │ Flexible steady-state compute    │
│ RI Standard      │ do 72%          │ Steady-state, znany type         │
│ RI Convertible   │ do 54%          │ Steady-state, zmienne typy       │
│ Dedicated Host   │ do 70% z BYOL   │ Compliance, BYOL licensing       │
│ Dedicated Inst.  │ do 70%          │ Hardware isolation (simpler)     │
│ Capacity Reserv. │ 0% (cena OD)    │ Gwarancja dostępności, DR        │
│ Capacity Block   │ Stała cena      │ GPU clusters, ML training slots  │
└──────────────────┴─────────────────┴──────────────────────────────────┘
```

---

### 19.2 On-Demand — Baseline

```
Charakterystyka:
  Bez zobowiązań, płatność per sekundę (min 60s)
  Najdroższy model
  Natychmiastowa dostępność (prawie zawsze)
  
Cena EC2 składa się z:
  Compute: rodzina × rozmiar × region × OS
  Data transfer: OUT z VPC do Internetu (pierwsze 100GB/mc darmowe)
  EBS: osobna opłata
  
Kiedy używać:
  - Testy i development
  - Nieprzewidywalne spiki
  - Nowe aplikacje (przed analizą wzorców zużycia)
  - Short burst workloads
```

---

### 19.3 Spot Instances — Deep Dive

### Czym Są Spot Instances

```
Spot = "nadwyżkowa" pojemność AWS do kupienia po obniżonej cenie
       (~60-90% taniej niż On-Demand)

Mechanizm:
  AWS ma pool niealokowanej pojemności w każdym AZ
  Spot price = rynkowa cena za tę pojemność (zmienia się co ~5min)
  Twoja "bid" = On-Demand price (domyślnie) — płacisz aktualny Spot price
  
  Gdy AWS potrzebuje pojemności:
    Daje 2-minutowe ostrzeżenie (Spot Instance Interruption Notice)
    Instancja jest "przerywana" (terminated lub stopped/hibernated)
    
Spot Interruption Notice:
  http://169.254.169.254/latest/meta-data/spot/interruption-action
  http://169.254.169.254/latest/meta-data/spot/termination-time
  
  Lub EventBridge: "EC2 Spot Instance Interruption Warning" event
  → Trigger Lambda → graceful shutdown, checkpoint, drain ELB
```

### Spot Fleet vs EC2 Fleet

```
Spot Fleet = stara API dla zarządzania pulą Spot
EC2 Fleet   = nowa, ujednolicona API dla Spot + OD

EC2 Fleet:
  Możesz mieszać Spot i OD
  Definiujesz InstanceRequirements lub konkretne typy
  Jeden request = wiele typów instancji i AZ
  
Spot Fleet Allocation Strategies:
┌─────────────────────────┬──────────────────────────────────────────┐
│ Strategia               │ Opis                                     │
├─────────────────────────┼──────────────────────────────────────────┤
│ capacity-optimized      │ AWS wybiera pool z największą dostępnością│
│                         │ Najniższe ryzyko przerwania              │
│ price-capacity-optimized│ Balans: niska cena + dobra dostępność    │
│ (REKOMENDOWANA)         │ AWS recommended dla większości           │
├─────────────────────────┼──────────────────────────────────────────┤
│ lowest-price            │ Najtańszy pool (wyższe ryzyko przerwania)│
│                         │ Stara strategia, avoid unless critical   │
├─────────────────────────┼──────────────────────────────────────────┤
│ diversified             │ Rozkłada po wszystkich poolach           │
│                         │ Dla Spot Fleet z ciągłą pojemnością      │
└─────────────────────────┴──────────────────────────────────────────┘
```

### Spot Request Types

```
One-Time Request:
  Jednorazowe żądanie Spot
  Gdy instancja przerwana → request zakończony
  
Persistent Request:
  Po przerwaniu: auto-restart gdy Spot dostępny
  Działa gdy instancja stopped (nie terminated)
  
  Cykl:
  Request → instancja → przerwanie → request open → instancja → ...

Hibernation (new):
  Spot instance hibernated zamiast terminated
  RAM zapisany do EBS root volume
  Po wznowieniu: aplikacja kontynuuje od miejsca przerwania
  Wymagania: musi być EBS-backed, RAM < EBS space, hibernation enabled
```

### Spot Blocks (deprecated 2021)

```
Spot Blocks pozwalały zarezerwować Spot na 1-6h bez przerwania.
AWS wycofał Spot Blocks — użyj Capacity Reservations + Spot Fleet
lub Capacity Block (dla GPU).
```

### Resilient Architecture dla Spot

```bash
# ASG z Mixed Instances — Spot + On-Demand:
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name "resilient-asg" \
  --mixed-instances-policy '{
    "LaunchTemplate": {
      "LaunchTemplateSpecification": {
        "LaunchTemplateId": "lt-0abc123",
        "Version": "$Latest"
      },
      "Overrides": [
        {"InstanceType": "c5.xlarge"},
        {"InstanceType": "c5a.xlarge"},
        {"InstanceType": "c6i.xlarge"},
        {"InstanceType": "c6a.xlarge"},
        {"InstanceType": "m5.xlarge"},   ← różne rodziny = mniejsze ryzyko
        {"InstanceType": "m6i.xlarge"}
      ]
    },
    "InstancesDistribution": {
      "OnDemandBaseCapacity": 2,              ← 2 OD zawsze
      "OnDemandPercentageAboveBaseCapacity": 20, ← 20% ponad baseline = OD
      "SpotAllocationStrategy": "price-capacity-optimized",
      "SpotInstancePools": 2
    }
  }' \
  --min-size 2 \
  --desired-capacity 10 \
  --max-size 20 \
  --vpc-zone-identifier "subnet-a,subnet-b,subnet-c"

# Graceful Spot termination (Lambda triggered by EventBridge):
# 1. Drain ELB target (deregister)
# 2. Checkpoint state do S3/DynamoDB
# 3. Finish current task (masz 2 min)
```

---

### 19.4 Reserved Instances (RI) — Deep Dive

### Typy RI

```
Standard RI:
  Commit: 1 lub 3 lata
  Discount: do 72% vs OD
  Flexibility: NISKA — zmiana tylko w ramach rodziny (zmiana rozmiaru w tej samej rodzinie)
  Przykład: m5.xlarge → m5.2xlarge (tylko rozmiar, ta sama rodzina/region/OS)
  Sprzedaż: RI Marketplace (możesz sprzedać nieużywane)
  
Convertible RI:
  Commit: 1 lub 3 lata
  Discount: do 54% vs OD
  Flexibility: WYSOKA — zmiana rodziny, rozmiaru, OS, tenancy
  Przykład: m5.xlarge → c6g.large → r5.4xlarge
  Sprzedaż: Nie można sprzedać (tylko exchange na inny Convertible RI)
  Exchange: `aws ec2 accept-reserved-instances-exchange-quote`
```

### RI Zakres (Scope)

```
Regional RI:
  Działa w całym regionie, dowolny AZ
  Automatycznie aplikuje się do pasujących instancji
  Benefit: AZ flexibility (instancja w eu-west-1a lub eu-west-1b)
  
Zonal RI:
  Konkretna AZ (eu-west-1a)
  Mniej elastyczne
  Zapewnia Capacity Reservation (gwarancja dostępności!)
  
  Regional NIE gwarantuje pojemności — tylko zniżkę
  Zonal = zniżka + gwarancja pojemności w tej AZ
```

### RI Payment Options

```
┌───────────────────┬──────────┬────────────┬────────────────────────┐
│ Opcja płatności   │ Upfront  │ Monthlyfee │ Total discount         │
├───────────────────┼──────────┼────────────┼────────────────────────┤
│ All Upfront       │ Cała kwota│ $0/mc     │ Najwyższy discount     │
│ Partial Upfront   │ ~50%     │ Reszta/12  │ Średni discount        │
│ No Upfront        │ $0       │ Pełna cena │ Najniższy discount     │
└───────────────────┴──────────┴────────────┴────────────────────────┘

Przykład (m5.xlarge, 3-year, Linux, us-east-1):
  OD:               $0.192/h = $1,681/rok
  RI 1yr No Upfront: $0.116/h = $1,016/rok (40% off)
  RI 3yr All Upfront: $1,648 total = $0.063/h (67% off)
  
Break-even: po 9-10 miesiącach All Upfront vs No Upfront
```

### Jak RI jest Aplikowane

```
RI benefit (zniżka) stosuje się automatycznie do pasujących instancji:
  
Running EC2: m5.xlarge, Linux, eu-west-1a
RI w portfelu: m5.xlarge, Linux, Regional (eu-west-1)
→ Automatycznie: RI benefit aplikowany, płacisz RI price

Running EC2: m5.2xlarge (nie pasuje)
RI w portfelu: m5.xlarge
→ Normalizacja: m5.2xlarge = 2× m5.xlarge
→ RI pokrywa 50% m5.2xlarge (1 RI unit = 1 xlarge = 2 large)

Normalization factors:
  nano=0.25, micro=0.5, small=1, medium=2, large=4, xlarge=8,
  2xlarge=16, 4xlarge=32, 8xlarge=64, 12xlarge=96, 16xlarge=128,
  24xlarge=192, metal=128
  
Konwersja rozmiaru w tej samej rodzinie:
  1 × m5.2xlarge (NF=16) = 2 × m5.xlarge (NF=8) = 8 × m5.small (NF=2)
```

### RI Marketplace

```
Sprzedaż nieużywanych Standard RI:
  Tylko Standard RI (nie Convertible)
  Tylko Linux RI (nie Windows/RHEL/SLES)
  Musi mieć > 30 dni do wygaśnięcia
  
aws ec2 describe-reserved-instances      ← sprawdź swoje RI
aws ec2 describe-reserved-instances-listings ← aukcje na Marketplace

Kupno na Marketplace: nowe RI z krótszym terminem (np. 6 miesięcy)
  Tańsze za jednostkę czasu niż nowe RI
```

---

### 19.5 Savings Plans — Elastyczna Alternatywa dla RI

### Typy Savings Plans

```
Compute Savings Plans:
  Commit: $/h (np. $10/h spend)
  Flexibility: NAJWYŻSZA
    - Dowolna rodzina instancji
    - Dowolny rozmiar
    - Dowolny AZ
    - Dowolny region
    - EC2, Fargate, Lambda (wszystko compute!)
  Discount: do 66%
  Zastępuje: Convertible RI dla elastycznych środowisk
  
EC2 Instance Savings Plans:
  Commit: $/h dla konkretnej rodziny w konkretnym regionie
  Flexibility: ŚREDNIA
    - Konkretna rodzina + region (np. m w eu-west-1)
    - Dowolny rozmiar (m5.large, m6g.xlarge, m7i.2xlarge...)
    - Dowolny AZ w tym regionie
    - Dowolny OS
    - Tylko EC2 (nie Fargate/Lambda)
  Discount: do 72% (taki sam jak Standard RI!)
  Zastępuje: Standard RI ale z rozmiarem flexibility
  
SageMaker Savings Plans:
  Commit: $/h dla SageMaker instances
  Flexibility: dowolny region, rozmiar, typ (ml.*)
  Discount: do 64%
  Tylko: SageMaker (nie EC2)
```

### Jak Zakupić Savings Plans

```bash
# Sprawdź rekomendacje (Cost Explorer):
aws ce get-savings-plans-purchase-recommendation \
  --savings-plans-type COMPUTE_SP \
  --term-in-years ONE_YEAR \
  --payment-option NO_UPFRONT \
  --account-scope PAYER

# Output: EstimatedMonthlySavings, RecommendedHourlyCommitment

# Zakup (przez Console lub CLI):
aws savingsplans purchase-savings-plan \
  --savings-plan-offering-id "offering-id-from-describe" \
  --commitment 10.00 \          ← $10/h przez 1 rok
  --purchase-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --client-token $(uuidgen)
```

### Savings Plans vs RI — Kiedy Co

```
┌─────────────────────────────┬──────────────────────┬──────────────────────┐
│ Kryterium                   │ Savings Plans        │ Reserved Instances   │
├─────────────────────────────┼──────────────────────┼──────────────────────┤
│ Zarządzanie                 │ Prostsze ($/h)        │ Złożone (per type)   │
│ Flexibility                 │ Compute SP: max       │ Standard: niska      │
│ Discount level              │ Compute: do 66%       │ Standard: do 72%     │
│ Fargate/Lambda              │ ✅ Compute SP         │ ❌                   │
│ Sprzedaż                    │ ❌                    │ ✅ (Standard)        │
│ Widoczność w Bill           │ $/h commitment        │ Per instance         │
│ Modyfikacja                 │ ❌ Nie można          │ Exchange (Convertible│
│ Zonal RI (capacity reserv.) │ ❌                    │ ✅ (Zonal RI only)   │
│ Multi-region                │ ✅ Compute SP         │ ❌                   │
└─────────────────────────────┴──────────────────────┴──────────────────────┘

Strategia optymalna:
  1. Compute SP → elastyczność, multi-family, Fargate/Lambda
  2. EC2 Instance SP → najlepszy discount dla stabilnej rodziny
  3. Standard RI → tylko gdy potrzebujesz capacity reservation (Zonal)
  4. Spot → fault-tolerant, burst, batch
```

---

### 19.6 Dedicated Hosts i Dedicated Instances

### Dedicated Host

```
Dedicated Host = FIZYCZNY SERWER dedykowany tylko dla twojego konta
               = widzisz fizyczne gniazda CPU i rdzenie

Zastosowania:
  1. BYOL (Bring Your Own License):
     Licencje SQL Server, Windows, Oracle, RHEL, SUSE
     które są per-socket lub per-core
     Dedicated Host: wiesz DOKŁADNIE ile socketów/rdzeni używasz
     
  2. Compliance wymagający izolacji sprzętowej:
     "Nasze dane nie mogą być na sprzęcie współdzielonym"
     Regulacje: banking, healthcare, government
     
  3. Visibility wymagania:
     Audyt: "które instancje na którym fizycznym serwerze"
     
Zarządzanie:
  Konfiguracja: 1 lub 2 socket, konkretna ilość cores
  Możesz uruchamiać wiele instancji na jednym Dedicated Host
  Instancje muszą pasować do socket/core budżetu hosta
  
  Licencja per-socket: m5.metal = 2 sockets → 1 socket license
  Możesz uruchomić 8× m5.xlarge na jednym m5 Dedicated Host
  (zamiast 8 oddzielnych licencji — 1 licencja per host/socket)
  
Auto-placement:
  ON: nowe instancje automatycznie rozmieszczane na dostępnych hostach
  OFF: musisz wskazać konkretny Host ID

Host Affinity:
  Instancja "przywiązana" do konkretnego hosta
  Po restart: wróci na ten sam host

Koszt:
  Płacisz per godzinę za Dedicated Host (nie per instancja!)
  Masz RI i Savings Plans dla Dedicated Hosts (On-Demand i Reserved)
  
  Przykład: Dedicated Host m5.xlarge → $5.47/h OD lub $2.62/h 1yr RI
```

### Dedicated Instance

```
Dedicated Instance = instancja na FIZYCZNYM SPRZĘCIE dedykowanym TYLKO
                     dla twojego konta — ale nie wiesz który host

vs Dedicated Host:
  Dedicated Instance: nie widzisz fizycznych socketów/rdzeni
                      prostsze zarządzanie
                      BYOL per-VM jest możliwe (nie per-socket)
                      może być na różnych hostach po restart
                      
  Dedicated Host:     widzisz fizyczny host
                      BYOL per-socket/core
                      Host affinity
                      droższy
                      
Koszt Dedicated Instance:
  $2 per Region per godzinę (opłata za dedicated tenancy)
  + normalna cena OD za instancje
  
Konfiguracja:
  W Launch Template: tenancy=dedicated
  Na poziomie VPC: można wymusić dedicated dla całego VPC
    aws ec2 modify-vpc-tenancy --vpc-id vpc-xxx --instance-tenancy dedicated
    (NIE rób dla prod VPC — wszystkie instancje dedykowane = drożej)
```

---

### 19.7 Capacity Reservations

### On-Demand Capacity Reservation

```
Problem: Potrzebujesz gwarancji że EC2 będzie dostępne w konkretnym AZ
         (np. dla DR, dla burst w znany dzień)
         
Capacity Reservation:
  Rezerwujesz pojemność TERAZ, nie płacisz za nieużywaną
  (płacisz OD rate niezależnie czy instancja działa)
  
  Możesz łączyć z:
    Savings Plans lub RI → dostać discount za zarezerwowaną pojemność
    
  Scope: konkretna AZ (nie regional jak RI)
  
aws ec2 create-capacity-reservation \
  --instance-type m5.xlarge \
  --instance-platform Linux/UNIX \
  --availability-zone eu-west-1a \
  --instance-count 10 \
  --end-date-type limited \
  --end-date 2026-12-31T00:00:00Z

Kiedy używać:
  DR planowanie — "mogę potrzebować 20 instancji w AZ-1a na wypadek awarii"
  Capacity block dla regularnych workloadów
  Compliance: "workload musi działać w konkretnym AZ"
  Przed świętami/wydarzeniami — zarezerwuj capacity z wyprzedzeniem

Capacity Reservation Group:
  Możesz grupować reservations dla ASG
  ASG prefers reservations z grupy przed ogólną pojemnością
  
  aws ec2 create-capacity-reservation-fleet \
    --allocation-strategy prioritized \
    --instance-type-specifications '[{
      "InstanceType": "m5.xlarge",
      "TotalTargetCapacity": 10,
      "Priority": 1
    }]'
```

---

### 19.8 UltraServers (EC2 UltraCluster)

**UltraServers** to superklastry EC2 z bardzo dużą ilością GPU — dla foundation model training.

```
EC2 UltraCluster:
  Zestaw p4d.24xlarge lub p5.48xlarge w Cluster Placement Group
  Połączone przez EFA (Elastic Fabric Adapter) — 400 Gbps collective bandwidth
  Całe klastry dostępne jako jednostka
  
Charakterystyka:
  p5.48xlarge: 8× H100 GPU, 192 vCPU, 2 TB RAM, 3.2 TB NVMe
  8 instancji p5 = 64× H100 GPU (jeden "UltraServer")
  EFA bandwidth: 3,200 Gbps (8 instancji)
  
Zakup:
  Capacity Block (nowy model) — zarezerwuj klaster na konkretny czas
  Alternatywnie: Spot Fleet + Cluster PG (możliwe przerwanie)
  Managed ML services: SageMaker HyperPod (zarządza klasterem automatycznie)
  
Use cases:
  GPT-4 size model training
  Foundation models (100B+ parameters)
  Scientific simulation (HPC)
```

---

### 19.9 EC2 Capacity Manager (AWS Capacity Manager)

```
AWS Capacity Manager to narzędzie do zarządzania pojemnością EC2
w dużych organizacjach.

Funkcje:
  1. Capacity Visibility:
     Ile mamy zarezerwowanej pojemności per typ/AZ
     Pokrycie przez RI/SP vs On-Demand
     
  2. Recommendations:
     Cost Explorer → Purchase Recommendations
     Która rodzina powinna być SP, która RI
     
  3. Capacity Reservations Fleet:
     Zarządzanie grupami rezerwacji
     Priorytetyzacja typów instancji
     
  4. EC2 Fleet Management:
     Dywersyfikacja Spot pools
     Auto-replace Spot interruptions
     
CLI — sprawdź pokrycie RI/SP:
aws ce get-reservation-coverage \
  --time-period Start=2026-09-01,End=2026-10-01 \
  --granularity MONTHLY

aws ce get-savings-plans-coverage \
  --time-period Start=2026-09-01,End=2026-10-01 \
  --granularity MONTHLY
```

---

### 19.10 Wybór Modelu — Decision Tree

```
WORKLOAD ANALIZOWANY:

Czy może być przerwany?
  TAK → SPOT (60-90% discount)
         (fault-tolerant: batch, ML training, data processing, video encoding)
  NIE → kontynuuj

Jak długo trwa workload?
  < 1 rok → ON-DEMAND lub SAVINGS PLANS (bez long-term commit)
  1-3 lata → kontynuuj

Czy workload jest przewidywalny?
  TAK → RI lub Savings Plans
  NIE → Savings Plans (elastyczniejsze)

Czy potrzebujesz flexibility między rodzinami/regionami?
  TAK → Compute Savings Plans (Fargate/Lambda też!)
  NIE → EC2 Instance Savings Plans lub Standard RI

Czy potrzebujesz BYOL per-socket?
  TAK → Dedicated Host + RI/SP
  NIE → kontynuuj

Czy potrzebujesz gwarancji pojemności?
  TAK → Capacity Reservation (+ łącz z SP/RI dla discount)
        lub Zonal RI (capacity + discount w jednym)
  NIE → Regional RI lub SP

Typowa strategia produkcyjna:
  70% On-Demand → analizuj przez 1-3 miesiące → zakup SP
  30% Spot → fault-tolerant komponenty (workers, batch)
  + Capacity Reservation dla DR
```

---

### 19.11 Well-Architected Framework — Modele Zakupu

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | AWS Cost Explorer Purchase Recommendations, regularne audyty pokrycia RI/SP (co kwartał), automatyczne zakupy SP przez Lambda/EventBridge |
| **Security** | Dedicated Host/Instance dla compliance (regulatory requirements), hardware isolation dla sensitive data, BYOL licencje audytowalność |
| **Reliability** | Zonal RI = capacity reservation + discount, Capacity Reservations dla DR, Mixed Instances (Spot + OD) dla resilience, Spot interruption handling (graceful shutdown) |
| **Performance Efficiency** | Spot Fleet: price-capacity-optimized strategia, Compute SP: flexibility do używania najlepszego typu, Capacity Block dla ML: gwarantowany access do GPU |
| **Cost Optimization** | Target: 20-40% OD + 40-60% SP/RI + 20-30% Spot. Rightsizing przed zakupem RI/SP. Graviton zamiast Intel. Spot dla batch. SP Coverage > 70% dla steady-state. |
| **Sustainability** | Spot = używa "spare" capacity (less new hardware). Rightsizing = mniej zasobów. Graviton = lepsza efektywność energetyczna. |

---

## 20. EBS Volumes, Snapshots, Lifecycle Manager, Instance Store

> **Zakres:** Typy woluminów EBS (gp3, io2, st1, sc1), parametry wydajności, Multi-Attach, szyfrowanie, snapshots (incremental, fast snapshot restore), Amazon Data Lifecycle Manager, Instance Store vs EBS. Deep-dive z konfiguracją i troubleshooting.

---

### 20.1 EBS — Elastic Block Store Przegląd

**EBS** = trwałe, sieciowe urządzenia blokowe dla EC2. Persystują niezależnie od cyklu życia instancji.

```
┌──────────────────────────────────────────────────────────────────┐
│                         VPC / AZ                                 │
│                                                                  │
│  EC2 Instance (eu-west-1a)         EBS Volume (eu-west-1a)      │
│  ┌──────────────────────────┐      ┌──────────────────────────┐  │
│  │                          │      │                          │  │
│  │  /dev/nvme0n1 (root)  ───┼──────►  gp3: 20GB              │  │
│  │  /dev/nvme1n1 (data)  ───┼──────►  io2: 1TB               │  │
│  │                          │      │                          │  │
│  └──────────────────────────┘      └──────────────────────────┘  │
│                                                                  │
│  Cechy EBS:                                                      │
│  - Dostępne tylko w tym samym AZ co EC2 (domyślnie)             │
│  - Sieciowe (przez NVMe protokół wewnętrznie)                   │
│  - Persystują po stop/terminate (DeleteOnTermination = false)    │
│  - Snapshoty do S3 (cross-AZ, cross-region)                     │
│  - Szyfrowanie at-rest + in-transit (KMS)                       │
└──────────────────────────────────────────────────────────────────┘
```

---

### 20.2 Typy Woluminów EBS — Szczegółowa Tabela

```
┌─────────┬────────────┬──────────┬────────────┬──────────────────────────────┐
│ Typ     │ Kategoria  │ Max IOPS │ Max Through│ Use Case                     │
├─────────┼────────────┼──────────┼────────────┼──────────────────────────────┤
│ gp3     │ SSD General│ 16,000   │ 1,000 MB/s │ Boot, dev/test, general app  │
│ gp2     │ SSD General│ 16,000   │ 250 MB/s   │ Legacy (gp3 > gp2 zawsze)    │
│ io2     │ SSD Provis.│ 64,000   │ 1,000 MB/s │ Critical DBs, IOPS-intensive │
│ io2 BE  │ SSD Block  │ 256,000  │ 4,000 MB/s │ SAP HANA, Oracle RAC, Multi-A│
│ io1     │ SSD Provis.│ 64,000   │ 1,000 MB/s │ Legacy (io2 > io1)          │
│ st1     │ HDD Throughp│ 500     │ 500 MB/s   │ Sequential: Hadoop, DWH, logs│
│ sc1     │ HDD Cold   │ 250      │ 250 MB/s   │ Archival, infrequent access  │
│ magnetic│ HDD Prev.  │ 40-200   │ 90 MB/s    │ Legacy, avoid               │
└─────────┴────────────┴──────────┴────────────┴──────────────────────────────┘
```

### gp3 — General Purpose SSD (Standard)

```
gp3 = nowy standard, zastępuje gp2 we wszystkich use cases

Charakterystyka:
  Rozmiar:      1 GB – 16 TB
  IOPS:         3,000 IOPS baseline ZAWSZE (niezależnie od rozmiaru!)
                Max: 16,000 IOPS (dodatkowe IOPS: $0.005/IOPS-mc ponad 3,000)
  Throughput:   125 MB/s baseline
                Max: 1,000 MB/s (dodatkowy: $0.040/MB/s-mc ponad 125)
  Latency:      single-digit milliseconds
  Koszt:        $0.08/GB-mc

gp3 vs gp2 KLUCZOWA różnica:
  gp2: IOPS = 3 × rozmiar GB (max 16,000)
       500 GB gp2 = 1,500 IOPS (provision z rozmiarem tied!)
       gp2 burst credits (jak T-type CPU credits) — do 3,000 IOPS burstowo
       
  gp3: IOPS NIEZALEŻNE od rozmiaru
       500 GB gp3 = 3,000 IOPS baseline (możesz provisioned 16,000 niezależnie)
       Throughput max 1,000 MB/s (vs gp2 max 250 MB/s)
       Tańszy: gp3 $0.08/GB vs gp2 $0.10/GB
       
  Migracja gp2 → gp3:
  aws ec2 modify-volume --volume-id vol-xxx --volume-type gp3
  Zero downtime! (hot modification)
```

### io2 Block Express — Provisioned IOPS SSD

```
io2 Block Express (najnowszy):
  Rozmiar:      4 GB – 64 TB
  IOPS:         Max 256,000 (!) — 1,000 IOPS/GB ratio
  Throughput:   4,000 MB/s
  Latency:      sub-millisecond (< 1ms P99)
  Multi-Attach: ✅ (do 16 EC2 jednocześnie!)
  Dostępność:   99.999% (5 nines)
  Koszt:        $0.125/GB-mc + $0.065/provisioned IOPS-mc

io2 (nie Block Express):
  Max IOPS: 64,000, Max Throughput: 1,000 MB/s
  
io1 (stary): Unikaj — io2 tańszy i lepszy przy tej samej cenie IOPS

Kiedy io2:
  Relacyjne DB: MySQL, PostgreSQL, Oracle (high IOPS)
  NoSQL: Cassandra, MongoDB
  SAP HANA: wymaga io2 Block Express (256K IOPS)
  EBS Multi-Attach: Oracle RAC, clustered applications
```

### st1 i sc1 — HDD Volumes

```
st1 (Throughput Optimized HDD):
  Rozmiar:      125 GB – 16 TB
  Max IOPS:     500
  Max Throughput: 500 MB/s (burst: sustained dla sequential)
  Koszt:        $0.045/GB-mc
  Nie może być root volume!
  Use: Hadoop HDFS, Kafka (log storage), sequential data processing
  
sc1 (Cold HDD):
  Rozmiar:      125 GB – 16 TB
  Max IOPS:     250
  Max Throughput: 250 MB/s
  Koszt:        $0.015/GB-mc (najtańszy EBS!)
  Nie może być root volume!
  Use: Archival data, infrequent access, backup storage

st1/sc1 NIE NADAJĄ się dla:
  Boot volumes (brak wsparcia)
  Random I/O (baza danych)
  Low-latency applications
  
HDD burst bucket:
  Podobnie jak T3 CPU credits — TB/s burst credits
  Baseline: 40 MB/s per TB (sc1) lub 40 MB/s per TB (st1)
  Burst: 250 MB/s (sc1) lub 500 MB/s (st1)
```

---

### 20.3 EBS Multi-Attach

```
io2 Block Express TYLKO = jeden wolumin podłączony do wielu EC2

┌──────────────────────────────────────────────────────────────┐
│                     AZ (eu-west-1a)                          │
│                                                              │
│  EC2-1  EC2-2  EC2-3  ...  EC2-16                           │
│    │      │      │            │                              │
│    └──────┴──────┴────────────┘                              │
│                  │                                           │
│          io2 Block Express Volume (10TB)                     │
│                                                              │
└──────────────────────────────────────────────────────────────┘

Wymagania:
  ✅ io2 Block Express only
  ✅ Wszystkie EC2 w tym samym AZ
  ✅ Max 16 EC2 jednocześnie
  ✅ Cluster-aware filesystem (GFS2, OCFS2) — nie ext4/XFS!
     (multi-writer filesystem potrzebny do koordynacji)
  
Dlaczego cluster-aware filesystem:
  Zwykły ext4/XFS: nie obsługuje concurrent writes z wielu hostów
  GFS2 (Red Hat): distributed lock manager
  OCFS2 (Oracle): cluster filesystem dla Oracle RAC
  
Use cases:
  Oracle RAC (Real Application Clusters)
  Linux HA clusters (Pacemaker + DRBD alternatywa)
  SAP HANA Scale-Out (rzadko przez EBS, częściej instance store lub FSx)
```

---

### 20.4 EBS Szyfrowanie

```
EBS Encryption:
  Szyfrowanie: AES-256
  KMS key: AWS managed key (aws/ebs) lub customer managed key (CMK)
  
  Co jest szyfrowane:
    ✅ Dane at-rest na woluminie
    ✅ Dane in-transit między EC2 a EBS (sieć AWS, zawsze zaszyfrowana)
    ✅ Snapshoty (dziedziczą szyfrowanie z woluminu)
    ✅ Woluminy z zaszyfrowanych snapshotów (automatycznie zaszyfrowane)
    
  Co NIE jest szyfrowane przez EBS:
    ❌ Dane wewnątrz OS (filesystem encryption to osobna warstwa)
    (ale EBS encryption obsługuje to na poziomie storage)

Domyślne szyfrowanie (konto-level):
  aws ec2 enable-ebs-encryption-by-default
  aws ec2 get-ebs-encryption-by-default  # sprawdź
  
  Po włączeniu: WSZYSTKIE nowe woluminy automatycznie zaszyfrowane
  Istniejące woluminy: nie są automatycznie szyfrowane (tylko nowe)

Szyfrowanie istniejącego niezaszyfrowanego woluminu:
  Nie można zaszyfrować istniejącego woluminu bezpośrednio!
  Procedura:
    1. Snapshot woluminu (niezaszyfrowany)
    2. Kopiuj snapshot z --encrypted (tworzy zaszyfrowaną kopię)
    3. Utwórz nowy wolumin z zaszyfrowanego snapshotu
    4. Podmień wolumin (stop EC2, detach old, attach new)
    
Szyfrowanie cross-account share:
  Zaszyfrowany snapshot z CMK → share z innym kontem
  Inne konto musi mieć grant do CMK przez Key Policy
  Inne konto nie może używać twojego aws/ebs managed key!
```

---

### 20.5 EBS Snapshots — Incremental Backup

```
Snapshot = point-in-time kopia EBS woluminu zapisana w S3
           (nie widzisz tego bucketu — zarządzane przez AWS)

Snapshot jest INCREMENTAL:
  Snapshot 1: kopiuje wszystkie bloki (np. 20 GB zajętych)
  Snapshot 2: kopiuje TYLKO zmienione bloki od Snapshot 1
  Snapshot 3: kopiuje TYLKO zmienione bloki od Snapshot 2
  
  Każdy snapshot self-sufficient — możesz przywrócić z dowolnego snapshotu
  (AWS internalize chain — nie musisz mieć wszystkich poprzednich)
  
Koszt snapshotów:
  $0.05/GB-mc (tylko unikalne dane — nie pełny rozmiar woluminu)
  Przykład: 100 GB wolumin, 5 GB zmian dziennie
    Snapshot 1: ~5 GB (zajęte) × $0.05 = $0.25/mc
    Snapshot 2: ~5 GB (zmienione) × $0.05 = $0.25/mc
    (nie 200 GB za dwa snapshoty!)

Tworzenie snapshotu:
  aws ec2 create-snapshot \
    --volume-id vol-0abc123 \
    --description "DB backup 2026-10-27" \
    --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=db-backup}]'
  
  Snapshot w trakcie: wolumin nadal dostępny! (nie musisz unmountować)
  Ale: dla spójności DB → flush + freeze lub zatrzymaj aplikację

Multi-volume snapshot (application consistent):
  aws ec2 create-snapshots \
    --instance-specification InstanceId=i-0abc123,ExcludeBootVolume=false \
    --description "Application consistent snapshot"
  
  Tworzy snapshoty wszystkich woluminów EC2 jednocześnie (crash-consistent)
```

### Fast Snapshot Restore (FSR)

```
Problem: Przywrócenie z dużego snapshotu do woluminu → "lazy initialization"
         Pierwsze odczyty z woluminu: wolne (bloki pobierane z S3 on-demand)
         Może trwać godziny zanim wszystkie bloki są "warm"
         
Fast Snapshot Restore:
  Pre-initializes wszystkie bloki snapshotu
  Przywrócony wolumin działa od razu z pełną wydajnością
  Brak "warming up" fazy
  
  Koszt: $0.75/FSR snapshot/AZ/godzinę
  (drogie! używaj tylko dla krytycznych snapshots w DR)
  
  Włącz:
  aws ec2 enable-fast-snapshot-restores \
    --source-snapshot-ids snap-0abc123 \
    --availability-zones eu-west-1a eu-west-1b
  
  Sprawdź:
  aws ec2 describe-fast-snapshot-restores \
    --filters Name=snapshot-id,Values=snap-0abc123

Alternatywa dla FSR (tańsza):
  Pre-warming: dd if=/dev/xvda of=/dev/null bs=128k
  (czyta cały wolumin sekwencyjnie → AWS pobiera bloki z S3 → wolumin warm)
  Trwa długo dla dużych woluminów ale bezpłatne
```

### Snapshot Cross-Region Copy

```bash
# Kopiuj snapshot do innego regionu:
aws ec2 copy-snapshot \
  --source-region eu-west-1 \
  --source-snapshot-id snap-0abc123 \
  --destination-region us-east-1 \
  --description "DR copy" \
  --encrypted \
  --kms-key-id alias/dr-key     # KMS key w destination region

# Automatyczna cross-region copy przez DLM (patrz 17.8)

# Snapshot sharing między kontami:
aws ec2 modify-snapshot-attribute \
  --snapshot-id snap-0abc123 \
  --attribute createVolumePermission \
  --operation-type add \
  --user-ids 123456789012
```

---

### 20.6 EBS Volume Modification (Hot Resize)

```
Możesz modyfikować woluminy BEZ zatrzymania EC2:

Co można modyfikować na gorąco (in-place, no downtime):
  ✅ Rozmiar (zwiększ, nie zmniejsz!)
  ✅ Typ (gp2 → gp3, gp3 → io2...)
  ✅ IOPS (dla gp3 i io2)
  ✅ Throughput (dla gp3)
  
Co MUSISZ zrobić po zwiększeniu rozmiaru (EBS):
  EBS volume jest większy, ale filesystem tego nie widzi!
  Musisz rozszerzyć filesystem po stronie OS:
  
  # Linux ext4:
  sudo resize2fs /dev/nvme0n1
  
  # Linux xfs:
  sudo xfs_growfs /mount/point
  
  # Linux LVM:
  sudo pvresize /dev/nvme0n1
  sudo lvextend -l +100%FREE /dev/mapper/vg-lv
  sudo resize2fs /dev/mapper/vg-lv

Cooldown period:
  Po modyfikacji: 6 godzin cooldown (nie możesz zmodyfikować ponownie)
  
aws ec2 modify-volume \
  --volume-id vol-0abc123 \
  --size 200 \
  --volume-type gp3 \
  --iops 6000 \
  --throughput 500

# Sprawdź status modyfikacji:
aws ec2 describe-volumes-modifications --volume-ids vol-0abc123
# State: optimizing → completed
```

---

### 20.7 EBS Wydajność — Limity i Bottlenecks

```
Każda instancja EC2 ma limit bandwidth dla EBS (EBS-optimized):
  
  t3.medium:    2,085 Mbps (260 MB/s), 16,000 IOPS
  m5.xlarge:    4,750 Mbps (594 MB/s), 18,750 IOPS
  m5.4xlarge:   8,750 Mbps (1,094 MB/s), 43,750 IOPS
  
  Wolumin io2 256,000 IOPS → potrzebujesz instancji wspierającej taki bandwidth!
  
  Znalezienie bottlenecka:
    CloudWatch metryki:
      VolumeReadOps/WriteOps  → IOPS
      VolumeReadBytes/WriteBytes → Throughput
      VolumeQueueLength → queue depth (>1 na steady state = throttling?)
      BurstBalance (gp2 only) → credit balance
      
    # Sprawdź czy instancja throttle EBS:
    iostat -x 1 /dev/nvme0n1
    # %iowait > 0 przy małym queue = disk bottleneck
    # queue_depth > 1 sustained = disk saturated
    
Multi-volume stripe (dla wyższego throughput):
  RAID 0 na EBS (mdadm lub LVM striping):
  2× io2 64K IOPS = 128K IOPS przez RAID 0 stripping
  
  # Utwórz RAID 0 z 2 woluminami:
  mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/nvme1n1 /dev/nvme2n1
  mkfs.xfs /dev/md0
  mount /dev/md0 /data
  
  Uwaga: RAID 0 = brak redundancji. Jeden wolumin padnie = cały array!
  Używaj tylko dla temp data lub mając snapshoty.
```

---

### 20.8 Amazon Data Lifecycle Manager (DLM)

**DLM** = automatyczne zarządzanie cyklem życia EBS snapshotów i AMI.

```
DLM Policy = reguła definiująca:
  1. Jakie zasoby zarządzać (tagi EC2 lub EBS)
  2. Harmonogram (co ile godzin/dni tworzyć snapshot)
  3. Retencja (ile snapshotów zachować lub ile dni trzymać)
  4. Cross-region copy (opcjonalnie)
  5. Cross-account share (opcjonalnie)
  6. Fast Snapshot Restore (opcjonalnie)
```

### Tworzenie DLM Policy

```bash
# Policy dla codziennych backupów produkcji:
aws dlm create-lifecycle-policy \
  --description "Daily production backups" \
  --state ENABLED \
  --execution-role-arn arn:aws:iam::123456789:role/AWSDataLifecycleManagerDefaultRole \
  --policy-details '{
    "PolicyType": "EBS_SNAPSHOT_MANAGEMENT",
    "ResourceTypes": ["VOLUME"],
    "TargetTags": [{"Key": "Environment", "Value": "Production"}],
    "Schedules": [{
      "Name": "Daily backup",
      "CreateRule": {
        "Interval": 24,
        "IntervalUnit": "HOURS",
        "Times": ["03:00"]
      },
      "RetainRule": {
        "Count": 7
      },
      "CopyTags": true,
      "CrossRegionCopyRules": [{
        "TargetRegion": "us-east-1",
        "Encrypted": true,
        "KmsKeyId": "arn:aws:kms:us-east-1:123:key/xxx",
        "RetainRule": {"Interval": 30, "IntervalUnit": "DAYS"},
        "CopyTags": true
      }]
    }]
  }'
```

### DLM Policy Typy

```
EBS Snapshot Policy:
  Tworzy snapshoty woluminów EBS
  Może: cross-region copy, cross-account share, FSR enable
  Target: EBS volumes lub EC2 instances (snapshot wszystkich woluminów)
  
AMI Policy:
  Tworzy AMI z EC2 instancji
  Może: cross-region distribution, deregister + delete snapshot
  Target: EC2 instances z tagiem
  
Event-Based Policy:
  Reaguje na eventy (np. stary snapshot osiągnie wiek X) → copy do innego regionu
  Zarządzanie cyklem życia importowanych/sharowanych snapshotów
  
Retencja:
  Count-based: zachowaj ostatnie N snapshotów
  Age-based: zachowaj snapshoty przez N dni/tygodni/miesięcy/lat
  
  Stare snapshoty (ponad retencję): automatycznie usuwane przez DLM
```

### DLM IAM Role

```
DLM potrzebuje IAM Role do wykonywania snapshotów:

# AWS tworzy domyślną rolę: AWSDataLifecycleManagerDefaultRole
# ale możesz użyć własnej:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateSnapshot",
        "ec2:CreateSnapshots",
        "ec2:DeleteSnapshot",
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "ec2:DescribeSnapshots",
        "ec2:EnableFastSnapshotRestores",
        "ec2:DescribeFastSnapshotRestores",
        "ec2:DisableFastSnapshotRestores",
        "ec2:CopySnapshot",
        "ec2:ModifySnapshotAttribute",
        "ec2:DescribeSnapshotAttribute"
      ],
      "Resource": "*"
    }
  ]
}
```

---

### 20.9 EBS Recycle Bin

```
Recycle Bin = ochrona przed przypadkowym usunięciem EBS snapshots i AMI

Konfiguracja:
  Utwórz retention rule:
  aws rbin create-rule \
    --retention-period RetentionPeriodValue=30,RetentionPeriodUnit=DAYS \
    --resource-type EBS_SNAPSHOT \
    --description "30-day retention for snapshots"

  Lub:
  aws rbin create-rule \
    --retention-period RetentionPeriodValue=30,RetentionPeriodUnit=DAYS \
    --resource-type EC2_IMAGE \     ← dla AMI
    --resource-tags '[{"ResourceTagKey": "Environment", "ResourceTagValue": "Production"}]'

Po przypadkowym usunięciu:
  Snapshot trafia do Recycle Bin (nie jest od razu usuwany)
  Przez 30 dni (lub ustawiony okres) możesz przywrócić:
  
  aws rbin list-rules           # sprawdź reguły
  aws rbin list-resources       # snapshoty w koszu
  aws rbin recover-resource \
    --identifier snap-0abc123 \
    --resource-type EBS_SNAPSHOT
  
  Po 30 dniach: trwale usunięty (nie można przywrócić)
```

---

### 20.10 Instance Store vs EBS — Kiedy Co

```
┌──────────────────────┬─────────────────────────┬─────────────────────────┐
│ Cecha                │ EBS                     │ Instance Store          │
├──────────────────────┼─────────────────────────┼─────────────────────────┤
│ Persystencja         │ ✅ Przeżywa stop/terminat│ ❌ Tracone przy stop/term│
│ Root volume          │ ✅ Tak                   │ ⚠️ Tylko start typy inst │
│ Snapshot             │ ✅ Tak                   │ ❌ Nie                   │
│ Hot-attach/detach    │ ✅ Tak (secondary vol.)  │ ❌ Nie (w instancji)     │
│ Koszt                │ Dodatkowy               │ Wliczony w cenę instancji│
│ Max IOPS             │ 256,000 (io2 BE)         │ Miliony (NVMe bezpoś.) │
│ Max Throughput       │ 4,000 MB/s              │ 10+ GB/s per vol.       │
│ Latency              │ ~125µs - 1ms            │ ~50-100µs (direct NVMe) │
│ Reliability          │ 99.999% (5 nines)       │ Awaria hosta = utrata   │
│ Multi-AZ             │ Snapshot → inne AZ      │ ❌ Fixed do hosta        │
│ Typy instancji       │ Wszystkie               │ Tylko d, i, r5d, m5d... │
└──────────────────────┴─────────────────────────┴─────────────────────────┘

Instance Store — idealne zastosowania:
  Tymczasowe buforowanie (Memcached, Redis jako cache)
  Temp data dla batch processing (scratch space)
  Log files (jeśli nie potrzebujesz po stop)
  ML model serving (model załadowany z S3 do instance store)
  
EBS — ZAWSZE dla:
  Relacyjne bazy danych (MySQL, PostgreSQL, Oracle)
  Root volume (OS)
  Dane które muszą przeżyć restart/failover
  Compliance wymagający persystencji i audytowania
```

---

### 20.11 Well-Architected Framework — EBS/Snapshots

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | DLM automatyzuje backup/retention, tagi woluminów (Application, Environment, Owner), CloudWatch alerty na BurstBalance < 50%, regularne testowanie przywracania ze snapshotów |
| **Security** | Default EBS encryption (konto-level), CMK zamiast aws/ebs dla compliance, snapshot sharing ograniczone do znanych kont, Recycle Bin dla ochrony przed przypadkowym usunięciem |
| **Reliability** | DLM daily snapshots + cross-region copy, Multi-volume snapshots (crash-consistent), Fast Snapshot Restore dla krytycznych workloadów, Recycle Bin retention ≥ 30 dni dla prod |
| **Performance Efficiency** | gp3 zamiast gp2 (niezależne IOPS/throughput, tańszy), io2 Block Express dla krytycznych IOPS, EBS-optimized instances, monitoruj VolumeQueueLength, RAID 0 dla aggregate throughput |
| **Cost Optimization** | gp3 ~20% tańszy niż gp2, sc1 dla archival, DLM retention policies (nie trzymaj 100 snapshotów), delete unused volumes (sprawdź describe-volumes filter state=available), snapshoty incremental = małe koszty przy małych zmianach |
| **Sustainability** | sc1 dla cold data (mniej zasobów), Snapshot compression (S3-level), usuwanie starych woluminów/snapshotów (mniej storage = mniej energii) |

---

## 21. Target Groups, Trust Stores, Auto Scaling Groups — Deep Dive

> **Zakres:** ALB/NLB/GWLB Target Groups (typy, health checks, routing), Trust Stores (mutual TLS), Auto Scaling Groups deep-dive (lifecycle hooks, warm pools, instance refresh, scaling policies, termination policies, mixed instances). 60+ Q&A.

---

### 21.1 Target Groups — Czym Są

**Target Group (TG)** = logiczna grupa odbiorców ruchu dla ELB (ALB/NLB/GWLB). ELB kieruje ruch do zdrowych targetów w TG.

```
┌────────────────────────────────────────────────────────────────────┐
│                         ELB                                        │
│                                                                    │
│  Listener (port 443)                                               │
│    Rule 1: host = api.example.com → Forward to TG-API             │
│    Rule 2: path = /images/* → Forward to TG-Static                │
│    Rule 3: default → Forward to TG-Web                            │
│                                                                    │
├───────────────┬───────────────────┬───────────────────────────────┤
│  TG-API       │  TG-Static        │  TG-Web                       │
│  10.0.1.5:8080│  10.0.2.10:80    │  10.0.1.20:80                 │
│  10.0.1.6:8080│  10.0.2.11:80    │  10.0.1.21:80                 │
│  (EC2 targets)│  (Lambda targets) │  (IP targets - on-prem)       │
└───────────────┴───────────────────┴───────────────────────────────┘
```

---

### 21.2 Typy Targetów w Target Group

```
┌──────────────┬──────────────────────────────────────────────────────┐
│ Target Type  │ Opis                                                 │
├──────────────┼──────────────────────────────────────────────────────┤
│ instance     │ EC2 instancja (przez Instance ID)                   │
│              │ Trafik → primary ENI EC2                            │
│              │ SG EC2: source = SG ALB lub CIDR subnetu ALB        │
├──────────────┼──────────────────────────────────────────────────────┤
│ ip           │ Konkretny IP (RFC 1918 lub peering VPC)             │
│              │ EC2 secondary ENI, kontenery ECS, on-prem przez DX  │
│              │ Port może różnić się per target (dynamic ports ECS)  │
├──────────────┼──────────────────────────────────────────────────────┤
│ lambda       │ Lambda function jako target (ALB only)              │
│              │ Request → HTTP event object → Lambda                │
│              │ Max payload: 1 MB, max timeout: 300s                │
├──────────────┼──────────────────────────────────────────────────────┤
│ alb          │ ALB jako target (NLB → ALB chaining)                │
│              │ NLB static IP → forward to ALB (L7 routing)        │
└──────────────┴──────────────────────────────────────────────────────┘
```

### Target Type: IP — Szczegóły

```
IP target — obsługiwane zakresy:
  ✅ RFC 1918: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
  ✅ 100.64.0.0/10 (shared address space)
  ✅ RFC 6598: on-premises przez Direct Connect / VPN
  ✅ Inne VPC (przez VPC peering + IP routing)
  
  ❌ Public IPs (nie można)
  ❌ 127.0.0.0/8 (localhost)
  
ECS Fargate — dlaczego ip:
  Fargate task = ENI z IP (awsvpc mode)
  Każdy task ma inny port (dynamiczny port mapping) i inny IP
  Target Type "ip" dla Fargate TG:
    Rejestracja: IP:Port task (np. 10.0.1.45:32768)
    Wyrejestrowanie: po stop task (ECS robi to automatycznie)
    
On-Premises targets (Hybrid Load Balancing):
  ALB może kierować ruch do on-prem serverów przez VPN lub DX!
  Zarejestruj: IP prywatnego serwera on-prem w TG
  ALB → (VPN/DX) → on-prem server 192.168.1.100:8080
  
  Użycie: stopniowa migracja (część ruchu → cloud, część → on-prem)
```

### Target Type: Lambda

```
ALB → Lambda — specjalny mechanizm:

Request HTTP → ALB → Lambda JSON event:
{
  "requestContext": {"elb": {...}},
  "httpMethod": "GET",
  "path": "/api/users",
  "queryStringParameters": {"page": "1"},
  "headers": {"Host": "api.example.com", ...},
  "body": "...",
  "isBase64Encoded": false
}

Lambda response (musi zwrócić):
{
  "statusCode": 200,
  "headers": {"Content-Type": "application/json"},
  "body": "{\"users\": [...]}",
  "isBase64Encoded": false
}

Ograniczenia:
  Max payload (request + response): 1 MB
  Timeout: max 300s (Lambda max) ale ALB timeout krócej (60s def.)
  Multi-value headers: domyślnie wyłączone (włącz dla querystring z duplikatami)
  Invoke = synchronous (RequestResponse)
  
Konfiguracja:
  aws elbv2 register-targets \
    --target-group-arn arn:aws:elasticloadbalancing:...:targetgroup/TG-Lambda/xxx \
    --targets Id=arn:aws:lambda:eu-west-1:123:function:MyFunction
  
  # Lambda permission:
  aws lambda add-permission \
    --function-name MyFunction \
    --statement-id AllowALBInvoke \
    --action lambda:InvokeFunction \
    --principal elasticloadbalancing.amazonaws.com \
    --source-arn arn:aws:elasticloadbalancing:...:targetgroup/TG-Lambda/xxx
```

---

### 21.3 Health Checks w Target Group

```
Health check = ALB/NLB regularnie sprawdza czy target jest "zdrowy"

Parametry Health Check:
┌──────────────────────────────────────────────────────────────────────┐
│ Protocol:          HTTP, HTTPS, TCP, GRPC                           │
│ Path:              /health, /ping, /api/v1/status  (HTTP/HTTPS only)│
│ Port:              traffic port (default) lub konkretny port        │
│ Healthy threshold: 3 (ile sukcesów = healthy)                       │
│ Unhealthy threshold: 3 (ile niepowodzeń = unhealthy)                │
│ Timeout:           5s (ile czekać na odpowiedź)                     │
│ Interval:          30s (co ile sprawdzać)                           │
│ Success codes:     200 (HTTP) lub 200-499 (zakres)                 │
└──────────────────────────────────────────────────────────────────────┘

Strategia health check endpoint:
  /health powinien sprawdzać:
    ✅ Aplikacja odpowiada (process running)
    ✅ DB connection (jeśli krytyczne)
    ✅ Cache connection
    ✅ Zwraca HTTP 200 szybko (< timeout)
    
  NIE sprawdzaj w /health:
    ❌ Zewnętrznych API (mogą być wolne lub down niezależnie od twojego app)
    ❌ Ciężkich operacji (nie obciążaj serwera health checkami)
    
Deep health check (opcjonalnie osobny endpoint):
  /health/deep — sprawdza wszystko łącznie z zewnętrznymi zależnościami
  Używany przez monitoring, NIE przez ELB (zbyt wolny)

Health check dla NLB:
  TCP health check: tylko sprawdza czy port otwarty (3-way handshake)
  HTTP/HTTPS: jak ALB (pełny HTTP request)
  
Health Check przy Deregistration:
  Deregistering state: nowe połączenia nie trafiają, stare kończone
  Deregistration delay: 300s (default) — czas na dokończenie in-flight requests
  Zmień: aws elbv2 modify-target-group-attributes \
    --target-group-arn ... \
    --attributes Key=deregistration_delay.timeout_seconds,Value=30
```

---

### 21.4 Target Group Attributes — Konfiguracja

```bash
# Sprawdź atrybuty TG:
aws elbv2 describe-target-group-attributes \
  --target-group-arn arn:aws:elasticloadbalancing:...:targetgroup/my-tg/xxx

# Kluczowe atrybuty:
#
# deregistration_delay.timeout_seconds = 300
#   Czas na zakończenie in-flight requests przy deregistracji
#
# slow_start.duration_seconds = 0
#   Czas warmup dla nowo zarejestrowanych targetów (0 = wyłączone)
#   Gdy > 0: nowy target dostaje stopniowo rosnący % ruchu przez X sekund
#
# load_balancing.algorithm.type = round_robin
#   round_robin (default) lub least_outstanding_requests
#
# stickiness.enabled = false
#   Session affinity (sticky sessions)
#
# stickiness.type = lb_cookie lub app_cookie
#
# stickiness.lb_cookie.duration_seconds = 86400 (1 dzień)

# Modyfikacja:
aws elbv2 modify-target-group-attributes \
  --target-group-arn arn:... \
  --attributes \
    Key=deregistration_delay.timeout_seconds,Value=60 \
    Key=slow_start.duration_seconds,Value=30 \
    Key=load_balancing.algorithm.type,Value=least_outstanding_requests
```

### Slow Start Mode

```
Slow Start = nowo zarejestrowany target dostaje stopniowo rosnący udział ruchu

Bez slow start:
  Nowy target → natychmiast 1/N ruchu → może nie być w pełni "warm"
  (JVM warmup, connection pool init, cache loading)

Z slow start (duration = 30s):
  Nowy target → przez 30s stopniowo od 0% do pełnego udziału
  
  Po 30s: pełny udział (1/N) jak inne targety
  
Konfiguracja: `slow_start.duration_seconds` = 30-900
Nie dostępne dla Lambda targets i z Least Outstanding Requests algorithm.
```

### Weighted Target Groups

```
ALB Listener Rule: Forward do WIELU TG z wagami (weighted forward):

Scenariusz: Blue/Green deployment lub Canary release

Listener Rule:
  Forward:
    TG-v1 (stara wersja):  weight=90
    TG-v2 (nowa wersja):   weight=10

90% ruchu → TG-v1 (stary)
10% ruchu → TG-v2 (nowy, testowany)

Stopniowe przenoszenie:
  Etap 1: v1=90, v2=10
  Etap 2: v1=70, v2=30
  Etap 3: v1=0, v2=100 (pełny shift)
  
Szybki rollback: zmień weight v2=0 → cały ruch wraca do v1

aws elbv2 modify-listener --listener-arn arn:... --default-actions '[{
  "Type": "forward",
  "ForwardConfig": {
    "TargetGroups": [
      {"TargetGroupArn": "arn:...:targetgroup/TG-v1/xxx", "Weight": 90},
      {"TargetGroupArn": "arn:...:targetgroup/TG-v2/xxx", "Weight": 10}
    ],
    "TargetGroupStickinessConfig": {
      "Enabled": true,
      "DurationSeconds": 300
    }
  }
}]'
```

---

### 21.5 Trust Stores — Mutual TLS (mTLS) dla ALB

**Trust Store** to nowa funkcja ALB (2023) — obsługa mutual TLS (mTLS) na poziomie load balancera.

### Czym Jest Mutual TLS

```
Standardowe TLS (one-way):
  Klient weryfikuje certyfikat SERWERA
  Serwer nie weryfikuje certyfikatu klienta
  
Mutual TLS (mTLS / two-way TLS):
  Klient weryfikuje certyfikat SERWERA
  Serwer weryfikuje certyfikat KLIENTA
  
  Zastosowania:
    API-to-API authentication (service mesh, microservices)
    Zero-trust security (każdy client musi mieć certyfikat)
    B2B integrations (partner systemy z certyfikatami)
    IoT devices (każde urządzenie ma swój certyfikat)
    Regulatory compliance (PCI DSS, HIPAA)
```

### ALB Trust Store — Jak Działa

```
Przed Trust Stores (stary sposób):
  mTLS obsługiwany na EC2 aplikacji (Nginx/HAProxy)
  ALB nie rozumiał mTLS — terminował TLS przed EC2

Z Trust Stores (nowy, 2023):
  ALB terminuje TLS I weryfikuje certyfikat klienta
  Certyfikat klienta → sprawdzany względem Trust Store (CA bundle)
  Jeśli certyfikat ważny i trusted → forward do backend
  Jeśli nie → 403 Forbidden (lub pass-through policy)

Przepływ:
  Klient → ALB (mTLS port 443)
    ALB: "Pokaż mi swój certyfikat"
    Klient: [certificate.pem]
    ALB: "Czy certyfikat klienta jest podpisany przez CA w Trust Store?"
    
    Jeśli TAK → forward do TG + header X-Amzn-Mtls-Clientcert-Leaf
    Jeśli NIE → 403 lub forward (zależy od policy)

ALB przekazuje do backendu (HTTP headers):
  X-Amzn-Mtls-Clientcert-Leaf: <URL-encoded client cert>
  X-Amzn-Mtls-Clientcert-Subject: CN=device-123, O=IoT Corp
  X-Amzn-Mtls-Clientcert-Validity-Not-Before: 2026-01-01
  X-Amzn-Mtls-Clientcert-Validity-Not-After: 2027-01-01
  X-Amzn-Mtls-Clientcert-Serial-Number: 1234567890
  
Backend aplikacja może używać tych headerów do identyfikacji klienta!
```

### Tworzenie Trust Store

```bash
# Krok 1: Utwórz Trust Store z bundle CA certyfikatów:
aws elbv2 create-trust-store \
  --name "iot-devices-trust-store" \
  --ca-certificates-bundle-s3-bucket my-certs-bucket \
  --ca-certificates-bundle-s3-key ca-bundle.pem

# ca-bundle.pem = plik z jednym lub więcej CA certyfikatami (PEM format):
# -----BEGIN CERTIFICATE-----
# MIID... (Root CA certyfikat)
# -----END CERTIFICATE-----
# -----BEGIN CERTIFICATE-----
# MIID... (Intermediate CA certyfikat)
# -----END CERTIFICATE-----

# Krok 2: Utwórz/zmodyfikuj HTTPS Listener z Trust Store:
aws elbv2 modify-listener \
  --listener-arn arn:aws:elasticloadbalancing:...:listener/app/my-alb/xxx \
  --mutual-authentication '{
    "Mode": "verify",               # verify = wymagaj i weryfikuj cert klienta
                                    # passthrough = przekaż do backendu bez weryfikacji
                                    # off = brak mTLS
    "TrustStoreArn": "arn:aws:elasticloadbalancing:...:truststore/iot-trust/xxx",
    "IgnoreClientCertificateExpiry": false
  }'

# Krok 3: Trust Store jest wersjonowany automatycznie
# Dodaj nowe CA (np. nowy intermediate CA):
aws elbv2 modify-trust-store \
  --trust-store-arn arn:... \
  --ca-certificates-bundle-s3-bucket my-certs-bucket \
  --ca-certificates-bundle-s3-key updated-ca-bundle.pem

# Sprawdź certyfikaty w Trust Store:
aws elbv2 describe-trust-store-revocations \
  --trust-store-arn arn:...
```

### Trust Store Certificate Revocation (OCSP/CRL)

```bash
# Dodaj CRL (Certificate Revocation List) do Trust Store:
aws elbv2 add-trust-store-revocations \
  --trust-store-arn arn:... \
  --revocation-contents S3Bucket=my-certs,S3Key=revoked-certs.crl,RevocationType=CRL

# CRL = lista odwołanych certyfikatów (Binary DER format)
# ALB sprawdza czy cert klienta jest na liście revoked
# Jeśli tak → reject connection

# Alternatywa: OCSP (Online Certificate Status Protocol)
# OCSP Stapling obsługiwany przez ALB automatycznie jeśli cert ma OCSP URL
```

---

### 21.6 Auto Scaling Groups — Deep Dive

ASG opisane w Rozdziale 12. Tutaj deep-dive zaawansowanych funkcji.

### Lifecycle Hooks — Szczegóły

```
Lifecycle Hook = zatrzymaj instancję przed/po action i wykonaj operację

LAUNCHING Hook:
  
  ASG decyduje: uruchom nową instancję
          │
          ▼
  EC2 startuje (Pending:Wait state) ← TU zatrzymuje się na X minut
          │                              
          │ Twój kod wykonuje:
          │   - Instalacja agentów
          │   - Rejestracja w service discovery  
          │   - Konfiguracja z AWS SSM
          │   - Warm-up cache
          │   - Testy
          │
          │ COMPLETE_LIFECYCLE_ACTION (success) lub ABANDON
          ▼
  EC2 → InService (lub Terminated jeśli ABANDON)
  
TERMINATING Hook:

  ASG decyduje: terminate instancję
          │
          ▼
  EC2 → Terminating:Wait ← ZATRZYMUJE SIĘ na X minut
          │
          │ Twój kod wykonuje:
          │   - Drain connections (jeśli nie za ELB)
          │   - Deregister z service discovery
          │   - Flush cache/logs do S3
          │   - Final backup
          │   - Deregister z Config Management
          │
          │ COMPLETE_LIFECYCLE_ACTION
          ▼
  EC2 → Terminated
  
Timeout (domyślnie 1 godzina, max 2h):
  Jeśli nie wywołasz complete-lifecycle-action → domyślna akcja (CONTINUE lub ABANDON)
  
Jak zaimplementować:
  EventBridge rule:
    source = ["aws.autoscaling"]
    detail.LifecycleTransition = ["autoscaling:EC2_INSTANCE_LAUNCHING"]
    → trigger Lambda (lub SQS → worker)
    
  Lambda wywołuje:
    aws autoscaling complete-lifecycle-action \
      --lifecycle-hook-name "my-launch-hook" \
      --auto-scaling-group-name "my-asg" \
      --lifecycle-action-token "<token-from-event>" \
      --lifecycle-action-result CONTINUE  # lub ABANDON
```

### Warm Pools — Szczegóły

```
Warm Pool = pre-warmed instancje gotowe do szybkiego dodania do ASG

Bez Warm Pool:
  Scale-out → uruchom nową instancję (2-5 min) → bootstrap (5-15 min) → InService
  Total lag: 7-20 min
  
Z Warm Pool:
  Warm Pool = N instancji w stanie Stopped/Running/Hibernated
  Scale-out → przenieś instancję z Warm Pool do InService (< 1 min)
  
                    ┌──────────────────────────────────┐
                    │         Warm Pool                 │
                    │  ┌──────┐ ┌──────┐ ┌──────┐     │
                    │  │Stopped│ │Stopped│ │Running│   │
                    │  └──────┘ └──────┘ └──────┘     │
                    └──────────────────────────────────┘
                              │  Scale event
                              ▼
                    ┌─────────────────────────────────┐
                    │           ASG (InService)        │
                    │  ┌─────┐ ┌─────┐ ┌─────┐ ┌────┐│
                    │  │Run. │ │Run. │ │Run. │ │NEW ││ ← z Warm Pool
                    │  └─────┘ └─────┘ └─────┘ └────┘│
                    └─────────────────────────────────┘
  
Konfiguracja:
aws autoscaling put-warm-pool \
  --auto-scaling-group-name my-asg \
  --min-size 2 \                           # min instancji w Warm Pool
  --max-group-prepared-capacity 10 \       # max (ASG desired + warm)
  --pool-state Stopped \                   # Stopped/Running/Hibernated
  --instance-reuse-policy '{"ReuseOnScaleIn": true}'  # wrócić do pool zamiast terminate

Stany Warm Pool:
  Stopped:     OS nie działa, EBS volumes persystują. Fastest boot.
  Running:     OS działa, aplikacja może być załadowana. Szybszy InService.
  Hibernated:  RAM zapisany do EBS. Najszybszy warmup (resume z RAM).
               (tylko dla instancji z hibernation support)
  
Warm Pool i lifecycle hooks:
  Można skonfigurować lifecycle hooks dla Warm Pool (oddzielne od ASG hooks)
  Instancja wchodzi do Warm Pool → hook → bootstrap → WarmPool:Stopped
  Przeniesienie do InService: szybkie (bootstrap już zrobiony)
```

### Instance Refresh — Szczegóły

```
Instance Refresh = rolling replace wszystkich instancji w ASG (nowe LT wersja, nowe AMI)

Bez Instance Refresh:
  Ręczne: terminate stare, ASG uruchamia nowe → ryzyko downtime, manualne

Z Instance Refresh:
  aws autoscaling start-instance-refresh \
    --auto-scaling-group-name my-asg \
    --preferences '{
      "MinHealthyPercentage": 80,          ← zawsze 80%+ instancji InService
      "InstanceWarmup": 300,               ← czekaj 300s po InService przed kontynuacją
      "CheckpointPercentages": [20, 50],   ← pauza na 20% i 50% (manualne ok)
      "CheckpointDelay": 600,              ← pauza przez 600s na checkpoint
      "SkipMatching": false,               ← zamień WSZYSTKIE (true = zostaw już zaktualizowane)
      "AutoRollback": true                 ← automatic rollback przy problemach
    }' \
    --desired-configuration '{"LaunchTemplate": {"LaunchTemplateName": "my-lt", "Version": "$Latest"}}'

Proces:
  1. ASG sprawdza minimalny % zdrowych
  2. Terminate jedną (lub kilka) starą instancję
  3. ASG uruchamia nową (z nowym LT)
  4. Czeka na InstanceWarmup
  5. ELB health check → InService
  6. Kontynuuje dopóki wszystkie zamienione
  
Checkpoint (pauza):
  Na 20%: pauzuje → możesz sprawdzić czy nowa wersja działa → `continue` lub `cancel`
  
Rollback:
  Ręczny: `aws autoscaling cancel-instance-refresh` → `start-instance-refresh` ze starym LT
  Automatyczny (AutoRollback): jeśli zdrowie ASG spada → auto-rollback do poprzedniego LT
  
Status sprawdzenie:
  aws autoscaling describe-instance-refreshes \
    --auto-scaling-group-name my-asg
  # Status: InProgress/Successful/Failed/Cancelling/Cancelled/RollbackInProgress
```

### Termination Policies — Które Instancje Usunąć

```
Domyślna kolejność termination policies:
1. AZ Balance: wybierz AZ z nadmiarową liczbą instancji
2. Allocation Strategy: preferuj termination starszych launch config/template
3. OldestLaunchTemplate: instancje z najstarszą LT wersją
4. OldestLaunchConfiguration: (legacy)
5. ClosestToNextInstanceHour: instancja której godzina billing kończy się najwcześniej
6. Default: losowa

Konfiguracja:
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --termination-policies OldestLaunchTemplate ClosestToNextInstanceHour Default

Niestandardowe (Lambda-based):
  aws autoscaling put-lifecycle-hook \
    --lifecycle-hook-name custom-termination \
    --auto-scaling-group-name my-asg \
    --lifecycle-transition autoscaling:EC2_INSTANCE_TERMINATING
  
  Lambda: wybiera którą instancję terminować na podstawie własnej logiki
          (np. najpierw Spot, potem OD; lub najpierw najdłużej działające)
```

### Scaling Policies — Głębsze Szczegóły

```
1. Simple Scaling (legacy, unikaj):
   alarm → add 2 instances
   Po akcji: cooldown (300s) → brak kolejnych akcji
   Problem: jeden step, brak granularności, cooldown blokuje
   
2. Step Scaling:
   Kilka kroków w zależności od wartości metryki:
   CPU 60-70%:  +1 instancja
   CPU 70-80%:  +2 instancje
   CPU 80-90%:  +4 instancje
   CPU > 90%:   +8 instancji
   
   Zaleta: szybsza reakcja na duże skoki
   
3. Target Tracking Scaling (NAJLEPSZE dla większości przypadków):
   Utrzymuj metrykę na docelowym poziomie
   
   Wbudowane metryki:
     ASGAverageCPUUtilization
     ASGAverageNetworkIn
     ASGAverageNetworkOut
     ALBRequestCountPerTarget  ← dla za ALB
   
   aws autoscaling put-scaling-policy \
     --auto-scaling-group-name my-asg \
     --policy-type TargetTrackingScaling \
     --target-tracking-configuration '{
       "PredefinedMetricSpecification": {
         "PredefinedMetricType": "ALBRequestCountPerTarget",
         "ResourceLabel": "app/my-alb/xxx/targetgroup/my-tg/yyy"
       },
       "TargetValue": 1000,             ← utrzymuj 1000 req/s per instancja
       "ScaleOutCooldown": 60,
       "ScaleInCooldown": 300,
       "DisableScaleIn": false
     }'
   
   Custom metric (np. SQS Queue Depth):
   aws autoscaling put-scaling-policy \
     --policy-type TargetTrackingScaling \
     --target-tracking-configuration '{
       "CustomizedMetricSpecification": {
         "MetricName": "ApproximateNumberOfMessagesVisible",
         "Namespace": "AWS/SQS",
         "Dimensions": [{"Name": "QueueName", "Value": "my-queue"}],
         "Statistic": "Average"
       },
       "TargetValue": 10             ← utrzymuj 10 wiadomości per instancja
     }'
   
4. Predictive Scaling:
   Machine learning analizuje historię 14 dni
   Prognozuje przyszłe zapotrzebowanie
   Pre-scales PRZED spodziewanym spikiem
   
   Mode: forecast_only (tylko prognoza, brak akcji) lub forecast_and_scale
   
   aws autoscaling put-scaling-policy \
     --policy-type PredictiveScaling \
     --predictive-scaling-configuration '{
       "MetricSpecifications": [{
         "TargetValue": 70,
         "PredefinedMetricPairSpecification": {
           "PredefinedMetricType": "ASGCPUUtilization"
         }
       }],
       "Mode": "ForecastAndScale",
       "SchedulingBufferTime": 300   ← scale-out 5 min przed prognozą
     }'

5. Scheduled Scaling:
   Znane okna wysokiego obciążenia:
   
   aws autoscaling put-scheduled-update-group-action \
     --auto-scaling-group-name my-asg \
     --scheduled-action-name "business-hours-up" \
     --recurrence "0 8 * * MON-FRI" \     ← cron: pon-pt o 8:00
     --min-size 10 \
     --desired-capacity 15
   
   aws autoscaling put-scheduled-update-group-action \
     --scheduled-action-name "business-hours-down" \
     --recurrence "0 18 * * MON-FRI" \    ← pon-pt o 18:00
     --min-size 2 \
     --desired-capacity 5
```

---

### 21.7 ASG Mixed Instances — Advanced

```
Zaawansowana konfiguracja Mixed Instances:

InstanceRequirements (zamiast explicit list typów):
aws autoscaling create-auto-scaling-group \
  --mixed-instances-policy '{
    "LaunchTemplate": {
      "LaunchTemplateSpecification": {
        "LaunchTemplateId": "lt-0abc123",
        "Version": "$Latest"
      },
      "Overrides": [{
        "InstanceRequirements": {
          "VCpuCount": {"Min": 2, "Max": 8},
          "MemoryMiB": {"Min": 4096, "Max": 16384},
          "CpuManufacturers": ["amazon-web-services"],   ← tylko Graviton
          "InstanceGenerations": ["current"],
          "ExcludedInstanceTypes": ["t2.*", "t3.*"],     ← nie burst types
          "NetworkBandwidthGbps": {"Min": 5}
        }
      }]
    },
    "InstancesDistribution": {
      "OnDemandBaseCapacity": 2,
      "OnDemandPercentageAboveBaseCapacity": 30,
      "SpotAllocationStrategy": "price-capacity-optimized"
    }
  }'
  
# AWS dobiera typy instancji spełniające Requirements automatycznie
# Graceful: gdy jeden typ Spot niedostępny → AWS wybiera inny spełniający requirements
```

---

### 21.8 ASG Health Checks — Typy

```
EC2 Health Check (domyślny):
  ASG sprawdza status EC2 instance health (2/2 checks pass)
  Tylko hardware/network failures
  Aplikacja może nie działać ale EC2 "zdrowy" = ASG nie wykryje
  
ELB Health Check (zalecane):
  ASG sprawdza health check TG w ELB
  Jeśli target niehealthy w TG → ASG terminuje i zastępuje
  
  aws autoscaling update-auto-scaling-group \
    --auto-scaling-group-name my-asg \
    --health-check-type ELB \
    --health-check-grace-period 300    ← 300s po InService zanim ELB checks stosowane
    
Health Check Grace Period:
  Po uruchomieniu instancji: X sekund zanim ASG zacznie sprawdzać zdrowie
  Wymagane: aplikacja potrzebuje czasu na start (30-300s typowo)
  Zbyt krótki: instancja terminate zanim aplikacja zdąży się uruchomić
  Zbyt długi: niezdrowe instancje nie wykrywane przez długi czas

Custom Health Check:
  Twój kod może oznaczać instancję jako unhealthy:
  aws autoscaling set-instance-health \
    --instance-id i-0abc123 \
    --health-status Unhealthy
  → ASG terminuje i zastępuje tę instancję
```

---

### 21.9 ASG Scaling Cooldown i Warmup

```
Cooldown (dla Simple Scaling — legacy):
  Po akcji scale-out lub scale-in: X sekund bez kolejnych akcji
  Default: 300s
  Zbyt długi = wolna reakcja na ciągłe spiki
  Zbyt krótki = thrashing (ciągłe scale-out i scale-in)
  
  Target Tracking i Step Scaling: używają scaleOutCooldown / scaleInCooldown
  oddzielnie (nie jeden wspólny cooldown)
  
Instance Warmup:
  Czas po uruchomieniu instancji zanim jest liczona do średniej metryki
  Bez warmup: nowa instancja z 0% CPU obniża średnią → scale-in triggered!
  Z warmup: nowa instancja "nie liczy się" przez X sekund
  
  Default Instance Warmup = health-check-grace-period
  
Scale-In Protection:
  Konkretna instancja chroniona przed scale-in:
  aws autoscaling set-instance-protection \
    --instance-ids i-0abc123 \
    --auto-scaling-group-name my-asg \
    --protected-from-scale-in
  
  Use case: instancja przetwarza ważne zadanie — nie terminate przed zakończeniem
  Lifecycle Hook = bardziej eleganckie rozwiązanie (automatyczne)
```

---

### 21.10 Well-Architected Framework — TG, Trust Stores, ASG

| Filar | Zastosowanie |
|---|---|
| **Operational Excellence** | Target Group slow start = brak spikes po deploy. Instance Refresh = zero-downtime updates. ASG Lifecycle Hooks = graceful bootstrap/shutdown. CloudWatch alarms na HealthyHostCount, UnhealthyHostCount. |
| **Security** | Trust Stores = mTLS per ALB listener (zero-trust). SG chaining ALB→EC2. mTLS certificates revocation (CRL). Lifecycle hook = deregister z service discovery przed terminate (brak ghost services). |
| **Reliability** | ELB health check type (nie EC2). Health check grace period prawidłowy. Lifecycle hooks z timeout na graceful drain. Multi-AZ ASG. Instance Refresh `AutoRollback=true`. |
| **Performance Efficiency** | Target Tracking `ALBRequestCountPerTarget`. Predictive Scaling dla known patterns. Slow Start Mode dla JVM/cache warmup. Warm Pools dla < 1 min scale-out. |
| **Cost Optimization** | Mixed Instances Policy: Spot + OD. `ClosestToNextInstanceHour` termination policy. Scale-In agresywny po godzinach pracy (Scheduled Scaling). Deregistration delay short (30-60s zamiast 300s) dla szybszego scale-in. |
| **Sustainability** | Aggressive scale-in po godzinach. Predictive pre-scale = nie oversized baseline. Graviton instance requirements w Mixed Instances. |

---

## 22. AWS DataSync i AWS Transfer Family

> **Zakres:** Regionalny (DataSync tasks, Transfer Family endpoints są regionalne) | **Typ:** Managed data transfer services

---

### 22.1 AWS DataSync — Przegląd

**AWS DataSync** to w pełni zarządzana usługa automatycznego transferu danych pomiędzy środowiskiem on-premises a AWS lub pomiędzy usługami AWS. Wyobraź sobie DataSync jako zaawansowany `rsync` w chmurze — automatycznie wykrywa zmiany, weryfikuje integralność danych i może działać według harmonogramu.

### Kiedy używać DataSync?
- **Migracja jednorazowa** — przenoszenie danych z on-prem do AWS (S3, EFS, FSx)
- **Periodic sync** — regularne synchronizowanie danych między on-prem a AWS
- **AWS ↔ AWS** — kopiowanie danych między regionami lub usługami AWS
- **Data pipeline** — automatyczne zasilanie data lake w S3

### Jak działa DataSync?

```mermaid
flowchart LR
    subgraph OnPrem["On-Premises"]
        NFS["NFS Share\n/data/files"]
        Agent["DataSync Agent\n(VM/OVA)"]
        NFS --> Agent
    end

    subgraph AWS["AWS Cloud"]
        DSync["DataSync Service\n(Task)"]
        S3["Amazon S3"]
        EFS["Amazon EFS"]
        FSx["Amazon FSx"]
    end

    Agent -->|"TLS encrypted\nover internet/DX"| DSync
    DSync --> S3
    DSync --> EFS
    DSync --> FSx
```

### Kluczowe cechy DataSync

| Cecha | Opis |
|-------|------|
| **Prędkość** | Do 10 Gbps, 10x szybciej niż open-source tools |
| **Weryfikacja** | Checksums end-to-end (integralność danych) |
| **Szyfrowanie** | TLS w transporcie, KMS w spoczynku |
| **Scheduling** | Hourly/daily/weekly/custom cron |
| **Filtrowanie** | Include/exclude patterns (glob) |
| **Throttling** | Ograniczenie pasma (nie saturuj WAN) |
| **Metadata** | Zachowuje permissions, timestamps, ownership |
| **Incremental** | Tylko zmienione pliki po pierwszym transferze |

---

### 22.2 DataSync Agent

**Agent DataSync** to maszyna wirtualna (OVA/AMI) wdrażana w środowisku on-premises lub w innym środowisku chmurowym, która pełni rolę "mostu" między źródłem danych a usługą DataSync w AWS.

### Gdzie wdrożyć agenta?

| Scenariusz | Gdzie agent? |
|-----------|--------------|
| On-prem → AWS | VM on-premises (VMware ESXi, Hyper-V, KVM) |
| AWS Region A → Region B | Nie potrzeba agenta (AWS-to-AWS) |
| EC2 → S3 (inny region) | Nie potrzeba agenta |
| Google Cloud → AWS | VM na GCP |
| Azure → AWS | VM na Azure |

### Komendy AWS CLI — DataSync Agent

```bash
# Lista agentów
aws datasync list-agents

# Szczegóły agenta
aws datasync describe-agent --agent-arn arn:aws:datasync:us-east-1:123456789:agent/agent-xxx

# Aktywacja agenta (po wdrożeniu VM)
aws datasync create-agent \
  --activation-key "XXXXX-XXXXX-XXXXX-XXXXX-XXXXX" \
  --agent-name "OnPremAgent01" \
  --tags Key=Environment,Value=Production
```

### Agent — ważne szczegóły

- Agent komunikuje się **outbound** na port 443 (HTTPS) do AWS — nie potrzeba otwierać portów inbound
- Jeden agent obsługuje wiele tasków DataSync
- Agent może być wdrożony na **AWS Snowcone** dla środowisk bez internetu
- Agent automatycznie skaluje liczbę wątków transferu

---

### 22.3 DataSync — Źródła i Cele

### Obsługiwane lokalizacje źródłowe (Source Locations)

| Typ | Protokół/Format | Wymaga agenta? |
|-----|----------------|----------------|
| NFS share (on-prem) | NFS v3/v4 | ✅ Tak |
| SMB share (on-prem) | SMB v2/v3 | ✅ Tak |
| HDFS (Hadoop) | HDFS protocol | ✅ Tak |
| Object storage S3-compatible | S3 API | ✅ Tak |
| Amazon S3 | S3 API | ❌ Nie |
| Amazon EFS | NFS | ❌ Nie |
| FSx for Windows | SMB | ❌ Nie |
| FSx for Lustre | Lustre | ❌ Nie |
| FSx for OpenZFS | NFS | ❌ Nie |
| FSx for NetApp ONTAP | NFS/SMB | ❌ Nie |

### Obsługiwane lokalizacje docelowe (Destination Locations)

- Amazon S3 (wszystkie klasy, w tym Glacier)
- Amazon EFS
- FSx for Windows File Server
- FSx for Lustre
- FSx for OpenZFS
- FSx for NetApp ONTAP

### Komendy — tworzenie lokalizacji

```bash
# Utwórz lokalizację NFS (on-prem)
aws datasync create-location-nfs \
  --server-hostname "192.168.1.100" \
  --subdirectory "/data/exports" \
  --on-prem-config AgentArns=arn:aws:datasync:us-east-1:123:agent/agent-xxx

# Utwórz lokalizację S3
aws datasync create-location-s3 \
  --s3-bucket-arn arn:aws:s3:::my-destination-bucket \
  --subdirectory "/migrated-data" \
  --s3-config BucketAccessRoleArn=arn:aws:iam::123:role/DataSyncS3Role

# Utwórz lokalizację EFS
aws datasync create-location-efs \
  --efs-filesystem-arn arn:aws:elasticfilesystem:us-east-1:123:file-system/fs-xxx \
  --ec2-config SubnetArn=arn:aws:ec2:us-east-1:123:subnet/subnet-xxx,SecurityGroupArns=arn:aws:ec2:us-east-1:123:security-group/sg-xxx
```

---

### 22.4 DataSync — Konfiguracja i Funkcje

### Task — podstawowy obiekt DataSync

**Task** = definicja transferu: skąd → dokąd + opcje.

```bash
# Utwórz task DataSync
aws datasync create-task \
  --source-location-arn arn:aws:datasync:us-east-1:123:location/loc-xxx \
  --destination-location-arn arn:aws:datasync:us-east-1:123:location/loc-yyy \
  --name "OnPremToS3Migration" \
  --options VerifyMode=ONLY_FILES_TRANSFERRED,OverwriteMode=ALWAYS,PreserveDeletedFiles=PRESERVE \
  --schedule ScheduleExpression="cron(0 2 * * ? *)"

# Uruchom task manualnie
aws datasync start-task-execution \
  --task-arn arn:aws:datasync:us-east-1:123:task/task-xxx

# Sprawdź status wykonania
aws datasync describe-task-execution \
  --task-execution-arn arn:aws:datasync:us-east-1:123:task/task-xxx/execution/exec-xxx
```

### Opcje Task — kluczowe parametry

| Parametr | Wartości | Opis |
|---------|---------|------|
| `VerifyMode` | `ONLY_FILES_TRANSFERRED` / `ALL_FILES` / `NONE` | Weryfikacja checksums |
| `OverwriteMode` | `ALWAYS` / `NEVER` | Nadpisuj istniejące pliki |
| `PreserveDeletedFiles` | `PRESERVE` / `REMOVE` | Co z plikami usuniętymi ze źródła |
| `PreserveDevices` | `NONE` / `PRESERVE` | Zachowaj device files |
| `PosixPermissions` | `NONE` / `PRESERVE` | Zachowaj chmod/chown |
| `TransferMode` | `CHANGED` / `ALL` | Wszystkie pliki czy tylko zmienione |
| `LogLevel` | `OFF` / `BASIC` / `TRANSFER` | Poziom logowania do CloudWatch |

### Bandwidth Throttling

```bash
# Ustaw limit 100 Mbps (wartość w MB/s)
aws datasync update-task \
  --task-arn arn:aws:datasync:us-east-1:123:task/task-xxx \
  --schedule ScheduleExpression="cron(0 2 * * ? *)" \
  --options BytesPerSecond=12500000  # 100 Mbps = 12.5 MB/s
```

### DataSync — Filtrowanie plików

```bash
# Wyklucz pliki tymczasowe i logi
aws datasync create-task \
  --source-location-arn arn:aws:datasync:... \
  --destination-location-arn arn:aws:datasync:... \
  --excludes FilterType=SIMPLE_PATTERN,Value="*.tmp|*.log|/temp/*"

# Uwzględnij tylko określone rozszerzenia
  --includes FilterType=SIMPLE_PATTERN,Value="*.pdf|*.docx"
```

### Diagram: DataSync Task Lifecycle

```mermaid
stateDiagram-v2
    [*] --> QUEUED: start-task-execution
    QUEUED --> LAUNCHING: Worker allocation
    LAUNCHING --> PREPARING: Agent connects
    PREPARING --> TRANSFERRING: File discovery done
    TRANSFERRING --> VERIFYING: All files sent
    VERIFYING --> SUCCESS: Checksums OK
    VERIFYING --> ERROR: Checksum mismatch
    TRANSFERRING --> ERROR: Transfer failed
    SUCCESS --> [*]
    ERROR --> [*]
```

---

### 22.5 AWS Transfer Family — Przegląd

**AWS Transfer Family** to w pełni zarządzana usługa serwerów protokołów transferu plików (SFTP, FTPS, FTP, AS2) zintegrowana bezpośrednio z Amazon S3 lub Amazon EFS.

Wyobraź sobie Transfer Family jako "bramę protokołową" — zewnętrzni partnerzy biznesowi mogą wysyłać pliki przez znany im protokół (np. SFTP), a pliki lądują bezpośrednio w S3.

### Kluczowe przypadki użycia

| Use Case | Opis |
|---------|------|
| **B2B file exchange** | Partner wysyła faktury przez SFTP → S3 |
| **FTP → AWS migracja** | Zastąp on-prem FTP serwer |
| **EDI/B2B** | Wymiana dokumentów EDI przez AS2 |
| **Data ingestion** | Zbieranie plików od zewnętrznych dostawców |
| **Regulatory compliance** | SFTP z MFT (Managed File Transfer) wymagania |

### Transfer Family — architektura

```mermaid
flowchart LR
    subgraph External["Zewnętrzni Partnerzy"]
        Partner1["Partner A\nSFTP Client"]
        Partner2["Partner B\nFTPS Client"]
        Partner3["EDI System\nAS2"]
    end

    subgraph TF["AWS Transfer Family"]
        Endpoint["Transfer Family\nEndpoint\n(public/VPC)"]
        Auth["Authentication\nService-managed\nCustom IdP\nAD"]
    end

    subgraph Storage["AWS Storage"]
        S3["Amazon S3\n(pliki partnerów)"]
        EFS["Amazon EFS\n(shared filesystem)"]
    end

    subgraph Processing["Przetwarzanie"]
        Lambda["Lambda\n(post-upload trigger)"]
        EventBridge["EventBridge"]
    end

    Partner1 -->|SFTP:22| Endpoint
    Partner2 -->|FTPS:21/990| Endpoint
    Partner3 -->|AS2:443| Endpoint
    Endpoint --> Auth
    Endpoint --> S3
    Endpoint --> EFS
    S3 --> Lambda
    Lambda --> EventBridge
```

---

### 22.6 Transfer Family — Protokoły

### Porównanie protokołów

| Protokół | Port | Szyfrowanie | Typowe użycie | Endpoint |
|---------|------|-------------|--------------|---------|
| **SFTP** | 22 | SSH (zawsze) | Linux/Unix transfer, DevOps | Public lub VPC |
| **FTPS** | 21 (control), 990 (implicit) | TLS/SSL | Windows legacy, fintech | Public lub VPC |
| **FTP** | 21 | ❌ Brak | Legacy systems, internal only | **VPC only** |
| **AS2** | 443 | TLS + podpis | EDI, B2B, handel elektroniczny | Public lub VPC |

### SFTP — Szczegóły

```bash
# Utwórz serwer SFTP
aws transfer create-server \
  --protocols SFTP \
  --identity-provider-type SERVICE_MANAGED \
  --endpoint-type PUBLIC \
  --tags Key=Name,Value=SFTPProductionServer

# Utwórz użytkownika SFTP
aws transfer create-user \
  --server-id s-xxxxxxxxxxxx \
  --user-name partner-user \
  --role arn:aws:iam::123:role/TransferFamilyS3Role \
  --home-directory /my-bucket/partners/partner-a \
  --home-directory-type LOGICAL \
  --home-directory-mappings Entry=/,Target=/my-bucket/partners/partner-a
```

### AS2 — Applicability Statement 2

AS2 to protokół używany głównie w **EDI (Electronic Data Interchange)** — wymiana dokumentów handlowych (zamówienia, faktury) między firmami. Transfer Family AS2 obsługuje:

- **Podpisywanie wiadomości** (message signing) — gwarancja integralności
- **Szyfrowanie wiadomości** (message encryption) — poufność
- **MDN (Message Disposition Notification)** — potwierdzenie odbioru
- Synchroniczne i asynchroniczne MDN

```bash
# Utwórz serwer AS2
aws transfer create-server \
  --protocols AS2 \
  --endpoint-type VPC \
  --endpoint-details VpcId=vpc-xxx,SubnetIds=subnet-xxx,SecurityGroupIds=sg-xxx

# Utwórz profil AS2 (lokalny)
aws transfer create-profile \
  --as2-id "MYCOMPANY" \
  --profile-type LOCAL \
  --certificate-ids cert-xxx

# Utwórz profil partnera AS2
aws transfer create-profile \
  --as2-id "PARTNERCOMPANY" \
  --profile-type PARTNER \
  --certificate-ids cert-yyy

# Utwórz agreement (umowa wymiany)
aws transfer create-agreement \
  --server-id s-xxx \
  --local-profile-id p-xxx \
  --partner-profile-id p-yyy \
  --base-directory /bucket/as2-inbox \
  --access-role arn:aws:iam::123:role/TransferAS2Role
```

---

### 22.7 Transfer Family — Autentykacja i Endpointy

### Metody autentykacji

| Metoda | Opis | Protokoły |
|-------|------|-----------|
| **Service-managed** | SSH public keys (SFTP) lub passwords | SFTP, FTPS, FTP |
| **Custom IdP (Lambda)** | Lambda weryfikuje credentials w zewnętrznym systemie | Wszystkie |
| **AWS Directory Service** | AD/LDAP authentication | SFTP, FTPS, FTP |
| **AWS IAM Identity Center** | SSO integration | SFTP |

### Custom Identity Provider (Lambda)

```mermaid
sequenceDiagram
    participant Client as SFTP Client
    participant TF as Transfer Family
    participant Lambda as Lambda (IdP)
    participant DB as User Database

    Client->>TF: Connect (username + password)
    TF->>Lambda: Invoke with username/password
    Lambda->>DB: Verify credentials
    DB-->>Lambda: User record + S3 path + role
    Lambda-->>TF: AuthResponse (role, homeDir, policy)
    TF-->>Client: Auth OK / Denied
```

```bash
# Utwórz serwer z custom IdP
aws transfer create-server \
  --protocols SFTP \
  --identity-provider-type AWS_LAMBDA \
  --identity-provider-details Function=arn:aws:lambda:us-east-1:123:function:SFTPAuthFunction \
  --endpoint-type PUBLIC
```

### Typy endpointów

| Typ | Dostępność | Użycie | Koszt extra |
|-----|----------|-------|------------|
| **Public** | Internet (publiczne IP) | Zewnętrzni partnerzy | ❌ Nie |
| **VPC (internet-facing)** | Internet przez VPC + EIP | Kontrola SG/NACLs | Elastic IP |
| **VPC (internal)** | Tylko wewnątrz VPC/VPN/DX | Internal only | ❌ Nie |

### Logical Directories — mapowanie katalogów

Logical directories pozwalają mapować ścieżki widziane przez użytkownika na rzeczywiste ścieżki S3:

```bash
# User widzi /uploads, ale S3 path to /company-bucket/partners/partner-a/incoming
aws transfer create-user \
  --server-id s-xxx \
  --user-name partner-a \
  --home-directory-type LOGICAL \
  --home-directory-mappings '[
    {"Entry": "/uploads", "Target": "/company-bucket/partners/partner-a/incoming"},
    {"Entry": "/downloads", "Target": "/company-bucket/partners/partner-a/outgoing"}
  ]' \
  --role arn:aws:iam::123:role/TransferRole
```

### Workflow — przetwarzanie po uploadzie

Transfer Family Managed Workflows = lambda/decryption/tagging po każdym pliku:

```bash
# Utwórz workflow (np. decrypt + move)
aws transfer create-workflow \
  --description "Decrypt and tag uploaded files" \
  --steps '[
    {
      "Type": "DECRYPT",
      "DecryptStepDetails": {
        "Name": "DecryptPGP",
        "Type": "PGP",
        "SourceFileLocation": "${original.file}",
        "OverwriteExisting": "TRUE",
        "DestinationFileLocation": {"S3FileLocation": {"Bucket": "processed-bucket", "Key": "decrypted/"}}
      }
    },
    {
      "Type": "TAG",
      "TagStepDetails": {
        "Name": "TagFile",
        "Tags": [{"Key": "Status", "Value": "processed"}]
      }
    }
  ]'
```

---

### 22.8 Porównanie Usług Transferu Danych

### Główna tabela porównawcza

| Usługa | Co robi | Użycie | Protokół | Źródło |
|-------|---------|-------|---------|-------|
| **DataSync** | Automatyczny transfer/sync plików | Migracja, periodic sync | Własny (TLS) | On-prem, AWS |
| **Transfer Family** | Managed SFTP/FTP/AS2 server | B2B, legacy FTP | SFTP/FTPS/FTP/AS2 | Zewnętrzni klienci |
| **Storage Gateway** | Hybrid storage — ciągły dostęp | Ongoing hybrid access | NFS/SMB/iSCSI | On-prem apps |
| **S3 Replication** | S3-to-S3 automatyczna replikacja | Multi-region, backup | Internal | S3 tylko |
| **Snow Family** | Fizyczny transfer danych | Brak łącza/ogromne dane | Fizyczna paczka | On-prem |
| **Direct Connect** | Dedykowane łącze sieciowe | Stałe połączenie | Ethernet | On-prem |

### Decision Tree — którą usługę wybrać?

```mermaid
flowchart TD
    A[Potrzebuję przenieść dane do AWS] --> B{Skąd?}
    B -->|S3 do S3| C[S3 Replication]
    B -->|On-prem lub AWS-to-AWS| D{Jak dużo danych?}
    D -->|>100 TB, brak łącza| E[Snow Family]
    D -->|Mniej, jest łącze| F{Jak często?}
    F -->|Jednorazowo lub periodic| G[DataSync]
    F -->|Ciągły dostęp app on-prem| H[Storage Gateway]
    F -->|Zewnętrzni partnerzy SFTP/FTP| I[Transfer Family]
    F -->|EDI/B2B AS2| I
```

### DataSync vs Transfer Family — kluczowa różnica

| Aspekt | DataSync | Transfer Family |
|-------|---------|----------------|
| **Inicjator transferu** | AWS (scheduled task) | Zewnętrzny klient |
| **Protokół kliencki** | Brak (agent push) | SFTP/FTP/AS2 |
| **Użytkownicy** | AWS admin, automated | Zewnętrzni partnerzy |
| **Typowy use case** | Migracja, backup sync | B2B file exchange |
| **Pricing** | Per GB transferred | Per hour + per GB |

---

### 22.9 Azure i Open-Source Odpowiedniki

### Azure Odpowiedniki

| AWS | Azure | Podobieństwa | Różnice |
|-----|-------|-------------|---------|
| **DataSync** | **Azure File Sync** | Sync on-prem ↔ chmura | Azure File Sync = tylko Azure Files (SMB); DataSync = S3/EFS/FSx |
| **DataSync** | **AzCopy** | Szybki transfer do cloud | AzCopy = CLI tool, nie managed service |
| **Transfer Family SFTP** | **Azure Blob Storage SFTP** | SFTP endpoint do cloud storage | Azure SFTP = Blob only; Transfer Family = S3 lub EFS |
| **Transfer Family AS2** | **Azure Logic Apps (EDI)** | B2B/EDI | Azure = workflow-based; AWS = dedicated server |

### Open-Source Odpowiedniki

| AWS | Open-Source | Opis |
|-----|------------|------|
| DataSync | `rsync` | Podobny koncept, brak checksum verification, skalowania |
| DataSync | `rclone` | Multi-cloud sync, ale wymaga zarządzania |
| Transfer Family | `OpenSSH (sftp-server)` | SFTP server on-prem, brak integracji S3 |
| Transfer Family | `vsftpd`, `ProFTPD` | FTP servers, brak cloud storage |
| Transfer Family AS2 | `Mendelson AS2` | Open-source AS2, self-hosted |

---

### 22.10 Well-Architected Framework

| Filar | DataSync | Transfer Family |
|-------|---------|----------------|
| **Operational Excellence** | CloudWatch metrics/logs dla każdego task execution; alarmy na błędy transferu | CloudWatch logs dla każdego połączenia; audit trail w CloudTrail |
| **Security** | TLS w transporcie, KMS at rest, IAM roles dla dostępu do S3/EFS, VPC endpoints | IAM roles, SSH key management, custom IdP, VPC-only endpoint dla internal, SG kontrola |
| **Reliability** | Automatyczny retry przy błędach; checksum verification; incremental transfer | Multi-AZ endpoint (VPC); health checks wbudowane; managed service = AWS odpowiedzialność |
| **Performance Efficiency** | Parallelism automatyczny (multi-thread agent); bandwidth throttling; 10 Gbps max | Automatyczne skalowanie połączeń; Elastic IPs dla stałych IP partnerów |
| **Cost Optimization** | Pay per GB transferred; brak opłat za idle; throttle bandwidth w godzinach szczytu | Pay per hour (server) + per GB; wyłącz server gdy nieużywany; VPC internal tańszy |
| **Sustainability** | Incremental transfer = mniej danych = mniej energii | Brak idle connections = efektywne zasoby |

---

### 22.11 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity

**DataSync w multi-account:**
- DataSync może transferować dane między kontami AWS (cross-account S3 bucket policy)
- Centralne zarządzanie task'ami przez AWS Organizations

**Transfer Family centralizacja:**
- Jeden serwer Transfer Family dla wielu partnerów
- Logical directories izolują partnerów od siebie
- Custom IdP w Lambda może centralnie zarządzać userami z różnych systemów

### Domain 2: Design for New Solutions

**Nowa architektura data ingestion:**
```
Partnerzy → Transfer Family (SFTP) → S3 → Lambda (przetwarzanie) → DynamoDB/RDS
```

**Nowa architektura migracji:**
```
On-prem NFS → DataSync Agent → DataSync Task → Amazon EFS/S3
```

**Design decisions:**
- Transfer Family gdy partnerzy potrzebują standardowego protokołu (SFTP)
- DataSync gdy kontrolujesz źródło i cel (bez zewnętrznych klientów)

### Domain 3: Continuous Improvement for Existing Solutions

**Optymalizacja kosztów DataSync:**
- Włącz scheduling (transferuj nocą gdy tańsza przepustowość)
- Ustaw `TransferMode=CHANGED` (tylko zmienione pliki)
- Throttle bandwidth w godzinach peak

**Optymalizacja Transfer Family:**
- Wyłącz serwer Transfer Family poza godzinami pracy (save ~$0.30/h)
- Użyj Managed Workflows zamiast osobnych Lambda (mniej kodu)
- S3 Lifecycle policies na katalogi uploadów

### Domain 4: Accelerate Workload Migration and Modernization

**Migracja FTP on-prem → Transfer Family:**
1. Deploy Transfer Family z tym samym hostnamen (DNS CNAME)
2. Zmigruj użytkowników (import SSH keys)
3. Przekieruj ruch (zmień DNS)
4. Wyłącz stary serwer FTP

**Migracja NAS on-prem → EFS/FSx:**
1. Deploy DataSync Agent w sieci on-prem
2. Utwórz location NFS/SMB
3. Utwórz location EFS/FSx
4. Transfer initial data (pełny)
5. Uruchom periodic sync (incremental)
6. Cutover: zatrzymaj aplikacje, finalny sync, przestaw DNS/mount

---

### 22.12 Reliability & Security

### DataSync — Security

```bash
# IAM Role dla DataSync → S3 (least privilege)
# Polityka przykład:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::my-destination-bucket",
        "arn:aws:s3:::my-destination-bucket/*"
      ]
    }
  ]
}
```

**Security checklist DataSync:**
- [ ] Agent komunikuje się outbound only (port 443) — nie otwieraj inbound
- [ ] Użyj VPC endpoint dla DataSync (nie przez internet)
- [ ] Oddzielna IAM role per task (least privilege)
- [ ] Włącz CloudTrail dla DataSync API calls
- [ ] Szyfruj dane w celu (KMS)

### Transfer Family — Security

```bash
# Restrykcyjna policy dla użytkownika Transfer Family
# Użytkownik widzi tylko swój katalog
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::company-bucket/partners/${transfer:UserName}/*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::company-bucket",
      "Condition": {
        "StringLike": {"s3:prefix": "partners/${transfer:UserName}/*"}
      }
    }
  ]
}
```

**Security checklist Transfer Family:**
- [ ] SFTP: wymagaj SSH keys (nie password)
- [ ] FTP: tylko VPC-internal endpoint (nigdy public)
- [ ] VPC endpoint z Security Groups (whitelist IP partnerów)
- [ ] Logical directories = izolacja między partnerami
- [ ] Managed Workflows: scan plików (antivirus Lambda)
- [ ] CloudWatch alarmy na failed login attempts
- [ ] Rotacja SSH keys dla partnerów

### Reliability

| Aspekt | DataSync | Transfer Family |
|-------|---------|----------------|
| **HA** | Multi-AZ task execution | Multi-AZ VPC endpoint |
| **Retry** | Automatyczny retry przy błędzie | Klient ponawia połączenie |
| **Checksum** | End-to-end verification | Protokół (SFTP) weryfikuje |
| **Monitoring** | CloudWatch metrics per task | CloudWatch logs per connection |
| **SLA** | AWS managed (99.9%) | AWS managed (99.9%) |

### Limity (Limits)

| Zasób | DataSync Limit |
|-------|---------------|
| Tasks per account | 100 |
| Locations per account | 100 |
| Agents per account | 100 |
| Max file size | 5 TB (S3 limit) |
| Max bandwidth | 10 Gbps per agent |

| Zasób | Transfer Family Limit |
|-------|----------------------|
| Servers per region | 10 (soft limit) |
| Users per server | 10,000 |
| Concurrent connections | 25,000 |
| Max file size | S3/EFS limits |
| Protocols per server | 3 (SFTP+FTPS+FTP) |

---

## 23. AWS Storage Gateway

> **Hybridowy most między centrum danych a chmurą AWS** — wirtualne urządzenie (VM lub sprzęt) uruchamiane lokalnie, które udostępnia standardowe protokoły (NFS, SMB, iSCSI) i transparentnie przechowuje dane w S3, EBS lub FSx.

---

### 23.1 Przegląd i Kontekst

Storage Gateway to usługa **hybrydowego storage**, która pozwala aplikacjom on-prem używać AWS jako backendu bez zmiany protokołów. Działa jako:

- **Wirtualna maszyna** (VMware ESXi, Microsoft Hyper-V, KVM, EC2)
- **Fizyczne urządzenie sprzętowe** (Storage Gateway Hardware Appliance)

**Zakres regionalny:** Gateway wdrażasz lokalnie (on-prem lub EC2), ale łączy się z konkretnym regionem AWS. Dane trafiają do S3/EBS/FSx w wybranym regionie. Jeden gateway = jeden region (możesz mieć wiele gatewayów w różnych regionach).

**4 typy Storage Gateway:**

| Typ | Protokół | Backend AWS | Główny use case |
|-----|----------|-------------|-----------------|
| S3 File Gateway | NFS, SMB | Amazon S3 | File share → S3 |
| FSx File Gateway | SMB | FSx for Windows | Branch office shares |
| Volume Gateway | iSCSI | S3 + EBS Snapshots | Backup bloków on-prem |
| Tape Gateway (VTL) | iSCSI VTL | S3 + Glacier | Zamiana taśm fizycznych |

**Azure odpowiednik:**
- Azure File Sync (odpowiednik S3/FSx File Gateway) — synchronizuje pliki między on-prem a Azure Files
- Azure StorSimple (odpowiednik Volume Gateway) — wycofany, zastąpiony przez Azure File Sync + Backup

---

### 23.2 S3 File Gateway

### Opis

S3 File Gateway udostępnia **NFS (v3, v4.1) i SMB** jako punkt montowania on-prem. Pliki zapisywane przez użytkowników trafiają do S3 jako natywne obiekty S3 (1 plik = 1 obiekt S3). Lokalny cache trzyma najczęściej używane dane — zmniejsza latencję.

```
On-prem apps/users
       │
    NFS/SMB mount
       │
  [S3 File Gateway VM]
       │   ↑ cache (local disk)
       │
  HTTPS → Amazon S3 (bucket)
```

### Kluczowe cechy

- Pliki dostępne zarówno przez NFS/SMB (on-prem) jak i bezpośrednio przez S3 API (AWS side)
- Obsługiwane klasy S3: Standard, Standard-IA, One Zone-IA, Intelligent-Tiering (NIE Glacier bezpośrednio)
- S3 Lifecycle policies działają normalnie po stronie S3
- **Metadane plików** → metadane obiektów S3
- Integracja z S3 Versioning, Object Lock, SSE
- Jeden bucket = jeden file share (możliwe wiele share na różne buckety)

### Use cases

- Migracja file sharesów do S3 bez zmiany aplikacji
- Backup plików do S3 (taniej niż EFS)
- Dostęp do S3 z aplikacji nie obsługujących S3 API

### AWS CLI

```bash
# Lista gatewayów
aws storagegateway list-gateways

# Lista file shares na gatewayу
aws storagegateway list-file-shares --gateway-arn arn:aws:storagegateway:...

# Refresh cache (wymuś synchronizację z S3)
aws storagegateway refresh-cache --file-share-arn arn:aws:storagegateway:...
```

---

### 23.3 FSx File Gateway

### Opis

FSx File Gateway to **SMB file share** z lokalnym cache, który synchronizuje dane z **FSx for Windows File Server**. Przeznaczony dla środowisk Windows z Active Directory.

```
Branch office
Windows clients
       │
     SMB
       │
  [FSx File Gateway VM]
       │   ↑ cache (local disk, hot data)
       │
  HTTPS → FSx for Windows File Server (AWS)
```

### Kluczowe cechy

- Tylko **SMB** (brak NFS)
- Pełna integracja z **Active Directory** — użytkownicy logują się domenowymi credentials
- Obsługuje **DFS (Distributed File System)** namespaces
- Lokalny cache = niskie latencje dla hot data
- Dane w FSx = wysoka dostępność, Multi-AZ FSx

### Use cases

- Branch office → centralne dane w FSx
- Konsolidacja file sharesów z wielu biur
- Zamiana fizycznych serwerów plików Windows

---

### 23.4 Volume Gateway

### Opis

Volume Gateway udostępnia **iSCSI block storage** on-prem, synchronizując dane z S3. Dostępne dwa tryby:

### Cached Mode (Tryb z Pamięcią Podręczną)

```
On-prem server
    iSCSI attach
         │
  [Volume Gateway]
    cache (hot blocks)
         │
    S3 (primary storage — wszystkie dane)
         │
    EBS Snapshots (point-in-time backups)
```

- **Dane primarne w S3** — lokalnie tylko cache (hot blocks)
- Lokalny dysk = tylko cache, nie potrzebujesz dużo miejsca on-prem
- Dostęp do danych przez iSCSI jak normalny dysk

### Stored Mode (Tryb z Pełnym Przechowywaniem)

```
On-prem server
    iSCSI attach
         │
  [Volume Gateway]
    primary storage (wszystkie dane lokalnie)
         │
    Async upload → S3 (backup)
         │
    EBS Snapshots (tworzone ze S3 backup)
```

- **Dane primarne on-prem** — niskie latencje, pełna lokalność
- Async upload do S3 jako backup
- Z backupu S3 możesz tworzyć EBS Snapshots i montować w EC2

### Porównanie trybów Volume Gateway

| Cecha | Cached Mode | Stored Mode |
|-------|-------------|-------------|
| Dane primarne | S3 | On-prem |
| Lokalny storage | Tylko cache (mniej miejsca) | Pełny wolumin (dużo miejsca) |
| Latencja | Wyższa dla cold data | Zawsze niska |
| Backup | Snapshots z S3 | Async upload → S3 → Snapshots |
| Use case | Duże dane, mały local cache | Niska latencja + backup do AWS |

### AWS CLI

```bash
# Lista woluminów
aws storagegateway list-volumes --gateway-arn arn:aws:storagegateway:...

# Utwórz EBS Snapshot z woluminu
aws storagegateway create-snapshot \
  --volume-arn arn:aws:storagegateway:... \
  --snapshot-description "backup-2024-10-27"

# Lista snapshotów
aws storagegateway list-volume-recovery-points --gateway-arn arn:aws:storagegateway:...
```

---

### 23.5 Tape Gateway (VTL — Virtual Tape Library)

### Opis

Tape Gateway emuluje **fizyczną bibliotekę taśm (VTL)** przez iSCSI. Istniejące aplikacje backupowe (Veeam, Veritas NetBackup, Commvault, Dell EMC NetWorker) widzą go jako normalną bibliotekę taśm — **zero zmian w oprogramowaniu backupowym**.

```
Backup software (Veeam, Veritas...)
    iSCSI VTL
         │
  [Tape Gateway VM]
    Virtual Tape Shelf (VTS)
         │
    S3 (active tapes)
         │
    S3 Glacier / Glacier Deep Archive (archived tapes)
```

### Kluczowe pojęcia

| Pojęcie | Opis |
|---------|------|
| Virtual Tape | Odpowiednik fizycznej taśmy, przechowywana w S3 |
| Virtual Tape Library (VTL) | Zestaw wirtualnych napędów i slotów taśm |
| Virtual Tape Shelf (VTS) | Archiwum taśm w Glacier |
| Tape Drive | Wirtualny napęd taśmowy (widoczny dla backup SW) |
| Media Changer | Wirtualny robot taśmowy |

### Lifecycle taśmy

```
Backup job → Write to Virtual Tape (S3)
                    │
              Tape ejected → VTS (Glacier lub Glacier Deep Archive)
                    │
              Retrieve → przywróć do VTL (kilka godzin)
                    │
              Restore backup
```

### Kompatybilność oprogramowania

- Veeam Backup & Replication
- Veritas NetBackup / Backup Exec
- Commvault
- Dell EMC NetWorker / Avamar
- IBM Spectrum Protect

### AWS CLI

```bash
# Lista wirtualnych taśm
aws storagegateway list-tapes --gateway-arn arn:aws:storagegateway:...

# Archiwizuj taśmę (wyślij do Glacier)
aws storagegateway delete-tape \
  --gateway-arn arn:aws:storagegateway:... \
  --tape-arn arn:aws:storagegateway:...

# Przywróć taśmę z archiwum
aws storagegateway retrieve-tape-archive \
  --tape-arn arn:aws:storagegateway:... \
  --gateway-arn arn:aws:storagegateway:...
```

---

### 23.6 Pełne Porównanie — Wszystkie Typy

| Cecha | S3 File GW | FSx File GW | Volume GW Cached | Volume GW Stored | Tape GW |
|-------|-----------|-------------|-----------------|-----------------|---------|
| Protokół | NFS, SMB | SMB | iSCSI | iSCSI | iSCSI VTL |
| Backend AWS | S3 | FSx for Windows | S3 + EBS Snaps | S3 + EBS Snaps | S3 + Glacier |
| Cache lokalny | ✅ | ✅ | ✅ (hot blocks) | Nie (full local) | ✅ |
| AD integracja | SMB only | ✅ pełna | ❌ | ❌ | ❌ |
| Dane primarne | S3 | FSx | S3 | On-prem | S3/Glacier |
| Use case | File share → S3 | Branch office | Duże woluminy | Backup bloków | Zamiana taśm |
| Backup SW | ❌ | ❌ | ❌ | ❌ | ✅ kompatybilny |
| Windows native | Częściowo | ✅ pełna | ✅ | ✅ | ✅ |

---

### 23.7 Opcje Wdrożenia

| Platforma | Szczegóły |
|-----------|-----------|
| VMware ESXi | OVA download, minimalne wymagania: 4 vCPU, 16 GB RAM |
| Microsoft Hyper-V | VHD download |
| KVM | qcow2 image |
| Amazon EC2 | Gateway jako EC2 instance (dla testów lub disaster recovery) |
| Hardware Appliance | Gotowe urządzenie fizyczne od AWS, model SGW-HD-FX lub SGW-HD-SX |

**Hardware Appliance** — gotowy do wdrożenia sprzęt, przydatny gdy środowisko nie ma hypervisora lub potrzebujesz gwarantowanej wydajności.

---

### 23.8 Przepustowość i Latencja

- Gateway buforuje zmiany lokalnie → wysyła do AWS przez HTTPS
- Wymaga stabilnego łącza internetowego lub Direct Connect / VPN
- **Direct Connect** zalecany dla dużych transferów (Volume Gateway Stored mode)
- Bandwidth throttling: konfigurujesz okna transferu (np. tylko w nocy)

```bash
# Ustaw bandwidth throttle (w bajtach/sekundę)
aws storagegateway update-bandwidth-rate-limit \
  --gateway-arn arn:aws:storagegateway:... \
  --average-upload-rate-limit-in-bits-per-sec 10000000 \
  --average-download-rate-limit-in-bits-per-sec 10000000
```

---

### 23.9 Storage Gateway vs DataSync

| Kryterium | Storage Gateway | DataSync |
|-----------|----------------|----------|
| Cel | Ongoing hybrid access | Jednorazowa/schedulowana migracja |
| Protokół on-prem | NFS, SMB, iSCSI | Agent na serwerze |
| Tryb pracy | Ciągły (persistent) | Zadania (tasks) |
| Cache | ✅ | ❌ |
| Latencja dostępu | Niska (cache) | N/A (transfer only) |
| Kiedy używać | Aplikacje potrzebują ciągłego dostępu | Migracja danych do AWS |
| Koszty | Gateway + transfer | Per GB transfer |

**Reguła:**
- Aplikacja **czyta i pisze** do storage on-prem → **Storage Gateway**
- Chcesz **przenieść dane** jednorazowo lub cyklicznie → **DataSync**

---

### 23.10 Bezpieczeństwo i Niezawodność

### Szyfrowanie

- Dane w tranzycie: **HTTPS/TLS** zawsze
- Dane w S3: **SSE-S3** domyślnie, opcjonalnie **SSE-KMS**
- Cache lokalny: **AES-256** szyfrowanie dysku cache
- Snapshots EBS: dziedziczą szyfrowanie z KMS

### IAM i dostęp

- Gateway komunikuje się z AWS przez **IAM role** (service account)
- Nie wymaga dostępu publicznego do S3 — może używać VPC Endpoint
- CloudWatch monitoring: metryki gateway, alerty na błędy

### Wysoka dostępność

- Gateway VM → uruchom na klastrowanym hypervisorze (VMware HA)
- Hardware Appliance → brak wbudowanej HA (single point of failure)
- Volume Gateway Stored: dane primarne on-prem — backup async do S3
- Jeśli gateway padnie → możesz przywrócić z S3/Glacier do EC2

```bash
# Aktywuj nowy gateway (po odtworzeniu VM)
aws storagegateway activate-gateway \
  --activation-key <key-from-console> \
  --gateway-name my-gateway \
  --gateway-timezone GMT+1:00 \
  --gateway-region eu-west-1 \
  --gateway-type FILE_S3
```

---

### 23.11 Limity i Ograniczenia

| Zasób | Limit |
|-------|-------|
| Gatewayów na region | 10 (soft limit, zwiększalny) |
| File sharesów na gateway | 10 |
| Wirtualnych taśm na gateway | 1500 |
| Rozmiar wirtualnej taśmy | 100 GB – 5 TB |
| Woluminów Cached na gateway | 32 |
| Woluminów Stored na gateway | 32 |
| Max rozmiar woluminu Cached | 32 TB |
| Max rozmiar woluminu Stored | 16 TB |
| Cache lokalny minimalny | 150 GB |

---

### 23.12 Well-Architected Framework

| Filar | Jak Storage Gateway wspiera |
|-------|---------------------------|
| **Operational Excellence** | CloudWatch metrics, CloudTrail auditing, automated snapshots |
| **Security** | TLS in transit, SSE-KMS at rest, IAM role-based access, VPC Endpoints |
| **Reliability** | Async replication do S3 = durable backup; możliwość restore do EC2; Multi-AZ FSx dla FSx GW |
| **Performance Efficiency** | Lokalny cache = niska latencja dla hot data; bandwidth throttling; Direct Connect dla dużych transferów |
| **Cost Optimization** | S3 Lifecycle policies na danych z File GW; Glacier dla archiwum taśm; płacisz tylko za używany storage S3 |
| **Sustainability** | Eliminacja fizycznych taśm; konsolidacja storage do AWS; mniejsza infrastruktura on-prem |

---

### 23.13 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity
- **Multi-account backup:** Volume Gateway Stored mode + cross-account S3 bucket replication
- **Centralizacja file sharesów:** FSx File Gateway w branch offices → centralny FSx w AWS
- **Compliance archiwizacja:** Tape Gateway → Glacier Deep Archive dla długoterminowego przechowywania

### Domain 2: Design for New Solutions
- **Hybrid cloud storage:** S3 File Gateway jako NAS replacement — aplikacje nie wymagają zmiany
- **Modernizacja backupu taśmowego:** Tape Gateway = zero zmian w backup software, dane w chmurze
- **Block storage backup:** Volume Gateway Cached mode dla serwerów wymagających iSCSI

### Domain 3: Continuous Improvement for Existing Solutions
- **Zmniejszenie kosztów storage:** Migracja hot data do cache, cold data do S3/Glacier
- **Monitorowanie:** CloudWatch → alerty na błędy synchronizacji, cache hit ratio
- **Bandwidth throttling:** Optymalizacja transferów w godzinach pracy vs nocnych okienek

### Domain 4: Accelerate Workload Migration and Modernization
- **Lift and shift:** Aplikacje używające NFS/SMB/iSCSI → bez zmian kodu, dane w S3
- **Tape to cloud:** Tape Gateway jako pierwszy krok → potem DataSync do pełnej migracji
- **DataSync po migracji:** Po zakończeniu migracji zastąp Storage Gateway natywnym S3/EFS

---

## 24. Amazon EBS — Elastic Block Store

> **EBS** (Elastic Block Store) to trwały, sieciowy dysk blokowy dla instancji EC2. Odpowiednik dysku twardego w chmurze — dane przeżywają restart instancji, ale dysk jest przywiązany do jednej **Availability Zone**.

---

### 24.1 Przegląd i Kontekst

### 🧠 Czym jest EBS?

EBS to **block storage** — widoczny dla OS jako `/dev/xvda`, `C:\` itp. Możesz formatować go dowolnym systemem plików, instalować aplikacje, bazy danych.

**Kluczowe cechy:**
- **Trwały** — dane przeżywają stop/start EC2 (w odróżnieniu od Instance Store)
- **AZ-scoped** — dysk istnieje w jednej AZ; EC2 musi być w tej samej AZ
- **Sieciowy** — podłączony przez sieć AWS (nie fizycznie wewnątrz serwera)
- **Jeden do jednego** (domyślnie) — jeden wolumin → jedna instancja EC2
- **Elastic Volumes** — zmień rozmiar/typ/IOPS bez odłączania

### 🌍 Gdzie regionalny/zonalny?

| Zasięg | EBS |
|--------|-----|
| Globalny | ❌ |
| Regionalny | ❌ |
| **Zonal (AZ)** | ✅ — przypisany do konkretnej AZ |

> Wolumin EBS w `eu-west-1a` **nie może** być podłączony do EC2 w `eu-west-1b`.

### ☁️ Porównanie z Azure

| | Amazon EBS | Azure Managed Disks |
|--|------------|---------------------|
| Typ | Block storage | Block storage |
| Zasięg | AZ | Zone (Availability Zone) |
| Typy | gp2/gp3/io1/io2/st1/sc1 | Standard HDD/SSD, Premium SSD, Ultra Disk |
| Multi-Attach | io1/io2 (do 16 inst.) | Ultra Disk, Premium SSD v2 |
| Snapshots | Tak, incremental | Tak, incremental |
| Szyfrowanie | KMS, AES-256 | SSE z Platform/Customer key |
| Odpowiednik gp3 | gp3 | Premium SSD v2 |
| Odpowiednik io2 | io2 Block Express | Ultra Disk |

---

### 24.2 Typy Woluminów EBS

```mermaid
graph TD
    EBS[EBS Volume Types]
    EBS --> SSD[SSD - IOPS-intensive]
    EBS --> HDD[HDD - Throughput-intensive]
    SSD --> GP2[gp2 - General Purpose]
    SSD --> GP3[gp3 - General Purpose v2]
    SSD --> IO1[io1 - Provisioned IOPS]
    SSD --> IO2[io2 - Provisioned IOPS v2]
    HDD --> ST1[st1 - Throughput Optimized]
    HDD --> SC1[sc1 - Cold HDD]
```

### 📊 Tabela porównawcza wszystkich typów

| Cecha | gp2 | gp3 | io1 | io2 | st1 | sc1 |
|-------|-----|-----|-----|-----|-----|-----|
| **Typ** | SSD | SSD | SSD | SSD | HDD | HDD |
| **Rozmiar** | 1 GiB–16 TiB | 1 GiB–16 TiB | 4 GiB–16 TiB | 4 GiB–64 TiB | 125 GiB–16 TiB | 125 GiB–16 TiB |
| **Max IOPS** | 16 000 | 16 000 | 64 000 | 256 000 (Block Express) | 500 | 250 |
| **IOPS baseline** | 3 IOPS/GiB (min 100) | 3 000 (niezależne) | Provisioned | Provisioned | N/A | N/A |
| **Max throughput** | 250 MiB/s | 1 000 MiB/s | 1 000 MiB/s | 4 000 MiB/s | 500 MiB/s | 250 MiB/s |
| **Burst** | Tak (≤1 TiB) | Nie (stałe 3000) | Nie | Nie | Tak | Tak |
| **Multi-Attach** | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **Boot volume** | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| **Latencja** | ms | ms | sub-ms | sub-ms | ms | ms |
| **Cena** | $$ | $ (tańszy niż gp2) | $$$ | $$$$ | $ | $$ |
| **Użycie** | Ogólne, boot | Ogólne, bazy danych | OLTP, duże bazy | Mission-critical DB | Big data, Kafka | Archiwizacja, backup |

### 🔍 gp2 vs gp3 — kluczowe różnice

| | gp2 | gp3 |
|--|-----|-----|
| IOPS baseline | 3 × rozmiar GiB | **3 000 stałe** (niezależnie od rozmiaru) |
| Burst do | 3 000 IOPS (dla <1 TiB) | N/A — zawsze 3 000 |
| Max IOPS | 16 000 | 16 000 (provisioned extra) |
| Throughput | 250 MiB/s | **1 000 MiB/s** |
| Cena | baseline | **20% tańszy** niż gp2 |
| Niezależne IOPS | ❌ — zależy od rozmiaru | ✅ — ustaw osobno |

> **SAP-C02 pułapka:** gp2 ma burst oparty o "credit bucket". Mały dysk gp2 (np. 10 GiB = 30 IOPS baseline) wyczerpie kredyty przy długotrwałym obciążeniu. gp3 tego nie ma — zawsze 3 000 IOPS.

### 🔍 io1 vs io2

| | io1 | io2 |
|--|-----|-----|
| Max IOPS | 64 000 | 256 000 (Block Express) |
| Durability | 99.8–99.9% | **99.999%** |
| IOPS:GiB ratio | 50:1 | **500:1** |
| Multi-Attach | ✅ | ✅ |
| Sub-ms latency | ✅ | ✅ |
| Cena | $$$ | $$$$ |

> io2 Block Express wymaga **Nitro-based** instancji EC2.

---

### 24.3 EBS Multi-Attach

**Tylko io1 i io2** — jeden wolumin podłączony do **do 16 instancji EC2** jednocześnie, w **tej samej AZ**.

```
┌─────────────────────┐
│   io2 EBS Volume    │
│   (single AZ)       │
└──────┬──────┬───────┘
       │      │
    EC2-1   EC2-2   ... EC2-16
    (same AZ only)
```

**Wymagania:**
- Cluster-aware filesystem (np. GFS2, OCFS2) — **nie ext4, xfs**
- Aplikacja musi zarządzać concurrent writes
- Użycie: Oracle RAC, SAP HANA clustering, high-availability databases

```bash
# Sprawdź Multi-Attach status
aws ec2 describe-volumes \
  --volume-ids vol-0abc123 \
  --query 'Volumes[].MultiAttachEnabled'
```

---

### 24.4 EBS Snapshots

### Jak działają snapshots

**Incremental** — pierwszy snapshot = pełna kopia, każdy kolejny = tylko zmienione bloki.

```mermaid
sequenceDiagram
    participant EC2
    participant EBS
    participant S3 as S3 (managed by AWS)
    EC2->>EBS: Write data
    EBS->>S3: Snapshot 1 (full, 100% blocks)
    EC2->>EBS: Write more data
    EBS->>S3: Snapshot 2 (only changed blocks)
    EC2->>EBS: Write more data
    EBS->>S3: Snapshot 3 (only changed blocks)
```

> Snapshots przechowywane w S3 (zarządzanym przez AWS — nie widocznym w twoim buckecie).

### Operacje na snapshotach

```bash
# Utwórz snapshot
aws ec2 create-snapshot \
  --volume-id vol-0abc123 \
  --description "MyApp backup"

# Skopiuj snapshot do innego regionu
aws ec2 copy-snapshot \
  --source-region eu-west-1 \
  --source-snapshot-id snap-0abc123 \
  --destination-region us-east-1 \
  --encrypted

# Udostępnij snapshot innemu kontu AWS
aws ec2 modify-snapshot-attribute \
  --snapshot-id snap-0abc123 \
  --attribute createVolumePermission \
  --operation-type add \
  --user-ids 123456789012

# Utwórz wolumin ze snapshotu
aws ec2 create-volume \
  --snapshot-id snap-0abc123 \
  --availability-zone eu-west-1a \
  --volume-type gp3
```

### Fast Snapshot Restore (FSR)

Normalnie wolumin ze snapshotu ma **lazy loading** — pierwsze odczyty wolne (dane ładowane z S3). FSR eliminuje to przez pre-warming.

| | Bez FSR | Z FSR |
|--|---------|-------|
| First read latency | Wysoka (lazy load) | Niska (pre-warmed) |
| Koszt | Brak extra | $$ za snapshot × AZ |
| Użycie | Standardowe | Krytyczne systemy, szybki DR |

```bash
# Włącz FSR
aws ec2 enable-fast-snapshot-restores \
  --availability-zones eu-west-1a eu-west-1b \
  --source-snapshot-ids snap-0abc123
```

### Snapshot Archive Tier

Przenieś snapshot do tańszego tier (cold storage) — 75% tańszy, ale restore trwa 24–72 godziny.

```bash
# Archiwizuj snapshot
aws ec2 modify-snapshot-tier \
  --snapshot-id snap-0abc123 \
  --storage-tier archive

# Przywróć z archiwum (tymczasowo)
aws ec2 restore-snapshot-tier \
  --snapshot-id snap-0abc123 \
  --temporary-restore-days 7
```

---

### 24.5 EBS Encryption

### Co jest szyfrowane?

- ✅ Data at rest (na dysku)
- ✅ Data in transit (między EC2 a EBS, przez sieć AWS)
- ✅ Snapshots (automatycznie jeśli wolumin zaszyfrowany)
- ✅ Woluminy tworzone ze zaszyfrowanych snapshotów

### Jak działa

```
EC2 (Nitro) ──encrypted──▶ EBS Volume
                 AES-256
                 KMS key (aws/ebs lub Customer Managed Key)
```

**Klucze:**
- `aws/ebs` — AWS managed key (domyślny)
- Customer Managed Key (CMK) — twój klucz w KMS, rotacja, audyt

```bash
# Stwórz zaszyfrowany wolumin (domyślny klucz)
aws ec2 create-volume \
  --availability-zone eu-west-1a \
  --size 100 \
  --volume-type gp3 \
  --encrypted

# Z własnym kluczem KMS
aws ec2 create-volume \
  --availability-zone eu-west-1a \
  --size 100 \
  --volume-type gp3 \
  --encrypted \
  --kms-key-id arn:aws:kms:eu-west-1:123456:key/mrk-abc123

# Włącz domyślne szyfrowanie EBS w regionie
aws ec2 enable-ebs-encryption-by-default \
  --region eu-west-1
```

### Jak zaszyfrować istniejący niezaszyfrowany wolumin

> EBS **nie pozwala** bezpośrednio szyfrować istniejącego woluminu. Procedura:

```mermaid
flowchart LR
    A[Unencrypted Volume] -->|Create snapshot| B[Unencrypted Snapshot]
    B -->|Copy with encryption| C[Encrypted Snapshot]
    C -->|Create volume| D[Encrypted Volume]
    D -->|Attach to EC2| E[EC2 with encrypted volume]
```

```bash
# 1. Snapshot niezaszyfrowanego woluminu
aws ec2 create-snapshot --volume-id vol-unencrypted --description "pre-encrypt"

# 2. Kopiuj snapshot z szyfrowaniem
aws ec2 copy-snapshot \
  --source-region eu-west-1 \
  --source-snapshot-id snap-unencrypted \
  --encrypted \
  --kms-key-id alias/my-key \
  --destination-region eu-west-1

# 3. Stwórz nowy zaszyfrowany wolumin
aws ec2 create-volume \
  --snapshot-id snap-encrypted \
  --availability-zone eu-west-1a \
  --volume-type gp3
```

---

### 24.6 EBS vs Instance Store

| Cecha | EBS | Instance Store |
|-------|-----|----------------|
| **Trwałość** | Trwały (przeżywa stop/start) | **Efemeryczny** (dane giną przy stop/terminate) |
| **Typ** | Sieciowy (network-attached) | Fizycznie w serwerze |
| **Latencja** | ms (sieć) | **Niższa** (lokalny dysk) |
| **IOPS** | Do 256 000 (io2) | Bardzo wysokie (NVMe local) |
| **Backup** | Snapshots | Brak natywnego |
| **Cena** | Płacisz za GB | **Wliczone** w cenę instancji |
| **Rozmiar** | Do 64 TiB (io2) | Zależny od typu instancji |
| **Użycie** | Bazy danych, OS, dane trwałe | Cache, bufor, temp data, Hadoop HDFS |
| **Stop EC2** | Dane przeżywają | **Dane giną** |
| **Terminate EC2** | Opcja DeleteOnTermination | Dane zawsze giną |

> **Instance Store** najlepszy do: buffer/cache/scratch data, Hadoop intermediate data, aplikacje replikujące dane same (Kafka, Cassandra).

---

### 24.7 EBS Performance

### IOPS vs Throughput

| Pojęcie | Opis | Kiedy ważne |
|---------|------|-------------|
| **IOPS** | Operacje I/O na sekundę | Bazy danych, random access |
| **Throughput** | MB/s transferu | Streaming, big data, sequential |
| **Latency** | Czas odpowiedzi | OLTP, trading systems |

### Queue Depth

Wydajność EBS rośnie z **queue depth** (liczba równoczesnych requestów):
- Queue depth = 1: IOPS wykorzystane ~30–40%
- Queue depth = 32+: pełne IOPS

### Nitro Instances

io2 Block Express (256 000 IOPS) wymaga **Nitro-based** EC2 (C5, M5, R5, I3en, X2iedn, etc.).

```bash
# Sprawdź wydajność woluminu
aws ec2 describe-volumes \
  --volume-ids vol-0abc123 \
  --query 'Volumes[].{IOPS:Iops,Throughput:Throughput,Type:VolumeType}'
```

---

### 24.8 EBS Data Lifecycle Manager (DLM)

**DLM** automatyzuje tworzenie i usuwanie snapshotów EBS oraz AMI.

```mermaid
flowchart TD
    DLM[DLM Policy] -->|Trigger: schedule| Snapshot[Create Snapshot]
    Snapshot -->|Retention policy| Old[Delete old snapshots]
    DLM -->|Tags filter| EBS[Target: EBS volumes with tag]
```

```bash
# Utwórz politykę DLM
aws dlm create-lifecycle-policy \
  --description "Daily EBS backup" \
  --state ENABLED \
  --execution-role-arn arn:aws:iam::123456:role/AWSDataLifecycleManagerDefaultRole \
  --policy-details '{
    "PolicyType": "EBS_SNAPSHOT_MANAGEMENT",
    "ResourceTypes": ["VOLUME"],
    "TargetTags": [{"Key": "Backup", "Value": "true"}],
    "Schedules": [{
      "Name": "DailySnapshots",
      "CreateRule": {"Interval": 24, "IntervalUnit": "HOURS", "Times": ["03:00"]},
      "RetainRule": {"Count": 7},
      "CopyTags": true
    }]
  }'

# Listuj polityki
aws dlm get-lifecycle-policies
```

**DLM vs AWS Backup:**
| | DLM | AWS Backup |
|--|-----|-----------|
| Zakres | Tylko EBS + AMI | Multi-service (RDS, EFS, S3...) |
| Cross-account | Tak | Tak |
| Cross-region | Tak | Tak |
| Centralne zarządzanie | Ograniczone | ✅ Full |
| Compliance vault | ❌ | ✅ |

---

### 24.9 EBS Elastic Volumes

Możesz **bez odłączania** woluminu zmienić:
- Rozmiar (tylko zwiększyć, nie zmniejszyć)
- Typ (np. gp2 → gp3)
- IOPS (dla io1/io2/gp3)
- Throughput (dla gp3)

```bash
# Zwiększ rozmiar i zmień typ
aws ec2 modify-volume \
  --volume-id vol-0abc123 \
  --size 200 \
  --volume-type gp3 \
  --iops 6000 \
  --throughput 500

# Sprawdź status modyfikacji
aws ec2 describe-volumes-modifications \
  --volume-ids vol-0abc123

# Po modyfikacji — rozszerz filesystem (Linux)
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1   # ext4
# lub
sudo xfs_growfs /           # xfs
```

> **Uwaga:** Po modyfikacji woluminu musi minąć min. **6 godzin** przed kolejną modyfikacją.

---

### 24.10 Ograniczenia EBS

| Limit | Wartość |
|-------|---------|
| Max rozmiar woluminu | 64 TiB (io2), 16 TiB (pozostałe) |
| Max IOPS na wolumin | 256 000 (io2 Block Express) |
| Max throughput na wolumin | 4 000 MiB/s (io2 Block Express) |
| Max woluminów na instancję | 28 (Nitro), 40 (inne) |
| Multi-Attach max instancji | 16 |
| Snapshot archiwum restore | 24–72 godziny |
| Modyfikacja woluminu cooldown | 6 godzin |
| FSR max snapshotów | 5 na AZ |

---

### 24.11 Reliability & Security

### Reliability
- EBS replikowany w ramach AZ (bez dodatkowych opłat)
- io2: **99.999%** durability (io1: 99.9%)
- Snapshots do S3 = cross-AZ durability
- Multi-AZ przez snapshots + create volume in another AZ

### Security
- Szyfrowanie domyślne per-region: `aws ec2 enable-ebs-encryption-by-default`
- KMS Customer Managed Keys — rotacja, audyt CloudTrail
- IAM policies kontrolują kto może tworzyć/usuwać woluminy
- Resource-based: snapshot sharing przez `modify-snapshot-attribute`
- VPC endpoint dla EC2 API (nie wpływa na EBS bezpośrednio — ruch EBS zawsze prywatny)

---

### 24.12 Well-Architected Framework

| Filar | EBS |
|-------|-----|
| **Operational Excellence** | DLM automatyzuje snapshoty; CloudWatch metryki (VolumeReadOps, VolumeWriteOps, BurstBalance) |
| **Security** | Szyfrowanie KMS, IAM, enable-ebs-encryption-by-default, CloudTrail dla API calls |
| **Reliability** | Replikacja w AZ, snapshots cross-region, io2 99.999% durability, Multi-Attach dla HA clustering |
| **Performance Efficiency** | gp3 (niezależne IOPS/throughput), io2 Block Express dla sub-ms, Instance Store dla lokalnego cache |
| **Cost Optimization** | gp3 tańszy niż gp2, Snapshot Archive Tier, DLM retention policies, Elastic Volumes zamiast nowych dysków |
| **Sustainability** | Elastic Volumes zamiast tworzenia nowych woluminów; migrate gp2→gp3 (efektywniejsze) |

---

### 24.13 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity
- Multi-account snapshot sharing (`modify-snapshot-attribute --user-ids`)
- Cross-region snapshot copy dla DR
- Resource Access Manager (RAM) nie obsługuje EBS bezpośrednio — użyj snapshot sharing

### Domain 2: Design for New Solutions
- gp3 jako domyślny wybór (tańszy, lepszy niż gp2)
- io2 Block Express dla Oracle RAC, SAP HANA
- Multi-Attach + cluster filesystem dla shared storage
- Instance Store dla ephemeral workloads (Spark, Hadoop)

### Domain 3: Continuous Improvement for Existing Solutions
- Migracja gp2 → gp3 (Elastic Volumes, zero downtime)
- DLM zamiast ręcznych snapshotów
- CloudWatch BurstBalance alarm (gp2 burst wyczerpanie)
- Snapshot Archive dla starych backupów

### Domain 4: Accelerate Workload Migration and Modernization
- AWS Application Migration Service (MGN) używa EBS internally
- CloudEndure → MGN → replika EBS w target regionie
- Database Migration Service (DMS) + EBS jako landing zone

---

## 25. Amazon EFS — Elastic File System

> **Zakres:** Regionalny (multi-AZ) | **Protokół:** NFSv4.1/4.0 | **OS:** Linux only | **Skalowanie:** Automatyczne (petabyte-scale)

### 25.1 Przegląd i kontekst

Amazon EFS (Elastic File System) to w pełni zarządzany, elastyczny system plików NFS dla instancji EC2, kontenerów i funkcji Lambda działających w AWS. W odróżnieniu od EBS (blokowy, jeden EC2) czy S3 (obiektowy), EFS oferuje **współdzielony dostęp do systemu plików** dla wielu instancji jednocześnie — jak serwer NFS w tradycyjnym datacenter, tylko bez zarządzania infrastrukturą.

**Kluczowe cechy:**
- Automatycznie rośnie i maleje — płacisz za faktycznie zajęte miejsce
- Multi-AZ — dane replikowane w kilku strefach dostępności
- Obsługuje tysiące równoczesnych połączeń
- Standard POSIX — działa jak zwykły filesystem

**Odpowiednik open-source/Linux:** Serwer NFS (`nfs-kernel-server`) + montowanie przez klientów NFS  
**Odpowiednik Azure:** Azure Files (SMB/NFS), Azure NetApp Files (premium NFS)

```bash
# Utwórz EFS filesystem
aws efs create-file-system \
  --performance-mode generalPurpose \
  --throughput-mode elastic \
  --encrypted \
  --tags Key=Name,Value=MyEFS

# Utwórz Mount Target w subnecie (jeden per AZ)
aws efs create-mount-target \
  --file-system-id fs-12345678 \
  --subnet-id subnet-abc123 \
  --security-groups sg-xyz789

# Zamontuj na EC2 (wymagane: amazon-efs-utils)
sudo mount -t efs -o tls fs-12345678:/ /mnt/efs
# lub przez DNS:
sudo mount -t nfs4 -o nfsvers=4.1,tls \
  fs-12345678.efs.eu-west-1.amazonaws.com:/ /mnt/efs
```

---

### 25.2 EFS vs EBS vs S3 — Porównanie

| Cecha | EFS | EBS | S3 |
|-------|-----|-----|----|
| **Typ storage** | File (NFS) | Block | Object |
| **Protokół** | NFSv4.1/4.0 | iSCSI (wewnętrznie) | HTTPS REST API |
| **Dostęp** | Wiele EC2 jednocześnie | Jeden EC2 (Multi-Attach: do 16) | Dowolnie (API, CLI, SDK) |
| **Zakres** | Regionalny (multi-AZ) | Zonal (jedna AZ) | Regionalny (globalny z replikacją) |
| **OS** | Linux only | Linux + Windows | Dowolny (API) |
| **Elastyczność** | Auto (petabyte-scale) | Ręczna zmiana rozmiaru | Auto (nieograniczony) |
| **Latencja** | ms (sub-ms w GP mode) | sub-ms (SSD) | ms–s (sieć) |
| **IOPS** | do 35k (GP) / wyższe (MaxI/O) | do 256k (io2) | brak pojęcia IOPS |
| **Cena (za GB/mies.)** | ~$0.30 (Standard) | ~$0.10 (gp3) | ~$0.023 (Standard) |
| **Typowy use case** | CMS, web serving, shared config | Bazy danych, OS, boot | Backup, archiwum, static assets |
| **Backup** | AWS Backup | Snapshots | Versioning, Replication |

---

### 25.3 EFS Performance Modes

Wybierany przy tworzeniu — **nie można zmienić po utworzeniu**.

| Mode | Opis | Max IOPS | Latencja | Kiedy używać |
|------|------|----------|----------|--------------|
| **General Purpose** (default) | Zoptymalizowany pod latencję | 35,000 | sub-ms | Web serving, CMS, home dirs, dev |
| **Max I/O** | Wyższy throughput/IOPS, wyższa latencja | >500,000 | ms | Big data, media processing, HPC, tysiące EC2 równolegle |

> ⚠️ **Egzamin:** Większość workloadów = General Purpose. Max I/O tylko gdy tysiące instancji równolegle i akceptujesz wyższą latencję.

```bash
# Sprawdź performance mode
aws efs describe-file-systems \
  --file-system-id fs-12345678 \
  --query 'FileSystems[].PerformanceMode'
```

---

### 25.4 EFS Throughput Modes

Można zmienić w dowolnym momencie (w odróżnieniu od Performance Mode).

| Mode | Opis | Baseline | Burst | Kiedy używać |
|------|------|----------|-------|--------------|
| **Bursting** | Throughput skaluje się ze storage | 50 KiB/s per GB | Do 100 MiB/s (< 1TB) lub 100 MiB/s per TB | Małe/średnie systemy z nieprzewidywalnym ruchem |
| **Provisioned** | Stały throughput niezależnie od rozmiaru | Ustalasz (np. 1 GiB/s) | Brak (stały) | Workloady wymagające stałego wysokiego throughput |
| **Elastic** (nowość) | Auto-skalowanie, brak limitów | Auto | Auto (do 3 GiB/s read, 1 GiB/s write) | **Zalecany default** — nieprzewidywalne workloady |

```bash
# Zmień na Elastic throughput
aws efs update-file-system \
  --file-system-id fs-12345678 \
  --throughput-mode elastic

# Zmień na Provisioned (1 GiB/s)
aws efs update-file-system \
  --file-system-id fs-12345678 \
  --throughput-mode provisioned \
  --provisioned-throughput-in-mibps 1024
```

> ⚠️ **Egzamin:** Bursting = throughput zależny od rozmiaru filesystem. Mały EFS (<1TB) = mały burst. Elastic = brak tego problemu, ale wyższy koszt.

---

### 25.5 EFS Storage Classes i Lifecycle Management

### Klasy storage

| Klasa | Opis | Dostępność | Koszt (US) | Retrieval fee |
|-------|------|-----------|-----------|---------------|
| **Standard** | Często używane dane | Multi-AZ | ~$0.30/GB | Brak |
| **Standard-IA** | Rzadko używane, multi-AZ | Multi-AZ | ~$0.025/GB | $0.01/GB |
| **One Zone** | Często używane, jedna AZ | Jedna AZ | ~$0.17/GB | Brak |
| **One Zone-IA** | Rzadko używane, jedna AZ | Jedna AZ | ~$0.013/GB | $0.01/GB |

> ⚠️ **One Zone** — tańsze ~47%, ale ryzyko utraty danych przy awarii AZ. Dobre dla dev/test.

### Lifecycle Management

EFS automatycznie przenosi pliki do IA po X dniach bezczynności:

```bash
# Włącz lifecycle — przenoś do IA po 30 dniach
aws efs put-lifecycle-configuration \
  --file-system-id fs-12345678 \
  --lifecycle-policies \
    TransitionToIA=AFTER_30_DAYS \
    TransitionToPrimaryStorageClass=AFTER_1_ACCESS
```

Opcje `TransitionToIA`: 7, 14, 30, 60, 90 dni  
`TransitionToPrimaryStorageClass=AFTER_1_ACCESS` — przenieś z powrotem do Standard przy pierwszym odczycie.

```mermaid
flowchart LR
    A[Nowy plik → Standard] -->|30 dni bez dostępu| B[Standard-IA]
    B -->|Odczyt pliku| C[Standard ponownie]
    style A fill:#4CAF50,color:#fff
    style B fill:#FF9800,color:#fff
    style C fill:#4CAF50,color:#fff
```

---

### 25.6 EFS Access Points

Access Points = logiczne punkty wejścia do EFS dla konkretnych aplikacji. Każdy Access Point może:
- Wymuszać konkretny katalog root (np. `/app1/data`)
- Wymuszać konkretne UID/GID dla dostępu POSIX
- Ograniczać dostęp przez IAM

```bash
# Utwórz Access Point
aws efs create-access-point \
  --file-system-id fs-12345678 \
  --posix-user Uid=1000,Gid=1000 \
  --root-directory \
    Path=/app1/data,CreationInfo="{OwnerUid=1000,OwnerGid=1000,Permissions=755}"

# Montuj przez Access Point
sudo mount -t efs -o tls,accesspoint=fsap-abc123 \
  fs-12345678:/ /mnt/app1
```

**Use case:** Wiele aplikacji na jednym EFS, każda widzi tylko swój katalog. Idealne dla ECS/EKS gdzie wiele kontenerów dzieli jeden EFS.

---

### 25.7 EFS Security

### Warstwy bezpieczeństwa

```mermaid
flowchart TD
    EC2[EC2 / Lambda / Container] --> SG[Security Group\nport 2049 NFS]
    SG --> MT[Mount Target]
    MT --> EFS[EFS Filesystem]
    EFS --> KMS[KMS Encryption\nat rest]
    EC2 -->|TLS| MT
    IAM[IAM Policy\nefs:ClientMount\nefs:ClientWrite] --> EFS
    style KMS fill:#FF5722,color:#fff
    style IAM fill:#9C27B0,color:#fff
```

| Warstwa | Mechanizm | Opis |
|---------|-----------|------|
| **Sieciowa** | Security Groups | Mount Target wymaga SG z TCP 2049 (NFS) |
| **Szyfrowanie in-transit** | TLS (`-o tls`) | Przez amazon-efs-utils |
| **Szyfrowanie at-rest** | KMS (AWS managed lub CMK) | Włączone przy tworzeniu, nie można wyłączyć |
| **Tożsamość** | IAM + Resource Policy | `efs:ClientMount`, `efs:ClientWrite`, `efs:ClientRootAccess` |
| **Dostęp aplikacji** | Access Points + POSIX UID/GID | Izolacja per-aplikacja |

```bash
# Przykład EFS Resource Policy — tylko szyfrowane połączenia
aws efs put-file-system-policy \
  --file-system-id fs-12345678 \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [{
      "Effect": "Deny",
      "Principal": {"AWS": "*"},
      "Action": "*",
      "Condition": {
        "Bool": {"aws:SecureTransport": "false"}
      }
    }]
  }'
```

---

### 25.8 EFS z Lambda, ECS, EKS

### EFS + Lambda

Lambda może montować EFS — pozwala na:
- Współdzielenie danych między wywołaniami (Lambda jest bezstanowa)
- Duże modele ML, dane referencyjne
- Wymaga VPC Lambda + Mount Target w tej samej AZ

```bash
# Konfiguracja Lambda z EFS
aws lambda update-function-configuration \
  --function-name MyFunction \
  --file-system-configs \
    Arn=arn:aws:elasticfilesystem:eu-west-1:123456789012:access-point/fsap-abc123,\
    LocalMountPath=/mnt/data
```

### EFS + ECS/Fargate

```json
// Task Definition — Volume EFS
{
  "volumes": [{
    "name": "efs-vol",
    "efsVolumeConfiguration": {
      "fileSystemId": "fs-12345678",
      "rootDirectory": "/app",
      "transitEncryption": "ENABLED",
      "authorizationConfig": {
        "accessPointId": "fsap-abc123",
        "iam": "ENABLED"
      }
    }
  }]
}
```

### EFS + EKS

```yaml
# PersistentVolume w EKS przez EFS CSI Driver
apiVersion: v1
kind: PersistentVolume
metadata:
  name: efs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  csi:
    driver: efs.csi.aws.com
    volumeHandle: fs-12345678
```

> `ReadWriteMany` — jedyny typ storage w AWS natywnie obsługujący RWX dla Kubernetes.

---

### 25.9 EFS Cross-Region i Backup

### Cross-Region

EFS **NIE** obsługuje natywnej replikacji cross-region (inaczej niż S3). Opcje:

| Metoda | Opis | RTO/RPO |
|--------|------|---------|
| **AWS DataSync** | Scheduled sync EFS→EFS cross-region | Minuty/godziny |
| **AWS Backup** | Backup vault cross-region | Godziny |
| **rsync + EC2** | Ręcznie lub przez cron | Zależy od implementacji |

```bash
# DataSync — synchronizuj EFS między regionami
aws datasync create-task \
  --source-location-arn arn:aws:datasync:eu-west-1:...:location/loc-src \
  --destination-location-arn arn:aws:datasync:us-east-1:...:location/loc-dst \
  --name "EFS-CrossRegion-Sync"
```

### AWS Backup + EFS

```bash
# Włącz backup EFS przez AWS Backup
aws backup create-backup-plan \
  --backup-plan '{
    "BackupPlanName": "EFSDaily",
    "Rules": [{
      "RuleName": "Daily",
      "TargetBackupVaultName": "Default",
      "ScheduleExpression": "cron(0 2 * * ? *)",
      "StartWindowMinutes": 60,
      "CompletionWindowMinutes": 120,
      "Lifecycle": {
        "DeleteAfterDays": 30
      }
    }]
  }'
```

---

### 25.10 Porównanie EFS vs Azure Files vs GCP Filestore

| | Amazon EFS | Azure Files | GCP Filestore |
|-|-----------|------------|---------------|
| **Protokół** | NFSv4.1 | SMB 3.0, NFSv4.1 | NFSv3 |
| **OS** | Linux | Windows + Linux | Linux |
| **Elastyczność** | Auto | Auto | Manual tiers |
| **Multi-AZ** | Tak (default) | Tak (ZRS tier) | Nie (zonal) |
| **Min rozmiar** | 0 (auto) | 100 GiB (Standard) | 1 TiB (Basic) |
| **Cena baseline** | $0.30/GB | $0.06/GB (Standard) | $0.20/GB |
| **Performance** | GP/MaxI/O + Elastic | Standard/Premium | Basic/High Scale/Enterprise |
| **K8s RWX** | Tak (EFS CSI) | Tak (Azure Files CSI) | Tak |

---

### 25.11 Ograniczenia EFS

| Ograniczenie | Wartość |
|-------------|---------|
| Max file systems per region | 1000 |
| Max mount targets per AZ | 1 per filesystem |
| Max size pojedynczego pliku | 52 TiB |
| Max throughput (Elastic mode) | 3 GiB/s read, 1 GiB/s write |
| Max IOPS (Max I/O mode) | >500,000 |
| Obsługiwane OS | Linux only (NFSv4.1) |
| Cross-Region natywnie | NIE |
| Windows support | NIE (użyj FSx for Windows) |
| Latencja vs EBS | Wyższa (sieć vs lokalny) |

---

### 25.12 Well-Architected Framework — EFS

| Filar | Rekomendacja EFS |
|-------|-----------------|
| **Operational Excellence** | Monitoruj `PercentIOLimit` i `BurstCreditBalance` w CloudWatch; użyj Elastic mode by uniknąć zarządzania burst credits |
| **Security** | Zawsze włącz szyfrowanie at-rest (KMS) i in-transit (TLS); użyj Resource Policy by wymusić TLS; Access Points dla izolacji aplikacji |
| **Reliability** | Multi-AZ Standard class dla produkcji; AWS Backup z retention policy; unikaj One Zone dla krytycznych danych |
| **Performance Efficiency** | General Purpose + Elastic throughput dla większości; Max I/O tylko dla HPC/big data z tysiącami połączeń; monitoruj `PercentIOLimit` |
| **Cost Optimization** | Lifecycle Management → IA po 30 dniach; One Zone-IA dla dev/test; monitoruj `StorageBytes` per storage class |
| **Sustainability** | Lifecycle do IA zmniejsza pojemność aktywnego storage; eliminuje nadprowizjonowanie (auto-scaling) |

---

### 25.13 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity
- EFS jako centralny storage dla wielu VPC przez VPC Peering / Transit Gateway + Mount Targets
- Access Points dla izolacji team/aplikacji przy wielodzierżawczości
- IAM Resource Policy dla cross-account EFS access

### Domain 2: Design for New Solutions
- EFS + Fargate + EKS = shared persistent storage dla kontenerów (ReadWriteMany)
- EFS + Lambda = stateful serverless (modele ML, cache)
- EFS z Auto Scaling Group — wszystkie instancje montują ten sam filesystem

### Domain 3: Continuous Improvement for Existing Solutions
- Migracja z NFS on-prem na EFS przez DataSync
- Zmiana throughput mode z Bursting na Elastic gdy `BurstCreditBalance` wyczerpany
- Włączenie Lifecycle Management dla redukcji kosztów istniejących EFS

### Domain 4: Accelerate Workload Migration and Modernization
- DataSync: NFS on-prem → EFS (migracja lift-and-shift)
- Storage Gateway File Gateway → EFS jako cache dla on-prem dostępu do EFS
- EFS jako wspólny storage przy migracji aplikacji monolitycznych do mikrousług (ECS)

---

### 25.14 Reliability i Security — Deep Dive

### Reliability
```
Production EFS:
- Performance Mode: General Purpose
- Throughput Mode: Elastic
- Storage Class: Standard (multi-AZ)
- Backup: AWS Backup, daily, 30 dni retention
- Mount Targets: po jednym w każdej AZ (us-east-1a, 1b, 1c)
- DNS: fs-xxx.efs.region.amazonaws.com → automatyczny failover do zdrowej AZ
```

### Security Checklist
- [ ] Szyfrowanie at-rest włączone przy tworzeniu (KMS)
- [ ] TLS (`-o tls`) przy montowaniu
- [ ] Security Group Mount Target: tylko TCP 2049 z zaufanych SG
- [ ] Resource Policy: deny bez SecureTransport
- [ ] Access Points zamiast bezpośredniego dostępu root
- [ ] IAM: `efs:ClientMount` + `efs:ClientWrite` explicite

---

## 26. Amazon FSx — Managed File Systems

> **Zakres:** Regionalny (Multi-AZ lub Single-AZ zależnie od wariantu) | **Typ:** Managed file storage | **Protokoły:** SMB, NFS, Lustre, iSCSI

---

### 26.1 Przegląd FSx

**Amazon FSx** to w pełni zarządzana usługa oferująca popularne systemy plików innych firm jako usługę w chmurze. AWS zajmuje się infrastrukturą, patchwaniem, backupami i HA — ty tylko używasz systemu plików.

**Cztery warianty FSx:**

```mermaid
graph TD
    FSx[Amazon FSx] --> W[FSx for Windows\nFile Server]
    FSx --> L[FSx for Lustre]
    FSx --> N[FSx for NetApp\nONTAP]
    FSx --> Z[FSx for OpenZFS]

    W --> W1[SMB / NTFS\nWindows workloads]
    L --> L1[Parallel FS\nHPC / ML]
    N --> N1[Multi-protocol\nEnterprise NAS]
    Z --> Z1[NFS / ZFS\nLinux migration]
```

**Kiedy FSx zamiast EFS?**
- Windows workloads → FSx Windows
- HPC / ML → FSx Lustre
- On-prem NetApp migration → FSx ONTAP
- On-prem ZFS/NFS migration → FSx OpenZFS
- Linux shared storage (simpler) → EFS

---

### 26.2 FSx for Windows File Server

### 🪟 Opis

W pełni zarządzany Windows file server oparty na Windows Server. Protokół **SMB** (Server Message Block), system plików **NTFS**. Natywna integracja z Active Directory.

**Azure odpowiednik:** Azure Files (SMB, premium tier)

### Architektura

```mermaid
graph LR
    AD[Active Directory\nAWS Managed AD\nlub self-managed] --> FSxW[FSx for Windows\nFile Server]
    FSxW --> EC2W[Windows EC2]
    FSxW --> EC2L[Linux EC2\nvia SMB]
    FSxW --> WorkSpaces[Amazon WorkSpaces]
    FSxW --> S3[AWS Backup\n/ S3 snapshots]
```

### Kluczowe cechy

| Cecha | Szczegóły |
|-------|-----------|
| **Protokół** | SMB 2.0, 2.1, 3.0, 3.1.1 |
| **System plików** | NTFS |
| **AD integracja** | AWS Managed Microsoft AD lub self-managed AD |
| **DFS** | DFS Namespaces + DFS Replication |
| **Deployment** | Single-AZ lub Multi-AZ |
| **Szyfrowanie** | At-rest (KMS), in-transit (SMB encryption) |
| **Deduplication** | ✅ (data deduplication Windows feature) |
| **Shadow copies** | ✅ VSS |
| **Rozmiar** | 32 GiB – 65,536 GiB |
| **Throughput** | 8 MB/s – 2,048 MB/s |

### Single-AZ vs Multi-AZ

| | Single-AZ | Multi-AZ |
|-|-----------|----------|
| **HA** | Jeden AZ | Aktywny + standby w różnych AZ |
| **Failover** | Brak auto-failover | Automatyczny failover (~30s) |
| **Koszt** | Niższy | Wyższy |
| **Zastosowanie** | Dev/test | Produkcja |

### DFS (Distributed File System)

**DFS Namespaces** — jeden wirtualny punkt dostępu do wielu udziałów:
```
\\corp.example.com\shares → wiele serwerów/FSx filesystem
```

**DFS Replication** — replikacja plików między wieloma FSx (multi-region DR):
```
FSx (us-east-1) ←→ DFS Replication ←→ FSx (eu-west-1)
```

### AWS CLI — przykłady

```bash
# Utwórz FSx for Windows (Single-AZ)
aws fsx create-file-system \
  --file-system-type WINDOWS \
  --storage-capacity 300 \
  --storage-type SSD \
  --subnet-ids subnet-12345678 \
  --windows-configuration \
    ActiveDirectoryId=d-9067633459,\
    ThroughputCapacity=32,\
    DeploymentType=SINGLE_AZ_2

# Lista systemów plików
aws fsx describe-file-systems

# Utwórz backup
aws fsx create-backup \
  --file-system-id fs-12345678

# Zmień throughput
aws fsx update-file-system \
  --file-system-id fs-12345678 \
  --windows-configuration ThroughputCapacity=64
```

### Ograniczenia

| Ograniczenie | Wartość |
|-------------|---------|
| Maks. rozmiar FS | 65,536 GiB |
| Maks. throughput | 2,048 MB/s |
| Maks. backup retention | 90 dni |
| Protokół Linux | SMB (via Samba client) |
| Cross-region replikacja | Tylko przez DFS |

---

### 26.3 FSx for Lustre

### ⚡ Opis

**Lustre** = wysokowydajny rozproszony system plików, używany w HPC (High Performance Computing), ML/AI, genomics, financial modeling. Sub-milisekundowe opóźnienia, setki GB/s throughput.

**Open-source odpowiednik:** Lustre (oryginalny open source, używany w superkomputerach)
**Azure odpowiednik:** Azure HPC Cache

### Architektura + S3 integracja

```mermaid
graph LR
    S3[Amazon S3\n"data lake"] -->|Lazy load on first access| Lustre[FSx for Lustre\nHigh-speed cache]
    Lustre -->|Write back / export| S3
    Lustre --> EC2[EC2 Instances\nHPC cluster]
    Lustre --> EKS[EKS / Batch\nML training]
    Lustre --> SageMaker[SageMaker]
```

**S3 integracja — jak działa:**
1. Tworzysz FSx Lustre linkując do S3 bucket
2. Pliki z S3 ładowane **lazy** — dopiero gdy EC2 pierwszy raz odczyta
3. Po przetworzeniu → eksport z powrotem do S3

### Scratch vs Persistent

| | Scratch 1 / Scratch 2 | Persistent 1 / Persistent 2 |
|-|----------------------|------------------------------|
| **Cel** | Tymczasowe obliczenia | Długoterminowe, HA |
| **Replikacja** | Brak (no HA) | Tak (w ramach AZ) |
| **Durability** | Niska | Wysoka |
| **Koszt** | Niższy | Wyższy |
| **S3 export** | ✅ | ✅ |
| **Automatyczny backup** | ❌ | ✅ |
| **Zastosowanie** | Jednorazowe job-y | Ciągłe workloady |

### Kluczowe cechy

| Cecha | Wartość |
|-------|---------|
| **Protokół** | Lustre (POSIX-compliant) |
| **Latency** | Sub-milisecond |
| **Throughput** | Do 1+ TB/s |
| **Rozmiar** | 1.2 TiB – 100s TiB |
| **S3 integration** | ✅ natywna (data repository) |
| **POSIX** | ✅ |
| **Szyfrowanie** | At-rest (KMS), in-transit |

### AWS CLI — przykłady

```bash
# Utwórz FSx Lustre z S3 data repository
aws fsx create-file-system \
  --file-system-type LUSTRE \
  --storage-capacity 1200 \
  --subnet-ids subnet-12345678 \
  --lustre-configuration \
    ImportPath=s3://my-bucket/data,\
    ExportPath=s3://my-bucket/output,\
    DeploymentType=SCRATCH_2

# Export danych z Lustre do S3
aws fsx create-data-repository-task \
  --type EXPORT_TO_REPOSITORY \
  --file-system-id fs-12345678 \
  --paths /output/results

# Import nowych danych z S3
aws fsx create-data-repository-task \
  --type IMPORT_METADATA_FROM_REPOSITORY \
  --file-system-id fs-12345678
```

### Ograniczenia

| Ograniczenie | Wartość |
|-------------|---------|
| Min rozmiar | 1.2 TiB (inkrements 2.4 TiB) |
| Protokół | Tylko Lustre (nie SMB/NFS) |
| Cross-region | Tylko przez S3 jako pośrednik |
| Windows | ❌ (Linux only) |

---

### 26.4 FSx for NetApp ONTAP

### 🐧 Opis

W pełni zarządzany NetApp ONTAP — enterprise NAS z obsługą **NFS, SMB i iSCSI** jednocześnie. Idealne do migracji on-prem NetApp do AWS lub hybrydowych środowisk.

**On-prem odpowiednik:** NetApp ONTAP (fizyczne urządzenia NAS)
**Azure odpowiednik:** Azure NetApp Files

### Architektura

```mermaid
graph TD
    OnPrem[On-Premises\nNetApp ONTAP] -->|SnapMirror replication| ONTAP[FSx for NetApp ONTAP]
    ONTAP --> NFS[NFS clients\nLinux EC2]
    ONTAP --> SMB[SMB clients\nWindows EC2]
    ONTAP --> iSCSI[iSCSI clients\nblock storage]
    ONTAP --> S3tier[S3 tiering\ncold data]
    ONTAP --> Replica[Replica AZ\nMulti-AZ HA]
```

### Kluczowe cechy

| Cecha | Szczegóły |
|-------|-----------|
| **Protokoły** | NFS v3/v4, SMB 2/3, iSCSI |
| **Deployment** | Multi-AZ (aktywny + standby) |
| **Data tiering** | Hot (SSD) → Cold (S3) automatycznie |
| **Replikacja** | SnapMirror (do innego FSx ONTAP lub on-prem) |
| **Deduplication** | ✅ |
| **Kompresja** | ✅ |
| **Snapshots** | ✅ (ONTAP snapshots) |
| **Thin provisioning** | ✅ |
| **Szyfrowanie** | At-rest (KMS), in-transit |

### Storage tiering

```
Hot data (SSD) → [automatyczny tiering] → Cold data (S3)
                        ↑
                 Polityka tiering:
                 - snapshot-only
                 - auto (dane nieużywane X dni)
                 - all (wszystko na S3 oprócz metadanych)
                 - none
```

### Migracja z on-prem ONTAP

```bash
# 1. Utwórz FSx ONTAP
aws fsx create-file-system \
  --file-system-type ONTAP \
  --storage-capacity 1024 \
  --subnet-ids subnet-aza subnet-azb \
  --ontap-configuration \
    DeploymentType=MULTI_AZ_1,\
    ThroughputCapacity=128,\
    PreferredSubnetId=subnet-aza

# 2. Skonfiguruj SnapMirror z on-prem → FSx
# (wykonywane z ONTAP System Manager lub CLI ONTAP)
snapmirror create -source-path onprem:vol -dest-path fsx:vol

# 3. Po migracji — break mirror i przełącz klientów
snapmirror break -destination-path fsx:vol
```

---

### 26.5 FSx for OpenZFS

### 🐡 Opis

W pełni zarządzany system plików ZFS z protokołem **NFS**. Idealne do migracji workloadów z on-prem Linux/NFS/ZFS, gdzie zależy nam na funkcjach ZFS (snapshots, clones, kompresja).

**Open-source odpowiednik:** OpenZFS (Linux, FreeBSD)
**Azure odpowiednik:** Brak bezpośredniego odpowiednika

### Kluczowe cechy

| Cecha | Szczegóły |
|-------|-----------|
| **Protokół** | NFS v3, v4.1, v4.2 |
| **Deployment** | Single-AZ lub Multi-AZ |
| **IOPS** | Do 1,000,000 IOPS |
| **Latency** | Sub-milisecond (SSD) |
| **Snapshots** | ✅ (ZFS snapshots — natychmiastowe) |
| **Clones** | ✅ (zero-copy clones) |
| **Kompresja** | ✅ (LZ4) |
| **Szyfrowanie** | At-rest (KMS), in-transit |
| **Rozmiar** | 64 GiB – 512 TiB |

### ZFS — zero-copy clone

```
Snapshot (point-in-time, instant) → Clone (writable kopia)
                                          ↑
                        Clone nie kopiuje danych — tylko zapisuje delta
                        Idealne do dev/test environments
```

### AWS CLI

```bash
# Utwórz FSx OpenZFS
aws fsx create-file-system \
  --file-system-type OPENZFS \
  --storage-capacity 64 \
  --subnet-ids subnet-12345678 \
  --open-zfs-configuration \
    DeploymentType=SINGLE_AZ_1,\
    ThroughputCapacity=64

# Utwórz snapshot
aws fsx create-snapshot \
  --volume-id fsvol-12345678 \
  --name "pre-migration-snapshot"

# Utwórz volume z snapshotu (clone)
aws fsx create-volume \
  --volume-type OPENZFS \
  --open-zfs-configuration \
    ParentVolumeId=fsvol-12345678,\
    OriginSnapshot={SnapshotARN=...,CopyStrategy=CLONE}
```

---

### 26.6 Tabela porównawcza

| Cecha | FSx Windows | FSx Lustre | FSx ONTAP | FSx OpenZFS |
|-------|-------------|------------|-----------|-------------|
| **Protokół** | SMB | Lustre | NFS+SMB+iSCSI | NFS |
| **System plików** | NTFS | Lustre | ONTAP | ZFS |
| **OS** | Windows/Linux | Linux | Windows+Linux | Linux |
| **AD integracja** | ✅ natywna | ❌ | ✅ | ❌ |
| **S3 integracja** | ❌ | ✅ natywna | ✅ tiering | ❌ |
| **Multi-AZ** | ✅ | ❌ (per AZ) | ✅ | ✅ |
| **Max throughput** | 2 GB/s | 1+ TB/s | Setki GB/s | 21 GB/s |
| **IOPS** | Setki tys. | Miliony | Miliony | 1,000,000 |
| **Snapshots** | VSS | ❌ | ✅ ONTAP | ✅ ZFS |
| **Deduplication** | ✅ | ❌ | ✅ | ❌ |
| **Kompresja** | ✅ | ❌ | ✅ | ✅ LZ4 |
| **Tiering do S3** | ❌ | ✅ (export) | ✅ auto | ❌ |
| **Zastosowanie** | Windows apps | HPC/ML/AI | Enterprise NAS | Linux migration |
| **Azure analog** | Azure Files | Azure HPC Cache | Azure NetApp Files | — |
| **Koszt** | Średni | Wysoki (HPC) | Wysoki | Średni |

---

### 26.7 FSx vs EFS

| | Amazon EFS | FSx for Windows | FSx for Lustre | FSx for ONTAP | FSx OpenZFS |
|-|-----------|----------------|---------------|---------------|-------------|
| **Protokół** | NFS v4 | SMB | Lustre | NFS+SMB+iSCSI | NFS |
| **OS** | Linux | Windows (+ Linux) | Linux | Both | Linux |
| **Skalowalność** | Auto (petabytes) | Manual (32-65k GiB) | Manual | Manual | Manual |
| **Performance** | General purpose | High (SSD/HDD) | Ultra-high | High | Very high |
| **Use case** | Web apps, CMS, shared Linux | Windows workloads | HPC, ML | Enterprise NAS | Linux NAS migration |
| **AD** | ❌ | ✅ | ❌ | ✅ | ❌ |
| **Multi-AZ** | ✅ natywnie | Opcja | ❌ | ✅ | ✅ |
| **Koszt** | Pay per GB | Provisioned | Provisioned | Provisioned | Provisioned |

**Reguła egzaminacyjna:**
- Windows + SMB + AD → **FSx Windows**
- HPC, ML, S3 data lake → **FSx Lustre**
- Multi-protocol enterprise → **FSx ONTAP**
- Linux NFS migration → **FSx OpenZFS** lub **EFS**
- Simple Linux shared → **EFS**

---

### 26.8 Bezpieczeństwo i niezawodność

### Szyfrowanie

```
At-rest:  KMS (AWS managed key lub customer managed key)
In-transit: TLS/SMB encryption (Windows), NFS over TLS (ONTAP/OpenZFS)
           Lustre: in-transit encryption via VPC network policies
```

### Dostęp sieciowy

| Wariant | Dostęp |
|---------|--------|
| FSx Windows | VPC only (ENI w subnecie) |
| FSx Lustre | VPC only |
| FSx ONTAP | VPC only (ENI) |
| FSx OpenZFS | VPC only |

**Wszystkie FSx** = dostęp tylko z VPC. Brak publicznego endpoint. Połączenie on-prem → VPN lub Direct Connect.

### IAM + Resource Policies

```bash
# Kontrola FSx przez IAM policy
{
  "Effect": "Allow",
  "Action": [
    "fsx:CreateFileSystem",
    "fsx:DescribeFileSystems",
    "fsx:CreateBackup"
  ],
  "Resource": "arn:aws:fsx:*:123456789:file-system/*"
}
```

### Backup

```bash
# Automatyczne backupy (okno maintenance)
aws fsx update-file-system \
  --file-system-id fs-12345678 \
  --windows-configuration \
    AutomaticBackupRetentionDays=30,\
    DailyAutomaticBackupStartTime=02:00

# Restore z backupu
aws fsx create-file-system-from-backup \
  --backup-id backup-12345678 \
  --subnet-ids subnet-12345678
```

### HA i failover

```mermaid
graph TD
    Client[Klient] --> DNS[DNS alias\n(automatyczny failover)]
    DNS --> Primary[Primary\nFile Server\nAZ-A]
    DNS --> Standby[Standby\nFile Server\nAZ-B]
    Primary -->|Synchroniczna replikacja| Standby
    Primary -->|Awaria| Failover{Failover\n~30s}
    Failover --> Standby
```

---

### 26.9 Well-Architected Framework

| Filar | FSx — implementacja |
|-------|---------------------|
| **Operational Excellence** | CloudWatch metrics (DataReadBytes, DataWriteBytes, FreeStorageCapacity), CloudTrail audit, automatyczne backupy |
| **Security** | VPC isolation, KMS encryption, IAM access control, AD integration (FSx Windows/ONTAP), Security Groups |
| **Reliability** | Multi-AZ deployment (Windows, ONTAP, OpenZFS), automatyczne backupy, cross-region backup via AWS Backup |
| **Performance Efficiency** | Wybór odpowiedniego wariantu (Lustre dla HPC), SSD vs HDD tier, throughput provisioning, FSx Lustre sub-ms latency |
| **Cost Optimization** | S3 tiering (ONTAP), Scratch vs Persistent (Lustre), prawidłowy rozmiar przez monitoring FreeStorageCapacity, Single-AZ dla dev/test |
| **Sustainability** | Data deduplication (Windows, ONTAP) redukuje przechowywane dane, S3 tiering dla cold data |

---

### 26.10 4 Domeny egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity

- **Multi-account FSx**: Udostępnianie FSx ONTAP przez VPC Peering lub AWS RAM
- **Hybrid**: FSx Windows z on-prem AD → użytkownicy on-prem i chmurowi z tym samym auth
- **Cross-region DR**: FSx Windows Multi-AZ + AWS Backup cross-region copy
- **SnapMirror** z on-prem ONTAP do FSx ONTAP = hybrydowy NAS

### Domain 2: Design for New Solutions

- **Greenfield HPC cluster**: FSx Lustre Scratch 2 + S3 data repository + EC2 HPC instances + EFA
- **Windows file server replacement**: FSx Windows Multi-AZ zamiast Windows Server na EC2
- **Enterprise NAS**: FSx ONTAP Multi-AZ dla środowisk wymagających NFS + SMB jednocześnie

### Domain 3: Continuous Improvement for Existing Solutions

- Migracja EFS → FSx ONTAP (lepsze enterprise features: deduplication, compression)
- Migracja Windows File Server na EC2 → FSx Windows (reduce management overhead)
- Optymalizacja kosztów: włącz storage tiering w FSx ONTAP

### Domain 4: Accelerate Workload Migration and Modernization

- **NetApp on-prem → FSx ONTAP**: SnapMirror replikacja, cut-over bez retrainingu zespołu
- **Windows File Server on-prem → FSx Windows**: DataSync + FSx
- **HPC on-prem → FSx Lustre**: zachowanie POSIX, integracja z S3

---

## 27. Amazon S3 — Simple Storage Service

### 27.1 Przegląd i Kontekst

**Amazon S3** (Simple Storage Service) to obiektowy magazyn danych AWS — jeden z najstarszych i najważniejszych serwisów platformy (dostępny od 2006 roku). Przechowuje dane jako **obiekty** w **bucketach**. Nie jest systemem plików (brak hierarchii katalogów — tylko klucze z `/` w nazwie symulują katalogi) ani blokiem (jak EBS).

**Kluczowe cechy:**
- Pojemność: **nieograniczona** (per bucket)
- Maksymalny rozmiar obiektu: **5 TB**
- Maksymalny rozmiar jednego PUT: **5 GB** (powyżej → Multipart Upload)
- Multipart Upload zalecany: **> 100 MB**
- Trwałość: **99.999999999% (11 dziewiątek)**
- Dostępność: zależy od klasy (99.99% Standard)

**Zakres geograficzny:**
- Bucket = **regionalny** (dane fizycznie w jednym regionie)
- Namespace bucketów = **globalny** (nazwa bucketa musi być unikalna na świecie)
- URL bucketa: `https://bucket-name.s3.region.amazonaws.com/key`

**Porównanie z innymi chmurami:**

| Cecha | AWS S3 | Azure Blob Storage | Google Cloud Storage |
|-------|--------|-------------------|---------------------|
| Jednostka | Bucket / Object | Container / Blob | Bucket / Object |
| Klasy | 7 klas | Hot/Cool/Cold/Archive | Standard/Nearline/Coldline/Archive |
| Globalny namespace | ✅ | ✅ (per account) | ✅ |
| Wersjonowanie | ✅ | ✅ (soft delete + versioning) | ✅ |
| Object Lock (WORM) | ✅ | ✅ (immutability policies) | ✅ |
| Transfer Acceleration | ✅ (CloudFront edge) | ✅ (Azure CDN) | ✅ (Premium Tier) |
| Static Website | ✅ | ✅ (static website hosting) | ✅ |
| SQL na obiektach | S3 Select | — | BigQuery Storage Read |

---

### 27.2 S3 Storage Classes

Wybór klasy = kompromis: **koszt przechowywania** vs **koszt pobierania** vs **dostępność**.

| Klasa | Min. czas przechow. | Retrieval time | Opłata za retrieve | Dostępność | AZ | Użycie |
|-------|--------------------|-----------------|--------------------|------------|-----|--------|
| **S3 Standard** | brak | Natychmiastowy | ❌ | 99.99% | ≥3 | Aktywne dane, częsty dostęp |
| **S3 Intelligent-Tiering** | brak | Natychmiastowy (częste/rzadkie), opcjonalnie minuty/godziny (archiwum) | ❌ (monitoring fee) | 99.9% | ≥3 | Nieprzewidywalny wzorzec dostępu |
| **S3 Standard-IA** | 30 dni | Natychmiastowy | ✅ per GB | 99.9% | ≥3 | Rzadki dostęp, duże pliki (backupy DR) |
| **S3 One Zone-IA** | 30 dni | Natychmiastowy | ✅ per GB | 99.5% | 1 | Dane odtwarzalne, rzadki dostęp |
| **S3 Glacier Instant Retrieval** | 90 dni | Natychmiastowy (ms) | ✅ per GB | 99.9% | ≥3 | Archiwa z rzadkim ale szybkim dostępem |
| **S3 Glacier Flexible Retrieval** | 90 dni | Expedited: 1-5 min / Standard: 3-5h / Bulk: 5-12h | ✅ per GB + per request | 99.99% | ≥3 | Archiwa, dostęp kilka razy w roku |
| **S3 Glacier Deep Archive** | **180 dni** | Standard: 12h / Bulk: 48h | ✅ per GB + per request | 99.99% | ≥3 | Długoterminowe archiwum (7-10 lat) |

> **Egzamin:** S3 Intelligent-Tiering ma **opłatę monitoringową** (~$0.0025/1000 obiektów) — bez opłaty za retrieve.

---

### 27.3 S3 Lifecycle Policies

Automatyczne przejścia między klasami i wygasanie obiektów.

**Dozwolone przejścia (waterfall — tylko w dół):**

```mermaid
graph TD
    A[S3 Standard] -->|min 30 dni| B[S3 Standard-IA]
    A -->|min 30 dni| C[S3 Intelligent-Tiering]
    A -->|min 30 dni| D[S3 One Zone-IA]
    B -->|min 30 dni od IA| E[S3 Glacier Instant Retrieval]
    C -->|min 30 dni| E
    D -->|min 30 dni od IA| E
    E -->|min 90 dni od Glacier IR| F[S3 Glacier Flexible Retrieval]
    F -->|min 90 dni od Flexible| G[S3 Glacier Deep Archive]
    B -->|min 30 dni od IA| F
    B -->|min 30 dni od IA| G
```

**Ograniczenia:**
- Nie można przejść z klasy niższej do wyższej (np. Glacier → Standard) przez lifecycle — tylko ręczna restore
- Minimum 30 dni w Standard przed przejściem do IA
- Minimum rozmiar obiektu dla IA: **128 KB** (mniejsze nie są przenoszone — naliczany pełny koszt 128KB)

```bash
# Przykład lifecycle policy (AWS CLI)
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration '{
    "Rules": [{
      "ID": "archive-old-logs",
      "Status": "Enabled",
      "Filter": {"Prefix": "logs/"},
      "Transitions": [
        {"Days": 30, "StorageClass": "STANDARD_IA"},
        {"Days": 90, "StorageClass": "GLACIER"}
      ],
      "Expiration": {"Days": 365}
    }]
  }'
```

---

### 27.4 S3 Versioning

Przechowuje **wszystkie wersje** obiektu — łącznie z usuniętymi.

**Stany bucketa:**
- `Unversioned` (domyślny) — jeden obiekt per klucz
- `Versioning-enabled` — każdy PUT tworzy nową wersję (Version ID)
- `Versioning-suspended` — nowe obiekty dostają `null` jako Version ID, stare zachowane

**Delete behavior:**
- `DELETE` bez Version ID → tworzy **Delete Marker** (obiekt "ukryty", nie usunięty)
- `DELETE` z konkretnym Version ID → trwale usuwa tę wersję
- Przywrócenie = usuń Delete Marker

**MFA Delete:**
- Wymaga MFA token przy: trwałym usunięciu wersji + wyłączeniu wersjonowania
- Włączyć może tylko root account przez CLI
- Chroni przed przypadkowym lub złośliwym usunięciem

```bash
# Włącz wersjonowanie
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled

# Lista wersji obiektu
aws s3api list-object-versions --bucket my-bucket --prefix myfile.txt

# Usuń konkretną wersję
aws s3api delete-object \
  --bucket my-bucket \
  --key myfile.txt \
  --version-id abc123xyz
```

---

### 27.5 S3 Replication

**Cross-Region Replication (CRR)** — replikacja między regionami.
**Same-Region Replication (SRR)** — replikacja w tym samym regionie.

**Wymagania:**
- Wersjonowanie MUSI być włączone na **obu** bucketach (source + destination)
- IAM Role z uprawnieniami do odczytu źródła i zapisu celu
- Buckety mogą być w różnych kontach AWS

**Co NIE jest replikowane:**
- Obiekty istniejące przed włączeniem replikacji (← pułapka egzaminacyjna!)
- Delete Markers (domyślnie) — można włączyć `DeleteMarkerReplication`
- Obiekty zaszyfrowane SSE-C
- Obiekty w klasie Glacier / Deep Archive
- Replikacje z innych bucketów (nie ma "chain replication" bez dodatkowej konfiguracji)

**Replication Time Control (RTC):**
- Gwarantuje replikację **99.99% obiektów w 15 minutach**
- Dodatkowy koszt
- Daje metryki replikacji w CloudWatch

```bash
# Konfiguracja replikacji CRR
aws s3api put-bucket-replication \
  --bucket source-bucket \
  --replication-configuration '{
    "Role": "arn:aws:iam::123456789:role/replication-role",
    "Rules": [{
      "Status": "Enabled",
      "Destination": {
        "Bucket": "arn:aws:s3:::destination-bucket",
        "StorageClass": "STANDARD_IA"
      }
    }]
  }'
```

---

### 27.6 S3 Object Lock — WORM

**Write Once Read Many** — obiekty nie mogą być modyfikowane ani usuwane przez zdefiniowany czas.

Wymaga: wersjonowanie włączone przy tworzeniu bucketa.

**Tryby Retention:**

| Tryb | Opis | Kto może usunąć przed końcem? |
|------|------|-------------------------------|
| **Governance** | Chroni przed przypadkowym usunięciem | User z `s3:BypassGovernanceRetention` |
| **Compliance** | Nikt nie może usunąć — nawet root/AWS | **Nikt** — nieodwołalny |

**Legal Hold:**
- Niezależny od retention period
- Włącz/wyłącz w dowolnym momencie (wymaga `s3:PutObjectLegalHold`)
- Blokuje usunięcie dopóki aktywny

> **Egzamin:** Compliance mode = **nikt** (włącznie z root) nie może usunąć przed końcem okresu. Governance = można obejść z odpowiednim IAM permission.

```bash
# Ustaw Object Lock (Governance, 365 dni)
aws s3api put-object-retention \
  --bucket my-bucket \
  --key important-file.pdf \
  --retention '{"Mode":"GOVERNANCE","RetainUntilDate":"2025-12-31T00:00:00Z"}'
```

---

### 27.7 S3 Encryption

**Server-Side Encryption (SSE):**

| Typ | Klucz zarządza | Audit | Koszt | Wymuszenie |
|-----|---------------|-------|-------|------------|
| **SSE-S3** (AES-256) | AWS (S3 service) | Brak per-key | ❌ extra | Domyślne od 2023 |
| **SSE-KMS** | AWS KMS (CMK) | CloudTrail per call | ✅ KMS API calls | Bucket policy |
| **SSE-C** | Klient (własny klucz) | Brak | ❌ extra | HTTPS wymagany |
| **DSSE-KMS** | AWS KMS (2x envelope) | CloudTrail | ✅✅ | Bucket policy |
| **Client-Side** | Klient (przed upload) | Klient | Zależy | — |

**SSE-KMS — ważne dla egzaminu:**
- Każde GET/PUT = wywołanie KMS API → **KMS limits** (5,500–30,000 req/s per region)
- Przy dużym ruchu S3 + SSE-KMS → możliwy throttling
- Rozwiązanie: użyj S3 Bucket Key (redukuje wywołania KMS o ~99%)

**Wymuszenie szyfrowania — bucket policy:**
```json
{
  "Sid": "DenyUnencryptedObjectUploads",
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEquals": {
      "s3:x-amz-server-side-encryption": "aws:kms"
    }
  }
}
```

---

### 27.8 S3 Access Control

**Hierarchia uprawnień (od najszerszego do najwęższego):**
1. **Block Public Access** — nadrzędne, blokuje wszystko publiczne (konto lub bucket)
2. **Bucket Policy** — resource-based, JSON, kontroluje dostęp do całego bucketa
3. **Access Points** — named endpoints z własną polityką
4. **ACL** — legacy, obiekt-level, przestarzałe (domyślnie wyłączone od 2023)

**Object Ownership:**
- `BucketOwnerEnforced` (domyślne) — ACL wyłączone, właściciel bucketa = właściciel wszystkich obiektów
- `BucketOwnerPreferred` — właściciel bucketa gdy uploader doda `bucket-owner-full-control`
- `ObjectWriter` — uploader = właściciel (legacy)

**Multi-Region Access Points (MRAP):**
- Globalny endpoint routujący do najbliższego bucketa
- Failover routing między regionami
- Działa z replikacją bi-directional

```bash
# Utwórz Access Point
aws s3control create-access-point \
  --account-id 123456789 \
  --name my-access-point \
  --bucket my-bucket \
  --vpc-configuration VpcId=vpc-abc123
```

---

### 27.9 S3 Presigned URLs

Tymczasowy URL dający dostęp do prywatnego obiektu bez credentials.

- Generowany przez IAM tożsamość (user, role)
- Ważność: domyślnie 1h, max: 7 dni (dla IAM user z długoterm. credentials), 1h (dla STS/role)
- Używane do: upload (PUT presigned), download (GET presigned)

> **Pułapka:** URL wygasa gdy wygasną credentials roli — nie po ustawionym czasie jeśli rola ma krótszą sesję.

```bash
# Wygeneruj presigned URL (pobieranie, 1 godzina)
aws s3 presign s3://my-bucket/my-file.pdf --expires-in 3600
```

---

### 27.10 S3 Event Notifications

Triggerowanie serwisów przy operacjach na obiektach.

**Destynacje:**
- **SNS** — powiadomienia, fan-out
- **SQS** — kolejkowanie, przetwarzanie asynchroniczne
- **Lambda** — przetwarzanie bezpośrednie
- **EventBridge** — zaawansowane filtrowanie, routing do wielu serwisów

**Zdarzenia:** `s3:ObjectCreated:*`, `s3:ObjectRemoved:*`, `s3:Replication:*`, `s3:ObjectRestore:*`

> **Egzamin:** EventBridge = najbardziej elastyczne (filtrowanie po metadata, routing do 20+ serwisów). SNS/SQS/Lambda = prostsze, tańsze.

```bash
# Konfiguracja notyfikacji → SQS
aws s3api put-bucket-notification-configuration \
  --bucket my-bucket \
  --notification-configuration '{
    "QueueConfigurations": [{
      "QueueArn": "arn:aws:sqs:us-east-1:123456789:my-queue",
      "Events": ["s3:ObjectCreated:*"],
      "Filter": {
        "Key": {"FilterRules": [{"Name": "suffix", "Value": ".jpg"}]}
      }
    }]
  }'
```

---

### 27.11 S3 Performance

**Transfer Acceleration:**
- Upload przez **CloudFront edge locations** → prywatna sieć AWS → bucket
- URL: `bucket.s3-accelerate.amazonaws.com`
- Opłata dodatkowa (~$0.04/GB US→EU)
- Kiedy używać: klienci globalnie rozproszeni, duże pliki

**Multipart Upload:**
- Wymagany dla obiektów **> 5 GB**
- Zalecany dla **> 100 MB**
- Upload równoległy parts → szybszy transfer
- Możliwość wznowienia po błędzie
- Należy czyścić niekompletne multipart uploads (lifecycle rule)

**Request Rate Limits (per prefix):**
- **3,500 PUT/COPY/POST/DELETE** per sekunda
- **5,500 GET/HEAD** per sekunda
- Optymalizacja: użyj wielu prefiksów (random hash w kluczu)

**S3 Select:**
- SQL na obiekcie S3 (CSV, JSON, Parquet, gzip)
- Pobiera tylko potrzebne kolumny/wiersze → tańszy transfer
- Max 5 GB obiekt, max 128 MB wynik
- Kiedy używać vs Athena: S3 Select = jeden obiekt, Athena = wiele obiektów/tabel

---

### 27.12 S3 Requester Pays

Koszt transferu danych ponosi ten kto pobiera (requester), nie właściciel bucketa.

- Requester musi być uwierzytelniony (AWS account) — brak dla anonymous
- Przydatne przy dużych publicznych datasetach (publiczne dane genomiczne, satelitarne)
- Nagłówek: `x-amz-request-payer: requester`

---

### 27.13 S3 Static Website Hosting

- Serwuje HTML/CSS/JS z bucketa jako strona WWW
- URL: `http://bucket-name.s3-website-region.amazonaws.com`
- **HTTPS nie jest obsługiwane bezpośrednio** → użyj CloudFront przed S3
- Wymaga publicznego dostępu (Block Public Access wyłączony)
- CORS konfiguracja w XML dla cross-origin requests
- Index document + Error document

> **Egzamin:** S3 Static Website = tylko HTTP. CloudFront + S3 = HTTPS + custom domain + OAC.

---

### 27.14 S3 Storage Lens

- **Organizational-wide** widoczność użycia S3 (wszystkie konta AWS Organizations)
- Metryki: rozmiar, liczba obiektów, aktywność requestów, koszt
- Dashboard w konsoli, eksport do S3
- Dwie wersje: Free (25 metryki) + Advanced (35+ metryki, płatny)

---

### 27.15 S3 Batch Operations

Operacje na **miliardach obiektów** jednocześnie:
- Kopiowanie obiektów
- Ustawianie tagów / ACL / Object Lock
- Wywoływanie Lambda per obiekt
- Przywracanie z Glacier
- Źródło: lista z S3 Inventory lub własny CSV manifest

```bash
# Utwórz Batch Operations job (kopiowanie)
aws s3control create-job \
  --account-id 123456789 \
  --operation '{"S3CopyObject":{"TargetResource":"arn:aws:s3:::dest-bucket"}}' \
  --manifest '{"Spec":{"Format":"S3BatchOperations_CSV_20180820","Fields":["Bucket","Key"]},"Location":{"ObjectArn":"arn:aws:s3:::my-bucket/manifest.csv","ETag":"abc123"}}' \
  --report '{"Bucket":"arn:aws:s3:::report-bucket","Format":"Report_CSV_20180820","Enabled":true,"ReportScope":"AllTasks"}' \
  --role-arn arn:aws:iam::123456789:role/batch-ops-role \
  --priority 10
```

---

### 27.16 Integracje z innymi serwisami AWS

| Serwis | Integracja z S3 |
|--------|----------------|
| **CloudFront** | Origin dla dystrybucji CDN, OAC/OAI |
| **Lambda** | Trigger z event notifications, Batch Operations |
| **Athena** | SQL queries na S3 data lake |
| **Glue** | ETL, Data Catalog |
| **EMR** | HDFS-compatible storage (s3a://) |
| **SageMaker** | Training data, model artifacts |
| **Redshift Spectrum** | Query S3 z Redshift |
| **AWS Backup** | Backup S3 (cross-region, cross-account) |
| **Macie** | PII discovery w S3 |
| **GuardDuty** | Analiza S3 access logs |
| **KMS** | SSE-KMS encryption |
| **CloudTrail** | Audit S3 API calls |
| **DataSync** | Migracja danych do/z S3 |
| **Storage Gateway** | File Gateway → S3 |
| **Transfer Family** | SFTP/FTP → S3 |

---

### 27.17 Reliability i Security

**Reliability:**
- S3 przechowuje dane w **≥3 AZ** (poza One Zone-IA)
- 11 dziewiątek trwałości = 1 obiekt na 10M utracony raz na 10,000 lat
- Versioning chroni przed przypadkowym usunięciem
- Replication (CRR) = cross-region DR
- Object Lock = ochrona przed ransomware

**Security best practices:**
- Włącz **Block Public Access** na poziomie konta i bucketa
- Użyj **SSE-KMS** z CMK dla wrażliwych danych
- Włącz **Access Logging** lub **CloudTrail Data Events**
- MFA Delete dla krytycznych bucketów
- S3 **Macie** do wykrywania PII
- **VPC Endpoint** (Gateway) dla prywatnego dostępu bez internetu
- Zasada **least privilege** w bucket policies

**Limity:**
| Limit | Wartość |
|-------|---------|
| Buckets per account | 100 (soft), 1000 (hard) |
| Max object size | 5 TB |
| Max single PUT | 5 GB |
| Max bucket policy size | 20 KB |
| S3 Batch Operations jobs | 1000 active |
| Lifecycle rules per bucket | 1000 |
| Replication rules per bucket | 1000 |

---

### 27.18 Well-Architected Framework — S3

| Filar | Praktyki S3 |
|-------|------------|
| **Operational Excellence** | S3 Storage Lens (widoczność), CloudWatch metryki, S3 Access Logs, automatyzacja przez lifecycle + Batch Operations |
| **Security** | Block Public Access, SSE-KMS + CMK, bucket policies (least privilege), MFA Delete, Macie (PII), VPC Gateway Endpoint, Object Lock |
| **Reliability** | Versioning, CRR (cross-region), Object Lock (WORM), 11-dziewiątek trwałości, Multi-Region Access Points |
| **Performance Efficiency** | Transfer Acceleration (global clients), Multipart Upload (>100MB), S3 Select (mniej transferu), partycjonowanie prefiksów |
| **Cost Optimization** | Lifecycle policies (Glacier/IA), Intelligent-Tiering, S3 Storage Lens (cost metrics), Requester Pays, S3 Bucket Key (KMS costs) |
| **Sustainability** | Lifecycle policies (usuń nieużywane), Intelligent-Tiering (optymalizacja automatyczna), Glacier Deep Archive (długoterminowe archiwa) |

---

### 27.19 4 Domeny Egzaminacyjne SAP-C02

### Design Solutions for Organizational Complexity
- Multi-account S3 access: cross-account bucket policies, Resource Access Manager (nie dla S3 bezpośrednio), S3 Access Points per VPC/account
- AWS Organizations SCP blokujące dostęp do S3 poza regionem
- S3 Storage Lens dla organizacji: widoczność wszystkich kont

### Design for New Solutions
- S3 jako data lake: partycjonowanie kluczy, Athena, Glue, Redshift Spectrum
- Event-driven architecture: S3 → EventBridge → Step Functions
- Static website: S3 + CloudFront + Route 53 + ACM
- WORM compliance: S3 Object Lock Compliance mode

### Continuous Improvement for Existing Solutions
- Redukcja kosztów: Storage Lens → identyfikacja rzadko używanych danych → lifecycle do Glacier
- Poprawa bezpieczeństwa: włącz Block Public Access, migruj z SSE-S3 na SSE-KMS (Batch Operations)
- Optymalizacja wydajności: Transfer Acceleration, S3 Bucket Key

### Accelerate Workload Migration and Modernization
- Storage Gateway File Gateway: NFS/SMB z on-prem → S3
- DataSync: masowa migracja on-prem → S3
- Snowball/Snowmobile: petabyte-scale transfer
- S3 jako staging dla Database Migration Service

---



## 28. AWS Backup — Centralne Zarządzanie Kopiami Zapasowymi

> **AWS Backup** to w pełni zarządzana usługa centralizująca tworzenie, zarządzanie i przywracanie kopii zapasowych zasobów AWS z jednego miejsca. Zamiast konfigurować backup dla każdej usługi osobno, tworzysz polityki (Backup Plans) i stosujesz je do wielu zasobów jednocześnie — w wielu kontach i regionach.

**Azure odpowiednik:** Azure Backup (Recovery Services Vault + Backup Center)  
**Zasięg:** Regionalny (Backup Vaults są regionalne) | Cross-Region copy dostępne | Multi-Account przez AWS Organizations  

---

### 28.1 Czym jest AWS Backup?

AWS Backup to **policy-based backup service**. Zamiast pisać skrypty do tworzenia snapshotów EBS, backupów RDS i kopii DynamoDB — definiujesz Backup Plan i AWS robi to za Ciebie według harmonogramu.

```mermaid
graph TD
    BP[Backup Plan] --> BR1[Rule: Daily backup\n7-day retention]
    BP --> BR2[Rule: Weekly backup\n30-day retention\nCopy to eu-west-1]
    BP --> BS[Backup Selection]
    BS --> T[By Tag: Environment=prod]
    T --> EC2[EC2 instances]
    T --> RDS[RDS databases]
    T --> EFS[EFS file systems]
    T --> DDB[DynamoDB tables]
    BR1 --> BV[Backup Vault\nus-east-1]
    BR2 --> BVR[Backup Vault\neu-west-1]
```

**Kluczowe pojęcia:**

| Pojęcie | Opis |
|---------|------|
| **Backup Plan** | Polityka backupu — kiedy, jak często, jak długo przechowywać |
| **Backup Rule** | Jeden harmonogram w planie (np. daily, weekly) |
| **Backup Selection** | Co backupować — zasoby wybrane przez tagi lub ARN |
| **Backup Vault** | Logiczny kontener na Recovery Points (zaszyfrowany KMS) |
| **Recovery Point** | Pojedyncza kopia zapasowa (snapshot, backup) |
| **Backup Gateway** | Wirtualna bramka do backupu VMware na premises |

---

### 28.2 Obsługiwane Usługi AWS

| Usługa | Typ backup | Cross-Region | Cross-Account | Item-Level Restore |
|--------|-----------|--------------|--------------|-------------------|
| **EC2** | AMI snapshot | ✅ | ✅ | ❌ |
| **EBS** | Snapshot | ✅ | ✅ | ❌ |
| **RDS** | DB snapshot | ✅ | ✅ | ❌ |
| **Aurora** | DB cluster snapshot | ✅ | ✅ | ❌ |
| **DynamoDB** | On-demand backup | ✅ | ✅ | ✅ (table/item) |
| **EFS** | Incremental backup | ✅ | ✅ | ✅ (file/dir) |
| **FSx for Windows** | VSS backup | ✅ | ✅ | ✅ |
| **FSx for Lustre** | Snapshot | ✅ | ✅ | ❌ |
| **FSx for ONTAP** | Snapshot | ✅ | ✅ | ✅ |
| **FSx for OpenZFS** | Snapshot | ✅ | ✅ | ✅ |
| **S3** | Continuous backup | ✅ | ✅ | ✅ (object) |
| **DocumentDB** | Cluster snapshot | ✅ | ✅ | ❌ |
| **Neptune** | Cluster snapshot | ✅ | ✅ | ❌ |
| **Storage Gateway** | Volume snapshot | ✅ | ✅ | ❌ |
| **VMware Cloud on AWS** | VM backup via Gateway | ✅ | ✅ | ❌ |
| **Timestream** | ✅ | ✅ | ✅ | ❌ |
| **Redshift** | ✅ | ✅ | ✅ | ❌ |

---

### 28.3 Backup Plans — Szczegóły

Backup Plan = jeden lub więcej **Backup Rules** + **Backup Selection**.

```bash
# Utwórz Backup Plan
aws backup create-backup-plan \
  --backup-plan '{
    "BackupPlanName": "prod-backup-plan",
    "Rules": [
      {
        "RuleName": "daily-backup",
        "TargetBackupVaultName": "prod-vault",
        "ScheduleExpression": "cron(0 5 * * ? *)",
        "StartWindowMinutes": 60,
        "CompletionWindowMinutes": 120,
        "Lifecycle": {
          "MoveToColdStorageAfterDays": 30,
          "DeleteAfterDays": 90
        },
        "CopyActions": [
          {
            "DestinationBackupVaultArn": "arn:aws:backup:eu-west-1:123456789:backup-vault:dr-vault",
            "Lifecycle": {
              "DeleteAfterDays": 30
            }
          }
        ]
      }
    ]
  }'
```

### Backup Rule — parametry:

| Parametr | Opis | Przykład |
|---------|------|---------|
| `ScheduleExpression` | cron/rate kiedy uruchamiać | `cron(0 5 * * ? *)` = 5:00 UTC daily |
| `StartWindowMinutes` | Okno startowe (jak długo czekać na start) | 60 min |
| `CompletionWindowMinutes` | Max czas na ukończenie backupu | 180 min |
| `Lifecycle.MoveToColdStorageAfterDays` | Kiedy przenieść do cold storage | 30 dni |
| `Lifecycle.DeleteAfterDays` | Kiedy usunąć | 90 dni |
| `CopyActions` | Kopiuj do innego vault (cross-region/account) | DestinationVaultARN |
| `EnableContinuousBackup` | Continuous backup (S3, RDS PITR) | true/false |

---

### 28.4 Backup Vaults

Vault = **logiczny kontener** na Recovery Points. Każdy vault jest **zaszyfrowany KMS** (domyślnie AWS managed key, można użyć CMK).

```bash
# Utwórz vault z CMK
aws backup create-backup-vault \
  --backup-vault-name prod-vault \
  --encryption-key-arn arn:aws:kms:us-east-1:123456789:key/mrk-abc123

# Pobierz vault
aws backup describe-backup-vault \
  --backup-vault-name prod-vault

# Dodaj resource-based policy do vaultu (kto może tworzyć/usuwać)
aws backup put-backup-vault-access-policy \
  --backup-vault-name prod-vault \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Deny",
        "Principal": "*",
        "Action": "backup:DeleteRecoveryPoint",
        "Resource": "*",
        "Condition": {
          "StringNotLike": {
            "aws:PrincipalArn": "arn:aws:iam::*:role/backup-admin"
          }
        }
      }
    ]
  }'
```

**Default vault** — AWS tworzy automatycznie vault `Default` w każdym regionie gdzie włączysz AWS Backup.

---

### 28.5 Backup Selection — Jak Wybierać Zasoby

Trzy sposoby:

```bash
# 1. Po tagach (najlepsze podejście dla dużych środowisk)
aws backup create-backup-selection \
  --backup-plan-id PLAN_ID \
  --backup-selection '{
    "SelectionName": "prod-tagged-resources",
    "IamRoleArn": "arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole",
    "ListOfTags": [
      {
        "ConditionType": "STRINGEQUALS",
        "ConditionKey": "BackupPolicy",
        "ConditionValue": "daily"
      }
    ]
  }'

# 2. Po ARN (konkretne zasoby)
aws backup create-backup-selection \
  --backup-plan-id PLAN_ID \
  --backup-selection '{
    "SelectionName": "specific-rds",
    "IamRoleArn": "arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole",
    "Resources": [
      "arn:aws:rds:us-east-1:123456789:db:prod-mysql"
    ]
  }'

# 3. Wszystkie zasoby danego typu
# Użyj wildcard w Resources: "arn:aws:ec2:*:*:instance/*"
```

---

### 28.6 Cross-Region i Cross-Account Backup

### Cross-Region — Copy Rule

Dodaj `CopyActions` w Backup Rule. Recovery Point zostaje skopiowany do innego regionu automatycznie.

```mermaid
sequenceDiagram
    participant Source as us-east-1\nBackup Vault
    participant AWS as AWS Backup Service
    participant Dest as eu-west-1\nBackup Vault
    
    AWS->>Source: Create Recovery Point (daily)
    AWS->>Dest: Copy Recovery Point (Copy Action)
    Note over Source,Dest: Cross-Region copy = dodatkowy koszt\n(data transfer + storage w DR region)
```

### Cross-Account — AWS Organizations

```bash
# W management account Organizations — włącz cross-account backup
aws organizations enable-aws-service-access \
  --service-principal backup.amazonaws.com

# Deleguj backup management do dedykowanego konta
aws backup put-backup-vault-access-policy \
  --backup-vault-name shared-vault \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "AWS": "arn:aws:iam::MEMBER_ACCOUNT_ID:root"
        },
        "Action": [
          "backup:CopyIntoBackupVault"
        ],
        "Resource": "*"
      }
    ]
  }'
```

**Backup Policies** (jak SCPs) — możesz wymusić backup na wszystkich kontach w OU:
```json
{
  "plans": {
    "ProdBackupPlan": {
      "regions": {"@@assign": ["us-east-1", "eu-west-1"]},
      "rules": {
        "DailyBackups": {
          "schedule_expression": {"@@assign": "cron(0 5 * * ? *)"},
          "target_backup_vault_name": {"@@assign": "Default"},
          "lifecycle": {
            "delete_after_days": {"@@assign": "35"}
          }
        }
      },
      "selections": {
        "tags": {
          "ProdResources": {
            "iam_role_arn": {"@@assign": "arn:aws:iam::$account:role/AWSBackupDefaultServiceRole"},
            "tag_key": {"@@assign": "BackupRequired"},
            "tag_value": {"@@assign": ["true"]}
          }
        }
      }
    }
  }
}
```

---

### 28.7 AWS Backup Vault Lock — WORM

Vault Lock = **Write Once Read Many** — uniemożliwia usunięcie Recovery Points przed upływem retention period. Nawet root/admin nie może usunąć.

| Feature | Governance Mode | Compliance Mode |
|---------|----------------|----------------|
| Kto może wyłączyć | Admin (IAM) | **Nikt** (nieodwracalne!) |
| Usunięcie przed retention | Admin może | **Niemożliwe** |
| Zastosowanie | Dev/Test | Compliance, HIPAA, SEC, FINRA |
| Cooling period | 3 dni przed aktywacją | 3 dni przed aktywacją |
| Zmiana po aktywacji | Można | **Nie można** |

```bash
# Włącz Vault Lock (Compliance Mode — NIEODWRACALNE po cooling period!)
aws backup put-backup-vault-lock-configuration \
  --backup-vault-name prod-vault \
  --min-retention-days 7 \
  --max-retention-days 365
  # Bez --changeable-for-days = Compliance Mode
  
# Governance Mode (można wyłączyć przez 3 dni)
aws backup put-backup-vault-lock-configuration \
  --backup-vault-name dev-vault \
  --min-retention-days 1 \
  --max-retention-days 30 \
  --changeable-for-days 3
```

> ⚠️ **UWAGA EGZAMINACYJNA:** Compliance Mode Vault Lock = żaden principal (włącznie z root i AWS Support) nie może usunąć backupu przed upływem retention. To jedyne rozwiązanie dla regulacji wymagających nienaruszalności backupów.

---

### 28.8 AWS Backup Audit Manager

Audit Manager = automatyczna weryfikacja zgodności polityk backupowych z frameworkami.

**Wbudowane frameworki compliance:**
- AWS Backup controls (własne)
- CIS AWS Foundations
- HIPAA
- PCI DSS
- SOC 2

```bash
# Utwórz framework audytowy
aws backup create-framework \
  --framework-name "prod-backup-compliance" \
  --framework-controls '[
    {
      "ControlName": "BACKUP_PLAN_MIN_FREQUENCY_AND_MIN_RETENTION_CHECK",
      "ControlInputParameters": [
        {"ParameterName": "requiredFrequencyUnit", "ParameterValue": "days"},
        {"ParameterName": "requiredFrequencyValue", "ParameterValue": "1"},
        {"ParameterName": "requiredRetentionDays", "ParameterValue": "35"}
      ]
    },
    {
      "ControlName": "BACKUP_RECOVERY_POINT_ENCRYPTED"
    },
    {
      "ControlName": "BACKUP_RESOURCES_PROTECTED_BY_CROSS_REGION"
    }
  ]'

# Generuj raport compliance
aws backup create-report-plan \
  --report-plan-name monthly-compliance \
  --report-delivery-channel '{
    "S3BucketName": "backup-reports-bucket",
    "Formats": ["CSV", "JSON"]
  }' \
  --report-setting '{
    "ReportTemplate": "BACKUP_JOB_REPORT"
  }'
```

---

### 28.9 AWS Backup vs Native Snapshots

| Cecha | AWS Backup | Native Snapshots/Backup |
|-------|-----------|------------------------|
| **Centralność** | Jedno miejsce dla wszystkich | Per-usługa, osobno |
| **Cross-service policy** | ✅ Jeden plan dla EC2+RDS+EFS | ❌ Osobne konfiguracje |
| **Cross-account** | ✅ Natywnie przez Organizations | ❌ Wymaga skryptów |
| **Cross-region** | ✅ Copy Action w planie | ⚠️ Ręcznie lub skrypt |
| **Compliance (WORM)** | ✅ Vault Lock | ❌ Brak |
| **Audit reporting** | ✅ Audit Manager | ❌ Brak |
| **Koszt** | Koszt storage + overhead mgmt | Tylko koszt storage |
| **Granularność** | Minuty/godziny/dni | Zależy od usługi |
| **Instant Restore (RDS PITR)** | ✅ Continuous backup | ✅ Native PITR |
| **IAM control** | ✅ Vault policies | ❌ Ograniczone |
| **Tag-based selection** | ✅ Automatyczne | ❌ Ręczne |
| **Monitoring** | CloudWatch + EventBridge | Osobno dla każdej usługi |

**Kiedy używać AWS Backup:**
- Wiele kont (Organizations) → wymóg compliance
- Regulacje (HIPAA, PCI, SEC) → Vault Lock
- Wiele usług → jeden plan do zarządzania
- Audyt i raportowanie → Audit Manager

**Kiedy wystarczą natywne snapshoty:**
- Jeden account, jeden region
- Prosta aplikacja, mało usług
- Brak wymogów compliance

---

### 28.10 Restore

```bash
# Lista recovery points dla zasobu
aws backup list-recovery-points-by-resource \
  --resource-arn arn:aws:rds:us-east-1:123456789:db:prod-mysql

# Przywróć RDS z recovery point
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789:recovery-point:abc123 \
  --metadata '{
    "DBInstanceIdentifier": "prod-mysql-restored",
    "DBInstanceClass": "db.r5.large",
    "Engine": "mysql"
  }' \
  --iam-role-arn arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole

# Przywróć EFS — item-level (konkretny katalog)
aws backup start-restore-job \
  --recovery-point-arn arn:aws:backup:us-east-1:123456789:recovery-point:efs123 \
  --metadata '{
    "file-system-id": "fs-abc123",
    "newFileSystem": "false",
    "Encrypted": "true"
  }' \
  --iam-role-arn arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole \
  --resource-type EFS

# Cross-account restore: najpierw skopiuj recovery point do docelowego konta
aws backup copy-job \
  --source-backup-vault-name prod-vault \
  --source-recovery-point-arn arn:aws:backup:... \
  --destination-backup-vault-arn arn:aws:backup:us-east-1:TARGET_ACCOUNT:backup-vault:restore-vault \
  --iam-role-arn arn:aws:iam::123456789:role/AWSBackupDefaultServiceRole
```

---

### 28.11 Backup dla VMware — AWS Backup Gateway

AWS Backup Gateway = wirtualny appliance (OVA) instalowany on-premises w środowisku VMware.

```mermaid
graph LR
    VM[VMware VMs\non-premises] --> GW[Backup Gateway\nOVA Appliance]
    GW --> AWS[AWS Backup Service]
    AWS --> BV[Backup Vault\nS3-backed]
    BV --> CR[Cross-Region Copy]
    BV --> CA[Cross-Account Copy]
```

**Wymagania:**
- vSphere 6.7 lub nowszy
- ESXi host z dostępem do internetu
- IAM role z uprawnieniami backup:*

---

### 28.12 Pricing

| Element | Koszt |
|---------|-------|
| Backup storage (warm) | ~$0.05/GB-miesiąc (region-dependent) |
| Backup storage (cold) | ~$0.01/GB-miesiąc |
| Restore | ~$0.02/GB |
| Cross-region copy | Data transfer + storage w DR region |
| DynamoDB continuous backup | $0.20/GB-miesiąc |
| S3 backup | $0.05/GB |
| Audit Manager | $1.00 per resource/miesiąc oceniany |

> **Uwaga:** AWS Backup jest tańszy od zarządzania backupem manualnie (brak kosztu Lambdy, EventBridge, custom skryptów). Ale AWS Backup for EFS jest droższy (~$0.05/GB) niż snapshot EBS (~$0.05/GB-miesiąc — różnica w granularności).

---

### 28.13 Integracja z AWS Organizations

```mermaid
graph TD
    MA[Management Account\nOrganizations] -->|Backup Policy| OU[Production OU]
    OU --> A1[Account A\napp-prod-1]
    OU --> A2[Account B\napp-prod-2]
    OU --> A3[Account C\napp-prod-3]
    
    A1 -->|Recovery Points| BV[Shared Backup Vault\nDedicated Backup Account]
    A2 --> BV
    A3 --> BV
    
    BV -->|Cross-Region Copy| DR[DR Backup Vault\neu-west-1]
```

**Backup Policies (Organization-level):**
- Działają jak SCPs ale dla backup
- Stosowane do wszystkich kont w OU
- Nie można ich nadpisać w kontach członkowskich
- Umożliwiają wymuszenie backup compliance bez dania uprawnień do kont

```bash
# Włącz backup policies w Organizations
aws organizations enable-policy-type \
  --root-id r-xxxx \
  --policy-type BACKUP_POLICY

# Przypnij policy do OU
aws organizations attach-policy \
  --policy-id p-xxxxxxxx \
  --target-id ou-xxxx-xxxxxxxx
```

---

### 28.14 Well-Architected Framework

| Filar | Zastosowanie AWS Backup |
|-------|------------------------|
| **Operational Excellence** | Automatyczne backupy eliminują ręczne operacje. CloudWatch metryki (BackupJobStatus, RestoreJobStatus). EventBridge do alertów o nieudanych backupach |
| **Security** | Vault Lock (WORM) dla compliance. KMS CMK szyfrowanie vaultów. IAM role + Vault Access Policies. Cross-account isolation — backup konto oddzielone od app kont |
| **Reliability** | Cross-Region backup dla DR. Cross-Account backup — izolacja od ransomware/accidental delete. Continuous backup (S3, RDS) = PITR. Testowanie restore przez jobs |
| **Performance Efficiency** | Incremental backupy (EFS, EBS snapshots). Cold storage lifecycle — przenoś stare backupy do cold po 30 dniach. Backup window — uruchamiaj poza godzinami produkcyjnymi |
| **Cost Optimization** | Lifecycle policy: warm→cold→delete. Konsolidacja wielu narzędzi backup w jedno. Tag-based selection zapobiega backupowaniu niepotrzebnych zasobów. Reserved capacity dla storage |
| **Sustainability** | Lifecycle do cold storage redukuje footprint. Usunięcie niepotrzebnych recovery points przez retention policy |

---

### 28.15 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity
- **Multi-account backup**: Backup Policies przez Organizations wymuszają compliance backup bez nadawania uprawnień
- **Cross-account isolation**: Ataki ransomware na konto prod nie dotykają backup account (Vault Lock)
- **Delegated administration**: Dedykowane konto backup jako centralny hub

### Domain 2: Design for New Solutions
- **Architektura backup-first**: Tag `BackupPolicy=daily` na wszystkich zasobach od startu
- **RPO/RTO design**: Continuous backup dla RPO ~1h; Cross-Region dla RTO w DR regionie
- **WORM compliance**: Vault Lock Compliance Mode dla HIPAA/PCI od początku projektu

### Domain 3: Continuous Improvement for Existing Solutions
- **Migracja z custom skryptów**: Replace Lambda+EventBridge backup scripts → AWS Backup Plan
- **Audit Manager**: Wykryj zasoby bez backup policy → remediacja przez backup plan
- **Cost optimization**: Dodaj lifecycle rules do istniejących planów → cold storage

### Domain 4: Accelerate Workload Migration and Modernization
- **VMware migration**: Backup Gateway → backup VM przed migracją → restore jako EC2 (AWS Application Migration Service)
- **Lift-and-shift safety net**: Backup wszystkich zasobów przed migracją
- **Modernization**: Migruj z backup tapes (on-prem) → AWS Backup + cold storage

---

### 28.16 Reliability i Security

### Reliability
```
✅ Cross-Region copy — DR w innym regionie
✅ Cross-Account copy — izolacja backup od produkcji
✅ Vault Lock — nienaruszalność backupów
✅ Continuous backup — PITR dla RDS, S3
✅ Monitoring — CloudWatch + EventBridge alerts na failed backup jobs
✅ Restore testing — możesz uruchomić restore job do test środowiska
```

### Security
```
✅ KMS encryption — każdy vault szyfrowany (domyślnie AWS managed, można CMK)
✅ Vault Access Policy — kto może tworzyć/usuwać/restore recovery points
✅ IAM roles — AWSBackupDefaultServiceRole (lub custom)
✅ Vault Lock Compliance Mode — zero-trust backup (nawet admin nie usuwa)
✅ Cross-account vault — backup account oddzielony od app accounts
✅ CloudTrail — wszystkie backup API calls są logowane
```

### Ograniczenia

| Ograniczenie | Wartość |
|-------------|---------|
| Max Backup Plans per account | 100 |
| Max Backup Rules per plan | 10 |
| Max Backup Vaults per account | 100 |
| Max Recovery Points per vault | Bez limitu (soft: miliony) |
| Backup Gateway per account | 100 |
| Max concurrent backup jobs | 1000 |
| Cross-region copy — dostępność | Nie wszystkie regiony obsługują wszystkie usługi |
| S3 backup — wymaganie | Versioning musi być włączone na bucket |
| DynamoDB continuous backup | Tylko przez AWS Backup (nie natywny DynamoDB backup) |

---

## 29. AWS Snow Family — Snowcone, Snowball Edge, Snowmobile

> 🧊 Fizyczne urządzenia do transferu danych i edge computingu w odizolowanych lub słabo skomunikowanych lokalizacjach.

---

### 29.1 Przegląd i Kontekst

**AWS Snow Family** = rodzina fizycznych urządzeń do:
1. **Offline data transfer** — przeniesienie dużych ilości danych do/z AWS bez sieci
2. **Edge computing** — uruchomienie obliczeń w miejscach bez stałego łącza (platformy wiertnicze, statki, wojsko, fabryki)

**Zasada kciuka:** jeśli transfer przez internet zajmie >1 tydzień → Snow szybszy i tańszy.

**Zakres usługi:** Regionalny — urządzenia powiązane z konkretnym regionem AWS. Snowmobile dostępny tylko w USA.

---

### 29.2 AWS Snowcone

Najmniejsze urządzenie Snow — rozmiar pudełka na buty (2 kg). Jedyne z wbudowanym DataSync.

| Parametr | Snowcone HDD | Snowcone SSD |
|----------|-------------|-------------|
| Storage użytkowy | 8 TB HDD | 14 TB SSD |
| vCPU | 2 | 2 |
| RAM | 4 GB | 4 GB |
| Sieć | 1 GbE / WiFi | 1 GbE / WiFi |
| DataSync built-in | ✅ | ✅ |
| EC2 on-device | ✅ (sbe2.medium) | ✅ (sbe2.medium) |
| Lambda on-device | ❌ | ❌ |
| Online transfer | ✅ via DataSync | ✅ via DataSync |

**Use case:** IoT edge, tactical/military, space-constrained locations, small migrations <14 TB.

---

### 29.3 AWS Snowball Edge

Główne urządzenie Snow (~22 kg). Dwa warianty:

### Storage Optimized vs Compute Optimized

| Parametr | Storage Optimized | Compute Optimized |
|----------|------------------|------------------|
| Storage użytkowy | **80 TB HDD** | **28 TB NVMe SSD** |
| Storage dla EC2 | 1 TB SSD NVMe | 28 TB NVMe |
| vCPU | 40 | 104 |
| RAM | 80 GB | 416 GB |
| GPU | ❌ | ✅ opcjonalnie (NVIDIA V100) |
| EC2 families | sbe1, sbe-c, sbe-m | sbe-c (compute-heavy) |
| Lambda | ✅ | ✅ |
| IoT Greengrass | ✅ | ✅ |
| EKS Anywhere | ✅ | ✅ |

**Clustering:** do 15 urządzeń razem. Minimalne quorum = 51% aktywnych.

**Use case Storage Optimized:** duże migracje danych, archiwizacja offline.
**Use case Compute Optimized:** HPC w terenie, ML inference, video analytics, GPU workloads.

---

### 29.4 AWS Snowmobile

Ciężarówka z opancerzonym kontenerem. 100 PB per truck.

| Parametr | Wartość |
|----------|---------|
| Pojemność | 100 PB |
| Transfer speed | do 1 Tb/s (dedicated fiber) |
| Szyfrowanie | AES-256, klucze klienta |
| Bezpieczeństwo | GPS, kamera, alarm, ochrona, eskort |
| Dostępność | Tylko USA, zamówienie przez kontakt z AWS |
| Edge compute | ❌ |

**Kiedy:** >10 PB → Snowmobile. <10 PB → wiele Snowball Edge równolegle (tańsze, szybsze logistycznie).

---

### 29.5 Tabela Porównawcza — Snow Family

| Parametr | Snowcone | Snowball Edge Storage | Snowball Edge Compute | Snowmobile |
|----------|----------|----------------------|----------------------|------------|
| **Storage** | 8/14 TB | 80 TB | 28 TB NVMe | 100 PB |
| **vCPU** | 2 | 40 | 104 | N/A |
| **RAM** | 4 GB | 80 GB | 416 GB | N/A |
| **GPU** | ❌ | ❌ | ✅ opcja | N/A |
| **EC2** | ✅ | ✅ | ✅ | ❌ |
| **Lambda** | ❌ | ✅ | ✅ | ❌ |
| **Clustering** | ❌ | ✅ 15 nodes | ✅ 15 nodes | N/A |
| **DataSync** | ✅ built-in | ❌ | ❌ | ❌ |
| **Online transfer** | ✅ | ❌ | ❌ | ❌ |
| **Waga** | 2 kg | ~22 kg | ~22 kg | ciężarówka |
| **Azure odpowiednik** | Data Box Disk | Data Box | Data Box Edge/Stack Edge | Data Box Heavy |

---

### 29.6 Edge Computing na Snow

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Console as AWS Console
    participant Snow as Snowball Edge
    participant S3 as Amazon S3

    Dev->>Console: Utwórz AMI + Lambda
    Console->>Snow: Pre-load AMI przed wysyłką
    Snow->>Snow: EC2 + Lambda on-device (bez internetu)
    Snow->>Snow: Przetwarzaj dane lokalnie
    Snow->>S3: Importuj wyniki po powrocie / sync online
```

**Typy instancji EC2 na Snow:**

| Rodzina | Odpowiednik | Dostępność |
|---------|------------|-----------|
| sbe2.medium | t3-like small | Snowcone |
| sbe1.* | m4 equivalent | Snowball Edge Storage |
| sbe-c.* | c5 equivalent | Snowball Edge Compute |
| sbe-m.* | m5 equivalent | Snowball Edge oba |

---

### 29.7 Proces Transferu Danych

```
1. Zamów job w AWS Console (import lub export)
2. AWS wysyła urządzenie (kilka dni)
3. Podłącz do sieci lokalnej
4. Unlock przez OpsHub lub CLI (manifest + unlock code z Console)
5. Kopiuj dane (S3-compatible API / NFS / CLI)
6. Zaplombuj i odeślij do AWS
7. AWS importuje dane do S3 (~2-3 dni)
8. AWS bezpiecznie wymazuje urządzenie (NIST 800-88)
9. Potwierdzenie w Console
```

```bash
# Unlock urządzenia
snowballEdge unlock-device \
  --endpoint https://192.168.1.100 \
  --manifest-file /path/manifest.bin \
  --unlock-code 1234-ABCD-5678-EFGH

# Kopiowanie przez S3-compatible API
aws s3 cp /data/ s3://mybucket/ \
  --endpoint-url http://192.168.1.100:8080 \
  --recursive

# NFS mount
mount -t nfs 192.168.1.100:/mybucket /mnt/snow
```

---

### 29.8 OpsHub

Darmowa GUI aplikacja (Mac/Windows) — alternatywa dla CLI.

| Funkcja | Opis |
|---------|------|
| Unlock device | Bez CLI |
| Launch EC2 | Zarządzanie instancjami |
| Manage files | Przeglądanie i kopiowanie |
| Monitor | CPU/RAM/storage |
| Configure network | IP, DNS |
| NFS shares | Tworzenie udziałów |

OpsHub łączy się bezpośrednio z urządzeniem przez LAN — nie wymaga internetu.

---

### 29.9 Szyfrowanie i Bezpieczeństwo

| Aspekt | Szczegóły |
|--------|-----------|
| Algorytm | AES-256-GCM |
| Klucze | KMS Customer Managed Keys |
| Klucze na urządzeniu | ❌ — nigdy nie opuszczają KMS |
| Tamper-evident | ✅ — widoczne ślady ingerencji fizycznej |
| TPM | ✅ — hardware security chip |
| Wymazanie po imporcie | ✅ — NIST 800-88 |
| Bezpieczeństwo Snowmobile | GPS + kamera + alarm + eskort + opancerzony kontener |

---

### 29.10 Snow vs Direct Connect vs DataSync — Decision Tree

| Dane | Bandwidth | Rekomendacja |
|------|-----------|-------------|
| <1 TB | Dowolny | Internet / S3 direct upload |
| 1–10 TB | >100 Mbps | DataSync |
| 1–10 TB | <10 Mbps | Snowcone |
| 10–80 TB | Brak lub słabe | Snowball Edge Storage |
| 10–80 TB | 10 Gbps DC | Direct Connect (szybciej) |
| 80 TB – 10 PB | Brak | Wiele Snowball Edge |
| >10 PB | N/A | Snowmobile |

---

### 29.11 Azure — Data Box Family

| AWS | Azure | Pojemność | Edge Compute |
|-----|-------|-----------|--------------|
| Snowcone | Data Box Disk | 35 TB (5×8 TB) | ❌ |
| Snowball Edge Storage | Data Box | 100 TB | ❌ |
| Snowball Edge Compute | Data Box Edge / Azure Stack Edge | compute + storage | ✅ |
| Snowmobile | Data Box Heavy | 1 PB | ❌ |

**Kluczowa różnica:** AWS Snow EC2 API on-device vs Azure Stack Edge — własny system. Snowmobile 100 PB nie ma rzeczywistego odpowiednika w Azure.

---

### 29.12 Well-Architected Framework

| Filar | Snow Family |
|-------|-------------|
| **Operational Excellence** | OpsHub GUI; automatyczny import; job tracking w Console |
| **Security** | AES-256 + KMS; klucze nigdy na urządzeniu; tamper-evident; NIST 800-88 erasure; chain of custody |
| **Reliability** | Clustering (15 nodes); DataSync retry dla Snowcone; redundantne nośniki |
| **Performance Efficiency** | Compute Optimized + GPU dla ML/HPC w terenie; NVMe dla szybkich odczytów |
| **Cost Optimization** | Offline transfer tańszy niż DC dla jednorazowych dużych migracji; płacisz za job nie za ciągłe łącze |
| **Sustainability** | AWS recykluje i reużywa urządzenia; jedno urządzenie = brak tygodni transmisji sieciowej |

---

### 29.13 4 Domeny Egzaminacyjne SAP-C02

### Domain 1: Design Solutions for Organizational Complexity
- Multi-account migration — import do konkretnego konta S3
- Centralized data lake — masowa migracja on-prem → S3 → Glue → Athena
- Edge locations bez łącza — fabryki, szpitale polowe, offshore

### Domain 2: Design for New Solutions
- Edge AI/ML — Snowball Edge Compute + GPU → inference bez cloud
- IoT data collection — Snowcone zbiera dane z sensorów
- Hybrid architecture — Snow jako extension AWS w terenie

### Domain 3: Continuous Improvement for Existing Solutions
- Skrócenie czasu migracji — wiele Snowball Edge równolegle
- Koszt vs czas — kalkulacja kiedy Snow tańszy od Direct Connect
- Automatyzacja po imporcie — S3 Event → Lambda → przetwarzanie

### Domain 4: Accelerate Workload Migration and Modernization
- Lift-and-shift datacenter — Snowmobile dla całych datacenter
- Application modernization — dane w S3 → migracja do cloud-native
- Database migration — export DB → Snow → S3 → DMS → RDS

---

## 30. Amazon SageMaker i AI/ML w AWS

### 30.1 Przegląd i Kontekst

Amazon SageMaker to w pełni zarządzana platforma ML (Machine Learning) w AWS, która pokrywa **cały cykl życia modelu** — od przygotowania danych, przez trenowanie, po wdrożenie i monitorowanie. To odpowiednik Azure Machine Learning / Azure AI Studio, Google Vertex AI lub open-source MLflow + Kubeflow.

**Kiedy SageMaker, kiedy coś innego?**

| Potrzeba | Usługa |
|---|---|
| Własny model, pełna kontrola | SageMaker (Studio, Training, Endpoints) |
| Gotowy model AI bez ML-dev | Amazon Bedrock (LLM), Rekognition, Comprehend |
| Małe ML na krawędzi | SageMaker Edge Manager / Neo |
| AI w aplikacji mobilnej | Amazon Rekognition, Comprehend, Translate |
| Generative AI / LLM | Amazon Bedrock (Claude, Llama, Titan) |
| Proste predykcje biznesowe | Amazon Forecast, Personalize |

```
┌─────────────────────────────────────────────────────────┐
│                    AI/ML w AWS                          │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │          WARSTWA AI (gotowe modele)             │   │
│  │  Rekognition │ Comprehend │ Translate │ Bedrock │   │
│  │  Polly │ Lex │ Forecast │ Personalize │ Textract│   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │          WARSTWA ML (własne modele)             │   │
│  │        Amazon SageMaker (end-to-end)            │   │
│  │  Studio │ Training │ Pipelines │ Endpoints       │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │          WARSTWA COMPUTE/DATA                   │   │
│  │  EC2 (GPU) │ S3 │ EMR │ Glue │ Athena │ Redshift│   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

### 30.2 Amazon SageMaker — Architektura i Składniki

SageMaker to **nie jedna usługa** — to rodzina usług pod wspólną marką.

### Główne składniki

| Składnik | Co robi | Analogia |
|---|---|---|
| **SageMaker Studio** | IDE/UI dla całego ML workflow | VS Code dla ML |
| **SageMaker Studio Classic** | Poprzednia wersja Studio (Jupyter-based) | Legacy IDE |
| **SageMaker Notebooks** | Managed Jupyter Notebooks | Azure Notebooks |
| **SageMaker Training** | Managed distributed training | Azure ML Training |
| **SageMaker Processing** | ETL/preprocessing jako managed jobs | Spark Batch Job |
| **SageMaker Pipelines** | ML CI/CD — automatyzacja workflow | Azure ML Pipelines |
| **SageMaker Experiments** | Tracking eksperymentów ML | MLflow Tracking |
| **SageMaker Model Registry** | Wersjonowanie modeli | MLflow Model Registry |
| **SageMaker Endpoints** | Real-time inference hosting | Azure ML Endpoints |
| **SageMaker Batch Transform** | Bulk inference offline | Azure Batch Scoring |
| **SageMaker Neo** | Kompilacja modeli na edge/IoT | TensorFlow Lite |
| **SageMaker Edge Manager** | Deploy + monitor modeli na urządzeniach edge | Azure IoT Edge |
| **SageMaker Clarify** | Bias detection + explainability | Azure Responsible AI |
| **SageMaker Data Wrangler** | No-code data prep | Azure Data Factory |
| **SageMaker Feature Store** | Centralne repo features ML | Feast (open-source) |
| **SageMaker Ground Truth** | Data labeling (human-in-the-loop) | Azure ML Labeling |
| **SageMaker Autopilot** | AutoML — automatyczne budowanie modeli | Azure AutoML |
| **SageMaker Canvas** | No-code ML dla biznesu | Power BI ML |
| **SageMaker JumpStart** | Gotowe modele + rozwiązania (model hub) | Azure Model Catalog |
| **SageMaker HyperPod** | Distributed training na dużą skalę | Azure ND-series clusters |
| **SageMaker Inference Recommender** | Doradca wyboru instancji do inference | — |

---

### 30.3 SageMaker Studio — Centrum Operacyjne

**SageMaker Studio** to unified IDE — przeglądarka internetowa dająca dostęp do całego ekosystemu ML.

### Konfiguracja Studio

```bash
# Stworzenie domeny Studio
aws sagemaker create-domain \
  --domain-name my-ml-domain \
  --auth-mode IAM \
  --default-user-settings '{
    "ExecutionRole": "arn:aws:iam::123456789012:role/SageMakerRole",
    "SecurityGroups": ["sg-xxxxxxxx"]
  }' \
  --vpc-id vpc-xxxxxxxx \
  --subnet-ids subnet-xxxxxxxx subnet-yyyyyyyy

# Dodanie user profile
aws sagemaker create-user-profile \
  --domain-id d-xxxxxxxxxx \
  --user-profile-name data-scientist-1
```

### Typy przestrzeni w Studio

| Typ | Opis | Kiedy używać |
|---|---|---|
| **JupyterServer** | Klasyczny Jupyter | Eksploracja, prototypowanie |
| **KernelGateway** | Kernel dla notebooków | Compute-intensive work |
| **TensorBoard** | Wizualizacja trenowania | Debugging modeli |
| **RStudio** | IDE dla R | Analiza statystyczna |
| **Code Editor** | VS Code-based IDE | Developerzy |

### Izolacja i sieć

- Studio działa **w VPC** — pełna izolacja sieciowa
- Wspiera **VPC-only mode** — brak dostępu do internetu (tylko PrivateLink)
- Każdy użytkownik ma własny **EFS home directory** (persistowane między sesjami)
- **Domain-level** vs **User-level** security settings

---

### 30.4 SageMaker Training — Trenowanie Modeli

### Jak działa Training Job

```mermaid
flowchart LR
    S3[(S3\nTraining Data)] --> TJ[Training Job\nContainer]
    ECR[(ECR\nCustom Image)] --> TJ
    TJ --> S3M[(S3\nModel Artifacts)]
    TJ --> CW[CloudWatch\nLogs & Metrics]
    TJ --> ES[Experiments\nTracking]
```

```bash
# Uruchomienie Training Job
aws sagemaker create-training-job \
  --training-job-name my-training-job \
  --algorithm-specification '{
    "TrainingImage": "123456789012.dkr.ecr.us-east-1.amazonaws.com/my-algo:latest",
    "TrainingInputMode": "File"
  }' \
  --role-arn arn:aws:iam::123456789012:role/SageMakerRole \
  --input-data-config '[{
    "ChannelName": "training",
    "DataSource": {
      "S3DataSource": {
        "S3DataType": "S3Prefix",
        "S3Uri": "s3://my-bucket/training-data/",
        "S3DataDistributionType": "FullyReplicated"
      }
    }
  }]' \
  --output-data-config '{"S3OutputPath": "s3://my-bucket/model-output/"}' \
  --resource-config '{
    "InstanceType": "ml.p3.2xlarge",
    "InstanceCount": 1,
    "VolumeSizeInGB": 50
  }' \
  --stopping-condition '{"MaxRuntimeInSeconds": 86400}'
```

### Tryby dystrybucji danych

| Tryb | Opis | Kiedy |
|---|---|---|
| `FullyReplicated` | Wszystkie dane na każdym węźle | Mały dataset |
| `ShardedByS3Key` | Każdy węzeł dostaje podzbiór | Duży dataset, distributed training |
| `FastFile` | Streaming z S3 (bez kopiowania) | Bardzo duże pliki, niska latencja startu |
| `Pipe` | Streaming przez FIFO pipe | Legacy, duże dane |

### Typy instancji ML

| Rodzina | GPU | Zastosowanie |
|---|---|---|
| `ml.p3.*` | NVIDIA V100 | Deep learning training |
| `ml.p4d.*` | NVIDIA A100 | Large scale DL, Transformer |
| `ml.p5.*` | NVIDIA H100 | Najnowsze LLM training |
| `ml.g4dn.*` | NVIDIA T4 | Inference, cost-effective training |
| `ml.g5.*` | NVIDIA A10G | Vision, NLP inference |
| `ml.trn1.*` | AWS Trainium | Cost-optimized DL training |
| `ml.inf1.*` | AWS Inferentia | Cost-optimized inference |
| `ml.inf2.*` | AWS Inferentia2 | Gen AI inference |
| `ml.c5.*` | CPU | Klasyczne ML (XGBoost, sklearn) |
| `ml.m5.*` | CPU | General purpose |

> **Ważne na egzamin:** AWS Trainium (trn1) = najtańszy training DL. AWS Inferentia (inf1/inf2) = najtańszy inference DL. Graviton-based `ml.c7g.*` = najtańszy CPU inference.

### Distributed Training Strategies

```bash
# Distributed Data Parallel (DDP) — każdy węzeł ma pełny model, różne dane
# Model Parallel (SMP) — model podzielony między węzły (duże modele nie mieszczą się w 1 GPU)

# SageMaker Model Parallel Library
import smdistributed.modelparallel.torch as smp

smp.init({
    "microbatches": 4,
    "placement_strategy": "spread",
    "pipeline": "interleaved",
    "tensor_parallel_degree": 4,
    "ddp": True
})
```

---

### 30.5 SageMaker Endpoints — Inference

### Real-Time Inference

```bash
# Deploy model do endpoint
aws sagemaker create-model \
  --model-name my-model \
  --primary-container '{
    "Image": "382416733822.dkr.ecr.us-east-1.amazonaws.com/xgboost:1.5-1",
    "ModelDataUrl": "s3://my-bucket/model-output/model.tar.gz"
  }' \
  --execution-role-arn arn:aws:iam::123456789012:role/SageMakerRole

aws sagemaker create-endpoint-config \
  --endpoint-config-name my-endpoint-config \
  --production-variants '[{
    "VariantName": "AllTraffic",
    "ModelName": "my-model",
    "InitialInstanceCount": 1,
    "InstanceType": "ml.m5.xlarge",
    "InitialVariantWeight": 1.0
  }]'

aws sagemaker create-endpoint \
  --endpoint-name my-endpoint \
  --endpoint-config-name my-endpoint-config
```

### Typy Inference

| Typ | Latencja | Kiedy | Koszt |
|---|---|---|---|
| **Real-Time Endpoint** | Milisekundy | Aplikacje online, < 60s response | Stały (instancja zawsze działa) |
| **Serverless Inference** | Sekundy (cold start) | Rzadki ruch, bursty | Pay-per-request |
| **Batch Transform** | Minuty-godziny | Bulk offline scoring | Pay-per-job |
| **Async Inference** | Sekundy-minuty | Duże payloady (video, audio) | Stały + kolejka |

### A/B Testing — Multi-Variant Endpoint

```bash
# Traffic split między dwa modele
aws sagemaker create-endpoint-config \
  --endpoint-config-name ab-test-config \
  --production-variants '[
    {
      "VariantName": "ModelA",
      "ModelName": "model-v1",
      "InitialInstanceCount": 1,
      "InstanceType": "ml.m5.large",
      "InitialVariantWeight": 0.9
    },
    {
      "VariantName": "ModelB",
      "ModelName": "model-v2",
      "InitialInstanceCount": 1,
      "InstanceType": "ml.m5.large",
      "InitialVariantWeight": 0.1
    }
  ]'
```

### Auto Scaling Endpointów

```bash
# Application Auto Scaling dla SageMaker Endpoints
aws application-autoscaling register-scalable-target \
  --service-namespace sagemaker \
  --resource-id endpoint/my-endpoint/variant/AllTraffic \
  --scalable-dimension sagemaker:variant:DesiredInstanceCount \
  --min-capacity 1 \
  --max-capacity 10

aws application-autoscaling put-scaling-policy \
  --policy-name scaling-policy \
  --service-namespace sagemaker \
  --resource-id endpoint/my-endpoint/variant/AllTraffic \
  --scalable-dimension sagemaker:variant:DesiredInstanceCount \
  --policy-type TargetTrackingScaling \
  --target-tracking-scaling-policy-configuration '{
    "TargetValue": 70.0,
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "SageMakerVariantInvocationsPerInstance"
    }
  }'
```

---

### 30.6 SageMaker Pipelines — MLOps i CI/CD dla ML

SageMaker Pipelines to **DAG-based** ML workflow orchestrator — analogia do Apache Airflow, Azure ML Pipelines, Kubeflow Pipelines.

```python
from sagemaker.workflow.pipeline import Pipeline
from sagemaker.workflow.steps import ProcessingStep, TrainingStep
from sagemaker.workflow.model_step import ModelStep

# Krok 1: Preprocessing
processing_step = ProcessingStep(
    name="PreprocessData",
    processor=sklearn_processor,
    inputs=[...],
    outputs=[...]
)

# Krok 2: Training
training_step = TrainingStep(
    name="TrainModel",
    estimator=xgb_estimator,
    inputs={"train": training_input}
)
training_step.add_depends_on([processing_step])

# Krok 3: Conditional — deploy tylko jeśli accuracy > threshold
from sagemaker.workflow.conditions import ConditionGreaterThanOrEqualTo
from sagemaker.workflow.condition_step import ConditionStep

condition = ConditionGreaterThanOrEqualTo(
    left=JsonGet(step_name="EvaluateModel", property_file=evaluation_report, json_path="metrics.accuracy.value"),
    right=0.85
)

condition_step = ConditionStep(
    name="CheckAccuracy",
    conditions=[condition],
    if_steps=[deploy_step],
    else_steps=[fail_step]
)

pipeline = Pipeline(
    name="my-ml-pipeline",
    steps=[processing_step, training_step, evaluation_step, condition_step]
)
pipeline.upsert(role_arn=role)
pipeline.start()
```

### Pipeline vs Step Functions vs MWAA (Airflow)

| | SageMaker Pipelines | Step Functions | MWAA (Airflow) |
|---|---|---|---|
| **Cel** | ML-specific workflow | Ogólny serverless workflow | Ogólny orchestration |
| **Integracja ML** | Natywna (model registry, experiments) | Wymaga custom state | Via operators |
| **UI** | Studio visual DAG | Step Functions console | Airflow UI |
| **Triggerowanie** | API, EventBridge, manualnie | API, EventBridge, cron | Cron, sensors, API |
| **Koszty** | Per step execution | Per state transition | Per environment + EC2 |
| **Kiedy** | Typowy ML workflow | Mix ML + inne serwisy | Złożone zależności, multi-team |

---

### 30.7 SageMaker Feature Store

**Feature Store** = centralne repozytorium obliczonych cech (features) ML — zapobiega "feature skew" między trenowaniem a inference.

```mermaid
flowchart TD
    RAW[(Raw Data\nS3/DB)] --> FT[Feature\nTransformation\nProcessing Job]
    FT --> OFS[Online Feature Store\nElastiCache-backed\nlow-latency]
    FT --> OFS2[Offline Feature Store\nS3-backed\nhistorical]
    OFS --> INF[Real-time\nInference]
    OFS2 --> TRAIN[Model\nTraining]
```

```bash
# Tworzenie Feature Group
aws sagemaker create-feature-group \
  --feature-group-name customer-features \
  --record-identifier-feature-name customer_id \
  --event-time-feature-name event_time \
  --feature-definitions '[
    {"FeatureName": "customer_id", "FeatureType": "String"},
    {"FeatureName": "age", "FeatureType": "Integral"},
    {"FeatureName": "purchase_amount", "FeatureType": "Fractional"},
    {"FeatureName": "event_time", "FeatureType": "String"}
  ]' \
  --online-store-config '{"EnableOnlineStore": true}' \
  --offline-store-config '{
    "S3StorageConfig": {"S3Uri": "s3://my-bucket/feature-store/"}
  }' \
  --role-arn arn:aws:iam::123456789012:role/SageMakerRole
```

| Cecha | Online Store | Offline Store |
|---|---|---|
| **Backend** | In-memory (managed) | S3 (Parquet/Iceberg) |
| **Latencja** | Milisekundy | Sekundy-minuty |
| **Zastosowanie** | Real-time inference | Training, batch |
| **Retencja** | Tylko ostatni rekord | Cała historia |
| **Cena** | Wyższy koszt/rekord | Niski (S3) |

---

### 30.8 SageMaker Model Monitor — Monitorowanie Dryftu

Model Monitor automatycznie wykrywa **data drift**, **model drift**, **bias drift**, **feature attribution drift**.

```bash
# Tworzenie monitoring schedule
aws sagemaker create-monitoring-schedule \
  --monitoring-schedule-name my-monitor \
  --monitoring-schedule-config '{
    "ScheduleConfig": {"ScheduleExpression": "cron(0 * ? * * *)"},
    "MonitoringJobDefinition": {
      "MonitoringInputs": [{
        "EndpointInput": {
          "EndpointName": "my-endpoint",
          "LocalPath": "/opt/ml/processing/input/endpoint"
        }
      }],
      "MonitoringOutputConfig": {
        "MonitoringOutputs": [{
          "S3Output": {
            "S3Uri": "s3://my-bucket/monitoring/",
            "LocalPath": "/opt/ml/processing/output"
          }
        }]
      },
      "MonitoringResources": {
        "ClusterConfig": {
          "InstanceCount": 1,
          "InstanceType": "ml.m5.xlarge",
          "VolumeSizeInGB": 20
        }
      },
      "MonitoringAppSpecification": {
        "ImageUri": "156813124566.dkr.ecr.us-east-1.amazonaws.com/sagemaker-model-monitor-analyzer"
      },
      "RoleArn": "arn:aws:iam::123456789012:role/SageMakerRole"
    }
  }'
```

### Typy monitoringu

| Typ | Co wykrywa | Kiedy ważne |
|---|---|---|
| **Data Quality** | Zmiana statystyk wejściowych (mean, std, null%) | Zmiana upstream danych |
| **Model Quality** | Degradacja accuracy, F1, AUC | Regularny model monitoring |
| **Bias Drift** | Zmiana stronniczości modelu | Compliance, fairness |
| **Feature Attribution Drift** | Zmiana ważności cech (SHAP) | Interpretability |

---

### 30.9 SageMaker Clarify — Bias i Explainability

Clarify analizuje:
- **Pre-training bias** — czy dane treningowe są stronnicze?
- **Post-training bias** — czy model dyskryminuje grupy?
- **Explainability** — które cechy wpływają na predykcję (SHAP values)?

```python
from sagemaker import clarify

clarify_processor = clarify.SageMakerClarifyProcessor(
    role=role,
    instance_count=1,
    instance_type='ml.m5.xlarge',
    sagemaker_session=session
)

bias_config = clarify.BiasConfig(
    label_values_or_threshold=[1],
    facet_name='Age',
    facet_values_or_threshold=[40]  # bias względem osób < 40
)

clarify_processor.run_bias(
    data_config=data_config,
    bias_config=bias_config,
    model_config=model_config,
    pre_training_methods='all',
    post_training_methods='all'
)
```

**Metryki bias (ważne na egzamin):**

| Metryka | Co mierzy |
|---|---|
| **CI (Class Imbalance)** | Nierówna reprezentacja grup |
| **DPL (Diff in Positive Proportions)** | Różnica w pozytywnych wynikach między grupami |
| **AD (Accuracy Difference)** | Różnica accuracy między grupami |
| **DPPL** | Różnica w predicted positive labels |

---

### 30.10 Amazon Bedrock — Generative AI

Amazon Bedrock to **w pełni zarządzane** API do Foundation Models (FM) — LLM, image generation, multimodal. Nie wymaga infrastruktury ML.

### Dostępne modele (2024/2025)

| Provider | Model | Zastosowanie |
|---|---|---|
| **Anthropic** | Claude 3 (Haiku, Sonnet, Opus), Claude 3.5 | Tekst, analiza, kod |
| **Amazon** | Titan Text, Titan Embeddings, Titan Image | Amazon-native |
| **Meta** | Llama 3, Llama 3.1 | Open-source LLM |
| **Mistral AI** | Mistral 7B, Mixtral 8x7B | Europejski LLM |
| **Stability AI** | Stable Diffusion XL | Generowanie obrazów |
| **Cohere** | Command R, Embed | RAG, embeddings |
| **AI21 Labs** | Jamba, Jurassic | Tekst długiego kontekstu |

```bash
# Wywołanie modelu przez Bedrock
aws bedrock-runtime invoke-model \
  --model-id anthropic.claude-3-sonnet-20240229-v1:0 \
  --body '{
    "anthropic_version": "bedrock-2023-05-31",
    "max_tokens": 1024,
    "messages": [{
      "role": "user",
      "content": "Explain AWS SageMaker in 3 sentences."
    }]
  }' \
  --cli-binary-format raw-in-base64-out \
  output.json
```

### Bedrock — Kluczowe Funkcje

| Funkcja | Opis |
|---|---|
| **Knowledge Bases** | RAG (Retrieval Augmented Generation) — własna wiedza |
| **Agents** | Automatyczne orchestrowanie multi-step tasków |
| **Guardrails** | Filtry bezpieczeństwa treści (PII, toxicity) |
| **Model Evaluation** | Automatyczna ocena jakości modelu |
| **Fine-tuning** | Dostosowanie modelu do własnych danych |
| **Provisioned Throughput** | Dedykowany throughput (nie shared) |
| **Model Invocation Logging** | Audyt wszystkich wywołań → S3/CloudWatch |

### Bedrock Knowledge Bases — RAG Architecture

```mermaid
flowchart LR
    DOCS[(Documents\nS3)] --> KB[Knowledge Base\nEmbedding + Vector Store]
    KB --> OS[(OpenSearch\nServerless\nVector DB)]
    USER[User Query] --> AG[Bedrock Agent]
    AG --> OS
    OS --> AG
    AG --> FM[Foundation Model\nClaude/Titan]
    FM --> AG
    AG --> USER
```

```bash
# Tworzenie Knowledge Base
aws bedrock create-knowledge-base \
  --name my-kb \
  --role-arn arn:aws:iam::123456789012:role/BedrockRole \
  --knowledge-base-configuration '{
    "type": "VECTOR",
    "vectorKnowledgeBaseConfiguration": {
      "embeddingModelArn": "arn:aws:bedrock:us-east-1::foundation-model/amazon.titan-embed-text-v1"
    }
  }' \
  --storage-configuration '{
    "type": "OPENSEARCH_SERVERLESS",
    "opensearchServerlessConfiguration": {
      "collectionArn": "arn:aws:aoss:us-east-1:123456789012:collection/my-collection",
      "vectorIndexName": "my-index",
      "fieldMapping": {
        "vectorField": "embedding",
        "textField": "text",
        "metadataField": "metadata"
      }
    }
  }'
```

### Bedrock Guardrails

```bash
aws bedrock create-guardrail \
  --name content-filter \
  --content-policy-config '{
    "filtersConfig": [
      {"type": "SEXUAL", "inputStrength": "HIGH", "outputStrength": "HIGH"},
      {"type": "VIOLENCE", "inputStrength": "MEDIUM", "outputStrength": "MEDIUM"},
      {"type": "HATE", "inputStrength": "HIGH", "outputStrength": "HIGH"}
    ]
  }' \
  --sensitive-information-policy-config '{
    "piiEntitiesConfig": [
      {"type": "EMAIL", "action": "ANONYMIZE"},
      {"type": "PHONE", "action": "BLOCK"}
    ]
  }'
```

### Bedrock vs SageMaker vs Third-party

| | Bedrock | SageMaker JumpStart | SageMaker Training |
|---|---|---|---|
| **Model ownership** | AWS/Partner managed | Własny fine-tune | Pełna kontrola |
| **Infrastructure** | Bezserwerowe API | Managed endpoints | Managed + customize |
| **Fine-tuning** | Tak (ograniczone) | Tak | Pełna dowolność |
| **Koszt** | Pay-per-token | Pay-per-endpoint | Pay-per-training-hour |
| **Kiedy** | Gotowe LLM szybko | Popularny model + customizacja | Własny model od zera |
| **Azure equiv** | Azure OpenAI Service | Azure Model Catalog | Azure ML Training |

---

### 30.11 AI Services (Pre-built) — Gotowe Modele AWS

### Amazon Rekognition — Computer Vision

```bash
# Detekcja obiektów
aws rekognition detect-labels \
  --image '{"S3Object": {"Bucket": "my-bucket", "Name": "photo.jpg"}}' \
  --max-labels 10 \
  --min-confidence 75

# Rozpoznanie twarzy
aws rekognition compare-faces \
  --source-image '{"S3Object": {"Bucket": "my-bucket", "Name": "reference.jpg"}}' \
  --target-image '{"S3Object": {"Bucket": "my-bucket", "Name": "unknown.jpg"}}' \
  --similarity-threshold 90

# Moderacja treści
aws rekognition detect-moderation-labels \
  --image '{"S3Object": {"Bucket": "my-bucket", "Name": "image.jpg"}}'
```

**Rekognition — funkcje:**
- `DetectLabels` — obiekty, sceny, aktywności
- `DetectFaces` — atrybuty twarzy (wiek, emocje, okulary)
- `CompareFaces` — weryfikacja tożsamości (1:1)
- `SearchFacesByImage` — wyszukiwanie twarzy w kolekcji (1:N)
- `DetectText` — OCR w obrazach
- `DetectModerationLabels` — NSFW, niebezpieczne treści
- `RecognizeCelebrities` — rozpoznanie znanych osób
- `StartLabelDetection` — analiza video (async)

**Regionalność:** Regionalny. Dane nie opuszczają regionu.

### Amazon Comprehend — NLP

```bash
# Analiza sentymentu
aws comprehend detect-sentiment \
  --text "AWS is absolutely amazing for machine learning!" \
  --language-code en

# Wykrywanie encji
aws comprehend detect-entities \
  --text "John Smith works at Amazon in Seattle." \
  --language-code en

# Detekcja PII
aws comprehend detect-pii-entities \
  --text "My email is john@example.com and SSN is 123-45-6789" \
  --language-code en

# Custom Classifier (własny model)
aws comprehend create-document-classifier \
  --document-classifier-name ticket-classifier \
  --data-access-role-arn arn:aws:iam::123456789012:role/ComprehendRole \
  --input-data-config '{"S3Uri": "s3://my-bucket/training-data/"}' \
  --output-data-config '{"S3Uri": "s3://my-bucket/output/"}' \
  --language-code en
```

**Comprehend — funkcje:**
- Sentiment Analysis (POSITIVE/NEGATIVE/NEUTRAL/MIXED)
- Entity Recognition (osoby, miejsca, organizacje, daty)
- Key Phrase Extraction
- Language Detection
- PII Detection (GDPR compliance)
- **Custom Classification** — własne kategorie
- **Custom Entity Recognition** — własne encje
- **Topic Modeling** — LDA clustering dokumentów
- **Comprehend Medical** — encje medyczne (diagnoza, leki, dawki)

### Amazon Textract — Document Analysis

```bash
# Ekstrakcja tekstu z PDF/obrazu
aws textract detect-document-text \
  --document '{"S3Object": {"Bucket": "my-bucket", "Name": "invoice.pdf"}}'

# Ekstrakcja formularzy (key-value pairs)
aws textract analyze-document \
  --document '{"S3Object": {"Bucket": "my-bucket", "Name": "form.pdf"}}' \
  --feature-types '["FORMS", "TABLES", "SIGNATURES"]'

# Async dla dużych dokumentów
aws textract start-document-analysis \
  --document-location '{"S3Object": {"Bucket": "my-bucket", "Name": "contract.pdf"}}' \
  --feature-types '["FORMS", "TABLES"]' \
  --notification-channel '{"SNSTopicArn": "arn:aws:sns:...", "RoleArn": "..."}'
```

**Textract vs Rekognition DetectText:**

| | Textract | Rekognition DetectText |
|---|---|---|
| **Cel** | Dokumenty strukturyzowane | Tekst w scenach/obrazach |
| **Formularze** | Tak (key-value) | Nie |
| **Tabele** | Tak | Nie |
| **PDF** | Tak | Nie |
| **Dokładność** | Wyższa dla dokumentów | Lepsza dla znaków ulicznych |

### Amazon Polly — Text-to-Speech

```bash
# Synteza mowy
aws polly synthesize-speech \
  --output-format mp3 \
  --voice-id Joanna \
  --text "Welcome to AWS certification training" \
  speech.mp3

# SSML — kontrola wymowy
aws polly synthesize-speech \
  --output-format mp3 \
  --voice-id Matthew \
  --text-type ssml \
  --text '<speak>The price is <say-as interpret-as="currency">$42.50</say-as></speak>' \
  speech_ssml.mp3
```

**Polly — typy głosów:**
- **Standard voices** — tańsze, mniej naturalne
- **Neural voices** (NTTS) — droższe, bardziej naturalne
- **Long-form voices** — do długich treści (audiobook)
- **Generative voices** — generatywne AI, najbardziej naturalne

### Amazon Lex — Konwersacyjne AI (Chatbot)

```bash
# Wysłanie wiadomości do Lex bota
aws lex-runtime-v2 recognize-text \
  --bot-id ABCDE12345 \
  --bot-alias-id TSTALIASID \
  --locale-id en_US \
  --session-id session-123 \
  --text "I want to book a flight to Warsaw"
```

**Lex architektura:**
- **Intents** — zamiary użytkownika (BookFlight, CancelOrder)
- **Slots** — zmienne do wypełnienia (skąd, dokąd, kiedy)
- **Utterances** — przykładowe zdania trenujące
- **Fulfillment** — Lambda do obsługi logiki biznesowej
- Integracja z **Connect** (call center), **Kendra** (search)

**Lex vs Bedrock Agents:**

| | Lex | Bedrock Agents |
|---|---|---|
| **Cel** | Strukturyzowany dialog, IVR | Autonomiczne agenty AI |
| **Model** | Custom intent/slot | Foundation Model |
| **Flexibility** | Ograniczona | Wysoka |
| **Kodowanie** | Mało | Lambda/API tools |
| **Kiedy** | Call center, FAQ bot | Complex reasoning tasks |

### Amazon Kendra — Intelligent Search

```bash
# Tworzenie indeksu Kendra
aws kendra create-index \
  --name my-knowledge-base \
  --edition ENTERPRISE_EDITION \
  --role-arn arn:aws:iam::123456789012:role/KendraRole

# Dodanie S3 data source
aws kendra create-data-source \
  --index-id idx-xxxxxxxx \
  --name s3-docs \
  --type S3 \
  --configuration '{
    "S3Configuration": {
      "BucketName": "my-docs-bucket",
      "InclusionPrefixes": ["docs/"]
    }
  }' \
  --role-arn arn:aws:iam::123456789012:role/KendraRole

# Wyszukiwanie
aws kendra query \
  --index-id idx-xxxxxxxx \
  --query-text "How do I reset my password?"
```

**Kendra — źródła danych:**
- S3, SharePoint, Confluence, Salesforce, ServiceNow
- RDS, OneDrive, Google Drive, GitHub
- Custom connectors via API

**Kendra vs OpenSearch vs Bedrock Knowledge Bases:**

| | Kendra | OpenSearch | Bedrock KB |
|---|---|---|---|
| **Typ** | Semantic + keyword | Keyword (+ vector plugin) | Vector (semantic) |
| **Setup** | Zarządzany, łatwy | Wymaga konfiguracji | Managed, prosty |
| **ML** | Wbudowane NLP | Manualne | Wbudowane embeddings |
| **Kiedy** | Enterprise search, duże org | Custom search, high-scale | RAG dla LLM |
| **Koszt** | Wysoki ($700+/miesiąc Enterprise) | Per instance | Per query |

### Amazon Forecast — Prognozowanie Szeregów Czasowych

```bash
# Import danych historycznych
aws forecast create-dataset-import-job \
  --dataset-import-job-name sales-import \
  --dataset-arn arn:aws:forecast:us-east-1:123456789012:dataset/sales \
  --data-source '{
    "S3Config": {
      "Path": "s3://my-bucket/historical-sales/",
      "RoleArn": "arn:aws:iam::123456789012:role/ForecastRole"
    }
  }'

# Trenowanie predyktora
aws forecast create-predictor \
  --predictor-name sales-predictor \
  --forecast-horizon 30 \
  --dataset-group-arn arn:aws:forecast:...:dataset-group/sales-dg \
  --algorithm-arn arn:aws:forecast:::algorithm/Auto
```

**Obsługiwane algorytmy Forecast:**
- `Auto` — AutoML wybiera najlepszy
- `DeepAR+` — RNN dla wielu szeregów
- `CNN-QR` — CNN z quantile regression
- `ETS`, `ARIMA`, `Prophet` — klasyczne algorytmy
- `NPTS` — Non-Parametric Time Series

**Forecast vs QuickSight ML Insights vs SageMaker:**

| | Forecast | QS ML Insights | SageMaker |
|---|---|---|---|
| **Cel** | Dedykowane forecasting | Wizualizacja + auto forecast | Własny model |
| **ML expertise** | Nie wymagane | Nie wymagane | Wymagane |
| **Kontrola** | Ograniczona | Minimalna | Pełna |

### Amazon Personalize — Rekomendacje

```bash
# Wgranie danych interakcji user-item
aws personalize create-dataset-import-job \
  --job-name interactions-import \
  --dataset-arn arn:aws:personalize:...:dataset/interactions \
  --data-source '{"dataLocation": "s3://my-bucket/interactions.csv"}' \
  --role-arn arn:aws:iam::123456789012:role/PersonalizeRole

# Trenowanie Solution (model)
aws personalize create-solution \
  --name product-recommendations \
  --dataset-group-arn arn:aws:personalize:...:dataset-group/retail-dg \
  --recipe-arn arn:aws:personalize:::recipe/aws-user-personalization

# Deployment do Campaign
aws personalize create-campaign \
  --name prod-recommendations \
  --solution-version-arn arn:aws:personalize:...:solution/...:1 \
  --min-provisioned-tps 1
```

**Personalize — przepisy (recipes):**

| Recipe | Opis |
|---|---|
| `User-Personalization` | Top-N rekomendacje dla użytkownika |
| `Popularity-Count` | Najpopularniejsze itemy (cold start) |
| `HRNN-Metadata` | Z atrybutami użytkownika/itemu |
| `Similar-Items` | "Klienci którzy kupili X kupili też..." |
| `Personalized-Ranking` | Sortowanie listy dla użytkownika |

### Amazon Translate — Tłumaczenie

```bash
aws translate translate-text \
  --source-language-code auto \
  --target-language-code pl \
  --text "Machine learning is transforming every industry."
```

**Translate — funkcje:**
- Auto-detect języka źródłowego
- 75+ języków
- Custom Terminology — własny słownik (terminologia techniczna)
- Parallel Data — dostosowanie tłumaczenia stylem

### Amazon Transcribe — Speech-to-Text

```bash
# Transkrypcja pliku audio
aws transcribe start-transcription-job \
  --transcription-job-name my-job \
  --language-code en-US \
  --media '{"MediaFileUri": "s3://my-bucket/audio.mp3"}' \
  --output-bucket-name my-output-bucket \
  --settings '{
    "ShowSpeakerLabels": true,
    "MaxSpeakerLabels": 5,
    "VocabularyName": "custom-vocab"
  }'
```

**Transcribe — funkcje:**
- Speaker diarization (kto mówi?)
- Custom Vocabulary (terminy techniczne/medyczne)
- Custom Language Model (domain-specific)
- PII Redaction (usuwanie danych osobowych z transkrypcji)
- **Transcribe Medical** — specjalizacja dla ochrony zdrowia
- Real-time streaming transcription

---

### 30.12 AWS Panorama — Edge Computer Vision

Panorama = urządzenie/SDK do uruchamiania CV (computer vision) na kamerach przemysłowych bez wysyłania wideo do chmury.

```
┌─────────────┐    Model z        ┌──────────────┐
│  IP Camera  │ ──────────────── │ Panorama     │
│  (RTSP)     │                  │ Appliance    │
└─────────────┘                  │ (edge device)│
                                 └──────┬───────┘
                                        │ Wyniki
                                        ▼
                                 ┌──────────────┐
                                 │  CloudWatch  │
                                 │  S3 (clips)  │
                                 └──────────────┘
```

---

### 30.13 Amazon Lookout — Anomaly Detection

| Usługa | Cel |
|---|---|
| **Lookout for Metrics** | Anomalie w metrykach biznesowych (sprzedaż, ruch) |
| **Lookout for Equipment** | Anomalie w danych przemysłowych IoT |
| **Lookout for Vision** | Defekty wizualne na linii produkcyjnej |

```bash
# Lookout for Metrics — tworzenie alertera
aws lookoutmetrics create-anomaly-detector \
  --anomaly-detector-name sales-detector \
  --anomaly-detector-config '{"AnomalyDetectorFrequency": "PT1H"}'
```

---

### 30.14 Amazon CodeGuru — AI Code Review

CodeGuru = AI-powered code review + profiling wydajności.

| Składnik | Opis |
|---|---|
| **CodeGuru Reviewer** | Statyczna analiza kodu (security, bugs) — PR review |
| **CodeGuru Profiler** | Runtime profiling (CPU hotspots, memory) |

```bash
# Asocjacja repozytorium
aws codeguru-reviewer associate-repository \
  --repository '{"CodeCommit": {"Name": "my-repo"}}'
```

---

### 30.15 AWS DeepLearning AMIs i Containers

**Deep Learning AMI (DLAMI)** — pre-konfigurowane AMI z zainstalowanymi frameworkami:
- TensorFlow, PyTorch, MXNet
- CUDA, cuDNN, NCCL
- Conda environments

```bash
# Listowanie DLAMI
aws ec2 describe-images \
  --owners amazon \
  --filters 'Name=name,Values=Deep Learning AMI*' \
  --query 'Images[*].[ImageId,Name]' \
  --output table
```

**Deep Learning Containers (DLC)** — Docker images w ECR dla SageMaker i EKS.

---

### 30.16 AWS Trainium i Inferentia — Custom ML Chips

### AWS Trainium

- Chip zaprojektowany przez AWS **wyłącznie do trenowania DL**
- Używany w instancjach `ml.trn1.*` (SageMaker) i `trn1.*` (EC2)
- Do 50% taniej niż GPU (p3/p4) dla tego samego workloadu
- Obsługuje PyTorch, TensorFlow przez **AWS Neuron SDK**

```bash
# Trening na Trainium
pip install torch-neuronx neuronx-cc

# W kodzie PyTorch
import torch_xla.core.xla_model as xm
device = xm.xla_device()  # automatycznie używa Trainium
```

### AWS Inferentia

- Chip zaprojektowany **wyłącznie do inference**
- `inf1.*` = Inferentia 1 (starsza generacja)
- `inf2.*` = Inferentia 2 (lepsza, nowsza)
- Do 70% taniej niż GPU dla inference

```bash
# Kompilacja modelu dla Inferentia
neuron-cc compile model.pb \
  --framework TENSORFLOW \
  --target inferentia \
  --output compiled_model.pb
```

### Porównanie chipów ML

| Chip | Producent | Instancja | Cel | Przewaga |
|---|---|---|---|---|
| **NVIDIA V100** | NVIDIA | p3.* | Training/Inference | Ekosystem, kompatybilność |
| **NVIDIA A100** | NVIDIA | p4d.* | Large model training | Szybkość, NVLink |
| **NVIDIA H100** | NVIDIA | p5.* | LLM training | Najnowszy, Transformer Engine |
| **NVIDIA T4** | NVIDIA | g4dn.* | Inference | Tani, wydajny inference |
| **NVIDIA A10G** | NVIDIA | g5.* | Inference, gen AI | Lepszy niż T4 |
| **AWS Trainium** | AWS | trn1.* | Training DL | Koszt-optymalizacja |
| **AWS Inferentia** | AWS | inf1/inf2 | Inference | Najniższy koszt inference |
| **Graviton3** | AWS | c7g.* | CPU inference | ARM, tani CPU ML |

---

### 30.17 SageMaker vs Azure ML vs Google Vertex AI

| Cecha | SageMaker | Azure ML | Vertex AI |
|---|---|---|---|
| **IDE** | Studio (Jupyter + Code Editor) | Azure ML Studio | Vertex Workbench |
| **Pipeline** | SageMaker Pipelines | Azure ML Pipelines | Vertex Pipelines (Kubeflow) |
| **AutoML** | Autopilot | Azure AutoML | Vertex AutoML |
| **Feature Store** | Feature Store | Tak (preview) | Vertex Feature Store |
| **Model Registry** | Model Registry | Model Registry | Model Registry |
| **Monitoring** | Model Monitor | Data Drift Monitor | Model Monitoring |
| **LLM/GenAI** | Bedrock + JumpStart | Azure OpenAI Service | Model Garden + Gemini |
| **Custom Chips** | Trainium, Inferentia | Nie | Tensor Processing Units (TPU) |
| **Labeling** | Ground Truth | Azure ML Labeling | Vertex Data Labeling |
| **Zarządzanie** | Najbardziej rozbudowane | Dobra integracja z Azure | Prosty, zintegrowany GCP |

---

### 30.18 Bezpieczeństwo SageMaker i AI

### Sieć i izolacja

```bash
# SageMaker VPC Config — training job bez dostępu do internetu
aws sagemaker create-training-job \
  --training-job-name secure-job \
  --vpc-config '{
    "SecurityGroupIds": ["sg-xxxxxxxx"],
    "Subnets": ["subnet-xxxxxxxx", "subnet-yyyyyyyy"]
  }' \
  --enable-network-isolation true \
  --enable-inter-container-traffic-encryption true \
  ...
```

### IAM dla SageMaker

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sagemaker:CreateTrainingJob",
        "sagemaker:DescribeTrainingJob",
        "sagemaker:CreateModel",
        "sagemaker:CreateEndpoint*"
      ],
      "Resource": "arn:aws:sagemaker:*:*:*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-ml-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:GetAuthorizationToken"
      ],
      "Resource": "*"
    }
  ]
}
```

### Szyfrowanie

| Komponent | Szyfrowanie |
|---|---|
| S3 (training data, model artifacts) | SSE-S3, SSE-KMS, SSE-C |
| EBS (training volume) | KMS customer managed key |
| EFS (Studio home dir) | KMS |
| Inter-container traffic | TLS (enable flag) |
| Endpoint invocations | TLS (HTTPS) |
| Feature Store offline | S3 + KMS |
| Feature Store online | Managed encryption |

### Compliance i audyt

- **CloudTrail** — wszystkie API calls SageMaker
- **Model Cards** — dokumentacja modelu (data, bias, intended use)
- **Model Monitor** — ciągłe monitorowanie dryfu i bias
- **SageMaker Clarify** — bias + explainability raporty
- **Bedrock Model Invocation Logging** — audit GenAI

---

### 30.19 Well-Architected Framework — AI/ML

| Filar | Zasady dla AI/ML |
|---|---|
| **Operational Excellence** | SageMaker Pipelines (MLOps CI/CD), Model Registry z wersjonowaniem, automatyczne retrenowanie przez EventBridge, Experiments tracking |
| **Security** | VPC isolation dla Training/Endpoints, Network Isolation mode, KMS dla danych i modeli, IAM least privilege, Bedrock Guardrails, PII detection |
| **Reliability** | Multi-AZ endpoints, Auto Scaling dla inference, Batch Transform dla odporności na błędy, model backupu przez S3 versioning |
| **Performance Efficiency** | Wybór właściwych instancji ML (Trainium/Inferentia dla kosztu, p5 dla wydajności), Graviton dla CPU workloads, distributed training, model quantization |
| **Cost Optimization** | Spot instances dla training, Serverless Inference dla niskiego ruchu, Inferentia zamiast GPU, Savings Plans dla SageMaker |
| **Sustainability** | Graviton (40% mniej energii), Trainium (wydajność/W), model pruning/quantization (mniejszy model = mniej energii), Spot instances (współdzielona infrastruktura) |

---

### 30.20 4 Domeny Egzaminacyjne SAP-C02

### Domena 1: Design Solutions for Organizational Complexity

- **Multi-account ML**: SageMaker Studio może działać cross-account przez VPC sharing + Resource Access Manager. Model Registry może być shared między kontami (cross-account model package groups).
- **Data Governance**: AWS Lake Formation + SageMaker Feature Store dla centralnego zarządzania danymi ML. Lake Formation row/column level security.
- **MLOps at scale**: SageMaker Pipelines + CodePipeline + CodeBuild dla CI/CD modeli. EventBridge triggeruje pipeline po nowych danych w S3.
- **Compliance**: Model Cards dokumentują modele dla audytu. CloudTrail + S3 Access Logs dla forensics.

### Domena 2: Design for New Solutions

- **Wybór właściwego podejścia**:
  - Gotowe AI: Rekognition, Comprehend, Translate → szybkie wdrożenie, zero ML expertise
  - AutoML: SageMaker Autopilot, Canvas → dla zespołów bez DS
  - Foundation Models: Bedrock → GenAI bez infrastruktury
  - Custom ML: SageMaker Training + Endpoints → pełna kontrola
- **RAG Architecture**: Bedrock Knowledge Bases + OpenSearch Serverless + S3
- **Real-time vs Batch**: Real-Time Endpoints dla API inference, Batch Transform dla nocnych raportów, Async Inference dla video/audio
- **Edge AI**: SageMaker Neo kompiluje modele, Edge Manager wdraża na IoT, Panorama dla kamer

### Domena 3: Continuous Improvement for Existing Solutions

- **Model Monitoring**: SageMaker Model Monitor → CloudWatch alarms → EventBridge → Lambda retrain trigger
- **Drift Detection**: Data quality + model quality + bias monitoring
- **A/B Testing**: Multi-variant endpoints z traffic splitting
- **Inference Recommender**: automatyczna optymalizacja instancji dla inference
- **Cost Optimization**: Analyze cost/performance tradeoff instancji (Inferentia vs GPU)

### Domena 4: Accelerate Workload Migration and Modernization

- **Migracja ML do AWS**: DLAMI dla szybkiego startu, DLC dla kontenerów, SageMaker Training dla managed
- **Legacy ML modernization**: SageMaker Pipelines zastępuje cron-based retrain, Feature Store centralizuje dane
- **On-premise to Bedrock**: API-first podejście, łatwa integracja przez SDK
- **ML na krawędzi**: SageMaker Neo + Greengrass dla IoT → edge inference bez latencji cloud
---
