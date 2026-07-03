# 🛡️ Secure, High-Availability Campus Network Design & Implementation

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![HSRP](https://img.shields.io/badge/HSRP-High%20Availability-success)
![DHCP](https://img.shields.io/badge/DHCP-Split--Scope-orange)
![SSH](https://img.shields.io/badge/SSH-Secure%20Management-green)
![ACL](https://img.shields.io/badge/ACL-Management%20Access-red)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📖 Project Overview

This project demonstrates the design and implementation of a **secure, highly available campus network** using Cisco technologies in **Cisco Packet Tracer**.

The objective is to transform a traditional flat network into a modern enterprise network by introducing:

- High Availability using HSRP
- VLAN segmentation
- Inter-VLAN Routing
- Split-Scope DHCP
- Secure SSH remote management
- Port Security
- Management Access Control using ACL

The network is designed to continue operating even if one Core Switch fails while protecting management access from unauthorized users.

---

# 🏗️ Network Architecture

The network follows a **Collapsed Core Architecture** consisting of two Layer 3 Core Switches and one Access Switch.

```
             [ Core-1 (Primary Gateway) ] <=========> [ Core-2 (Standby Gateway) ]
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

```

---

# 🌐 VLAN Design

| VLAN | Purpose | Network |
|------|---------|----------------|
| VLAN 10 | Student | 192.168.10.0/24 |
| VLAN 20 | Staff | 192.168.20.0/24 |
| VLAN 99 | Management | 192.168.99.0/24 |

Each VLAN has its own subnet and communicates through Inter-VLAN Routing on the Core Switches.

---

# ⭐ Key Features

## ✅ High Availability (HSRP)

- Core-1 operates as the Active Gateway.
- Core-2 remains in Standby mode.
- Virtual Gateway is used by all clients.
- Automatic failover occurs when the Active Core fails.
- Preemption automatically restores Core-1 after recovery.

---

## ✅ Split-Scope DHCP

DHCP service is distributed across both Core Switches.

| Device | DHCP Range |
|---------|----------------|
| Core-1 | 192.168.x.11 – 192.168.x.150 |
| Core-2 | 192.168.x.151 – 192.168.x.254 |

This prevents overlapping IP addresses while maintaining DHCP service during failures.

---

## ✅ Inter-VLAN Routing

Layer 3 SVIs on both Core Switches provide routing between:

- Student VLAN
- Staff VLAN
- Management VLAN

---

## ✅ SSH Remote Management

Network devices are managed remotely using SSH instead of Telnet.

Implemented:

- SSH Version 2
- RSA Encryption
- Local Username Authentication

---

## 🆕 Management Access Control (ACL)

To improve security, SSH access is restricted to the Management VLAN.

| Source VLAN | SSH Access |
|------------|------------|
| Student (VLAN10) | ❌ Denied |
| Staff (VLAN20) | ❌ Denied |
| Management (VLAN99) | ✅ Allowed |

This prevents unauthorized users from remotely managing network devices.

---

## ✅ Port Security

Access Switch interfaces use Port Security with Sticky MAC Address learning.

Features:

- Sticky MAC
- Shutdown violation mode
- Rogue device protection

---

# 🧪 Demonstration

The project demonstrates the following scenarios:

✅ VLAN Segmentation

✅ Inter-VLAN Routing

✅ HSRP Failover

✅ DHCP Redundancy

✅ SSH Remote Management

✅ Management ACL

✅ Port Security

---

# 📸 Screenshots

Add screenshots inside the **images/** folder.

Example:

```
images/
│
├── topology.png
├── vlan.png
├── hsrp-active.png
├── hsrp-failover.png
├── dhcp-client.png
├── ssh-success.png
├── ssh-denied.png
└── port-security.png
```

Example:

```markdown
## Network Topology

![Topology](images/topology.png)
```

---

# 📂 Repository Structure

```
Secure-Campus-Network-Design
│
├── Campus-Topology.pkt
├── README.md
│
├── Configurations/
│   ├── Core-1.txt
│   ├── Core-2.txt
│   └── Access-1.txt
│
├── docs/
│   ├── High-Availability Campus Network Design.pdf
│   └── Project-Slides.pdf
│
└── images/
    ├── topology.png
    ├── hsrp-active.png
    ├── hsrp-failover.png
    ├── dhcp-client.png
    ├── ssh-success.png
    ├── ssh-denied.png
    └── port-security.png
```

---

# 🛠️ Technologies Used

- Cisco Packet Tracer
- Cisco IOS CLI
- HSRP
- VLAN
- Inter-VLAN Routing
- DHCP
- Split-Scope DHCP
- SSH
- Standard ACL
- Port Security

---

# 🚀 Future Improvements

Possible enhancements include:

- EtherChannel
- OSPF
- Dynamic Routing
- Syslog Server
- NTP
- SNMP Monitoring
- AAA Authentication with RADIUS
- Network Monitoring Dashboard

---

# 👨‍💻 Author

**Seng Heng**

Bachelor of Information Technology Engineering

Royal University of Phnom Penh (RUPP)

GitHub:
https://github.com/Moon-25

---

## ⭐ Acknowledgements

This project was developed for academic purposes to demonstrate enterprise network design principles, high availability, and secure network management using Cisco technologies.