# Cisco Packet Tracer Project

This folder contains the complete Cisco Packet Tracer simulation of the **Bank Secure Network**.

## File

`Bank-Secure-Network.pkt`

The `.pkt` file contains the complete network topology and device configurations.

## Network Components

The simulation includes:

- Cisco 2911 Router
- Cisco 2960 Switches
- Administrative PCs
- Employee PCs
- ATMs
- Banking Server
- DNS/DHCP Server
- Internet/Cloud connection

## Network Configuration

The Packet Tracer project implements:

- VLAN segmentation
- 802.1Q trunking
- Router-on-a-stick
- Inter-VLAN routing
- DHCP
- DHCP relay
- DNS
- HTTP
- Access Control Lists (ACLs)

## VLANs

| VLAN | Name | Network |
|------|------|---------|
| 10 | ADMIN | 192.168.10.0/24 |
| 20 | EMPLOYEE | 192.168.20.0/24 |
| 30 | ATM | 192.168.30.0/24 |
| 40 | SERVER | 192.168.40.0/24 |

## Server Addressing

| Device | IP Address |
|--------|------------|
| Banking Server | 192.168.40.10 |
| DNS/DHCP Server | 192.168.40.20 |

## Usage

Open the `.pkt` file using **Cisco Packet Tracer**.

The project can be used to inspect:

- Device configurations
- VLAN configuration
- Routing
- DHCP
- DNS
- Server services
- ACL rules
- Network connectivity

## Note

The `.pkt` file represents an academic network simulation and should be opened using a compatible version of Cisco Packet Tracer.
