# Bank Secure Network — IP Addressing & VLAN Plan

> **Document status:** This document records the IP addressing and VLAN plan documented for the Bank Secure Network project. It describes the planned addressing; it does not state that every host lease, network service or connectivity path has been independently verified. Individual client addresses are not specified here.

---

## 1. Overview

The Bank Secure Network is a Computer Networks project designed and implemented in **Cisco Packet Tracer**. It connects administrative systems, employee systems, ATMs and banking services through one **Cisco 2911 router** and **three Cisco 2960 switches**, using four VLANs.

A structured IP addressing and VLAN plan is important for this network because it:

- gives every group of devices a clearly defined place in the network,
- keeps the address space organized and easy to manage,
- makes it clear which gateway each group of devices uses, and
- provides the foundation for controlling communication between the different parts of the bank.

---

## 2. VLAN Allocation

| VLAN ID | VLAN Name | Purpose | Network Address | Subnet Mask | Default Gateway | Connected/Intended Devices |
|:---:|---|---|---|---|---|---|
| 10 | ADMIN | Administrative systems | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | Admin PCs |
| 20 | EMPLOYEE | Employee systems | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | Employee PCs |
| 30 | ATM | ATM systems | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | ATM PCs |
| 40 | SERVER | Banking and network services | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | Banking/Web Server, DNS/DHCP Server |

---

## 3. IP Addressing Scheme

**VLANs, subnets and gateways**

- Each VLAN is a separate logical network and uses its **own IP subnet**.
- Every subnet has a **default gateway**: the address that devices in that VLAN send traffic to when the destination is outside their own subnet. In this plan, the gateway is the `.1` address of each subnet.
- The third octet identifies the VLAN (`10`, `20`, `30`, `40`), so the address of a device shows which group it belongs to.

**Reading the `/24` notation**

- `/24` corresponds to the subnet mask `255.255.255.0`: the first three octets identify the network and the last octet identifies the host.
- A subnet's **network address** (for example, `192.168.10.0`) identifies the subnet itself and is not assigned to an individual host.

| VLAN | Network Address | Subnet Mask | Default Gateway |
|:---:|---|---|---|
| 10 (ADMIN) | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` |
| 20 (EMPLOYEE) | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` |
| 30 (ATM) | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` |
| 40 (SERVER) | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` |

---

## 4. Server IP Allocation

Both servers are in VLAN 40 (SERVER) and use **static server addresses**, with subnet mask `255.255.255.0` and default gateway `192.168.40.1`.

| Server | VLAN | Static Server Address | Subnet Mask | Default Gateway |
|---|:---:|---|---|---|
| Banking/Web Server | 40 (SERVER) | `192.168.40.10` | `255.255.255.0` | `192.168.40.1` |
| DNS/DHCP Server | 40 (SERVER) | `192.168.40.20` | `255.255.255.0` | `192.168.40.1` |

---

## 5. Device Addressing

| Device Group | VLAN | Addressing | Subnet Mask | Default Gateway |
|---|:---:|---|---|---|
| Admin PCs | 10 (ADMIN) | Not individually specified; DHCP pool starting address `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| Employee PCs | 20 (EMPLOYEE) | Not individually specified; DHCP pool starting address `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |
| ATM PCs | 30 (ATM) | Not individually specified; DHCP pool starting address `192.168.30.10` | `255.255.255.0` | `192.168.30.1` |
| Banking/Web Server | 40 (SERVER) | Static: `192.168.40.10` | `255.255.255.0` | `192.168.40.1` |
| DNS/DHCP Server | 40 (SERVER) | Static: `192.168.40.20` | `255.255.255.0` | `192.168.40.1` |

> **Note on client addresses:** The DHCP pool starting addresses above are the documented first addresses of each pool. They are **not** the exact addresses assigned to individual clients, and this document does not list individual PC addresses, pool end addresses, exclusions or lease details.

---

## 6. VLAN-to-Device Mapping

| VLAN | Name | Device Types in This VLAN |
|:---:|---|---|
| 10 | ADMIN | Admin PCs |
| 20 | EMPLOYEE | Employee PCs |
| 30 | ATM | ATM PCs |
| 40 | SERVER | Banking/Web Server, DNS/DHCP Server |

The Cisco 2911 router and the three Cisco 2960 switches provide connectivity for all of these VLANs.

---

## 7. Gateway & Inter-VLAN Routing

**Purpose of the gateways**

A device sends any traffic destined for a different network, including another VLAN, to the default gateway of its own VLAN:

| VLAN | Gateway | Used by |
|:---:|---|---|
| 10 (ADMIN) | `192.168.10.1` | Admin PCs |
| 20 (EMPLOYEE) | `192.168.20.1` | Employee PCs |
| 30 (ATM) | `192.168.30.1` | ATM PCs |
| 40 (SERVER) | `192.168.40.1` | Banking/Web Server, DNS/DHCP Server |

**How inter-VLAN communication works**

The project uses **router-on-a-stick** inter-VLAN routing: the Cisco 2911 router provides the Layer 3 gateway for each VLAN, and traffic between VLANs is routed through it. Devices in different VLANs cannot exchange traffic at Layer 2, so VLAN separation alone does not stop inter-VLAN communication once routing is in place. Because all inter-VLAN traffic passes through the router, it can be controlled with ACLs.

ACLs are applied on the router's Employee and ATM subinterfaces. They block specific flows and then explicitly permit other IPv4 traffic, so they do not block all inter-VLAN traffic. See [ACL Configuration & Security Implementation](acl-config.md) for the implemented rules, interface assignments, test evidence, and remaining unverified restrictions.

---

## 8. Addressing Design Rationale

Admin, Employee, ATM and Server networks each use a **separate address space**. This supports the design in the following ways:

- **Organization:** the address block of a device shows which group it belongs to (for example, a `192.168.30.x` address belongs to the ATM network).
- **Management:** each group has its own subnet and gateway, making the addressing easier to plan, document and troubleshoot.
- **Segmentation:** separate address spaces line up with the VLAN boundaries, so traffic between groups passes through the router, where it can be filtered by ACLs.
- **Separation of services:** banking and network services have their own subnet, separate from user and ATM devices.

---

## 9. IP & VLAN Summary

| VLAN ID | VLAN Name | Purpose | Network Address | Subnet Mask | Default Gateway | Devices | Known Addresses |
|:---:|---|---|---|---|---|---|---|
| 10 | ADMIN | Administrative systems | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | Admin PCs | DHCP pool starting address: `192.168.10.10` |
| 20 | EMPLOYEE | Employee systems | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | Employee PCs | DHCP pool starting address: `192.168.20.10` |
| 30 | ATM | ATM systems | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | ATM PCs | DHCP pool starting address: `192.168.30.10` |
| 40 | SERVER | Banking and network services | `192.168.40.0/24` | `255.255.255.0` | `192.168.40.1` | Banking/Web Server, DNS/DHCP Server | Static: Banking/Web `192.168.40.10`; DNS/DHCP `192.168.40.20` |
