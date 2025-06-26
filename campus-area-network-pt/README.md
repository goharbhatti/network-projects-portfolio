# Campus Area Network Design & Implementation

**Version:** 1.0
**Author:** Gohar Nawaz
**Tool Used:** Cisco Packet Tracer
**Last Updated:** June 2025

---

## 📘 Overview

This project demonstrates the complete design and implementation of a Campus Area Network (CAN) connecting a main campus and a branch site, based on a hierarchical enterprise network architecture. It includes VLAN segmentation, inter-VLAN routing, DHCP, NAT, ACLs, a DMZ with critical servers, ASA firewall configuration, EtherChannel for link redundancy, wireless integration via a Cisco WLC, guest and staff SSIDs on LAPs, and a site-to-site IPsec VPN between the main and branch firewalls.

---

## 🏫 Network Zones

### 📍 Main Area

* **Faculties:**

  * Health Sciences
  * Business
  * Engineering
  * Art and Design
* **Departments:**

  * IT Department (with 2 additional PCs for management)
* **Devices per Faculty/Department:**

  * 2 PCs
  * 1 Tablet
  * 1 Smartphone
  * 1 Printer
  * 1 Lightweight Access Point (LAP) configured with **Guest** and **Staff** SSIDs

### 🏢 Branch Area

* **Faculties:**

  * Health Sciences
  * Business
  * Engineering
  * Art and Design
* **Devices per Faculty:** Same as main area (except no IT department)

---

## 🧩 Network Design

### 🧱 VLAN Configuration and Subnetting

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

#### Main Area

* **LAN Users (VLAN 20 / 172.16.0.0/16):** \~65,534 hosts
* **WLAN Users (VLAN 50 / 10.10.0.0/16):** \~65,534 hosts

#### Branch Area

* **LAN Users (VLAN 60 / 172.17.0.0/16):** \~65,534 hosts
* **WLAN Users (VLAN 90 / 10.11.0.0/16):** \~65,534 hosts

> These capacities are theoretical and reflect addressable hosts based on subnet size, not real-world usage limits.

---

### 🏛️ Layered Network Architecture

#### Main Area

* **Access Layer:** One switch per faculty and department
* **Distribution Layer:** Two multilayer Layer 3 switches

  * Connected to each access switch for redundancy
  * Connected to each other using 3 links with **LACP EtherChannel**
* **Core Layer:** Cisco ASA Firewall

#### Wireless Integration

* **WLC IP:** 10.10.0.15/16
* **Gateway:** 10.10.0.1
* **WLC connected to L3 Switch 1** (10.20.20.33/30 ↔ Firewall 10.20.20.34/30)

#### ASA Firewall Internal Interfaces

* **Inside 1 (to L3 Switch 1):** 10.20.20.34/30
* **Inside 2 (to L3 Switch 2):** 10.20.20.38/30

#### DMZ Zone (via ASA)

* **DMZ IP Subnet:** 10.20.20.0/27
* **Firewall DMZ Interface IP:** 10.20.20.1
* **DMZ Devices:**

  * DHCP Server 1: 10.20.20.5
  * DHCP Server 2: 10.20.20.6
  * DNS Server: 10.20.20.7
  * Web Server: 10.20.20.8
  * Email Server: 10.20.20.9
  * FTP Server: 10.20.20.10

#### Internet Connectivity

* **Firewall Outside IP:** 105.100.50.2/30 ↔ ISP: 105.100.50.1
* **ISP to Internet:** ISP: 20.20.20.2 ↔ Internet Gateway: 20.20.20.1

---

### 🏢 Branch Network

#### Firewall and Layer 3 Switches

* **Branch Firewall IPs:**

  * Inside 1: 10.20.20.41 ↔ L3 Switch 1: 10.20.20.42/30
  * Inside 2: 10.20.20.45 ↔ L3 Switch 2: 10.20.20.46/30
* **L3 Switches Interconnect:** 3 links via **LACP EtherChannel**

#### Internet Connectivity for Branch

* **Firewall Outside IP:** 205.200.100.2/30 ↔ ISP: 205.200.100.1
* **ISP to Internet:** ISP: 30.30.30.2 ↔ Internet Gateway: 30.30.30.1

---

## 🔐 Site-to-Site VPN

A **site-to-site IPsec VPN** is configured between the ASA firewalls of the main and branch networks to securely tunnel traffic between both sites over the internet. This ensures encrypted communication for sensitive data across both campuses.

---

## 🧾 Interface & IP Address Table

| Device               | Interface | IP Address    | Subnet Mask     | Description            |
| -------------------- | --------- | ------------- | --------------- | ---------------------- |
| ASA Main             | Inside 1  | 10.20.20.34   | 255.255.255.252 | To L3 Switch 1         |
| ASA Main             | Inside 2  | 10.20.20.38   | 255.255.255.252 | To L3 Switch 2         |
| ASA Main             | DMZ       | 10.20.20.1    | 255.255.255.224 | DMZ interface          |
| ASA Main             | Outside   | 105.100.50.2  | 255.255.255.252 | To Main ISP            |
| L3 Switch 1 (Main)   | Uplink    | 10.20.20.33   | 255.255.255.252 | To ASA Inside 1        |
| L3 Switch 2 (Main)   | Uplink    | 10.20.20.37   | 255.255.255.252 | To ASA Inside 2        |
| WLC (Main)           | Mgmt      | 10.10.0.15    | 255.255.0.0     | WLAN controller        |
| DHCP Server 1        | NIC       | 10.20.20.5    | 255.255.255.224 | Primary DHCP server    |
| DHCP Server 2        | NIC       | 10.20.20.6    | 255.255.255.224 | Backup DHCP server     |
| DNS Server           | NIC       | 10.20.20.7    | 255.255.255.224 | DNS service            |
| Web Server           | NIC       | 10.20.20.8    | 255.255.255.224 | Web hosting            |
| Email Server         | NIC       | 10.20.20.9    | 255.255.255.224 | Email server           |
| FTP Server           | NIC       | 10.20.20.10   | 255.255.255.224 | File Transfer Protocol |
| ISP Main             | Inside    | 105.100.50.1  | 255.255.255.252 | To ASA Outside         |
| ISP Main             | Outside   | 20.20.20.2    | 255.255.255.252 | To Internet            |
| Internet             | Gateway   | 20.20.20.1    | 255.255.255.252 | Core internet          |
| ISP Branch           | Inside    | 30.30.30.1    | 255.255.255.252 | To Internet            |
| ISP Branch           | Outside   | 205.200.100.1 | 255.255.255.252 | To Branch ASA Outside  |
| ASA Branch           | Outside   | 205.200.100.2 | 255.255.255.252 | To Branch ISP          |
| ASA Branch           | Inside 1  | 10.20.20.41   | 255.255.255.252 | To Branch L3 Switch 1  |
| ASA Branch           | Inside 2  | 10.20.20.45   | 255.255.255.252 | To Branch L3 Switch 2  |
| L3 Switch 1 (Branch) | Uplink    | 10.20.20.42   | 255.255.255.252 | To Branch ASA Inside 1 |
| L3 Switch 2 (Branch) | Uplink    | 10.20.20.46   | 255.255.255.252 | To Branch ASA Inside 2 |

---

## 🧰 Technologies Used

* **Routing:** Static routing for edge networks, Inter-VLAN Routing on L3 switches
* **Switching:** VLANs, trunking, LACP EtherChannel
* **Security:**

  * Cisco ASA firewall configuration
  * DMZ segmentation
  * ACLs (not yet documented here)
  * IPsec VPN between main and branch firewalls
* **Services:** DHCP, DNS, Web, Email, FTP
* **Wireless:** Lightweight APs with Guest and Staff SSIDs, and WLC configuration

---

## 📬 Contact

**Gohar Nawaz**
📧 [goharbhatti46@gmail.com](mailto:goharbhatti46@gmail.com)
🔗 [LinkedIn](https://linkedin.com/in/gohar-nawaz-bhatti)

---

> **Version 1.0 – June 2025**
> *This document is subject to updates as the project evolves or additional features are added (e.g., ACLs, VPN, automation).*
