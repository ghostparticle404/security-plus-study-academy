# Day 25 — Wireless and Network Access Control

**Week 05: Network Security**  
**Canonical path:** `docs/week-05/day-25-wireless-and-network-access-control.md`  
**Prerequisites:** [Day 21 — Segmentation and VLANs](day-21-segmentation-and-vlans.md), [Day 22 — Firewalls and Filtering](day-22-firewalls-and-filtering.md), [Day 23 — IDS, IPS, and Network Monitoring](day-23-ids-ips-and-network-monitoring.md), and [Day 24 — VPNs and Secure Connectivity](day-24-vpns-and-secure-connectivity.md).  
**Security+ SY0-701 alignment:** Domain **3.0 Security Architecture**, particularly **Objective 3.2** (security principles applied to enterprise infrastructure, including wireless and network access controls); related **Objective 4.1** (common security techniques applied to computing resources). These are topical mappings; consult CompTIA's official objectives for authoritative wording.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain wireless authentication, encryption, association, and network authorization as separate steps.
2. Compare WPA2-Personal, WPA3-Personal, WPA2/WPA3-Enterprise, and open or captive-portal guest networks.
3. Describe how **802.1X**, **EAP**, **RADIUS**, and **NAC** work together to grant or restrict access.
4. Recognize wireless threats including evil twins, rogue access points, deauthentication attacks, and weak configuration.
5. Design guest wireless access that is isolated from employee, finance, camera, and management networks.
6. Interpret simple wireless/NAC events and select proportionate corrective controls in Security+ scenarios.

## Why wireless and network access control matter

In the small office from [Day 21](day-21-segmentation-and-vlans.md), we separated employees, finance, printers, guests, cameras, and management into VLANs. That separation only helps if devices are placed in the **right** VLAN and traffic between VLANs is controlled. Wireless access creates another entry point: someone in a waiting room—or outside the building—may be within radio range of an access point (AP).

Imagine a visitor who asks for Wi-Fi. The visitor needs internet access, not access to payroll files, security cameras, or the switch-management interface. A secure design answers several questions: **Which wireless network can the visitor join? How is the connection protected? How do we determine which network the device belongs to? What traffic is allowed after connection?**

A password on the Wi-Fi network is not a substitute for a firewall, and a guest SSID is not automatically isolated. Good wireless security combines identity, encryption, segmentation, monitoring, and sensible access policy.

> **Security+ decision point:** Separate **joining the wireless network** from **being permitted to reach a resource**. Authentication and encryption protect access to the wireless link; VLANs, ACLs, firewalls, and NAC policies determine the next trust boundary.

## 1. Wireless fundamentals: radios, APs, SSIDs, and clients

A **wireless local area network (WLAN)** connects devices using radio instead of an Ethernet cable. An **access point (AP)** bridges wireless clients into a wired or otherwise upstream network. A **station (STA)** is a wireless client, such as a laptop or phone.

An **SSID** (service set identifier) is the human-readable network name, such as `Office-Staff` or `Office-Guest`. A **BSSID** commonly identifies a particular AP radio/interface with a MAC address. Several APs can advertise the same SSID to support roaming; that does not make them one physical radio.

| Term | Meaning | Security implication |
| --- | --- | --- |
| SSID | Name of a wireless network | A name alone proves nothing about who operates the network |
| BSSID | Identifier for a basic service set, commonly an AP radio MAC address | Useful for identifying which AP a client reached, but MAC addresses can be spoofed |
| AP | Device providing wireless connectivity | Must be configured, patched, and managed securely |
| Association | Client establishes a wireless relationship with an AP | Not equivalent to authorization to access internal systems |
| WLAN controller / cloud management | Centralizes AP configuration and monitoring | Protect administrator credentials and management plane |
| RF coverage | Area in which wireless signals can be received | Wireless boundaries do not end neatly at office walls |

### The connection sequence, simplified

```text
Client sees Office-Staff SSID
          |
          v
Selects AP and begins Wi-Fi authentication/association
          |
          v
Establishes link protection (e.g., WPA3 handshake)
          |
          v
Receives appropriate network configuration (e.g., DHCP)
          |
          v
Firewall/NAC policy governs allowed destinations
```

This is a conceptual overview, not a complete frame-by-frame exchange. In an enterprise 802.1X deployment, EAP authentication and key establishment occur as part of the controlled connection process; a client should not gain unrestricted data access before authorization.

### Frequency bands are not security controls

Wi-Fi operates across bands including 2.4 GHz, 5 GHz, and (on supported Wi-Fi 6E/7 equipment) 6 GHz. Band choice affects range, interference, channel availability, and performance; it does **not** by itself establish confidentiality or access rights. Avoid treating a different frequency or a hidden SSID as a substitute for proper authentication.

## 2. Wireless security modes: what actually protects the link?

Wireless security includes **authentication** (establishing credentials or identity) and **link-layer encryption** (protecting wireless frames over the radio connection). These are not interchangeable.

| Mode | Typical authentication | Main use | Key concern |
| --- | --- | --- | --- |
| Open Wi-Fi | No Wi-Fi password | Public hotspots | Without other protections, radio traffic may lack link-layer confidentiality |
| WPA2-Personal | Shared passphrase (PSK) | Small networks and legacy compatibility | Shared secret; weak passwords can enable offline guessing after capturing suitable handshake material |
| WPA3-Personal | SAE (Simultaneous Authentication of Equals) | Modern small-office/home networks | Client/AP compatibility and secure configuration still matter |
| WPA2-Enterprise | 802.1X/EAP with a backend authentication service | Managed organizations | Correct EAP method and server certificate validation are essential |
| WPA3-Enterprise | Enterprise authentication with stronger modern requirements | Managed organizations with capable clients | Deployment must match client and infrastructure support |
| Enhanced Open (OWE) | No user credential; opportunistic wireless encryption | Public/guest access where supported | Encrypts radio traffic but does not authenticate the AP as an organization or user |

**WPA** means Wi-Fi Protected Access. **WPA2** commonly uses AES-CCMP; **WPA3-Personal** replaces the traditional WPA2-Personal PSK handshake with **SAE**. WPA3 also requires **Protected Management Frames (PMF)** for certified WPA3 connections. Do not say WPA3 makes a wireless network immune to phishing, malicious APs, compromised endpoints, or all denial-of-service attacks.

### WPA2-Personal versus WPA3-Personal

With WPA2-Personal, all users of an SSID commonly share a passphrase. A shared password is operationally simple but difficult to revoke for one departing user without changing it for everyone. WPA3-Personal's SAE offers improved resistance to offline dictionary attacks against captured exchanges, but administrators should still choose strong credentials and update AP firmware.

**Transition mode** may permit WPA2 and WPA3 clients on the same SSID during migration. It can be useful for compatibility, but it does not guarantee every connected client receives WPA3 protections. Review which clients negotiate which mode.

### What about WEP and WPS?

**WEP** is obsolete and insecure; it should not be deployed. **WPS** (Wi-Fi Protected Setup), especially PIN-based enrollment, has a history of security weaknesses and is generally unnecessary in managed environments. Prefer controlled provisioning rather than relying on convenience enrollment mechanisms.

### What a captive portal does—and does not do

A **captive portal** presents a web page to users before they receive ordinary internet access. It may collect an acceptance of terms, a voucher, or a guest identity. It is **not**, by itself, a Wi-Fi encryption protocol. A captive portal on an open SSID does not retroactively encrypt all wireless traffic. Prefer a protected wireless mode where practical, plus HTTPS for web services.

<details markdown="1">
<summary>Check your understanding: Is a guest password enough?</summary>

**No.** A guest password controls who can join that SSID, but it does not establish guest-to-internal isolation. The AP's VLAN mapping, switch configuration, firewall policy, and client isolation settings must also be correct.

</details>

## 3. Enterprise authentication: 802.1X, EAP, and RADIUS

A managed organization may want each employee to authenticate individually rather than share one password. **IEEE 802.1X** is a framework for **port-based network access control** used on wired and wireless networks. It involves three roles:

1. **Supplicant:** The user's laptop or other device requesting network access.
2. **Authenticator:** The AP or switch that controls access to the network.
3. **Authentication server:** Commonly a **RADIUS** server that validates the request and returns authorization information.

**EAP** (Extensible Authentication Protocol) is a framework for authentication methods. **RADIUS** (Remote Authentication Dial-In User Service) commonly carries EAP messages between the authenticator and authentication server and can convey policy attributes such as VLAN assignments. RADIUS is not the same thing as EAP, and 802.1X is not itself an encryption cipher.

```text
Employee laptop          Wireless AP             RADIUS server
 (supplicant)          (authenticator)         (authentication)
      |                      |                         |
      |--- join/request ---->|                         |
      |<-- EAP exchange ---->|<---- RADIUS/EAP ------->|
      |                      |                         |
      |             authentication decision            |
      |                      |                         |
      |<--- authorized link -|                         |
      |                      |                         |
      |---- traffic subject to VLAN/firewall policy -->|
```

The diagram simplifies several message exchanges and key-establishment steps. Its purpose is to distinguish the **roles**, not provide a packet-level protocol trace.

### EAP-TLS and certificates

**EAP-TLS** uses certificates for strong, mutual authentication when configured correctly. Organizations typically issue trusted client certificates to managed devices and configure clients to validate the authentication server's certificate and expected server identity. That validation matters: a client that trusts any presented authentication server can be exposed to credential interception or impersonation attempts with other EAP methods.

| Method or term | What to remember |
| --- | --- |
| 802.1X | Framework that controls network access |
| EAP | Authentication method framework |
| EAP-TLS | Certificate-based EAP method supporting mutual authentication |
| RADIUS | AAA protocol commonly used by APs/switches and authentication servers |
| AAA | Authentication, authorization, accounting |
| Dynamic VLAN | Network segment assigned by policy after successful authentication |

### Authentication is not authorization

An employee may authenticate successfully but still be placed in a limited-access VLAN. A contractor may receive only application-specific permissions. A printer may use a distinct onboarding mechanism because it cannot support the same supplicant configuration as a managed laptop. The important design principle is **identity and device role should influence access**; success at login must not imply unrestricted network reachability.

## 4. Network access control (NAC): identity plus device posture

**Network access control (NAC)** enforces conditions for joining or using a network. Depending on the product and design, NAC may consider:

- User or device identity, often from 802.1X credentials or certificates.
- Device type and ownership: managed laptop, personal phone, printer, camera, guest.
- **Posture**: whether required security software, operating-system updates, disk encryption, or other controls are present.
- Connection context, such as switch port, SSID, location, or authentication result.

A NAC policy might allow a managed, compliant employee laptop into the employee VLAN; place an unknown device in a guest or quarantine VLAN; or deny a device that fails a required control. **Posture checks are snapshots or ongoing signals, not guarantees that a device is uncompromised.**

| Device | Authentication/posture result | Example decision |
| --- | --- | --- |
| Managed employee laptop | Valid certificate; required controls present | Employee VLAN; limited approved internal services |
| Finance laptop | Valid finance identity and managed device | Finance VLAN; finance applications only |
| Visitor phone | Guest credentials or guest workflow | Guest VLAN; internet only |
| Unknown personal laptop | No enterprise certificate | Guest onboarding or deny internal access |
| Noncompliant managed laptop | Missing required security control | Quarantine/remediation network or deny, according to policy |
| Camera or printer | Approved device enrollment and identity | Dedicated IoT/printer VLAN with tightly restricted destinations |

### Agent-based and agentless approaches

Some NAC systems use an installed **agent** to report device posture. Others use **agentless** techniques such as network observation, identity integrations, or management-system signals. An agent can provide richer host information but introduces deployment and maintenance requirements. Agentless discovery may be easier for IoT devices but offers less direct assurance about endpoint health.

### MAC filtering and MAB are not strong authentication

**MAC address filtering** permits or blocks devices by their reported MAC address. **MAC Authentication Bypass (MAB)** may be used to onboard devices that cannot perform 802.1X. Both rely on identifiers that can be spoofed, so they should be treated as weaker identification mechanisms and combined with segmentation and restrictive policies. They do not replace certificates or robust user/device authentication.

<details markdown="1">
<summary>Reveal decision: A printer cannot use 802.1X</summary>

**Possible approach:** Enroll the printer through a controlled device inventory and an approved alternative mechanism, then place it in a printer VLAN. Permit only the required print and management flows; log access and review the exception. Do not give the printer unrestricted employee-network access merely because it lacks an 802.1X supplicant.

</details>

## 5. Wireless threats and what the controls actually stop

Wireless security must account for threats to the radio link, user trust, AP administration, and network segmentation.

| Threat | What happens | Appropriate defensive response |
| --- | --- | --- |
| **Rogue AP** | Unauthorized AP connects to or operates near the enterprise network | Inventory and authorized AP management; wired switch controls; wireless monitoring |
| **Evil twin** | Attacker impersonates a trusted SSID/AP to entice clients | Validate enterprise authentication server certificates; train users; monitor for impersonation |
| **Deauthentication/disassociation abuse** | Forged management traffic attempts to disrupt clients | PMF where supported/required; monitoring; RF investigation |
| **Weak shared password** | An attacker guesses or obtains the PSK | Strong passphrase; prefer WPA3-SAE; rotate compromised secrets |
| **Misconfigured guest SSID** | Guest clients can reach private networks | Guest VLAN, firewall default deny, isolation testing |
| **Unpatched AP** | Vulnerability or weak management service is exposed | Patch firmware; secure management; disable unnecessary services |
| **Wireless jamming/interference** | RF disruption causes degraded service | Spectrum investigation, coverage planning, redundancy where justified |

### Rogue AP versus evil twin

A **rogue AP** is an unauthorized AP; it may be installed accidentally by an employee or intentionally by an attacker. An **evil twin** deliberately imitates a legitimate wireless network to lure clients. A rogue AP need not be an evil twin, and an evil twin need not be connected to the organization's wired network.

### SSID hiding is not meaningful access control

Disabling SSID broadcast does not make a WLAN secret. Network identifiers can still be discovered through wireless management exchanges. Similarly, allowing only known MAC addresses is not a reliable security boundary because MAC addresses can be observed and spoofed. Use appropriate WPA protection, enterprise authentication, and network policy instead.

### Protected Management Frames (PMF)

PMF protects certain management frames against forgery and tampering. It helps reduce some management-frame attacks, but does not prevent all radio interference, jamming, or denial of service. **Do not claim PMF makes Wi-Fi immune to deauthentication in every circumstance.**

## 6. Design lab: secure guest wireless for the six-VLAN office

**Scenario:** A small office has staff, finance, printers, guests, cameras, and management networks. Visitors need web access while waiting for meetings. Employees need access to approved office services. The administrator needs secure AP management. Your job is to design the guest WLAN without exposing private resources.

### Existing logical layout

The following addresses are **fictional lab examples**, not a production addressing recommendation:

| VLAN | Role | Example subnet | Intended access |
| --- | --- | --- | --- |
| 10 | Employees | `192.168.10.0/24` | Approved internal services and internet |
| 20 | Finance | `192.168.20.0/24` | Finance services and internet |
| 30 | Printers | `192.168.30.0/24` | Print services from approved clients |
| 40 | Guests | `192.168.40.0/24` | Internet and required infrastructure services only |
| 50 | Cameras | `192.168.50.0/24` | Approved recording/management services only |
| 99 | Management | `192.168.99.0/24` | Administrative access to network equipment |

### Proposed design

```text
                  INTERNET
                     |
              [Edge firewall]
                     |
           [Managed Layer 3 switch]
              |               |
       [Managed AP]      [Internal servers]
          |     |
   Office-Staff  Office-Guest
    802.1X       Guest access
    VLAN 10      VLAN 40
          |          |
    Employee PCs   Visitor phones

Management of AP/switch: VLAN 99, admin-only access
Inter-VLAN routing: explicitly restricted by firewall/ACL policy
```

**Important:** An SSID-to-VLAN mapping is a configuration choice, not a security boundary by itself. The upstream switch port must carry the intended tagged VLANs, and the firewall or Layer 3 ACLs must actually enforce the restrictions. If the AP uses an untagged management/native VLAN, document it and prevent accidental guest bridging into that network.

### Step 1 — Decide how guests authenticate

For a small office, a dedicated guest SSID using **WPA3-Personal** with a strong, separately managed passphrase can provide wireless-link protection when all guest clients support it. If broad compatibility is required, evaluate a documented WPA2/WPA3 transition or another supported guest-onboarding approach, understanding the weaker negotiated modes. A captive portal may add terms-of-use or voucher management, but it does not replace encryption.

For managed employees, use **802.1X with EAP-TLS** when the organization has the infrastructure to issue, renew, and revoke certificates securely.

### Step 2 — Map SSIDs to the correct VLANs

- `Office-Staff` → VLAN 10 (or role-based VLAN assignments for managed users).
- `Office-Guest` → VLAN 40.
- AP and switch management → VLAN 99, accessible only from approved administrative systems.

Do **not** map the guest SSID to the employee VLAN merely because both require internet access.

### Step 3 — Write the guest policy before configuring the firewall

| Source | Destination | Example decision | Why |
| --- | --- | --- | --- |
| Guest VLAN 40 | Approved DNS resolver | Allow DNS | Resolve names |
| Guest VLAN 40 | DHCP service | Allow required DHCP exchange | Obtain network configuration |
| Guest VLAN 40 | Public internet | Allow permitted outbound services | Guest browsing |
| Guest VLAN 40 | Employee VLAN 10 | Deny | Protect staff devices |
| Guest VLAN 40 | Finance VLAN 20 | Deny | Protect financial systems |
| Guest VLAN 40 | Printer VLAN 30 | Deny by default | No automatic printer access |
| Guest VLAN 40 | Camera VLAN 50 | Deny | Protect cameras |
| Guest VLAN 40 | Management VLAN 99 | Deny | Protect infrastructure |
| Guest client A | Guest client B | Block where supported | Reduce guest-to-guest exposure |

The DNS and DHCP entries depend on where those services run; allow only the necessary destinations and ports. An allow-to-internet rule must **not** accidentally permit traffic to internal/private destinations through another interface or route. Use appropriate stateful return-traffic handling and explicit inter-VLAN restrictions, as discussed in [Day 22](day-22-firewalls-and-filtering.md).

### Step 4 — Add AP and NAC safeguards

- Require MFA for AP/controller administration where supported; use individual admin accounts.
- Restrict management interfaces to VLAN 99 and approved administrator endpoints.
- Patch AP firmware and back up known-good configurations securely.
- Disable unnecessary services, insecure remote management, and obsolete wireless modes.
- Monitor AP inventory, association/authentication failures, and unauthorized SSIDs.
- For staff, use certificate-based authentication and appropriate device-posture policies where feasible.

### Step 5 — Test isolation instead of assuming it works

Use an **authorized** test laptop connected to `Office-Guest`:

1. Confirm it receives an address from the guest subnet.
2. Confirm it can resolve a public DNS name using the approved resolver.
3. Confirm permitted internet access works.
4. Attempt to connect to a **designated test host** on the employee and finance VLANs; access should fail.
5. Attempt to reach a **designated test management interface**; access should fail.
6. If client isolation is enabled, confirm two test guest devices cannot directly communicate as prohibited.
7. Review firewall/NAC logs to confirm the expected denies and allow decisions.

**Testing caution:** A ping timeout alone does not prove isolation: the destination may block ICMP. Test an approved, known-listening service on a controlled host and verify the corresponding firewall logs. A successful web page load also does not prove all internal destinations are blocked.

<details markdown="1">
<summary>Reveal sample design assessment</summary>

**Strong design:** Guests use a dedicated protected SSID mapped to VLAN 40; guest-to-internal traffic is denied by firewall/ACL policy; AP management is restricted to VLAN 99; optional client isolation prevents guest-to-guest traffic; and an administrator verifies the policy with controlled tests and logs.

**Weak design:** A second SSID uses the same employee VLAN, and the administrator assumes the separate name and password provide isolation. They do not.

</details>

## 7. Reading wireless and NAC logs

The following entries are **fictional examples** for learning. They are not output captured from a real AP or NAC product.

```text
09:14:02 AP-2  SSID=Office-Staff  client=lab-laptop-01  auth=EAP-TLS  result=SUCCESS  vlan=10
09:15:18 AP-2  SSID=Office-Staff  client=unknown-07     auth=EAP-TLS  result=FAIL     reason=UNTRUSTED_CERT
09:16:40 NAC-1 device=lab-laptop-02 posture=NONCOMPLIANT action=QUARANTINE vlan=70
09:17:05 FW-1  src=192.168.40.23 dst=192.168.20.10 proto=TCP dport=443 action=DENY
09:18:22 AP-2  SSID=Office-Guest  client=guest-phone-04 result=ASSOCIATED vlan=40
```

| Event | Interpretation | Next analyst action |
| --- | --- | --- |
| Staff EAP-TLS success | Certificate-based authentication succeeded | Confirm correct role/VLAN if investigating access |
| Untrusted certificate | Authentication failed certificate trust checks | Check enrollment, certificate chain, expiration, and server/client policy; do not disable validation |
| NAC quarantine | Device failed a posture requirement | Identify failed requirement and approved remediation path |
| Guest-to-finance deny | Segmentation rule blocked attempted access | Determine whether expected test, misconfiguration, or suspicious behavior |
| Guest association | Client joined guest WLAN | Verify guest VLAN and effective restrictions; association alone does not prove internet access |

### Correlation matters

One blocked guest-to-finance connection might be a benign misclick or a controlled test. Repeated attempts across many internal hosts may warrant investigation. Correlate timestamps, AP identifiers, device inventory, firewall decisions, and authentication logs. Recall from [Day 23](day-23-ids-ips-and-network-monitoring.md) that **an alert is evidence to interpret**, not proof of malicious intent.

## 8. Safe technical demonstrations

These examples are for **your own device or an authorized lab**. Commands and interfaces vary by operating system and adapter.

### macOS: inspect the current wireless interface

```bash
networksetup -listallhardwareports
networksetup -getairportnetwork en0
```

First confirm which device corresponds to Wi-Fi; it may not be `en0`. The second command shows the current network name when supported. It does not prove the AP is legitimate or the network is isolated.

### Linux: inspect local wireless connection details

```bash
nmcli device status
nmcli -f IN-USE,SSID,SECURITY,SIGNAL device wifi list
```

These commands require NetworkManager and a compatible wireless interface. They show nearby networks and advertised security modes; they do not establish whether an SSID is trustworthy.

### Check local addressing and routes

```bash
# macOS
ifconfig
netstat -rn

# Linux
ip address
ip route
```

A guest IP address in the expected subnet supports that the client received the intended network configuration, but **route and firewall policy must also be checked**.

### Interpreting an authorized connection test

If a designated internal test service should be blocked from guest Wi-Fi, attempt only the approved connection and inspect the firewall logs. Do not scan nearby networks, collect other users' traffic, or attempt to bypass access controls. Security+ asks you to **select and validate controls**, not to attack networks without authorization.

## 9. Common misconceptions and Security+ exam traps

| Tempting but wrong statement | More accurate interpretation |
| --- | --- |
| “A hidden SSID is secure.” | SSID hiding is not meaningful authentication or encryption. |
| “Guest Wi-Fi is isolated because it has a different name.” | Isolation requires correct VLAN mapping and enforceable network policy. |
| “A captive portal encrypts Wi-Fi.” | A portal is an onboarding/web-access mechanism, not wireless link encryption. |
| “802.1X and RADIUS are the same protocol.” | 802.1X controls network access; RADIUS commonly connects authenticator and AAA server. |
| “MAC filtering is strong device authentication.” | MAC addresses can be spoofed. |
| “WPA3 stops all wireless attacks.” | WPA3 improves specific protections but does not eliminate rogue APs, phishing, endpoint compromise, or jamming. |
| “Authentication means access to every subnet.” | Authorization and segmentation still restrict resources. |
| “A failed ping proves the firewall works.” | The target may block ICMP; validate intended traffic and logs. |
| “NAC posture proves a device is safe.” | Posture is one security signal, not proof of absence of compromise. |

**Exam strategy:** Identify the requirement first. If the question asks for **individual employee authentication**, think 802.1X/EAP and an AAA backend. If it asks for **guest separation**, think VLAN/ACL/firewall and client isolation. If it asks about **certificate-based mutual authentication**, think EAP-TLS. If it asks about **device compliance before access**, think NAC posture assessment and quarantine/remediation.

## 10. Key terminology and takeaways

- **WLAN:** Local network using Wi-Fi radio connections.
- **SSID:** Network name; not proof of authenticity.
- **AP:** Device connecting wireless clients to a network.
- **WPA2/WPA3:** Families of Wi-Fi security protections.
- **SAE:** Authentication mechanism used in WPA3-Personal.
- **PMF:** Protects selected management frames.
- **802.1X:** Port-based network access control framework.
- **Supplicant / authenticator / authentication server:** Three key 802.1X roles.
- **EAP:** Framework for authentication methods.
- **EAP-TLS:** Certificate-based EAP method.
- **RADIUS:** AAA protocol frequently used with enterprise network access.
- **NAC:** Controls access using identity, device status, and policy.
- **Posture assessment:** Evaluation of device security requirements.
- **Quarantine VLAN:** Restricted network used for remediation or limited access.
- **Rogue AP / evil twin:** Unauthorized AP / impersonating wireless network.
- **Client isolation:** Restriction on direct communication among wireless clients.

**Five essential takeaways:**

1. Protect the **wireless link** with an appropriate security mode; do not rely on hidden SSIDs or portals.
2. Use **individual identity and certificates** for managed enterprise access where feasible.
3. Treat **NAC posture** as an additional access-control signal, not absolute assurance.
4. Map guest traffic to a **separate VLAN** and enforce deny rules for internal resources.
5. **Verify** the effective configuration with controlled connection tests and correlated logs.

## 11. Practice questions

**Question 1.** A company wants employees to authenticate individually to corporate Wi-Fi using managed device certificates. Which solution BEST meets this requirement?

A. WPA2-Personal with one shared password
B. 802.1X using EAP-TLS and a RADIUS authentication server
C. Hidden SSID with MAC filtering
D. Open Wi-Fi with a captive portal

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** 802.1X with EAP-TLS supports certificate-based authentication and individual device identity, commonly backed by RADIUS. **A** shares credentials, **C** provides weak identification, and **D** does not provide the requested enterprise certificate authentication.

</details>

**Question 2.** Visitors can join `Office-Guest`, but a test shows they can connect to a finance server. What should the administrator correct FIRST?

A. Rename the guest SSID to make it less obvious
B. Increase AP transmit power
C. Configure and verify guest VLAN mapping and firewall/ACL isolation
D. Replace the captive portal logo

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Guest-to-finance connectivity indicates a failure in effective segmentation or policy enforcement. **A**, **B**, and **D** do not enforce the necessary trust boundary.

</details>

**Question 3.** A managed laptop passes 802.1X authentication, but NAC places it on a remediation network because disk encryption is disabled. Which concept BEST explains this outcome?

A. Successful authentication does not guarantee compliance-based authorization
B. The AP is necessarily an evil twin
C. WPA3 automatically blocks all unencrypted disks
D. RADIUS always assigns every device to the same VLAN

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** NAC can apply device-posture requirements after or alongside identity authentication and restrict noncompliant devices. **B** is unsupported, **C** confuses wireless and disk encryption, and **D** is false.

</details>

**Question 4.** An employee's laptop displays a familiar corporate SSID at a conference. The network requests corporate credentials, but the authentication server certificate is unexpected. What is the BEST action?

A. Ignore the certificate warning because the SSID matches
B. Disable certificate validation to complete the connection
C. Enter the credentials and report the warning later
D. Refuse the connection and verify the network with IT

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** An SSID can be impersonated by an evil twin. Unexpected server certificate information is a warning; do not weaken validation or disclose credentials. **A**, **B**, and **C** increase exposure.

</details>

**Question 5.** A small business provides guest Wi-Fi with a web page requiring visitors to accept terms. The SSID is open. Which statement is MOST accurate?

A. The portal automatically provides WPA3 encryption
B. The portal replaces the need for network segmentation
C. The portal controls an onboarding step but does not itself encrypt wireless frames
D. Open Wi-Fi prevents nearby devices from observing any radio traffic

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** A captive portal is not a link-layer encryption protocol. **A** and **D** incorrectly assume encryption; **B** ignores the need to isolate guest traffic from internal systems.

</details>

## 12. Further reading

- [CompTIA Security+ certification and exam information](https://www.comptia.org/certifications/security) — obtain the official SY0-701 objectives for precise exam scope.
- [NIST SP 800-153 — Guidelines for Securing Wireless Local Area Networks](https://csrc.nist.gov/pubs/sp/800/153/final) — wireless security principles (older publication; verify current applicability).
- [NIST SP 800-97 — Establishing Wireless Robust Security Networks](https://csrc.nist.gov/pubs/sp/800/97/final) — historical context for enterprise wireless security.
- [IEEE 802.1X overview](https://1.ieee802.org/security/802-1x/) — port-based network access control standard.
- [RFC 5216 — EAP-TLS](https://www.rfc-editor.org/rfc/rfc5216) — foundational EAP-TLS specification; see also [RFC 9190](https://www.rfc-editor.org/rfc/rfc9190) for TLS 1.3 support.
- [RFC 2865 — RADIUS](https://www.rfc-editor.org/rfc/rfc2865) — RADIUS protocol background.
- [Wi-Fi Alliance security overview](https://www.wi-fi.org/discover-wi-fi/security) — WPA3 and wireless security resources.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
