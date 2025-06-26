# Campus Area Network Design & Implementation (Packet Tracer)

This project demonstrates the complete design and implementation of a Campus Area Network (CAN) using **Cisco Packet Tracer**, based on the “Complete CAN Design – Part 1 & 2” series. It replicates an enterprise-grade network with hierarchical architecture, VLANs, inter-VLAN routing, DHCP, ACLs, and ASA-based firewall protection.

---

## 🧱 Project Overview

| Feature                | Details                                                |
|------------------------|--------------------------------------------------------|
| Tool Used              | Cisco Packet Tracer                                    |
| Architecture           | 3-tier (Core, Distribution, Access)                    |
| VLAN Segmentation      | Separate VLANs for Management, LAN, WLAN               |
| Inter-VLAN Routing     | Done via Layer 3 Switch                                |
| DHCP                   | Centralized DHCP setup for multiple VLANs              |
| ACLs                   | Standard & Extended ACLs for filtering                 |
| Firewall               | Cisco ASA simulation for edge protection               |
| NAT & VPN              | PAT and Site-to-Site VPN implementation (optional)     |

---

## 🧩 VLAN Configuration and IP Addressing

### 📍 Main Campus
__________________________________________________________________________________
| VLAN | Name        | Purpose                       | IP Subnet                  |
|------|-------------|-------------------------------|----------------------------|
| 10   | Management  | Network devices, admin access | 192.168.10.0/24            |
| 20   | LAN         | Wired workstation users       | 172.16.0.0/16              |
| 50   | WLAN        | Wireless users                | 10.10.0.0/16               |
-----------------------------------------------------------------------------------


### 🏢 Branch Office
___________________________________________________________________________________
| VLAN | Name        | Purpose                       | IP Subnet                   |
|------|-------------|--------------------|----------------------------------------|
| 60   | LAN         | Branch wired users          | 172.17.0.0/16                 |
| 90   | WLAN        | Branch wireless users       | 10.11.0.0/16                  |
------------------------------------------------------------------------------------
> **Note:** The branch office does not use a management VLAN.

---

## 📁 Files Included

| File                  | Description                                      |
|-----------------------|--------------------------------------------------|
| `CAN_project.pkt`     | Full Packet Tracer project                       |
| `CAN_Topology.png`    | Network topology screenshot                      |
| `config.txt`          | Optional CLI commands used (exported manually)   |

---

## 📷 Topology Overview

![Network Topology](CAN_Topology.png)

---

## 🛠️ Tools & Commands Used

- Cisco routers and switches (Packet Tracer)
- Cisco ASA 5505 (simulated)
- DHCP configuration
- VLAN and Trunking
- Inter-VLAN Routing
- Static & Dynamic NAT
- Access Control Lists (ACLs)
- Site-to-Site VPN (optional)

---

## 📚 What I Learned

- Designing large-scale campus networks
- Segmenting and securing traffic with VLANs & ACLs
- Implementing real-world services (DHCP, NAT, VPN)
- Using Cisco ASA for firewall and VPN configuration
- Applying best practices in hierarchical network design

---

## 📬 Contact

**Gohar Nawaz**  
📧 goharbhatti46@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/gohar-nawaz-bhatti/)
