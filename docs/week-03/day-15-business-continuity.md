[SecurityPlus_Lesson_15_Business_Continuity.md](https://github.com/user-attachments/files/33915556/SecurityPlus_Lesson_15_Business_Continuity.md)
# Day 15 — Business Continuity, Disaster Recovery & Week 3 Review

> **Security+ SY0-701** · Week 03, Lesson 5/5 · Domain 3, Objective 3.4 · 65–85 minutes  
> Previous: [Day 14 — Backups and Recovery](day-14-backups-recovery.md)  
> Next: Day 16 — Networking Foundations

## 1. Learning objectives

Distinguish business continuity (BC), disaster recovery (DR), and incident response (IR); explain business impact analysis (BIA); calculate RPO and RTO; compare hot/warm/cold sites; and apply recovery planning in a scenario.

## 2. The big idea: Keep the organization working

A law firm's case-management server fails hours before a court deadline. Restoring it may take all day. Must the firm stop working?

- **Business continuity plan (BCP):** Keep essential business functions operating, perhaps through an approved alternate filing procedure.
- **Disaster recovery plan (DRP):** Restore systems, infrastructure, and data.
- **Incident response plan (IRP):** Detect, contain, investigate, and respond to a security incident.

🧠 **Memory hook:** BC keeps the restaurant serving meals; DR repairs the kitchen; IR investigates who sabotaged the oven. 🍳

A **runbook** contains operational steps. A plan sets objectives, owners, decisions, and communications. These documents should work together.

## 3. Business impact analysis (BIA)

A **BIA** identifies critical business functions, dependencies, disruption impacts, and recovery priorities.

| Function | Possible disruption impact | Priority consideration |
| --- | --- | --- |
| Court filing | Missed legal deadline | Urgent |
| Client communications | Loss of timely contact | High |
| Case documents | Work interruption | High |
| Marketing website | Fewer inquiries | Often lower short-term priority |

Don't recover the most expensive server first merely because it is expensive. Restore the services supporting the most critical processes. Check dependencies: identity provider, network, DNS, database, and third-party vendors.

## 4. Recovery metrics: RPO, RTO, MTTR, MTBF

| Metric | Meaning | Example |
| --- | --- | --- |
| **RPO** | Maximum acceptable data loss, expressed in time | Lose at most 30 minutes of updates |
| **RTO** | Maximum acceptable service restoration time | Restore within 2 hours |
| **MTTR** | Mean time to repair/recover | Average repair duration: 90 minutes |
| **MTBF** | Mean time between failures (repairable systems) | 1,000 hours |

**Worked example:** Database fails at **10:00**. Latest usable backup is **09:40**. Service returns **12:30**.

- Potential data-loss window = **20 minutes**.
- Actual restoration duration = **2 hours 30 minutes**.
- An **RPO target of 30 minutes** was met.
- An **RTO target of 2 hours** was missed.

**Exam trap:** RPO and RTO are **targets**; observed data loss and recovery time are **results**. RPO = data; RTO = downtime.

## 5. Hot, warm, and cold recovery sites

| Site | Readiness | Typical cost | Tradeoff |
| --- | --- | --- | --- |
| **Hot** | Running or nearly ready | High | Fast recovery |
| **Warm** | Partially prepared | Medium | Requires some restoration/configuration |
| **Cold** | Basic facilities, substantial setup needed | Lower | Slowest recovery |

These are relative categories, not guaranteed recovery times. **Failover** switches to a secondary environment; **failback** returns to primary after repairs. Reconcile data created at the alternate site before failback.

**Under the hood:** A replicated database may copy deletions or ransomware damage too. Replication supports availability; versioned, tested backups support historical recovery.

## 6. Test the plan

| Exercise | Activity | Main benefit |
| --- | --- | --- |
| **Tabletop** | Discuss simulated incident | Tests decisions and roles |
| **Walkthrough** | Review procedures step by step | Finds documentation gaps |
| **Functional** | Perform selected recovery tasks | Tests technical execution |
| **Full-scale** | Realistic end-to-end exercise | Tests operational coordination |

A successful backup job does **not** prove you can restore. Verify backup keys, application dependencies, permissions, data integrity, and measured recovery time.

**Real-world case:** Ransomware hits a law firm's file server at 07:30; a court filing is due at noon. IR contains and investigates the attack. BC activates an approved secure alternate filing process. DR selects a validated clean recovery point and restores in a controlled environment. Returning a compromised server to production just to meet a deadline may cause a second incident.

## 7. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| BC vs. DR vs. IR? | _Write here._ |
| What does a BIA identify? | _Write here._ |
| RPO vs. RTO? | _Write here._ |
| MTTR vs. MTBF? | _Write here._ |
| Hot vs. warm vs. cold? | _Write here._ |
| Why test failback? | _Write here._ |

<details>
<summary><strong>Reveal completed notes table</strong></summary>

| Prompt | Model answer |
| --- | --- |
| BC/DR/IR | BC maintains essential functions; DR restores technology; IR handles the security incident. |
| BIA | Critical processes, dependencies, impacts, recovery priorities. |
| RPO/RTO | Acceptable data loss in time / acceptable downtime. |
| MTTR/MTBF | Average repair/recovery duration / average time between failures. |
| Sites | Hot is most prepared, warm partly prepared, cold requires substantial setup. |
| Failback | Prevent loss/inconsistency of data generated on the alternate system. |

</details>

## 8. Hands-on tabletop lab (20 minutes)

**Scenario:** A 12-person legal clinic's document server is offline; an administrator account may be compromised. A filing deadline is four hours away.

Create a matrix with columns: `Time | Decision | Owner | Evidence needed | Next action`.

Plan the **first 15 minutes**, **first hour**, **hours 1–4**, and **after restoration**. Name an incident lead, business owner, DR lead, and communications contact.

<details>
<summary><strong>Reveal example response</strong></summary>

| Time | Decision | Owner | Action |
| --- | --- | --- | --- |
| 0–15 min | Activate response | Incident lead | Contain affected systems and preserve evidence |
| 15–60 min | Prioritize filing | Business owner | Verify deadline; activate approved alternate process |
| 15–60 min | Select clean backup | DR lead | Review backup history and dependencies |
| 1–4 hours | Restore or continue workaround | IT and business lead | Validate service before authorized cutover |
| After | Review | Incident lead | Reconcile data and document lessons learned |

This is an illustrative plan, not a rigid incident-response script.

</details>

## 9. Eight original Security+-style questions

Choose the **BEST** answer before revealing the explanation. These are original practice questions, not actual CompTIA items.

### Q1

A company must keep essential services running while its primary data center is unavailable. Which plan BEST addresses this?

A. Business continuity plan  
B. Password policy  
C. Certificate renewal plan  
D. Vulnerability scanning schedule

<details>
<summary>Reveal answer and explanation</summary>

**A.** BC keeps essential operations functioning. The others are specific security tasks, not continuity planning.

</details>

### Q2

A company tolerates at most 15 minutes of lost transactions. Which metric expresses this?

A. MTBF  
B. RTO  
C. RPO  
D. MTTR

<details>
<summary>Reveal answer and explanation</summary>

**C.** RPO describes acceptable data-loss time. RTO describes downtime; MTBF and MTTR describe reliability and repair.

</details>

### Q3

Which recovery site is MOST prepared for rapid activation, usually at greater cost?

A. Cold  
B. Hot  
C. Unprepared office  
D. Offline tape archive

<details>
<summary>Reveal answer and explanation</summary>

**B.** Hot sites have the greatest readiness. Warm and cold sites need more work before operation.

</details>

### Q4

Which process identifies business-critical functions and impacts of disruption?

A. Business impact analysis  
B. Port scan  
C. Hash comparison  
D. Packet capture

<details>
<summary>Reveal answer and explanation</summary>

**A.** BIA establishes critical functions, dependencies, and recovery priorities.

</details>

### Q5

An alternate site handled new transactions. What is the MAIN risk of failing back without reconciliation?

A. Improved encryption  
B. Loss or inconsistency of newer data  
C. Certificate revocation  
D. Higher MTBF

<details>
<summary>Reveal answer and explanation</summary>

**B.** Failback without synchronization may discard or duplicate changes made at the alternate site.

</details>

### Q6

Managers discuss a simulated ransomware event without touching production systems. Which exercise is this?

A. Full-scale exercise  
B. Tabletop  
C. Penetration test  
D. Live failover

<details>
<summary>Reveal answer and explanation</summary>

**B.** Tabletops are discussion-based and test coordination and decisions.

</details>

### Q7

A database fails at 10:00, latest backup is 09:40, service returns 12:30. Which is correct?

A. Loss window 2h30m; restoration 20m  
B. Loss window 20m; restoration 2h30m  
C. Both 20m  
D. Both 2h30m

<details>
<summary>Reveal answer and explanation</summary>

**B.** Potential data loss is 20 minutes; actual restoration took 2 hours 30 minutes.

</details>

### Q8

Which action BEST supports safe production return after ransomware recovery?

A. Reconnect immediately  
B. Disable audit logging  
C. Validate clean restore point, security posture, and application function  
D. Delete evidence

<details>
<summary>Reveal answer and explanation</summary>

**C.** Validate the recovered environment and authorize cutover. The other actions increase risk.

</details>

**Score guide:** 7–8 correct: strong understanding. 5–6: revisit metrics and recovery sites. 0–4: redo the case study and tabletop.

## 10. Week 3 cumulative review — Days 11–15

Answer without looking back:

1. Give one example each of data at rest, in transit, and in use.
2. How do data classification and labeling differ?
3. Why doesn't ordinary file deletion guarantee sanitization?
4. Why is RAID not a backup?
5. What do RPO and RTO measure?
6. How do BC, DR, and IR complement each other?
7. What happens if the only backup decryption key is lost?
8. Which CIA objective is threatened most directly by a long outage?

<details>
<summary><strong>Reveal Week 3 answers</strong></summary>

1. Rest: saved PDF; transit: HTTPS upload; use: file open in an editor.
2. Classification assigns sensitivity; labeling communicates or enforces handling rules.
3. Copies may persist in snapshots, backups, or recoverable media; use suitable sanitization.
4. RAID may replicate corruption and deletion; backups provide earlier recovery points.
5. RPO is acceptable data loss; RTO is acceptable downtime.
6. IR handles the event, DR restores systems, and BC maintains essential operations.
7. Backups may become unreadable, threatening availability.
8. Availability.

</details>

## 11. Discussion challenge

> A nonprofit has a **four-hour RTO** and **one-hour RPO** for its donor database. Backups run nightly, restores have never been tested, and management proposes a cold recovery site.

Which objective might the backups miss? What would you need to know about the cold site? What improvements should happen before the next outage? Design a tabletop test without disrupting the nonprofit.

## 12. Study from the Treadmill 🏃

Watch these **specific** Professor Messer SY0-701 lessons:

1. [Business Continuity](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/business-continuity-sy0-701/) — continuity planning and recovery priorities.
2. [Backups](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/backups-sy0-701/) — backup and recovery concepts.

**Active-listening prompts:** Why does BIA precede recovery design? What distinguishes RTO from RPO? How can a backup job succeed while a restore fails?

## 13. One-minute recap

- **BC:** Maintain essential business functions.
- **DR:** Restore technology and data.
- **IR:** Respond to security incidents.
- **BIA:** Identify critical functions and recovery priorities.
- **RPO:** Acceptable data loss in time.
- **RTO:** Acceptable downtime.
- **Hot/warm/cold:** Different levels of recovery-site readiness.
- **Failover/failback:** Switch to alternate systems and return safely.
- **Test recovery:** Written plans and successful backups alone are insufficient.

**Week 3 complete! Next: Day 16 — Networking Foundations: Packets, Ports & Protocols.**

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
