# Day 27 — Attack Surfaces and Vectors

**Security+ Study Academy | Week 06: Threats, Vulnerabilities, and Mitigations**  
**File:** `docs/week-06/day-27-attack-surfaces-and-vectors.md`  
**Prerequisite:** [Day 26 — Threat Actors and Motivations](day-26-threat-actors-and-motivations.md)  
**Estimated study time:** 85–110 minutes

## Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish an **attack surface**, an **entry point**, an **attack vector**, and an **attack path**.
2. Identify physical, network, application, cloud, wireless, human, and third-party exposures.
3. Classify common vectors including phishing, malicious files, exposed services, removable media, and supply-chain compromise.
4. Model how an attacker could move from an initial entry point toward a valuable asset.
5. Prioritize exposure-reduction measures based on likelihood, impact, and available evidence.

## 1. Four concepts that sound similar
Security teams need to know **what is exposed**, **how an attacker might enter**, and **what could happen next**. These concepts describe different parts of the same problem.

| Term | Meaning | Example |
| --- | --- | --- |
| **Attack surface** | All reachable or exploitable opportunities across an environment | Public web apps, employee accounts, Wi-Fi, office doors, vendor integrations |
| **Entry point** | Specific place where access could begin | A remote-access portal or employee inbox |
| **Attack vector** | Method or route used to attempt compromise | Credential phishing against that inbox |
| **Attack path** | Sequence of steps or relationships leading toward a target | Stolen password → VPN login → file share → confidential documents |

A **vulnerability** is a weakness, such as a missing security patch or excessive permissions. A **threat** is a potential source of harm. A **control** reduces risk by preventing, detecting, or limiting attacks. A **valuable asset** is something the organization needs to protect.

> **Exam distinction:** A public login page is an exposed entry point. A phishing email is an attack vector. An unpatched authentication flaw is a vulnerability. The route from initial access to data theft is an attack path.

<details markdown="1">
<summary>Reveal quick check: classify the four concepts</summary>

A company has a public customer portal (**attack surface**). Its login form is a possible **entry point**. An attacker tries password spraying (**attack vector**) and, after compromising an account, accesses sensitive reports (**attack path**).

</details>

## 2. What belongs on an attack surface?
An attack surface is broader than the list of internet-facing IP addresses. It includes technology, people, processes, and physical environments. An asset can be internal and still exposed to a malicious insider or an attacker who has already gained access.

### Network and infrastructure
Examples include public IP addresses, exposed management interfaces, VPN gateways, open ports, remote desktop services, DNS records, firewalls, and misconfigured network devices.

**Example:** An administrative interface is reachable from the public internet even though only internal administrators need it.

**Reduce exposure:** Restrict management access, close unused ports, patch services, segment networks, and monitor configuration drift.

### Applications and APIs
Websites, mobile backends, APIs, authentication endpoints, plugins, and file-upload functions all add possible entry points. A properly authenticated API is still part of the attack surface; authentication reduces risk but does not make the interface disappear.

**Example:** An API endpoint returns more customer data than the requesting user is authorized to view.

**Reduce exposure:** Enforce server-side authorization, validate inputs, rate-limit sensitive endpoints, inventory APIs, and test access controls.

### Cloud, SaaS, and identity
Cloud storage permissions, identity providers, OAuth grants, service accounts, shared links, cloud management consoles, and SaaS integrations create exposures even when the organization owns no public server.

**Example:** A cloud storage bucket unintentionally allows anonymous reads.

**Reduce exposure:** Review permissions, enforce least privilege, protect administrative accounts, rotate exposed secrets, and audit integrations.

### Wireless and mobile
Wireless access points, guest networks, Bluetooth, mobile devices, and insecure device enrollment can provide proximity-based opportunities. Connect this to [Day 25 — Wireless and Network Access Control](../week-05/day-25-wireless-and-network-access-control.md).

**Example:** Guest Wi-Fi can reach internal printers and staff workstations because network isolation is missing.

**Reduce exposure:** Segment guest traffic, use appropriate Wi-Fi authentication, manage endpoints, and restrict device-to-device access where needed.

### Human and organizational
Employees, contractors, help desks, password-reset workflows, public staff directories, and business processes are also part of the attack surface.

**Example:** A caller impersonates a manager and pressures the help desk into resetting MFA enrollment.

**Reduce exposure:** Verify identities through approved channels, train staff to report suspicious requests, and add controls to high-risk recovery workflows.

### Physical and third-party
Doors, unattended devices, removable media, delivery areas, vendors, software dependencies, and outsourced services can provide routes to compromise.

**Example:** A contractor's support account retains broad access after the contract ends.

**Reduce exposure:** Apply visitor controls, disable unused vendor access, monitor privileged sessions, and review software and service dependencies.

## 3. Attack vectors: how attempts begin
Classify vectors by **how the attacker attempts to gain access**, not by an assumed actor identity. The same actor may use several vectors in one campaign.

| Vector | Typical mechanism | Illustrative entry point | Primary defensive ideas |
| --- | --- | --- | --- |
| **Phishing / social engineering** | Deceptive message or request | Email, SMS, voice call, chat | Verification, phishing-resistant MFA, reporting |
| **Credential attacks** | Reuse, guessing, spraying, stolen secrets | Login portal, VPN, SaaS | MFA, rate limiting, monitoring, password hygiene |
| **Exploitation of vulnerable services** | Abuse of software weakness | Public web app or remote service | Patching, exposure reduction, secure configuration |
| **Malicious attachments / links** | User opens or runs harmful content | Inbox or browser | Filtering, endpoint protection, user awareness |
| **Removable media** | Malicious or unauthorized device/content | USB port or workstation | Device policy, endpoint controls, physical awareness |
| **Wireless attacks** | Abuse of wireless exposure or trust | Wi-Fi or nearby device | Strong encryption, segmentation, approved networks |
| **Supply-chain / third-party compromise** | Trusted vendor, update, or integration is abused | Software dependency or vendor access | Vendor review, least privilege, monitoring, provenance |
| **Physical intrusion** | Direct access to facilities or hardware | Office, server room, unattended laptop | Locks, badges, encryption, screen locking |

### Email is not always the whole vector
A message containing a link to a fake sign-in page can combine **phishing**, **credential theft**, and **account takeover**. Describe each stage accurately instead of forcing an entire incident into a single label.

### Commonly confused attack terms
- **Password spraying:** Tries a small number of common passwords across many accounts; may avoid rapid lockouts.
- **Credential stuffing:** Tests username/password pairs obtained from another breach.
- **Brute force:** Systematically attempts many candidate secrets against an account or system.
- **Typosquatting:** Uses a lookalike domain to exploit mistyped addresses.
- **Watering-hole attack:** Compromises a site frequently visited by the intended targets.
- **Drive-by compromise:** Exploits a visitor through malicious web content, often without an intentional download.
- **Supply-chain attack:** Reaches a target through a supplier, dependency, or trusted delivery mechanism.

<details markdown="1">
<summary>Reveal quick check: spraying versus stuffing</summary>

Trying `Winter2026!` against hundreds of employee accounts is **password spraying**. Trying hundreds of username/password pairs leaked from another service is **credential stuffing**. Both target authentication but use different input patterns.

</details>

## 4. Build an attack surface inventory
An inventory turns an abstract concept into something a defender can review. Include assets that are internet-facing **and** internal assets reachable after compromise.

| Field | What to record | Example |
| --- | --- | --- |
| Asset or process | What exists? | Client document portal |
| Business owner | Who is responsible? | Operations manager |
| Exposure | Who can reach it? | Public internet |
| Entry points | Where could access begin? | Login and file-upload pages |
| Trust boundary | What separation must be crossed? | External user → authenticated client |
| Data or function | What is at stake? | Confidential client files |
| Existing controls | What reduces risk? | MFA, role-based access, logs |
| Known gaps | What needs verification? | Old test accounts may remain active |
| Priority | What should be addressed first? | High pending account review |

**Trust boundary:** A point where data or access crosses between different levels of trust, such as the public internet and a private application, or a standard user and an administrator.

**Do not assume:** An open port is not automatically exploitable; a closed port does not mean an organization has no attack surface. Inventory and assess the surrounding configuration.

## 5. Model an attack path
A useful path model connects an initial condition to a business consequence. The path is a **hypothesis to validate**, not proof that an attack occurred.

```text
External attacker
      |
      v
Phishing message sent to employee
      |
      v
Employee enters credentials on fake site
      |
      v
Attacker attempts SaaS account login
      |
      v
Weak or absent MFA permits access
      |
      v
Overly broad file permissions
      |
      v
Confidential client documents exposed
```

Each arrow represents a dependency or step. Ask: **What must be true for the attacker to proceed?** If phishing-resistant MFA blocks the stolen credential, that path may stop. If access is limited to one project, the potential impact may shrink.

### Initial access, movement, and impact
1. **Initial access:** The attacker reaches a system or account through an entry point.
2. **Privilege escalation:** The attacker obtains permissions beyond the initial access, if possible.
3. **Lateral movement:** The attacker attempts to reach additional systems or accounts.
4. **Collection and exfiltration:** The attacker accesses and possibly transfers data outside authorized boundaries.
5. **Impact:** The organization suffers loss, disruption, fraud, or another consequence.

These stages are **possible**, not mandatory. Some incidents begin with a privileged insider or affect data without lateral movement.

<details markdown="1">
<summary>Reveal quick check: what breaks the path?</summary>

If stolen credentials are insufficient to pass phishing-resistant MFA, the attacker may be unable to sign in. If the account is compromised anyway, least-privilege permissions can limit accessible files. Defense in depth means multiple controls can interrupt or constrain the path.

</details>

## 6. External versus internal exposure
An **external attack surface** is reachable from outside the organization's trusted environment, such as public web services or cloud sign-in pages. An **internal attack surface** is accessible from inside a network or to identities already granted some access.

**Example:** A database is not directly exposed to the internet. However, if a compromised application server can connect to it with excessive privileges, the database remains part of a potential attack path.

Do not confuse **internet-facing** with **high risk** or **internal** with **safe**. Evaluate authentication, authorization, sensitivity, configuration, and reachable dependencies.

## 7. Prioritize exposure reduction
A practical priority decision considers:

- **Reachability:** Can an attacker reach the entry point easily?
- **Exploitability:** Is there a credible, usable weakness?
- **Business impact:** What would happen if the asset were compromised?
- **Privileges and dependencies:** Could access lead to more valuable systems?
- **Existing controls and detection:** What could prevent or reveal misuse?
- **Evidence and uncertainty:** What is verified, and what still needs testing?

### Example priority table
| Finding | Reasoning | Illustrative priority |
| --- | --- | --- |
| Public administrator portal without MFA | Exposed privileged access and weak authentication | High |
| Unused internal test server with limited access | Needs review, but lower immediate reachability | Medium |
| Public marketing page with current patches and limited functions | Exposed but no demonstrated critical weakness | Lower, monitor |
| Vendor account with broad access after contract ended | Unnecessary permissions create a strong attack path | High |

These priorities are **illustrative**, not universal. A real assessment should incorporate organizational context, verified weaknesses, and current threat information.

### Reduce the surface, harden the surface, monitor the surface
**Reduce:** Remove unused accounts, services, APIs, firewall rules, and vendor integrations.  
**Harden:** Patch, configure securely, enforce MFA and least privilege, and segment access.  
**Monitor:** Log authentication and access, review changes, detect anomalies, and test controls.

## 8. Practical lab: map a small law firm's attack surface
**Scenario:** A 20-person law firm uses Microsoft 365 for email and documents, a public client portal, an office Wi-Fi network with a guest SSID, managed laptops, and an outsourced IT provider. Staff sometimes exchange files using personal cloud links. The firm holds confidential case files.

**Your task:** Build an attack surface model without scanning, probing, or accessing any real system. Use only the scenario details.

### Part A — Identify assets and entry points
Create a table with at least **six** rows. Include the email service, client portal, guest Wi-Fi, laptops, outsourced IT access, and personal cloud links. For each, record:

1. The asset or process.
2. Who might reach it.
3. One possible entry point.
4. One credible vector.
5. The data or business process at risk.
6. One preventive or detective control.

### Part B — Draw two hypothetical paths
Create one **identity-based path** and one **third-party or network-based path**. Label each step as either a **scenario fact** or an **assumption requiring verification**.

### Part C — Choose the first three actions
Prioritize three improvements. Explain the expected risk reduction, not just the name of the security tool.

<details markdown="1">
<summary>Reveal sample attack surface inventory</summary>

| Asset/process | Exposure and entry point | Plausible vector | Control |
| --- | --- | --- | --- |
| Microsoft 365 | Internet-accessible account sign-in | Credential phishing | Phishing-resistant MFA and sign-in monitoring |
| Client portal | Public web login and upload | Exploitation of application flaw | Patching, access testing, secure upload handling |
| Guest Wi-Fi | Visitors within wireless range | Unauthorized internal access if poorly segmented | Isolate guest and staff networks |
| Staff laptops | Users and local interfaces | Malicious attachments or removable media | Endpoint controls and patching |
| Outsourced IT | Vendor remote-support account | Stolen vendor credentials | Least privilege, MFA, access reviews |
| Personal cloud links | External sharing workflow | Oversharing or unauthorized access | Approved sharing platform and permissions review |

**Important:** The scenario does not establish that any of these weaknesses actually exists. The table identifies areas to validate.

</details>

<details markdown="1">
<summary>Reveal sample attack paths and priorities</summary>

**Identity path:** Phishing message → employee discloses credentials *(assumption)* → attacker signs in if MFA is ineffective *(assumption)* → attacker reaches documents if permissions allow *(assumption)*.

**Third-party path:** Outsourced IT account exists *(fact)* → vendor account is compromised *(assumption)* → attacker gains remote access if permissions permit *(assumption)* → attacker reaches sensitive systems if segmentation is inadequate *(assumption)*.

**Possible first actions:** (1) Verify MFA and privileged access on all staff and vendor identities; (2) review sharing permissions and replace personal file-sharing workflows; (3) confirm guest Wi-Fi isolation and review client portal access controls. Reprioritize when evidence reveals actual exposures.

</details>

### Lab deliverable: threat-aware surface model
Submit an inventory, two labeled paths, and a short paragraph explaining how the threat actor profile from [Day 26](day-26-threat-actors-and-motivations.md) could change which exposures deserve attention.

## 9. Security+ knowledge check
Choose the **single best answer** for each question before opening the explanation.

### Question 1
Which description best defines an **attack surface**?

A. The identity of the attacker

B. The complete set of opportunities through which a system or organization could be attacked

C. Only the list of confirmed software vulnerabilities

D. The financial damage caused by an incident

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** An attack surface includes exposed systems, identities, processes, people, and physical opportunities, not just known vulnerabilities.

</details>

### Question 2
A public VPN login page is most directly an example of which concept?

A. An entry point

B. Data exfiltration

C. A threat actor motivation

D. A completed attack path

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** The login page is a place where an attacker might attempt access. The method used against it would be the vector.

</details>

### Question 3
An attacker tests username/password pairs from a previous breach against a company portal. Which vector is this?

A. Password spraying

B. Credential stuffing

C. Physical tailgating

D. Watering-hole attack

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Credential stuffing reuses breached credential pairs. Password spraying typically tries a small number of passwords against many accounts.

</details>

### Question 4
Which condition most clearly increases an **internal** attack surface?

A. Disabling unused internal services

B. Removing expired contractor accounts

C. Giving all staff unrestricted access to shared servers

D. Restricting administrative access to dedicated devices

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Broad permissions expand what a compromised or malicious internal identity can reach.

</details>

### Question 5
A user receives a fake help-desk message requesting an MFA reset. What is the primary initial vector?

A. Social engineering

B. Wireless jamming

C. SQL injection

D. Physical theft

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** The attacker manipulates a person or workflow to obtain access.

</details>

### Question 6
Which sequence is the best example of an **attack path**?

A. Laptop, switch, router, printer

B. Confidentiality, integrity, availability

C. Stolen credentials → cloud login → excessive permissions → sensitive file access

D. Phishing, ransomware, hacktivism, espionage

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** It connects an initial access method through dependencies to a target asset.

</details>

### Question 7
A vendor account retains administrative access after a support contract ends. Which action most directly **reduces** the attack surface?

A. Add a warning banner to the login page

B. Disable the unused vendor account

C. Increase the size of log storage

D. Rename the administrator account

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Removing unnecessary access eliminates an available entry point rather than merely monitoring it.

</details>

### Question 8
A database is not directly reachable from the internet. Which statement is most accurate?

A. It cannot be part of an attack path

B. It is automatically secure

C. It may still be reachable through a compromised application or privileged identity

D. It requires no access controls

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Internal dependencies and permissions can expose assets after initial compromise.

</details>

### Question 9
Which is the best example of a **supply-chain attack vector**?

A. An employee forgets to lock a workstation

B. A trusted software update channel delivers malicious code after the supplier is compromised

C. A user mistypes a password once

D. A company blocks an unused port

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** A compromised supplier or trusted distribution process can become the route to the target.

</details>

### Question 10
An analyst draws an attack path but has not confirmed that MFA is disabled or that file permissions are excessive. How should these steps be described?

A. Proven facts because the path is technically possible

B. Irrelevant details that can be omitted

C. Assumptions requiring validation

D. Evidence that a nation-state actor is involved

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Threat modeling explores plausible paths; it does not establish that their prerequisites exist or that an incident occurred.

</details>

## 10. Review and self-assessment
Before moving to Day 28, confirm that you can:

- [ ] Explain attack surface, entry point, vector, path, and vulnerability without conflating them.
- [ ] Identify external and internal exposure across technical and human systems.
- [ ] Distinguish credential stuffing from password spraying.
- [ ] Explain phishing, removable-media, wireless, and supply-chain vectors.
- [ ] Build an attack surface inventory and model two plausible paths.
- [ ] Mark assumptions separately from confirmed facts.
- [ ] Recommend controls that reduce, harden, or monitor exposure.

**Suggested score:** 8–10 correct = ready to advance; 6–7 = revisit the vector table and attack path; 0–5 = review the core concepts and repeat the lab.

### Key terms
**Attack surface:** Set of opportunities through which an asset or organization may be attacked.  
**Entry point:** Specific access location or interface.  
**Attack vector:** Method or route used to attempt compromise.  
**Attack path:** Sequence connecting initial access to a target or impact.  
**Trust boundary:** Separation between different trust or privilege levels.  
**Credential stuffing:** Testing breached username/password pairs.  
**Password spraying:** Testing a small set of passwords across many accounts.  
**Supply-chain attack:** Compromise through a supplier, dependency, or trusted delivery mechanism.  
**Lateral movement:** Attempt to access additional systems or accounts after initial compromise.  
**Attack surface reduction:** Removal of unnecessary services, interfaces, accounts, or permissions.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.  

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
