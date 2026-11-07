# Week 03 Review — Data Protection, Retention & Resilience

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 03 · Five themes · 20 review questions + 5 cumulative questions**  
> **Optional · Estimated time:** 35–50 minutes  
> **Prerequisites:** Week 03 lessons or equivalent prior knowledge

## Welcome to your Week 03 Review!

**You've studied the concepts. Now let's see how well you can apply them.**

This optional review helps you evaluate your understanding of the five Week 03 themes, identify any remaining knowledge gaps, and determine which concepts deserve additional practice.

You can complete this review whether you studied all five lessons or skipped some based on your diagnostic results.

**Haven't started the week yet?** Take the [Week 03 Diagnostic](00-diagnostic.w3.md) to identify what you already know and which lessons to prioritize.

### How to complete this review

1. Answer **S1–S20** without consulting your notes or lessons.
2. Record your answers before opening the answer key.
3. Check your results by theme, not just your overall score.
4. Follow the targeted lesson links for questions you missed.
5. Complete **C1–C5**, which combine concepts from multiple lessons.
6. Reflect on your progress and decide whether you're ready for Week 04.

These are original educational practice questions, not actual CompTIA exam items.

## Week 03 Themes

| Questions | Theme | Lesson |
| --- | --- | --- |
| S1–S4 | Data at Rest, in Transit & in Use | [Day 11](day-11-data-states.md) |
| S5–S8 | Data Classification, Labeling & Handling | [Day 12](day-12-data-classification.md) |
| S9–S12 | Data Retention, Disposal & Minimization | [Day 13](day-13-data-retention-disposal.md) |
| S13–S16 | Backups, Recovery Objectives & Resilience | [Day 14](day-14-backups-recovery.md) |
| S17–S20 | Business Continuity & Disaster Recovery | [Day 15](day-15-business-continuity.md) |

---

## Part 1 — End-of-Week Knowledge Check

**Directions:** Choose the single BEST answer to each question. Complete all 20 before revealing the answer key.

### S1

**A law firm stores confidential client documents on encrypted laptops and uses HTTPS for all transfers. An employee accidentally shares a document with an unauthorized recipient. Which statement BEST explains the exposure?**

A. Encryption at rest prevents all unauthorized sharing  
B. TLS guarantees that the intended recipient is authorized  
C. Encryption protects particular data states and connections but does not replace recipient verification and authorization  
D. The file must have been decrypted by an attacker during transmission

### S2

**A medical clinic uses an application that processes sensitive records in memory. Which control BEST addresses the risk of unauthorized access during active processing?**

A. Process isolation and least-privilege access controls  
B. A faster internet connection  
C. A differential backup schedule  
D. A longer retention period

### S3

**An organization wants to release a PDF with sensitive witness information permanently removed. Which approach is MOST appropriate?**

A. Draw black rectangles over the sensitive text  
B. Rename the file to indicate that it is public  
C. Encrypt the PDF and publish the password  
D. Use a proper redaction process and verify that the underlying content is no longer recoverable from the released copy

### S4

**A cloud storage service encrypts documents at rest, but an employee creates a public sharing link to a confidential folder. Which control failure MOST directly explains the exposure?**

A. AES cannot protect cloud data  
B. Inadequate access and sharing restrictions  
C. TLS is incompatible with cloud storage  
D. Hashing automatically makes files public

### S5

**An organization classifies a witness contact list as Restricted. Which handling requirement BEST matches that classification?**

A. Publish the list on the organization's website  
B. Allow anyone with a link to download it  
C. Limit access to specifically authorized personnel and use approved secure sharing  
D. Remove the classification label before transferring it

### S6

**A company's data owner establishes that confidential contracts require restricted sharing. Which responsibility would typically belong to the data custodian?**

A. Implementing the required permissions and technical safeguards  
B. Deciding the legal meaning of every contract  
C. Reclassifying all information as Public  
D. Eliminating the need for data ownership

### S7

**A public website contains instructions for submitting payments. An attacker changes the destination bank account without authorization. Which statement is MOST accurate?**

A. Public information does not require security controls  
B. Only confidentiality has been compromised  
C. Classification automatically prevents modification  
D. Public information still requires integrity protection

### S8

**A document is marked Confidential, but an employee copies its contents into an unlabeled spreadsheet. Which security approach BEST reduces the risk of unauthorized disclosure?**

A. Depend only on the original filename  
B. Combine content-aware DLP with appropriate access controls and handling procedures  
C. Assume all labels are preserved automatically  
D. Disable all document monitoring

### S9

**A law firm is retiring a collection of encrypted laptops. Some devices contain SSDs, while others contain HDDs. What should administrators do FIRST when planning secure disposal?**

A. Apply the same overwrite command to every device  
B. Empty the Trash on each laptop  
C. Inventory the devices, verify retention and recovery obligations, and select suitable sanitization methods  
D. Remove the devices from the asset register without further checks

### S10

**A company wants to use cryptographic erase to retire an encrypted storage device. Which condition is ESSENTIAL for the process to work as intended?**

A. All relevant data was appropriately encrypted, and the necessary decryption material can be effectively eliminated  
B. The device contains only public data  
C. The encryption keys remain available in an unrestricted backup  
D. The device uses an SSD rather than an HDD

### S11

**An employee deletes a confidential file from a cloud collaboration platform, but the file remains in version history and immutable backups. What is the BEST explanation?**

A. Cloud providers never delete information  
B. Deletion from the active workspace does not necessarily remove retained copies  
C. TLS has failed to encrypt the file  
D. The file has automatically become public

### S12

**A nonprofit wants to reduce the risk associated with retaining unnecessary applicant records. Which principle BEST supports this objective?**

A. High availability  
B. Data replication  
C. Port security  
D. Data minimization

### S13

**A company creates a full backup on Sunday and differential backups on Monday, Tuesday, and Wednesday. In a traditional differential-backup scheme, which backups are required to restore Wednesday's data?**

A. Sunday's full backup and Wednesday's differential backup  
B. Sunday's full backup plus every differential backup  
C. Wednesday's differential backup only  
D. Tuesday's and Wednesday's differential backups

### S14

**A database fails at 14:00. The latest usable backup is from 13:30, and the service returns at 16:15. Which statement is correct?**

A. The potential data-loss window is 2 hours 15 minutes, and restoration took 30 minutes  
B. Both the potential data-loss window and restoration time are 30 minutes  
C. The potential data-loss window is 30 minutes, and restoration took 2 hours 15 minutes  
D. Both values are 2 hours 15 minutes

### S15

**A law firm's file server and backup NAS share the same administrator credentials and are always connected to the same network. Which improvement BEST reduces the risk of ransomware destroying both production data and backups?**

A. Store all backups on the production server  
B. Use separately administered offline or immutable backups, including an offsite copy  
C. Disable backup alerts  
D. Increase the number of mirrored disks without changing access controls

### S16

**A backup system reports successful jobs every night, but the organization has never restored a complete application. What is the MOST important remaining concern?**

A. Backups automatically guarantee recovery within the RTO  
B. The organization has too many recovery points  
C. Backup encryption is unnecessary  
D. Backup-job success does not demonstrate complete application recoverability or realistic restoration time

### S17

**A ransomware incident disables a legal clinic's document server. Staff use an approved alternative workflow to meet a filing deadline while IT restores clean systems. Which pairing BEST describes the activities?**

A. Business continuity maintains the workflow; disaster recovery restores the technology  
B. Disaster recovery maintains the workflow; business continuity investigates the attacker  
C. Incident response replaces all continuity planning  
D. Business continuity is limited to restoring hardware

### S18

**A company has limited recovery resources after a major outage. Which factor should MOST strongly determine the order in which systems are restored?**

A. The original purchase price of each server  
B. The number of employees who know the system administrator  
C. The business impact and critical dependencies identified through the BIA  
D. The alphabetical order of system names

### S19

**Managers meet to discuss their roles and decisions during a simulated ransomware incident without changing production systems. Which exercise are they conducting?**

A. Full-scale failover  
B. Tabletop exercise  
C. Penetration test  
D. Cryptographic erase

### S20

**An organization operates from a warm recovery site during an outage. When the primary site is repaired, what should happen BEFORE failback?**

A. Immediately discard all alternate-site transactions  
B. Assume the primary database is current  
C. Disable monitoring to simplify the transition  
D. Validate the restored environment and reconcile data created at the alternate site

---

## Part 2 — Answer Key & Targeted Review

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal Week 03 answer key and explanations — S1–S20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the targeted lesson links to review any concepts you missed.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| S1 | **C** | [Day 11 — Data in Transit](day-11-data-states.md#4-in-transit-securing-moving-data) | TLS protects traffic between its endpoints, but does not establish that the recipient is appropriate or authorized. |
| S2 | **A** | [Day 11 — Data in Use](day-11-data-states.md#5-in-use-securing-active-processing) | Process isolation and least privilege help restrict access to sensitive data while applications process it. |
| S3 | **D** | [Day 11 — Redaction](day-11-data-states.md#6-dlp-masking-tokenization-and-redaction) | Proper redaction removes sensitive content from the released copy. A visual overlay may leave underlying text recoverable. |
| S4 | **B** | [Day 11 — Data at Rest](day-11-data-states.md#3-at-rest-securing-stored-data) | Storage encryption does not replace authorization or secure sharing configurations. |
| S5 | **C** | [Day 12 — Classification Levels](day-12-data-classification.md#3-classification-levels) | Restricted data requires appropriately strict access and handling controls under the organization's classification scheme. |
| S6 | **A** | [Day 12 — Data Ownership](day-12-data-classification.md#4-under-the-hood-data-types-and-ownership) | Data owners establish business requirements; custodians implement technical and operational safeguards. |
| S7 | **D** | [Day 12 — Classification Levels](day-12-data-classification.md#3-classification-levels) | Public information may not require confidentiality, but unauthorized modification can still create serious integrity risks. |
| S8 | **B** | [Day 12 — Labels and Enforcement](day-12-data-classification.md#5-labels-and-technical-enforcement) | Labels may be lost when content is copied. Content-aware controls and handling procedures provide additional protection. |
| S9 | **C** | [Day 13 — Retiring Laptops](day-13-data-retention-disposal.md#7-case-study-retiring-25-law-firm-laptops) | Secure disposal begins with inventory, retention and legal-hold checks, backup verification, and selection of suitable media sanitization. |
| S10 | **A** | [Day 13 — Cryptographic Erase](day-13-data-retention-disposal.md#5-cryptographic-erase-make-the-key-unavailable) | Cryptographic erase depends on effective encryption and elimination of all relevant decryption key material. |
| S11 | **B** | [Day 13 — Cloud Deletion](day-13-data-retention-disposal.md#6-cloud-deletion-and-the-copy-problem) | Versions, snapshots, backups, and replicas can remain after active files are deleted. |
| S12 | **D** | [Day 13 — Data Lifecycle](day-13-data-retention-disposal.md#2-the-big-idea-data-has-a-lifecycle) | Data minimization limits unnecessary collection and retention, reducing exposure. |
| S13 | **A** | [Day 14 — Backup Types](day-14-backups-recovery.md#3-full-incremental-and-differential-backups) | Traditional differential recovery requires the most recent full backup and the latest differential backup. |
| S14 | **C** | [Day 14 — RPO and RTO](day-14-backups-recovery.md#4-rpo-versus-rto-two-clocks-two-questions) | The backup predates failure by 30 minutes, while service restoration took 2 hours 15 minutes. These are observed windows, not automatically the RPO/RTO targets. |
| S15 | **B** | [Day 14 — Ransomware-Resilient Backups](day-14-backups-recovery.md#5-3-2-1-backups-and-ransomware-resilience) | Administrative separation, offsite copies, and offline or immutable backups reduce an attacker's ability to destroy recovery data. |
| S16 | **D** | [Day 14 — Backup Resilience](day-14-backups-recovery.md#5-3-2-1-backups-and-ransomware-resilience) | Successful backup jobs do not prove an application, its dependencies, or encryption keys can be restored within the target time. |
| S17 | **A** | [Day 15 — BC, DR, and IR](day-15-business-continuity.md#2-the-big-idea-keep-the-organization-working) | Business continuity maintains essential operations; disaster recovery restores technology. Incident response handles the security event. |
| S18 | **C** | [Day 15 — Business Impact Analysis](day-15-business-continuity.md#3-business-impact-analysis-bia) | A BIA identifies critical functions, dependencies, and impacts to guide recovery priorities. |
| S19 | **B** | [Day 15 — Testing Recovery Plans](day-15-business-continuity.md#6-test-the-plan) | Tabletop exercises test decisions and coordination through discussion rather than live technical changes. |
| S20 | **D** | [Day 15 — Recovery Sites and Failback](day-15-business-continuity.md#5-hot-warm-and-cold-recovery-sites) | Safe failback requires validating the primary environment and reconciling transactions generated at the alternate site. |

</details>

## Your Week 03 Scorecard

Record your results by theme.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 11 — Data States](day-11-data-states.md) | S1–S4 | ____ / 4 |
| [Day 12 — Data Classification](day-12-data-classification.md) | S5–S8 | ____ / 4 |
| [Day 13 — Retention & Disposal](day-13-data-retention-disposal.md) | S9–S12 | ____ / 4 |
| [Day 14 — Backups & Recovery](day-14-backups-recovery.md) | S13–S16 | ____ / 4 |
| [Day 15 — Business Continuity](day-15-business-continuity.md) | S17–S20 | ____ / 4 |
| **TOTAL** | **20 questions** | **____ / 20** |

### Interpreting your results

| Score per theme | Interpretation | Recommended Action |
| --- | --- | --- |
| **4/4** | Strong understanding demonstrated | Continue to cumulative questions and revisit the theme during later spaced practice. |
| **3/4** | Mostly understood | Review the specific concept you missed using the targeted lesson link. |
| **0–2/4** | Knowledge gaps remain | Revisit the full lesson, worked examples, and optional lab before moving on. |

**Remember:** A perfect score on four questions is encouraging, but it does not establish complete mastery. Use the cumulative scenarios below to test whether you can connect and apply what you've learned.

### Compare Your Progress

If you completed the [Week 03 Diagnostic](00-diagnostic.w3.md), compare your results.

| Theme | Diagnostic Score | Review Score |
| --- | --- | --- |
| Data at Rest, in Transit & in Use | ____ / 4 | ____ / 4 |
| Data Classification, Labeling & Handling | ____ / 4 | ____ / 4 |
| Data Retention, Disposal & Minimization | ____ / 4 | ____ / 4 |
| Backups, Recovery Objectives & Resilience | ____ / 4 | ____ / 4 |
| Business Continuity & Disaster Recovery | ____ / 4 | ____ / 4 |
| **TOTAL** | **____ / 20** | **____ / 20** |

**Reflection:** Where did your understanding improve? Which concepts still need practice? Did you learn something you thought you already understood?

---

## Part 3 — Cumulative Application

These five questions combine concepts from multiple Week 03 lessons.

They are designed to assess your ability to **apply and connect security concepts**, rather than simply recognize definitions.

Choose the single BEST answer for each question.

### C1

**A law firm stores confidential case files on encrypted laptops and uses HTTPS for sharing. An employee accidentally sends a restricted witness document to the wrong recipient. Which response BEST addresses the underlying weakness?**

A. Replace HTTPS with plain HTTP  
B. Remove all classification labels  
C. Strengthen recipient verification, access restrictions, and DLP controls while investigating the disclosure  
D. Assume encryption makes the disclosure harmless

### C2

**A company wants to donate retired SSDs that previously stored restricted information. Some records remain subject to legal holds, and the devices contain encrypted data. What should administrators do FIRST?**

A. Destroy all encryption keys immediately  
B. Verify preservation requirements, required recoverable copies, and approved sanitization procedures before releasing the devices  
C. Empty the Trash and donate the devices  
D. Disable backup retention policies

### C3

**A law firm's production server and always-connected backup NAS share administrative credentials. Ransomware encrypts production files and attempts to delete the backups. Which improvement BEST supports recovery?**

A. Increase RAID mirroring on the production server  
B. Keep all backups in the same administrative trust boundary  
C. Disable backup monitoring to reduce network traffic  
D. Maintain separately administered, tested, offline or immutable recovery copies with appropriate offsite protection

### C4

**A legal clinic's case-management system fails four hours before a court deadline. The latest usable backup is two hours old, and restoration is expected to take six hours. Which approach BEST addresses the immediate operational need?**

A. Activate an approved business-continuity workaround while disaster recovery proceeds according to criticality and dependencies  
B. Wait for the server to return without notifying anyone  
C. Destroy the existing backups to prevent confusion  
D. Restore the marketing website before evaluating the filing deadline

### C5

**A nonprofit has a one-hour RPO and a four-hour RTO for its donor database. Backups run nightly, no restore test has been performed, and management proposes a cold recovery site. What is the BEST assessment?**

A. The strategy automatically meets both objectives  
B. A cold site guarantees recovery in under four hours  
C. Nightly backups may fail the RPO, and an untested cold site may fail the RTO  
D. RPO and RTO do not apply to donor records

### Cumulative Answer Key

<details markdown="1">
<summary><strong>Reveal cumulative answers and connected lessons — C1–C5</strong></summary>

| Question | Answer | Explanation | Connected Lessons |
| --- | --- | --- | --- |
| C1 | **C** | Encryption and HTTPS protect particular data states and connections, but cannot establish that the recipient is authorized. Classification, recipient verification, access controls, and DLP provide additional protection. | [Day 11](day-11-data-states.md), [Day 12](day-12-data-classification.md) |
| C2 | **B** | Legal holds and retention obligations must be addressed before disposal. Required records and recovery copies must remain accessible, and sanitization must be appropriate for the media and required assurance. | [Day 12](day-12-data-classification.md), [Day 13](day-13-data-retention-disposal.md), [Day 14](day-14-backups-recovery.md) |
| C3 | **D** | Shared credentials and always-connected backups create a common attack path. Administrative separation, offline or immutable copies, offsite protection, and tested restores improve resilience. | [Day 13](day-13-data-retention-disposal.md), [Day 14](day-14-backups-recovery.md), [Day 15](day-15-business-continuity.md) |
| C4 | **A** | Business continuity maintains critical filing operations while disaster recovery restores the affected technology. Recovery decisions should account for deadlines, dependencies, and validated recovery options. | [Day 14](day-14-backups-recovery.md), [Day 15](day-15-business-continuity.md) |
| C5 | **C** | Nightly backups may leave a data-loss window longer than one hour. A cold site requires substantial preparation, and without testing there is no evidence that restoration can meet the four-hour target. | [Day 14](day-14-backups-recovery.md), [Day 15](day-15-business-continuity.md) |

</details>

### Cumulative Reflection

Choose **one scenario** above and explain your reasoning in 3–5 sentences.

1. What facts are directly observed?
2. Which data-protection, retention, or recovery principles are involved?
3. What additional evidence would you need before making a stronger claim?
4. Which control would you recommend, and what limitations would it have?

### Optional Hands-on Reinforcement

If you'd like to apply what you've learned, revisit one of these activities:

- [Day 11 — Map a Data Flow and Inspect HTTPS](day-11-data-states.md#9-hands-on-lab-map-the-flow-and-inspect-https)
- [Day 12 — Build a Classification Matrix](day-12-data-classification.md#9-hands-on-lab-classification-matrix)
- [Day 13 — Design a Disposal Register](day-13-data-retention-disposal.md#9-hands-on-exercise-design-a-disposal-register)
- [Day 14 — Back Up and Restore Fictional Files](day-14-backups-recovery.md#8-hands-on-lab-back-up-and-restore-fictional-files)
- [Day 15 — Business Continuity Tabletop](day-15-business-continuity.md#8-hands-on-tabletop-lab-20-minutes)

These activities develop practical skills that multiple-choice questions alone cannot fully assess.

---

## Week 03 — Final Reflection

Before continuing, ask yourself:

- Can I distinguish data at rest, in transit, and in use, and select appropriate controls for each?
- Can I explain the difference between data classification, labeling, and handling?
- Can I identify the responsibilities of data owners and custodians?
- Can I explain data minimization, legal holds, and secure disposal?
- Can I distinguish Clear, Purge, Destroy, and cryptographic erase?
- Can I compare full, incremental, and differential backups?
- Can I calculate potential data-loss windows and restoration durations, and distinguish them from RPO and RTO targets?
- Can I explain the difference between business continuity, disaster recovery, and incident response?
- Can I identify appropriate recovery sites, recovery priorities, and testing methods?

**If you can confidently explain and apply these concepts, you're ready to move forward.**

If some concepts still feel uncertain, use your answer-key links to revisit the relevant sections. You don't necessarily need to repeat an entire lesson.

## What's Next?

**Congratulations on completing Week 03 — Data Protection, Retention & Resilience!**

You've explored how organizations protect information throughout its lifecycle, classify and handle sensitive records, retain and dispose of data responsibly, and prepare to recover from disruptions.

These concepts are foundational to secure operations, risk management, incident response, and organizational resilience.

In **Week 04**, you'll begin exploring networking foundations, including packets, ports, protocols, and how network communication supports modern cybersecurity.

**Ready to continue?** Move on to Week 04 and begin with its diagnostic assessment.

Remember: The goal isn't simply to finish the lessons. It's to understand the concepts well enough to recognize, explain, and apply them in real-world situations.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
