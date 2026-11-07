# Week 03 Diagnostic — Data Protection, Retention & Resilience

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 03 · Five themes · 20 questions**  
> **Optional · Estimated time:** 25–35 minutes  
> **Prerequisites:** None; familiarity with Weeks 01–02 may help

## Welcome to your weekly diagnostic!

**You don't have to study something you already know.**

This optional assessment helps you identify your strengths and knowledge gaps before beginning the week's lessons. It isn't graded, and your score doesn't determine whether you can continue.

Answer **20 questions without consulting your notes**. Afterward, review your results by theme—not just your overall score.

If you answer all four questions correctly for a theme and can explain your reasoning, you may choose to skip that lesson. If you miss questions, follow the links to the specific sections that address those concepts.

Remember: **a diagnostic is a starting point, not proof of complete mastery.** You'll have another opportunity to demonstrate your learning in the end-of-week review.

## What does this diagnostic cover?

| Questions | Theme | Lesson |
| --- | --- | --- |
| P1–P4 | Data at Rest, in Transit & in Use | [Day 11 — Data States](day-11-data-states.md) |
| P5–P8 | Data Classification, Labeling & Handling | [Day 12 — Data Classification](day-12-data-classification.md) |
| P9–P12 | Data Retention, Disposal & Minimization | [Day 13 — Retention & Disposal](day-13-data-retention-disposal.md) |
| P13–P16 | Backups, Recovery Objectives & Resilience | [Day 14 — Backups & Recovery](day-14-backups-recovery.md) |
| P17–P20 | Business Continuity & Disaster Recovery | [Day 15 — Business Continuity](day-15-business-continuity.md) |

## How to take the diagnostic

1. Answer all 20 questions without consulting your lessons, notes, or the answer key.
2. Record your answers on paper or in a separate note.
3. After finishing, expand the answer key.
4. Count your correct answers separately for each theme.
5. Use the targeted lesson-section links to identify knowledge gaps.
6. Decide which lessons to study, review, or potentially skip.

These are original Security+-style educational questions, not actual CompTIA exam questions.

---

## Pre-diagnostic — 20 Questions

Choose the **single BEST answer** for each question.

### P1

**A law firm encrypts its laptops using full-disk encryption. An employee leaves a laptop unlocked, and malware running under the employee's account reads a confidential document. Why did full-disk encryption fail to prevent this exposure?**

A. Full-disk encryption protects only network traffic  
B. Full-disk encryption does not use cryptographic keys  
C. An unlocked system may provide decrypted data to authorized processes  
D. Full-disk encryption automatically disables access controls

### P2

**A financial analyst uploads a confidential spreadsheet to a company portal using HTTPS. Which data state is TLS primarily protecting during the upload?**

A. Data at rest  
B. Data in transit  
C. Data in use  
D. Archived data

### P3

**A hospital wants to prevent employees from uploading sensitive patient records to unauthorized cloud-storage services. Which technology is MOST appropriate?**

A. Data Loss Prevention (DLP)  
B. Network Time Protocol (NTP)  
C. Redundant Array of Independent Disks (RAID)  
D. Dynamic Host Configuration Protocol (DHCP)

### P4

**A payment processor replaces customers' actual card numbers with substitute values that can be resolved through a separate protected vault. Which technique is being used?**

A. Data masking  
B. Hash comparison  
C. Full-disk encryption  
D. Tokenization

### P5

**A company must determine whether employee schedules, client contracts, and confidential witness statements require different protections. What should it do FIRST?**

A. Encrypt every file using the same shared password  
B. Classify information according to sensitivity and impact  
C. Delete all documents older than one year  
D. Make every document accessible to all employees

### P6

**A document is labeled RESTRICTED, but anyone with its public sharing link can download it. What is the PRIMARY security weakness?**

A. Restricted documents cannot be encrypted  
B. The document's filename is too descriptive  
C. Classification labels do not automatically enforce access restrictions  
D. Public links always provide stronger authentication

### P7

**Who is typically accountable for deciding a business dataset's classification and access requirements?**

A. Data owner  
B. Data custodian  
C. Network switch  
D. Backup software vendor

### P8

**An employee copies sensitive witness information into a new, unlabeled spreadsheet and emails it outside the organization. Which approach would BEST help address this risk?**

A. Depend exclusively on document filenames  
B. Assume the original classification label will always remain attached  
C. Use TLS without checking recipients or content  
D. Combine content-aware DLP, access controls, and handling procedures

### P9

**A law firm normally deletes certain records after their approved retention period. Counsel issues a legal hold covering those records. What should the firm do?**

A. Delete the records according to the normal schedule  
B. Suspend routine deletion of records within the hold's scope  
C. Remove classification labels and publish the records  
D. Destroy the encryption keys immediately

### P10

**A company plans to donate laptops containing SSDs previously used to store confidential records. Which approach BEST supports secure disposal?**

A. Empty the Trash and reinstall the operating system  
B. Rename all confidential files  
C. Use an approved, validated, media-appropriate sanitization method  
D. Delete the browser history

### P11

**An organization securely encrypts a storage device and later eliminates all required decryption key material so that the ciphertext cannot be recovered, assuming all prerequisites are met. What is this process called?**

A. Cryptographic erase  
B. Data masking  
C. Differential backup  
D. Data classification

### P12

**A company deletes an old customer database from its production cloud environment. Investigators later discover copies in snapshots and backup archives. What does this demonstrate?**

A. Cloud encryption is always ineffective  
B. TLS automatically preserves deleted files  
C. Data classification prevents deletion  
D. Deleting production data does not necessarily remove retained copies

### P13

**A company's database must be recoverable with no more than 30 minutes of potential lost transactions. Which recovery metric expresses this requirement?**

A. Recovery Time Objective (RTO)  
B. Recovery Point Objective (RPO)  
C. Mean Time Between Failures (MTBF)  
D. Mean Time to Repair (MTTR)

### P14

**A full backup is created on Sunday, followed by incremental backups on Monday, Tuesday, and Wednesday. In a traditional incremental-backup scheme, what is required to restore Wednesday's data?**

A. Wednesday's incremental backup only  
B. Sunday's full backup and Wednesday's incremental backup only  
C. Sunday's full backup and all subsequent incremental backups  
D. Tuesday's and Wednesday's incremental backups only

### P15

**A company uses RAID mirroring to protect its database. An administrator accidentally deletes an important table, and the deletion appears on both mirrored disks. What was missing?**

A. A recoverable historical backup  
B. An additional network switch  
C. A longer database password  
D. Faster disk replication

### P16

**An organization's backup jobs report success every night, but nobody has attempted to restore the data. What should administrators do NEXT?**

A. Assume the backups will work during a disaster  
B. Disable backup encryption  
C. Delete all older recovery points  
D. Perform and document a controlled restore test

### P17

**A law firm's case-management server fails shortly before a court deadline. Staff activate an approved alternative filing procedure while IT restores the server. Which activity is an example of business continuity?**

A. Replacing the failed server's storage hardware  
B. Maintaining essential filing operations through the alternative procedure  
C. Investigating the technical cause of the server failure  
D. Reinstalling the case-management application

### P18

**Which process identifies critical business functions, dependencies, disruption impacts, and recovery priorities?**

A. Business Impact Analysis (BIA)  
B. Vulnerability scanning  
C. Certificate revocation  
D. Password rotation

### P19

**An organization needs an alternate recovery location that is already running or nearly ready for rapid activation, accepting higher costs. Which recovery-site type BEST fits?**

A. Cold site  
B. Unprepared office space  
C. Hot site  
D. Offline tape archive

### P20

**A company switches to an alternate data center during an outage. Employees continue processing transactions there. Before returning to the primary data center, what is MOST important?**

A. Delete all transactions created at the alternate site  
B. Disable audit logging  
C. Assume the primary and alternate databases are identical  
D. Reconcile and synchronize changes before failback

---

## Answer Key & Targeted Study Recommendations

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal diagnostic answer key and explanations — P1–P20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the links below to review the exact concepts you need to strengthen.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| P1 | **C** | [Day 11 — Data at Rest](day-11-data-states.md#3-at-rest-securing-stored-data) | Full-disk encryption protects stored data against many offline attacks. Once a device is unlocked, applications and compromised sessions may access decrypted content. |
| P2 | **B** | [Day 11 — Data in Transit](day-11-data-states.md#4-in-transit-securing-moving-data) | TLS protects data moving between its endpoints. It does not automatically protect stored copies or active processing after decryption. |
| P3 | **A** | [Day 11 — DLP](day-11-data-states.md#6-dlp-masking-tokenization-and-redaction) | DLP can identify, alert on, or block sensitive-data movement to unauthorized destinations. |
| P4 | **D** | [Day 11 — Tokenization](day-11-data-states.md#6-dlp-masking-tokenization-and-redaction) | Tokenization substitutes sensitive values with tokens resolved through a separate protected system. |
| P5 | **B** | [Day 12 — Classification Levels](day-12-data-classification.md#3-classification-levels) | Classification assigns sensitivity or impact levels so organizations can apply proportionate controls. |
| P6 | **C** | [Day 12 — Labels and Enforcement](day-12-data-classification.md#5-labels-and-technical-enforcement) | A label communicates sensitivity but does not restrict access unless technical controls enforce the associated policy. |
| P7 | **A** | [Day 12 — Data Ownership](day-12-data-classification.md#4-under-the-hood-data-types-and-ownership) | The data owner is accountable for business classification and access requirements; custodians typically implement the safeguards. |
| P8 | **D** | [Day 12 — Wrong Attachment Case Study](day-12-data-classification.md#7-case-study-the-wrong-attachment) | Labels may be lost when content is copied. Content-aware DLP, recipient verification, and handling procedures provide complementary protection. |
| P9 | **B** | [Day 13 — Retention Schedules and Legal Holds](day-13-data-retention-disposal.md#3-retention-schedules-and-legal-holds) | A legal hold suspends routine disposal for relevant records, even when their normal retention period has ended. |
| P10 | **C** | [Day 13 — Deletion vs. Sanitization](day-13-data-retention-disposal.md#4-deletion-is-not-sanitization) | SSD wear leveling and remapping can leave data outside ordinary logical overwrites. Appropriate validated sanitization is required. |
| P11 | **A** | [Day 13 — Cryptographic Erase](day-13-data-retention-disposal.md#5-cryptographic-erase-make-the-key-unavailable) | Cryptographic erase relies on effective encryption and elimination of all necessary decryption key material, subject to recovery and retention requirements. |
| P12 | **D** | [Day 13 — Cloud Copies](day-13-data-retention-disposal.md#6-cloud-deletion-and-the-copy-problem) | Snapshots, backups, replicas, and exported copies may persist after production data is deleted. |
| P13 | **B** | [Day 14 — RPO vs. RTO](day-14-backups-recovery.md#4-rpo-versus-rto-two-clocks-two-questions) | RPO expresses the maximum acceptable data-loss window in time. RTO expresses acceptable service-restoration time. |
| P14 | **C** | [Day 14 — Backup Types](day-14-backups-recovery.md#3-full-incremental-and-differential-backups) | Traditional incremental restoration requires the full backup plus every incremental backup in the chain up to the desired recovery point. |
| P15 | **A** | [Day 14 — Backup vs. Redundancy](day-14-backups-recovery.md#2-the-big-idea-your-digital-time-machine) | RAID mirrors changes, including accidental deletions. Historical backups provide earlier recoverable versions. |
| P16 | **D** | [Day 14 — Ransomware Resilience and Restore Testing](day-14-backups-recovery.md#5-3-2-1-backups-and-ransomware-resilience) | Successful backup jobs do not prove data can be restored. Controlled restore tests validate recoverability and help assess actual recovery times. |
| P17 | **B** | [Day 15 — Business Continuity vs. Disaster Recovery](day-15-business-continuity.md#2-the-big-idea-keep-the-organization-working) | Business continuity maintains essential functions during disruption. Disaster recovery restores technology; incident response handles security incidents. |
| P18 | **A** | [Day 15 — Business Impact Analysis](day-15-business-continuity.md#3-business-impact-analysis-bia) | A BIA identifies critical processes, dependencies, disruption impacts, and recovery priorities. |
| P19 | **C** | [Day 15 — Recovery Sites](day-15-business-continuity.md#5-hot-warm-and-cold-recovery-sites) | Hot sites offer the greatest readiness and generally higher cost. Warm and cold sites require more preparation before operation. |
| P20 | **D** | [Day 15 — Failover and Failback](day-15-business-continuity.md#5-hot-warm-and-cold-recovery-sites) | Data created at an alternate site must be reconciled before failback to avoid lost, duplicated, or inconsistent transactions. |

</details>

---

## Your Diagnostic Scorecard

Record your results by theme, not just your overall score.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 11 — Data States](day-11-data-states.md) | P1–P4 | ____ / 4 |
| [Day 12 — Data Classification](day-12-data-classification.md) | P5–P8 | ____ / 4 |
| [Day 13 — Retention & Disposal](day-13-data-retention-disposal.md) | P9–P12 | ____ / 4 |
| [Day 14 — Backups & Recovery](day-14-backups-recovery.md) | P13–P16 | ____ / 4 |
| [Day 15 — Business Continuity](day-15-business-continuity.md) | P17–P20 | ____ / 4 |
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
- Which data-protection or recovery concepts were unfamiliar?
- Could you explain why the incorrect options were wrong?
- Which lessons should you prioritize?

Treat lucky guesses as opportunities for review, even if your answer was correct.

---

## Build Your Personalized Study Plan

Use your diagnostic results to decide which lessons to complete.

- **Unsure how to protect data throughout its journey?** Start with [Day 11 — Data at Rest, in Transit & in Use](day-11-data-states.md).
- **Confused about classification, labels, or data ownership?** Study [Day 12 — Data Classification, Labeling & Handling](day-12-data-classification.md).
- **Need to understand legal holds, sanitization, or secure deletion?** Review [Day 13 — Data Retention, Disposal & Minimization](day-13-data-retention-disposal.md).
- **Want to strengthen backup and recovery knowledge?** Complete [Day 14 — Backups, RPO, RTO & Resilience](day-14-backups-recovery.md).
- **Unsure about continuity plans, recovery sites, or BIA?** Study [Day 15 — Business Continuity & Disaster Recovery](day-15-business-continuity.md).

## What Comes Next?

**Ready to start learning?**

Begin with [Day 11 — Protecting Data at Rest, in Transit, and in Use](day-11-data-states.md), or jump directly to the lessons identified by your diagnostic results.

**Already confident in all five themes?**

You can move directly to the [Week 03 End-of-Week Review](review.w3.md), which will contain a different set of questions and cumulative scenarios.

The review gives you another opportunity to demonstrate what you know and identify any remaining knowledge gaps.

**Remember: The goal isn't to complete every lesson. The goal is to develop the knowledge and skills needed to succeed in cybersecurity and on the Security+ exam.**

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
