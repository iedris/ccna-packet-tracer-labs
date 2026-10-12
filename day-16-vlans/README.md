# Day 16: VLANs (Virtual Local Area Networks)

## 📌 Lab Overview
This lab covers the configuration and verification of VLANs on Cisco Catalyst switches, separating broadcast domains and assigning access ports to designated VLANs.

## 🗺️ Topology
![Network Topology](topology.png)

## 🎯 Objectives
- Create and name VLANs according to lab requirements.
- Assign switch access ports to the appropriate VLANs.
- Verify VLAN database and port membership.
- Test connectivity between hosts in the same and different VLANs.

## 📋 VLAN & Port Assignment Table
| Device | Interface | VLAN ID | VLAN Name | Subnet |
| :--- | :--- | :--- | :--- | :--- |
| SW1 | Fa0/1 - Fa0/2 | 10 | Engineering | 192.168.10.0/24 |
| SW1 | Fa0/3 - Fa0/4 | 20 | HR | 192.168.20.0/24 |

## 🔍 Verification Commands
```bash
show vlan brief
show interfaces status
