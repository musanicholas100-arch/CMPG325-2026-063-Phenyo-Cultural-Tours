# CMPG325-2026-063-Phenyo-Cultural-Tours

# CMPG325-2026-063 - Phenyo Cultural Tours (Potchefstroom)

**Student No:** 46898064  
**Client ID:** CLI-063  
**IP Block:** 172.30.38.0/23  
**Constraint:** All device administration must be secured  
**Challenge:** Port Security (switchport access control)  
**Change Request CR5:** 25% user growth without renumbering

## Project Overview

Phenyo Cultural Tours is a tourism company in Potchefstroom requiring a secure, scalable network for bookings, tour operations, finance and guest services.

## IP Addressing Plan - VLSM with CR5 Growth

Total block: 172.30.38.0/23 (510 usable host addresses) - mask 255.255.254.0

| VLAN | Dept | Subnet | Usable Host Range | Gateway | Growth Proof |
| --- | --- | --- | --- | --- | --- |
| 10 | Booking | 172.30.38.0/26 | .2-.62 | 172.30.38.1 | 30 -> 38, 24 spare |
| 20 | Tour Ops | 172.30.38.64/26 | .66-.126 | 172.30.38.65 | 30 -> 38, 24 spare |
| 30 | Finance | 172.30.38.128/27 | .130-.158 | 172.30.38.129 | 15 -> 19, 11 spare |
| 40 | Marketing | 172.30.38.160/27 | .162-.190 | 172.30.38.161 | 15 -> 19, 11 spare |
| 99 | Servers | 172.30.38.192/27 | .194-.222 | 172.30.38.193 | 10 -> 13, 17 spare |
| 80 | Guest WiFi | 172.30.39.0/25 | .2-.126 | 172.30.39.1 | 60 -> 75, 51 spare |
| 1 | Mgmt | 172.30.39.128/25 | .130-.254 | 172.30.39.129 | reserve |
| Reserve | Future | 172.30.38.224/27 | .225-.254 | - | For CR5 / future expansion |

### CR5 Verification

CR5 is satisfied because the existing VLSM subnets provide sufficient capacity for the required 25% user growth within each existing network. Booking can grow from 30 to 38 users, Tour Operations from 30 to 38, Finance from 15 to 19, Marketing from 15 to 19, and Guest WiFi from 60 to 75 without changing the existing subnet addresses. Therefore, no renumbering of the existing networks is required. The 172.30.38.224/27 reserve subnet provides an additional 30 usable addresses for future expansion.

## Topology

See `/Design/topology.png` - Core L3 3560 + 4 Access 2960 + ISP Router + Server Farm + 2 APs

## Security Constraint Implementation

- enable secret, service password-encryption
- console & VTY passwords, SSH only, local admin user
- banner motd

## Port Security

- Configure switchport port security on access ports
- Sticky MAC address learning
- Maximum 2 MAC addresses per access port
- Violation mode: restrict
- Verify configuration with appropriate show commands

## Network Services

- Inter-VLAN routing on the Core L3 3560 using SVIs
- Centralized DHCP/DNS server: 172.30.38.195
- Web server: 172.30.38.194
- DNS domain: phenyotours.local
- Website: www.phenyotours.co.za
- Guest WiFi: VLAN 80
- Management: VLAN 1
