# CCNA Lab: OSPF Part 2 (Interface Configuration & Default Route)

## Overview
This lab covers OSPF Area 0 configuration directly under interfaces, tuning the reference bandwidth for accurate path costing, passive interfaces, and propagating an external default route from an ASBR to reach the ISP.

---

## Topology Diagram
![Topology](./topology.png)

---

## Addressing Table

| Device | Interface | IP Address | Subnet Mask | Description |
| :--- | :--- | :--- | :--- | :--- |
| **ISPR1** | G3/0 | `203.0.113.2` | `255.255.255.252` | ISP Gateway (Pre-configured) |
| **R1** | Loopback0 | `1.1.1.1` | `255.255.255.255` | Router ID |
| | G3/0 | `203.0.113.1` | `255.255.255.252` | Outside link to ISP |
| | G0/0 | `10.0.12.1` | `255.255.255.252` | Link to R2 |
| | F1/0 | `10.0.13.1` | `255.255.255.252` | Link to R3 |
| **R2** | Loopback0 | `2.2.2.2` | `255.255.255.255` | Router ID |
| | G0/0 | `10.0.12.2` | `255.255.255.252` | Link to R1 |
| | F1/0 | `10.0.24.1` | `255.255.255.252` | Link to R4 |
| **R3** | Loopback0 | `3.3.3.3` | `255.255.255.255` | Router ID |
| | F1/0 | `10.0.13.2` | `255.255.255.252` | Link to R1 |
| | F2/0 | `10.0.34.1` | `255.255.255.252` | Link to R4 |
| **R4** | Loopback0 | `4.4.4.4` | `255.255.255.255` | Router ID |
| | F1/0 | `10.0.24.2` | `255.255.255.252` | Link to R2 |
| | F2/0 | `10.0.34.2` | `255.255.255.252` | Link to R3 |
| | G0/0 | `192.168.4.254` | `255.255.255.0` | Default Gateway for PC1 |
| **PC1** | NIC | `192.168.4.1` | `255.255.255.0` | Gateway: `192.168.4.254` |

---

## Lab Objectives
1. Configure hostnames, IP addresses, and Loopback interfaces on all routers.
2. Enable OSPF process `1` in Area `0` directly on the physical and loopback interfaces.
3. Configure passive interfaces on non-peering ports (Loopbacks and LAN).
4. Set OSPF reference bandwidth to `10000` (10 Gbps) so FastEthernet has a cost of `100` and GigabitEthernet a cost of `10`.
5. Configure `R1` with a static default route to `ISPR1` and propagate it via OSPF (`default-information originate`).
6. Verify neighbor states, routing tables, and end-to-end reachability from `PC1` to `203.0.113.2`.

---

## Device Configurations

### R1
```ios
enable
configure terminal
hostname R1

interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 0
 exit

interface G3/0
 ip address 203.0.113.1 255.255.255.252
 no shutdown
 exit

interface G0/0
 ip address 10.0.12.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
 exit

interface F1/0
 ip address 10.0.13.1 255.255.255.252
 ip ospf 1 area 0
 no shutdown
 exit

ip route 0.0.0.0 0.0.0.0 203.0.113.2

router ospf 1
 router-id 1.1.1.1
 auto-cost reference-bandwidth 10000
 passive-interface Loopback0
 default-information originate
 exit

end
write memory
