# Screenshots

This folder contains screenshots documenting the configuration and testing of the **Bank Secure Network** in Cisco Packet Tracer.

## Screenshot Categories

### Network Topology

`topology.png`

Shows the complete physical network topology, including the router, switches, PCs, ATMs, servers, and Internet connection.

### VLAN Configuration

`vlan-config.png`

Shows the VLAN configuration on the switches.

The network uses:

- VLAN 10 – ADMIN
- VLAN 20 – EMPLOYEE
- VLAN 30 – ATM
- VLAN 40 – SERVER

### Trunk Configuration

`trunk-config.png`

Shows the trunk links used to carry traffic between VLANs across the network.

### DHCP Server

`dhcp-server.png`

Shows the DHCP configuration and address pools on the centralized DNS/DHCP server.

### DHCP Client

`dhcp-client.png`

Shows an end device receiving its IP configuration automatically through DHCP.

### DNS Configuration

`dns-config.png`

Shows the DNS record used for internal banking server name resolution.

Example:

`banking.local` → `192.168.40.10`

### DNS Testing

`dns-test.png`

Shows successful DNS resolution and connectivity testing using the `banking.local` hostname.

### Banking Server

`banking-server.png`

Shows the banking web service hosted on the Banking Server and accessed through the internal network.

### ACL Testing

`acl-testing.png`

Shows the results of ACL security testing, including permitted and blocked traffic between network segments.

## Purpose

These screenshots provide visual evidence that the network configuration, services, connectivity, and security controls were implemented and tested successfully.
