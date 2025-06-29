# 🖧 Packet Tracer Department Network Project
## 📘 Project Overview

This project demonstrates the design of a basic departmental network in **Cisco Packet Tracer** using the network `192.168.40.0/24`. It connects two departments — **Accounts** and **Delivery** — using routers, switches, and subnetting. The goal is to enable communication between the departments while applying proper subnetting and IP addressing.

---

## 📋 Part 1: Scenario / Requirements

- Two departments: **Accounts** and **Delivery**
- Each department has **at least 2 PCs**
- Devices should be connected using **appropriate routers and switches**
- All devices should be assigned **valid IPs, subnet masks, and gateways**
- Use network **192.168.40.0**
- **Devices in Delivery** should be able to **ping devices in Accounts**

---

## 🧮 Part 2: Subnet Calculation

### Given:
- Network Address: `192.168.40.0`
- Default Subnet Mask: `255.255.255.0` (/24)
- Required Subnets: 2 (one for each department)

---

### Step-by-step Subnetting

#### Step 1: Determine number of bits to borrow
We need at least 2 subnets  
→ 2¹ = 2 ⇒ **Borrow 1 bit**

#### Step 2: New Subnet Mask
- Borrowing 1 bit from host portion gives us: `/25`
- Subnet Mask = `255.255.255.128`

#### Step 3: Number of Hosts per Subnet
- Host bits: `32 - 25 = 7`
- Usable hosts = `2^7 - 2 = 126` per subnet

#### Step 4: Subnet Breakdown

| Subnet | Network Address     | Usable IP Range                  | Broadcast Address   |
|--------|---------------------|----------------------------------|---------------------|
| 1      | `192.168.40.0/25`   | `192.168.40.1 – 192.168.40.126`  | `192.168.40.127`    |
| 2      | `192.168.40.128/25` | `192.168.40.129 – 192.168.40.254`| `192.168.40.255`    |

---

## 🗂️ Part 3: IP Address Allocation

| Device       | Department | IP Address       | Subnet              | Gateway           |
|--------------|------------|------------------|----------------------|-------------------|
| PC1          | Accounts   | 192.168.40.10     | 192.168.40.0/25     | 192.168.40.1      |
| PC2          | Accounts   | 192.168.40.11     | 192.168.40.0/25     | 192.168.40.1      |
| Router G0/0  | Router     | 192.168.40.1      | Accounts Subnet     | -                 |
| PC3          | Delivery   | 192.168.40.130    | 192.168.40.128/25   | 192.168.40.129    |
| PC4          | Delivery   | 192.168.40.131    | 192.168.40.128/25   | 192.168.40.129    |
| Router G0/1  | Router     | 192.168.40.129    | Delivery Subnet     | -                 |

---
## 🖥️ Part 4: Network Topology
     [Router]
     /      \
 [Switch1]   [Switch2]
 /      \    /      \
PC1    PC2  PC3    PC4

- Each switch connects to 2 PCs and 1 router interface

---

## 🛠️ Part 4.1: Assigning IP Addresses to Devices

After subnetting and planning the IP addresses, assign the IPs to each device as follows.

---

### 🖥️ Assign IP Address to PCs (PC1, PC2, PC3, PC4)

#### Steps (repeat for each PC):
1. Click on the PC (e.g., PC1)
2. Go to the **Desktop** tab
3. Click **IP Configuration**
4. Enter the following values:

| PC     | IP Address       | Subnet Mask         | Default Gateway   |
|--------|------------------|---------------------|-------------------|
| PC1    | 192.168.40.10    | 255.255.255.128     | 192.168.40.1      |
| PC2    | 192.168.40.11    | 255.255.255.128     | 192.168.40.1      |
| PC3    | 192.168.40.130   | 255.255.255.128     | 192.168.40.129    |
| PC4    | 192.168.40.131   | 255.255.255.128     | 192.168.40.129    |

📌 *Ensure you don't leave any field blank.*

---

### 🚦 Assign IP Address to Router Interfaces

#### Steps:
1. Click the **Router**
2. Go to the **CLI tab**
3. Enter configuration mode and apply these commands:

```bash
enable
configure terminal

interface gig0/0
 ip address 192.168.40.1 255.255.255.128
 no shutdown

interface gig0/1
 ip address 192.168.40.129 255.255.255.128
 no shutdown

exit
write memory

```
## ⚙️ Part 5: Router Configuration Commands

Use the following commands on the router:

```bash
enable
configure terminal

interface gig0/0
 ip address 192.168.40.1 255.255.255.128
 no shutdown

interface gig0/1
 ip address 192.168.40.129 255.255.255.128
 no shutdown

exit
write memory
```


## 🔌 Part 6: Cabling

> 🔧 **Note:** In real-world or Packet Tracer implementation, cabling is usually done **immediately after the topology is designed** (before IP addressing or configuration).  
> However, for clarity in this documentation, it's listed here **after the IP and router setup** so that readers understand what’s being connected and why.

| From              | To              
|-------------------|-------------------------|
| PC ↔ Switch       | Copper Straight-through |
| Switch ↔ Router   | Copper Straight-through |

📌 You can also use **automatic cabling** in Packet Tracer (lightning bolt icon) to let the software choose the correct cable type automatically.


## 🧪 Part 7: Testing & Results (Step-by-Step)

Once all devices are configured and connected, test if PCs in the Delivery department can communicate with PCs in the Accounts department.

---

### ✅ Step-by-Step: Test from PC3 (Delivery) to PC1 (Accounts)

1. Click on **PC3**
2. Navigate to the **Desktop** tab
3. Open the **Command Prompt**
4. Type the following command:

   ```bash
   ping 192.168.40.10
Press Enter

If the network is configured correctly, you will see:

Pinging 192.168.40.10 with 32 bytes of data:
Reply from 192.168.40.10: bytes=32 time<1ms TTL=128
Reply from 192.168.40.10: bytes=32 time<1ms TTL=128

Repeat from PC4 to PC2
Click on PC4

Go to Desktop → Command Prompt

Type the command:
ping 192.168.40.11
Confirm that replies are received from PC2 (Accounts)


If successful, it confirms that:

Subnets are correctly configured

Router is routing between them

PCs are using correct gateways

## ✅ Conclusion

This project provided hands-on experience in designing and configuring a simple departmental network using Cisco Packet Tracer. By applying subnetting, IP addressing, device configuration, and connectivity testing, I was able to simulate real-world networking scenarios involving inter-department communication.

Key takeaways:
- Subnetting allows logical separation within a network while efficiently utilizing IP space.
- Proper IP planning and gateway configuration are critical for successful routing.
- Testing connectivity through ping validates both logical and physical setup.
- Cisco Packet Tracer is an excellent tool for visualizing and practicing network designs before real-world deployment.

This foundational setup prepares the ground for more complex topologies, dynamic routing protocols, VLANs, and WAN simulations in future projects.



