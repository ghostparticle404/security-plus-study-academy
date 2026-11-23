# Day 22 — Firewalls and Filtering

**Week 05: Network Security**  
**Canonical path:** `docs/week-05/day-22-firewalls-and-filtering.md`  
**Prerequisites:** [Day 20 — Routing, Switching, and Packet Flow](../week-04/day-20-routing-switching-and-packet-flow.md) and [Day 21 — Segmentation and VLANs](day-21-segmentation-and-vlans.md)  
**Security+ SY0-701 alignment:** Domain 3.0, Security Architecture; particularly **Objective 3.2**, security principles for enterprise infrastructure, including firewalls and network appliances. Related: Domain 4.0, Security Operations, especially **Objective 4.4**, security alerting and monitoring. Objective references indicate topical alignment, not a quotation of official CompTIA wording.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain what a firewall does and where it enforces a trust boundary.
2. Compare **stateless**, **stateful**, and **next-generation firewalls (NGFWs)**, including their visibility and limitations.
3. Interpret firewall rules using source, destination, protocol, port, direction, action, and order.
4. Trace a packet through an ordered rule set and predict whether it will be allowed or denied.
5. Distinguish **network firewalls**, **host firewalls**, **web application firewalls (WAFs)**, and **intrusion detection/prevention systems (IDS/IPS)**.
6. Evaluate a small-office firewall policy using least privilege, default deny, logging, and change control.

## Why firewalls matter

Yesterday, we separated a small office into employee, finance, printer, guest, camera, and management VLANs. That design creates useful **zones**, but the existence of VLANs does not itself determine which zones may communicate. A router or Layer 3 switch can route between VLANs if configured to do so. A **firewall** makes and enforces decisions about permitted traffic at a boundary.

Imagine that a guest Wi-Fi device tries to connect to a finance server. The guest device has an IP address and may know the server's address; the important security question is whether the connection should be permitted. A firewall policy can explicitly deny that flow while allowing the same guest device to reach the internet.

A firewall is a **traffic-control enforcement point**. It examines traffic against configured policy and allows, blocks, rejects, or sometimes inspects and logs the traffic. Firewalls are essential, but they are not a substitute for secure endpoint configuration, identity controls, patching, or monitoring.

## 1. Firewalls enforce network policy

A **firewall rule** expresses a condition and an action. Rules commonly examine:

| Field | Meaning | Example |
|---|---|---|
| Source | Where traffic originates | Guest VLAN `192.168.40.0/24` |
| Destination | Where traffic is going | Finance server `192.168.20.10` |
| Protocol | Layer 3/4 protocol | TCP, UDP, ICMP |
| Port/service | Application-associated transport port, where applicable | TCP 443 (HTTPS) |
| Direction/interface/zone | Boundary the traffic crosses | Guest → Internet |
| Action | What to do with a match | Allow, drop, reject |
| Logging | Whether to record the decision | Log denied attempts |

**Source and destination are directional.** An allow rule for Employees → Printers does not necessarily allow Printers → Employees to initiate new connections. Whether return traffic works automatically depends on the firewall's connection-tracking behavior and policy.

### Allow, drop, and reject

- **Allow / permit:** Traffic matching the rule may pass, subject to any other required inspection or security controls.
- **Drop / deny silently:** The firewall discards matching traffic without an explicit refusal response. The sender may experience a timeout.
- **Reject:** The firewall blocks traffic and may send a response such as an ICMP unreachable message or TCP reset, depending on implementation.

Terminology differs among vendors. Some products use *deny* to mean silent drop; others distinguish *deny* and *reject*. **Read the platform's actual semantics.**

<details markdown="1">
<summary>Check your understanding: source or destination?</summary>

A rule permits `192.168.10.0/24` to reach `192.168.30.25` over TCP 9100. The source is the **employee subnet**, the destination is a **specific printer**, and TCP 9100 is the permitted destination service. The rule does not grant arbitrary printer-to-employee connections.

</details>

## 2. Stateless packet filtering

A **stateless firewall** evaluates each packet independently against criteria such as source/destination IP address, protocol, and port. It does **not** maintain a connection-state table that recognizes packets as belonging to an established conversation.

Suppose a client at `192.168.10.25` opens an HTTPS connection to a server at `203.0.113.10:443`. The outbound TCP packet typically uses a temporary source port, such as `51514`, and destination port `443`. The server's reply reverses the endpoints: source port `443`, destination port `51514`.

With stateless filtering, the administrator must account for **both directions** if filtering applies in both directions. A rule allowing only outbound traffic to destination TCP 443 does not inherently recognize the inbound reply as related traffic.

**Advantages:** Simple decisions, predictable packet-level matching, and potentially low overhead.  
**Limitations:** No inherent understanding of whether a packet belongs to a legitimate established connection; reverse-path rules can become broad or cumbersome.

**Security+ exam clue:** “Evaluates each packet independently” points to **stateless** filtering.

## 3. Stateful inspection and connection tracking

A **stateful firewall** tracks connection or flow state so that it can recognize traffic related to an allowed exchange. For TCP, it can observe connection establishment and track subsequent packets; for UDP, which has no handshake, devices commonly use timed flow entries based on observed traffic.

A simplified TCP example:

1. An employee client sends a TCP SYN to a permitted web server on port 443.
2. The firewall evaluates the new connection against policy and allows it.
3. The firewall creates or updates an entry in its **state table** (also called a connection table).
4. The server's matching response is recognized as return traffic and can be permitted without a broad rule allowing all unsolicited inbound connections.
5. The state entry eventually expires or is removed when the flow closes or times out.

This is **not** the same as trusting all traffic that claims to be a reply. Implementations validate traffic against tracked state and other policy. Nor does “stateful” automatically mean that the firewall can understand the contents of encrypted HTTPS messages.

| Feature | Stateless filtering | Stateful inspection |
|---|---|---|
| Decision basis | Individual packet headers and rules | Packet headers, rules, and tracked flow state |
| Return traffic | Must be accounted for explicitly where filtering applies | Related return traffic can be recognized automatically |
| Connection table | Not required | Fundamental to operation |
| Application awareness | Not inherent | Not necessarily present |
| Typical tradeoff | Simpler, less context | More context, uses resources for state tracking |

<details markdown="1">
<summary>Check your understanding: what happens to the reply?</summary>

**Scenario:** An employee initiates an allowed TCP connection to an external HTTPS server. A stateful firewall receives the matching response.

**Answer:** It can allow the response using the connection-state entry. This does **not** imply that any external host can initiate a new inbound connection.

</details>

## 4. Next-generation firewalls (NGFWs)

A **next-generation firewall (NGFW)** adds deeper inspection and application-oriented controls beyond conventional port-and-address rules. Capabilities vary by product, but commonly include:

- **Application identification and control:** Differentiate applications or application categories even when they share TCP 443.
- **Intrusion prevention:** Detect and block traffic patterns associated with known exploits or malicious activity.
- **Threat intelligence integration:** Apply reputation or indicator-based policy to known risky destinations.
- **User or identity-aware policy:** Apply rules based on authenticated users or groups, where supported.
- **TLS inspection:** Optionally decrypt and inspect some encrypted sessions when explicitly configured and legally/operationally appropriate.

**Important limitations:** An NGFW does not magically see encrypted payloads. TLS inspection requires appropriate configuration, certificates, trust, privacy safeguards, and exceptions. Some traffic may be unavailable for decryption or deliberately excluded. Application identification can be imperfect, and inspection adds operational cost and potential latency.

### Three-way comparison

| Scenario | Stateless | Stateful | NGFW |
|---|---|---|---|
| Permit TCP 443 to a specified destination | Yes | Yes | Yes |
| Recognize return packets from an allowed flow | Not inherently | Yes | Typically yes |
| Identify a specific application using TCP 443 | Not inherently | Not inherently | Often, with appropriate visibility |
| Detect known exploit patterns | Not inherently | Not inherently | Often through integrated IPS capabilities |
| Read encrypted HTTPS contents by default | No | No | No; inspection must be enabled and feasible |

**Security+ exam clue:** “Block a prohibited application that uses the same port as allowed web traffic” suggests an **application-aware NGFW**, rather than simple port filtering.

## 5. Firewall placement: perimeter, internal, and endpoint

**Perimeter firewalls** filter traffic between an organization's network and external networks. **Internal segmentation firewalls** enforce rules between VLANs or security zones, helping limit lateral movement. **Host-based firewalls** run on individual endpoints or servers and can restrict inbound and outbound connections for that host.

A useful design combines these controls rather than relying on a single perimeter device.

```text
                    Internet
                       |
              [Perimeter firewall]
                       |
               [Internal switch]
                 /      |      \
          Employees   Finance   Guest
             VLAN      VLAN     VLAN
                \       |       /
              [Inter-zone policy]

Each laptop/server can ALSO run a host firewall.
```

This is a conceptual sketch. In a real network, inter-zone enforcement may be performed by the same firewall, a separate internal firewall, or supported Layer 3 policy devices. The actual packet path must pass through the intended enforcement point; a firewall cannot inspect traffic that bypasses it.

### Firewall versus WAF versus IDS/IPS

| Control | Primary job | Typical focus |
|---|---|---|
| Network firewall | Enforce network traffic policy | IPs, ports, zones, state; possibly applications |
| Host firewall | Enforce traffic policy on one endpoint | Local inbound/outbound services and programs |
| **WAF** (web application firewall) | Filter HTTP(S) requests to protect web applications | Web requests, application-layer attack patterns |
| **IDS** (intrusion detection system) | Detect suspicious activity and alert | Monitoring; typically does not block traffic by itself |
| **IPS** (intrusion prevention system) | Detect and actively prevent selected suspicious traffic | Inline blocking or prevention |

A WAF is not a universal replacement for a network firewall: protecting a website against malicious HTTP requests is different from controlling SSH or SMB access between internal VLANs. An NGFW may incorporate IPS functions, but the terms describe different capabilities.

## 6. Rule order: how a firewall evaluates traffic

Many firewall rule lists use **top-down, first-match evaluation**: the first applicable rule determines the action. **However, this is not universal.** Some platforms separate rules by interface, zone, chain, or processing stage; others have special or implicit rules. For Security+ scenarios, use the evaluation model stated in the question.

### Worked example: first-match, top-down

Assume the firewall evaluates the following rules **in the listed order**, stopping at the first match. All traffic shown is **newly initiated** (not preexisting stateful return traffic).

| Order | Source | Destination | Service | Action |
|---|---|---|---|---|
| 1 | Guest VLAN `192.168.40.0/24` | Finance VLAN `192.168.20.0/24` | Any | **Deny** |
| 2 | Guest VLAN `192.168.40.0/24` | Any | TCP 443 | **Allow** |
| 3 | Employees `192.168.10.0/24` | Printer `192.168.30.25` | TCP 9100 | **Allow** |
| 4 | Any | Any | Any | **Deny** |

**Case A:** Guest `192.168.40.12` → finance server `192.168.20.10`, TCP 443.  
Rule 1 matches and **denies**. Rule 2 is never reached, even though it also describes TCP 443.

**Case B:** Guest `192.168.40.12` → public server `203.0.113.10`, TCP 443.  
Rule 1 does not match (destination is not Finance). Rule 2 matches and **allows**.

**Case C:** Employee `192.168.10.25` → printer `192.168.30.25`, TCP 9100.  
Rules 1 and 2 do not match. Rule 3 **allows**.

**Case D:** Employee `192.168.10.25` → finance server `192.168.20.10`, TCP 22.  
No specific allow matches. Rule 4 **denies**.

### Shadowed rules and overly broad allows

Consider this problematic order:

| Order | Source | Destination | Service | Action |
|---|---|---|---|---|
| 1 | Any | Any | Any | Allow |
| 2 | Guest VLAN | Finance VLAN | Any | Deny |

Under **first-match** evaluation, rule 2 is **shadowed**: traffic that would match it already matches rule 1 and is allowed. The intended guest-to-finance protection does not work.

**Rule-order troubleshooting checklist:**

1. Identify the correct rule set, interface, direction, and zone.
2. Determine whether the flow is new or tracked return traffic.
3. Read the matching fields exactly: source, destination, protocol, and ports.
4. Follow the stated evaluation order, including any platform-specific stages.
5. Stop at the first match **if** that is the specified policy model.
6. Check the implicit/default action if no explicit rule matches.

<details markdown="1">
<summary>Practice tracing: what if the rules are reversed?</summary>

**Question:** A top-down first-match list contains `Allow Guest → Any TCP 443` above `Deny Guest → Finance Any`. What happens to a new Guest → Finance TCP 443 connection?

**Answer:** It is **allowed** by the first rule. The later deny cannot override a prior matching allow in this evaluation model. Fix the order or narrow the allow rule.

</details>

## 7. Default deny, least privilege, and rule hygiene

A sound starting principle is **deny by default, then permit specific business-required flows**. This is a policy principle, not a claim that every product ships with the same default configuration.

A good rule answers: **Who needs to connect to what, using which service, and why?** Compare:

- **Overly broad:** Allow `Guest VLAN → Internal networks`, any service.
- **Narrower:** Allow `Employee VLAN → Printer 192.168.30.25`, TCP 9100.
- **Narrower:** Allow `Admin workstation → Management interface`, TCP 443, with authentication and logging.

Other practices include:

- Document rule owner, business purpose, approval, and review date.
- Place more-specific exceptions appropriately for the platform's evaluation model.
- Remove obsolete rules and review temporary exceptions.
- Log meaningful denies and sensitive allows without collecting unnecessary data.
- Review changes for unintended exposure and test both permitted and prohibited flows.
- Restrict firewall management access and protect configuration backups.
- Remember that outbound filtering matters too: compromised hosts may try to contact external systems.

**Logging is evidence, not a guarantee of prevention.** A denied-connection log records a decision the firewall made; it does not by itself prove a successful intrusion or identify a malicious actor.

## 8. Guided lab: review small-office firewall rules

**Goal:** Apply the segmentation design from [Day 21](day-21-segmentation-and-vlans.md) to a firewall policy. This is a **paper exercise**; no production firewall changes are required.

Use these illustrative zones:

| Zone | Subnet | Intended purpose |
|---|---|---|
| Employees | `192.168.10.0/24` | Staff devices |
| Finance | `192.168.20.0/24` | Sensitive finance systems |
| Printers | `192.168.30.0/24` | Shared printers |
| Guest | `192.168.40.0/24` | Visitor internet access |
| Cameras | `192.168.50.0/24` | Camera devices |
| Management | `192.168.99.0/24` | Authorized administration |

### Proposed rules

Assume a first-match policy and that the firewall handles tracked return traffic for allowed flows.

| Order | Source | Destination | Service | Action | Purpose |
|---|---|---|---|---|---|
| 1 | Guest | Internal RFC1918 networks | Any | Deny | Prevent guest access to internal private networks |
| 2 | Guest | Internet destinations | DNS to approved resolver; HTTP/HTTPS | Allow | Basic guest browsing (resolver reachability must be designed) |
| 3 | Employees | Printer `192.168.30.25` | TCP 9100 | Allow | Printing |
| 4 | Authorized admin host in Management | Camera management interface | TCP 443 | Allow | Device administration |
| 5 | Any | Any | Any | Deny + log | Block other new flows |

**Important design note:** Rule 1's “Internal RFC1918 networks” is intentionally broad for a simple lab. RFC1918 private ranges also appear in some VPNs and upstream networks. Production policies should define actual internal destinations and required exceptions carefully. Likewise, the “Internet destinations” object and DNS resolver policy must be implemented precisely; these labels are design shorthand, **not copy-paste firewall syntax**.

### Questions

1. Can a guest initiate HTTPS to `192.168.20.10`? Which rule decides?
2. Can an employee print to `192.168.30.25:9100`?
3. Can a camera initiate SSH to a finance server?
4. Can an authorized management host use HTTPS to the camera interface?
5. What additional explicit rule would you consider if employees need DNS resolution through an internal resolver?

<details markdown="1">
<summary>Reveal lab answers and explanations</summary>

1. **No.** Rule 1 denies guest traffic to internal private destinations before the guest internet allow is considered.
2. **Yes.** Rule 3 permits the specified employee-to-printer TCP 9100 flow.
3. **No.** No allow rule covers camera-to-finance SSH; rule 5 denies it.
4. **Yes.** Rule 4 permits the specifically authorized management source to use TCP 443 to the camera management interface.
5. **Permit the required clients to the approved internal DNS resolver on UDP/TCP 53**, with the source, destination, and direction specified. Place the rule before a broader matching deny when the platform uses first-match processing. Avoid allowing arbitrary guest access to all internal hosts just to support DNS.

</details>

### Optional safe inspection on your own computer

On macOS, you can view whether the built-in application firewall is enabled with the following **read-only** command:

```bash
/usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate
```

This queries the macOS application firewall, **not** the complete set of packet-filter (`pf`) rules and not your network perimeter firewall. Do not infer from this single output that every inbound or outbound connection is protected.

On a Linux system that uses UFW, the following **read-only** command may show status and rules:

```bash
sudo ufw status verbose
```

UFW is not installed or enabled on every Linux distribution. Do not run commands that alter rules on a shared, remote, or production system without authorization and a recovery plan.

## 9. Common misconceptions and Security+ exam traps

| Misconception | More accurate explanation |
|---|---|
| “A VLAN blocks all traffic to other VLANs.” | VLANs separate Layer 2 domains; Layer 3 routing and policy determine cross-VLAN reachability. |
| “Stateful means deep inspection of every application.” | Stateful means tracking connection/flow state; application inspection is a separate capability. |
| “Port 443 always means safe web browsing.” | Applications and malicious traffic can use TCP 443; port alone does not prove content or intent. |
| “The last matching rule wins.” | Many policies are first-match; always use the platform or question's stated evaluation semantics. |
| “A deny below an allow still protects the destination.” | A prior broad allow may shadow the deny in a first-match rule list. |
| “An NGFW can automatically read all TLS traffic.” | Decryption requires supported, appropriately configured inspection and has limitations. |
| “A firewall replaces endpoint security.” | Host hardening, identity controls, patching, and monitoring remain necessary. |
| “IDS and IPS are the same.” | IDS detects/alerts; IPS can actively prevent selected traffic. |

## 10. Key terminology and takeaways

- **Firewall:** Enforcement point for traffic-control policy.
- **Stateless filtering:** Evaluates packets without maintaining conversation state.
- **Stateful inspection:** Tracks connections/flows and can recognize matching return traffic.
- **NGFW:** Adds capabilities such as application awareness and integrated threat prevention, depending on product and configuration.
- **Rule order:** Sequence of policy evaluation; can change the outcome under first-match processing.
- **Implicit deny / default deny:** Block traffic not explicitly permitted, where configured or implemented by the platform.
- **Shadowed rule:** A rule that cannot affect matching traffic because an earlier rule already handles it.
- **WAF:** Specialized control for HTTP(S) web application traffic.
- **IDS / IPS:** Detection versus prevention of suspicious activity.
- **Least privilege:** Allow only the communications required for a legitimate purpose.

**Main takeaway:** A secure network does not merely have a firewall; it has **intentional, tested rules** at meaningful trust boundaries. To answer Security+ rule-evaluation questions, identify the flow, determine the firewall's evaluation model, and trace the rule matches in order.

## 11. Security+ practice questions

**Question 1.** An administrator needs a device that recognizes return packets as part of an allowed outbound connection without broadly permitting unsolicited inbound connections. Which firewall type best fits?

A. Stateless packet filter  
B. Stateful firewall  
C. Passive network tap  
D. Unmanaged switch

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B. Stateful firewall.** Stateful inspection tracks permitted flows and can recognize associated return traffic. A stateless filter evaluates packets independently; a tap observes traffic, and an unmanaged switch is not a connection-aware firewall.

</details>

**Question 2.** A company permits web browsing but needs to block a specific prohibited application that uses TCP 443. Which capability is MOST relevant?

A. IP subnetting  
B. Simple destination-port filtering  
C. NGFW application identification  
D. VLAN tagging

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C. NGFW application identification.** A port-only rule cannot reliably distinguish applications sharing TCP 443. An NGFW may identify and control applications when it has sufficient visibility.

</details>

**Question 3.** A firewall uses top-down, first-match evaluation. Rule 1 allows `Guest → Any, TCP 443`; rule 2 denies `Guest → Finance, Any`. What happens to a new guest HTTPS connection to a finance server?

A. It is denied by rule 2 because deny always overrides allow  
B. It is allowed by rule 1 because that rule matches first  
C. It is denied only if the server uses a private IP address  
D. It is allowed only if an IDS approves it

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B. It is allowed by rule 1 because that rule matches first.** The broader allow shadows the later deny for TCP 443 traffic. Rule ordering matters under the explicitly stated first-match model.

</details>

**Question 4.** Which technology is designed primarily to filter malicious HTTP requests targeting a public web application?

A. Web application firewall  
B. Layer 2 access switch  
C. DHCP relay  
D. Network time server

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A. Web application firewall.** A WAF focuses on HTTP(S) requests and web application attacks. The other options do not provide this specialized application-layer filtering.

</details>

**Question 5.** A firewall policy should permit employees to print to one network printer while preventing access to other devices in the printer VLAN. Which rule best follows least privilege?

A. Allow Employees → Any, any protocol  
B. Allow Any → Printer VLAN, any protocol  
C. Allow Employees → Printer VLAN, all TCP ports  
D. Allow Employees → `192.168.30.25`, TCP 9100

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D. Allow Employees → `192.168.30.25`, TCP 9100.** This narrows both destination and service to the stated business need. Actual printing environments may use different ports or protocols, which should be identified before implementing the rule.

</details>

## 12. Further reading

- [CompTIA Security+ certification and exam information](https://www.comptia.org/certifications/security)
- [NIST SP 800-41 Rev. 1 — Guidelines on Firewalls and Firewall Policy](https://csrc.nist.gov/pubs/sp/800/41/r1/final) — foundational firewall-policy concepts; some implementation examples are dated.
- [NIST SP 800-207 — Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final) — trust boundaries, least privilege, and policy enforcement.
- [RFC 1918 — Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918) — private IPv4 address ranges used in the lab.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
