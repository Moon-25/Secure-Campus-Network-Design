\# Secure, High-Availability Campus Network Design \& Implementation



\## 📌 Project Overview

This repository contains the complete architecture, implementation configurations, and validation data for an enterprise-grade, high-availability campus network. The project transitions a legacy "flat" network architecture into a resilient, logically segmented, and hardened infrastructure designed to eliminate single points of failure and mitigate internal security vulnerabilities.



Designed and simulated within the \*\*Cisco Packet Tracer\*\* environment using authentic Cisco IOS CLI configurations.



\---



\## 🛠️ Core Architectural Features

\* \*\*High Availability \& Fault Tolerance:\*\* Deployed First Hop Redundancy Protocol (FHRP) via HSRP across redundant Multilayer Core Switches to enable transparent, automated default gateway failover.

\* \*\*Logical Network Segmentation:\*\* Engineered Inter-VLAN routing and 802.1Q trunking links to isolate organizational traffic domains, reducing broadcast storms and improving data privacy.

\* \*\*Automated IP Management Resilience:\*\* Implemented an Active/Active Split-Scope DHCP design across separate physical core nodes to guarantee persistent address resolution and state preservation.

\* \*\*Layer 2 Edge Hardening:\*\* Applied Port Security with dynamic Sticky MAC address learning configurations to prevent rogue device attachments and physical cable-swapping attacks.

\* \*\*Management Plane Encryption:\*\* Enforced SSHv2 using 1024-bit RSA keys while structurally disabling clear-text Telnet vectors across all infrastructure interfaces.



\---



\## 📐 Network Topology \& Design

The architecture leverages a hierarchical \*\*Collapsed Core\*\* topology, merging core and distribution layer services into a redundant dual-switch backend supporting an access-layer switching edge.

\[ Core-1 (Primary Gateway) ] <=========> \[ Core-2 (Standby Gateway) ]

&#x20;            (HSRP Priority: 150)       EtherChannel  (HSRP Priority: 100)

&#x20;                      \\                                    /

&#x20;                       \\                                  /

&#x20;                  Trunk \\                                / Trunk

&#x20;                         \\                              /

&#x20;                          \[ Access-1 (Edge Switch) ]

&#x20;                          /            |           \\

&#x20;                   Fa0/1 /        Fa0/2|            \\ Fa0/3

&#x20;                        /              |             \\

&#x20;                \[ Student PC ]    \[ Staff PC ]   \[ Admin Laptop ]

&#x20;                  (VLAN 10)         (VLAN 20)       (VLAN 99)



\### 📊 Logical Addressing Schema (VLSM)

The design subnets the private Class C space `192.168.0.0/16` into distinct `/24` subnets optimized for departmental isolation\[cite: 2]:



| VLAN ID | Subnet Name | Network Address | Subnet Mask | Default Gateway (HSRP VIP) |

| :---: | :--- | :--- | :--- | :--- |

| \*\*10\*\* | Student | `192.168.10.0` | `255.255.255.0` | `192.168.10.1`\[cite: 2] |

| \*\*20\*\* | Staff | `192.168.20.0` | `255.255.255.0` | `192.168.20.1`\[cite: 2] |

| \*\*99\*\* | Management | `192.168.99.0` | `255.255.255.0` | `192.168.99.1`\[cite: 2] |



\---



\## 🚀 Implementation Strategy \& Snippets



\### 1. Layer 3 Redundancy \& Preemption (Core-1)

Configured Switched Virtual Interfaces (SVIs) on the primary core switch to claim the active default gateway operations via precise priority tracking and preemption metrics\[cite: 2]:

```cisco

interface Vlan10

&#x20;description GATEWAY\_STUDENT

&#x20;ip address 192.168.10.2 255.255.255.0

&#x20;standby 1 ip 192.168.10.1

&#x20;standby 1 priority 150

&#x20;standby 1 preempt

2. Active/Active Split-Scope DHCP Architecture

To prevent overlapping scopes while ensuring service high availability, Core-1 leases the bottom-half address profile, while Core-2 covers the top-half sequence\[cite: 2]:



Cisco CLI

! Core-1 Address Pooling and Exclusion Blueprint

ip dhcp excluded-address 192.168.10.1 192.168.10.10

ip dhcp excluded-address 192.168.10.151 192.168.10.254

!

ip dhcp pool STUDENT\_POOL

&#x20;network 192.168.10.0 255.255.255.0

&#x20;default-router 192.168.10.1

&#x20;dns-server 8.8.8.8

3\. Edge Defense Hardening (Access-1)

Ports assigned to end-user segments lock down dynamically to the first signature discovered, establishing an immediate mitigation wall\[cite: 2]:



Cisco CLI

interface FastEthernet0/1

&#x20;description CONNECTED\_TO\_PC\_A\_STUDENT

&#x20;switchport access vlan 10

&#x20;switchport mode access

&#x20;switchport port-security

&#x20;switchport port-security mac-address sticky

🔍 Validation \& Empirical Testing Data

HSRP Verification (show standby brief)

Under normal operational profiles, Core-1 maintains custody of the Active state, holding the Virtual IP definitions natively\[cite: 2]:



Plaintext

Interface   Grp  Pri P State    Active          Standby         Virtual IP

Vl10        1    150 P Active   local           192.168.10.3    192.168.10.1

Vl20        20   150 P Active   local           192.168.20.3    192.168.20.1

Vl99        99   150 P Active   local           192.168.99.3    192.168.99.1

Upon executing an administrative shutdown simulation on Core-1, log monitors verified that Core-2 immediately transitions into active status in sub-second timelines, preserving user sessions seamlessly\[cite: 2]:



Plaintext

Core-2# show standby brief

Interface   Grp  Pri P State    Active          Standby         Virtual IP

Vl10        1    100   Active   local           unknown         192.168.10.1

Security Incident Log (Port Security Breach)

When a rogue asset (attacker laptop) was hot-swapped onto the edge switch interface Fa0/1, the interface instantly transitioned to an err-disabled state, preserving structural integrity\[cite: 2]:



Plaintext

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down



Access-1# show port-security interface fa0/1

Port Security              : Enabled

Port Status                : Secure-shutdown

Violation Mode             : Shutdown

Security Violation Count   : 1

