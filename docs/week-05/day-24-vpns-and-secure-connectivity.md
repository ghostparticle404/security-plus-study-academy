# Day 24 — VPNs and Secure Connectivity

**Week 05: Network Security**  
**Canonical path:** `docs/week-05/day-24-vpns-and-secure-connectivity.md`  
**Prerequisites:** [Day 18 — Ports and Protocols](../week-04/day-18-ports-and-protocols.md), [Day 22 — Firewalls and Filtering](day-22-firewalls-and-filtering.md). Recommended review: [Day 21 — Segmentation and VLANs](day-21-segmentation-and-vlans.md) and [Day 23 — IDS, IPS, and Network Monitoring](day-23-ids-ips-and-network-monitoring.md).  
**Security+ SY0-701 alignment:** Domain 3.0, **Security Architecture**, especially **Objective 3.2** (security principles applied to enterprise infrastructure, including VPNs and secure access). Related: **Objective 2.4** (security solutions using cryptographic technologies) and **Objective 4.1** (common security techniques applied to computing resources). These are topical alignments, not quotations of the official exam objectives.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain what a **virtual private network (VPN)** does and distinguish encryption, tunneling, authentication, and authorization.
2. Compare **remote-access** and **site-to-site** VPNs, including the devices and trust boundaries involved.
3. Explain the roles of **IPsec**, **IKEv2**, **ESP**, **TLS-based VPNs**, and **WireGuard** without confusing protocols with products.
4. Distinguish **full-tunnel** and **split-tunnel** remote access and evaluate their security and performance tradeoffs.
5. Diagram a remote worker's secure connection to a segmented office and select appropriate firewall and identity controls.
6. Interpret simple VPN connection logs and choose a secure connectivity approach for realistic Security+ scenarios.

## Why secure connectivity matters

A small office has six VLANs: employees, finance, printers, guests, cameras, and management. Its internal firewall rules restrict which networks can communicate. But what happens when a finance employee works from a hotel, or when the office opens a second location?

Sending sensitive traffic directly across an untrusted network without suitable protections creates opportunities for interception, tampering, and impersonation. A **VPN** creates a protected logical connection across an underlying network, commonly the public internet. It can allow remote employees to access approved internal resources or connect networks at different sites.

A VPN does **not** make an endpoint trustworthy, guarantee anonymity, or automatically authorize access to every internal system. The design still needs identity verification, least privilege, endpoint security, and monitoring.

> **Security+ decision point:** Ask four separate questions: *Who is connecting? What protects the traffic? Which destinations are permitted? What evidence shows the connection is behaving normally?*

## 1. Four building blocks: tunneling, encryption, authentication, authorization

These concepts are related, but they solve different problems.

| Concept | Question answered | Example |
| --- | --- | --- |
| **Tunneling** | How is one type of traffic carried across another network? | Encapsulating an internal IP packet for delivery over the internet |
| **Encryption** | Can someone who intercepts the traffic read its protected contents? | Encrypting packets inside an IPsec tunnel |
| **Authentication** | Is the connecting user, device, or peer who it claims to be? | Certificate validation, user sign-in, MFA |
| **Authorization** | Which resources may this authenticated identity access? | Allowing a finance user to reach the finance application, not the management VLAN |

**Encapsulation** adds information around an original packet so it can travel through the intervening network. Encryption protects data confidentiality when correctly implemented; integrity checks help detect unauthorized changes. Authentication and authorization determine who may establish the connection and what they may do after connecting.

### Visualizing an encapsulated packet

A simplified conceptual example of an IPsec tunnel:

```text
Original internal packet:
[IP: employee laptop -> finance server][Application data]

Protected for transit across the internet:
[Outer IP: VPN client -> VPN gateway]
[IPsec protection: original internal packet + integrity/authentication data]

VPN gateway validates/decrypts as appropriate, then forwards
only if the internal security policy permits the traffic.
```

This diagram is intentionally simplified. Exact packet fields differ by protocol and mode. A tunnel protects traffic **between its endpoints**; once traffic leaves the tunnel endpoint, other protections and policies still matter.

## 2. Remote-access VPN versus site-to-site VPN

### Remote-access VPN: one user or device connects

A **remote-access VPN** connects a remote endpoint to a private network or selected private applications. Typically, the client runs VPN software and authenticates to a VPN gateway or concentrator.

```text
Remote employee laptop
        |
  Hotel / home Wi-Fi
        |
   Public internet
        |
   Encrypted VPN tunnel
        |
[Office VPN gateway] -- [Internal firewall] -- [Finance app]
                              |
                              +-- [Other VLANs: denied unless approved]
```

A successful VPN sign-in is **not** permission to access the entire office network. The gateway and internal firewall should enforce specific user, device, network, and application policies.

### Site-to-site VPN: networks connect

A **site-to-site VPN** links networks, commonly using gateways at each site. End users often do not need to start a VPN client themselves.

```text
Office A LAN -- [Gateway A] ==== encrypted tunnel ==== [Gateway B] -- Office B LAN
                                 public internet
```

The gateways negotiate security associations and protect traffic selected by policy or routing. Administrators must plan **nonoverlapping address ranges**, routing, firewall rules, key management, and failover. Overlapping private IP subnets can complicate routing across sites.

| Feature | Remote access | Site to site |
| --- | --- | --- |
| Typical endpoints | User device and gateway | Two network gateways |
| Primary purpose | Give a remote user approved access | Connect networks or sites |
| Authentication focus | User and device, plus gateway | Gateway/peer identities; possibly additional controls |
| Common challenge | Lost devices, MFA, endpoint posture | Routing, overlapping subnets, tunnel availability |
| Typical scenario | Accountant working from home | Headquarters connecting to branch office |

**Exam trap:** A site-to-site VPN does not necessarily encrypt every packet generated by either office. The traffic protected depends on the VPN configuration and routing/policy selectors.

## 3. IPsec: protecting IP traffic

**Internet Protocol Security (IPsec)** is a suite of standards that can provide confidentiality, integrity, and peer authentication for IP traffic. It is frequently used for site-to-site VPNs and can also support remote access.

### ESP, AH, and IKEv2: different jobs

| Term | Function | Important distinction |
| --- | --- | --- |
| **ESP (Encapsulating Security Payload)** | Protects IP traffic; commonly supplies encryption and integrity/authentication | Commonly used for IPsec VPN data traffic |
| **AH (Authentication Header)** | Provides integrity and data-origin authentication for selected packet fields; **no encryption** | Rare in typical modern VPN deployments; NAT compatibility is problematic |
| **IKEv2 (Internet Key Exchange v2)** | Authenticates peers, negotiates cryptographic parameters, and establishes/manages IPsec security associations | Control-plane negotiation, not the payload-encryption protocol itself |
| **Security association (SA)** | Agreed parameters governing protected traffic | Includes keys/algorithms and related state |

**IKEv2** is often paired with IPsec ESP. The negotiation establishes protected communication parameters; ESP then protects actual user traffic.

### IPsec tunnel mode versus transport mode

- **Tunnel mode:** Protects an entire original IP packet and carries it inside a new outer IP packet. Common for gateway-to-gateway VPNs and many remote-access designs.
- **Transport mode:** Primarily protects the IP payload while retaining the original IP header for routing. Used in some host-to-host arrangements.

Do not confuse **IPsec tunnel mode** with the broader idea of a VPN tunnel. Different VPN technologies have different encapsulation designs.

### IPsec ports and protocols

| Item | Typical use |
| --- | --- |
| UDP **500** | IKE negotiation |
| UDP **4500** | IPsec NAT traversal (NAT-T) |
| IP protocol **50** | ESP; **not TCP/UDP port 50** |
| IP protocol **51** | AH; **not TCP/UDP port 51** |

Network address translation (**NAT**) changes addressing information as packets pass a gateway. **NAT-T** can encapsulate ESP traffic in UDP 4500 to help it traverse NAT devices. Do not assume that opening TCP port 50 permits ESP; ESP is a separate IP protocol number.

## 4. TLS-based VPNs and other secure transport choices

**TLS (Transport Layer Security)** is widely used to protect application connections such as HTTPS. VPN products may also use TLS to establish protected remote-access sessions. These are often called **SSL VPNs**, but modern secure deployments should use current TLS versions rather than obsolete SSL protocols.

A TLS-based remote-access solution may provide:

- **Client-based access:** Software establishes a protected tunnel or application session.
- **Clientless or browser-based access:** A web portal exposes selected applications, usually without granting arbitrary access to the entire internal network.

Actual capabilities vary by implementation. A browser portal is not equivalent to unrestricted network-layer access.

### WireGuard

**WireGuard** is a modern VPN protocol and software design using a streamlined cryptographic construction and public-key-based peer configuration. It commonly uses **UDP**, with **51820** as a frequent default listening port, although deployments can choose another port. Its lightweight design does not eliminate the need for identity provisioning, access policy, endpoint hardening, or key rotation procedures.

### Comparing the approaches

| Approach | Common deployment | Strength or design consideration | Caution |
| --- | --- | --- | --- |
| IPsec with IKEv2 | Site-to-site and remote access | Network-layer protection and established interoperability | Negotiation, NAT, routing, and policy complexity |
| TLS-based VPN | Remote users and application portals | Can provide application-specific access or client-based connectivity | Capabilities depend on product and configuration |
| WireGuard | Remote access and site-to-site | Small protocol design and efficient implementation | Key distribution and authorization require deliberate management |
| HTTPS/TLS to an application | Individual web applications | Direct end-to-end application transport protection | Does not itself create a general-purpose VPN |
| SSH | Secure remote administration | Encrypted, authenticated administrative session | Not a general substitute for a network VPN |

**Security+ distinction:** The exam may ask for a **secure remote network connection**, **secure website access**, or **secure administrator access**. Those are different requirements; choose the technology that matches the use case.

## 5. Full tunnel versus split tunnel

A remote-access VPN client needs a **routing policy** determining which traffic enters the VPN.

### Full-tunnel configuration

```text
Laptop -> VPN gateway -> office security controls -> internet
                     \-> permitted internal resources
```

In a typical full-tunnel design, most or all internet-bound traffic is routed through the corporate VPN. This may improve centralized inspection, logging, and policy enforcement, but it adds bandwidth demand and may increase latency. Local-network exceptions and implementation details vary.

### Split-tunnel configuration

```text
Laptop -> VPN gateway -> approved internal applications
       \-> local internet connection -> public websites
```

Split tunneling sends selected corporate destinations through the VPN while other traffic uses the device's ordinary internet route. It can reduce backhaul and improve performance, but internet-bound traffic may bypass corporate network controls unless other controls (such as endpoint filtering and secure web gateways) apply.

| Question | Full tunnel | Split tunnel |
| --- | --- | --- |
| Where does ordinary web traffic usually go? | Through corporate gateway | Directly through local internet |
| Centralized network inspection | Easier for routed traffic | Less comprehensive without additional controls |
| Corporate VPN bandwidth demand | Higher | Often lower |
| Typical tradeoff | Central control versus performance | Performance versus visibility/policy consistency |

**Important:** Neither option alone guarantees security. Endpoint protections, DNS behavior, IPv6 routing, local LAN access, and leak prevention must match the intended policy.

## 6. VPNs, Zero Trust, and least privilege

A traditional VPN may provide broad **network reachability** once a connection is established. **Zero Trust** approaches avoid treating network location as sufficient evidence of trust. Access decisions can consider user identity, device posture, application, context, and ongoing policy enforcement.

**Zero Trust Network Access (ZTNA)** commonly grants access to specific applications rather than exposing a large private subnet. ZTNA and VPNs are not automatically mutually exclusive: organizations may combine them or transition gradually.

A sound remote-access design can include:

1. **MFA** for user authentication, preferably phishing-resistant methods where feasible.
2. **Device checks** for supported operating systems, encryption, patching, and endpoint security.
3. **Least-privilege access** based on role and actual business need.
4. **Internal segmentation** so remote users cannot reach guest, camera, or management networks without authorization.
5. **Logging and monitoring** of connection attempts, session changes, unusual locations, and denied flows.
6. **Rapid revocation** when a device is lost or a user leaves the organization.

### Certificates, shared secrets, and credentials

Site-to-site peers may authenticate with **pre-shared keys (PSKs)** or certificates, depending on the protocol and configuration. Certificates support scalable identity management but require a functioning **public key infrastructure (PKI)** and certificate lifecycle practices. Human user passwords alone are generally not sufficient protection for high-value remote access; MFA is a critical additional control.

**Exam trap:** Encryption protects traffic in transit, but it does not repair an infected laptop or prevent an authorized user from accessing data outside their role.

## 7. Guided design: a remote-access VPN for our six-VLAN office

Recall our example office network from Day 21:

| VLAN | Example subnet | Purpose |
| --- | --- | --- |
| 10 | `192.168.10.0/24` | Employee workstations |
| 20 | `192.168.20.0/24` | Finance systems |
| 30 | `192.168.30.0/24` | Printers |
| 40 | `192.168.40.0/24` | Guest devices |
| 50 | `192.168.50.0/24` | Cameras/IoT |
| 99 | `192.168.99.0/24` | Network management |

*These are fictional lab addresses, not a recommended universal addressing plan.*

### Scenario

An accountant must access a finance application at `192.168.20.25` while working from home. The company wants remote access without exposing the finance server directly to the internet.

**Proposed architecture:**

```text
Accountant laptop (home)
       |
   Internet
       |
[VPN gateway: MFA + device validation]
       |
[Remote-access address pool: 10.250.24.0/24]
       |
[Internal firewall / policy enforcement]
       |                    |                    |
       v                    X                    X
Finance app:443       Management VLAN       Camera VLAN
192.168.20.25         192.168.99.0/24       192.168.50.0/24
```

The remote-access pool is a separate example network, not an additional office VLAN. In a real deployment, routing and firewall policy must recognize it.

### Example policy matrix

| Source | Destination | Service | Decision | Reason |
| --- | --- | --- | --- | --- |
| Authenticated finance VPN users | `192.168.20.25` | TCP 443 | Allow | Approved finance application |
| Authenticated finance VPN users | `192.168.99.0/24` | Any | Deny | Management isolation |
| Authenticated finance VPN users | `192.168.50.0/24` | Any | Deny | Camera/IoT isolation |
| Other remote-access identities | `192.168.20.25` | Any | Deny | Finance role restriction |
| Any unauthorized remote flow | Other internal destinations | Any | Deny by default | Least privilege |

These rules are **conceptual**. Actual products may evaluate user identity, group membership, destination objects, and state in different ways. Order matters on platforms using first-match policies (Day 22).

### Exercise: draw the connection

On paper or in a diagramming tool:

1. Draw the remote laptop, home router, public internet, VPN gateway, internal firewall, and finance server.
2. Mark the segment protected by the VPN tunnel.
3. Label where MFA occurs and where authorization rules are enforced.
4. Draw an **allowed** path to the finance application and a **denied** path to the management VLAN.
5. Decide whether full or split tunneling fits the organization's monitoring and bandwidth requirements.
6. Name **two logs** you would inspect if the user reports a failed connection.

<details markdown="1">
<summary>Reveal example diagram analysis</summary>

The VPN tunnel should extend from the remote VPN client to the VPN gateway. Authentication occurs as part of establishing approved remote access, while authorization must also restrict traffic after connection. The finance application is reachable only over its permitted service (for example, HTTPS on TCP 443). The management VLAN remains blocked. Relevant evidence includes VPN authentication/session logs and firewall allow/deny logs. Full tunneling is appropriate when centralized web inspection is a requirement; split tunneling may be appropriate when direct internet access is acceptable and endpoint protections maintain policy.

</details>

## 8. Reading VPN logs: authentication versus connectivity

A VPN connection has multiple stages. A user can successfully authenticate yet still fail to reach an application because of routing, DNS, authorization, or firewall rules.

Consider these **fictional** log entries:

```text
09:02:11 VPN AUTH user=ana result=SUCCESS mfa=PASS device=COMPLIANT
09:02:12 VPN SESSION user=ana assigned_ip=10.250.24.18 status=UP
09:02:18 FIREWALL src=10.250.24.18 dst=192.168.20.25 proto=TCP dport=443 action=ALLOW
09:03:04 FIREWALL src=10.250.24.18 dst=192.168.99.10 proto=TCP dport=22 action=DENY
```

**Interpretation:** Ana authenticated, received a VPN address, and established a session. The finance application flow was allowed. A later attempt to reach a management host over SSH was blocked. The deny event deserves context, but does not by itself prove malicious intent.

### Common troubleshooting sequence

1. **Authentication:** Was the account accepted? Did MFA succeed? Was the certificate valid?
2. **Tunnel establishment:** Did the client negotiate and establish the session?
3. **Addressing and routing:** Did the client receive an address and routes to the intended resource?
4. **DNS:** Does the application hostname resolve correctly through the intended DNS resolver?
5. **Firewall policy:** Is the source identity/address allowed to reach the destination and port?
6. **Application health:** Is the target service actually listening and available?

<details markdown="1">
<summary>Reveal: what if authentication succeeds but the app fails?</summary>

Do **not** immediately reset the user's password. A successful authentication log indicates that the identity stage worked. Check the assigned VPN address, route table, DNS resolution, internal firewall denies, and application availability. Troubleshoot the failed stage rather than treating all failures as credential problems.

</details>

## 9. Safe technical demonstrations

These are **read-only observations** on a computer you own or are authorized to administer. They do not establish or attack a VPN.

### Inspect routing on macOS

```bash
netstat -rn
```

Review the destination, gateway, and interface columns. If a VPN is connected, a tunnel interface may appear; actual interface names and routes depend on the client.

### Inspect routing on Linux

```bash
ip route show
```

For IPv6 routes, use:

```bash
ip -6 route show
```

### Inspect network interfaces on macOS

```bash
ifconfig
```

Look for active interfaces and their addresses. Do not assume every `utun` interface represents the specific VPN you are studying; operating systems may create tunnel interfaces for other purposes.

### Inspect a connection to an approved HTTPS service

```bash
curl -I https://example.com
```

This requests HTTP response headers from a public example site. It does **not** prove that traffic used a VPN or that your device is anonymous. Confirm the route and VPN client status separately.

### A security caution about tests

Avoid disabling your firewall or VPN protections just to compare behavior. Use approved test systems, a lab environment, or existing logs when investigating routing and tunnel policy.

## 10. Common misconceptions and Security+ exam traps

| Misconception | Correct interpretation |
| --- | --- |
| “A VPN makes every network safe.” | It protects traffic between tunnel endpoints; compromised endpoints and weak access policies remain risks. |
| “VPN means anonymity.” | VPN operators and destination services may still observe identifying information; anonymity is not guaranteed. |
| “A VPN encrypts traffic forever.” | VPN protection ends at the relevant tunnel endpoint; subsequent hops require their own protections. |
| “IKEv2 encrypts the user data.” | IKEv2 negotiates/authenticates IPsec associations; ESP commonly protects data traffic. |
| “ESP is port 50.” | ESP uses IP protocol number 50, not TCP/UDP port 50. |
| “Full tunnel is always best.” | It can improve centralized visibility, but has performance and infrastructure costs. |
| “Split tunnel is automatically insecure.” | It reduces some centralized visibility; actual risk depends on endpoint and network controls. |
| “Successful VPN login means access to everything.” | Authentication and authorization are distinct; segmentation and least privilege still apply. |
| “A TLS VPN uses obsolete SSL.” | “SSL VPN” is a legacy category name; modern deployments should use secure TLS configurations. |
| “VPN and HTTPS do the same job.” | A VPN protects a selected network path; HTTPS protects an application connection. They can be layered. |

## 11. Key terminology and takeaways

| Term | Remember |
| --- | --- |
| **VPN** | Protected logical connection over another network |
| **Tunnel** | Encapsulated path between endpoints |
| **Remote-access VPN** | Individual remote device to private resources |
| **Site-to-site VPN** | Network gateway to network gateway |
| **IPsec** | Suite protecting IP traffic |
| **IKEv2** | Negotiates/authenticates IPsec security associations |
| **ESP** | IPsec payload protection, IP protocol 50 |
| **NAT-T** | Encapsulates ESP in UDP, commonly port 4500 |
| **TLS VPN** | Remote access using TLS-based protected sessions |
| **WireGuard** | Modern VPN protocol using public-key peer configuration |
| **Full tunnel** | Usually routes general internet traffic through the corporate VPN |
| **Split tunnel** | Routes selected traffic through VPN, other traffic locally |
| **ZTNA** | Application-specific access guided by identity and policy |
| **MFA** | More than one authentication factor |

**Takeaway:** Secure connectivity is a design decision, not simply a checkbox labeled “VPN enabled.” Match the connection method to the resource, authenticate the peer, authorize only necessary access, protect traffic in transit, and retain enough telemetry to investigate failures or abuse.

## 12. Security+ practice questions

**Question 1.** A company wants to connect its headquarters LAN to a branch-office LAN over the internet without requiring every employee to launch a VPN client. Which design is the BEST fit?

A. Clientless browser-based VPN for every employee
B. Site-to-site VPN between the office gateways
C. SSH session from each employee to headquarters
D. Publicly expose internal file-sharing services

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** A site-to-site VPN connects networks through gateways and can protect traffic without individual users manually launching VPN clients. **A** grants application-oriented access, **C** is for secure remote administration, and **D** unnecessarily exposes services.

</details>

**Question 2.** An administrator is reviewing IPsec firewall requirements. Which statement is accurate?

A. ESP always uses TCP port 50
B. IKEv2 is the protocol that carries every encrypted application packet
C. UDP 4500 is commonly used for IPsec NAT traversal
D. AH provides confidentiality through encryption

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** IPsec NAT-T commonly uses UDP 4500. **A** confuses an IP protocol number with a transport port. **B** confuses IKEv2 negotiation with ESP data protection. **D** is incorrect because AH does not encrypt payloads.

</details>

**Question 3.** Remote workers need corporate application access, but the organization wants ordinary internet browsing to pass through its central web inspection gateway. Which approach most directly meets that routing requirement?

A. Full-tunnel remote-access VPN
B. Split-tunnel VPN with direct local internet breakout
C. Unencrypted HTTP access to corporate applications
D. A site-to-site VPN between two branch routers

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** A full-tunnel design generally routes internet-bound traffic through corporate infrastructure, enabling central inspection of that traffic. **B** usually bypasses the central gateway for ordinary browsing. **C** removes transport protection, and **D** does not describe the individual remote-user requirement.

</details>

**Question 4.** A finance employee successfully authenticates to a VPN with MFA but cannot connect to the finance web application. VPN logs show an active session. What should the analyst check NEXT?

A. Assume the password is wrong and reset it
B. Disable MFA until the application works
C. Give the employee unrestricted access to all VLANs
D. Check assigned routes, DNS, and firewall policy for the application destination

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** Successful authentication and an active tunnel narrow the problem to connectivity, name resolution, authorization, or application availability. **A**, **B**, and **C** do not address the likely failed stage and could weaken security.

</details>

**Question 5.** A remote-access solution allows an authenticated contractor to open one approved internal web application but does not expose the broader private subnet. Which security principle is MOST clearly demonstrated?

A. Implicit trust of all VPN users
B. Unrestricted lateral movement
C. Least-privilege, application-specific access
D. Elimination of all endpoint security requirements

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Restricting the contractor to one necessary application demonstrates least privilege and resembles an application-focused ZTNA design. **A** and **B** contradict the restriction; **D** is false because endpoint security remains necessary.

</details>

## 13. Further reading

- [CompTIA Security+ certification and exam information](https://www.comptia.org/certifications/security) — consult the official SY0-701 exam objectives for the authoritative wording.
- [NIST SP 800-77 Rev. 1 — Guide to IPsec VPNs](https://csrc.nist.gov/pubs/sp/800/77/r1/final) — IPsec architecture and deployment considerations.
- [NIST SP 800-46 Rev. 2 — Guide to Enterprise Telework, Remote Access, and BYOD Security](https://csrc.nist.gov/pubs/sp/800/46/r2/final) — remote-access risk and controls (older guidance; assess currency before implementation).
- [NIST SP 800-207 — Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final) — identity- and resource-oriented access design.
- [RFC 7296 — Internet Key Exchange Protocol Version 2 (IKEv2)](https://www.rfc-editor.org/rfc/rfc7296) — IKEv2 protocol specification.
- [RFC 4303 — IP Encapsulating Security Payload (ESP)](https://www.rfc-editor.org/rfc/rfc4303) — ESP specification.
- [WireGuard documentation](https://www.wireguard.com/) — protocol overview and implementation background.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
