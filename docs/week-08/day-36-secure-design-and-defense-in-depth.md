# Day 36 — Secure Design and Defense in Depth

> **CompTIA Security+ SY0-701** · **Week 08, Lesson 1 of 5** · Domain 3, Objective **3.2: Applying security principles to secure enterprise infrastructure** (related: 1.1, 1.2, 2.5, 3.1, 3.4)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 35 — Penetration Testing and Remediation](../week-07/day-35-penetration-testing-and-remediation.md)  
> **Next:** Day 37 (title pending curriculum confirmation)

## 1. Learning objectives

By the end of this lesson, you will be able to:

- Explain how defense in depth limits the consequences of individual control failures.
- Apply least privilege, secure defaults, separation of duties and attack surface reduction to an architecture.
- Identify trust boundaries and select controls for traffic crossing them.
- Distinguish inline from passive monitoring and fail-open from fail-closed behavior.
- Evaluate shared dependencies, residual risk and availability trade-offs.
- Design and validate layered protection for a small healthcare clinic.

Exam alignment: this lesson primarily develops scenario-based control selection, placement, security zones and failure modes under objective 3.2. Related concepts support control classification, zero trust, mitigation and resilience. See the [official SY0-701 objectives](https://assets.ctfassets.net/82ripq7fjls2/6TYWUym0Nudqa8nGEnegjG/0f9b974d3b1837fe85ab8e6553f4d623/CompTIA-Security-Plus-SY0-701-Exam-Objectives.pdf).

## 2. The big idea: A locked front door is not a security architecture

Imagine a clinic that locks its entrance every evening. The lock reduces unauthorized physical access, but it cannot stop a receptionist from opening a malicious attachment, a stolen account from accessing cloud records, or a failed disk from destroying appointment information. Each problem requires a different protection mechanism.

**Security architecture** is the arrangement of systems, identities, data, connections and controls to satisfy security and operational requirements. **Secure design** means incorporating those requirements into the arrangement from the beginning, rather than relying entirely on fixes after deployment.

**Defense in depth** combines complementary protections so that an attacker who bypasses one safeguard still faces other barriers, and defenders retain ways to detect, contain and recover from incidents.

| Security objective | Clinic example | Consequence of failure |
| --- | --- | --- |
| Confidentiality | Restrict access to patient records | Unauthorized disclosure |
| Integrity | Authorize and audit prescription changes | Incorrect treatment information |
| Availability | Maintain tested recovery procedures | Delayed patient care |

These are the three elements of the CIA triad. A useful design must balance all three, particularly when systems support physical safety.

🧠 **Memory hook:** Prevent what you can, detect what gets through, contain what spreads, recover what is damaged.

## 3. Defense in depth: Complementary layers of protection

### 3.1 How layers work together

An attack rarely consists of a single event. A malicious attachment may lead to execution, credential theft, access to other systems and eventual data loss. Controls placed at different points can interrupt different stages.

| Layer | Example control | What it contributes | What it cannot guarantee |
| --- | --- | --- | --- |
| Governance and people | Training and access approval | Reduces unsafe decisions | Perfect human behavior |
| Physical | Locked equipment room | Restricts direct equipment access | Protection against remote attacks |
| Identity | Multifactor authentication (MFA) | Reduces password-only account compromise | Protection against every stolen session |
| Network | Firewall and enforced segmentation | Limits network paths | Prevention of every attack over allowed paths |
| Endpoint | Endpoint detection and response (EDR) | Detects and can contain suspicious processes | Detection of every malicious action |
| Application | Server-side authorization | Restricts application operations | Immunity to implementation flaws |
| Data | Encryption and permission restrictions | Protects information within defined boundaries | Protection from all authorized misuse |
| Recovery | Isolated backups and restoration tests | Supports restoration after damage | Reversal of information disclosure |

Consider a malicious attachment delivered to a staff member. Email filtering might reject it. If filtering fails, application execution controls may prevent it from running. If execution succeeds, EDR might detect the process. If detection fails, network segmentation can restrict access to other systems. If data is damaged, protected backups may support recovery.

The layers are not necessarily traversed in this order. An attacker using stolen cloud credentials might bypass the endpoint and perimeter entirely. Architecture must account for multiple entry points, not one imagined linear attack.

### 3.2 Control categories and functions

Review the distinction between how a control is implemented and what it accomplishes. A technical control uses technology; a managerial control governs decisions; an operational control relies on people and processes; a physical control protects physical assets.

| Function | Example |
| --- | --- |
| Preventive | Firewall denying unauthorized traffic |
| Detective | Alert on unusual access to records |
| Corrective | Reimage a compromised workstation |
| Deterrent | Notice warning that access is monitored |
| Directive | Policy requiring separate administrator accounts |
| Compensating | Restricted access gateway protecting a legacy system that cannot be patched immediately |

A control can serve multiple functions. A security camera may deter and detect, while an EDR platform may detect and prevent. Examine its configured behavior and the scenario rather than assuming its name determines its function.

### 3.3 Independence and common-mode failure

Two controls may look independent on a diagram but depend on the same administrator credentials, cloud identity provider or management network. A **common-mode failure** occurs when a shared cause defeats multiple protections.

For example, an attacker who compromises a privileged account might disable endpoint monitoring and delete online backups if both are managed with that account. Separate backup administration, restricted deletion privileges and protected backup copies reduce this shared exposure.

**Exam trap:** Defense in depth is not simply purchasing more products. Two identical filters with identical rules can share the same blind spots. Complementary mechanisms and distinct trust boundaries are more valuable than an unexamined control count.

## 4. Secure design principles

### 4.1 Least privilege and need to know

**Least privilege** grants a person, service or device only the permissions required to perform its assigned tasks. **Need to know** restricts access to information according to a legitimate information requirement.

A receptionist may need appointment details but not psychotherapy notes. An application service may need to read and update specific database records but not administer the entire database server. Permissions should be reviewed when responsibilities change.

Use separate accounts for routine work and administration; where feasible, grant elevated privileges only when needed and for limited periods. This limits the damage caused by mistakes and compromised credentials.

### 4.2 Secure defaults and complete mediation

A secure default starts with restricted access and explicitly enables approved functionality. A default-deny firewall blocks traffic unless a rule permits it. This contrasts with default allow, where traffic passes unless a rule blocks it.

**Complete mediation** means authorization is enforced whenever protected resources are accessed, rather than relying on an earlier check or hiding a user-interface element. For example, an application must verify access to a patient record on the server even if the interface hides the record from unauthorized users.

**Important limitation:** A default-deny policy still requires accurate allow rules. An overly broad exception can undermine the entire policy.

### 4.3 Separation of duties and simplicity

Separation of duties divides sensitive responsibilities to reduce the risk of abuse or error by one person. For example, one employee requests a change to backup retention and another approves it. This differs from least privilege: even two appropriately privileged individuals may need independent approval responsibilities.

Design simplicity reduces opportunities for mistakes. Ten overlapping firewall policies that nobody understands may be less effective than a smaller set of documented, tested rules. Simplicity does not mean removing necessary layers; it means avoiding unnecessary complexity.

### 4.4 Attack surface reduction and secure by design

The **attack surface** includes exposed interfaces, services, identities and other opportunities for interaction with a system. Disable unused services, remove unnecessary software, restrict management interfaces and retire obsolete accounts.

Secure by design incorporates protections during requirements, design, implementation and testing. Secure by default concerns the initial configuration delivered to users. A product may support strong security features yet ship with unsafe defaults; therefore, the concepts are related but not identical.

Threat modeling helps identify assets, entry points, trust boundaries, plausible threats and mitigations before deployment. Revisit the model when the architecture changes.

## 5. Trust boundaries, security zones and zero trust

### 5.1 From flat networks to controlled zones

A **trust boundary** is a point where security assumptions or access requirements change. A security zone groups systems according to security needs. In a flat network, a compromised device may have broad connectivity to other devices, increasing opportunities for lateral movement.

Virtual local area networks (VLANs) separate Layer 2 broadcast domains. To enforce access restrictions between routed VLANs, configure appropriate firewall policies or access control lists (ACLs). A VLAN label alone does not establish an authorization policy.

```text
Internet
   |
Edge firewall
   |
   +-- Guest Wi-Fi zone (internet access only)
   +-- Staff zone (approved application access)
   +-- Medical device zone (restricted device flows)
   +-- Server zone (application and database tiers)
   +-- Management zone (authorized administration)
```

This diagram describes logical zones, not a required physical topology. Inter-zone traffic is mediated by routing and enforcement devices. Firewall rules should describe source, destination, protocol, port and direction as appropriate; identity and application-aware controls may add further context.

### 5.2 North-south and east-west traffic

North-south traffic crosses an environment's external boundary; east-west traffic moves between internal workloads or zones. A perimeter firewall may protect inbound connections without inspecting internal traffic that never crosses it. Internal segmentation and workload protections address that gap.

Microsegmentation applies finer-grained restrictions, often between individual workloads, using network or host-level enforcement. Its benefits depend on policy accuracy, visibility and coverage.

### 5.3 Zero trust

Zero trust rejects implicit authorization based solely on network location or organizational ownership. Access is evaluated using identity, requested resource, device condition, policy and other relevant context. A policy decision point evaluates access; a policy enforcement point applies the decision. Continued evaluation may trigger revocation when risk changes.

An authenticated user connected through a virtual private network (VPN) should not automatically gain access to every server. Authentication establishes identity; authorization determines permitted actions.

**Exam tip:** Segmentation restricts connectivity; zero trust governs resource access without assuming that internal location is sufficient proof of trust. They complement each other.

## 6. Control placement and failure behavior

### 6.1 Inline versus passive controls

| Attribute | Inline | Passive/out-of-band |
| --- | --- | --- |
| Placement | In the traffic path | Receives a copy of traffic or telemetry |
| Typical example | Intrusion prevention system (IPS) | Network intrusion detection system (IDS) |
| Direct blocking | Possible | Usually not directly in the observed path |
| Failure impact | May interrupt traffic | May cause loss of visibility |
| Design concern | Latency and availability | Monitoring coverage and dropped copies |

An IDS can trigger another system to block traffic, but a passive sensor itself is not forwarding the original packets. An inline device can inspect and potentially block the traffic it handles. Active versus passive describes behavior; inline versus out-of-band describes placement. These concepts are related, not interchangeable.

Encrypted traffic can limit inspection of payload contents unless an authorized inspection architecture provides plaintext visibility. Endpoint logs and application telemetry can supplement network observation.

### 6.2 Fail-open and fail-closed

| Failure mode | Behavior when enforcement fails | Main trade-off |
| --- | --- | --- |
| Fail-open | Permits traffic or access through the failed control | Preserves some availability but reduces protection |
| Fail-closed | Denies traffic or access through the failed control | Preserves restriction but may disrupt legitimate operations |

For a failed inspection appliance, fail-open might allow packets to bypass inspection while upstream routing remains available. Fail-closed might interrupt the connection. Neither guarantees overall safety, confidentiality or availability; surrounding controls and dependencies matter.

A clinic might require administrative database access to fail closed while maintaining a separately designed emergency clinical workflow. That workflow can use tightly scoped, audited emergency access where supported, not unrestricted access for everyone.

**Exam trap:** Do not confuse fail-open with high availability. High availability seeks to keep the service operating through redundancy and failover; fail-open specifies behavior when a control cannot enforce its policy.

### 6.3 Selecting effective controls

Place controls where they can observe or enforce the traffic and operations they are intended to protect. A web application firewall (WAF) can inspect supported HTTP application traffic, but does not replace server-side authorization. A jump server centralizes controlled administrative access, but becomes a sensitive asset requiring hardening and monitoring. A firewall cannot block a connection that never traverses its enforcement point.

## 7. Architectural trade-offs and validation

Security design begins with requirements. Identify critical assets, permissible downtime, users, required communication flows and threats. Compare alternatives according to their risk reduction and operational effects.

| Decision | Benefit | Cost or limitation |
| --- | --- | --- |
| Finer segmentation | Smaller compromise scope | More rules and troubleshooting |
| Inline inspection | Direct prevention | Processing delay and potential outages |
| Redundant appliances | Increased availability | Cost and configuration complexity |
| Centralized identity | Consistent policy and visibility | Shared dependency on identity service |
| Isolated backups | Reduced ransomware exposure | Restoration time and administrative effort |

A **single point of failure** is a component whose failure can interrupt an essential function. Redundancy can address component failures, but two devices with the same flawed configuration may fail together. Resilience requires examining both physical and logical dependencies.

**Residual risk** remains after controls are implemented. Document it, identify the responsible risk owner and verify that the remaining exposure is within organizational requirements. Do not claim that layered controls eliminate all risk.

Validate the design using permitted and denied connection tests, role-based access tests, logging checks, failure simulations and restoration exercises. A diagram is a hypothesis about how protection works; testing provides evidence.

## 8. Worked case study: Securing a veterinary hospital's records

A veterinary hospital has ten staff computers, a cloud appointment system, an on-premises file server and a guest wireless network. All local devices currently share one network. Staff use a shared administrator account, and backups are stored on a continuously mounted drive.

The hospital has a limited budget and cannot tolerate prolonged disruption during operating hours. Its highest-priority risks are account compromise, ransomware propagation and loss of appointment information.

| Finding | Reasoning | Proposed protection | Remaining limitation |
| --- | --- | --- | --- |
| Shared administrator | Actions cannot reliably be attributed to individuals | Individual accounts, MFA and separate admin roles | Session theft remains possible |
| Flat network | Guest and staff devices have unnecessary connectivity | Guest and staff zones with enforced restrictions | Approved applications can still contain flaws |
| Mounted backup drive | Malware with write permission could damage copies | Separate backup credentials and isolated/versioned copies | Recovery still takes time |
| No central alerting | Compromise may continue unnoticed | Endpoint and identity alerts reviewed by assigned staff | Alerts can be missed |

The hospital first inventories essential flows and confirms which staff need access to the file server. It creates a restricted guest network, enforces staff-to-server rules and removes shared administration. It then verifies that individual users cannot alter backup retention and conducts a sample restoration using test files.

To reduce disruption, changes are staged with rollback instructions. The hospital schedules a separate test of identity outages and documents a temporary manual appointment workflow.

The architecture reduces likely impact without requiring expensive duplicate copies of every product. It also leaves explicit residual risks, such as cloud provider downtime and misuse by authorized users.

**Takeaway:** Select layers according to attack paths and business requirements, then test the dependencies and recovery capabilities rather than assuming that installation equals protection.

## 9. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Explain defense in depth. | _Write here._ |
| Distinguish least privilege and separation of duties. | _Write here._ |
| Explain default deny. | _Write here._ |
| Explain why VLANs alone are insufficient. | _Write here._ |
| Distinguish segmentation and zero trust. | _Write here._ |
| Compare inline and passive monitoring. | _Write here._ |
| Compare fail-open and fail-closed. | _Write here._ |
| Explain common-mode failure and residual risk. | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Explain defense in depth. | Complementary controls prevent, detect, contain and support recovery even when individual safeguards fail. |
| Distinguish least privilege and separation of duties. | Least privilege limits individual permissions; separation of duties divides sensitive responsibilities among people or roles. |
| Explain default deny. | Access is denied unless explicitly authorized. |
| Explain why VLANs alone are insufficient. | VLANs separate broadcast domains; routed traffic needs appropriate enforcement policies. |
| Distinguish segmentation and zero trust. | Segmentation restricts network paths; zero trust evaluates resource access without implicit trust based on location. |
| Compare inline and passive monitoring. | Inline devices handle original traffic and can block it; passive sensors observe copies or telemetry. |
| Compare fail-open and fail-closed. | Fail-open permits access when enforcement fails; fail-closed denies it. |
| Explain common-mode failure and residual risk. | A shared cause can defeat several safeguards; residual risk is the exposure remaining after mitigation. |

</details>

## 10. Hands-on lab: Design layered protection for a clinic

**Time:** 20–25 minutes  
**Tools:** Paper or a Markdown editor; optional spreadsheet.  
**Safety and scope:** This is a fictional design exercise. Do not connect to, scan or modify any real healthcare system. Use only the supplied synthetic inventory.

### Step 1 — Identify assets and requirements

A clinic has the following systems:

| Asset | Required function |
| --- | --- |
| Reception computers | Scheduling through a web application |
| Clinician computers | Read and update authorized patient records |
| Medical devices | Send measurements to a designated collector |
| Guest Wi-Fi | Internet access |
| Application server | Serve HTTPS requests from authorized staff |
| Database server | Accept database requests from application server |
| Backup repository | Store protected database backups |
| IT management workstation | Administer approved infrastructure |

Assume the database uses TCP 5432, the application uses TCP 443, and the collector receives HTTPS measurements on TCP 443. These are fictional design assumptions, not universal medical device requirements.

Write one confidentiality, integrity and availability requirement for the clinic.

### Step 2 — Draw zones

Draw distinct guest, staff, medical device, server, backup and management zones. Separate the application and database into controlled subzones or equivalent workload policies. Add a collector in the server environment and mark every enforcement boundary.

### Step 3 — Construct a traffic policy

Complete the following matrix using Allow or Deny. Assume stateful enforcement, approved destination addresses and a default-deny policy. Supporting services such as DNS and time synchronization require separate documented rules.

| Source | Destination | Service | Decision |
| --- | --- | --- | --- |
| Guest | Internet | Approved web traffic | |
| Guest | Database | TCP 5432 | |
| Reception | Application | TCP 443 | |
| Reception | Database | TCP 5432 | |
| Clinician | Application | TCP 443 | |
| Medical device | Collector | TCP 443 | |
| Medical device | Management | Any | |
| Application | Database | TCP 5432 | |

Specify a separate authenticated backup workflow. Avoid granting all staff write or deletion access to the repository.

### Step 4 — Simulate control failures

For each scenario, identify a remaining protection and an operational response:

1. A receptionist's password is stolen.
2. Malware executes on a staff workstation despite email filtering.
3. The inline inspection appliance fails.
4. An endpoint administrator account is compromised.
5. The database becomes unavailable.

### Step 5 — Validate and interpret

Write four test cases covering an allowed flow, a prohibited flow, application authorization and backup recovery. For each test, record the expected outcome. These are design predictions, not observed results.

Answer the following questions:

- Why can reception and clinicians use the same application port without receiving the same permissions?
- Does blocking guest-to-database traffic prevent every possible data breach?
- How could a shared administrator undermine the backup layer?
- What would you require before permitting fail-open inspection for clinical traffic?

<details markdown="1">
<summary>Reveal lab answers</summary>

**Requirements:** Only authorized clinicians can read protected records; prescription or record changes must be authorized and auditable; essential clinical functions must remain recoverable within approved downtime limits.

**Zones:** Separate the guest, staff, device, application, database, backup and management environments. Enforce the boundaries with firewalls or equivalent workload policies. The collector must accept only necessary device traffic.

| Source | Destination | Service | Expected decision |
| --- | --- | --- | --- |
| Guest | Internet | Approved web traffic | Allow |
| Guest | Database | TCP 5432 | Deny |
| Reception | Application | TCP 443 | Allow |
| Reception | Database | TCP 5432 | Deny |
| Clinician | Application | TCP 443 | Allow |
| Medical device | Collector | TCP 443 | Allow |
| Medical device | Management | Any | Deny |
| Application | Database | TCP 5432 | Allow |

A dedicated backup service uses narrowly scoped credentials and an explicitly permitted transport; deletion and retention changes require separate authorization. Protected copies are isolated from ordinary endpoint administration.

**Failure simulations:** MFA can reduce password-only compromise; revoke sessions and investigate account activity. EDR and segmentation can detect or contain malicious execution; isolate and investigate the workstation. A failed inline appliance requires tested failover or a risk-approved failure policy. Separate backup administration limits the effect of endpoint administrator compromise; revoke the account and examine related actions. Database failure requires validated failover or restoration and a clinical downtime procedure.

**Validation cases:** Authorized clinician HTTPS connection succeeds; guest database connection is denied; receptionist's request for a restricted clinical record is rejected by application authorization; a synthetic backed-up record is restored and checked for integrity within the defined test target.

**Interpretation:** Network connectivity does not determine application privileges. Guest isolation addresses one attack path, not every breach. Shared privileged credentials can defeat multiple layers. Fail-open requires explicit risk assessment, restricted flows, remaining safeguards, alerting, time limits where appropriate and a tested recovery plan approved by responsible stakeholders.

</details>

## 11. Eight original Security+-style questions

These original practice questions are not actual CompTIA exam items.

### Q1. An organization wants to reduce the effect of a compromised email filter. Which design BEST demonstrates defense in depth?

A. Install a second filter using the same rules and administration account.  
B. Combine email filtering, endpoint execution restrictions, monitoring and segmentation.  
C. Replace endpoint monitoring with stronger email filtering.  
D. Allow all internal traffic after messages pass the filter.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Complementary controls address different stages of compromise. A shares dependencies and blind spots; C removes a distinct layer; D increases internal exposure.

</details>

### Q2. A receptionist can schedule appointments but cannot modify clinical diagnoses. Which principle is MOST directly demonstrated?

A. High availability  
B. Fail-open  
C. Load balancing  
D. Least privilege

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Permissions match job requirements. A and C concern availability or distribution; B describes failure behavior.

</details>

### Q3. Two VLANs are routed through a device that permits all inter-VLAN traffic. What should the administrator implement to restrict communication?

A. Explicit inter-zone firewall rules or ACLs  
B. Identical VLAN names  
C. Longer DHCP lease durations  
D. Additional DNS records

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** Enforcement rules restrict routed traffic. B changes labels, C affects address leases and D affects name resolution, not access policy.

</details>

### Q4. A failed inline inspection device continues forwarding traffic without inspection. Which failure mode is configured?

A. Fail-closed  
B. Passive detection  
C. Fail-open  
D. Separation of duties

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Traffic passes despite the loss of enforcement. A would deny traffic; B describes observation rather than failure behavior; D concerns responsibility allocation.

</details>

### Q5. A security team needs to observe copies of network packets without placing its sensor in the forwarding path. Which solution BEST meets this requirement?

A. Inline IPS  
B. Passive network IDS attached to a monitoring feed  
C. Default-deny gateway  
D. Inline WAF

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** A passive IDS analyzes copied traffic. A, C and D are enforcement devices in the stated configurations and normally process original traffic.

</details>

### Q6. One administrator credential can disable monitoring and delete recovery copies. Which architectural problem is illustrated?

A. Geographic dispersion  
B. Secure defaults  
C. Need to know  
D. Common-mode failure exposure

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** One compromised dependency can defeat otherwise distinct controls. A concerns location diversity; B concerns initial restrictions; C concerns legitimate information access.

</details>

### Q7. A user authenticates through the corporate VPN. The application subsequently evaluates identity, device posture and resource-specific permissions. Which approach is BEST illustrated?

A. Zero trust  
B. Implicit perimeter trust  
C. Network address translation  
D. Unrestricted lateral access

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** Access is evaluated using resource-specific policy and contextual evidence. B and D rely on broad trust; C translates network addresses and does not establish user authorization.

</details>

### Q8. A clinic must preserve essential care during an inspection appliance outage without granting unrestricted network access. Which design BEST balances the requirements?

A. Permanently disable all inspection and segmentation.  
B. Require every device to use the database administrator account.  
C. Use tested redundancy and a restricted, monitored downtime workflow.  
D. Deny all clinical activity indefinitely during every failure.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Redundancy and scoped downtime procedures preserve essential operations while retaining protections. A and B introduce broad exposure; D disregards availability and clinical requirements.

</details>

**Score guide:** 7–8 correct: strong understanding; 5–6: revisit Sections 3–7, focusing on missed concepts; 0–4: repeat Sections 9–10 and retry.

## 12. Discussion challenge

A small legal practice moves its document management system to a cloud provider while keeping local workstations and a local backup server. Partners want convenient remote access, but the firm cannot employ a full-time security analyst.

Design three complementary protection layers, identify one shared dependency, and explain which traffic and resources must remain accessible. Decide how the firm should handle an identity provider outage without creating permanent unrestricted access. Justify your choices in terms of cost, staffing and residual risk.

**Bonus:** Explain how your proposal protects each element of the CIA triad and identify one measurable test of each.

## 13. Study from the Treadmill 🏃

**Direct links to specific videos:**

- [Professor Messer — Security Controls (1.1)](https://www.youtube.com/watch?v=STM3EUvL7wg): Review the functions of complementary controls and distinguish preventive, detective and corrective behavior.
- [Professor Messer — Zero Trust (1.2)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/zero-trust-sy0-701/): Focus on policy decisions, enforcement, adaptive identity and why network location alone is insufficient.

**Active-listening prompts:**

- Which controls still provide protection after prevention fails?
- What evidence can influence a zero-trust access decision?
- How would you apply these concepts at a clinic's application and network boundaries?

## 14. One-minute recap

- **Defense in depth** combines complementary safeguards across attack paths.
- **Least privilege** limits permissions; **separation of duties** divides sensitive responsibilities.
- **Default deny** permits only explicitly approved access.
- **Security zones** require enforcement; **zero trust** rejects implicit location-based trust.
- **Inline controls** can directly block traffic; passive sensors observe it.
- **Fail-open** preserves passage through a failed control; **fail-closed** preserves restriction.
- **Common-mode failures** can defeat multiple layers; validate dependencies and document residual risk.

**Golden rule:** Design for the failure of a control, not for the assumption that it will always work.

**Next:** Day 37 — title pending curriculum confirmation.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
