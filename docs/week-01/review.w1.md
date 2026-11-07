# Optional Week 01 Review — Security Foundations

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Five themes · Two parallel 20-question checks · Five cumulative questions**  
> **Optional · Suggested time:** 25–35 minutes per check, plus 10–15 minutes for cumulative practice

**Use this page in either direction.** Take the **pre-diagnostic before studying** to discover what you already know and which lessons to prioritize, or complete the lessons first and use the **parallel post-check** to evaluate what you learned. The cumulative questions connect multiple lessons.

These are **original educational practice questions**, not actual CompTIA exam questions. No login, scripting, or external tool is required. This Markdown version uses manual scoring and works on GitHub as well as MkDocs.

> **How to avoid answer leakage:** Record your answers on paper or in a separate note before opening any answer section. The post-check is a different set of questions on the same five themes; do not use the answer key while testing.

## Your five themes

| Theme | Lesson | Questions on each 20-item check |
| --- | --- | --- |
| CIA Triad | [Day 01 — CIA Triad](day-01-cia-triad.md) | 1–4 |
| Security Controls | [Day 02 — Security Controls](day-02-security-controls.md) | 5–8 |
| Authentication, Authorization & Least Privilege | [Day 03 — Authentication, Authorization & Least Privilege](day-03-authentication-authorization.md) | 9–12 |
| Audit Logs & Incident Timelines | [Day 04 — Audit Logs & Incident Timelines](day-04-audit-logs.md) | 13–16 |
| Non-repudiation & Digital Signatures | [Day 05 — Non-repudiation & Digital Signatures](day-05-non-repudiation-digital-signatures.md) | 17–20 |

## How to use your results

1. **Before the lessons:** Answer P1–P20 without notes. Record your answers, then open the pre-diagnostic answer key.
2. **Score each theme separately:** Each theme has four questions. Use the recommendation table below to decide what to study.
3. **Study selectively:** Read any recommended lessons and complete their examples and hands-on activities.
4. **After studying:** Answer S1–S20 without notes, then check the separate post-check key.
5. **Integrate and retain:** Try C1–C5. Revisit this review later for spaced practice.

| Score for one theme (out of 4) | Interpretation | Suggested next step |
| --- | --- | --- |
| **4/4** | Promising diagnostic evidence | You *may* skip the introductory lesson if you can also explain the reasoning and apply it in a new scenario. Recheck in cumulative practice. |
| **3/4** | Mostly familiar, possible gap | Review the specific missed concept and relevant lesson section; complete the lesson's exit ticket. |
| **0–2/4** | Important knowledge gaps | Study the full linked lesson, including its worked example and practice questions. |

**These are study recommendations, not certification of mastery.** Four questions per topic cannot reliably establish comprehensive mastery; the cumulative scenarios and later spaced checks provide additional evidence. The 20-question total is useful for tracking progress, but **topic-level scores drive your study plan**.

## Part A — Pre-diagnostic (before studying)

**Directions:** Select the single BEST answer to each question. Do not open the answer key until you've answered all 20.

### P1

**A clinic's patient database remains online and unchanged, but a visitor reads restricted records. Which objective is most directly affected?**

A. Availability  
B. Confidentiality  
C. Integrity  
D. Non-repudiation  

### P2

**An attacker changes the destination account on a payment instruction without permission. Which objective is directly compromised?**

A. Integrity  
B. Availability  
C. Confidentiality  
D. Authentication  

### P3

**A firm can restore files from yesterday's backup, but its portal must remain online if a single server fails. Which measure best addresses the immediate requirement?**

A. Data classification  
B. SHA-256 hashing  
C. Redundant server with failover  
D. Digital signature  

### P4

**A ransomware actor steals client files and makes the originals inaccessible. Which objectives are clearly affected?**

A. Integrity only  
B. Confidentiality and availability  
C. Availability only  
D. Confidentiality only  

### P5

**A written rule requires employees to report suspected phishing. Which control function is primarily illustrated?**

A. Corrective  
B. Compensating  
C. Directive  
D. Detective  

### P6

**A network appliance reports suspicious traffic but never blocks it. What function is it performing?**

A. Detective  
B. Preventive  
C. Corrective  
D. Deterrent  

### P7

**An unpatchable legacy server is isolated from other systems. Why is the isolation a compensating control?**

A. It removes the software flaw  
B. It substitutes for an unavailable preferred protection  
C. It restores lost data  
D. It proves user identity  

### P8

**An organization installs a fence around its data center. Which category best describes the fence?**

A. Technical  
B. Operational  
C. Managerial  
D. Physical  

### P9

**An employee logs in successfully but cannot read payroll records. Which decision most directly explains the denial?**

A. Identification  
B. Authentication  
C. Authorization  
D. Accounting  

### P10

**Which pair represents two different authentication-factor categories?**

A. Password and PIN  
B. Fingerprint and face scan  
C. Password and hardware security key  
D. Two different passwords  

### P11

**A contractor needs elevated access for one approved change. Which option best limits unnecessary privilege?**

A. Permanent admin membership  
B. Shared administrator account  
C. Just-in-time elevation  
D. Disable activity logging  

### P12

**Access is allowed only when a user has an assigned case, uses a managed device, and requests a permitted action. Which model fits best?**

A. DAC  
B. ABAC  
C. MAC  
D. Role-only RBAC  

### P13

**A log says `user=admin action=EXPORT result=SUCCESS`. What can investigators safely conclude?**

A. The administrator personally exported files  
B. The export was approved  
C. An export was recorded under that account or session  
D. The log cannot help an investigation  

### P14

**A VPN event appears five minutes after a file export even though the VPN connection began first. What should analysts check?**

A. Clock synchronization and time zones  
B. Encryption key length  
C. File compression settings  
D. Monitor brightness  

### P15

**A team wants to correlate VPN sign-ins with file downloads across multiple systems. Which technology is designed to help?**

A. UPS  
B. SIEM  
C. Load balancer  
D. Password manager  

### P16

**A monitoring system flags repeated failed logins followed by success, but compromise is not confirmed. The notification is best classified as what?**

A. Incident proof  
B. Backup  
C. Alert  
D. Digital signature  

### P17

**Which key is used to create a digital signature on a software release?**

A. Publisher's private key  
B. Publisher's public key  
C. Recipient's private key  
D. Recipient's public key  

### P18

**A signed document is modified within the bytes covered by its signature. What should verification show?**

A. The signature updates automatically  
B. Verification fails  
C. The document becomes encrypted  
D. The certificate becomes a backup  

### P19

**What is the primary purpose of a trusted certificate when verifying a signature?**

A. Guarantee that the signer is honest  
B. Recover a lost private key  
C. Associate a public key with an identity under a trust framework  
D. Keep the signed document secret  

### P20

**A signature verifies, but the private key may have been stolen. Which statement is most defensible?**

A. The named person definitely clicked Sign  
B. The signed data must be confidential  
C. The signature proves legal authority  
D. Verification alone does not identify the human who used the key  

<details markdown="1">
<summary><strong>Reveal pre-diagnostic answer key and explanations — only after completing P1–P20</strong></summary>

| Question | Answer | Why |
| --- | --- | --- |
| P1 | **B** | Unauthorized disclosure compromises confidentiality, even when records remain accurate and accessible. |
| P2 | **A** | An unauthorized modification undermines the trustworthiness of the instruction. |
| P3 | **C** | Failover reduces service interruption; backups support restoration after disruption. |
| P4 | **B** | Theft affects confidentiality; inability to access files affects availability. Additional evidence is needed to assess integrity. |
| P5 | **C** | A directive control tells people what behavior is required. |
| P6 | **A** | Alerting is detective; blocking would be preventive. |
| P7 | **B** | Isolation reduces exposure while the underlying vulnerability remains. |
| P8 | **D** | A fence is a physical safeguard; its function may be preventive or deterrent. |
| P9 | **C** | Authentication verifies identity; authorization determines access rights. |
| P10 | **C** | Password is knowledge and hardware security key is possession. |
| P11 | **C** | JIT access grants temporary privileges for the approved task. |
| P12 | **B** | ABAC evaluates multiple attributes of the subject, resource, action, or environment. |
| P13 | **C** | An account/session association does not establish which human acted. |
| P14 | **A** | Clock drift or mismatched time zones can distort cross-system timelines. |
| P15 | **B** | A SIEM aggregates and correlates event records from different sources. |
| P16 | **C** | An alert warrants investigation but is not automatically a confirmed incident. |
| P17 | **A** | The signer uses the private key; others verify with the matching public key. |
| P18 | **B** | A valid signature is bound to the signed content; altering that content breaks verification. |
| P19 | **C** | A certificate helps establish the identity-to-public-key relationship. |
| P20 | **D** | Verification supports key use and integrity; human attribution needs other evidence. |
</details>

### Pre-diagnostic scorecard

| Theme | Correct out of 4 | Recommended action |
| --- | --- | --- |
| [CIA Triad](day-01-cia-triad.md) | ____ / 4 | ____________________ |
| [Security Controls](day-02-security-controls.md) | ____ / 4 | ____________________ |
| [Authentication, Authorization & Least Privilege](day-03-authentication-authorization.md) | ____ / 4 | ____________________ |
| [Audit Logs & Incident Timelines](day-04-audit-logs.md) | ____ / 4 | ____________________ |
| [Non-repudiation & Digital Signatures](day-05-non-repudiation-digital-signatures.md) | ____ / 4 | ____________________ |
| **Total** | **____ / 20** | Use topic scores, not just total score |

**Reflection:** Which answers were guesses? Which correct answers can you explain without looking at the key? Treat lucky guesses as topics to review.

## Part B — Parallel post-check (after studying)

**Directions:** These are new questions targeting the same learning outcomes as the pre-diagnostic. Select the single BEST answer. Use no notes; don't reveal the key until finished.

### S1

**A university's grades remain private and accessible, but an unauthorized person raises one student's score. What failed?**

A. Availability  
B. Integrity  
C. Confidentiality  
D. Authentication  

### S2

**A public-service portal is unreachable during a traffic flood, with no evidence of data access or changes. What is the directly observed impact?**

A. Confidentiality  
B. Integrity  
C. Availability  
D. Non-repudiation  

### S3

**A firm wants to detect whether a downloaded document differs from a trusted reference. Which control is most relevant?**

A. Trusted SHA-256 digest comparison  
B. Redundant power supply  
C. Security awareness policy  
D. Access badge  

### S4

**A hospital makes emergency records available to every internet visitor to reduce access delays. Which trade-off is most evident?**

A. Better integrity at the cost of availability  
B. Better availability at the cost of confidentiality  
C. Better non-repudiation at the cost of integrity  
D. Better hashing at the cost of authentication  

### S5

**An intrusion prevention system actively rejects prohibited packets. Which function is emphasized?**

A. Detective  
B. Corrective  
C. Preventive  
D. Compensating  

### S6

**Leadership performs a formal risk assessment before purchasing a new cloud platform. Which control category fits best?**

A. Managerial  
B. Physical  
C. Operational  
D. Technical  

### S7

**Technicians restore corrupted endpoints from clean images after an incident. Which function is illustrated?**

A. Deterrent  
B. Directive  
C. Corrective  
D. Compensating  

### S8

**A firewall blocks disallowed connections and sends alerts about repeated denials. Which statement is best?**

A. It can serve preventive and detective functions  
B. It is solely physical  
C. It is solely directive  
D. It removes every vulnerability  

### S9

**A visitor claims to be an employee, then presents a valid security key. Which stage checks the evidence supporting the identity claim?**

A. Accounting  
B. Authorization  
C. Authentication  
D. Classification  

### S10

**A payment workflow requires one employee to create a transfer and a second to approve it. What principle is used?**

A. Least availability  
B. Separation of duties  
C. DAC  
D. Key escrow  

### S11

**A document owner can decide who may read their shared folder. Which access model is most closely illustrated?**

A. MAC  
B. ABAC  
C. DAC  
D. RBAC  

### S12

**A staff member's compromised account can edit only assigned cases, not all firm records. What principle limits the impact?**

A. Least privilege  
B. Non-repudiation  
C. Load balancing  
D. Time synchronization  

### S13

**Which field most directly helps group multiple events from one authenticated session?**

A. CPU temperature  
B. Session ID  
C. File extension  
D. Screen size  

### S14

**What is the strongest reason to forward important logs to restricted central storage?**

A. It proves the human actor  
B. It removes the need for investigation  
C. It reduces reliance on a potentially compromised host's local records  
D. It guarantees every event was captured  

### S15

**An endpoint record includes a process's command line and parent process. Which source is most likely to supply this detail?**

A. EDR or Sysmon telemetry  
B. Door-access controller  
C. UPS status log  
D. Certificate-expiration calendar  

### S16

**An alert reports a large file export at night. What is the best initial investigative approach?**

A. Name the account owner as the culprit  
B. Correlate identity, VPN, endpoint and approved-change evidence  
C. Delete logs to preserve privacy  
D. Assume all exports are malicious  

### S17

**Which key should a recipient use to verify a publisher's digital signature?**

A. Recipient's private key  
B. Publisher's private key  
C. Publisher's authentic public key  
D. Recipient's password  

### S18

**A vendor provides a download and a hash on the same attacker-controlled page. What is the key limitation?**

A. The hash always encrypts the file  
B. Both file and expected digest could have been replaced  
C. The file cannot be downloaded  
D. The hash identifies the person who created the file  

### S19

**A digitally signed contract is sent in readable form to an unintended recipient. Which additional control would protect confidentiality?**

A. A second signature  
B. More audit fields  
C. Encryption  
D. A longer filename  

### S20

**A signature is valid, but a lawyer disputes authorizing the contract. Which evidence is most useful to investigate next?**

A. Key custody and signing-platform audit records  
B. Monitor resolution  
C. Document page count alone  
D. Number of backup copies  

<details markdown="1">
<summary><strong>Reveal post-check answer key and explanations — only after completing S1–S20</strong></summary>

| Question | Answer | Why |
| --- | --- | --- |
| S1 | **B** | Unauthorized alteration affects integrity. |
| S2 | **C** | The service cannot be used by authorized people. |
| S3 | **A** | Comparing hashes against a trusted reference helps detect changes to bytes. |
| S4 | **B** | Broad access may improve accessibility but expose private information. |
| S5 | **C** | Actively blocking traffic is preventive. |
| S6 | **A** | Risk assessment is a managerial control. |
| S7 | **C** | Restoration repairs damage after an incident. |
| S8 | **A** | Blocking prevents and alerting detects; one technical control can perform multiple functions. |
| S9 | **C** | Authentication verifies the claimed identity. |
| S10 | **B** | Dividing sensitive responsibilities reduces unilateral misuse. |
| S11 | **C** | DAC lets a resource owner grant access, subject to system rules. |
| S12 | **A** | Restricting permissions to necessary tasks limits damage. |
| S13 | **B** | A session identifier can connect events from the same session. |
| S14 | **C** | Centralization makes it harder for an attacker on one host to erase the only copies. |
| S15 | **A** | Endpoint process telemetry can record process trees and command lines. |
| S16 | **B** | Corroboration helps distinguish compromise from authorized jobs and avoids premature attribution. |
| S17 | **C** | Signature verification uses the signer's corresponding public key. |
| S18 | **B** | A hash comparison needs a trustworthy reference to support integrity assurance. |
| S19 | **C** | Signatures do not hide content; encryption is the relevant confidentiality protection. |
| S20 | **A** | Key custody and surrounding audit evidence help evaluate attribution and approval. |
</details>

### Compare your learning

| Theme | Pre-diagnostic | Post-check | Next action |
| --- | --- | --- | --- |
| [CIA Triad](day-01-cia-triad.md) | ____ / 4 | ____ / 4 | ____________________ |
| [Security Controls](day-02-security-controls.md) | ____ / 4 | ____ / 4 | ____________________ |
| [Authentication, Authorization & Least Privilege](day-03-authentication-authorization.md) | ____ / 4 | ____ / 4 | ____________________ |
| [Audit Logs & Incident Timelines](day-04-audit-logs.md) | ____ / 4 | ____ / 4 | ____________________ |
| [Non-repudiation & Digital Signatures](day-05-non-repudiation-digital-signatures.md) | ____ / 4 | ____ / 4 | ____________________ |
| **Total** | **____ / 20** | **____ / 20** | |

A higher score suggests progress, but it is not a complete measure of competence. If you missed a post-check question, revisit the linked lesson and explain the correct reasoning in your own words. If you scored perfectly, return to these ideas in later weeks rather than assuming they will stay memorized.

## Part C — Cumulative application (after the week)

These five questions combine concepts from more than one lesson. They are **additional** to the two 20-question checks and are useful for spaced review.

### C1

**A paralegal's account exports 400 restricted files after unusual sign-ins. Which combination best supports an evidence-based investigation?**

A. Assume the paralegal did it and reset nothing  
B. Correlate identity, session, and file logs while assessing confidentiality exposure  
C. Treat the event as an availability failure only  
D. Rely only on the username  

### C2

**A company signs a software update but stores its signing private key in a shared folder. Which risk is most important?**

A. The signature guarantees confidentiality  
B. A stolen key could sign malicious updates that appear authentic  
C. The public key can no longer verify signatures  
D. Backups automatically prevent misuse  

### C3

**An old billing server cannot be patched; network isolation is deployed, with centralized alerts for unusual access. How should the controls be described?**

A. Isolation is compensating/preventive; alerts are detective  
B. Both controls are exclusively corrective  
C. Isolation patches the underlying defect  
D. Alerts prove the human actor  

### C4

**A digitally signed invoice has a changed bank account number. Verification fails. Which combination best describes the evidence?**

A. Confidentiality failure and proven identity theft  
B. Integrity concern supported by failed signature verification  
C. Availability failure with guaranteed outage  
D. Proof that the signer intentionally changed it  

### C5

**An administrator account logs in with MFA and downloads confidential files outside its authorized project. What is the strongest conclusion?**

A. MFA proves the person was authorized  
B. Authentication may have succeeded while authorization or permissions were inadequate; corroborate the actor  
C. Audit records prove a particular human downloaded them  
D. Digital signatures would prevent every download  

<details markdown="1">
<summary><strong>Reveal cumulative answers and connected lessons</strong></summary>

| Question | Answer | Explanation | Connected lessons |
| --- | --- | --- | --- |
| C1 | **B** | Cross-source logs help assess the export; unauthorized disclosure would affect confidentiality. | Days 01, 03, 04 |
| C2 | **B** | A compromised private signing key weakens source assurance despite valid signature verification. | Days 02, 03, 05 |
| C3 | **A** | Segmentation is an alternative protective measure; alerting detects suspicious activity. | Days 01, 02, 04 |
| C4 | **B** | The changed signed bytes indicate an integrity issue; verification does not establish who altered them. | Days 01, 05 |
| C5 | **B** | MFA does not grant authorization, and account logs do not conclusively identify a human. | Days 01, 03, 04, 05 |
</details>

### Cumulative reflection

Choose **one** scenario above and explain, in 3–5 sentences:

1. What facts are directly observed?
2. Which security objective or control principle is involved?
3. What evidence would you need before making a stronger claim?
4. Which control would you recommend, and what limitation would it have?

**Optional hands-on reinforcement:** Try the [Day 01 file-hash experiment](day-01-cia-triad.md), the [Day 03 permissions matrix](day-03-authentication-authorization.md), or the [Day 04 fictional log investigation](day-04-audit-logs.md). Those activities test skills that multiple-choice questions cannot fully measure.

## What to do next

- **Ready to continue?** If you can explain your answers, perform the relevant activities, and handle cumulative scenarios, move to Week 02.
- **A theme still feels uncertain?** Return to the linked lesson and its exit ticket. Reattempt a *different* question set after studying; avoid memorizing this answer key.
- **Planning a 12–16-week study schedule?** You can complete this optional review before or after the five lessons without adding a mandatory sixth instructional day.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
