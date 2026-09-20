# Bank Secure Network — IP Addressing & VLAN Plan

> **Note:** This document records only the confirmed addressing information for the project. Any value that has not been specified is marked **TBD** (To Be Confirmed) rather than assumed.

---

## 1. Overview

The Bank Secure Network is a Computer Networks project designed and implemented in **Cisco Packet Tracer**. It connects administrative systems, employee systems, ATMs and banking services through a **Cisco 2911 router** and **three Cisco 2960 switches**.

A structured IP addressing and VLAN plan is important for this network because it:

- gives every group of devices a clearly defined place in the network,
- keeps the address space organized and easy to manage,
- makes it clear which gateway each group of devices uses, and
- provides the foundation for controlling communication between the different parts of the bank.

---

## 2. VLAN Allocation

| VLAN ID | VLAN Name | Purpose | Network Address | Default Gateway | Connected/Intended Devices |
|:---:|---|---|---|---|---|
| 10 | ADMIN | Administrative systems | TBD | `192.168.10.1` | Admin PCs |
| 20 | EMPLOYEE | Employee systems | TBD | `192.168.20.1` | Employee PCs |
| 30 | ATM | ATM systems | TBD | `192.168.30.1` | ATM PCs |
| 40 | SERVER | Banking and network services | TBD | `192.168.40.1` | Banking/Web Server, DNS/DHCP Server |

---

## 3. IP Addressing Scheme

**Relationship between VLANs, subnets and gateways**

- Each VLAN is a separate logical network, and each VLAN uses its **own IP subnet**.
- Every subnet has a **default gateway**, which is the address that devices in that VLAN send traffic to when the destination is outside their own subnet.
- The gateway addresses in this project follow a consistent pattern: the third octet matches the VLAN's position in the plan (`10`, `20`, `30`, `40`) and the gateway is the `.1` address of that block.

**Confirmed gateway addresses**

| VLAN | Gateway |
|---|---|
| 10 (ADMIN) | `192.168.10.1` |
| 20 (EMPLOYEE) | `192.168.20.1` |
| 30 (ATM) | `192.168.30.1` |
| 40 (SERVER) | `192.168.40.1` |

**Subnet masks and network addresses**

The subnet mask has not been specified in the confirmed information, and the network address of each subnet depends on it. Because they cannot be derived with certainty from the gateway addresses alone, they are marked **TBD** in this document and must be confirmed against the project's configuration before being filled in.

| VLAN | Subnet Mask | Network Address |
|:---:|---|---|
| 10 | TBD | TBD |
| 20 | TBD | TBD |
| 30 | TBD | TBD |
| 40 | TBD | TBD |

---

## 4. Server IP Allocation

| Server | VLAN | IP Address | Status |
|---|:---:|---|---|
| DNS/DHCP Server | 40 | `192.168.40.20` | Confirmed |
| Banking/Web Server | 40 | `TBD` | To be confirmed |

The Banking/Web Server address has not been specified and is left blank here to be completed once confirmed.

---

## 5. Device Addressing

| Device Group | VLAN | IP Address | Default Gateway |
|---|:---:|---|---|
| Admin PCs | 10 | TBD | `192.168.10.1` |
| Employee PCs | 20 | TBD | `192.168.20.1` |
| ATM PCs | 30 | TBD | `192.168.30.1` |
| DNS/DHCP Server | 40 | `192.168.40.20` | `192.168.40.1` |
| Banking/Web Server | 40 | TBD | `192.168.40.1` |

> Individual PC addresses have not been specified in the confirmed information, so no host addresses are listed for the client devices.

---

## 6. VLAN-to-Device Mapping

| VLAN | Name | Device Types in This VLAN |
|:---:|---|---|
| 10 | ADMIN | Admin PCs |
| 20 | EMPLOYEE | Employee PCs |
| 30 | ATM | ATM PCs |
| 40 | SERVER | Banking/Web Server, DNS/DHCP Server |

Network infrastructure devices (the Cisco 2911 router and the three Cisco 2960 switches) provide connectivity for all of these VLANs.

---

## 7. Gateway & Inter-VLAN Routing

**Purpose of the gateways**

Each VLAN has its own default gateway. A device sends any traffic destined for a different network, whether another VLAN or the Internet/Cloud, to the gateway of its own VLAN:

| VLAN | Gateway | Used by |
|:---:|---|---|
| 10 | `192.168.10.1` | Admin PCs |
| 20 | `192.168.20.1` | Employee PCs |
| 30 | `192.168.30.1` | ATM PCs |
| 40 | `192.168.40.1` | Banking/Web Server, DNS/DHCP Server |

**How inter-VLAN communication works**

Devices in different VLANs cannot exchange traffic at Layer 2, so the **Cisco 2911 router** provides the Layer 3 connection between them. Traffic leaving a VLAN goes to that VLAN's gateway, and the router forwards it toward the destination VLAN. Because all inter-VLAN traffic passes through the router, communication between VLANs can be controlled rather than left unrestricted.

---

## 8. Addressing Design Rationale

Admin, Employee, ATM and Server networks each use a **separate address space**. This supports the design in the following ways:

- **Organization:** the address block of a device immediately shows which group it belongs to (for example, a `192.168.30.x` address belongs to the ATM network).
- **Management:** each group has its own gateway, making the addressing easier to plan, document and troubleshoot.
- **Security:** separate address spaces line up with the VLAN boundaries, so traffic between groups must pass through the router, where access between segments can be controlled.
- **Isolation of critical services:** banking and network services have their own address space, separate from user and ATM devices.

---

## 9. IP & VLAN Summary

Values marked **TBD** have not been confirmed.

| VLAN ID | VLAN Name | Purpose | Default Gateway | Network / Mask | Devices | Known Device IPs |
|:---:|---|---|---|:---:|---|---|
| 10 | ADMIN | Administrative systems | `192.168.10.1` | TBD | Admin PCs | TBD |
| 20 | EMPLOYEE | Employee systems | `192.168.20.1` | TBD | Employee PCs | TBD |
| 30 | ATM | ATM systems | `192.168.30.1` | TBD | ATM PCs | TBD |
| 40 | SERVER | Banking and network services | `192.168.40.1` | TBD | Banking/Web Server, DNS/DHCP Server | DNS/DHCP: `192.168.40.20`; Banking/Web: TBD |
