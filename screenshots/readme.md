# Screenshots

This folder contains screenshots documenting the configuration and selected tests of the **Bank Secure Network**, an educational project built in Cisco Packet Tracer.

Screenshots document configuration and selected tests. Test outcomes are limited to the evidence recorded in the project documentation: see [ACL Configuration & Security Implementation](../documentation/acl-config.md) and the Testing & Verification section of the [root README](../README.md). A configuration screenshot alone does not show that a service works end to end.

---

## Configuration Screenshots

### Network Topology

`topology.png`

Shows the network topology, including the router, switches, PCs, ATMs, servers and Internet connection.

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

Shows the trunk links used to carry VLAN traffic across the network.

### DHCP Server

`dhcp.png`

Shows the DHCP configuration and address pools on the centralized DNS/DHCP server. This is a configuration screenshot and does not by itself verify client lease assignment.

### DNS Configuration

`dns.png`

Shows the DNS record used for internal banking server name resolution.

Example: `banking.local` → `192.168.40.10`

This is a configuration screenshot and does not by itself verify name resolution.

### Banking Server

`banking-server.png`

Shows the banking server configuration/access. It does not by itself verify HTTP, HTTPS or FTP functionality end to end.

---

## Test and Verification Screenshots

The results recorded in the project documentation are summarized in the [root README](../README.md#testing--verification).

### Employee ACL Tests

`employee-acl-connectivity-test.png`

Evidence of two Employee PC (VLAN 20) tests: the blocked ping to the Admin VLAN (`192.168.10.10`) and the successful ping to the Banking Server (`192.168.40.10`). The Banking Server ping verifies ICMP reachability only.

`employee-acl-counter-verification.png`

Evidence of the Employee ACL deny-counter increase during repeated blocked-traffic tests.

### ACL Testing

`acl-testing.png`

ACL testing screenshot. It is not treated as proof that every ACL restriction or network path was tested; the ATM-to-Admin and ATM-to-Employee restrictions are configured but still need dedicated tests. For the ACL rules, interface assignments, verified results and remaining untested restrictions, see [ACL Configuration & Security Implementation](../documentation/acl-config.md).

### DHCP Client

`dhcp-client.png`

Client-side DHCP screenshot. DHCP end-to-end results are not transcribed in the project documentation, so DHCP lease assignment is not counted as verified there.

### DNS Testing

`dns-test.png`

DNS test screenshot for the `banking.local` hostname. DNS resolution results are not transcribed in the project documentation, so DNS resolution is not counted as verified there.

---

## Purpose

These screenshots support the project documentation with visual evidence of how the network was configured and of the selected tests listed above. They do not demonstrate that every service, connectivity path or security control was tested.
