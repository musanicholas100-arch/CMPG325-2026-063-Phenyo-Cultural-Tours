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
Total block: 172.30.38.0/23 (510 hosts) - mask 255.255.254.0

| VLAN | Dept | Subnet | Hosts | Gateway | Growth Proof |
|------|------|--------|-------|---------|--------------|
| 10 | Booking | 172.30.38.0/26 |.2-.62 (61) | 172.30.38.1 | 30 -> 38, 24 spare |
| 20 | Tour Ops | 172.30.38.64/26 |.66-.126 | 172.30.38.65 | 30 -> 38, 24 spare |
| 30 | Finance | 172.30.38.128/27 |.130-.158 | 172.30.38.129 | 15 -> 19, 11 spare |
| 40 | Marketing | 172.30.38.160/27 |.162-.190 | 172.30.38.161 | 15 -> 19, 11 spare |
| 99 | Servers | 172.30.38.192/27 |.194-.222 | 172.30.38.193 | 10 -> 13, 17 spare |
| 80 | Guest WiFi | 172.30.39.0/25 |.2-.126 | 172.30.39.1 | 60 -> 75, 51 spare |
| 1 | Mgmt | 172.30.39.128/25 |.130-.254 | 172.30.39.129 | reserve |
| Reserve | Future | 172.30.38.224/27 | 30 hosts | - | For CR5 |

CR5 Satisfied: No renumbering needed, 126+ IPs still free in upper /24.

## Topology
See /Design/topology.png - Core L3 3560 + 4 Access 2960 + ISP Router + Server Farm + 2 APs

## Security Constraint Implementation
- enable secret, service password-encryption
- console & vty passwords, SSH only, local admin user
- banner motd
