# 🧠 Enterprise VLAN Network (Manual Configuration and Automation configuration with Ansible)

## 🎯 Objective
Design and implement a small enterprise VLAN network using Cisco devices in GNS3.
Demonstrates VLAN segmentation, inter-VLAN routing, DHCP, and trunk configuration.

---

## 🧩 Network Overview
- VLAN 10 – IT (192.168.10.0/24)
- VLAN 20 – HR (192.168.20.0/24)
- VLAN 30 – Guests (192.168.30.0/24)
- Inter-VLAN routing on Core-Router (Router-on-a-Stick)
- DHCP provided by the Core-Router

---

## 🖥️ Tools
- GNS3
- Cisco CSR {csr1000v-universalk9.17.03.03-serial} (Core-Router)
- Cisco IOU {i86bi_linux_l2-adventerprisek9-ms.SSA.high_iron_20190423} L2 Switch
- 3 VPCS

---

## 📊 Topology
![Network Diagram](diagram/enterprise_vlan_diagramm.png)

---

## ⚙️ Configuration Summary

### Router (Core-Router)
```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
```
### Switch (Sw)
```bash
vlan 10
 name IT
vlan 20
 name HR
vlan 30
 name Guests

interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
 switchport access vlan 30
 switchport mode access
```

