# Bank Secure Network — Problem Statement

## 1. Background

Banks depend on their networks for almost every part of their operation: administrators manage systems, employees serve customers, ATMs process transactions, and servers hold banking services and data. This communication must be both **reliable** (services must stay available) and **secure** (sensitive systems must be protected from misuse).

A bank's network is not a single group of similar devices. It contains groups with very different roles and levels of trust:

| Group | Typical role | Why it needs separate treatment |
|---|---|---|
| Administrators | Manage and maintain systems | Have elevated access and should not share a segment with general users |
| Employees | Day-to-day banking work | Need access to banking services, but not to administrative resources |
| ATMs | Customer-facing transaction systems | Are exposed to the public and should be kept apart from internal staff devices |
| Servers | Host banking and network services | Are the most critical assets and need the strongest protection |

Placing all of these on one unrestricted network means every device can reach every other device. A problem affecting one group, whether a misconfiguration, a misused account or a compromised machine, can then affect all of them.

---

## 2. Existing/General Problem

In a poorly segmented banking network, the following issues can occur:

- **Unauthorized access:** any device on the network may attempt to reach sensitive systems.
- **Unrestricted communication between departments:** employee, ATM and administrative devices can communicate freely, even when there is no business need.
- **Poor network segmentation:** all devices share the same broadcast domain, so traffic and problems are not contained.
- **Security risks to sensitive banking servers:** servers sit on the same network as ordinary user and ATM devices, which increases their exposure.
- **Difficulty managing IP addresses and network services:** manually assigning addresses and managing name resolution becomes error-prone and hard to scale.
- **Lack of controlled access between network segments:** there is no single place where communication rules can be defined and enforced.

The core problem is that a flat, unrestricted network gives the bank **no separation between roles and no control over who can talk to what**.

---

## 3. Why We Chose This Project

We chose to design a **Bank Secure Network** because a banking environment is a clear, realistic case where network design directly affects security. It allowed us to apply core Computer Networks concepts to a practical problem rather than to isolated exercises.

The technologies used in this project fit the problem well:

- **VLANs** separate different groups of devices into their own logical networks.
- **Inter-VLAN routing** allows those separate networks to communicate in a controlled way through the router.
- **DHCP** automates IP configuration, so addresses do not have to be assigned manually.
- **DNS** allows services to be reached by name.
- **ACLs** let us define which traffic is permitted and which is restricted.

Together, these technologies address the problems described above, and they can be built, configured and tested in Cisco Packet Tracer.

---

## 4. Proposed Solution

We designed and implemented a secure banking network in **Cisco Packet Tracer**, made up of a Cisco 2911 router, three Cisco 2960 switches, Admin PCs, Employee PCs, ATM PCs, a Banking/Web Server, a DNS/DHCP Server and an Internet/Cloud connection.

At a high level, the solution consists of:

- **VLAN-based segmentation** of the network, with switch configuration and trunking to carry the VLANs between the network devices.
- **Separate Admin, Employee, ATM and Server networks:**

| VLAN | Name | Gateway |
|---|---|---|
| 10 | ADMIN | 192.168.10.1 |
| 20 | EMPLOYEE | 192.168.20.1 |
| 30 | ATM | 192.168.30.1 |
| 40 | SERVER | Server network |

- **Router-based inter-VLAN communication**, so that traffic between VLANs passes through the router.
- **DHCP and DNS services**, provided by the DNS/DHCP Server at `192.168.40.20`.
- **A Banking/Web Server** hosted in the server network.
- **ACL-based access control** to restrict unauthorized traffic between network segments.
- **Testing and verification** to confirm that the network behaves as designed.

---

## 5. How Each Major Step Helps

| Technology / Design Choice | Problem It Addresses |
|---|---|
| **VLANs** | Poor segmentation: devices are grouped by role into separate logical networks. |
| **Trunking between switches** | Allows the separate VLANs to be carried across the switching infrastructure. |
| **Inter-VLAN routing** | Uncontrolled communication: traffic between VLANs passes through the router, where it can be managed. |
| **DHCP** | Difficulty managing IP addresses: devices receive their IP configuration automatically. |
| **DNS** | Difficulty managing network services: services can be reached by name instead of by address alone. |
| **ACLs** | Unauthorized access: traffic that should not be allowed between segments can be restricted. |
| **Dedicated Server VLAN** | Risk to critical systems: banking and network services are isolated from user and ATM devices. |
| **Testing and verification** | Uncertainty about correctness: confirms that the intended communication works and that the restrictions behave as designed. |

---

## 6. Project Objective

The objective of this project is to **design, implement and verify a segmented and access-controlled banking network in Cisco Packet Tracer**, in which:

- Admin, Employee, ATM and Server systems are placed in separate VLANs.
- Communication between VLANs is routed and controlled rather than unrestricted.
- IP configuration and name resolution are provided by dedicated network services.
- Critical banking and network services are isolated in their own segment.
- Access between segments is restricted using ACLs.
- The working of the network is tested and verified.

---

## 7. Scope

**Included in this project**

- VLAN configuration and IP addressing
- Switch configuration and trunking
- Inter-VLAN routing on the Cisco 2911 router
- DHCP and DNS services
- A Banking/Web Server
- ACL-based network security and other network security features
- Connectivity testing and verification
- An Internet/Cloud connection

**Limitations**

- This is a **simulated, educational project** built in Cisco Packet Tracer.
- It demonstrates core networking and network-security concepts applied to a banking scenario.
- It is **not a complete production banking infrastructure**, and it does not claim to meet the full requirements of a real-world banking environment.

---

## 8. Expected Outcome

By the end of the project, we expect to have a working simulated banking network that demonstrates that:

- The network is divided into **four separate VLANs** (Admin, Employee, ATM and Server), each with its own purpose.
- Devices receive their **IP configuration automatically** through DHCP and can use **DNS** for name resolution.
- Devices in different VLANs can communicate **through the router** where this is permitted.
- **ACLs restrict** traffic that should not be allowed between segments.
- The **Banking/Web Server** and DNS/DHCP Server are kept in a **dedicated server network**.
- The behavior of the network has been **tested and verified** against the design.

Overall, the project shows how VLANs, inter-VLAN routing, DHCP, DNS and ACLs work together to make a banking network more organized, controlled and secure.
