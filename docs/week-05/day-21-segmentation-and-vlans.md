# Day 21 — Segmentation and VLANs

**Week 05: Network Security**  
**Canonical path:** `docs/week-05/day-21-segmentation-and-vlans.md`  
**Prerequisite:** [Day 20 — Routing, Switching, and Packet Flow](../week-04/day-20-routing-switching-and-packet-flow.md)  
**Security+ SY0-701 alignment:** Domain 3.0, Security Architecture; especially **Objective 3.2**, applying security principles to secure enterprise infrastructure (including network segmentation and access controls). Related architectural context: Objective 3.1. This lesson explains the concepts; it is not a reproduction of CompTIA's official objective wording.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain how network segmentation reduces unnecessary connectivity and limits lateral movement.
2. Distinguish physical segmentation, VLAN-based logical segmentation, subnetting, and microsegmentation.
3. Explain access ports, trunk ports, IEEE 802.1Q tagging, and the role of a Layer 3 device in inter-VLAN routing.
4. Identify trust boundaries and select appropriate traffic controls between segments.
5. Sketch a small-office VLAN layout, including example subnets, gateways, and permitted flows.
6. Recognize common configuration errors and evaluate segmentation choices in Security+ scenarios.

## Why segmentation matters

Imagine an office with employee laptops, payment terminals, guest Wi-Fi, printers, and security cameras. If every device can freely reach every other device, a compromised guest device or camera may have a path toward systems containing sensitive data. A single shared network makes that risk harder to contain.

**Network segmentation** divides a network into smaller zones and establishes rules for traffic between them. The goal is not simply to create different address ranges: it is to **control which systems can communicate, over which protocols, and for what business purpose**.

Segmentation can help:

- Limit **lateral movement** after an initial compromise.
- Reduce the exposure of sensitive systems to untrusted devices.
- Restrict broadcast domains when VLANs are used.
- Apply different monitoring and access policies to different device classes.
- Make incident containment more manageable.

Segmentation is one layer of defense, not a guarantee that malware cannot spread. Weak firewall rules, shared credentials, compromised management systems, or incorrectly configured trunks can undermine the design.

## 1. Start with the trust boundary

A **trust boundary** is a point where communications cross between systems or zones with different security assumptions. Crossing that boundary should trigger a deliberate policy decision.

For example, a guest's phone and a finance workstation have different roles. Even if both need internet access, the guest phone does not need to initiate connections to the finance workstation.

**Questions to ask when drawing a boundary:**

1. What devices and data live in this zone?
2. Who manages these devices, and how trustworthy are they?
3. Which destinations must these devices reach to perform their jobs?
4. Which traffic should be blocked by default?
5. Where will traffic be filtered, logged, and monitored?

**Least privilege** applies to networks as well as user accounts: permit only the network access required for the task. A useful starting policy is **deny by default between zones, then allow documented exceptions**. This must be implemented carefully so essential services continue to work.

## 2. Segmentation methods: what is actually being separated?

| Method | How separation works | What it does well | Important limitation |
|---|---|---|---|
| Physical segmentation | Separate switches, cabling, or network equipment | Strong equipment-level separation | Higher cost and operational complexity; connections between networks still need controls |
| VLAN-based logical segmentation | Managed switches place ports or tagged traffic into separate Layer 2 broadcast domains | Flexible separation on shared switch infrastructure | VLANs alone do not define safe inter-VLAN access policies |
| IP subnetting | Splits an IP address space into smaller Layer 3 networks | Organizes addressing and routing | A subnet boundary is **not automatically** an enforced security boundary |
| Firewall zoning | Firewall interfaces, subinterfaces, or policies define zones and allowed flows | Explicit inspection and enforcement across zones | Overly broad allow rules can defeat the design |
| Microsegmentation | Fine-grained controls around workloads, hosts, or applications | Restricts east–west traffic within a larger network or data center | Requires accurate policy and asset visibility |

**Remember:** A VLAN is a Layer 2 concept; an IP subnet is a Layer 3 addressing concept. Administrators commonly map one IP subnet to one VLAN, but the two are not identical.

### Broadcast domains and VLANs

A **broadcast domain** is the set of devices that receive a Layer 2 broadcast on a given network. Each VLAN normally forms its own broadcast domain. A switch forwards broadcasts within the relevant VLAN, not indiscriminately into every VLAN.

A VLAN does not encrypt traffic. Devices in the **same VLAN** may still communicate directly unless additional controls—such as host firewalls, private VLANs, or port isolation—restrict them.

<details markdown="1">
<summary>Check your understanding: VLAN or subnet?</summary>

**Scenario:** A company changes the IP addresses of two departments but leaves every device in the same untagged switch VLAN.

**Answer:** It has changed the Layer 3 addressing arrangement, but it has **not created separate Layer 2 VLAN broadcast domains**. Different subnets may require routing to communicate normally, but address changes alone are not a reliable security isolation control.

</details>

## 3. How VLANs work on a switch

A **managed switch** can assign switch ports to VLANs. A VLAN identifier (VLAN ID) distinguishes the logical Layer 2 networks; IEEE **802.1Q** is the common standard for tagging VLAN traffic across Ethernet links.

### Access ports

An **access port** normally carries traffic for one assigned VLAN. It commonly connects to an endpoint such as a desktop, printer, or camera. The endpoint usually sends and receives ordinary **untagged** Ethernet frames; the switch associates those frames with the access VLAN.

Example: ports 1–8 belong to VLAN 10 (Employees), and ports 9–12 belong to VLAN 30 (Printers). Traffic from an employee laptop on port 3 belongs to VLAN 10, even though the laptop is unaware of the VLAN number.

### Trunk ports

A **trunk port** carries traffic for multiple VLANs across one physical connection, commonly between switches or between a switch and a VLAN-aware router, firewall, or wireless access point.

On an 802.1Q trunk, frames for most VLANs are tagged with their VLAN ID. A configured **native VLAN** may carry untagged frames, depending on the equipment and configuration. Do not assume every frame on a trunk is tagged.

**Security practice:** Restrict a trunk to the VLANs it actually needs; avoid unnecessary VLAN exposure. Treat native-VLAN configuration consistently at both ends. Disable unused ports and configure them according to organizational policy rather than leaving them in a production VLAN.

### VLAN ID versus IP subnet

Consider this typical pairing:

| VLAN | Purpose | Example IPv4 subnet | Example default gateway |
|---|---|---|---|
| 10 | Employees | `192.168.10.0/24` | `192.168.10.1` |
| 20 | Finance | `192.168.20.0/24` | `192.168.20.1` |
| 30 | Printers | `192.168.30.0/24` | `192.168.30.1` |
| 40 | Guest Wi-Fi | `192.168.40.0/24` | `192.168.40.1` |
| 50 | Cameras / IoT | `192.168.50.0/24` | `192.168.50.1` |
| 99 | Management | `192.168.99.0/24` | `192.168.99.1` |

These are **illustrative private-address examples**, not mandatory VLAN numbers or a universal standard. A `/24` subnet contains 256 total IPv4 addresses, typically 254 usable host addresses under conventional subnetting rules.

## 4. How traffic crosses VLANs

A Layer 2 switch does not normally forward ordinary traffic from VLAN 10 into VLAN 20. To communicate between IP subnets in different VLANs, packets need **inter-VLAN routing**, performed by a router, a Layer 3 switch, or a firewall with routing capabilities.

### Packet-flow example

An employee laptop at `192.168.10.25` needs to reach a finance application at `192.168.20.50`:

1. The laptop compares the destination with its own subnet mask and sees that the destination is outside its local subnet.
2. It sends the packet toward its configured default gateway, such as `192.168.10.1`.
3. The routing device determines how to reach the finance subnet.
4. **If an enforcement point applies an access policy**, it evaluates whether this employee and service are permitted to reach the finance application.
5. An allowed packet is forwarded toward VLAN 20; a denied packet is dropped according to policy.

**Critical distinction:** Routing enables connectivity; a firewall policy or appropriate access control restricts it. A Layer 3 switch can route between VLANs without automatically enforcing the strict isolation you intended.

### Router-on-a-stick and Layer 3 switching

- **Router-on-a-stick:** One physical router interface uses VLAN-tagged subinterfaces to route among multiple VLANs. The switch-to-router link is typically an 802.1Q trunk.
- **Layer 3 switch:** Switch virtual interfaces (**SVIs**) can act as default gateways for VLANs and perform routing in the switch. ACLs or other security controls must be configured if restrictions are required.
- **Firewall-based inter-VLAN routing:** A firewall terminates or routes VLAN interfaces and applies zone-based policies. This can provide useful centralized control and logging, depending on design and platform.

There is no single required architecture for every office. The exam may ask you to choose the control that actually **enforces** separation rather than merely creates VLANs.

## 5. Build a small-office segmentation design

### The business scenario

A 20-person office has employee laptops, a small finance team, network printers, guest Wi-Fi, IP cameras, and a managed switch. The company needs internet access, shared printing, and secure administration, but guests should never browse internal systems.

### Example logical diagram

```text
                         Internet
                            |
                    [Firewall / Router]
                    VLAN gateways + rules
                            |
                      802.1Q trunk
                            |
                     [Managed Switch]
                      /   |   |   \
             VLAN 10    20  30   50       VLAN 99
             Employees Finance Printers Cameras  Management
                            |
                     [Wi-Fi Access Point]
                       VLAN 10 / 40
                    Staff SSID / Guest SSID
```

**Diagram note:** This is a logical sketch, not a port-by-port wiring plan. The wireless access point may connect via a trunk that carries the VLANs needed for its SSIDs. Actual port and firewall arrangements depend on the hardware.

### Sample traffic policy

| Source | Destination | Proposed rule | Reason |
|---|---|---|---|
| Guest VLAN 40 | Internet | Allow required outbound services | Guests need internet connectivity |
| Guest VLAN 40 | Internal VLANs 10/20/30/50/99 | Deny | Guests have no internal business role |
| Employees VLAN 10 | Approved printers VLAN 30 | Allow only necessary printing protocols and destinations | Support printing without general printer-network access |
| Employees VLAN 10 | Finance VLAN 20 | Deny by default; add narrowly scoped exceptions if justified | Protect sensitive applications |
| Cameras VLAN 50 | Approved recording server | Allow required application traffic only | Avoid broad IoT connectivity |
| Cameras VLAN 50 | Employee and finance networks | Deny | Reduce lateral-movement paths |
| Management VLAN 99 | Switches, firewall, access points | Allow authorized administration protocols from approved administrator devices | Restrict management-plane access |
| Other VLANs | Management VLAN 99 | Deny by default | Protect infrastructure administration |

**Design caveat:** Permit essential supporting services such as DHCP, DNS, NTP, and required authentication or update traffic through explicit, documented rules. DHCP may need a relay when the server is on another subnet. Also consider IPv6; filtering only IPv4 can leave an unintended path.

### Trust-boundary checklist

- **Guest boundary:** Internet access without access to private LAN destinations; also consider wireless client isolation.
- **Finance boundary:** Only approved users and services can reach sensitive applications.
- **IoT boundary:** Cameras and similar devices reach only their required controllers, update services, and time sources.
- **Management boundary:** Administrative access originates from trusted administrator devices; use strong authentication and secure protocols.
- **Endpoint boundary:** Host firewalls and endpoint controls still matter, even inside a VLAN.

<details markdown="1">
<summary>Reveal design decision: Where should the firewall policy live?</summary>

**Answer:** At a point through which the relevant inter-zone traffic actually passes. If the Layer 3 switch routes directly between VLANs, a separate perimeter firewall may **never see that traffic**. In that case, apply suitable ACLs or segmentation enforcement on the Layer 3 switch, or change the topology so traffic passes through the firewall. Confirm the actual packet path before relying on a policy.

</details>

## 6. Guided lab: sketch and test your design

**Goal:** Draw a small-office VLAN layout and justify each trust boundary. This is a paper or diagramming exercise; no network hardware is required.

### Part A — Sketch

1. Draw a box labeled **Firewall/Router** connected to **Internet**.
2. Connect it to a **Managed Switch** using a line labeled **802.1Q trunk** (assuming the chosen firewall supports tagged VLAN interfaces).
3. Draw endpoint groups for Employees, Finance, Printers, Guests, Cameras, and Management.
4. Label each group with the VLAN ID, example subnet, and gateway from the table above.
5. Add an access point with a staff SSID mapped to VLAN 10 and a guest SSID mapped to VLAN 40.
6. Mark **deny** boundaries and the few justified **allow** flows.

### Part B — Predict the path

For each proposed connection, answer: *Same VLAN or different VLAN? Does it need routing? What policy should decide whether it is permitted?*

| Attempted connection | Expected design decision |
|---|---|
| Guest phone → finance workstation | Deny across the guest-to-finance boundary |
| Employee laptop → approved printer | Permit required printing traffic only |
| Camera → finance file share | Deny |
| Authorized admin laptop → switch management interface | Permit approved secure management traffic |
| Finance workstation → required DNS resolver | Permit DNS to the designated resolver |

### Part C — Optional safe observation commands

On a device you own or are authorized to administer, you can inspect local addressing and routing:

**macOS / Linux:**

```bash
ip route            # Linux: view routes, if iproute2 is installed
```

```bash
netstat -rn         # macOS: view the routing table
```

**Windows PowerShell:**

```powershell
Get-NetIPConfiguration
Get-NetRoute
```

These commands reveal local configuration, **not proof that VLAN separation is enforced**. To verify enforcement in an authorized lab, inspect switch VLAN/trunk settings and firewall or ACL rules, then test permitted and prohibited paths from appropriate test devices. A failed ping alone does not prove segmentation because ICMP may be filtered independently.

<details markdown="1">
<summary>Reveal the lab's most important lesson</summary>

**A successful segmentation design identifies both the boundary and the enforcement point.** Merely assigning six VLAN IDs does not prove that finance data is protected. Confirm that routing, firewall rules, ACLs, wireless SSID mapping, and management access all support the intended policy.

</details>

## 7. Common misconfigurations and security pitfalls

### Overly permissive inter-VLAN rules

A rule such as “allow all internal VLANs to all internal VLANs” can erase the security benefit of separating them. Prefer specific source, destination, protocol, and port combinations.

### Unnecessary trunk VLANs

A trunk that carries VLANs not needed by the connected device expands exposure and makes configuration errors more consequential. Allowlist required VLANs.

### VLAN hopping misconceptions

**VLAN hopping** refers to techniques that exploit certain switch or trunk misconfigurations to reach traffic associated with another VLAN. Examples discussed in networking security include switch spoofing and double-tagging under specific conditions. These are **not automatic capabilities** of any device plugged into a VLAN. Properly configured access ports, disabled unwanted dynamic trunk negotiation where applicable, consistent native-VLAN handling, and restricted trunks reduce risk.

### Confusing guest SSIDs with isolation

A separate wireless network name (**SSID**) is not itself proof of segmentation. Verify that the guest SSID maps to the intended VLAN and that upstream policies block internal access. Consider wireless client isolation when guests should not reach each other.

### Forgetting the management plane

Switches, firewalls, and access points often expose administrative interfaces. Restrict these interfaces to trusted management sources, use secure administration protocols, and protect credentials. A separate management VLAN helps organization, but access controls provide the actual restriction.

### Forgetting east–west traffic

**North–south traffic** generally refers to traffic entering or leaving a network or environment. **East–west traffic** generally refers to traffic between internal systems or workloads. A perimeter firewall may protect north–south traffic while missing lateral movement that stays inside the network.

### Treating VLANs as complete isolation

VLANs help organize Layer 2 traffic, but they are not equivalent to physical air gaps, encryption, or comprehensive zero-trust enforcement. Use layered controls appropriate to the threat model.

## 8. Security+ comparisons and exam traps

| If the question says… | Think about… |
|---|---|
| “Separate broadcast domains on the same managed switch” | VLANs |
| “Assign distinct IP address ranges” | Subnetting; not necessarily access control |
| “Allow only approved flows between departments” | Firewall rules, ACLs, or another enforcement mechanism |
| “Multiple VLANs over one switch-to-switch cable” | 802.1Q trunk |
| “One endpoint connected to one VLAN” | Access port |
| “Default gateway for a VLAN on a Layer 3 switch” | SVI |
| “Restrict traffic between individual workloads” | Microsegmentation |
| “Compromised endpoint reaches other internal hosts” | Lateral movement; assess east–west controls |
| “Guest Wi-Fi cannot reach internal devices” | Guest VLAN **plus enforced filtering**, not merely a separate SSID |

**Exam trap:** “The administrator created separate VLANs, so all departments are securely isolated.” This conclusion is too strong. Ask whether inter-VLAN routing is enabled and what rules govern it.

## 9. Key terminology and takeaways

- **Segmentation:** Dividing networks into zones with controlled communication paths.
- **Trust boundary:** A point between systems or zones with different trust assumptions.
- **VLAN:** Logical Layer 2 broadcast domain on managed network infrastructure.
- **802.1Q:** Common Ethernet VLAN-tagging standard.
- **Access port:** Usually an endpoint-facing port assigned to one VLAN.
- **Trunk port:** Link carrying multiple VLANs, commonly with 802.1Q tags.
- **Native VLAN:** VLAN associated with untagged traffic on an 802.1Q trunk in relevant configurations.
- **Subnet:** Layer 3 IP addressing division; not by itself a firewall policy.
- **Inter-VLAN routing:** Layer 3 forwarding between VLAN-associated subnets.
- **SVI:** Switch virtual interface, often a VLAN's Layer 3 gateway.
- **ACL:** Access control list that can permit or deny specified traffic.
- **Microsegmentation:** Fine-grained control among hosts or workloads.
- **Lateral movement:** An attacker moving from one compromised system to other systems.
- **East–west traffic:** Traffic among internal systems or workloads.

**Core takeaway:** Design VLANs around business roles and trust, but evaluate security by the **traffic that is actually allowed across boundaries**, not by the number of VLANs on the diagram.

## 10. Practice questions

### Question 1

A company places guest devices and finance workstations on separate VLANs. What additional step MOST directly prevents guests from reaching finance systems?

A. Increase the guest VLAN's DHCP lease time  
B. Apply a deny policy between guest and finance networks  
C. Rename the guest wireless SSID  
D. Change the finance subnet mask to `/24`

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** A traffic-filtering policy enforces the trust boundary. VLAN separation alone does not specify what a routing device may forward between VLANs. DHCP lease duration, SSID names, and a conventional subnet mask do not enforce the required denial.

</details>

### Question 2

An administrator needs to carry VLANs 10, 20, and 40 between two managed switches using one Ethernet cable. Which configuration is MOST appropriate?

A. Configure an 802.1Q trunk allowing those VLANs  
B. Configure both ends as access ports in VLAN 10  
C. Disable VLAN tagging and use a single broadcast domain  
D. Assign the same IP address to both switches

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** An 802.1Q trunk carries multiple VLANs over a shared link. Access ports normally carry one assigned VLAN; disabling VLAN separation or duplicating IP addresses does not solve the requirement.

</details>

### Question 3

A Layer 3 switch has SVIs for Employees and Finance and routes freely between them. A perimeter firewall has a rule denying Employees-to-Finance traffic, but tests show the traffic still passes. What is the MOST likely explanation?

A. VLAN tags encrypt traffic before it reaches the firewall  
B. The Layer 3 switch routes traffic locally, bypassing the perimeter firewall  
C. An SVI cannot serve as a default gateway  
D. IP subnetting prevents ACLs from working

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** If the switch performs inter-VLAN routing locally, the perimeter firewall may not see those flows. The administrator must enforce the restriction along the actual path, for example with suitable ACLs or revised routing topology.

</details>

### Question 4

A security team wants to limit communications between individual application workloads, even when they share a larger network segment. Which approach BEST fits the requirement?

A. Increase the size of the shared subnet  
B. Replace DNS with static hostnames  
C. Implement microsegmentation policies  
D. Move all workloads into one unrestricted VLAN

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Microsegmentation enforces fine-grained communication policies between workloads. Larger subnets and unrestricted VLANs do not restrict east–west traffic; DNS changes are unrelated to the main control objective.

</details>

### Question 5

A small office creates a guest SSID and maps it to VLAN 40. Which verification BEST demonstrates that the intended guest isolation works?

A. Confirm that the SSID has a different name from the staff SSID  
B. Confirm that guest devices receive addresses through DHCP  
C. Confirm that the access point has a blinking status LED  
D. Verify the VLAN mapping and filtering rules, then test that guest clients cannot reach internal networks

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** Verify both the configuration and the effective access policy. SSID naming, DHCP operation, and device status alone cannot demonstrate the trust boundary.

</details>

## 11. Further reading

- [CompTIA Security+ certification and exam information](https://www.comptia.org/certifications/security) — consult the official SY0-701 objectives for authoritative exam scope.
- [IEEE 802.1 Working Group](https://www.ieee802.org/1/) — standards context for bridged networks and VLANs.
- [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final) — broader context for minimizing implicit trust and controlling access.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
