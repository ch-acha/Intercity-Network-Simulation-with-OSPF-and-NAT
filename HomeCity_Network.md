
## 🏠 HomeCity Network – Berinda's Company Configuration

This document outlines the network topology, routing configuration, VLAN setup, and NAT implementation for **Berinda’s Company**, located in **HomeCity**, as part of a multi-city enterprise network simulation using Cisco Packet Tracer.

---

## 🛠️ Network Architecture

Berinda’s network is connected to the main intercity backbone via the **HomeCity router**, which links to a **Layer 3 switch** using the subnet:

- **HomeCity Router ↔ Layer 3 Switch**: `100.64.1.0/30`

From there, traffic flows to a **Layer 2 switch**, which is used for **distribution and additional device connectivity**.

- **Layer 3 Switch Interface to Layer 2 Switch**: `100.64.2.2/24`

The **Berinda router** is connected to the Layer 2 switch and acts as a gateway to Berinda’s internal network. From the Berinda router, the setup extends into two VLAN-separated segments via Layer 3 and Layer 2 switching.

---

## 🌐 VLAN Configuration

### VLAN Design

| VLAN ID | Name     | Purpose  |
|---------|----------|----------|
| 10      | `Servers`| Devices like internal servers and service systems |
| 20      | `Admin`  | Administrative staff workstations and devices    |

These VLANs are hosted behind a **Layer 3 switch**, with two Layer 2 switches connected for endpoint expansion.

### Layer 3 Switch Configuration

- **SVI (Switched Virtual Interfaces)** were created for VLAN 10 and VLAN 20 to support **inter-VLAN routing**.
- The switch is **VTP-enabled (VTP Server)** and connected to Layer 2 switches via **DTP (Dynamic Trunking Protocol)** for VLAN propagation and trunk formation.
- **Routing was enabled** (`ip routing`) and static routes were added to forward VLAN traffic to the Berinda router.

```bash
ip route 192.168.10.0 255.255.255.0 192.168.9.1
```
(Here, 192.168.9.1 is the Berinda router interface facing the Layer 3 switch)

🧭 Berinda Router Configuration
The Berinda router connects both internally (to the VLAN network) and externally (to HomeCity’s Layer 3 switch) and is configured as follows:

🔹 Static Routing
To ensure internal VLANs can route outward:

```bash
ip route 192.168.10.0 255.255.255.0 192.168.9.2
```
(Here, 192.168.9.2 is the Layer 3 switch interface connected to the Berinda router)

🔹 NAT Configuration
NAT was implemented to allow private IP addresses within Berinda's network to reach other cities through the HomeCity router.

```bash

ip nat inside source list 1 interface <interface-to-homecity> overload
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
```
Where <interface-to-homecity> corresponds to the interface on the Berinda router connected to the HomeCity Layer 3 switch (100.64.2.3).

🔹 Default Route
A static default route forwards traffic from the Berinda router to the HomeCity Layer 3 switch:

```bash
ip route 0.0.0.0 0.0.0.0 100.64.2.2
```

🌐 HomeCity Layer 3 Switch Configuration
Enabled routing with ip routing.

Forwarded traffic between:

Berinda’s router (100.64.2.3)

HomeCity router (100.64.1.2)

🛰️ OSPF Integration on HomeCity Router
To ensure Berinda’s public NAT IPs are accessible from other cities, the HomeCity router:

Advertised its connected networks

Redistributed static routes so Berinda's NATed IP (100.64.2.0/24) becomes reachable city-wide

```bash
router ospf 1
 redistribute static subnets
```
✅ Summary
Berinda’s Company is now fully connected to the intercity OSPF network with:

Internal VLAN separation and inter-VLAN routing

NAT for external communication

Static routes for internal-external traffic forwarding

OSPF redistribution for full city-wide service accessibility

## 📍 Key IP Ranges

| Device/Link                       | IP/Subnet           |
|----------------------------------|---------------------|
| HomeCity ↔ Layer 3 Switch        | 100.64.1.0/30       |
| Layer 3 Switch ↔ L2 Distribution | 100.64.2.0/24       |
| Berinda Router ↔ Layer 3 Switch  | 192.168.9.0/30      |
| VLAN 10 (Servers)                | 192.168.10.0/24     |
| VLAN 20 (Admin)                  | 192.168.20.0/24     |
| NAT Public IP (Berinda Network) | 100.64.2.3          |

🖥️ Technologies Used
Cisco Packet Tracer (8.x)

OSPF

VLANs, VTP, DTP

NAT and static routing

Layer 2 & 3 switching
