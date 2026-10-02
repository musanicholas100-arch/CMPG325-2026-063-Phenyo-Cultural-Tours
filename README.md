# CMPG325-2026-063 — Phenyo Cultural Tours (Potchefstroom)

**Client ID:** CLI-063
**IP Address Block:** `172.30.38.0/23`
**Security Constraint:** All device administration must be secured
**Security Challenge:** Port Security (switchport access control)
**Change Request CR5:** Support 25% user growth without renumbering

## 1. Project Overview

Phenyo Cultural Tours is a tourism company based in Potchefstroom, South Africa. The company requires a secure and scalable network to support Booking, Tour Operations, Finance, Marketing, Server, Management, and Guest Wi-Fi services.

This project implements a VLAN-based network using a multilayer core switch, access switches, centralized DHCP and DNS services, a web server, wireless access points, and a wireless LAN controller.

## 2. Repository Structure

The repository is organized into the following folders:

| Folder          | Purpose                                        |
| --------------- | ---------------------------------------------- |
| `Configs/`      | Network device configuration notes and backups |
| `Design/`       | Network topology diagram and addressing plan   |
| `Docs/`         | Project report and supporting documentation    |
| `PacketTracer/` | Cisco Packet Tracer project file               |
| `Testing/`      | Screenshots and connectivity test evidence     |

The main `README.md` is located in the repository root.

## 3. Network Topology

The network consists of:

* One Cisco 3560 multilayer core switch.
* Four Cisco 2960 access switches.
* One ISP router.
* Two servers for network and web services.
* One WLC-3504 wireless LAN controller.
* Wireless access points for staff and guest connectivity.
* Wired departmental PCs and wireless laptops.

**Topology diagram:** `Design/topology.png`

## 4. IP Addressing Plan — VLSM

The assigned address block is `172.30.38.0/23`, with subnet mask `255.255.254.0` and 510 usable IPv4 addresses across the entire block.

| VLAN | Department      | Network            | Usable Host Range    | Gateway         |
| ---: | --------------- | ------------------ | -------------------- | --------------- |
|   10 | Booking         | `172.30.38.0/26`   | `172.30.38.1–62`*    | `172.30.38.1`   |
|   20 | Tour Operations | `172.30.38.64/26`  | `172.30.38.65–126`*  | `172.30.38.65`  |
|   30 | Finance         | `172.30.38.128/27` | `172.30.38.129–158`* | `172.30.38.129` |
|   40 | Marketing       | `172.30.38.160/27` | `172.30.38.161–190`* | `172.30.38.161` |
|   99 | Servers         | `172.30.38.192/27` | `172.30.38.193–222`* | `172.30.38.193` |
|   80 | Guest Wi-Fi     | `172.30.39.0/25`   | `172.30.39.1–126`*   | `172.30.39.1`   |
|    1 | Management      | `172.30.39.128/25` | `172.30.39.129–254`* | `172.30.39.129` |
|    — | Future Reserve  | `172.30.38.224/27` | `172.30.38.225–254`  | Not assigned    |

*The usable range includes the gateway address, which is reserved and must not be assigned to a client.

### Infrastructure IP Addresses

| Device or Service             | IP Address      |
| ----------------------------- | --------------- |
| DHCP/DNS Server (`SERVER-01`) | `172.30.38.195` |
| Web Server (`SERVER-WEB`)     | `172.30.38.194` |
| WLC Management                | `172.30.39.130` |
| Management Gateway            | `172.30.39.129` |

The future reserve subnet is separate from the existing server subnet.

## 5. CR5 — 25% User Growth

The departmental subnets are designed to accommodate the specified growth without changing their existing network addresses.

| Department      | Current Users | Target After Growth | Subnet |
| --------------- | ------------: | ------------------: | ------ |
| Booking         |            30 |                  38 | `/26`  |
| Tour Operations |            30 |                  38 | `/26`  |
| Finance         |            15 |                  19 | `/27`  |
| Marketing       |            15 |                  19 | `/27`  |
| Guest Wi-Fi     |            60 |                  75 | `/25`  |

The listed subnets have sufficient host capacity for these targets, including one reserved gateway address per subnet. The separate `172.30.38.224/27` reserve subnet provides additional capacity for future expansion.

## 6. VLANs and Inter-VLAN Routing

The following VLANs are configured:

* **VLAN 10:** Booking
* **VLAN 20:** Tour Operations
* **VLAN 30:** Finance
* **VLAN 40:** Marketing
* **VLAN 80:** Guest Wi-Fi
* **VLAN 99:** Servers
* **VLAN 1:** Management

Inter-VLAN routing is implemented on the Cisco 3560 multilayer core switch using switched virtual interfaces (SVIs). DHCP relay is configured on the departmental and guest VLAN interfaces to forward client requests to the centralized DHCP server.

## 7. Network Services

* **DHCP:** Centralized address assignment from `172.30.38.195`.
* **DNS:** Name resolution through `172.30.38.195`.
* **Web Server:** `172.30.38.194`.
* **Internal DNS Domain:** `phenyotours.local`.
* **Website Hostname:** `www.phenyotours.co.za`.
* **Guest Wireless Network:** `Phenyo-Guest`, assigned to VLAN 80.
* **Staff Wireless Network:** `Phenyo-Staff`.
* **Management Network:** VLAN 1.

## 8. Security Implementation

The network uses the following device-administration security controls:

* Enable secret configured on network devices.
* Password encryption enabled.
* Local administrative account configured.
* Console authentication enabled.
* SSH version 2 configured for remote administration.
* Telnet disabled on configured VTY lines.
* Login warning banner configured.
* Port security enabled on designated access ports.

### Port Security

The access-port security configuration includes:

* Sticky MAC address learning.
* Maximum of two MAC addresses per port.
* Violation mode set to `restrict`.
* Verification using Cisco IOS `show port-security` commands.

These controls limit the number of MAC addresses learned on protected access ports and restrict traffic when a security violation occurs.

## 9. Testing and Verification

The following checks were performed during implementation:

* Verified VLAN interfaces and gateway addresses.
* Verified connected networks using `show ip route`.
* Tested DHCP address assignment on departmental and wireless clients.
* Tested connectivity to gateways and the DHCP/DNS server.
* Tested DNS resolution for `www.phenyotours.co.za` and `phenyotours.local`.
* Opened the website through its DNS hostname.
* Verified port-security status and violation counters.
* Verified SSH version 2.
* Tested staff and guest wireless connectivity.

Screenshots documenting these tests should be stored in `Testing/`.

## 10. Project Evidence

Recommended evidence files inside `Testing/`:

* `01_Network_Topology.png`
* `02_VLANs.png`
* `03_Routing_Table.png`
* `04_Booking_Connectivity.png`
* `05_Website_DNS_Test.png`
* `06_Port_Security.png`
* `07_SSH_Security.png`

The actual filenames may differ if your screenshots were saved under different names. Update this list to match the files uploaded to GitHub.

## 11. Project Files

* **Network design:** `Design/`
* **Device configurations:** `Configs/`
* **Documentation:** `Docs/`
* **Packet Tracer project:** `PacketTracer/`
* **Testing evidence:** `Testing/`

The final submission should include the saved Cisco Packet Tracer project, supporting documentation, screenshots, and the required demonstration video.

## 12. Conclusion

The project implements a segmented network with inter-VLAN routing, centralized network services, wireless connectivity, and device-administration security. Connectivity and service tests were performed to verify the main network functions.

Any remaining wireless-controller requirements should be documented according to the functionality that was successfully configured and verified.
