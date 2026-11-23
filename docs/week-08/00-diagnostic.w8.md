# Week 08 — Diagnostic Assessment

> **CompTIA Security+ SY0-701** · **Week 08: Security Architecture**  
> **Questions:** 20 · **Suggested time:** 20–30 minutes

## Instructions

Complete this diagnostic before studying Days 36–40. Select one best answer per question without consulting the lessons. These are original practice questions, not actual CompTIA exam items. Record your answers before opening the answer key. Use the topic scorecard to identify study priorities; this is not a certification passing-score prediction.

## Diagnostic questions

### Day 36 — Secure Design And Defense In Depth

#### Q1. A stolen employee password bypasses a login control. Which design best limits subsequent access to sensitive records?

A. Replace the login page  
B. Add independent resource authorization and network segmentation  
C. Duplicate the same password check  
D. Remove internal access restrictions

#### Q2. A payment system requires one employee to initiate a refund and another to approve it. Which principle is applied?

A. Default deny  
B. Availability  
C. Separation of duties  
D. Passive monitoring

#### Q3. A new firewall policy blocks all traffic except explicitly approved flows. Which principle does this demonstrate?

A. Default deny  
B. Fail-open  
C. Implicit trust  
D. Load balancing

#### Q4. An organization needs a network control that can directly stop prohibited traffic. Which placement is appropriate?

A. Offline log archive  
B. Passive sensor on a mirrored port  
C. Periodic report generator  
D. Inline enforcement control

### Day 37 — Zero Trust Architecture

#### Q5. An employee connects from the corporate LAN. What should a Zero Trust architecture do before granting access to a sensitive resource?

A. Grant access based on location  
B. Evaluate identity, device and applicable resource policy  
C. Trust all domain members indefinitely  
D. Disable contextual checks

#### Q6. Which component makes the policy decision to grant or deny a resource request?

A. Policy enforcement point  
B. Data plane  
C. Policy engine  
D. Policy administrator

#### Q7. A previously approved laptop becomes noncompliant during a sensitive session. What is the most appropriate Zero Trust response?

A. Reevaluate access and revoke or restrict it as policy requires  
B. Wait until the next annual audit  
C. Trust the initial login permanently  
D. Grant broader permissions

#### Q8. Which activity belongs to the data plane rather than the control plane?

A. Evaluating contextual risk  
B. Selecting an access policy  
C. Coordinating session establishment  
D. Transferring authorized application data

### Day 38 — Cloud Models And Shared Responsibility

#### Q9. A company rents cloud virtual machines and installs its own operating systems. Who normally patches the guest operating systems?

A. The internet service provider  
B. The customer  
C. The physical hardware manufacturer  
D. The cloud provider in every case

#### Q10. A developer deploys custom code to a managed application platform. Which task normally remains with the customer?

A. Replacing physical disks  
B. Maintaining the provider hypervisor  
C. Fixing vulnerabilities in deployed application code  
D. Securing the provider building

#### Q11. A company uses a hosted SaaS document editor. Which task remains a customer responsibility?

A. Reviewing employee sharing permissions  
B. Patching the provider host kernel  
C. Replacing provider network switches  
D. Maintaining provider cooling equipment

#### Q12. A provider advertises a compliance certification. What should the customer do before assuming its own workload meets regulatory requirements?

A. Stop collecting evidence  
B. Transfer all accountability to the provider  
C. Disable customer security controls  
D. Map control ownership and verify applicable evidence

### Day 39 — Virtualization Containers And Serverless

#### Q13. Which component is typically shared by containers running in the same Linux execution environment?

A. Each container guest operating system  
B. The kernel  
C. Each application configuration file  
D. Each application identity

#### Q14. Which Linux mechanism limits the CPU or memory resources available to a container?

A. Image signing  
B. DNS  
C. Control groups (cgroups)  
D. Namespaces alone

#### Q15. A team verifies that a container image has a trusted signature. What additional action addresses known vulnerable packages?

A. Scan the image for vulnerabilities  
B. Disable image verification  
C. Grant privileged runtime access  
D. Assume the signature proves the image is safe

#### Q16. A serverless function processes uploaded documents. Which security responsibility normally remains with the application owner?

A. Replacing physical servers  
B. Patching the provider hypervisor  
C. Maintaining provider cooling  
D. Restricting the function identity and validating inputs

### Day 40 — Resilience And Infrastructure Security

#### Q17. Two redundant servers depend on the same power supply. What architectural weakness remains?

A. Excessive geographic diversity  
B. A shared single point of failure  
C. An excessively short RPO  
D. A cold recovery site

#### Q18. A service must be restored within two hours and can tolerate losing at most 15 minutes of recent data. Which targets apply?

A. RTO 15 minutes; RPO two hours  
B. MTBF two hours; RTO 15 minutes  
C. RTO two hours; RPO 15 minutes  
D. RPO two hours; MTTR 15 minutes

#### Q19. A brief utility power interruption occurs before a generator can start. Which control bridges that gap?

A. Uninterruptible power supply (UPS)  
B. Cold site  
C. Security camera  
D. Data replication

#### Q20. Ransomware encrypts production data and the changes replicate to a secondary system. Which control best supports restoration of clean historical data?

A. More synchronous replicas  
B. A larger load balancer  
C. Additional active nodes  
D. Independent protected backups with tested restoration

## Answer key

<details markdown="1">
<summary>Reveal answers and explanations</summary>

**Q1. B — Add independent resource authorization and network segmentation**  
Independent complementary controls limit the impact of a failed authentication layer; duplicating the same check does not necessarily provide independence.  
Review: [Day 36, Section 3: Defense in depth: Complementary layers of protection](day-36-secure-design-and-defense-in-depth.md#3-defense-in-depth-complementary-layers-of-protection).

**Q2. C — Separation of duties**  
Separation of duties divides sensitive responsibilities so one individual cannot complete the entire transaction alone.  
Review: [Day 36, Section 4: Secure design principles](day-36-secure-design-and-defense-in-depth.md#4-secure-design-principles).

**Q3. A — Default deny**  
Default deny requires explicit permission rather than allowing unspecified traffic.  
Review: [Day 36, Section 4: Secure design principles](day-36-secure-design-and-defense-in-depth.md#4-secure-design-principles).

**Q4. D — Inline enforcement control**  
An inline control sits in the traffic path and can block traffic; passive observation alone cannot directly do so.  
Review: [Day 36, Section 6: Control placement and failure behavior](day-36-secure-design-and-defense-in-depth.md#6-control-placement-and-failure-behavior).

**Q5. B — Evaluate identity, device and applicable resource policy**  
Network location alone does not establish trust; access depends on relevant evidence and policy.  
Review: [Day 37, Section 3: The principles behind Zero Trust](day-37-zero-trust-architecture.md#3-the-principles-behind-zero-trust).

**Q6. C — Policy engine**  
The policy engine decides; the administrator coordinates and the enforcement point implements the decision.  
Review: [Day 37, Section 7: Policy engine, administrator and enforcement point](day-37-zero-trust-architecture.md#7-policy-engine-administrator-and-enforcement-point).

**Q7. A — Reevaluate access and revoke or restrict it as policy requires**  
Continuous verification reevaluates relevant changes during a session rather than treating initial authentication as permanent authorization.  
Review: [Day 37, Section 8: Session lifecycle, implementation and trade-offs](day-37-zero-trust-architecture.md#8-session-lifecycle-implementation-and-trade-offs).

**Q8. D — Transferring authorized application data**  
The data plane carries actual communication; the control plane evaluates and coordinates policy.  
Review: [Day 37, Section 4: Control plane and data plane](day-37-zero-trust-architecture.md#4-control-plane-and-data-plane).

**Q9. B — The customer**  
In typical IaaS the customer manages the guest OS and workload while the provider manages underlying infrastructure.  
Review: [Day 38, Section 4: Infrastructure as a Service (IaaS)](day-38-cloud-models-and-shared-responsibility.md#4-infrastructure-as-a-service-iaas).

**Q10. C — Fixing vulnerabilities in deployed application code**  
PaaS abstracts platform management but the customer remains responsible for its code and relevant configuration.  
Review: [Day 38, Section 5: Platform as a Service (PaaS)](day-38-cloud-models-and-shared-responsibility.md#5-platform-as-a-service-paas).

**Q11. A — Reviewing employee sharing permissions**  
SaaS customers still govern their identities, access permissions and data use.  
Review: [Day 38, Section 6: Software as a Service (SaaS)](day-38-cloud-models-and-shared-responsibility.md#6-software-as-a-service-saas).

**Q12. D — Map control ownership and verify applicable evidence**  
Provider certification does not establish customer compliance; responsibilities and evidence must be evaluated for the workload.  
Review: [Day 38, Section 7: The shared responsibility matrix](day-38-cloud-models-and-shared-responsibility.md#7-the-shared-responsibility-matrix).

**Q13. B — The kernel**  
Containers isolate processes but share the execution environment kernel, unlike typical VMs with separate guest operating systems.  
Review: [Day 39, Section 4: Containers: Images, runtimes and shared kernels](day-39-virtualization-containers-and-serverless.md#4-containers-images-runtimes-and-shared-kernels).

**Q14. C — Control groups (cgroups)**  
Cgroups constrain resource usage; namespaces primarily isolate visibility of resources.  
Review: [Day 39, Section 4: Containers: Images, runtimes and shared kernels](day-39-virtualization-containers-and-serverless.md#4-containers-images-runtimes-and-shared-kernels).

**Q15. A — Scan the image for vulnerabilities**  
Signing and scanning address different risks: provenance or integrity versus known vulnerabilities.  
Review: [Day 39, Section 5: Securing the container supply chain and runtime](day-39-virtualization-containers-and-serverless.md#5-securing-the-container-supply-chain-and-runtime).

**Q16. D — Restricting the function identity and validating inputs**  
Serverless abstracts infrastructure management, not application code, input handling or workload permissions.  
Review: [Day 39, Section 7: Serverless computing and event-driven execution](day-39-virtualization-containers-and-serverless.md#7-serverless-computing-and-event-driven-execution).

**Q17. B — A shared single point of failure**  
A shared power dependency can disable both servers, defeating redundancy.  
Review: [Day 40, Section 4: Single points of failure and failure domains](day-40-resilience-and-infrastructure-security.md#4-single-points-of-failure-and-failure-domains).

**Q18. C — RTO two hours; RPO 15 minutes**  
RTO is the targeted restoration duration; RPO expresses tolerable recent data loss in time units.  
Review: [Day 40, Section 7: Recovery objectives and continuity of operations](day-40-resilience-and-infrastructure-security.md#7-recovery-objectives-and-continuity-of-operations).

**Q19. A — Uninterruptible power supply (UPS)**  
A UPS provides short-term power while longer-duration power sources become available.  
Review: [Day 40, Section 9: Physical, environmental and power security](day-40-resilience-and-infrastructure-security.md#9-physical-environmental-and-power-security).

**Q20. D — Independent protected backups with tested restoration**  
Replication may propagate corruption; independent historical backups and restore testing provide a separate recovery option.  
Review: [Day 40, Section 8: Backups and trustworthy recovery](day-40-resilience-and-infrastructure-security.md#8-backups-and-trustworthy-recovery).

</details>

## Topic-level scorecard

| Topic | Questions | Correct | Study priority |
| --- | --- | --- | --- |
| [Day 36](day-36-secure-design-and-defense-in-depth.md) | 1–4 | ___ / 4 | ___ |
| [Day 37](day-37-zero-trust-architecture.md) | 5–8 | ___ / 4 | ___ |
| [Day 38](day-38-cloud-models-and-shared-responsibility.md) | 9–12 | ___ / 4 | ___ |
| [Day 39](day-39-virtualization-containers-and-serverless.md) | 13–16 | ___ / 4 | ___ |
| [Day 40](day-40-resilience-and-infrastructure-security.md) | 17–20 | ___ / 4 | ___ |
| **Total** | **1–20** | **___ / 20** | |

### Interpret your topic scores

- **4/4:** Strong starting knowledge; skim the lesson and complete the practical lab.
- **3/4:** Review the missed concept and complete the practice questions.
- **2/4:** Study the full lesson and complete guided notes.
- **0–1/4:** Prioritize the lesson, guided notes and lab before retesting.

### Interpret your overall score

| Score | Suggested approach |
| --- | --- |
| 17–20 | Target gaps; validate understanding through labs. |
| 13–16 | Review weaker topics and practice application. |
| 9–12 | Study all five lessons, prioritizing low-scoring topics. |
| 0–8 | Work through all lessons in sequence and complete the labs. |

Record your score for comparison with the Week 08 post-check.

**Next:** [Day 36 — Secure Design and Defense in Depth](day-36-secure-design-and-defense-in-depth.md).

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
