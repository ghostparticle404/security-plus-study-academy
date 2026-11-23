# Day 37 — Zero Trust Architecture

> **CompTIA Security+ SY0-701** · **Week 08, Lesson 2 of 5** · Domain 1, Objective **1.2: Fundamental security concepts** (related: 3.2 and 4.6)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 36 — Secure Design and Defense in Depth](day-36-secure-design-and-defense-in-depth.md)  
> **Next:** Day 38 — Topic pending curriculum confirmation

**Prerequisites:** Days 3 and 36; familiarity with authentication, authorization, network segmentation and security controls from Weeks 1–7.

## 1. Learning objectives

By the end of this lesson, you will be able to:

- Explain why network location alone cannot establish trust.
- Distinguish the control plane from the data plane.
- Identify policy engine, policy administrator and policy enforcement point responsibilities.
- Apply adaptive identity, threat scope reduction and policy-driven access control to access requests.
- Evaluate identity, device and contextual evidence to grant, deny or revoke access.
- Design least-privilege access policies and assess their security and availability trade-offs.

## 2. The big idea: Access is a decision, not a permanent entitlement

Imagine an office building where everyone who passes the front door can enter every room. The receptionist checks an employee badge, but nobody checks whether that employee is allowed into payroll, the server room or the legal archive. A stolen badge opens far more than the thief needs.

Traditional perimeter-centric networks can develop the same weakness. A firewall protects the boundary, but overly broad internal permissions allow an intruder who compromises one endpoint to reach other systems. This is lateral movement: moving from an initial foothold to additional resources.

Zero Trust (ZT) is a security approach that removes automatic trust based on network location or asset ownership. Zero Trust Architecture (ZTA) applies that approach to enterprise systems, resources and access decisions. A request must satisfy applicable policy regardless of whether it originates inside a corporate office, from home or from a cloud workload.

| Question | Perimeter-centric design with implicit internal trust | Zero Trust design |
| --- | --- | --- |
| Where is the requester? | Often a primary trust criterion | One contextual signal among several |
| What becomes accessible? | Potentially a broad network | Explicitly authorized resources and actions |
| What happens after login? | Access may persist until logout | Access may be reevaluated as conditions change |
| What if a device is compromised? | Internal reachability may enable movement | Scoped permissions and segmentation restrict exposure |
| Is a firewall useful? | Yes | Yes, but it is not sufficient alone |

These are architectural tendencies, not mutually exclusive technologies. A traditional network can implement strong internal controls; a Zero Trust deployment can still use a perimeter firewall.

🧠 **Memory hook:** Being inside the building is not permission to enter every room.

**CIA triad connection:** Confidentiality benefits from narrowly scoped access; integrity benefits from restricting modification privileges; availability requires resilient policy infrastructure so authorized users can still perform essential work.

## 3. The principles behind Zero Trust

### Verify explicitly

Evaluate the identity of the requester and the conditions of the request. Authentication establishes an identity claim; authorization determines what that identity may do. A successful authentication does not automatically authorize an action.

For example, a payroll employee authenticates using multifactor authentication (MFA). The employee is entitled to read payroll reports but not change another employee's bank information. A separate authorization decision must protect that operation.

### Apply least privilege

Grant the minimum resource, action and duration necessary for a legitimate task. Least privilege applies to people, applications, service accounts and automated workloads.

A backup process might need read access to a database and write access to a backup repository. It does not necessarily need permission to delete the production database or remove existing immutable backups. Separate identities and narrowly scoped permissions reduce the consequences of compromise.

Just-in-time (JIT) access provides temporary privileges when needed. Just-enough access limits the scope of those privileges. For example, a database administrator receives permission to update one database for an approved maintenance window rather than permanent administrative access to every server.

### Assume breach

Design as though an adversary could already control an account, endpoint or workload. This is a planning assumption, not a declaration that every system is infected. Independent safeguards, monitoring and segmentation reduce the damage caused by a failed control.

### Continuous verification

Access decisions are not necessarily final for the entire session. New evidence can trigger reevaluation, including endpoint compromise alerts, account suspension, expired credentials or changed permissions. Reevaluation can be event-driven, periodic or tied to a sensitive operation. It does not mean prompting for a password on every packet.

| Principle | Example implementation | Limitation |
| --- | --- | --- |
| Verify explicitly | MFA and device certificates | Valid credentials may still be abused |
| Least privilege | Resource-specific read-only role | Poor role design creates excess permissions |
| Assume breach | Segmentation and endpoint detection | Permitted paths can still be abused |
| Continuous verification | Revoke sessions when device risk rises | Revocation speed depends on integration |

**Exam trap:** Zero Trust is not a single product, nor does it mean nobody can ever receive access. It means trust is conditional and explicitly evaluated.

## 4. Control plane and data plane

A plane describes a functional part of a system. Separating decision-making from traffic handling helps explain how security policies become actual restrictions.

The **control plane** manages how access and communication should occur. It evaluates policy and coordinates decisions that govern the data plane. In networking, routing information and firewall policy are familiar examples of control-plane functions.

The **data plane** handles actual communication: forwarding packets, transporting application requests and responses, and applying relevant traffic enforcement. In Zero Trust, a request to retrieve a document or query an application travels through an enforcement mechanism before reaching its destination.

```text
CONTROL PLANE
  Identity + device + risk signals
                |
                v
       Policy decision point
       [Engine + Administrator]
                |
           enforce decision
                v
DATA PLANE
  Subject --> Enforcement point --> Resource
```

For example, a remote accountant requests a financial application. Policy decision components determine whether the account and laptop satisfy the access policy. The enforcement component establishes or blocks the application connection. The application traffic itself belongs to the data plane.

| Control plane | Data plane |
| --- | --- |
| Decides and coordinates policy | Handles actual resource communication |
| Evaluates identity and risk information | Applies access decisions to traffic or requests |
| Authorizes establishment or termination of access | Permits or interrupts the relevant connection |

**Important limitation:** The planes are logical functions, not necessarily separate physical devices. A gateway may implement several functions in one product. Management interfaces must themselves be protected because unauthorized policy changes can affect many downstream resources.

🧠 **Memory hook:** Control plane decides the rules; data plane carries the traffic under those rules.

## 5. Adaptive identity and contextual trust

Adaptive identity means that identity-related security decisions respond to evidence surrounding the request rather than relying on a static username and password. An identity provider (IdP) may authenticate a subject and supply identity attributes; other systems contribute endpoint or risk evidence.

### Identity evidence

A subject can be a human or a non-human entity. Human evidence includes account status, role, authentication method and employment relationship. A workload may present a short-lived credential, signed token or certificate associated with a service identity.

### Device posture

Device posture describes the security condition of the endpoint making a request. Relevant signals may include enrollment, operating system patch status, disk encryption, endpoint protection status and a valid device credential. A managed device is not automatically compliant: enrollment establishes an administrative relationship, while compliance describes whether required conditions are satisfied.

An endpoint detection and response (EDR) system may report malicious behavior on a previously compliant machine. If that information reaches the decision system promptly, policy may revoke sensitive access and permit only remediation services.

### Context and resource sensitivity

Time, network source, unusual travel patterns, requested operation and data classification help determine the appropriate response. A location change can be legitimate, so context is evidence rather than conclusive proof of compromise. Sensitive actions may require stronger authentication even within an existing session.

| Signal | Example | Possible response |
| --- | --- | --- |
| Identity | Account disabled | Deny or revoke |
| Authentication | Password only for sensitive data | Require stronger authentication |
| Device | Laptop missing required patches | Restrict to remediation |
| Behavior | Unexpected bulk export | Block export and investigate |
| Resource | Public information page | Allow anonymous read if policy permits |
| Context | Unusual location | Evaluate risk and request additional evidence |

Step-up authentication requires stronger or fresher proof for a particular request. It cannot fix every problem: an infected endpoint remains infected even if its legitimate owner completes MFA.

**Exam tip:** Choose the control that addresses the failed condition. Stronger identity proof is appropriate for an authentication concern; device isolation or remediation is appropriate for a compromised endpoint.

## 6. Policy-driven access control and threat scope reduction

Policy-driven access control evaluates requests against explicit rules. Role-based access control (RBAC) grants permissions according to organizational roles. Attribute-based access control (ABAC) evaluates attributes of the subject, resource, action and environment. Zero Trust can use both rather than requiring one particular access model.

A payroll policy could require the payroll role, a compliant managed device, recent MFA and a low-risk session before granting read access. Exporting financial data could require separate authorization and approval. Conditions should be documented, tested and auditable.

```text
IF account is active
AND role permits requested action
AND device meets resource requirements
AND authentication meets required assurance
AND risk is acceptable
THEN allow the specified action
ELSE deny or require an explicitly permitted remediation step
```

This is illustrative pseudocode, not a configuration for a real product. Separate rules are needed for public resources, emergency access and non-human identities. An explicit denial should not be overridden accidentally by a broader allow rule. Default deny makes unrecognized requests fail safely rather than granting unrestricted access.

Threat scope reduction limits the resources exposed to a compromised subject. Instead of giving a contractor access to an entire subnet, grant access to one maintenance application. Microsegmentation creates granular boundaries between workloads or resource groups, reducing lateral movement. Network access control (NAC) can restrict devices joining a network, while application-level authorization restricts what those devices may actually do.

Security zones group resources with common protection requirements. A guest zone, employee zone and restricted database zone may have distinct connectivity policies. A zone label is not evidence that all requests within it are trustworthy. An internal connection still needs appropriate verification and authorization.

| Mechanism | Scope | Example |
| --- | --- | --- |
| VLAN segmentation | Network separation | Guest devices separated from staff |
| Microsegmentation | Fine-grained traffic boundaries | Only application servers reach a database port |
| Application authorization | Resource and operation | Read record but not export records |
| Temporary privilege | Time and task | Maintenance access expires after one hour |

**Important limitation:** Network segmentation cannot prevent every malicious operation over a permitted connection. Application permissions, data protection and monitoring remain necessary.

## 7. Policy engine, administrator and enforcement point

The logical model distinguishes three main responsibilities. The policy engine and policy administrator together form the **policy decision point (PDP)** in the NIST reference architecture.

| Component | Responsibility | Example |
| --- | --- | --- |
| Policy engine (PE) | Evaluates evidence and policy; decides grant, deny or revoke | Determines that a device no longer satisfies requirements |
| Policy administrator (PA) | Coordinates execution of the decision and session establishment or termination | Directs a gateway to terminate access |
| Policy enforcement point (PEP) | Enables, monitors and terminates the communication path according to policy | Gateway blocks requests to a protected application |

The policy engine may consume information from identity services, device management, threat intelligence, security analytics and resource inventories. The administrator translates its decision into operational instructions, potentially coordinating session credentials or access tokens. The enforcement point is positioned so relevant access cannot bypass it.

### Follow an access request

1. A subject requests a protected resource through the enforcement mechanism.
2. The decision infrastructure obtains relevant identity, device and resource information.
3. The policy engine evaluates the information against applicable rules.
4. The policy administrator coordinates implementation of the resulting decision.
5. The PEP permits or blocks the requested communication.
6. Monitoring and updated signals may trigger a new decision that modifies or revokes access.

This is a logical sequence, not a universal wire protocol. Products may cache decisions or combine components. A reverse proxy, endpoint agent, application gateway or service mesh may act as part of a PEP. Direct access to a backend that bypasses the gateway undermines the architecture; backend network and application controls should prevent unauthorized alternate paths.

**Exam trap:** The policy engine makes the decision. The policy administrator coordinates it. The enforcement point applies it. Do not confuse the policy administrator component with a human system administrator.

## 8. Session lifecycle, implementation and trade-offs

A session is an ongoing interaction authorized under certain conditions. Access tokens often encode or reference permissions, audience and expiration. A resource must validate tokens appropriately; merely possessing a token should not authorize arbitrary resources. Short lifetimes reduce exposure but do not guarantee immediate revocation. Some self-contained tokens remain accepted until expiry unless the resource checks revocation information or another enforcement mechanism terminates access.

Consider a user who logs in at 09:00 and whose account is disabled at 09:15. Blocking new logins is insufficient if an existing session remains active. The architecture needs a way to propagate changes and terminate or reevaluate existing sessions. Logging helps establish whether the expected revocation actually occurred.

### Deploy incrementally

Begin with an inventory of resources, identities, dependencies and permitted data flows. Prioritize sensitive applications, define precise access rules, test with a limited group, then expand coverage. Replace shared accounts with attributable identities where practical. Observe legitimate dependencies before enforcing restrictions that could interrupt critical services. Measure blocked unauthorized requests, excessive permissions, revocation latency and legitimate access failures.

Zero Trust Network Access (ZTNA) commonly provides policy-mediated access to specific applications instead of broad network connectivity. A virtual private network (VPN) creates a protected tunnel; its authorization scope depends on configuration. A VPN can be narrow and policy-aware, and ZTNA does not automatically replace every VPN use case. Both require sound identity and endpoint controls.

| Trade-off | Risk | Mitigation |
| --- | --- | --- |
| Strict device requirements | Legitimate users locked out | Enrollment and remediation paths |
| Centralized policy | Decision infrastructure outage | Redundancy and tested recovery |
| Frequent verification | User friction | Risk-based checks and secure session reuse |
| Extensive telemetry | Privacy and retention concerns | Collect necessary signals and restrict access |
| Legacy systems | Cannot consume modern identity signals | Protective gateway and network restrictions |

Fail-closed behavior denies access when required verification cannot be completed. This protects sensitive resources but may interrupt operations. Fail-open behavior permits access despite a failure and can create serious exposure. Critical healthcare or industrial environments may require narrowly defined emergency access with strong accountability, expiration and review instead of blanket fail-open access.

**Important limitation:** Zero Trust cannot eliminate software vulnerabilities, malicious authorized insiders or all data exfiltration. It complements defense in depth, secure development, backups and incident response.

## 9. Worked case study: A university research portal

A university hosts unpublished research in a cloud application. Researchers work from campus and home; external reviewers need access to assigned manuscripts. A legacy VPN currently gives authenticated users reachability to several internal systems. The university cannot replace all legacy infrastructure immediately.

The security team identifies three problems: reviewers have excessive network reach, researcher passwords may be stolen, and sessions remain active after account suspension. It designs a resource-specific gateway in front of the portal and restricts direct backend connections.

| Request | Evidence | Decision and reasoning |
| --- | --- | --- |
| Researcher reads own project | Active account, compliant device, MFA, project membership | Grant scoped read access |
| Reviewer requests assigned manuscript | Active temporary account, approved browser, MFA, assignment | Grant read-only access to that manuscript |
| Reviewer requests entire archive | Valid identity but no archive entitlement | Deny; authentication is not authorization |
| Researcher attempts export from compromised device | Valid identity, EDR high-risk alert | Revoke sensitive session; initiate investigation |
| Expired reviewer account reconnects | Valid old password, account expired | Deny |

The engine evaluates these conditions, the administrator coordinates each decision and the gateway enforces it. Application-level checks verify project membership on every relevant operation. The storage service accepts connections only from approved application workloads using separate service identities.

The team tests the policy by simulating a revoked account and checking whether existing sessions stop within its defined target. It also tests a direct backend request to confirm the gateway cannot be bypassed. Audit records include the subject, resource, action, time, decision and policy reason, without storing passwords or complete sensitive documents.

The design has costs: external reviewers need onboarding, legacy services require adapters, and policy outages can affect research deadlines. The team provides redundant gateways and a support process for legitimate device failures. It retains encrypted backups because restricted access does not prevent every destructive event.

**Takeaway:** Zero Trust changes the unit of access from broad network membership to an explicitly governed relationship between a subject, action and resource.

## 10. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Why is an internal IP address insufficient evidence of trust? | _Write here._ |
| Distinguish authentication from authorization. | _Write here._ |
| Explain continuous verification without repeated password prompts. | _Write here._ |
| Distinguish the control and data planes. | _Write here._ |
| Describe the PE, PA and PEP. | _Write here._ |
| Give three device posture signals. | _Write here._ |
| Explain threat scope reduction. | _Write here._ |
| Explain why token expiration alone may not achieve immediate revocation. | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Why is an internal IP address insufficient evidence of trust? | An internal system or account can be compromised; location does not prove identity, device health or authorization. |
| Distinguish authentication from authorization. | Authentication establishes identity; authorization determines permitted resources and actions. |
| Explain continuous verification without repeated password prompts. | Policies can reevaluate sessions when signals change or at defined intervals and require additional proof only when necessary. |
| Distinguish the control and data planes. | The control plane evaluates and coordinates policy; the data plane handles resource communication under enforced rules. |
| Describe the PE, PA and PEP. | The engine decides, the administrator coordinates execution, and the enforcement point permits or stops communication. |
| Give three device posture signals. | Patch compliance, disk encryption and endpoint protection health are examples. |
| Explain threat scope reduction. | Limit reachable resources and permitted actions so one compromise has restricted impact. |
| Explain why token expiration alone may not achieve immediate revocation. | An already-issued token may be accepted until expiration unless revocation checks or session termination are implemented. |

</details>

## 11. Hands-on lab: Map identity and device trust decisions

**Time:** 20–25 minutes  
**Tools:** Paper or a local text editor; optional spreadsheet.  
**Safety and scope:** All identities, events and resources below are fictional. No live systems, credentials or patient records are required.

### Scenario and policy

A clinic needs to protect its electronic health record (EHR) application, appointment system and maintenance portal. Use the following simplified rules in the listed priority order:

1. Disabled identities and confirmed compromised devices are denied protected access; existing protected sessions are revoked.
2. Every protected request requires a role with entitlement to the requested action.
3. EHR access requires a compliant managed device and MFA. Clinicians may read assigned patient records. Only a separately authorized records supervisor may export records.
4. Receptionists may edit appointments using compliant managed devices and MFA.
5. External technicians may access only the maintenance portal during their approved window with MFA and an approved device.
6. Missing MFA may trigger step-up if all other required conditions are met. Noncompliant devices may receive a separate remediation path, not protected application access.
7. Everything not explicitly allowed is denied.

### Step 1: Evaluate requests

Copy this table and complete the final column. Assume accounts are active and risk is low unless stated otherwise. All maintenance devices marked approved satisfy the relevant portal policy. Assigned records and export permissions are explicitly indicated.

| ID | Subject and request | Device | MFA | Additional information | Decision and reason |
| --- | --- | --- | --- | --- | --- |
| A | Clinician reads assigned record | Managed, compliant | Yes | Assigned patient | _Write here._ |
| B | Receptionist reads EHR | Managed, compliant | Yes | No EHR entitlement | _Write here._ |
| C | Clinician reads assigned record | Managed, noncompliant | Yes | Missing patches | _Write here._ |
| D | Receptionist edits appointment | Managed, compliant | No | Valid appointment role | _Write here._ |
| E | Technician opens maintenance portal | Approved | Yes | Outside approved window | _Write here._ |
| F | Clinician exports all records | Managed, compliant | Yes | No export entitlement | _Write here._ |
| G | Supervisor exports records | Managed, compliant | Yes | Explicit export authorization | _Write here._ |
| H | Clinician continues EHR session | Managed, compromised | Yes | EDR confirms compromise | _Write here._ |
| I | Technician opens maintenance portal | Approved | Yes | Within approved window | _Write here._ |
| J | Receptionist edits appointment | Managed, compliant | Yes | Account disabled | _Write here._ |

### Step 2: Draw the decision architecture

Place an identity provider, device management system and EDR system above the policy engine. Draw the administrator between the engine and an application gateway. Draw subjects connecting through the gateway to the three protected applications. Label the gateway as the PEP and identify the control and data planes. Add a backend restriction that prevents direct unauthorized access around the gateway.

### Step 3: Simulate a changing risk signal

At 10:00, request A succeeds. At 10:12, EDR confirms compromise of that same endpoint. At 10:13, the endpoint sends another EHR request. Describe how the new information should travel through the architecture and what happens to the previous session. Identify one operational reason that enforcement may be delayed and how to test it.

### Step 4: Interpret your design

Answer these questions:

1. Why does B fail even with MFA and a compliant device?
2. Why should C not simply receive another MFA prompt?
3. What is the difference between D and J?
4. How does the technician policy reduce threat scope?
5. Which component decides H, and which implements the traffic restriction?
6. What should the clinic do if its decision service becomes unavailable during an emergency?

<details markdown="1">
<summary>Reveal lab answers</summary>

| ID | Expected decision | Reason |
| --- | --- | --- |
| A | Allow scoped read | All EHR conditions satisfied |
| B | Deny | No EHR entitlement |
| C | Deny EHR; offer remediation | Device compliance requirement failed |
| D | Step-up, then reevaluate | Missing MFA is the only stated failed condition |
| E | Deny | Maintenance window expired or not started |
| F | Deny export | No export entitlement |
| G | Allow authorized export | Supervisor satisfies stated policy |
| H | Revoke and block | Confirmed compromise overrides previous access |
| I | Allow maintenance portal only | Required role, device, MFA and window satisfied |
| J | Deny; revoke existing sessions if present | Disabled account overrides other positive signals |

Expected architecture: identity and device evidence feeds the policy engine; the engine and administrator compose the decision point; the administrator coordinates gateway enforcement. Subjects communicate with protected applications through the gateway in the data plane. Backend restrictions prevent bypass.

At 10:12 the compromise event must reach decision infrastructure, the engine must reassess the session, and the administrator must coordinate revocation at the gateway. The 10:13 request should fail once revocation is enforced. Delays may arise from polling intervals, cached decisions or disconnected integrations. Test using synthetic events and compare event, decision and termination timestamps; investigate any latency exceeding the organization's target.

1. B establishes identity but lacks authorization for the resource.
2. C fails device policy; stronger identity proof does not install patches or restore compliance.
3. D can satisfy the missing authentication condition; J has an explicitly disabled identity and must not be allowed merely by completing MFA.
4. The technician can reach one application during a defined interval rather than the entire clinic network.
5. The policy engine decides; the administrator coordinates the instruction; the PEP enforces it.
6. Use redundant decision infrastructure and a preplanned, narrowly scoped emergency access procedure with accountability, expiration and retrospective review. Do not automatically allow every request.

</details>

## 12. Eight original Security+-style questions

These are original practice questions, not actual CompTIA exam items.

### Q1. An employee connects from the corporate LAN. Which approach BEST reflects Zero Trust?

A. Allow access to all internal applications.  
B. Treat the source IP as sufficient proof of identity.  
C. Evaluate identity, device posture and resource authorization.  
D. Disable all internal network communication.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Access depends on explicit evidence and policy. A and B rely on implicit network trust. D prevents legitimate operation rather than implementing conditional access.

</details>

### Q2. Which component evaluates policy and determines whether access should be revoked?

A. Policy engine  
B. Policy enforcement point  
C. Data plane  
D. Policy administrator

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** The engine makes the decision. B applies decisions to communication, C describes actual traffic handling, and D coordinates implementation.

</details>

### Q3. A gateway forwards authorized application requests to a server. Which functional plane handles these requests?

A. Control plane  
B. Identity directory  
C. Policy engine  
D. Data plane

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Actual application communication belongs to the data plane. A governs access, B stores identity information, and C evaluates policy rather than forwarding the requests.

</details>

### Q4. A user completes MFA, but endpoint protection reports confirmed malware. Which response BEST addresses the new evidence?

A. Trust the device until the user logs out.  
B. Revoke sensitive access and restrict the device to approved remediation.  
C. Require a longer password and preserve the session.  
D. Add the user to an administrator group.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Changed device risk warrants reevaluation and containment. A ignores new evidence, C addresses passwords rather than malware, and D increases potential damage.

</details>

### Q5. A vendor needs to maintain one application for two hours. Which design MOST directly reduces threat scope?

A. Permanent administrator access through a VPN  
B. Shared credentials for all maintenance staff  
C. Temporary access restricted to the application and required actions  
D. Unrestricted access from the vendor's registered IP address

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Resource, action and time restrictions implement least privilege. A and D grant excessive reach; B also undermines individual accountability.

</details>

### Q6. Which component coordinates the establishment or termination of a session based on the policy engine's decision?

A. Policy administrator  
B. Resource owner exclusively  
C. Network switch data plane  
D. Endpoint vulnerability scanner

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** The administrator operationalizes the decision and communicates with enforcement. B may define requirements but is not the specified logical component; C handles traffic and D supplies vulnerability evidence.

</details>

### Q7. An organization disables an account, but a previously issued token remains usable. Which architectural improvement BEST addresses this issue?

A. Increase token lifetime.  
B. Place the application on the internal LAN.  
C. Require users to change their passwords annually.  
D. Integrate session revocation or token reevaluation with enforcement.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Existing access must be reevaluated or terminated. A increases exposure; B changes location without solving authorization; C does not invalidate existing sessions by itself.

</details>

### Q8. Which statement BEST describes the relationship between Zero Trust and defense in depth?

A. Zero Trust removes the need for backups and firewalls.  
B. Zero Trust complements layered controls by making access conditional and limiting privilege.  
C. Defense in depth requires every internal resource to trust every employee.  
D. Zero Trust is achieved solely by enabling MFA.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Conditional access is one part of a layered security strategy. A discards essential controls, C misrepresents defense in depth, and D confuses one authentication safeguard with an architecture.

</details>

**Score guide:** 7–8 correct: strong understanding; 5–6: revisit Sections 3–8, particularly the roles and distinctions you missed; 0–4: repeat Sections 10–11 and retry the questions.

## 13. Discussion challenge

A law firm employs attorneys, paralegals and external translators. Some attorneys travel with managed laptops; translators use personal devices. Confidential legal documents are stored in a cloud application, and the firm's managing partner wants convenient access from any device. The firm has a limited budget and cannot replace its legacy document system this year.

Design an initial Zero Trust architecture. Explain which resources and actions each role should access, what identity and device evidence should be required, where enforcement should occur and how access changes when a contract ends or a laptop is compromised. Explain which controls you would prioritize first and which risks remain. Consider whether a restricted browser-based workflow could meet a translator's legitimate needs without granting broad download access.

**Bonus:** Explain how your design protects confidentiality and integrity without creating unacceptable availability risks during a court filing deadline.

## 14. Study from the Treadmill 🏃

**Direct links to specific videos:**

- [Professor Messer — Zero Trust (SY0-701, 1.2)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/zero-trust-sy0-701/): Focus on the planes of operation, adaptive identity and policy decision/enforcement roles.
- [Professor Messer — Identity and Access Management (SY0-701, 4.6)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/identity-and-access-management-sy0-701/): Focus on identity lifecycles, permission assignments and authentication versus authorization.

**Active-listening prompts:**

- Which component decides whether a request is allowed, and which enforces that decision?
- Why does successful authentication not establish unrestricted trust?
- How should access change when a user's employment or device health changes?

**Further reading:** NIST SP 800-207, *Zero Trust Architecture*, provides the reference architecture and its logical components. The official SY0-701 objective document should be used to confirm exam scope; official document verification remains pending editorial review.

## 15. One-minute recap

- **Zero Trust:** No automatic trust based solely on network location or ownership.
- **Least privilege:** Limit resources, operations and duration to legitimate needs.
- **Adaptive identity:** Use identity, device and contextual evidence to evaluate requests.
- **Control plane:** Evaluates and coordinates policy; **data plane:** handles actual communication.
- **PE decides; PA coordinates; PEP enforces.**
- **Continuous verification:** Reevaluate and revoke access as relevant conditions change.
- **Threat scope reduction:** Restrict lateral movement and the impact of compromised identities or devices.

**Golden rule:** A successful login is evidence of identity, not permission to do everything.

**Next:** Day 38 — Topic pending curriculum confirmation.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
