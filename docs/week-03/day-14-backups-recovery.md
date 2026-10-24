[SecurityPlus_Lesson_14_Backups_Recovery.md](https://github.com/user-attachments/files/33915510/SecurityPlus_Lesson_14_Backups_Recovery.md)
# Day 14 — Backups, Recovery Objectives & Resilience

> **CompTIA Security+ SY0-701** · **Week 03, Lesson 4 of 5** · Domain 3: Security Architecture · Objective **3.4 (resilience and recovery)**  
> **Level:** Beginner–intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 13 — Data Retention & Disposal](day-13-data-retention-disposal.md)  
> **Next:** Day 15 — Business Continuity & Disaster Recovery

## 1. Learning objectives

By the end, you should be able to distinguish backups from redundancy; compare full, incremental, differential, snapshots, and images; calculate RPO and RTO; explain 3-2-1, immutable backups, and restore testing; and design a practical recovery strategy.

## 2. The big idea: Your digital time machine

Imagine a law firm discovers ransomware has encrypted its case files. It has two servers—but both contain the same encrypted files. Can the firm recover yesterday's clean documents?

**A backup is a recoverable copy of data or system state.** It is not the same as redundancy.

| Concept | Purpose | Important limitation |
| --- | --- | --- |
| Backup | Recover historical data | Doesn't guarantee uninterrupted service |
| Redundancy | Survive a component failure | May replicate corruption or deletion |
| High availability | Minimize downtime | Doesn't replace historical recovery points |
| Disaster recovery | Restore technology after disruption | Requires planning, resources, and tests |
| Business continuity | Maintain essential business functions | May require manual workarounds while IT recovers |

🧠 **Memory hook:** Backup = **time machine**. Redundancy = **spare tire**. High availability = **keep the car running**. 🚗

**Exam trap:** RAID mirroring is **not** a backup. Deleting or encrypting a mirrored file can affect both disks.

## 3. Full, incremental, and differential backups

**Full:** Copies all selected data. Straightforward restore, but consumes more time and storage.

**Incremental:** Copies changes **since the most recent backup**. Smaller backup jobs, but a traditional restore may need a full plus every subsequent incremental.

**Differential:** Copies changes **since the last full backup**. A traditional restore needs the full and the most recent differential; differential files tend to grow over time.

| Day | Incremental backup contains | Differential backup contains |
| --- | --- | --- |
| Sunday | Full baseline | Full baseline |
| Monday | Monday changes | Monday changes |
| Tuesday | Tuesday changes | Monday + Tuesday changes |
| Wednesday | Wednesday changes | Monday + Tuesday + Wednesday changes |

To restore Wednesday: **incremental** = Sunday full + Monday + Tuesday + Wednesday increments. **Differential** = Sunday full + Wednesday differential.

**Snapshot:** A point-in-time representation of storage or a VM, often implemented with copy-on-write. A snapshot on the same storage is not necessarily an independent backup.

**System image:** Captures a larger system state, potentially including OS, applications, configuration, and files. Useful for rebuilding machines, but restoration compatibility must be tested.

Modern backup systems may synthesize full backups or expose instant recovery points; know the *classic* distinctions for the exam.

## 4. RPO versus RTO: Two clocks, two questions

**Recovery Point Objective (RPO):** Maximum acceptable **data loss measured in time**. How far back can we afford to restore?

**Recovery Time Objective (RTO):** Maximum acceptable **time to restore service**. How long can we be down?

| Metric | Question | Example target |
| --- | --- | --- |
| RPO | How much recent work may be lost? | At most 1 hour |
| RTO | How long may the service be unavailable? | At most 2 hours |

### Worked scenario

A database fails at **14:00**. Its latest usable recovery point is **13:30**. Service returns at **16:15**.

- **Actual potential data-loss window:** 30 minutes.
- **Actual recovery duration:** 2 hours 15 minutes.
- If RPO target was **1 hour**, it was met.
- If RTO target was **2 hours**, it was missed.

**Important:** RPO and RTO are *targets*, not automatically the measured loss or recovery time. Actual data loss can be smaller if no transactions occurred during the gap.

🧠 **Memory hook:** R**P**O = how far back to a recovery **P**oint; R**T**O = how much **T**ime until service returns.

Business owners set appropriate objectives using a **business impact analysis (BIA)**. Not every application needs the same recovery speed.

## 5. 3-2-1 backups and ransomware resilience

A common starting principle is **3-2-1**:

1. **Three copies** of important data, including production.
2. **Two different media or storage failure domains**, where practical.
3. **One offsite copy**.

For ransomware resilience, add **offline or immutable** copies, administrative separation, and verified restores. Some teams call expanded strategies **3-2-1-1-0**: an additional offline/immutable copy and zero unverified backup errors; exact implementations vary.

### Technical safeguards

| Safeguard | Why it matters |
| --- | --- |
| Separate backup admin accounts | Compromised production credentials shouldn't delete recovery copies |
| Immutable retention | Prevents deletion or alteration during a defined retention window when correctly configured |
| Offline or disconnected copies | Reduces network-reachable attack surface |
| Geographic separation | Protects against fire, flood, theft, and regional disruption |
| Backup encryption and recoverable keys | Protects confidential archives without losing access to them |
| Monitoring | Detects failed jobs, missing agents, and suspicious deletions |
| Restore drills | Demonstrates recoverability and realistic RTO |

**Nuance:** Immutable backups may preserve **already infected or corrupted data**. You still need clean historical recovery points and validation.

**Replication is not a backup:** A replica can promptly copy malicious encryption or accidental deletion.

## 6. Case study: A law firm's backup system fails

A 20-person law firm backs up its shared case server nightly to a NAS in the same office. The NAS is always online, and its administrator password is the same as the file server's.

An attacker compromises the server, encrypts its files, and attempts to delete the NAS backups.

**What failed?** The firm had backups, but not enough *separation*. Both storage locations shared a physical site and administrative trust boundary.

**Better design:**

- Maintain an offsite, separately administered backup copy.
- Use immutable retention or an offline copy for critical data.
- Apply MFA and least privilege to backup management where supported.
- Encrypt backups and securely retain recovery keys.
- Monitor jobs and test restores, including a complete application recovery.
- Document who authorizes recovery and which systems return first.

**Incident-response note:** Before restoring after ransomware, contain the incident, investigate the compromise, identify a known-good recovery point, and avoid immediately reconnecting restored systems to an unsafe environment.

## 7. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Why is RAID not a backup? | _Write here._ |
| Full vs. incremental vs. differential? | _Write here._ |
| Define RPO and RTO. | _Write here._ |
| What does 3-2-1 mean? | _Write here._ |
| Why use separate backup credentials? | _Write here._ |
| Why test restores? | _Write here._ |

<details>
<summary><strong>Reveal completed guided-notes table</strong></summary>

| Prompt | Model answer |
| --- | --- |
| RAID | Redundancy may mirror deletion/corruption and lacks historical recovery points. |
| Types | Full copies everything selected; incremental changes since last backup; differential changes since last full. |
| RPO/RTO | RPO = acceptable data-loss time; RTO = acceptable service-restoration time. |
| 3-2-1 | Three copies, two media/failure domains, one offsite. |
| Credentials | Prevent a production account compromise from automatically controlling backups. |
| Restore testing | Successful jobs don't prove files, keys, databases, or systems can be restored within the target. |

</details>

## 8. Hands-on lab: Back up and restore fictional files

**Time:** 20 minutes · **Tools:** macOS or Linux terminal. Commands affect only a new practice folder.

**Create sample data:**

```bash
mkdir -p ~/secplus-day14/{source,backups,restore}
printf 'Case A: version 1\n' > ~/secplus-day14/source/case-a.txt
printf 'Case B: version 1\n' > ~/secplus-day14/source/case-b.txt
```

**Create a full backup archive:**

```bash
cd ~/secplus-day14
tar -czf backups/full-01.tar.gz -C source .
```

**Simulate corruption and restore separately:**

```bash
printf 'Case A: corrupted version\n' > source/case-a.txt
tar -xzf backups/full-01.tar.gz -C restore
cat restore/case-a.txt
```

Expected: `Case A: version 1`.

**Compare hashes:**

```bash
shasum -a 256 source/case-a.txt restore/case-a.txt
```

On Linux, `sha256sum` may be available instead. Check archive listing with `tar -tzf backups/full-01.tar.gz`.

**Think like an analyst:** Did you recover an earlier version? Would this backup survive theft of the laptop? Did the exercise prove an entire application could be recovered?

<details>
<summary><strong>Reveal lab answers</strong></summary>

Yes, the restored file contains the earlier version. No, a backup stored on the same device shares its failure domain. No, restoring a text file does not validate a full application, database consistency, dependencies, or a real-world RTO.

</details>

## 9. Eight original Security+-style questions

Choose the **BEST** answer before opening each explanation. These are original educational questions, not actual CompTIA exam items.

### Q1. A business can lose at most 30 minutes of transaction history. Which objective applies?

A. RTO  
B. RPO  
C. MTTR  
D. MTBF

<details><summary>Reveal answer and explanation</summary>

**B — RPO.** It describes acceptable data-loss time. RTO describes acceptable downtime; MTTR and MTBF are reliability metrics.

</details>

### Q2. A service must return within two hours after an outage. Which objective is specified?

A. RPO  
B. MTTF  
C. RTO  
D. Retention period

<details><summary>Reveal answer and explanation</summary>

**C — RTO.** It sets the maximum acceptable restoration time, not the amount of data loss.

</details>

### Q3. A Sunday full backup is followed by Monday, Tuesday, and Wednesday incremental backups. Which are traditionally needed to restore Wednesday?

A. Wednesday incremental only  
B. Sunday full plus Wednesday incremental  
C. Sunday full plus all three incrementals  
D. Tuesday and Wednesday incrementals

<details><summary>Reveal answer and explanation</summary>

**C.** Traditional incremental recovery requires the full and the complete chain of subsequent incremental backups.

</details>

### Q4. A Sunday full backup is followed by daily differential backups. What is normally needed to restore Wednesday?

A. Sunday full plus Wednesday differential  
B. Every differential since Sunday  
C. Wednesday differential only  
D. Tuesday plus Wednesday differentials

<details><summary>Reveal answer and explanation</summary>

**A.** Each differential contains changes since the last full, so the latest differential plus that full suffices in the classic model.

</details>

### Q5. Which solution BEST protects backups from ransomware using stolen production admin credentials?

A. A writable production share  
B. Shared administrator passwords  
C. Separately administered immutable or offline backups  
D. Disabled backup alerts

<details><summary>Reveal answer and explanation</summary>

**C.** Separate administration plus immutability or offline storage reduces an attacker's ability to destroy recovery copies.

</details>

### Q6. RAID mirrors a database to another disk. A mistaken deletion appears on both disks. What was missing?

A. Additional mirroring  
B. Recoverable historical backup  
C. Faster Ethernet  
D. Longer passwords

<details><summary>Reveal answer and explanation</summary>

**B.** Mirroring protects against some disk failures, not replicated deletion. Backups retain earlier recoverable versions.

</details>

### Q7. A database fails at 14:00; latest backup is 13:30; service returns at 16:15. Which is correct?

A. Potential loss 2h15m; recovery 30m  
B. Potential loss 30m; recovery 2h15m  
C. Both 30m  
D. Both 2h15m

<details><summary>Reveal answer and explanation</summary>

**B.** The backup predates failure by 30 minutes; service restoration took 2 hours 15 minutes. These are measured windows, not automatically the defined objectives.

</details>

### Q8. Every backup job reports success, but nobody has tested recovery. What should IT do NEXT?

A. Assume restoration will work  
B. Disable backup encryption  
C. Perform and document a controlled restore test  
D. Delete historical copies

<details><summary>Reveal answer and explanation</summary>

**C.** A completed job does not prove data usability, key availability, database consistency, or acceptable restoration time.

</details>

**Score guide:** 7–8 correct: explain RPO/RTO and both backup chains from memory. 5–6: review Sections 3–5. 0–4: redo the worked timeline and lab.

## 10. Discussion challenge

> A law firm backs up 1 TB of client files nightly to an always-connected NAS in the same office. The NAS and file server share an administrator password. Management says, “We have backups, so ransomware isn't a concern.”

What are the three biggest weaknesses? Which additional backup copy would you prioritize? How would you determine RPO/RTO? What restore test would prove readiness?

**Bonus:** If an encrypted backup survives but its only decryption key is lost, which CIA objective is most directly threatened?

## 11. Study from the Treadmill 🏃

**Watch these specific lessons:**

1. [Professor Messer — Backups (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/backups-sy0-701/) — backup approaches and restoration concepts.
2. [Professor Messer — Business Continuity (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/business-continuity-sy0-701/) — resilience planning and recovery priorities.

**Active listening:** Which backup type grows since the last full? Why is RAID not a backup? What would make a backup unusable even if the file exists?

## 12. One-minute recap

- **Full:** All selected data.
- **Incremental:** Changes since most recent backup.
- **Differential:** Changes since last full.
- **RPO:** Acceptable data-loss time.
- **RTO:** Acceptable restoration time.
- **3-2-1:** Three copies, two media/failure domains, one offsite.
- **Immutable/offline backups:** Reduce attacker access to recovery copies.
- **Restore testing:** The only way to demonstrate practical recoverability.

**Next: Day 15 — Business Continuity, Disaster Recovery & Week 3 Review.**

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
