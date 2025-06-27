# Campus Area Network Design & Implementation

**Version:** 1.2
**Author:** Gohar Nawaz
**Tool Used:** Cisco Packet Tracer
**Last Updated:** June 2025

---

## 📘 Overview

This project delivers an enterprise-grade Campus Area Network (CAN) solution interconnecting a main and branch campus with high availability, scalability, and security. Built in Cisco Packet Tracer, it includes:

* VLAN segmentation
* OSPF routing
* HSRP redundancy
* DHCP services
* Access Control Lists
* ASA firewall security
* DMZ hosting core services
* Wireless LAN with Guest/Staff SSIDs
* Site-to-site IPsec VPN
* LACP-based EtherChannel links
* Password encryption across devices

---

## 🏫 Network Zones

### 📍 Main Area

**Faculties:** Health Sciences, Business, Engineering, Art and Design
**Department:** IT (with 2 extra management PCs)

**Devices per Faculty/Department:**

* 2 PCs
* 1 Tablet
* 1 Smartphone
* 1 Printer
* 1 Lightweight Access Point (LAP) with Guest & Staff SSIDs

### 🏢 Branch Area

**Faculties:** Health Sciences, Business, Engineering, Art and Design
**Devices per Faculty:** Same as above (no IT dept)

---

## 🧩 Network Design

### 🧱 VLAN Configuration & Subnetting

#### Main Area

| VLAN | Name       | Purpose                   | IP Subnet       |
| ---- | ---------- | ------------------------- | --------------- |
| 10   | Management | Network device management | 192.168.10.0/24 |
| 20   | LAN        | Wired LAN users           | 172.16.0.0/16   |
| 50   | WLAN       | Wireless LAN users        | 10.10.0.0/16    |

#### Branch Area

| VLAN | Name | Purpose            | IP Subnet     |
| ---- | ---- | ------------------ | ------------- |
| 60   | LAN  | Wired LAN users    | 172.17.0.0/16 |
| 90   | WLAN | Wireless LAN users | 10.11.0.0/16  |

---

### 📊 User Capacity Estimates

**Main Area:**

* LAN: \~65,534 users (VLAN 20)
* WLAN: \~65,534 users (VLAN 50)

**Branch Area:**

* LAN: \~65,534 users (VLAN 60)
* WLAN: \~65,534 users (VLAN 90)

> Capacity is theoretical, based on subnetting.

---

### 🏛️ Layered Network Architecture

#### Main Area

* **Access Layer:** One switch per department
* **Distribution Layer:** Two L3 switches with HSRP:

  * VLAN 10 → 192.168.10.3 (L3-1), .2 (L3-2), VIP: .1
  * VLAN 20 → 172.16.0.3 / .2, VIP: .1
  * VLAN 50 → 10.10.0.3 / .2, VIP: .1
* **Core Layer:** Cisco ASA
* **Redundancy:** LACP EtherChannel (3 links)

#### Wireless

* **WLC IP:** 10.10.0.15/16
* **Gateway:** 10.10.0.1 (HSRP VIP)

#### ASA (Main)

* Inside 1: 10.20.20.34/30 ↔ L3-1 (.33)
* Inside 2: 10.20.20.38/30 ↔ L3-2 (.37)
* DMZ: 10.20.20.1/27

  * DHCP1: .5, DHCP2: .6, DNS: .7, Web: .8, Email: .9, FTP: .10
* Outside: 105.100.50.2/30 ↔ ISP (.1) ↔ Internet: 20.20.20.1

#### Branch Network

* L3-1: VLAN 60 → 172.17.0.3, VLAN 90 → 10.11.0.3
* L3-2: VLAN 60 → .2, VLAN 90 → .2
* HSRP VIPs: .1 for both VLANs
* Firewall Inside 1: 10.20.20.41 ↔ L3-1 (.42)
* Firewall Inside 2: 10.20.20.45 ↔ L3-2 (.46)
* Outside: 205.200.100.2 ↔ Branch ISP (.1) ↔ Internet: 30.30.30.1
* EtherChannel between L3s: 3 links (LACP)

---

## 🔐 Security & Services

* **Routing:** OSPF between all L3 devices
* **HSRP:** Redundant gateway IPs for end devices
* **Firewall:** Cisco ASA on edge with encrypted access
* **DMZ:** Servers segmented from internal LAN
* **VPN:** IPsec tunnel between Main & Branch ASA firewalls
* **Wireless:** Dual SSIDs (Staff, Guest) per LAP via WLC
* **Access Control:** Device passwords and encryption enabled

---

## 🧾 Interface & IP Address Table

| Device               | Interface | IP Address(s)                       | Subnet Mask        | Description           |
| -------------------- | --------- | ----------------------------------- | ------------------ | --------------------- |
| ASA Main             | Inside 1  | 10.20.20.34                         | 255.255.255.252    | To L3 Switch 1        |
| ASA Main             | Inside 2  | 10.20.20.38                         | 255.255.255.252    | To L3 Switch 2        |
| ASA Main             | DMZ       | 10.20.20.1                          | 255.255.255.224    | DMZ interface         |
| ASA Main             | Outside   | 105.100.50.2                        | 255.255.255.252    | To Main ISP           |
| L3 Switch 1 (Main)   | VLANs     | 192.168.10.3, 172.16.0.3, 10.10.0.3 | Respective Subnets | HSRP active           |
| L3 Switch 2 (Main)   | VLANs     | 192.168.10.2, 172.16.0.2, 10.10.0.2 | Respective Subnets | HSRP standby          |
| WLC                  | Mgmt      | 10.10.0.15                          | 255.255.0.0        | WLAN Controller       |
| DHCP 1               | NIC       | 10.20.20.5                          | 255.255.255.224    | Primary DHCP          |
| DHCP 2               | NIC       | 10.20.20.6                          | 255.255.255.224    | Backup DHCP           |
| DNS                  | NIC       | 10.20.20.7                          | 255.255.255.224    | DNS                   |
| Web Server           | NIC       | 10.20.20.8                          | 255.255.255.224    | Web Hosting           |
| Email Server         | NIC       | 10.20.20.9                          | 255.255.255.224    | Email                 |
| FTP Server           | NIC       | 10.20.20.10                         | 255.255.255.224    | FTP                   |
| ISP Main             | Inside    | 105.100.50.1                        | 255.255.255.252    | To ASA Outside        |
| ISP Main             | Outside   | 20.20.20.2                          | 255.255.255.252    | To Internet           |
| Internet             | Gateway   | 20.20.20.1                          | 255.255.255.252    | Public IP             |
| ISP Branch           | Inside    | 30.30.30.1                          | 255.255.255.252    | To Internet           |
| ISP Branch           | Outside   | 205.200.100.1                       | 255.255.255.252    | To Branch ASA         |
| ASA Branch           | Outside   | 205.200.100.2                       | 255.255.255.252    | To Branch ISP         |
| ASA Branch           | Inside 1  | 10.20.20.41                         | 255.255.255.252    | To Branch L3 Switch 1 |
| ASA Branch           | Inside 2  | 10.20.20.45                         | 255.255.255.252    | To Branch L3 Switch 2 |
| L3 Switch 1 (Branch) | VLANs     | 172.17.0.3, 10.11.0.3               | Respective Subnets | HSRP active           |
| L3 Switch 2 (Branch) | VLANs     | 172.17.0.2, 10.11.0.2               | Respective Subnets | HSRP standby          |

---

## 📬 Contact

**Gohar Nawaz**
📧 [goharbhatti46@gmail.com](mailto:goharbhatti46@gmail.com)
🔗 [LinkedIn](https://linkedin.com/in/gohar-nawaz-bhatti)

---

> **Version 1.2 – Final Draft, June 2025**
> *This documentation may be enhanced further as new services or technologies are implemented (e.g., automation scripts).*
