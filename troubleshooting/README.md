# 🏢 Enterprise Network Design, Configuration & Troubleshooting

A comprehensive enterprise network design, implementation, and NOC troubleshooting lab built using **Cisco Packet Tracer**. This project demonstrates network engineering skills including VLAN segmentation, inter-VLAN routing, DHCP, NAT/PAT, security ACLs, and documented incident response.

---

## 📋 Project Overview

This project simulates a **medium-scale enterprise network** with:
- **Multi-VLAN segmentation** for department isolation
- **Inter-VLAN routing** using router-on-a-stick
- **DHCP services** for dynamic IP allocation
- **NAT/PAT** for internet connectivity
- **Extended ACLs** for network security
- **SSH** for secure remote management
- **NOC-style troubleshooting** with documented incident response

---

## 🏗️ Network Architecture
INTERNET SERVER
192.168.50.10
|
ISP-R1
|
EDGE-R1 (NAT/PAT)
|
CORE-R1 (Inter-VLAN Routing)
|
SW1
/
PCs SW2
/ |
Finance Guest


### VLAN Design

| VLAN ID | VLAN Name | Network | Gateway | Purpose |
|---------|-----------|---------|---------|---------|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | Network device management |
| 20 | Finance | 192.168.20.0/24 | 192.168.20.1 | Finance department users |
| 30 | Guest | 192.168.30.0/24 | 192.168.30.1 | Guest network (restricted) |
| 99 | Native | - | - | Trunk native VLAN |

---

## 🛠️ Technologies Used

| Technology | Description |
|------------|-------------|
| **VLANs** | Network segmentation for security and performance |
| **802.1Q Trunking** | VLAN tagging between switches |
| **Inter-VLAN Routing** | Router-on-a-stick configuration |
| **DHCP** | Dynamic IP address assignment |
| **Static Routing** | Route configuration for network connectivity |
| **NAT/PAT** | Network Address Translation for internet access |
| **Extended ACLs** | Traffic filtering and security policies |
| **SSH** | Secure remote device management |
| **Port Security** | MAC address filtering on access ports |
| **NOC Troubleshooting** | Documented incident response methodology |

---

## 📊 IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| ISP-R1 | Serial0/0/0 | 203.0.113.1 | 255.255.255.252 |
| ISP-R1 | Gig0/0 | 192.168.50.1 | 255.255.255.0 |
| EDGE-R1 | Serial0/0/0 | 203.0.113.2 | 255.255.255.252 |
| EDGE-R1 | Gig0/0 | 192.168.1.1 | 255.255.255.0 |
| EDGE-R1 | Gig0/1 | 192.168.2.1 | 255.255.255.0 |
| CORE-R1 | Gig0/0.10 | 192.168.10.1 | 255.255.255.0 |
| CORE-R1 | Gig0/0.20 | 192.168.20.1 | 255.255.255.0 |
| CORE-R1 | Gig0/0.30 | 192.168.30.1 | 255.255.255.0 |
| CORE-R1 | Gig0/1 | 192.168.1.2 | 255.255.255.0 |
| SW1 | VLAN 10 | 192.168.10.10 | 255.255.255.0 |
| SW2 | VLAN 10 | 192.168.10.20 | 255.255.255.0 |

---

## 📸 Network Screenshots

### Topology & Configuration Verification

| # | Screenshot | Description |
|---|------------|-------------|
| 01 | [final-topology.png](screenshots/01-final-topology.png) | Complete enterprise network topology |
| 02 | [vlan-configuration.png](screenshots/02-vlan-configuration.png) | VLAN configuration verification |
| 03 | [trunk-verification.png](screenshots/03-trunk-verification.png) | Trunk port verification on switches |
| 04 | [router-subinterfaces.png](screenshots/04-router-subinterfaces.png) | Router subinterface configuration |
| 05 | [routing-table.png](screenshots/05-routing-table.png) | Routing table verification |
| 06 | [dhcp-verification.png](screenshots/06-dhcp-verification.png) | DHCP binding verification |
| 07 | [ssh-management.png](screenshots/07-ssh-management.png) | SSH remote management access |
| 08 | [nat-pat.png](screenshots/08-nat-pat.png) | NAT/PAT translation verification |
| 09 | [acl-security.png](screenshots/09-acl-security.png) | ACL security configuration |

---

## 📝 NOC Troubleshooting Incidents

This project includes **5 documented troubleshooting incidents** demonstrating NOC (Network Operations Center) methodology:

| Incident | Issue | Status |
|----------|-------|--------|
| [Incident-01](troubleshooting/Incident-01-DHCP-Failure.md) | DHCP Failure - PC cannot obtain IP address | ✅ Resolved |
| [Incident-02](troubleshooting/Incident-02-VLAN-Gateway-Failure.md) | VLAN Gateway Failure - Inter-VLAN routing issue | ✅ Resolved |
| [Incident-03](troubleshooting/Incident-03-Trunk-VLAN-Failure.md) | Trunk VLAN Failure - VLANs not traversing trunk | ✅ Resolved |
| [Incident-04](troubleshooting/Incident-04-Internet-Connectivity.md) | Internet Connectivity - NAT/PAT misconfiguration | ✅ Resolved |
| [Incident-05](troubleshooting/Incident-05-Guest-Finance-ACL.md) | Guest Finance ACL - Access control policy issue | ✅ Resolved |

### Troubleshooting Methodology Used:
1. **Incident Discovery** - Issue identification
2. **Symptom Analysis** - Problem observation
3. **Investigation** - Root cause analysis
4. **Resolution** - Fix implementation
5. **Validation** - Verification and testing

---

## 🎯 Key Configuration Highlights

### VLAN Configuration
SW1(config)# vlan 10
SW1(config-vlan)# name Management
SW1(config-vlan)# vlan 20
SW1(config-vlan)# name Finance
SW1(config-vlan)# vlan 30
SW1(config-vlan)# name Guest


### Trunk Configuration
SW1(config)# interface gig0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,30


### Router Subinterface Configuration
CORE-R1(config)# interface gig0/0.10
CORE-R1(config-subif)# encapsulation dot1Q 10
CORE-R1(config-subif)# ip address 192.168.10.1 255.255.255.0


### DHCP Configuration
CORE-R1(config)# ip dhcp pool MANAGEMENT
CORE-R1(dhcp-config)# network 192.168.10.0 255.255.255.0
CORE-R1(dhcp-config)# default-router 192.168.10.1
CORE-R1(dhcp-config)# dns-server 8.8.8.8

text

### NAT/PAT Configuration
EDGE-R1(config)# interface serial0/0/0
EDGE-R1(config-if)# ip nat outside
EDGE-R1(config)# interface gig0/0
EDGE-R1(config-if)# ip nat inside
EDGE-R1(config)# access-list 1 permit 192.168.0.0 0.0.255.255
EDGE-R1(config)# ip nat inside source list 1 interface serial0/0/0 overload

text

### Security ACL
CORE-R1(config)# access-list 110 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
CORE-R1(config)# access-list 110 permit ip any any
CORE-R1(config)# interface gig0/0.30
CORE-R1(config-subif)# ip access-group 110 in

text

---

## 📂 Project Structure
enterprise-network-cisco-packet-tracer/
│
├── 📄 README.md # Project overview (this file)
│
├── 📁 topology/
│ └── 📦 Enterprise-Network.pkt # Cisco Packet Tracer file
│
├── 📁 documentation/
│ └── 📄 Enterprise_Network_Project_Detailed_History.docx # Complete project documentation
│
├── 📁 screenshots/
│ ├── 🖼️ 01-final-topology.png
│ ├── 🖼️ 02-vlan-configuration.png
│ ├── 🖼️ 03-trunk-verification.png
│ ├── 🖼️ 04-router-subinterfaces.png
│ ├── 🖼️ 05-routing-table.png
│ ├── 🖼️ 06-dhcp-verification.png
│ ├── 🖼️ 07-ssh-management.png
│ ├── 🖼️ 08-nat-pat.png
│ └── 🖼️ 09-acl-security.png
│
└── 📁 troubleshooting/
├── 📄 Incident-01-DHCP-Failure.md
├── 📄 Incident-02-VLAN-Gateway-Failure.md
├── 📄 Incident-03-Trunk-VLAN-Failure.md
├── 📄 Incident-04-Internet-Connectivity.md
└── 📄 Incident-05-Guest-Finance-ACL.md

text

---

## 🚀 How to Use This Project

### 1️⃣ Open the Packet Tracer File
Open Cisco Packet Tracer
File → Open
Navigate to topology/Enterprise-Network.pkt

text

### 2️⃣ Verify Network Connectivity
Check router interfaces
show ip interface brief

Check routing table
show ip route

Verify DHCP bindings
show ip dhcp binding

Check NAT translations
show ip nat translations

text

### 3️⃣ Test End-to-End Connectivity
From any PC
ping 192.168.50.10 # Internet Server
ping 8.8.8.8 # External DNS

text

---

## 📊 Project Results

### ✅ Achievements

| Metric | Status |
|--------|--------|
| Network Segmentation | ✅ 4 VLANs configured |
| Inter-VLAN Routing | ✅ Working |
| DHCP Services | ✅ Operational |
| Internet Connectivity | ✅ NAT/PAT working |
| Security ACLs | ✅ Guest traffic blocked |
| SSH Management | ✅ Secure access enabled |
| Troubleshooting Documentation | ✅ 5 incidents documented |

### 🎓 Skills Demonstrated

- **Network Design** - Multi-VLAN enterprise architecture
- **Device Configuration** - Switches, routers, DHCP, NAT
- **Security Implementation** - ACLs, SSH, port security
- **Troubleshooting** - Structured NOC methodology
- **Documentation** - Technical writing and incident reporting
- **Network Verification** - Testing and validation

---

## 🙏 Acknowledgments

- Cisco Networking Academy
- NOC Troubleshooting Best Practices
- Enterprise Network Design Guidelines

---
