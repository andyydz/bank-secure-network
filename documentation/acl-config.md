# Bank Secure Network — ACL Configuration & Security Implementation

> **Documentation status:** Two named extended ACLs, `EMPLOYEE-SECURITY` and `ATM-SECURITY`, are implemented on the Cisco 2911 router and applied inbound on the Employee (VLAN 20) and ATM (VLAN 30) subinterfaces. The Employee-to-Admin restriction and Employee-to-Banking-Server connectivity were tested in Cisco Packet Tracer. The ATM restrictions are configured but **have not yet been independently tested** (see [Section 9](#9-testing--verification)).

**Confirmed project information**

| Item | Value |
|---|---|
| Router | Cisco 2911 |
| Switches | Three Cisco 2960 |
| Simulation platform | Cisco Packet Tracer |
| VLAN 10 – ADMIN gateway | `192.168.10.1` (`192.168.10.0/24`) |
| VLAN 20 – EMPLOYEE gateway | `192.168.20.1` (`192.168.20.0/24`) |
| VLAN 30 – ATM gateway | `192.168.30.1` (`192.168.30.0/24`) |
| VLAN 40 – SERVER gateway | `192.168.40.1` (`192.168.40.0/24`) |
| Banking Server | `192.168.40.10` |
| DNS/DHCP Server | `192.168.40.20` |

---

## 1. ACL Overview

**What is an ACL?**
An Access Control List (ACL) is an ordered list of `permit` and `deny` statements that a Cisco router uses to filter packets. Each statement matches traffic based on criteria such as source address, destination address, protocol and port, and tells the router what to do with matching packets.

**Why ACLs matter in a banking network**
A bank's network carries traffic from very different groups (administrators, employees, ATMs and servers), and not all of them should be able to reach each other. ACLs give the network administrator a way to define which communication is permitted and to restrict the rest.

**How ACLs control traffic between segments**
In this project, VLANs separate the devices into different networks, and the **Cisco 2911 router** routes traffic between them. Because inter-VLAN traffic passes through the router, ACLs applied there can permit or deny traffic between segments.

**How ACL processing works (general behavior)**

- Statements are evaluated **from top to bottom**.
- The **first matching statement** is applied, and evaluation stops.
- If no statement matches, the packet is dropped by the **implicit `deny any`** at the end of every ACL. In this project, both ACLs end with an explicit `permit ip any any`, so the implicit deny is never reached for IP traffic (see [Section 6](#6-rule-by-rule-explanation)).

---

## 2. Security Requirements

Traffic between the following network segments needs to be controlled:

| Segment | VLAN | Security consideration |
|---|:---:|---|
| Admin | 10 | Administrative systems; access to them from less-trusted segments should be restricted. |
| Employee | 20 | General staff systems; access to the Admin segment should be blocked. |
| ATM | 30 | ATM systems; access to the Admin and Employee segments should be blocked. |
| Server | 40 | Banking Server and DNS/DHCP Server; critical services that must remain reachable by the segments that need them. |

---

## 3. ACL Design

**Design intent**
The ACLs restrict traffic from the Employee and ATM segments toward the more sensitive internal segments (Admin, and for ATMs also Employee), while leaving all other traffic from those segments permitted, including access to the Server VLAN.

**Implemented ACL policy and test status**

This table separates what the ACL statements configure from what has actually been tested. Only flows matched by a `deny` statement or the final `permit ip any any` statement are described, and no flow is marked as tested unless there is test evidence.

| Traffic direction | Configured policy | Test status |
|---|:---:|---|
| Employee (VLAN 20) → Admin (VLAN 10) | Deny | **Tested:** ping failed and the deny counter increased (see [Section 9](#9-testing--verification)) |
| Employee (VLAN 20) → Server (VLAN 40) | Permit (`permit ip any any`) | **Tested:** ping to the Banking Server (`192.168.40.10`) succeeded, 0% loss |
| ATM (VLAN 30) → Admin (VLAN 10) | Deny | Configured, **not yet independently tested** |
| ATM (VLAN 30) → Employee (VLAN 20) | Deny | Configured, **not yet independently tested** |
| ATM (VLAN 30) → Server (VLAN 40) | Permit (`permit ip any any`) | Not tested |
| Other flows from Employee or ATM not matched by a deny rule | Permit (`permit ip any any`) | Not tested |

> **Scope note:** No other ACLs or interface assignments were confirmed beyond the two inbound ACLs above, so this document makes no claims about restrictions on traffic entering from the Admin or Server VLANs. DHCP, DNS and HTTP/HTTPS behavior were not validated by the evidence in this document.

---

## 4. ACL Placement

| Item | Value |
|---|---|
| Device where ACLs are applied | Cisco 2911 Router (the inter-VLAN routing point) |
| ACL type | Extended, named |

| Subinterface | VLAN | ACL | Direction |
|---|:---:|---|:---:|
| `GigabitEthernet0/1.20` | 20 (Employee) | `EMPLOYEE-SECURITY` | `in` |
| `GigabitEthernet0/1.30` | 30 (ATM) | `ATM-SECURITY` | `in` |

These assignments were verified with `show ip interface`. Because the ACLs are applied **inbound** on the VLAN subinterfaces, each one filters packets as they arrive from the Employee or ATM segment, before the router routes them toward another VLAN.

---

## 5. ACL Configuration

**Implemented ACLs** (as shown by `show access-lists` on the router)

```
Extended IP access list EMPLOYEE-SECURITY
 10 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 20 permit ip any any

Extended IP access list ATM-SECURITY
 10 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 20 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 30 permit ip any any
```

**Applying the ACLs to the subinterfaces (illustrative)**

> These commands are **reconstructed from the verified `show ip interface` results** (ACL name, subinterface and inbound direction). They were not copied from the router's running configuration.

```
interface GigabitEthernet0/1.20
 ip access-group EMPLOYEE-SECURITY in

interface GigabitEthernet0/1.30
 ip access-group ATM-SECURITY in
```

**What the commands and elements mean**

| Element | Meaning |
|---|---|
| `Extended IP access list <name>` | A named extended ACL, which can match on source, destination, protocol and port. |
| `10`, `20`, `30` | Sequence numbers that set the order in which rules are evaluated. |
| `permit` / `deny` | The action taken on traffic that matches the rule. |
| `ip` | Matches IPv4 traffic, regardless of transport protocol or port, between the specified source and destination addresses. |
| `192.168.x.0 0.0.0.255` | A `/24` network written with a wildcard mask; the `0` bits must match and the `255` bits are ignored. |
| `any` | Matches any address. |
| `ip access-group <name> in` | Applies the ACL to inbound traffic on the interface or subinterface. |

---

## 6. Rule-by-Rule Explanation

### `EMPLOYEE-SECURITY` (applied inbound on `GigabitEthernet0/1.20`)

| Rule | Source | Destination | Protocol | Action | Purpose |
|:---:|---|---|:---:|:---:|---|
| 10 | `192.168.20.0/24` (Employee) | `192.168.10.0/24` (Admin) | IP | Deny | Prevents Employee devices from reaching the Admin VLAN. |
| 20 | any | any | IP | Permit | Permits other IPv4 traffic that does not match an earlier deny statement, including traffic toward the Server VLAN. |

### `ATM-SECURITY` (applied inbound on `GigabitEthernet0/1.30`)

| Rule | Source | Destination | Protocol | Action | Purpose |
|:---:|---|---|:---:|:---:|---|
| 10 | `192.168.30.0/24` (ATM) | `192.168.10.0/24` (Admin) | IP | Deny | Prevents ATM devices from reaching the Admin VLAN. |
| 20 | `192.168.30.0/24` (ATM) | `192.168.20.0/24` (Employee) | IP | Deny | Prevents ATM devices from reaching the Employee VLAN. |
| 30 | any | any | IP | Permit | Permits other IPv4 traffic that does not match an earlier deny statement, including traffic toward the Server VLAN. |

**Explanation of the final `permit ip any any` rule**
Each ACL ends with an explicit permit-all rule. The `permit ip any any` rules allow IPv4 traffic that does not match an earlier deny statement, so such traffic is **permitted**, not dropped. This includes traffic toward the Server VLAN, although successful connectivity still depends on the remaining network configuration and the destination service. It also means these ACLs block **only** the specific flows in their `deny` statements. The implicit `deny any` is not the effective final rule for IP traffic in either ACL.

**Rule order matters**
The deny rules come first so they are evaluated before the permit-all rule. If `permit ip any any` were placed first, it would match every packet and the deny rules would never apply.

---

## 7. Traffic Flow

When a device in the Employee or ATM VLAN attempts to reach a device in another VLAN:

1. The device sends the traffic to the **default gateway** of its own VLAN (`192.168.20.1` for Employee, `192.168.30.1` for ATM).
2. The packet arrives **inbound** on the matching router subinterface (`GigabitEthernet0/1.20` or `GigabitEthernet0/1.30`).
3. The router checks the packet against the ACL on that subinterface **from the first rule to the last**.
4. The **first rule that matches** decides the outcome:
   - **`deny`** → the router drops the packet (for example, Employee → Admin).
   - **`permit`** → the router routes the packet toward the destination VLAN (for example, Employee → Server).

Because these ACLs are stateless IPv4 packet filters, they evaluate each packet against the rules on the subinterface where it enters the router. The inbound Employee ACL blocks matching traffic from VLAN 20 toward the Admin subnet, while the inbound ATM ACL blocks matching traffic from VLAN 30 toward the Admin and Employee subnets. These ACLs do not automatically inspect return traffic entering through other subinterfaces, and no stateful connection tracking is provided.

---

## 8. Security Benefits

- **Restricts access to the Admin VLAN:** the Employee and ATM VLANs cannot send IP traffic to `192.168.10.0/24` through the router.
- **Isolates ATMs from the Employee VLAN:** the ATM ACL includes a deny rule for traffic toward `192.168.20.0/24`.
- **Permits other IPv4 traffic:** the explicit `permit ip any any` rules allow traffic that does not match an earlier deny statement. This includes traffic toward the Server VLAN, although successful connectivity still depends on the remaining network configuration and the destination service.
- **Complements VLAN segmentation:** VLANs separate the devices into different networks, while ACLs define the rules for the communication that passes between those networks. VLANs create the boundaries, and ACLs control what may cross them.

---

## 9. Testing & Verification

ACL behavior was tested in Cisco Packet Tracer. Only the tests below were performed and recorded; no other result is claimed.

**Verified tests**

| # | Source | Destination | Test | Expected result | Actual result | Evidence |
|:---:|---|---|---|:---:|---|---|
| 1 | Employee PC (VLAN 20) | `192.168.10.10` (Admin VLAN) | Ping | Blocked | **Blocked.** The ping failed; the PC received destination-host-unreachable replies from gateway `192.168.20.1`. | [Connectivity test](../screenshots/employee-acl-connectivity-test.png) |
| 2 | Employee PC (VLAN 20) | Banking Server `192.168.40.10` | Ping | Permitted | **Permitted.** The ping succeeded with 0% packet loss in the captured test. | [Connectivity test](../screenshots/employee-acl-connectivity-test.png) |

**Supporting counter evidence (Employee-to-Admin restriction)**
The Employee ACL deny-rule counter increased from **4 to 8** across repeated blocked-traffic tests ([counter verification screenshot](../screenshots/employee-acl-counter-verification.png)). Together with the verified inbound assignment of `EMPLOYEE-SECURITY` on `GigabitEthernet0/1.20`, the increasing deny counter is evidence that the Employee-to-Admin restriction was functioning during this test.

**Pending tests (configured, not independently verified)**

| # | Source | Destination | Configured policy | Status |
|:---:|---|---|:---:|---|
| 3 | ATM (VLAN 30) | Admin (VLAN 10) | Deny (rule 10) | **Pending:** rule is configured; a dedicated connectivity test is still needed |
| 4 | ATM (VLAN 30) | Employee (VLAN 20) | Deny (rule 20) | **Pending:** rule is configured; a dedicated connectivity test is still needed |

The presence of these rules in `show access-lists` is configuration evidence only and is not recorded as a successful connectivity test. DHCP, DNS, HTTP/HTTPS and ATM-to-Server behavior were also not validated by this evidence.

**Standard IOS commands for inspecting ACLs**

```
show access-lists
show ip interface GigabitEthernet0/1.20
show ip interface GigabitEthernet0/1.30
show running-config
```

- `show access-lists` displays the configured ACLs and, in most cases, match counters for each rule.
- `show ip interface <subinterface>` shows which ACL is applied to the subinterface and in which direction.
- `show running-config` shows the complete configuration currently running on the router.

---

## 10. Limitations

- ACLs are **packet filters**. They do not inspect packet contents or detect attacks, and they do not provide complete banking-grade security.
- These are stateless IPv4 packet filters: each packet is evaluated individually on the subinterface where it enters the router, and no stateful connection tracking is provided.
- ACLs do not authenticate users or devices, and they do not encrypt traffic.
- The implemented ACLs end with `permit ip any any`, so they block only the specific flows listed in their deny rules; all other traffic from the Employee and ATM VLANs is permitted.
- The ACL evidence covers only the Employee and ATM ingress subinterfaces. No ACL assignment was confirmed for other subinterfaces.
- ACL rules are maintained manually, so they must be reviewed and updated as the network changes; an incorrect rule or rule order can allow or block traffic unintentionally.
- ATM restriction behavior has not yet been independently tested.
- This is an **educational simulation in Cisco Packet Tracer**, so the implementation demonstrates the concepts and does not represent a complete production banking environment.

---

## 11. Future Improvements

The following are possible **future improvements** and are **not part of the current implementation**:

- Dedicated connectivity tests for the ATM-to-Admin and ATM-to-Employee deny rules
- More granular access policies (for example, restricting the Server VLAN to specific hosts, protocols and ports instead of `permit ip any any`)
- Logging of ACL matches and denied traffic
- Stronger network monitoring
- Intrusion Detection / Prevention Systems (IDS/IPS)
- More advanced security controls beyond ACLs

---

## 12. Conclusion

In the Bank Secure Network, ACLs work together with VLAN segmentation to control how traffic moves between segments. `EMPLOYEE-SECURITY` and `ATM-SECURITY` are applied inbound on the Employee and ATM router subinterfaces to deny traffic toward the Admin VLAN (and, for ATMs, the Employee VLAN), while permitting other traffic. The collected evidence validates the **Employee-to-Admin restriction** (failed ping and increasing deny counter) and **Employee-to-Banking-Server connectivity** (successful ping with 0% loss). The **ATM restriction rules are configured, but their behavior still needs independent testing.**
