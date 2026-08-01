# Multi-Site Enterprise Network Architecture & Security Implementation

##  Project Overview
This project demonstrates the design and deployment of a secure, segmented multi-site enterprise network using Cisco Packet Tracer. It simulates a multi-branch environment (Cairo & Alex branches) connected via dynamic routing, featuring centralized network services, strict traffic filtering, and Layer 2/3 security hardening.

---

##  Topology & Addressing Plan

### Cairo Branch
 **VLAN 10 (Staff):** `192.168.10.0/24`
 **VLAN 20 (CyberSec):** `192.168.20.0/24`
 **VLAN 30 (Servers):** `192.168.30.0/24` (Hosts Centralized DHCP & DNS Servers)

### Alex Branch
 **VLAN 40 (Staff):** `192.168.40.0/24`
 **VLAN 50 (CyberSec):** `192.168.50.0/24`

---

##  Key Technical Implementations

### 1. Network Segmentation & Layer 2 Security
 **VLAN Configuration & Trunking:** Logically isolated departmental traffic using 802.1Q trunk lines between switches and routers.
 **Native VLAN Hardening:** Modified default Native VLANs on trunks to prevent VLAN hopping attacks.
 **Port Security:** Configured static MAC binding (`mac-address sticky`) and violation actions on end-access switch ports to mitigate unauthorized physical device access.

### 2. Routing & Traffic Management
 **Inter-VLAN Routing:** Implemented Router-on-a-Stick (ROAS) using sub-interfaces with `encapsulation dot1Q`.
 **Inter-Site Routing:** Enabled dynamic routing via **OSPF** across Cairo and Alex routers to allow cross-branch communication.

### 3. Centralized Network Services
 **DHCP & DHCP Relay:** Configured a centralized DHCP server in Cairo with individual address pools per VLAN. Implemented `ip helper-address` on router sub-interfaces across both branches to relay DHCP requests across Layer 3 boundaries.
 **DNS Service:** Set up local DNS service with `A Records` mapping corporate domains (e.g., `www.company.com`) to target web servers.

### 4. Traffic Filtering & Control (ACLs)
 **Extended Access Control Lists (ACLs):**
   Restricted **Staff VLANs** (VLAN 10 & 40) from initiating connections to **CyberSec VLANs** (VLAN 20 & 50) and internal administrative servers.
   Explicitly permitted necessary traffic flows for centralized services (DHCP and DNS requests).
   Provided full operational access for the **CyberSec team** across all network segments.

### 5. Administrative Device Hardening
 Configured SSH for secure remote management across routers and switches.
 Enforced password encryption (`service password-encryption`) and privileged access authentication.

---

##  Verification & Testing
1. **DHCP Relay Testing:** Verified remote hosts in Alex branch successfully lease IP configurations from the Cairo DHCP server.
2. **DNS Resolution:** Confirmed end-user devices resolve domain names and access target web services via standard browsers.
3. **ACL Policy Enforcement:** Executed ICMP/TCP reachability tests to verify that Staff traffic to CyberSec segments is denied, while CyberSec full reachability remains functional.

---

##  Repository Structure
* `/topology.pkt` - Cisco Packet Tracer source file.
* `/configs/` - Exported CLI running configurations for Routers and Switches.
* `/screenshots/` - Topology diagrams and CLI verification outputs.