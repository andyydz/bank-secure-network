# Cisco Packet Tracer Project

This folder contains the Cisco Packet Tracer simulation of the **Bank Secure Network**, an educational networking project. It is a classroom-style simulation, not a production banking network.

## File

`Bank-Secure-Network-FINAL.pkt`

The file is located in this `packet-tracer/` directory and contains the network topology and device configurations.

## Network Components

- Cisco 2911 Router
- Three Cisco 2960 Switches
- Admin PCs
- Employee PCs
- ATM PCs
- Banking Server
- DNS/DHCP Server

## Documented Configuration

The simulation is designed around the following features:

- VLAN segmentation
- 802.1Q trunking
- Router-on-a-stick inter-VLAN routing
- DHCP and DHCP relay
- DNS
- HTTP
- Access Control Lists (ACLs)

This list describes the documented design. It does not mean every feature or connectivity path has been independently tested (see [ACL Testing Status](#acl-testing-status)).

## VLANs

| VLAN | Name | Network | Default Gateway |
|------|------|---------|-----------------|
| 10 | ADMIN | `192.168.10.0/24` | `192.168.10.1` |
| 20 | EMPLOYEE | `192.168.20.0/24` | `192.168.20.1` |
| 30 | ATM | `192.168.30.0/24` | `192.168.30.1` |
| 40 | SERVER | `192.168.40.0/24` | `192.168.40.1` |

## Server Addressing

| Device | VLAN | IP Address |
|--------|:----:|------------|
| Banking Server | 40 | `192.168.40.10` |
| DNS/DHCP Server | 40 | `192.168.40.20` |

## ACL Testing Status

The verified ACL rules, interface assignments and test evidence are documented in [ACL Configuration & Security Implementation](../documentation/acl-config.md).

- **Tested:** the Employee-to-Admin restriction, and ping from an Employee PC to the Banking Server.
- **Configured, not yet independently tested:** the ATM-to-Admin and ATM-to-Employee restrictions.

No other service or connectivity path is claimed as verified here.

## Usage

1. Open `Bank-Secure-Network-FINAL.pkt` (in this `packet-tracer/` folder) with **Cisco Packet Tracer**.
2. Use it to inspect the device configurations, VLANs, routing, DHCP, DNS, server services, ACL rules and connectivity.

## Note

The `.pkt` file is an academic simulation and should be opened with a compatible version of Cisco Packet Tracer.
