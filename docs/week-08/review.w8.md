# Week 08 — Review and Knowledge Check

> **CompTIA Security+ SY0-701** · **Week 08: Security Architecture**  
> **Questions:** 20 post-check + 5 cumulative · **Suggested time:** 30–40 minutes

## Instructions

Complete the [Week 08 diagnostic](00-diagnostic.w8.md) before studying the lessons. After completing Days 36–40, answer the following questions without notes. Record answers before revealing the key. These are original practice questions, not actual CompTIA items. Compare the first 20 answers with your diagnostic; score cumulative questions separately.

## Post-check questions

### Day 36 — Secure Design and Defense in Depth

#### Q1. A stolen VPN credential permits network entry. Which design best prevents unrestricted access to payroll?

A. A second identical VPN password check  
B. A larger VPN gateway  
C. Independent payroll authorization and segmented access  
D. Removing internal firewalls

#### Q2. A purchasing clerk can create a supplier but cannot approve payment to that supplier. Which principle is illustrated?

A. Separation of duties  
B. Fail-open  
C. Load balancing  
D. Implicit trust

#### Q3. A new application role has no permissions until an administrator explicitly grants them. Which principle applies?

A. Availability  
B. Passive monitoring  
C. Implicit trust  
D. Default deny

#### Q4. A security appliance must reject malicious packets before they reach a server. Which deployment meets this requirement?

A. Mirrored-port monitoring  
B. Inline enforcement  
C. Offline packet analysis  
D. Daily log reporting

### Day 37 — Zero Trust Architecture

#### Q5. A contractor requests a confidential file from an office workstation. What should determine access?

A. Office location alone  
B. A permanent trust decision  
C. The workstation brand  
D. Identity, device evidence and resource policy

#### Q6. Which component evaluates policy and risk signals to decide whether a database session is allowed?

A. Policy administrator  
B. Policy engine  
C. Policy enforcement point  
D. Data plane

#### Q7. An endpoint reports malware during an authorized session. What should happen?

A. Reevaluate the session and restrict or revoke access according to policy  
B. Wait for password expiration  
C. Ignore the alert until logout  
D. Increase session privileges

#### Q8. Which operation is a data-plane activity?

A. Evaluating device posture  
B. Selecting an access policy  
C. Sending an authorized report to a client  
D. Coordinating session setup

### Day 38 — Cloud Models and Shared Responsibility

#### Q9. A business deploys a self-managed Linux VM on IaaS. Who normally installs guest kernel security updates?

A. The building owner  
B. The customer  
C. The provider in all cases  
D. The ISP

#### Q10. A team deploys its own web application on a managed PaaS runtime. Who fixes an injection flaw in its code?

A. The physical server vendor  
B. The provider facilities team  
C. The ISP  
D. The application customer

#### Q11. A SaaS collaboration tenant exposes files through public sharing links. Which action is normally the customer’s responsibility?

A. Replace provider disks  
B. Patch provider hypervisors  
C. Review and restrict tenant sharing settings  
D. Maintain provider cooling

#### Q12. An auditor asks whether a cloud workload satisfies a security requirement. What should the customer do?

A. Map responsibilities and obtain evidence for applicable controls  
B. Assume provider certification covers everything  
C. Disable tenant logging  
D. Assign every control to the provider

### Day 39 — Virtualization, Containers and Serverless

#### Q13. Two ordinary Linux containers run in the same execution environment. What underlying component do they share?

A. Kernel  
B. Separate guest kernels  
C. Application credentials  
D. Private configuration files

#### Q14. A runaway container consumes excessive memory. Which mechanism can enforce a memory limit?

A. Image signatures  
B. DNS records  
C. Control groups (cgroups)  
D. Namespaces alone

#### Q15. A signed container image contains an outdated library. Which additional control detects known vulnerable dependencies?

A. Signature verification alone  
B. Image vulnerability scanning  
C. Privileged execution  
D. Disabling registry authentication

#### Q16. An event-triggered function reads customer records. Which task remains with its developer?

A. Maintain physical hosts  
B. Replace provider switches  
C. Patch provider hypervisors  
D. Limit function permissions and validate event inputs

### Day 40 — Resilience and Infrastructure Security

#### Q17. Two application nodes use one network switch. What risk remains?

A. Excessive geographic diversity  
B. A short recovery point objective  
C. A shared single point of failure  
D. A cold site

#### Q18. A service must resume within four hours and tolerate no more than 30 minutes of lost data. Which targets apply?

A. RTO four hours; RPO 30 minutes  
B. RTO 30 minutes; RPO four hours  
C. MTBF four hours; RTO 30 minutes  
D. RPO four hours; MTTR 30 minutes

#### Q19. A server must remain powered during the seconds before a generator starts. Which control is appropriate?

A. Cold site  
B. Camera  
C. Replication  
D. UPS

#### Q20. A destructive database update is copied to all live replicas. Which control supports recovery to an earlier clean state?

A. More live replicas  
B. Independent protected backups with tested restoration  
C. Additional load balancers  
D. Larger network switches

## Cumulative questions

Apply earlier networking, identity, data protection, vulnerability management and malware concepts to Week 08 architectures.

#### Q21. A clinic separates guest Wi-Fi from clinical systems and restricts permitted traffic between them. Which design best explains this?

A. Shared administrator credentials  
B. Segmentation with controlled trust boundaries  
C. A flat network  
D. Unrestricted routing

#### Q22. A user passes MFA but requests sensitive records from a noncompliant device. What should happen?

A. Allow access because MFA succeeded  
B. Disable device checks  
C. Trust the source IP alone  
D. Apply resource policy using both identity and device evidence

#### Q23. A cloud customer encrypts stored records but grants all staff access to decryption and files. What improvement is most appropriate?

A. Restrict access to data and keys according to least privilege  
B. Remove encryption  
C. Trust all authenticated users  
D. Disable audit logs

#### Q24. A container image includes a vulnerable dependency identified by a scanner. What should the team do before redeployment?

A. Sign the unchanged image only  
B. Ignore the finding because containers are isolated  
C. Update or replace the dependency, rebuild and rescan  
D. Grant more runtime privileges

#### Q25. A ransomware incident compromises production and its live replica. Which recovery design best combines earlier malware and data protection concepts?

A. Automatic replication alone  
B. Protected historical backups and verified clean restoration  
C. A larger UPS  
D. More privileged accounts

## Post-check answer key

<details markdown="1">
<summary>Reveal answers and explanations</summary>

**Q1. C — Independent payroll authorization and segmented access**  
Independent controls constrain access even after the perimeter fails.  
Review: [Day 36, Section 3: Defense in depth: Complementary layers of protection](day-36-secure-design-and-defense-in-depth.md#3-defense-in-depth-complementary-layers-of-protection).

**Q2. A — Separation of duties**  
Dividing approval and creation prevents one person from completing the sensitive workflow alone.  
Review: [Day 36, Section 4: Secure design principles](day-36-secure-design-and-defense-in-depth.md#4-secure-design-principles).

**Q3. D — Default deny**  
Access is denied unless explicitly authorized.  
Review: [Day 36, Section 4: Secure design principles](day-36-secure-design-and-defense-in-depth.md#4-secure-design-principles).

**Q4. B — Inline enforcement**  
Inline placement permits direct blocking; observation alone does not.  
Review: [Day 36, Section 6: Control placement and failure behavior](day-36-secure-design-and-defense-in-depth.md#6-control-placement-and-failure-behavior).

**Q5. D — Identity, device evidence and resource policy**  
Zero Trust evaluates relevant evidence rather than trusting network location.  
Review: [Day 37, Section 3: The principles behind Zero Trust](day-37-zero-trust-architecture.md#3-the-principles-behind-zero-trust).

**Q6. B — Policy engine**  
The engine decides; the administrator coordinates and the enforcement point applies the decision.  
Review: [Day 37, Section 7: Policy engine, administrator and enforcement point](day-37-zero-trust-architecture.md#7-policy-engine-administrator-and-enforcement-point).

**Q7. A — Reevaluate the session and restrict or revoke access according to policy**  
Continuous verification responds to changed risk during a session.  
Review: [Day 37, Section 8: Session lifecycle, implementation and trade-offs](day-37-zero-trust-architecture.md#8-session-lifecycle-implementation-and-trade-offs).

**Q8. C — Sending an authorized report to a client**  
The data plane carries application traffic; policy decisions belong to the control plane.  
Review: [Day 37, Section 4: Control plane and data plane](day-37-zero-trust-architecture.md#4-control-plane-and-data-plane).

**Q9. B — The customer**  
Typical IaaS leaves guest OS management to the customer.  
Review: [Day 38, Section 4: Infrastructure as a Service (IaaS)](day-38-cloud-models-and-shared-responsibility.md#4-infrastructure-as-a-service-iaas).

**Q10. D — The application customer**  
The customer retains responsibility for its application code.  
Review: [Day 38, Section 5: Platform as a Service (PaaS)](day-38-cloud-models-and-shared-responsibility.md#5-platform-as-a-service-paas).

**Q11. C — Review and restrict tenant sharing settings**  
Tenant access and sharing configuration remain customer responsibilities.  
Review: [Day 38, Section 6: Software as a Service (SaaS)](day-38-cloud-models-and-shared-responsibility.md#6-software-as-a-service-saas).

**Q12. A — Map responsibilities and obtain evidence for applicable controls**  
Provider assurance does not automatically establish workload compliance.  
Review: [Day 38, Section 7: The shared responsibility matrix](day-38-cloud-models-and-shared-responsibility.md#7-the-shared-responsibility-matrix).

**Q13. A — Kernel**  
Containers share the execution environment kernel while isolating processes.  
Review: [Day 39, Section 4: Containers: Images, runtimes and shared kernels](day-39-virtualization-containers-and-serverless.md#4-containers-images-runtimes-and-shared-kernels).

**Q14. C — Control groups (cgroups)**  
Cgroups limit resource consumption; namespaces isolate resource visibility.  
Review: [Day 39, Section 4: Containers: Images, runtimes and shared kernels](day-39-virtualization-containers-and-serverless.md#4-containers-images-runtimes-and-shared-kernels).

**Q15. B — Image vulnerability scanning**  
Scanning addresses known vulnerabilities; signatures establish integrity or provenance, not absence of flaws.  
Review: [Day 39, Section 5: Securing the container supply chain and runtime](day-39-virtualization-containers-and-serverless.md#5-securing-the-container-supply-chain-and-runtime).

**Q16. D — Limit function permissions and validate event inputs**  
Serverless abstracts infrastructure, not workload authorization or input security.  
Review: [Day 39, Section 7: Serverless computing and event-driven execution](day-39-virtualization-containers-and-serverless.md#7-serverless-computing-and-event-driven-execution).

**Q17. C — A shared single point of failure**  
Failure of the shared switch can disconnect both nodes.  
Review: [Day 40, Section 4: Single points of failure and failure domains](day-40-resilience-and-infrastructure-security.md#4-single-points-of-failure-and-failure-domains).

**Q18. A — RTO four hours; RPO 30 minutes**  
RTO targets restoration time; RPO targets tolerable data loss expressed in time.  
Review: [Day 40, Section 7: Recovery objectives and continuity of operations](day-40-resilience-and-infrastructure-security.md#7-recovery-objectives-and-continuity-of-operations).

**Q19. D — UPS**  
A UPS bridges short power interruptions.  
Review: [Day 40, Section 9: Physical, environmental and power security](day-40-resilience-and-infrastructure-security.md#9-physical-environmental-and-power-security).

**Q20. B — Independent protected backups with tested restoration**  
Replication can copy destructive changes; historical backups provide a separate recovery path.  
Review: [Day 40, Section 8: Backups and trustworthy recovery](day-40-resilience-and-infrastructure-security.md#8-backups-and-trustworthy-recovery).

</details>

## Cumulative answer key

<details markdown="1">
<summary>Reveal answers and explanations</summary>

**Q21. B — Segmentation with controlled trust boundaries**  
Segmentation applies earlier network security concepts to limit lateral movement.  
Review: [Day 36, Section 5: Trust boundaries, security zones and zero trust](day-36-secure-design-and-defense-in-depth.md#5-trust-boundaries-security-zones-and-zero-trust).

**Q22. D — Apply resource policy using both identity and device evidence**  
Authentication is not sufficient authorization; device posture can affect access.  
Review: [Day 37, Section 5: Adaptive identity and contextual trust](day-37-zero-trust-architecture.md#5-adaptive-identity-and-contextual-trust).

**Q23. A — Restrict access to data and keys according to least privilege**  
Encryption complements rather than replaces access control.  
Review: [Day 38, Section 9: Applying security controls across cloud boundaries](day-38-cloud-models-and-shared-responsibility.md#9-applying-security-controls-across-cloud-boundaries).

**Q24. C — Update or replace the dependency, rebuild and rescan**  
Vulnerability remediation and validation complement isolation and provenance controls.  
Review: [Day 39, Section 5: Securing the container supply chain and runtime](day-39-virtualization-containers-and-serverless.md#5-securing-the-container-supply-chain-and-runtime).

**Q25. B — Protected historical backups and verified clean restoration**  
Independent backups and recovery validation address corruption that live replication may propagate.  
Review: [Day 40, Section 8: Backups and trustworthy recovery](day-40-resilience-and-infrastructure-security.md#8-backups-and-trustworthy-recovery).

</details>

## Topic-level scorecard

| Topic | Questions | Diagnostic | Post-check | Change | Next steps |
| --- | --- | --- | --- | --- | --- |
| [Day 36](day-36-secure-design-and-defense-in-depth.md) | 1–4 | ___ / 4 | ___ / 4 | ___ | ___ |
| [Day 37](day-37-zero-trust-architecture.md) | 5–8 | ___ / 4 | ___ / 4 | ___ | ___ |
| [Day 38](day-38-cloud-models-and-shared-responsibility.md) | 9–12 | ___ / 4 | ___ / 4 | ___ | ___ |
| [Day 39](day-39-virtualization-containers-and-serverless.md) | 13–16 | ___ / 4 | ___ / 4 | ___ | ___ |
| [Day 40](day-40-resilience-and-infrastructure-security.md) | 17–20 | ___ / 4 | ___ / 4 | ___ | ___ |
| **Total** | **1–20** | **___ / 20** | **___ / 20** | **___** | |

Subtract your diagnostic score from your post-check score for each topic. Each additional correct answer represents 25 percentage points within a topic, or 5 percentage points overall. Parallel questions measure similar concepts, not identical difficulty.

### Interpret your topic scores

- **4/4:** Strong understanding; validate through the lab.
- **3/4:** Review the missed concept and retry.
- **2/4:** Revisit the linked sections and guided notes.
- **0–1/4:** Repeat the lesson and lab before retesting.

### Overall post-check

| Score | Next step |
| --- | --- |
| 17–20 | Consolidate remaining gaps. |
| 13–16 | Revisit weaker topics. |
| 9–12 | Repeat guided notes and targeted labs. |
| 0–8 | Work through the five lessons again. |

## Cumulative scorecard

| Question | Integrated concept | Correct? |
| --- | --- | --- |
| 21 | Network segmentation | ___ |
| 22 | MFA and contextual access | ___ |
| 23 | Encryption and least privilege | ___ |
| 24 | Vulnerability remediation | ___ |
| 25 | Ransomware and recovery | ___ |
| **Total** | | **___ / 5** |

**4–5:** Strong integration; **2–3:** revisit missed concepts; **0–1:** review relevant earlier lessons and Week 08 labs. These scores are study indicators, not certification passing-score predictions.

## Reflection and next steps

- Which topic improved most?
- Which incorrect answer reflected a misconception rather than a forgotten term?
- Which lab would best address your remaining weakness?

**Next:** Week 09 — continue with Day 41 when available.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
