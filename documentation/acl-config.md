# Bank Secure Network — ACL Configuration & Security Implementation

> **Documentation status:** The Bank Secure Network uses ACL-based security to control traffic between network segments. The specific rules, interface names, protocols and test results have **not yet been supplied** for this document, so they are marked **TBD** or shown as clearly labelled **placeholders**. Nothing in this document claims that a particular type of traffic is allowed or blocked until the actual configuration is filled in.

**Confirmed project information used in this document**

| Item | Value |
|---|---|
| Router | Cisco 2911 |
| Switches | Three Cisco 2960 |
| Simulation platform | Cisco Packet Tracer |
| VLAN 10 – ADMIN gateway | `192.168.10.1` |
| VLAN 20 – EMPLOYEE gateway | `192.168.20.1` |
| VLAN 30 – ATM gateway | `192.168.30.1` |
| VLAN 40 – SERVER gateway | `192.168.40.1` |
| DNS/DHCP Server | `192.168.40.20` |

---

## 1. ACL Overview

**What is an ACL?**
An Access Control List (ACL) is an ordered list of `permit` and `deny` statements that a Cisco router uses to filter packets. Each statement matches traffic based on criteria such as source address, destination address, protocol and port, and tells the router what to do with matching packets.

**Why ACLs matter in a banking network**
A bank's network carries traffic from very different groups (administrators, employees, ATMs and servers), and not all of them should be able to reach each other. ACLs give the network administrator a way to define which communication is permitted and to restrict the rest.

**How ACLs control traffic between segments**
In this project, VLANs separate the devices into different networks, and the **Cisco 2911 router** routes traffic between them. Because inter-VLAN traffic passes through the router, ACLs can be applied there to decide whether traffic between segments is permitted or denied.

**How ACL processing works (general behavior)**

- Statements are evaluated **from top to bottom**.
- The **first matching statement** is applied, and evaluation stops.
- If no statement matches, the packet is dropped by the **implicit `deny any`** at the end of every ACL.

---

## 2. Security Requirements

Traffic between the following network segments needs to be controlled:

| Segment | VLAN | Security consideration |
|---|:---:|---|
| Admin | 10 | Administrative systems; access to and from them should be deliberately controlled. |
| Employee | 20 | General staff systems; access to other segments should be limited to what is required. |
| ATM | 30 | ATM systems; access should be restricted to what ATM operation requires. |
| Server | 40 | Banking/Web Server and DNS/DHCP Server; critical services that require protected access. |

The exact set of communications each segment requires is defined by the implemented ACL policy (see [Section 3](#3-acl-design)).

---

## 3. ACL Design

**Design intent**
The intent of the ACL design is to allow only the communication between segments that is required and to restrict other inter-VLAN traffic, so that access between the Admin, Employee, ATM and Server networks is controlled.

**Implemented traffic policy**

The table below is to be completed from the actual configuration. No entry has been assumed.

| Traffic Direction | Policy | Notes |
|---|:---:|---|
| Admin (VLAN 10) → Server (VLAN 40) | TBD | |
| Employee (VLAN 20) → Server (VLAN 40) | TBD | |
| ATM (VLAN 30) → Server (VLAN 40) | TBD | |
| Employee (VLAN 20) → Admin (VLAN 10) | TBD | |
| ATM (VLAN 30) → Admin (VLAN 10) | TBD | |
| ATM (VLAN 30) → Employee (VLAN 20) | TBD | |
| Other flows (for example, from the Server VLAN) | TBD | |

> **Design note:** The DNS/DHCP Server (`192.168.40.20`) provides network services from the Server VLAN, so the ACL policy needs to be reviewed with respect to how other VLANs reach it. What the implemented ACLs actually permit for this traffic is **TBD**.

---

## 4. ACL Placement

| Item | Value |
|---|---|
| Device where ACLs are applied | Cisco 2911 Router *(the inter-VLAN routing point)* |
| Interface / subinterface(s) | **TBD** |
| Direction (`in` / `out`) | **TBD** |
| ACL type (standard / extended) | **TBD** |
| ACL name(s) / number(s) | **TBD** |

The placement details above must be filled in from the actual router configuration.

---

## 5. ACL Configuration

> **Placeholder configuration.** The actual ACL commands were not provided. The block below shows only the *structure* of Cisco IOS ACL commands, with every project-specific value left as a placeholder. **It is not the implemented configuration and must be replaced with the real commands from the router.**

**Defining the ACL (placeholder)**

```
! ---- PLACEHOLDER: replace with the implemented ACL ----
ip access-list extended <ACL-NAME-TBD>
 remark <PURPOSE-OF-RULE-TBD>
 <permit|deny> <protocol-TBD> <source-network-TBD> <source-wildcard-TBD> <destination-network-TBD> <destination-wildcard-TBD> [eq <port-TBD>]
 <permit|deny> <protocol-TBD> <source-TBD> <destination-TBD>
 ! ... additional rules (TBD)
```

**Applying the ACL to an interface (placeholder)**

```
! ---- PLACEHOLDER: replace with the actual interface and direction ----
interface <INTERFACE-OR-SUBINTERFACE-TBD>
 ip access-group <ACL-NAME-TBD> <in|out>
```

**What the placeholder commands mean**

| Command / element | Meaning |
|---|---|
| `ip access-list extended <name>` | Creates a named extended ACL, which can match on source, destination, protocol and port. |
| `remark` | Adds a description to document the purpose of a rule. |
| `permit` / `deny` | The action taken on traffic that matches the rule. |
| Source / destination + wildcard mask | Identifies the network or host being matched. Wildcard masks depend on the subnet masks, which are **TBD**. |
| `eq <port>` | Optionally matches a specific port number. |
| `interface <interface>` | Selects the router interface or subinterface where the ACL will be applied. |
| `ip access-group <name> in\|out` | Applies the ACL to the interface for inbound or outbound traffic. |

---

## 6. Rule-by-Rule Explanation

This table should list every implemented rule in the order it appears in the ACL. The rows below are **placeholders**.

| Rule | Source | Destination | Protocol/Port | Action | Purpose |
|:---:|---|---|---|:---:|---|
| 1 | TBD | TBD | TBD | TBD | TBD |
| 2 | TBD | TBD | TBD | TBD | TBD |
| 3 | TBD | TBD | TBD | TBD | TBD |
| … | TBD | TBD | TBD | TBD | TBD |
| Implicit | any | any | any | deny | Drops any traffic not matched by an earlier rule (default behavior of every Cisco IOS ACL). |

---

## 7. Traffic Flow

When a device in one VLAN attempts to reach a device in another VLAN:

1. The device sends the traffic to the **default gateway** of its own VLAN (for example, `192.168.10.1` for VLAN 10).
2. The **Cisco 2911 router** receives the packet and determines the destination network.
3. On the interface where the ACL is applied, the router checks the packet against the ACL **from the first rule to the last**.
4. The **first rule that matches** decides the outcome:
   - **`permit`** → the router forwards the packet to the destination VLAN.
   - **`deny`** → the router drops the packet.
5. If **no rule matches**, the implicit `deny any` drops the packet.

Which flows are permitted and which are dropped depends on the implemented rules in [Section 6](#6-rule-by-rule-explanation).

---

## 8. Security Benefits

- **Reduces unauthorized access:** traffic that is not explicitly permitted between segments can be dropped by the router.
- **Controlled communication:** inter-VLAN communication is limited to what the ACL policy allows, instead of every VLAN being able to reach every other VLAN.
- **Protects critical services:** ACLs allow access to the Server VLAN to be restricted, supporting the isolation of the banking and network services.
- **Complements VLAN segmentation:** VLANs separate the devices into different networks, while ACLs define the rules for the communication that passes between those networks. VLANs create the boundaries, and ACLs control what may cross them.

---

## 9. Testing & Verification

ACL functionality was tested in Cisco Packet Tracer by attempting communication between segments and comparing the results with the intended policy.

**Test results**

The table below is a template. Every result must be recorded from the actual tests.

| # | Source | Destination | Test | Expected Result | Actual Result |
|:---:|---|---|---|:---:|:---:|
| 1 | TBD | TBD | TBD | TBD | TBD |
| 2 | TBD | TBD | TBD | TBD | TBD |
| 3 | TBD | TBD | TBD | TBD | TBD |
| 4 | TBD | TBD | TBD | TBD | TBD |

Successful (permitted) and blocked (denied) communication should both be included, so that the tests demonstrate that permitted traffic works and restricted traffic is stopped.

**Standard IOS commands for inspecting ACLs**

```
show access-lists
show ip interface <INTERFACE-OR-SUBINTERFACE-TBD>
show running-config
```

- `show access-lists` displays the configured ACLs and, in most cases, match counters for each rule.
- `show ip interface <interface>` shows which ACL is applied to an interface and in which direction.
- `show running-config` shows the complete configuration currently running on the router.


---

## 10. Limitations

- ACLs are **packet filters**. They do not inspect packet contents or detect attacks, and they do not provide complete banking-grade security.
- Traditional ACLs evaluate packets individually and do not track the state of a connection.
- ACLs do not authenticate users or devices, and they do not encrypt traffic.
- ACL rules are maintained manually, so they must be reviewed and updated as the network changes; an incorrect rule or rule order can allow or block traffic unintentionally.
- This is a **simulation in Cisco Packet Tracer**, so the implementation demonstrates the concepts and does not represent a complete production banking environment.
- Rules are limited to the traffic and behavior covered by the implemented configuration (see [Section 6](#6-rule-by-rule-explanation)).

---

## 11. Future Improvements

The following are possible **future improvements** and are **not part of the current implementation**:

- More granular access policies (for example, by specific hosts, protocols and ports)
- Logging of ACL matches and denied traffic
- Stronger network monitoring
- Intrusion Detection / Prevention Systems (IDS/IPS)
- More advanced security controls beyond ACLs

---

## 12. Conclusion

In the Bank Secure Network, ACLs work together with VLAN segmentation to control how traffic moves between the Admin, Employee, ATM and Server networks. VLANs separate the devices into distinct segments, and the router-based ACLs define which communication between those segments is permitted and which is restricted. Once the actual rules, placement and test results are added to the sections marked **TBD**, this document will serve as the complete record of the project's ACL implementation.
