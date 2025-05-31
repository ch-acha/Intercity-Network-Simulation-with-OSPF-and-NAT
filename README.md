# 🌍 Intercity Network Simulation with OSPF and NAT (Cisco Packet Tracer)

This project simulates a **multi-city enterprise network** using **Cisco Packet Tracer**, showing how companies in different cities can communicate over a shared carrier backbone using **OSPF** for routing and **NAT** for service access.

---

## 🏙️ Cities Simulated

- **HomeCity**
- **CityA**
- **CityB**
- **CityC**
- **CityD**

Each city has a dedicated router and internal LAN. The routers are connected using **fiber optic links**, simulating a real-world ISP backbone.

---

## 🛠️ Step-by-Step Setup

### 🔌 1. Router Interconnections

The routers were connected using point-to-point `/30` links:

| Connection            | Subnet         |
|------------------------|----------------|
| HomeCity ↔ CityA       | `10.1.1.0/30`   |
| HomeCity ↔ CityC       | `10.1.1.16/30`  |
| CityA ↔ CityB          | `10.1.1.4/30`   |
| CityB ↔ CityD          | `10.1.1.8/30`   |
| CityD ↔ CityC          | `10.1.1.12/30`  |

### 🔁 2. OSPF Configuration

All routers were configured with **OSPF (process ID 1)** to dynamically share routing information.

Each router had its interfaces added to OSPF using the appropriate subnet and wildcard mask.

### ✅ 3. OSPF Testing

Once OSPF was set up, connectivity was tested using `ping` from one city's router to another  
(e.g. from **HomeCity** to **CityD**). Full reachability confirmed that OSPF was working correctly.

---

### 🏢 Private Company Networks

After setting up and testing the backbone network, private company networks were added to simulate business use cases.

---

#### 🏠 Berinda's Company (HomeCity)

- **Internal LAN**: `192.168.100.0/24`  
- **Connected to HomeCity via Layer 3 Switch**  
  (transit subnet: `100.64.2.0/24`)
- **NAT configured** on Berinda's router
- Can reach HomeCity router via ping (e.g. `100.64.1.1`)

---

#### 🏢 Mati's Company (CityA)

- **Internal LAN**: `192.168.50.0/24`  
- **Connected to CityA via Layer 3 Switch**  
  (transit subnet: `100.54.2.0/24`)
- **NAT configured** on Mati’s router
- Can reach CityA router via ping (e.g. `100.54.1.1`)

---

## 📸 Screenshots / Diagrams (Coming Soon)



---

### 👤 Author

Created by Me – Networking/lab enthusiast

