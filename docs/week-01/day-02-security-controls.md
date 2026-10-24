[SecurityPlus_Lesson_02_Security_Controls.md](https://github.com/user-attachments/files/33914849/SecurityPlus_Lesson_02_Security_Controls.md)
# Day 02 — Security Controls: Categories, Functions, and Defense in Depth

> **CompTIA Security+ SY0-701** · Domain 1 · Objective 1.1  
> **Level:** Beginner · **Time:** 45–65 minutes plus optional video  
> **Prerequisite:** [Day 01 — CIA Triad](day-01-cia-triad.md)

Welcome back! Yesterday we learned *what* security protects: confidentiality, integrity, and availability. Today we learn *how* to protect them.

## Learning objectives

After this chapter, you should be able to:

1. Name the four security-control **categories** and six **functions**.
2. Classify a control by both category and function.
3. Explain why one control may perform multiple functions.
4. Design basic **defense in depth** for a small organization.
5. Select the BEST control in Security+-style scenarios.

## 1. The big idea: The museum heist

Imagine a museum protecting a priceless painting. It has a locked case, motion sensor, visible guards, written visitor rules, and a plan to repair damage. These safeguards are **security controls**: measures intended to reduce risk. No single one is perfect.

A Security+ question may ask **what kind** of control something is (its *category*) or **what it does** (its *function*). These are different dimensions.

| Dimension | Question | Example |
| --- | --- | --- |
| Category | What kind of safeguard is it? | Firewall = **technical** |
| Function | What does it accomplish? | Firewall blocking traffic = **preventive** |

**Memory hook:** Category is a tool's department; function is its job description. A camera can be physical equipment *and* perform a detective function.

## 2. The four control categories

### Technical

Technology-enforced safeguards: **firewalls, encryption, multifactor authentication (MFA), access-control lists (ACLs), endpoint protection, intrusion detection systems (IDS)**. They operate quickly and consistently, but misconfiguration and software flaws can undermine them.

**Example:** A law firm's document system restricts payroll files to authorized HR accounts.

### Managerial

Controls for **governing and evaluating** security: risk assessments, policies, vendor reviews, and security program planning. Some other frameworks call these *administrative* controls; know the SY0-701 term **managerial**.

**Example:** Leadership requires a risk review before buying a new case-management platform.

### Operational

Controls carried out through **people and routine processes**: security awareness training, guard patrols, offboarding procedures, and manual reviews.

**Example:** IT follows a checklist to disable accounts and retrieve badges when employees leave.

### Physical

Controls that protect **physical spaces and equipment**: locks, fences, gates, bollards, barriers, and surveillance cameras.

**Example:** A locked server room reduces unauthorized physical access.

> **Nuance:** Categories can overlap in practice. A guard's patrol is an operational activity; a door lock is physical; a camera is physical equipment that can feed a technical monitoring system. Read the question's emphasis.

## 3. The six control functions

| Function | Main job | Example |
| --- | --- | --- |
| **Preventive** | Block unwanted actions | Firewall denies an unauthorized connection |
| **Detective** | Discover activity or incidents | IDS alerts on suspicious traffic |
| **Corrective** | Address or repair damage | Restore corrupted files from a clean backup |
| **Deterrent** | Discourage misconduct | Visible guard or surveillance warning |
| **Directive** | Specify required behavior | Policy requiring phishing reports |
| **Compensating** | Substitute when preferred control isn't feasible | Isolate an unpatchable legacy server |

### IDS versus IPS

An **intrusion detection system (IDS)** watches and alerts. An **intrusion prevention system (IPS)** can actively block malicious traffic. That makes detection and prevention different **functions**, even though both may be technical systems.

### Corrective versus compensating

**Corrective** answers: *How do we fix the damage?* **Compensating** answers: *What alternative protects us because the preferred safeguard isn't available?*

Network segmentation around an unpatchable server can be **technical**, **preventive**, and **compensating** at the same time. It does **not** remove the underlying vulnerability.

### Deterrent versus directive

A visible guard may discourage someone from attempting a break-in (**deterrent**). A policy telling employees to wear identification badges communicates expected behavior (**directive**). One measure may have multiple effects, but questions often ask for the **primary** one.

## 4. Under the hood: One control, multiple labels

Consider a firewall that denies remote desktop traffic from the internet and logs repeated connection attempts.

- **Category:** Technical.
- **Preventive function:** It blocks unauthorized connections.
- **Detective function:** It logs or alerts on suspicious attempts.
- **CIA link:** It may reduce unauthorized access affecting confidentiality and integrity.

**Exam tip:** Avoid memorizing rigid equations such as “firewall = only preventive.” Instead, identify the **behavior described** and whether the question asks for a category or function.

## 5. Defense in depth: Don't trust one lock

**Defense in depth** means combining complementary safeguards so a single failure does not immediately compromise the system.

Imagine a phishing email containing malware:

1. An email filter might block it (**technical, preventive**).
2. Awareness training might help an employee recognize it (**operational**, preventive in effect).
3. Endpoint detection might alert on malicious behavior (**technical, detective**).
4. Least-privilege permissions may limit damage (**technical, preventive**).
5. Tested backups and recovery procedures help restore operations (**corrective when used**).

Every layer has limits. Filters miss messages, people make mistakes, monitoring creates false positives, and backups fail if they are never tested.

**Memory hook:** Cybersecurity is an ensemble cast. If Firewall Man misses the villain, Detective Logs and Backup Woman still have lines. 🦸

| CIA objective | Examples of layered protection |
| --- | --- |
| Confidentiality | Permissions + MFA + encryption + monitoring |
| Integrity | Restricted write access + change approval + file integrity monitoring |
| Availability | Redundancy + incident response + tested backups |

## 6. Worked case study: The unpatchable law-firm server

A legal clinic depends on an old case-management application. Its vendor no longer issues security patches, but an immediate replacement would interrupt critical services.

**Preferred long-term response:** Migrate to a supported system.

**Interim controls:**

| Action | Category | Function |
| --- | --- | --- |
| Restrict server access to approved networks | Technical | Preventive; compensating |
| Alert on unusual logins | Technical | Detective |
| Assign risk owner and migration deadline | Managerial | Governance/directive |
| Train staff to report unusual access requests | Operational | Preventive in effect |
| Restore from tested clean backups after damage | Operational/technical | Corrective |

**Important:** Isolation narrows exposure; it does not patch the software. Risk should be documented and reviewed while migration proceeds.

**Think like an analyst:** How would the plan change if the old application had to be accessible from the public internet?

## 7. Guided notes: Write first, reveal later

Fill in the table without looking back. Then expand the completed reference table.

| Prompt | Your notes |
| --- | --- |
| Category versus function? | _Write your answer._ |
| Four categories, one example each? | _Write your answer._ |
| Six functions? | _Write your answer._ |
| IDS versus IPS? | _Write your answer._ |
| Why can segmentation be compensating and preventive? | _Write your answer._ |
| Why use defense in depth? | _Write your answer._ |

<details>
<summary><strong>Reveal the completed notes table (sample answers)</strong></summary>

| Prompt | Sample answer |
| --- | --- |
| Category versus function? | **Category** describes the kind of safeguard; **function** describes what it accomplishes. |
| Four categories? | **Technical:** firewall. **Managerial:** risk assessment. **Operational:** security training. **Physical:** door lock. |
| Six functions? | **Preventive, detective, corrective, deterrent, directive, compensating.** |
| IDS versus IPS? | An **IDS** detects and alerts; an **IPS** can actively block. |
| Segmentation? | It **prevents** some unwanted access and can **compensate** for inability to patch an old system. |
| Defense in depth? | Complementary controls provide multiple chances to prevent, detect, limit, and recover from an attack. |

</details>

## 8. Optional hands-on exercise: Build a control matrix (15 minutes)

In a notebook or spreadsheet, create columns: **Asset | Threat | Control | Category | Function | Limitation**. Choose two controls for each asset: (1) a staff laptop, (2) a client portal, (3) a paper-records cabinet.

<details>
<summary><strong>Reveal an example control matrix</strong></summary>

| Asset | Threat | Control | Category | Function | Limitation |
| --- | --- | --- | --- | --- | --- |
| Laptop | Theft | Full-disk encryption | Technical | Preventive | An unlocked session may expose files |
| Laptop | Account compromise | MFA | Technical | Preventive | Some MFA can be phished |
| Portal | Malicious requests | Web application firewall | Technical | Preventive | Rules can miss new attacks |
| Portal | Suspicious activity | Central alerts | Technical | Detective | Alerts require investigation |
| Paper files | Unauthorized access | Locked cabinet | Physical | Preventive | Keys may be stolen |
| Paper files | Unexplained access | Check-out log | Operational | Detective/accountability | Entries may be incomplete |

</details>

## 9. Practice: Original Security+-style questions

Choose the **BEST** answer before revealing each explanation. These are original educational questions, not actual exam items.

### Q1
An appliance alerts on suspicious network traffic but does not block packets. Its function is:

A. Preventive  
B. Detective  
C. Corrective  
D. Compensating

<details><summary>Reveal answer and explanation</summary>

**B — Detective.** Alerting discovers suspicious activity. **A** requires blocking; **C** repairs after an incident; **D** is an alternative to an unavailable preferred safeguard.
</details>

### Q2
Bollards prevent vehicles from reaching a data center entrance. What **category** are they?

A. Managerial  
B. Operational  
C. Physical  
D. Technical

<details><summary>Reveal answer and explanation</summary>

**C — Physical.** Bollards are barriers. Their **function** is preventive, but the question asks for **category**.
</details>

### Q3
A medical device cannot be patched, so IT restricts it to an isolated network. Why is this **compensating**?

A. It fixes the vulnerability.  
B. It substitutes for a preferred safeguard that cannot be applied.  
C. It only detects attacks.  
D. It guarantees safety.

<details><summary>Reveal answer and explanation</summary>

**B.** Isolation provides alternative protection. The underlying vulnerability remains, and no control guarantees safety.
</details>

### Q4
A company policy requires employees to report suspected phishing. The policy's primary function is:

A. Directive  
B. Corrective  
C. Detective  
D. Compensating

<details><summary>Reveal answer and explanation</summary>

**A — Directive.** It specifies required behavior. An actual employee report may help detection, but the **policy** is directive.
</details>

### Q5
Which BEST illustrates defense in depth against credential attacks?

A. Three copies of the same password list  
B. MFA, least privilege, and suspicious-login monitoring  
C. One shared administrator password  
D. Disabled audit logs

<details><summary>Reveal answer and explanation</summary>

**B.** MFA helps prevent compromise, least privilege limits damage, and monitoring helps detect misuse. The other choices increase risk.
</details>

### Q6
A visibly patrolling security guard discourages unauthorized visitors. Which **function** is emphasized?

A. Corrective  
B. Deterrent  
C. Compensating  
D. Directive

<details><summary>Reveal answer and explanation</summary>

**B — Deterrent.** The scenario emphasizes discouraging behavior. Guards can also detect or prevent incidents in other circumstances.
</details>

### Q7
After ransomware, technicians restore workstations from trusted images. Which function is this?

A. Corrective  
B. Preventive  
C. Deterrent  
D. Directive

<details><summary>Reveal answer and explanation</summary>

**A — Corrective.** Restoring systems addresses damage after the event. Preparing backups supports recovery readiness; using them to repair is corrective.
</details>

### Q8
A firewall blocks prohibited connections and alerts on repeated denied attempts. Which is MOST accurate?

A. It is only preventive.  
B. It is managerial.  
C. It performs preventive and detective functions.  
D. It is physical because hardware may be involved.

<details><summary>Reveal answer and explanation</summary>

**C.** Blocking is preventive; alerting is detective. The firewall is a technical control, whether implemented in software or dedicated hardware.
</details>

**Score guide:** 7–8: strong understanding. 5–6: review categories versus functions. 0–4: revisit Sections 2–4 and retry.

## 10. Discussion / reflection

> A small nonprofit can afford only **two** immediate improvements: MFA for staff, central alerting, tested backups, or annual awareness training. Which two would you prioritize **first**, and what facts about the organization would change your decision?

There is no universally correct pair. Good security decisions are grounded in threats, critical assets, existing controls, and business impact.

## 11. Study from the Treadmill 🏃

**Recommended:** Professor Messer's free **Security+ SY0-701** video lesson on *Security Controls* (Objective 1.1). Find it through the official course index:  
https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/security-plus-sy0-701-video-course/

**Listening challenge:** Pause and classify a firewall, a guard patrol, and a backup restore by category and function. Can any one of them have more than one function?

## 12. Exit ticket

Without notes, explain:

- The difference between **category** and **function**.
- All **four categories** and **six functions**.
- Why an IDS is typically detective and an IPS can be preventive.
- Why network isolation of an unpatchable system can be compensating.
- How three layers might protect a law firm's client files.

Next: **Day 03 — Authentication, Authorization, and Least Privilege.**

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.

**Curriculum note:** This is an independent educational resource aligned to CompTIA Security+ SY0-701 concepts, not affiliated with or endorsed by CompTIA. Confirm the exam version and official objectives for the exam you plan to take.
