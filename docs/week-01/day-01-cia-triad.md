[SecurityPlus_Lesson_01_CIA_Triad.md](https://github.com/user-attachments/files/33914801/SecurityPlus_Lesson_01_CIA_Triad.md)
# Day 01 — The CIA Triad: What Are We Actually Protecting?

> **CompTIA Security+ SY0-701** · Domain 1: General Security Concepts · Objective 1.2 (fundamental security concepts)  
> **Level:** Beginner · **Estimated time:** 45–65 minutes (plus optional video)  
> **Prerequisites:** None

Welcome to Security+, one day at a time. You don't need to know how to code, configure a firewall, or decode a packet to start. Today you'll learn the three goals that give most security decisions their purpose.

## What you'll be able to do

By the end of this lesson, you should be able to:

1. Define **confidentiality**, **integrity**, and **availability** in your own words.
2. Identify which of the three is affected in a realistic incident—and recognize when *more than one* is affected.
3. Match common security controls to the objectives they help protect.
4. Explain why a security control can strengthen one objective while making another harder to achieve.
5. Interpret Security+-style scenarios using clues rather than memorized keywords.

## 1. The big idea: Security is more than keeping secrets

Imagine a hospital that keeps patient records in a locked room. Nobody unauthorized can read them. Great for **confidentiality**.

Now imagine someone quietly changes a patient's listed medication dose. The room is still locked, but the records are no longer trustworthy. That's an **integrity** failure.

Finally, imagine the hospital loses power and clinicians cannot retrieve records during an emergency. Even if the records remain secret and accurate, **availability** has failed.

These three goals form the **CIA triad**:

| Objective | Core question | Failure example |
| --- | --- | --- |
| **Confidentiality** | Who is allowed to see this information? | A stranger downloads private patient files. |
| **Integrity** | Can we trust that the information is accurate and unaltered? | Someone changes an invoice's bank account number. |
| **Availability** | Can authorized people use the information or system when needed? | A ransomware incident makes records inaccessible. |

**Memory hook:** Think of a restaurant's secret recipe. **C**: only the chefs can read it. **I**: nobody replaces the salt with sugar. **A**: the chefs can actually find it during dinner service. A perfectly secret recipe locked in an unreachable vault is a very secure way to go out of business. 🍝

> **Important:** The CIA in *CIA triad* stands for **Confidentiality, Integrity, Availability**—not the intelligence agency. Your first cybersecurity mission does not require sunglasses indoors.

## 2. Confidentiality: Prevent unauthorized disclosure

Confidentiality means information is disclosed only to authorized people, processes, or systems. It is not the same as secrecy at all costs: a physician should be able to read a patient's chart, while a random visitor should not.

### Controls that support confidentiality

- **Access control:** Give each identity permissions appropriate to its role. A receptionist may schedule appointments without accessing clinical notes.
- **Encryption:** Convert readable **plaintext** into **ciphertext** that cannot be understood without the required cryptographic key. Encryption can protect stored files (**at rest**) and network communications (**in transit**).
- **Multifactor authentication (MFA):** Require evidence from different factor categories, making account takeover harder.
- **Data classification:** Label information by sensitivity so the organization can apply appropriate handling rules.
- **Least privilege:** Give users only the access they need to perform their work.

**Technical nuance:** Encryption does not automatically prevent data leaks. If an attacker signs in as an authorized user, an application may decrypt data for them. Security depends on identity, permissions, endpoint security, and monitoring as well as cryptography.

### Real-world example

A paralegal accidentally shares a cloud folder using an **“Anyone with the link”** setting. Confidential client documents become accessible to people outside the firm. The files were not necessarily changed or deleted, but their **confidentiality** was compromised.

## 3. Integrity: Protect trustworthiness

Integrity means information and systems remain accurate, complete, and protected from unauthorized or improper modification. Changes aren't inherently bad: authorized staff *should* be able to update records. The goal is to prevent or detect unapproved changes.

### Controls that support integrity

- **Cryptographic hashes:** Algorithms such as SHA-256 generate a fixed-length digest from data. Comparing a downloaded file's hash to a trusted expected hash can reveal changes.
- **Digital signatures:** Help verify that signed data has not changed and that the corresponding private key was used to sign it, provided the signing key's identity is trusted.
- **Permissions and change control:** Restrict who may edit sensitive records and require review for important changes.
- **File integrity monitoring (FIM):** Detect unexpected changes to monitored files.
- **Audit logs:** Record actions to help reconstruct what changed and when; protect the logs against tampering.

**Technical nuance:** A hash does not stop changes—it helps *detect* them. A malicious party who can replace both a file and an unprotected published hash may fool a simple comparison. Always obtain the expected digest from a trustworthy source.

### Real-world example

A law firm's client receives an invoice that looks legitimate, but the bank account number has been replaced. The attacker did not need to destroy the invoice; altering one field could redirect payment. This is an **integrity** failure, potentially alongside a confidentiality failure if the attacker accessed private correspondence.

## 4. Availability: Keep systems usable

Availability means authorized users can access systems and data when they need them. An inaccessible system can be a security failure even when no information is stolen.

### Controls that support availability

- **Redundancy:** Maintain spare components or services so one failure does not cause total outage.
- **Failover:** Shift operations to a standby system when the primary fails.
- **Backups and tested restoration:** Recover data and services after corruption, deletion, or ransomware.
- **DDoS mitigation:** Filter or absorb malicious traffic intended to overwhelm a service.
- **Uninterruptible power supplies (UPS):** Provide temporary power to support safe shutdown or continuity.
- **Capacity planning and monitoring:** Detect resource exhaustion before users lose service.

**Technical nuance:** A backup is not the same as high availability. A backup may allow recovery *after hours of downtime*. Redundancy and failover aim to reduce interruption in the first place. Later we'll learn **RTO** (target recovery time) and **RPO** (acceptable data-loss window).

### Real-world example

A court filing portal is unreachable just before a deadline because a denial-of-service attack overwhelms its infrastructure. Even if no filing is stolen or altered, **availability** has been compromised.

## 5. The objectives interact—and sometimes compete

A single event can affect multiple CIA objectives. Ransomware might encrypt operational files (**availability**), modify or destroy data (**integrity**), and steal copies for extortion (**confidentiality**). Do not assume every ransomware incident involves theft; look for evidence in the scenario.

Controls can also create trade-offs. Requiring multiple approvals before granting emergency access can protect confidentiality but delay availability. Making everything universally accessible improves convenience while threatening confidentiality. Good security balances risk, usability, and the organization's mission.

### Quick mapping table

| Situation | Most directly affected | Why |
| --- | --- | --- |
| An outsider reads an employee's private HR file | Confidentiality | Unauthorized disclosure |
| An attacker alters a financial record | Integrity | Unauthorized modification |
| A server outage prevents users from accessing files | Availability | Service cannot be used |
| Ransomware steals and encrypts files | Confidentiality **and** availability (possibly integrity) | Theft and inaccessibility; assess any modifications |
| An admin changes an approved record correctly | None necessarily | Authorized change is not itself an integrity failure |

## 6. Worked case study: The disappearing case files

A small law firm uses a cloud document system. On Monday morning:

1. A staff member discovers that a shared folder was configured for public access.
2. A case document contains a payment instruction nobody approved.
3. The document service goes offline for two hours.

**Your analysis:**

- **Event 1 → Confidentiality:** Sensitive material could be exposed. Investigate access logs to determine whether unauthorized access occurred; a public setting creates exposure but does not alone prove someone downloaded the files.
- **Event 2 → Integrity:** An unauthorized or unexplained edit calls the record's trustworthiness into question. Check version history, change logs, and the source of the instruction.
- **Event 3 → Availability:** Authorized staff cannot use the service. Investigate provider status, connectivity, and recovery options.

**Security analyst mindset:** Describe the observed facts first, then the suspected impact, then what evidence would confirm it. An exam scenario may tell you the impact directly; a real investigation rarely does.

## 7. Take notes that you can actually use

Use a two-column page, digital note, or flashcards. Don't transcribe this chapter word-for-word; retrieve the ideas from memory.

| Prompt | Your notes |
| --- | --- |
| Explain confidentiality without using the word “secret.” | _Write your answer._ |
| Give two preventive confidentiality controls. | _Write your answer._ |
| How is an integrity check different from encryption? | _Write your answer._ |
| Why are backups not identical to high availability? | _Write your answer._ |
| Give an incident that affects two CIA objectives. | _Write your answer._ |

<details>
<summary><strong>Reveal the completed notes table (sample answers)</strong></summary>

Try filling in your own answers first. These are model responses, not the only correct answers.

| Prompt | Sample answer |
| --- | --- |
| Explain confidentiality without using the word “secret.” | Confidentiality ensures that information is disclosed only to people, systems, or processes authorized to access it. |
| Give two preventive confidentiality controls. | **Least-privilege access permissions** restrict who can read sensitive files; **encryption** helps keep stored or transmitted data unreadable without the necessary key. |
| How is an integrity check different from encryption? | An **integrity check** (such as comparing SHA-256 hashes against a trusted value) helps detect changes to data. **Encryption** transforms plaintext into ciphertext to protect confidentiality; it does not, by itself, prove a file is unchanged. |
| Why are backups not identical to high availability? | **Backups** provide copies that can be restored after data loss or disruption, potentially with downtime. **High availability** uses measures such as redundancy and failover to minimize service interruption. |
| Give an incident that affects two CIA objectives. | **Ransomware that steals files and makes them inaccessible** affects confidentiality (unauthorized disclosure) and availability (users cannot access files). If data is improperly altered, integrity may also be affected. |

</details>

**One-minute recall:** Close this page and explain CIA to an imaginary colleague using a hospital, law firm, or online store. If you can explain it simply *and* accurately, you're learning it.

## 8. Optional hands-on activity (10 minutes)

**File integrity experiment — no special tools required.**

1. Create a text file named `case-note.txt` containing `Invoice total: 500`.
2. On macOS or Linux, run `shasum -a 256 case-note.txt` (on many Linux distributions, `sha256sum case-note.txt` also works). Record the digest.
3. Change the file to `Invoice total: 900`, save, and run the same command again.
4. Compare the digests. Even a small edit should produce a very different-looking result.

**What this demonstrates:** A hash can reveal that file content changed. It does **not** tell you who edited the file or whether the edit was authorized. For that, you need additional evidence and controls.

## 9. Practice: original Security+-style questions

Choose the **best** answer before expanding each explanation. These are original practice questions, not actual CompTIA exam items.

### Q1
A company employee opens payroll files belonging to another department without permission. No files were changed. Which security objective was **most directly** violated?

A. Availability  
B. Integrity  
C. Confidentiality  
D. Resilience

<details><summary>Answer and explanation</summary>

**C — Confidentiality.** Unauthorized viewing is disclosure. Integrity would involve improper modification, and availability concerns access when needed. Resilience describes an ability to withstand and recover from disruption, not the direct violation here.
</details>

### Q2
A software vendor publishes a SHA-256 digest on its authenticated website. A customer calculates the digest of the downloaded installer and gets a different value. What is the **best** conclusion?

A. The installer is definitely malware.  
B. The installer does not match the expected file bytes.  
C. The installer is encrypted.  
D. The website is unavailable.

<details><summary>Answer and explanation</summary>

**B — The installer does not match the expected bytes.** A mismatch signals different content or an error in the comparison. It does not by itself prove maliciousness. Hashing is not encryption, and availability is unrelated.
</details>

### Q3
A company maintains nightly backups but needs its customer portal to remain online if one server fails. Which additional control **best** addresses that requirement?

A. File hashing  
B. Data classification  
C. Failover to a redundant server  
D. Stronger passwords

<details><summary>Answer and explanation</summary>

**C — Failover to a redundant server.** Backups support recovery; redundancy and failover help maintain service during component failure. The other controls serve different goals.
</details>

### Q4
An attacker encrypts a hospital's operational files and threatens to publish copies they stole. Which CIA objectives are **clearly** affected by the facts given?

A. Confidentiality and availability  
B. Integrity only  
C. Availability only  
D. Confidentiality only

<details><summary>Answer and explanation</summary>

**A — Confidentiality and availability.** Theft threatens confidentiality, while inaccessible operational files threaten availability. File encryption changes data representation, but the scenario doesn't provide enough detail to make integrity the *best* additional answer.
</details>

### Q5
A security administrator enables full-disk encryption on laptops. What is the **primary** security objective of this control?

A. Confidentiality  
B. Availability  
C. Integrity  
D. Load balancing

<details><summary>Answer and explanation</summary>

**A — Confidentiality.** Full-disk encryption helps protect data on a lost or stolen device when encryption keys remain protected. It does not replace backups, integrity monitoring, or access controls while the device is unlocked.
</details>

### Q6
A hospital deploys a backup system and a UPS. Which statement is **most accurate**?

A. Both controls exclusively protect confidentiality.  
B. Both primarily support availability, but in different ways.  
C. A UPS verifies file integrity.  
D. Backups guarantee zero downtime.

<details><summary>Answer and explanation</summary>

**B — Both primarily support availability.** A UPS bridges short power interruptions; backups support restoration after data loss or disruption. Neither guarantees uninterrupted operations.
</details>

### Q7
An authorized accountant updates a transaction record with correct information using approved procedures. What is the **best** assessment?

A. Integrity has necessarily been compromised because the record changed.  
B. Confidentiality has necessarily been compromised.  
C. The change can preserve integrity because it is authorized and accurate.  
D. Availability has necessarily been compromised.

<details><summary>Answer and explanation</summary>

**C — The change can preserve integrity.** Integrity is not a ban on modification. It is about correctness and protection from unauthorized or improper changes.
</details>

### Q8
A financial institution discovers that a payment instruction was modified and a customer transferred money to the wrong account. Which objective should the analyst prioritize when describing the **unauthorized edit itself**?

A. Confidentiality  
B. Integrity  
C. Availability  
D. Authentication

<details><summary>Answer and explanation</summary>

**B — Integrity.** The unauthorized alteration undermined the trustworthiness of the instruction. Other controls might have failed, but the edit is an integrity issue.
</details>

## 10. Discussion / reflection

> A nonprofit manages sensitive information about people seeking legal assistance. Its files are encrypted, but staff share one login and the only backup has never been tested. Which CIA objectives are at risk? What would you fix first, and what additional facts would change your priority?

There is no single universal first step: good answers explain the risk, the organization's needs, and the evidence behind the recommendation.

## 11. Study from the Treadmill 🏃

**Recommended:** Professor Messer's free **CompTIA Security+ SY0-701** course. Find the lesson on *Security Concepts / the CIA Triad* within the course index:  
https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/security-plus-sy0-701-video-course/

**While listening:** Pause after each CIA objective and invent an example from a school, clinic, or small business. Afterward, explain why encryption does not solve every security problem.

## 12. Exit ticket

Before moving on, can you answer these without notes?

- What does each letter in CIA stand for?
- What is the difference between an integrity failure and an availability failure?
- Which CIA objectives might ransomware affect, and what evidence would you need?
- Name one control for each objective and one limitation of each control.

If yes, you're ready for **Day 02 — Security Controls: Categories and Functions**.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.

**Curriculum note:** This chapter is aligned to the SY0-701 concepts. Exam versions change; always confirm the exam code and official objectives for the test you intend to take. This is an independent educational resource, not affiliated with or endorsed by CompTIA.
