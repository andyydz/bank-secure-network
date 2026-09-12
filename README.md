# Bank Secure Network

A secure banking network designed and simulated using **Cisco Packet Tracer**.

The project demonstrates how a banking environment can be divided into separate network segments using VLANs and secured using routing, DHCP, DNS, server services, and Access Control Lists (ACLs).

## Project Overview

The network is designed to represent a simplified banking infrastructure containing:

- Administrative users
- Bank employees
- ATMs
- Banking servers
- DNS/DHCP services
- Internet connectivity

The network uses **VLAN segmentation** to separate different departments and device types while **router-on-a-stick** enables communication between VLANs.

Security controls, including **Access Control Lists (ACLs)**, are used to restrict unauthorized communication between network segments.

## Objectives

- Design a structured banking network.
- Implement VLAN-based network segmentation.
- Configure inter-VLAN routing.
- Configure DHCP for automatic IP assignment.
- Configure DNS for internal name resolution.
- Host a banking web service.
- Implement ACL-based network security.
- Test and verify network connectivity and security rules.
- Document the complete network implementation.

## Network Architecture

The network consists of:

- 1 Cisco 2911 Router
- 3 Cisco 2960 Switches
- 2 Servers
- 2 ATMs
- 2 PCs
- Internet/Cloud connection

### Logical Network Segmentation

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.1 |
| 20 | EMPLOYEE | 192.168.20.0/24 | 192.168.20.1 |
| 30 | ATM | 192.168.30.0/24 | 192.168.30.1 |
| 40 | SERVER | 192.168.40.0/24 | 192.168.40.1 |

## Services

### DHCP

A dedicated DNS/DHCP server provides automatic IP configuration to clients in the:

- ADMIN VLAN
- EMPLOYEE VLAN
- ATM VLAN

DHCP relay is configured on the router so that clients in different VLANs can obtain addresses from the centralized DHCP server.

### DNS

Internal DNS resolution is configured for the banking server.

Example:

`banking.local` → `192.168.40.10`

### Banking Web Server

The Banking Server hosts an internal HTTP service that can be accessed using:

`http://banking.local`

## Security

The network uses VLAN segmentation and Access Control Lists to control communication between different network segments.

The intended security model includes:

- Controlled access to banking servers
- Restricted communication between user VLANs
- Controlled ATM access
- Prevention of unauthorized cross-VLAN communication

Detailed ACL rules and testing results are documented in the repository.

## Testing

The network is tested using:

- Ping
- DNS name resolution
- DHCP address assignment
- Web server access
- Inter-VLAN connectivity tests
- ACL allow/deny tests

Screenshots of the testing process are available in the [`screenshots`](./screenshots/) folder.

## Project Files

### Packet Tracer

The complete Cisco Packet Tracer project is available in the [`packet-tracer`](./packet-tracer/) folder.

### Documentation

Detailed project documentation is available in the [`documentation`](./documentation/) folder.

### Screenshots

Configuration and testing evidence is available in the [`screenshots`](./screenshots/) folder.

### Presentation

The project presentation is available in the [`presentation`](./presentation/) folder.

## Technologies Used

- Cisco Packet Tracer
- Cisco IOS
- VLAN
- IEEE 802.1Q Trunking
- Router-on-a-Stick
- DHCP
- DNS
- HTTP
- Access Control Lists (ACLs)
- IPv4 Networking

## Project Status

**In Progress**

The network infrastructure, VLANs, routing, DHCP, DNS, and server services have been implemented and tested. ACL configuration and final security testing will be completed as part of the final implementation.

## Disclaimer

This is an academic network simulation created using Cisco Packet Tracer. It is not intended to represent a production banking network.
