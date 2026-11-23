# Day 38 — Cloud Models and Shared Responsibility

> **CompTIA Security+ SY0-701** · **Week 08, Lesson 3 of 5** · Domain 3, Objective **3.1: Compare and contrast security implications of different architecture models** (related: 3.3, 3.4, 5.3)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 37 — Zero Trust Architecture](day-37-zero-trust-architecture.md)  
> **Prerequisite:** [Day 36 — Secure Design and Defense in Depth](day-36-secure-design-and-defense-in-depth.md)  
> **Next:** Day 39 — title pending curriculum confirmation

## 1. Learning objectives

By the end of this lesson, you will be able to:

- Distinguish infrastructure, platform and software as a service using their management boundaries.
- Compare public, private, community and hybrid cloud deployments and distinguish hybrid from multicloud.
- Interpret a shared responsibility matrix and assign control owners for a given workload.
- Explain how identity, encryption, patching, logging and recovery responsibilities change across service models.
- Evaluate cloud architecture trade-offs involving visibility, isolation, cost, resilience and vendor dependence.
- Identify responsibility gaps and recommend controls supported by evidence and contractual commitments.

## 2. The big idea: Renting technology does not mean renting away responsibility

Imagine three ways of opening a restaurant. You rent an empty commercial kitchen and install your own appliances; rent a fully equipped kitchen and supply your recipes and ingredients; or order meals from a catering company. Each arrangement reduces some of your work, but none removes your responsibility for choosing appropriate food, protecting customer information or checking that the service meets your needs.

Cloud computing works similarly. Instead of buying every physical server, an organization consumes computing capabilities operated through a service. The cloud service provider (CSP) operates some layers of the technology stack while the customer controls others. Security failures frequently occur at the boundary rather than because either party lacks security tools.

| Question | Traditional on-premises | Cloud service |
| --- | --- | --- |
| Who owns or operates the hardware? | Normally the organization or its hosting contractor | Normally the provider |
| Who patches the guest operating system? | Organization | Depends on service model |
| Who decides which employees can read sensitive records? | Organization | Organization, using available provider controls |
| Who proves the workload meets business requirements? | Organization | Organization, with provider evidence where relevant |

🧠 **Memory hook:** The provider secures the layers it operates; the customer must secure what it controls and verify what it delegates.

The confidentiality, integrity and availability (CIA) triad still applies. A provider may encrypt storage, yet a customer may expose files through a public sharing link. A provider may maintain redundant hardware, yet a customer may accidentally delete a database without a recoverable backup. Security design must account for both parties.

## 3. What makes a cloud service different?

Cloud computing typically provides on-demand self-service, broad network access, resource pooling, rapid elasticity and measured service. These characteristics allow a team to request resources through a management console or application programming interface (API), scale them when demand increases and pay according to a service's pricing model.

Virtualization often supports resource pooling. A hypervisor separates virtual machines (VMs) so multiple workloads can use the same physical host without normally accessing one another's memory or storage. Isolation is a security boundary, not proof that escape vulnerabilities cannot exist. Providers maintain underlying isolation and customers must enforce isolation between their own workloads and identities.

Cloud management introduces a powerful control plane: APIs and consoles that create resources, change access rules and delete infrastructure. The data plane handles the workload's actual traffic and data operations. A compromised management credential may therefore affect many systems without exploiting an individual server.

For example, an administrator might deploy twenty VMs from one template in minutes. If that template contains an unsafe network rule, the same speed multiplies the error. Automation improves repeatability but also amplifies poorly reviewed configurations.

**Exam trap:** A virtualized server is not necessarily a cloud service. Virtualization is a technology; cloud computing describes a way of delivering computing capabilities.

## 4. Infrastructure as a Service (IaaS)

Infrastructure as a Service provides virtual computing, storage and networking resources. The customer builds and operates a workload above the provider-managed infrastructure. Examples include Amazon EC2 virtual machines, Azure Virtual Machines and Google Compute Engine.

Consider a company deploying an internet-facing web application on a Linux VM. The provider maintains physical servers and the underlying virtualization layer. The company selects its operating system, installs the web server, deploys application code and controls the virtual network. It must patch the guest operating system, remove unnecessary services, protect credentials and configure access restrictions.

A virtual firewall or security group can restrict connections to a VM. A rule allowing HTTPS from the internet may be appropriate for a public endpoint, whereas a rule allowing administrative access from every internet address generally introduces unnecessary exposure. The provider implements the filtering mechanism; the customer chooses the rules.

| Advantage | Security implication |
| --- | --- |
| Extensive operating system control | Supports specialized hardening and agents; requires skilled maintenance |
| Flexible network design | Enables segmentation; incorrect routes or rules can expose resources |
| Custom software support | Enables legacy workloads; unsupported dependencies may persist |
| Rapid provisioning | Speeds deployment; insecure images can replicate quickly |

**Important limitation:** A provider's vulnerability management of physical hosts does not automatically patch the customer-managed guest OS. Even a marketplace image that was secure when published requires maintenance after deployment.

**Exam tip:** If the scenario explicitly says the organization administers a cloud VM and its operating system, guest OS patching is normally a customer responsibility.

## 5. Platform as a Service (PaaS)

Platform as a Service supplies a managed environment in which customers deploy applications without ordinarily administering the underlying operating system. The platform may provide web runtimes, managed databases, deployment tools and scaling mechanisms. Azure App Service and Google App Engine illustrate the model, although available features and boundaries differ.

Imagine a team building a scheduling application. Rather than installing and maintaining a web server on a VM, it deploys code to a managed application platform. The service runs the code, manages infrastructure and maintains supported platform components. The developers remain responsible for the application they write, the libraries they package, authentication and authorization logic, secrets, and configuration of exposed endpoints.

A SQL injection vulnerability in customer-written code remains a customer concern even if the database engine is patched by the provider. The customer must use parameterized queries and appropriate database permissions. A managed database does not understand which business records each employee is entitled to access.

| Managed component | Customer concern |
| --- | --- |
| Platform runtime | Choose supported versions and migrate when required |
| Deployment mechanism | Protect deployment credentials and review code |
| Managed database engine | Restrict database identities, access, and data exposure |
| Automatic scaling | Set sensible quotas and monitor unexpected usage |

PaaS reduces some operational burdens but may constrain custom agents, low-level networking or forensic access. A team must confirm that the platform exposes sufficient application logs, supports private connectivity and meets recovery requirements before adopting it.

**Exam trap:** PaaS does not guarantee secure application code. The provider maintains the platform; the customer maintains its workload and security configuration.

## 6. Software as a Service (SaaS)

Software as a Service delivers a ready-to-use application, typically through a browser, client or API. Microsoft 365, Google Workspace and Salesforce are familiar examples. The provider operates the application and its underlying infrastructure. Customers manage how their organization uses the service.

Suppose a law firm adopts cloud document collaboration. The firm no longer patches the SaaS application's servers, but it still controls user accounts, sharing permissions, administrator roles, retention settings and the documents employees upload. If a departing employee retains access because nobody disables the account, provider patching cannot resolve that organizational failure.

SaaS security commonly involves:

- Identity lifecycle management: create, modify and disable accounts when business roles change.
- Multifactor authentication (MFA): require additional verification, especially for privileged users.
- Tenant configuration: restrict external sharing, unnecessary integrations and overly broad administrative privileges.
- Data governance: classify content, establish retention and deletion rules, and evaluate geographic storage requirements.
- Monitoring: activate available audit features, investigate alerts and understand licensing-dependent log retention.

SaaS providers may offer encryption and recovery features, but a customer must determine whether those features meet its needs. A recycle bin with a finite retention period is not necessarily an independent backup. A provider may restore infrastructure after a service disruption but be unable to restore a particular tenant's records to the desired point in time.

**Important limitation:** The customer ordinarily cannot repair a vulnerability in the provider's application code. Its response may include reporting the issue, disabling affected features, restricting access, monitoring abuse and coordinating with the provider.

## 7. The shared responsibility matrix

A responsibility matrix maps technical layers and security duties to the parties responsible for them. It is a design and governance tool, not a universal contractual statement. Two products sold by the same provider can assign responsibilities differently.

The following matrix is a simplified baseline for conventional services. C = customer; P = provider; S = shared or dependent on service configuration.

| Layer or duty | On-premises | IaaS | PaaS | SaaS |
| --- | --- | --- | --- | --- |
| Physical facilities and hardware | C | P | P | P |
| Physical networking | C | P | P | P |
| Underlying virtualization | C | P | P | P |
| Guest OS maintenance | C | C | P | P |
| Managed runtime | C | C | P | P |
| Customer-developed application code | C | C | C | Not normally applicable |
| Provider application maintenance | Not applicable | Varies | Varies | P |
| Logical network and service access | C | C/S | S | S |
| Customer accounts and permissions | C | C | C | C |
| Customer endpoint protection | C | C | C | C |
| Data classification and authorized use | C | C | C | C |
| Encryption configuration and key management | C | S | S | S |
| Logging, investigation and recovery | C | S | S | S |

The word **shared** does not mean both parties perform the same task. It means an outcome depends on complementary tasks. For example, a provider may generate audit events and maintain the logging service, while the customer enables relevant events, exports them, sets retention and investigates suspicious activity.

### From a generic matrix to an operational assignment

A useful operational matrix identifies a specific task, the accountable owner, the implementation owner and acceptable evidence. Consider patching an IaaS guest OS: the infrastructure team performs patching, the workload owner is accountable for meeting the patch policy, and an authenticated scan or patch compliance report provides evidence.

| Control outcome | Provider action | Customer action | Evidence |
| --- | --- | --- | --- |
| Secure identities | Operate authentication service | Require MFA; remove stale accounts | Sign-in logs and access review |
| Protected storage | Provide encryption capability | Select settings and restrict key access | Configuration review |
| Recovery | Provide documented durability and restore features | Set recovery objectives and test restore | Restore test record |
| Incident investigation | Preserve and share provider-side evidence as agreed | Collect tenant logs and coordinate response | Incident records and export tests |

🧠 **Memory hook:** Own the outcome; assign the tasks; verify the evidence.

**Exam trap:** The phrase “security of the cloud versus security in the cloud” is a helpful model, but does not replace product documentation. Managed services can move some duties previously performed by the customer to the provider.

## 8. Cloud deployment models and trust boundaries

Service models answer *what is managed for me?* Deployment models answer *how and for whom is the infrastructure deployed?* These are separate dimensions. An organization can consume IaaS in a public or private cloud.

| Deployment model | Description | Main trade-off |
| --- | --- | --- |
| Public | Cloud infrastructure offered for use by the general public | Elasticity and shared infrastructure versus dependency on provider controls |
| Private | Cloud infrastructure reserved for one organization | Greater customization versus operational and financial burden |
| Community | Infrastructure reserved for organizations with shared concerns | Common governance requirements versus coordination complexity |
| Hybrid | Distinct cloud environments integrated to support data or application portability | Flexibility versus integration and monitoring complexity |
| Multicloud | Use of services from multiple cloud providers | Provider choice versus inconsistent security and operational complexity |

Public cloud does not mean all customer data is publicly accessible. A private cloud may be operated by a third party and may be hosted outside the organization's premises. Multicloud and hybrid can overlap, but using two unrelated public SaaS products does not itself establish an integrated hybrid cloud.

For example, a hospital may retain sensitive records in a private cloud while using a public cloud for a patient-facing scheduling service. Integration requires authenticated connections, controlled data flows, consistent identities and logging. An overly broad network tunnel can defeat segmentation if it permits public-facing workloads to reach internal record systems unnecessarily.

Hybrid environments introduce identity synchronization, routing, DNS and certificate management challenges. A user disabled in one directory must not retain equivalent access through an unsynchronized identity in another environment. Monitoring teams must correlate events across different formats and time sources.

**Exam tip:** Choose a deployment model according to the stated business and security requirements, not the assumption that private is always safer or public is always cheaper.

## 9. Applying security controls across cloud boundaries

### Identity and least privilege

Identity and access management (IAM) determines which human or workload identity may perform which operation on which resource. A storage-reading identity does not need permission to delete the storage account or create access keys. Separate administrator identities from daily-use accounts and prefer short-lived workload credentials over embedded long-lived secrets when supported.

Zero Trust principles from Day 37 remain relevant: authenticate and authorize each access according to policy rather than trusting a workload simply because it runs inside a provider network. A stolen cloud API token can operate from outside that network unless policy constrains its use.

### Data protection and encryption

Transport Layer Security (TLS) protects supported connections in transit. Storage encryption protects certain stored-data exposure scenarios. Neither replaces authorization: an attacker using an authorized account may receive decrypted content through the normal application interface.

Provider-managed keys reduce operational burden. Customer-managed keys can increase control over key access and revocation but introduce lifecycle and availability risks. Losing or disabling a required key can make protected information inaccessible. Review which parties can use keys, which operations are audited and which recovery options exist.

### Network exposure and tenant isolation

Use private service endpoints where appropriate, restrict network ingress and egress, and segment workloads according to trust boundaries. Public object storage should be an explicit business decision, not a default. A provider can isolate tenants while a customer accidentally exposes its own resources through configuration.

### Logging and incident response

Distinguish management-plane events (such as changing an access policy) from data-plane events (such as reading an object). A logging product may collect one category by default and require separate configuration for the other. Test whether investigators can answer who performed an action, what resource was affected, when it occurred and what permissions applied.

Cloud incident response can require preserving snapshots, exporting logs, revoking credentials and opening a provider support case. The provider may control underlying infrastructure evidence that the customer cannot collect directly. Establish communication paths before an incident.

### Availability and recovery

An availability zone is an isolated infrastructure location within a provider region, subject to the provider's design. Deploying across zones may reduce exposure to a single-zone failure. Regional replication may address different failure scenarios but adds cost, data residency questions and replication complexity.

Replication is not a substitute for backups: deletion or corruption can propagate to replicas. Recovery point objective (RPO) describes tolerable data loss expressed in time; recovery time objective (RTO) describes the target time to restore service. Select recovery controls and test them against both objectives.

## 10. Architectural trade-offs and third-party risk

Cloud adoption changes risks rather than eliminating them. Managed services may improve patch turnaround and resilience while reducing direct visibility into underlying systems. Infrastructure services can offer more control but require larger operational capacity.

| Consideration | Question to ask before deployment |
| --- | --- |
| Cost and scalability | Can the team detect unexpected usage, and are quotas appropriate? |
| Visibility | Are the required logs available, exportable and retained long enough? |
| Patching | Who patches each layer, and who handles unsupported versions? |
| Recovery | What failures are covered and how will restoration be tested? |
| Data residency | Where are primary data, backups and support accesses located? |
| Vendor dependence | Can data and configurations be exported in usable formats? |
| Incident response | Who preserves evidence and how quickly must parties notify each other? |

A service-level agreement (SLA) establishes specified service commitments, often including availability measures and remedies. Service credits do not restore lost records or compensate all business losses. A provider's compliance report may support vendor assessment but does not prove that the customer's tenant configuration is compliant.

Third-party integrations can create additional trust relationships. An email archiving application granted broad access to a SaaS tenant may expose messages if its credentials are compromised. Review requested permissions, data processing arrangements, subcontractors and revocation procedures. Assign each integration an internal owner.

**Important limitation:** Risk transference through contracts or insurance does not remove the customer's responsibility for managing remaining risk.

## 11. Worked case study: A legal practice moves three workloads to the cloud

A fictional legal practice has twelve staff members and a small IT team. It needs document collaboration, a custom client intake application and a legacy case-management server that requires administrator access to its OS. The team must preserve confidential records and maintain access during disruptions.

The practice chooses a SaaS collaboration platform, a managed PaaS application environment and an IaaS VM for the legacy server. This avoids maintaining operating systems where no business requirement demands it while preserving necessary control over the legacy workload.

| Finding | Reasoning | Recommended control and owner |
| --- | --- | --- |
| Public document sharing enabled in SaaS | Provider cannot determine which clients may receive a given file | Practice restricts external sharing and reviews permissions |
| Intake application accepts unrestricted database queries | Managed database patching cannot repair unsafe business logic | Development team uses parameterized queries and least privilege |
| Legacy VM has an outdated OS package | Guest OS maintenance is not delegated under the chosen IaaS model | IT team applies tested patches and validates results |
| Provider logs are available for only a short period | Investigations may begin after events expire | Security owner configures permitted exports and retention |
| No tested point-in-time recovery | Infrastructure redundancy does not prove recoverability | Practice defines RPO/RTO, confirms features and tests restores |

The team restricts connections from the intake application to only the required database interface. The legacy VM is not directly reachable from the public internet. A separate administrative access path uses MFA and restricted roles. Each workload has an owner responsible for periodically reviewing permissions and configuration.

Before deployment the practice reviews provider documentation and contracts for incident notification, data location, backup coverage and support access. It documents unresolved requirements rather than treating a provider security certification as blanket assurance.

**Takeaway:** The correct service model is the one that balances necessary control with the organization's ability to operate that control securely. Every delegated duty still needs a defined boundary and a way to verify the required outcome.

## 12. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Distinguish IaaS, PaaS and SaaS. | _Write here._ |
| Who normally patches an IaaS guest OS? | _Write here._ |
| Why does SaaS still require customer IAM? | _Write here._ |
| Distinguish service and deployment models. | _Write here._ |
| Distinguish hybrid and multicloud. | _Write here._ |
| Explain why logging may be shared. | _Write here._ |
| Explain why replication is not a backup. | _Write here._ |
| What must be checked before accepting a generic responsibility matrix? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Distinguish IaaS, PaaS and SaaS. | IaaS supplies infrastructure; PaaS also manages the application platform; SaaS delivers an operated application. |
| Who normally patches an IaaS guest OS? | The customer, unless a separate managed service explicitly assumes that task. |
| Why does SaaS still require customer IAM? | The customer knows which people and integrations should have access and what privileges they need. |
| Distinguish service and deployment models. | Service models describe managed layers; deployment models describe infrastructure arrangement and eligible users. |
| Distinguish hybrid and multicloud. | Hybrid integrates distinct cloud environments; multicloud uses multiple providers. They may overlap. |
| Explain why logging may be shared. | The provider generates and exposes events; the customer selects, retains and investigates relevant events. |
| Explain why replication is not a backup. | Replication can propagate deletion or corruption; independent recoverable versions may be needed. |
| What must be checked before accepting a generic responsibility matrix? | The specific service documentation, configuration, contract and evidence of control operation. |

</details>

## 13. Hands-on lab: Assign cloud control owners

**Time:** 20–25 minutes  
**Tools:** Paper or a text editor; no cloud account required.  
**Safety and scope:** All services, records and findings below are fictional. Do not change any real cloud settings or upload sensitive data.

### Step 1 — Read the service descriptions (3 minutes)

A community clinic uses three services:

- A: a VM running a customer-administered Linux appointment server (IaaS).
- B: a managed application platform running clinic-written referral code (PaaS).
- C: a hosted email and document application (SaaS).

Assume the provider operates physical infrastructure for all three. For B and C it maintains the underlying operating system. The clinic manages its own user accounts and business data. All services offer configurable tenant logging. Recovery capabilities vary and are not yet verified.

### Step 2 — Complete the control matrix (7 minutes)

Enter C (clinic), P (provider), S (shared) or N/A. For shared duties, describe each party's task. Do not assume a service includes features not stated above.

| Duty | A: IaaS | B: PaaS | C: SaaS |
| --- | --- | --- | --- |
| Physical host maintenance | | | |
| Guest or platform OS patching | | | |
| Repair clinic-written application code | | | |
| Maintain hosted SaaS application code | | | |
| Remove departed employee accounts | | | |
| Configure and retain tenant audit logs | | | |
| Meet clinic recovery objectives | | | |
| Classify patient data | | | |

### Step 3 — Triage the fictional findings (6 minutes)

| ID | Observation |
| --- | --- |
| F1 | A has an unpatched guest OS package. |
| F2 | B has an application identity with permission to delete unrelated records. |
| F3 | C allows anonymous sharing of a patient-document folder. |
| F4 | C's former employee account remains active. |
| F5 | The clinic cannot locate data-access audit events for B. |
| F6 | A's snapshots have never been restored in a test. |

For each finding, write the primary action owner, one corrective action and one way to verify that action. Prioritize active exposure or access risks, considering patient safety and continuity of care.

### Step 4 — Interpret the architecture (5 minutes)

1. Which findings would remain possible even if the provider maintained every physical server perfectly?
2. If B's managed runtime itself contains a vulnerability, how does ownership differ from F2?
3. Does F5 prove that nobody accessed the data? What should be investigated?
4. The clinic requires an RPO of four hours and RTO of eight hours. Does the existence of snapshots establish either target? Explain.
5. The clinic considers a managed service to replace A. What information should it obtain before delegating patching?

<details markdown="1">
<summary>Reveal lab answers</summary>

| Duty | A | B | C |
| --- | --- | --- | --- |
| Physical host maintenance | P | P | P |
| Guest or platform OS patching | C | P | P |
| Repair clinic-written application code | C | C | N/A |
| Maintain hosted SaaS application code | N/A | N/A | P |
| Remove departed employee accounts | C | C | C |
| Configure and retain tenant audit logs | S | S | S |
| Meet clinic recovery objectives | S | S | S |
| Classify patient data | C | C | C |

For logs, the provider supplies event capabilities and the clinic configures, retains and reviews relevant events. For recovery, the provider supplies contractual service capabilities, while the clinic defines targets, selects any needed additional backup controls and tests restoration. The precise division remains subject to documentation.

| Finding | Owner and corrective action | Verification |
| --- | --- | --- |
| F1 | Clinic: assess exposure and patch the guest OS using change control. | Confirm installed version and rescan. |
| F2 | Clinic: remove unnecessary delete privileges from the application identity. | Review effective permissions and test required operations. |
| F3 | Clinic: restrict anonymous sharing and investigate possible exposure. | Review sharing policy, links and available access logs. |
| F4 | Clinic: disable account and revoke active sessions where supported. | Confirm account status and session revocation. |
| F5 | Shared: clinic checks logging configuration and permissions; provider clarifies event availability. | Generate a synthetic authorized event and verify capture and export. |
| F6 | Clinic leads restore validation with provider recovery features. | Restore synthetic data in isolation and record recovery time and data age. |

1. All six findings can occur despite perfectly maintained physical infrastructure.
2. The provider normally patches the managed runtime; the clinic restricts its workload identity in F2. Unsupported runtime versions may also require customer migration.
3. No. Events might be disabled, filtered, expired or inaccessible. Check event types, permissions, retention and export configuration; contact the provider if necessary.
4. No. Snapshot frequency, successful completion, data consistency and actual restoration time determine whether the targets can be met. Test with representative non-sensitive data.
5. Obtain the service-specific matrix, supported versions, patch commitments, maintenance windows, exceptions, evidence and escalation procedures. Confirm which application and configuration duties remain with the clinic.

Prioritize F3 and F4 for immediate containment when confirmed, investigate F2's effective privileges, and assess F1 based on exploitability and exposure. Missing logs and untested recovery also require prompt attention. Exact sequencing depends on clinical continuity and evidence.

</details>

## 14. Eight original Security+-style questions

These are original practice questions, not actual CompTIA examination items.

### Q1. A company administers Linux VMs rented from a cloud provider. Who normally patches their guest operating systems?

A. The cloud provider's physical security team  
B. The customer  
C. The internet service provider  
D. The hypervisor manufacturer

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** In conventional IaaS the customer administers the guest OS. A and D relate to underlying infrastructure or technology, not the guest's maintenance. C supplies connectivity rather than workload administration.

</details>

### Q2. A developer wants to deploy custom application code without administering the underlying OS. Which model BEST meets this requirement?

A. On-premises infrastructure  
B. Conventional IaaS  
C. A ready-to-use SaaS email service  
D. PaaS

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** PaaS supplies a managed application environment. A and B ordinarily require OS administration. C delivers a finished application rather than the requested custom code hosting platform.

</details>

### Q3. A SaaS tenant's departed administrator remains active. Which action addresses the primary failure?

A. Disable the account and revoke access through the tenant's identity process.  
B. Ask the provider to replace physical disks.  
C. Move the provider to another availability zone.  
D. Patch the provider's hypervisor.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** The customer must manage its account lifecycle. B, C and D concern infrastructure and do not remove the former employee's authorization.

</details>

### Q4. An organization integrates its private cloud with a public cloud to move workloads between environments. Which deployment model is described?

A. Community  
B. Public only  
C. Hybrid  
D. SaaS

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Integrated distinct cloud environments describe hybrid cloud. A concerns a defined community, B omits the private environment, and D is a service model.

</details>

### Q5. A provider generates management audit events, but the customer has not configured their required retention. What BEST describes this situation?

A. Logging is entirely the provider's duty.  
B. Logging has complementary provider and customer responsibilities.  
C. Audit logs are unnecessary in managed services.  
D. Physical encryption resolves the retention gap.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** The provider supplies event generation and the customer configures retention according to requirements. A incorrectly delegates all duties; C removes investigative visibility; D addresses a different control outcome.

</details>

### Q6. A PaaS-hosted application contains SQL injection in customer-written code. Which action addresses the root cause?

A. Replace the provider's physical switches.  
B. Move the application to a different region.  
C. Increase the application's storage quota.  
D. Correct the application queries using parameterization.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Parameterized queries separate query structure from untrusted values. The customer owns its application code. A, B and C do not correct the vulnerable query handling.

</details>

### Q7. A cloud database replicates changes to a secondary location. An administrator accidentally deletes records and the deletion replicates. Which additional control BEST addresses this risk?

A. Independently recoverable backups with tested restoration  
B. Additional identical live replicas only  
C. A larger compute instance  
D. A new public IP address

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** Recoverable historical versions can restore records from before deletion. B can replicate the same error; C and D do not recover lost data.

</details>

### Q8. Two managed services have different documented patching boundaries. What should a security architect do FIRST when assigning operational control ownership?

A. Apply the same matrix to all services without modification.  
B. Assume the provider owns all security controls.  
C. Review service-specific documentation and contractual responsibilities.  
D. Assign all provider infrastructure tasks to customer developers.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Service-specific boundaries determine who can perform each task and what evidence is available. A and B risk gaps; D assigns work to parties that may lack access and authority.

</details>

**Score guide:** 7–8 correct: strong understanding; 5–6: revisit Sections 4–9, focusing on the missed distinctions; 0–4: repeat the guided notes in Section 12 and the lab in Section 13 before retrying.

## 15. Discussion challenge

A nonprofit with limited IT staff wants to migrate a donor database to the cloud. It can buy a SaaS donor-management application or develop a custom application on PaaS. The organization needs detailed access logs, reliable exports, restricted volunteer permissions and a tested recovery process.

Recommend a service model and justify it. Identify three customer-owned controls, two provider-owned controls and two shared outcomes. Explain what evidence you would request before signing a contract and what circumstances would make you reverse your recommendation.

**Bonus:** Explain how your design protects each CIA triad objective while accounting for limited staff and budget.

## 16. Study from the Treadmill 🏃

**Direct links to specific videos:**

- [Professor Messer — Cloud Infrastructures (SY0-701, 3.1)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/cloud-infrastructures-sy0-701/) — Focus on responsibility boundaries, hybrid deployments and third-party dependencies.
- [Inside Cloud and Security — CompTIA Security+ Exam Cram: 3.1 Architecture Models](https://www.youtube.com/watch?v=FNWeXNVlmw8) — Focus on the cloud responsibility matrix and deployment models beginning at 02:30; later material extends beyond today's scope.

**Active-listening prompts:**

- Which security duties follow customer data even as more technology layers become managed?
- Why might identical security policies require different implementation steps across providers?
- What evidence would convince you that a shared control operates effectively?

Exam alignment reference: [CompTIA SY0-701 Domain 3 objectives, version 5.0 (ECPI-hosted copy)](https://getcertified.ecpi.edu/wp-content/uploads/2024/03/CompTIA-SecurityPlus701-Objectives-3.0-Security-Architecture.pdf).

## 17. One-minute recap

- **IaaS:** customer normally maintains the guest OS and workload.
- **PaaS:** provider manages the platform; customer secures deployed code and configuration.
- **SaaS:** provider operates the application; customer still governs identities, access and data use.
- **Service models** describe management boundaries; **deployment models** describe infrastructure arrangements.
- **Shared responsibility** requires complementary tasks, documented ownership and evidence.
- **Replication, encryption and provider certifications** do not independently establish recovery, authorization or customer compliance.
- **Golden rule:** Never assume a control has been delegated merely because the workload runs in the cloud.

**Next:** Day 39 — title pending curriculum confirmation.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
