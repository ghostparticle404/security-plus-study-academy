
# Week 01 Review — Security Foundations

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 01 · Five themes · 20 review questions + 5 cumulative questions**  
> **Optional · Estimated time:** 35–50 minutes  
> **Prerequisites:** Week 01 lessons or equivalent prior knowledge

## Welcome to your Week 01 Review!

**You've studied the concepts. Now let's see how well you can apply them.**

This optional review helps you evaluate your understanding of the five Week 01 themes, identify any remaining knowledge gaps, and determine which concepts deserve additional practice.

You can complete this review whether you studied all five lessons or skipped some based on your diagnostic results.

**Haven't started the week yet?** Take the [Week 01 Diagnostic](00-diagnostic.w1.md) to identify what you already know and which lessons to prioritize.

### How to complete this review

1. Answer **S1–S20** without consulting your notes or lessons.
2. Record your answers before opening the answer key.
3. Check your results by theme, not just your overall score.
4. Follow the targeted lesson links for questions you missed.
5. Complete **C1–C5**, which combine concepts from multiple lessons.
6. Reflect on your progress and decide whether you're ready for Week 02.

These are original educational practice questions, not actual CompTIA exam items.

## Week 01 Themes

| Questions | Theme | Lesson |
| --- | --- | --- |
| S1–S4 | CIA Triad | [Day 01](day-01-cia-triad.md) |
| S5–S8 | Security Controls | [Day 02](day-02-security-controls.md) |
| S9–S12 | Authentication, Authorization & Least Privilege | [Day 03](day-03-authentication-authorization.md) |
| S13–S16 | Audit Logs & Incident Timelines | [Day 04](day-04-audit-logs.md) |
| S17–S20 | Non-repudiation & Digital Signatures | [Day 05](day-05-non-repudiation-digital-signatures.md) |

---

## Part 1 — End-of-Week Knowledge Check

**Directions:** Choose the single BEST answer to each question. Complete all 20 before revealing the answer key.

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

---

## Part 2 — Answer Key & Targeted Review

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal Week 01 answer key and explanations — S1–S20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the targeted lesson links to review any concepts you missed.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| S1 | **B** | [Day 01 — Integrity](day-01-cia-triad.md#3-integrity-protect-trustworthiness) | Unauthorized alteration affects integrity. |
| S2 | **C** | [Day 01 — Availability](day-01-cia-triad.md#4-availability-keep-systems-usable) | The service cannot be used by authorized people. |
| S3 | **A** | [Day 01 — Integrity Checks](day-01-cia-triad.md#3-integrity-protect-trustworthiness) | Comparing hashes against a trusted reference helps detect changes to bytes. |
| S4 | **B** | [Day 01 — CIA Trade-offs](day-01-cia-triad.md#5-the-objectives-interactand-sometimes-compete) | Broad access may improve accessibility but expose private information. |
| S5 | **C** | [Day 02 — IDS vs. IPS](day-02-security-controls.md#ids-versus-ips) | Actively blocking traffic is preventive. |
| S6 | **A** | [Day 02 — Managerial Controls](day-02-security-controls.md#managerial) | Risk assessment is a managerial control. |
| S7 | **C** | [Day 02 — Corrective Controls](day-02-security-controls.md#3-the-six-control-functions) | Restoration repairs damage after an incident. |
| S8 | **A** | [Day 02 — Multiple Control Functions](day-02-security-controls.md#4-under-the-hood-one-control-multiple-labels) | Blocking prevents and alerting detects; one technical control can perform multiple functions. |
| S9 | **C** | [Day 03 — Authentication](day-03-authentication-authorization.md#3-authentication-proving-identity) | Authentication verifies the claimed identity. |
| S10 | **B** | [Day 03 — Separation of Duties](day-03-authentication-authorization.md#least-privilege) | Dividing sensitive responsibilities reduces unilateral misuse. |
| S11 | **C** | [Day 03 — Access-Control Models](day-03-authentication-authorization.md#5-four-access-control-models) | DAC lets a resource owner grant access, subject to system rules. |
| S12 | **A** | [Day 03 — Least Privilege](day-03-authentication-authorization.md#least-privilege) | Restricting permissions to necessary tasks limits damage. |
| S13 | **B** | [Day 04 — Log Fields](day-04-audit-logs.md#3-under-the-hood-read-a-log-entry) | A session identifier can connect events from the same session. |
| S14 | **C** | [Day 04 — Protecting Audit Evidence](day-04-audit-logs.md#5-protecting-audit-evidence) | Centralization makes it harder for an attacker on one host to erase the only copies. |
| S15 | **A** | [Day 04 — Log Sources](day-04-audit-logs.md#common-log-sources) | Endpoint process telemetry can record process trees and command lines. |
| S16 | **B** | [Day 04 — Investigating Exports](day-04-audit-logs.md#6-case-study-the-midnight-law-firm-export) | Corroboration helps distinguish compromise from authorized jobs and avoids premature attribution. |
| S17 | **C** | [Day 05 — Signature Verification](day-05-non-repudiation-digital-signatures.md#3-under-the-hood-how-digital-signatures-work) | Signature verification uses the signer's corresponding public key. |
| S18 | **B** | [Day 05 — Trusted Hashes](day-05-non-repudiation-digital-signatures.md#2-a-cryptographic-fingerprint-hashing) | A hash comparison needs a trustworthy reference to support integrity assurance. |
| S19 | **C** | [Day 05 — Signatures vs. Encryption](day-05-non-repudiation-digital-signatures.md#5-real-world-example-the-signed-contract-dispute) | Signatures do not hide content; encryption is the relevant confidentiality protection. |
| S20 | **A** | [Day 05 — Signature Attribution](day-05-non-repudiation-digital-signatures.md#5-real-world-example-the-signed-contract-dispute) | Key custody and surrounding audit evidence help evaluate attribution and approval. |

</details>

## Your Week 01 Scorecard

Record your results by theme.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 01 — CIA Triad](day-01-cia-triad.md) | S1–S4 | ____ / 4 |
| [Day 02 — Security Controls](day-02-security-controls.md) | S5–S8 | ____ / 4 |
| [Day 03 — Authentication & Authorization](day-03-authentication-authorization.md) | S9–S12 | ____ / 4 |
| [Day 04 — Audit Logs](day-04-audit-logs.md) | S13–S16 | ____ / 4 |
| [Day 05 — Digital Signatures](day-05-non-repudiation-digital-signatures.md) | S17–S20 | ____ / 4 |
| **TOTAL** | **20 questions** | **____ / 20** |

### Interpreting your results

| Score per theme | Interpretation | Recommended Action |
| --- | --- | --- |
| **4/4** | Strong understanding demonstrated | Continue to cumulative questions. Revisit the theme during later spaced practice. |
| **3/4** | Mostly understood | Review the specific concept you missed using the targeted lesson link. |
| **0–2/4** | Knowledge gaps remain | Revisit the full lesson, worked examples, and optional lab before moving on. |

**Remember:** A perfect score on four questions is encouraging, but it doesn't establish complete mastery of a topic. Use the cumulative scenarios below to test whether you can connect and apply what you've learned.

### Compare Your Progress

If you completed the [Week 01 Diagnostic](00-diagnostic.md), compare your results.

| Theme | Diagnostic Score | Review Score |
| --- | --- | --- |
| CIA Triad | ____ / 4 | ____ / 4 |
| Security Controls | ____ / 4 | ____ / 4 |
| Authentication & Authorization | ____ / 4 | ____ / 4 |
| Audit Logs | ____ / 4 | ____ / 4 |
| Digital Signatures | ____ / 4 | ____ / 4 |
| **TOTAL** | **____ / 20** | **____ / 20** |

**Reflection:** Where did your understanding improve? Which concepts still need practice? Did you learn something you thought you already understood?

---

## Part 3 — Cumulative Application

These five questions combine concepts from multiple Week 01 lessons.

They are designed to assess your ability to **apply and connect security concepts**, rather than simply recognize definitions.

Choose the single BEST answer for each question.

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

### Cumulative Answer Key

<details markdown="1">
<summary><strong>Reveal cumulative answers and connected lessons — C1–C5</strong></summary>

| Question | Answer | Explanation | Connected Lessons |
| --- | --- | --- | --- |
| C1 | **B** | Cross-source logs help assess the export; unauthorized disclosure would affect confidentiality. | [Day 01](day-01-cia-triad.md), [Day 03](day-03-authentication-authorization.md), [Day 04](day-04-audit-logs.md) |
| C2 | **B** | A compromised private signing key weakens source assurance despite valid signature verification. | [Day 02](day-02-security-controls.md), [Day 03](day-03-authentication-authorization.md), [Day 05](day-05-non-repudiation-digital-signatures.md) |
| C3 | **A** | Segmentation is an alternative protective measure; alerting detects suspicious activity. | [Day 01](day-01-cia-triad.md), [Day 02](day-02-security-controls.md), [Day 04](day-04-audit-logs.md) |
| C4 | **B** | The changed signed bytes indicate an integrity issue; verification does not establish who altered them. | [Day 01](day-01-cia-triad.md), [Day 05](day-05-non-repudiation-digital-signatures.md) |
| C5 | **B** | MFA does not grant authorization, and account logs do not conclusively identify a human. | [Day 01](day-01-cia-triad.md), [Day 03](day-03-authentication-authorization.md), [Day 04](day-04-audit-logs.md), [Day 05](day-05-non-repudiation-digital-signatures.md) |

</details>

### Cumulative Reflection

Choose **one scenario** above and explain your reasoning in 3–5 sentences.

1. What facts are directly observed?
2. Which security objectives or control principles are involved?
3. What additional evidence would you need before making a stronger claim?
4. Which control would you recommend, and what limitations would it have?

### Optional Hands-on Reinforcement

If you'd like to apply what you've learned, revisit one of these activities:

- [Day 01 — File Integrity Experiment](day-01-cia-triad.md#8-optional-hands-on-activity-10-minutes)
- [Day 03 — Permissions Matrix](day-03-authentication-authorization.md#9-hands-on-exercise-design-a-permissions-matrix)
- [Day 04 — Log Investigation](day-04-audit-logs.md#8-hands-on-lab-investigate-a-tiny-log-file)

These activities develop practical skills that multiple-choice questions alone cannot fully assess.

---

## Week 01 — Final Reflection

Before continuing, ask yourself:

- Can I distinguish confidentiality, integrity, and availability in realistic scenarios?
- Can I classify security controls by category and function?
- Can I explain authentication, authorization, and least privilege?
- Can I interpret basic security logs without making unsupported assumptions?
- Can I explain what digital signatures prove—and what they don't?

**If you can confidently explain and apply these concepts, you're ready to move forward.**

If some concepts still feel uncertain, use your answer-key links to revisit the relevant sections. You don't necessarily need to repeat an entire lesson.

## What's Next?

**Congratulations on completing Week 01 — Security Foundations!**

You've explored the fundamental principles that support cybersecurity decisions: protecting information, implementing controls, managing access, interpreting evidence, and establishing trust.

In **Week 02 — Cryptography & Public Key Infrastructure**, you'll build on these foundations by exploring encryption, hashing, key pairs, certificates, and cryptographic key management.

**Ready to continue?** Start with the Week 02 diagnostic, or proceed to [Day 06 — Hashing vs. Encryption](../week-02/day-06-hashing-vs-encryption.md).

Remember: The goal isn't simply to finish the lessons. It's to understand the concepts well enough to recognize, explain, and apply them in real-world situations.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
