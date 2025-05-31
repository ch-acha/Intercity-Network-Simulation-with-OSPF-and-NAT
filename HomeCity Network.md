
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
