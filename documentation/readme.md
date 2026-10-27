# Documentation

This folder contains the technical documentation for the **Bank Secure Network**, an educational Cisco Packet Tracer simulation of a segmented banking network. It covers the network design, the IP addressing and VLAN plan, and the ACL implementation.

## Documentation Index

| Document | Link | Purpose |
|---|---|---|
| Network Design | [Network-Design.md](Network-Design.md) | Physical and logical topology, device roles, VLAN design, traffic flow, and design rationale |
| IP & VLAN Plan | [IP-VLAN-Plan.md](IP-VLAN-Plan.md) | VLAN IDs, subnets, gateways, server IP addresses, and documented DHCP pool starting addresses |
| ACL Configuration & Security Implementation | [acl-config.md](acl-config.md) | Implemented ACL rules, router subinterface assignments, configuration details, and test evidence |

## Addressing Overview

All VLANs use the subnet mask `255.255.255.0` (`/24`).

| VLAN | Name | Network | Gateway |
|:---:|---|---|---|
| 10 | ADMIN | `192.168.10.0/24` | `192.168.10.1` |
| 20 | EMPLOYEE | `192.168.20.0/24` | `192.168.20.1` |
| 30 | ATM | `192.168.30.0/24` | `192.168.30.1` |
| 40 | SERVER | `192.168.40.0/24` | `192.168.40.1` |

Servers in VLAN 40: Banking/Web Server `192.168.40.10`, DNS/DHCP Server `192.168.40.20`. See the [IP & VLAN Plan](IP-VLAN-Plan.md) for details.

## ACL and Verification Scope

- `EMPLOYEE-SECURITY` denies IPv4 traffic from the Employee subnet (`192.168.20.0/24`) to the Admin subnet (`192.168.10.0/24`).
- `ATM-SECURITY` denies IPv4 traffic from the ATM subnet (`192.168.30.0/24`) to the Admin subnet (`192.168.10.0/24`) and the Employee subnet (`192.168.20.0/24`).
- Both ACLs end with `permit ip any any`, so other IPv4 traffic that does not match a deny rule is permitted. These are specific restrictions, not a comprehensive security policy.

**Verification status:** Employee-to-Admin blocking and Employee-to-Banking-Server ICMP reachability were tested. The ATM restrictions and end-to-end DHCP, DNS, HTTP, HTTPS and FTP behavior have not been independently verified by the available evidence.

For the full rules, interface assignments and test evidence, see [ACL Configuration & Security Implementation](acl-config.md).

## Related Project Resources

These resources are outside the `documentation/` folder:

- [Root README](../README.md): project overview, architecture and testing summary
- [Packet Tracer instructions](../packet-tracer/readme.md): how to open the simulation file
- [Screenshot index](../screenshots/readme.md): configuration and test screenshots

---

This project is an educational simulation and is not a production banking network.
