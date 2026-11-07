# Week 01 Diagnostic — Security Foundations

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 01 · Five themes · 20 questions**  
> **Optional · Estimated time:** 25–35 minutes  
> **Prerequisites:** None

## Welcome to your weekly diagnostic!

**You don't have to study something you already know.**

This optional assessment helps you identify your strengths and knowledge gaps before beginning the week's lessons. It isn't graded, and your score doesn't determine whether you can continue.

Answer **20 questions without consulting your notes**. Afterward, review your results by theme—not just your overall score.

If you answer all four questions correctly for a theme and can explain your reasoning, you may choose to skip that lesson. If you miss questions, follow the links to the specific sections that address those concepts.

Remember: **a diagnostic is a starting point, not proof of complete mastery.** You'll have another opportunity to demonstrate your learning in the end-of-week review.

## What does this diagnostic cover?

| Questions | Theme | Lesson |
| --- | --- | --- |
| P1–P4 | CIA Triad | [Day 01 — CIA Triad](day-01-cia-triad.md) |
| P5–P8 | Security Controls | [Day 02 — Security Controls](day-02-security-controls.md) |
| P9–P12 | Authentication, Authorization & Least Privilege | [Day 03 — Authentication & Authorization](day-03-authentication-authorization.md) |
| P13–P16 | Audit Logs & Incident Timelines | [Day 04 — Audit Logs](day-04-audit-logs.md) |
| P17–P20 | Non-repudiation & Digital Signatures | [Day 05 — Digital Signatures](day-05-non-repudiation-digital-signatures.md) |

## How to take the diagnostic

1. Answer all 20 questions without consulting the lessons, notes, or answer key.
2. Record your answers on paper or in a separate note.
3. After finishing, expand the answer key.
4. Count your correct answers separately for each theme.
5. Use the targeted lesson links to address knowledge gaps.
6. Decide which lessons to study, review, or potentially skip.

These are original Security+-style educational questions, not actual CompTIA exam questions.

---

## Pre-diagnostic — 20 Questions

Choose the **single BEST answer** for each question.

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

---

## Answer Key & Targeted Study Recommendations

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal diagnostic answer key and explanations — P1–P20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the links below to review the exact concepts you need to strengthen.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| P1 | **B** | [Day 01 — Confidentiality](day-01-cia-triad.md#2-confidentiality-prevent-unauthorized-disclosure) | Unauthorized disclosure compromises confidentiality, even when records remain accurate and accessible. |
| P2 | **A** | [Day 01 — Integrity](day-01-cia-triad.md#3-integrity-protect-trustworthiness) | An unauthorized modification undermines the trustworthiness of the instruction. |
| P3 | **C** | [Day 01 — Availability](day-01-cia-triad.md#4-availability-keep-systems-usable) | Failover reduces service interruption; backups support restoration after disruption. |
| P4 | **B** | [Day 01 — CIA Trade-offs](day-01-cia-triad.md#5-the-objectives-interactand-sometimes-compete) | Theft affects confidentiality; inability to access files affects availability. Additional evidence is needed to assess integrity. |
| P5 | **C** | [Day 02 — Control Functions](day-02-security-controls.md#3-the-six-control-functions) | A directive control tells people what behavior is required. |
| P6 | **A** | [Day 02 — IDS vs. IPS](day-02-security-controls.md#ids-versus-ips) | Alerting is detective; blocking would be preventive. |
| P7 | **B** | [Day 02 — Corrective vs. Compensating](day-02-security-controls.md#corrective-versus-compensating) | Isolation reduces exposure while the underlying vulnerability remains. |
| P8 | **D** | [Day 02 — Physical Controls](day-02-security-controls.md#physical) | A fence is a physical safeguard; its function may be preventive or deterrent. |
| P9 | **C** | [Day 03 — Authorization](day-03-authentication-authorization.md#4-authorization-what-are-you-allowed-to-do) | Authentication verifies identity; authorization determines access rights. |
| P10 | **C** | [Day 03 — Authentication Factors](day-03-authentication-authorization.md#the-three-classic-factor-categories) | Password is knowledge and hardware security key is possession. |
| P11 | **C** | [Day 03 — Least Privilege](day-03-authentication-authorization.md#least-privilege) | JIT access grants temporary privileges for the approved task. |
| P12 | **B** | [Day 03 — Access-Control Models](day-03-authentication-authorization.md#5-four-access-control-models) | ABAC evaluates multiple attributes of the subject, resource, action, or environment. |
| P13 | **C** | [Day 04 — Reading Log Entries](day-04-audit-logs.md#3-under-the-hood-read-a-log-entry) | An account/session association does not establish which human acted. |
| P14 | **A** | [Day 04 — NTP and Correlation](day-04-audit-logs.md#4-time-ntp-and-correlation) | Clock drift or mismatched time zones can distort cross-system timelines. |
| P15 | **B** | [Day 04 — SIEM and Correlation](day-04-audit-logs.md#4-time-ntp-and-correlation) | A SIEM aggregates and correlates event records from different sources. |
| P16 | **C** | [Day 04 — Events, Alerts, Incidents](day-04-audit-logs.md#2-the-big-idea-security-systems-keep-receipts) | An alert warrants investigation but is not automatically a confirmed incident. |
| P17 | **A** | [Day 05 — Digital Signature Workflow](day-05-non-repudiation-digital-signatures.md#3-under-the-hood-how-digital-signatures-work) | The signer uses the private key; others verify with the matching public key. |
| P18 | **B** | [Day 05 — Signed Document Changes](day-05-non-repudiation-digital-signatures.md#5-real-world-example-the-signed-contract-dispute) | A valid signature is bound to the signed content; altering that content breaks verification. |
| P19 | **C** | [Day 05 — Certificates](day-05-non-repudiation-digital-signatures.md#4-certificates-whose-public-key-is-this) | A certificate helps establish the identity-to-public-key relationship. |
| P20 | **D** | [Day 05 — Signature Limitations](day-05-non-repudiation-digital-signatures.md#3-under-the-hood-how-digital-signatures-work) | Verification supports key use and integrity; human attribution needs other evidence. |

</details>

---

## Your Diagnostic Scorecard

Record your score for each theme, not just your total.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 01 — CIA Triad](day-01-cia-triad.md) | P1–P4 | ____ / 4 |
| [Day 02 — Security Controls](day-02-security-controls.md) | P5–P8 | ____ / 4 |
| [Day 03 — Authentication & Authorization](day-03-authentication-authorization.md) | P9–P12 | ____ / 4 |
| [Day 04 — Audit Logs](day-04-audit-logs.md) | P13–P16 | ____ / 4 |
| [Day 05 — Digital Signatures](day-05-non-repudiation-digital-signatures.md) | P17–P20 | ____ / 4 |
| **TOTAL** | **20 questions** | **____ / 20** |

### What do your results mean?

| Score per theme | Interpretation | Recommended Action |
| --- | --- | --- |
| **4/4** | Strong initial understanding | You may consider skipping the introductory lesson if you can explain your answers and apply the concepts in a new scenario. |
| **3/4** | Mostly familiar | Review the targeted section for the missed question and complete the lesson's exit ticket. |
| **0–2/4** | Knowledge gaps identified | Study the full lesson, including its worked examples, practice questions, and optional lab. |

**Important:** Four correct answers do not prove complete mastery. They provide an initial indication of your understanding. Continue checking your knowledge through cumulative practice and later assessments.

### Reflection

Before moving forward, consider:

- Which questions did you answer confidently?
- Which answers were educated guesses?
- Which concepts were unfamiliar?
- Could you explain why the incorrect options were wrong?
- Which lessons should you prioritize?

Treat lucky guesses as opportunities for review, even if your answer was correct.

---

## Build Your Personalized Study Plan

Use your diagnostic results to decide which lessons to complete.

- **Need to understand the CIA Triad?** Start with [Day 01 — Confidentiality, Integrity & Availability](day-01-cia-triad.md).
- **Confused about security controls?** Study [Day 02 — Categories, Functions & Defense in Depth](day-02-security-controls.md).
- **Need practice with identity and access?** Review [Day 03 — Authentication, Authorization & Least Privilege](day-03-authentication-authorization.md).
- **Want to improve log analysis?** Complete [Day 04 — Audit Logs & Incident Timelines](day-04-audit-logs.md).
- **Unsure about signatures and non-repudiation?** Study [Day 05 — Digital Signatures & Trust](day-05-non-repudiation-digital-signatures.md).

## What Comes Next?

**Ready to start learning?**

Begin with [Day 01 — The CIA Triad](day-01-cia-triad.md), or jump directly to the lessons identified by your diagnostic results.

**Already confident in all five themes?**

You can move directly to the [Week 01 End-of-Week Review](review.w1.md), which contains a different set of questions and cumulative scenarios.

The review gives you another opportunity to demonstrate what you know and identify any remaining knowledge gaps.

**Remember: The goal isn't to complete every lesson. The goal is to develop the knowledge and skills needed to succeed in cybersecurity and on the Security+ exam.**

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
