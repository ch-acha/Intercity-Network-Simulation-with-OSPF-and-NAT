# 🏠 Meli's Company – HomeCity Network Configuration

This document outlines the network topology and configuration for **Meli’s Company**, located in **HomeCity**, connected to the intercity backbone through the same Layer 2 infrastructure as Berinda’s Company.

---

## 🛠️ Network Topology

Meli's router connects through a **Layer 2 switch** that connects to the HomeCity core via a **Layer 3 switch**.

### 📡 IP Addressing and Connectivity

| Connection                           | IP/Subnet           |
|-------------------------------------|---------------------|
| Meli Router ↔ Layer 3 Switch        | 192.168.9.0/30      |
| Layer 3 Switch ↔ Layer 2 Switch     | Trunk (VLAN 10/20)  |
| NAT Public IP (Meli Network)        | 100.64.2.4          |

---

## 🌐 VLAN Configuration

Two VLANs were configured for Meli’s internal network, with devices segmented for security and organization.

| VLAN ID | Name     | Purpose                  |
|---------|----------|--------------------------|
| 10      | Servers  | Devices in ports Fa0/2–12 |
| 20      | Admin    | Devices in ports Fa0/13–24 |

### Layer 2 Switch Configuration

- Switchports Fa0/2–12 set to **access VLAN 10**
- Switchports Fa0/13–24 set to **access VLAN 20**
- Connected to Layer 3 switch via **trunk**
- **VTP** is enabled for VLAN propagation

### Layer 3 Switch Configuration

- **SVI (Switched Virtual Interfaces)** created for VLAN 10 and VLAN 20
- **Routing enabled** (`ip routing`) to allow **inter-VLAN communication**
- VLAN trunk link connected to Layer 2 switch

### Static Route to Router

```bash
ip route 192.168.10.0 255.255.255.0 192.168.9.1
```

### 🧭 Meli Router Configuration
The router handles traffic from internal VLANs and connects to the HomeCity backbone via the Layer 3 switch.

### 🔹 Static Routing
A static route ensures communication between the internal network and the Layer 3 switch:

```bash
ip route 192.168.10.0 255.255.255.0 192.168.9.2
192.168.9.2 is the interface of the Layer 3 switch connected to the router.
```
### 🔹 NAT Configuration
NAT is used to translate private IPs to a single public IP for outbound connectivity.

```bash
ip nat inside source list 1 interface <interface-to-homecity> overload
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
```
<interface-to-homecity> refers to the router interface connected to the Layer 3 switch with IP 100.64.2.4.

### 🔹 Default Route
A default route forwards all unknown traffic toward the HomeCity network:

```bash
ip route 0.0.0.0 0.0.0.0 100.64.2.2
```
This allows internet-bound or non-local traffic to exit through HomeCity's main routing infrastructure.

### ✅ Summary
Meli's Company is now fully connected to the HomeCity core network with:

Separate VLANs for internal segmentation

Inter-VLAN routing via Layer 3 SVI

NAT configuration for external communication

Static routing for full Layer 2/3 connectivity

Trunking and VTP for VLAN consistency

### 🖥️ Technologies Used
VLANs, VTP, DTP

Layer 2 and Layer 3 switching

Static routing and NAT

Cisco Packet Tracer

