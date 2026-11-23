# Day 40 — Resilience and Infrastructure Security

> **CompTIA Security+ SY0-701** · **Week 08, Lesson 5 of 5** · Domain 3, Objective **3.4: Resilience and recovery** (related: Objectives 3.1, 3.2, 1.2 and 5.2)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 39 — Virtualization, Containers and Serverless](day-39-virtualization-containers-and-serverless.md)  
> **Next:** Day 41 — Topic pending curriculum confirmation

**Prerequisites:** Days 14–15 and [Day 36 — Secure Design and Defense in Depth](day-36-secure-design-and-defense-in-depth.md).

**Exam scope:** Objective 3.4 covers availability architecture, recovery sites, diversity, continuity, capacity, testing, backups and power. Physical protection and infrastructure design connect this material to Objectives 1.2, 3.1 and 3.2; recovery metrics connect to Objective 5.2. Consult the [CompTIA exam objectives document (hosted copy)](https://maylortaylor.github.io/secplusstudy/files/CompTIA%20Security%2B%20SY0-701%20Exam%20Objectives%20%287.0%29_Jan2026.pdf).

## 1. Learning objectives

By the end of this lesson, you will be able to:

- Distinguish redundancy, high availability, fault tolerance, resilience and disaster recovery.
- Identify single points of failure and shared failure domains in infrastructure diagrams.
- Compare clustering, load balancing and active-active or active-passive deployments.
- Select recovery sites and diversity strategies according to business requirements.
- Interpret recovery time and recovery point objectives and calculate simple availability estimates.
- Evaluate backup, replication, snapshot and journaling strategies.
- Select physical, environmental and power controls for specific outage scenarios.
- Compare recovery testing methods and design a layered continuity strategy within budget constraints.

## 2. The big idea: Security must survive failure

Imagine an emergency clinic with excellent encryption, strong authentication and carefully configured firewalls. A power failure takes its network offline. Clinicians cannot retrieve patient records, reception cannot verify appointments, and the pharmacy cannot access its prescription system. No attacker has stolen a password, yet the organization has experienced a security failure: authorized people cannot access essential information when they need it.

Availability is part of the confidentiality, integrity and availability (CIA) triad. It is not simply a performance metric. In environments such as healthcare, transportation and emergency response, unavailable information can create physical safety consequences. Resilience also protects integrity: an interrupted database write can leave records inconsistent, and an unsafe recovery process can overwrite valid information with outdated or compromised copies.

Day 36 introduced defense in depth. Today we extend that idea to operational failure. Instead of asking only what prevents unauthorized access, ask what happens when each protective or operational component stops working. Can the organization continue? Can it recover trustworthy data? Can it maintain security during the disruption?

| Security goal | Example | Failure to prevent |
| --- | --- | --- |
| Confidentiality | Encrypt recovery media | Theft of backup records |
| Integrity | Verify restored records | Recovery of corrupted data |
| Availability | Provide independent power and redundant services | Loss of access during an outage |

🧠 **Memory hook:** Prevention reduces the chance of failure; resilience reduces its consequences; recovery restores trustworthy operation.

## 3. Resilience, redundancy and availability

### The vocabulary of failure

**Redundancy** provides additional components or resources so the organization is not wholly dependent on one instance. A spare firewall stored in a cabinet qualifies as redundancy, but a technician may need hours to install it. Redundancy therefore does not automatically provide continuous availability.

**High availability (HA)** is an architectural approach that minimizes service interruptions using standby or concurrently operating resources, monitoring and failover. A correctly configured redundant firewall pair can transfer traffic to a surviving firewall after a failure. Some sessions may still be interrupted; HA is not a promise of zero downtime.

**Fault tolerance** is the ability to continue functioning despite specified faults, ideally without noticeable interruption within the designed failure assumptions. A storage array that continues serving files after one supported disk failure illustrates this idea. Multiple simultaneous failures may exceed those assumptions.

**Resilience** is broader: the ability to withstand disruption, adapt, continue essential functions and recover. It includes technology, people, procedures and physical facilities. **Disaster recovery (DR)** concentrates on restoring technology and data after serious disruption. **Business continuity (BC)** concentrates on keeping essential business functions operating, including through alternatives that do not require the original technology.

| Concept | Main question | Example |
| --- | --- | --- |
| Redundancy | Is there another component? | Spare router |
| HA | Can service resume or continue rapidly? | Router pair with automatic failover |
| Fault tolerance | Can operation continue despite a defined fault? | Mirrored storage surviving a disk failure |
| DR | How do we restore systems and data? | Restore database at alternate site |
| BC | How do we continue essential work? | Approved manual registration procedure |

**Exam trap:** A spare device is not necessarily HA. A backup is not necessarily a continuously available service.

### Measuring availability

Availability over a measurement interval can be expressed as `(total time − downtime) / total time × 100`. The following examples assume a 365-day year and count all downtime.

| Availability target | Approximate annual downtime allowance |
| --- | --- |
| 99% | 87.6 hours |
| 99.9% | 8.76 hours |
| 99.99% | 52.56 minutes |
| 99.999% | 5.26 minutes |

For example, 99.9% allows 0.1% downtime. Multiplying 8,760 hours by 0.001 gives 8.76 hours. Service-level agreements (SLAs) may exclude maintenance, define outage differently or use monthly measurement periods. Always read the actual measurement rules before comparing providers.

Mean time between failures (MTBF) describes average operating time between failures for repairable equipment; mean time to repair (MTTR) describes average repair duration. Under a simplified steady-state repair model, availability can be estimated as `MTBF / (MTBF + MTTR)`. This is not a reliable prediction of every real outage: correlated failures, detection delays and dependencies complicate the model.

## 4. Single points of failure and failure domains

A **single point of failure (SPOF)** is a component or dependency whose failure can interrupt a required service. The relevant unit is the entire service, not just its most visible server.

Consider this simplified web application:

```text
Users
  |
Single internet connection
  |
Single firewall
  |
Load balancer
  |------------|
Web A        Web B
  |------------|
Single database
```

The two web servers protect against certain individual web-server failures. They do not protect against loss of the internet connection, firewall, load balancer or database. Even if every device has a spare, both servers may depend on one electrical circuit or building cooling system.

A **failure domain** is a collection of resources that can be affected by the same disruption. Examples include a rack, power circuit, building, cloud availability zone, identity service or shared software dependency. Two devices in the same failure domain are not independent protection against failure of that domain.

To analyze a design, trace a successful user request from client to application to data and back. Include DNS, authentication, certificates, routing, storage, monitoring, power and administrators. Then ask which resources are shared by the primary and alternate paths.

**Example:** Two internet service providers may lease fiber through the same underground conduit. A construction accident can sever both. Provider diversity is useful only if the relevant physical and logical dependencies are sufficiently independent.

**Important limitation:** Adding redundant components introduces more configurations, credentials, software and monitoring obligations. A poorly understood redundant design can fail more often than a simpler well-managed design.

## 5. High availability: Load balancing, clustering and failover

### Load balancing

A load balancer distributes incoming requests across backend instances. It may operate at the transport layer (for example, TCP connections) or application layer (for example, HTTP requests). Health checks help determine whether a backend should receive new traffic.

When a backend fails its configured health checks, the load balancer can remove it from rotation. Requests then go to healthy instances. This approach combines scaling and availability, provided sufficient capacity remains. Existing connections to the failed backend may still break.

A meaningful health check tests more than whether a port is open. An HTTP endpoint might report success while the database is unavailable. Conversely, poorly designed dependency checks can remove every backend during a temporary shared dependency failure. The check should reflect the service's actual readiness and avoid introducing unnecessary load.

Session state also matters. If a shopping cart exists only in one server's memory, switching servers can lose the cart. Shared session storage, stateless application design or carefully configured session affinity can help. Shared storage itself needs protection.

### Clustering

A cluster consists of cooperating nodes managed as a coordinated service. Nodes may share workload, maintain redundant copies of state or elect a leader. A database cluster can promote a replica when the current primary fails, subject to replication health and coordination rules.

Clusters often use quorum: enough participating nodes must agree to authorize certain operations. This can prevent conflicting writes during a network partition. A split-brain condition occurs when separated nodes each incorrectly act as the authoritative primary. Quorum and fencing mechanisms can help prevent it, but their exact operation depends on the platform.

| Method | Primary mechanism | Typical application | Limitation |
| --- | --- | --- | --- |
| Load balancing | Distribute traffic among healthy targets | Web/API services | Shared dependencies may still fail |
| Clustering | Coordinate multiple service nodes | Databases, storage, application platforms | Coordination and state management complexity |
| Failover | Transfer service to an alternate resource | Firewalls, database primaries | Detection and transition may interrupt sessions |

These approaches are not mutually exclusive. A clustered application can also sit behind a load balancer.

### Active-active and active-passive

In an active-active design, multiple instances handle production work. In an active-passive design, one instance handles production traffic while another waits to take over. A passive node can be running and synchronized; passive does not necessarily mean powered off.

Active-active can make better use of resources, but coordinating writes and distributing state may be difficult. Active-passive can simplify ownership of state, but standby capacity must be adequate and regularly tested.

**Failover** moves operations to an alternate resource. **Failback** returns operations to the intended primary after it is repaired and synchronized. Returning too early risks another outage or loss of writes made while the alternate was active.

**Exam tip:** When the question emphasizes distributing incoming requests, think load balancing. When it emphasizes cooperation among service nodes, think clustering. When it emphasizes switching to a standby, think failover.

## 6. Recovery sites and architectural diversity

A recovery site provides an alternative location for restoring operations. Its readiness determines how much preparation is needed after a disruption.

| Site | Typical readiness | Relative cost | Relative recovery speed |
| --- | --- | --- | --- |
| Hot | Operational infrastructure and sufficiently current software/data | High | Fastest |
| Warm | Some equipment and configuration ready; additional restoration needed | Medium | Intermediate |
| Cold | Basic facility and utilities; equipment and data must be provisioned | Low | Slowest |

These are relative categories rather than guaranteed recovery times. A hot site with broken replication or expired credentials might be unusable. A well-automated warm site could recover faster than a poorly maintained hot site.

**Geographic dispersion** separates sites so that a regional event is less likely to affect both. Distance alone is not enough; review flood exposure, electrical grids, internet routing, staffing, legal restrictions on data location and supply chains. A recovery site must meet data protection requirements just as the primary site does.

**Platform diversity** uses different technology platforms to reduce dependence on one implementation. For example, an organization might maintain an independent recovery environment that does not use the same vulnerable management agent as its production systems. Diversity can reduce correlated failures, but multiplies skills, testing and patch management requirements.

**Multicloud** uses more than one cloud provider. It can reduce exposure to a provider outage when the alternate environment is actually capable of delivering the required service. Merely purchasing services from two providers does not guarantee resilience. Both environments may share external DNS, an identity provider or a deployment pipeline. Data transfer charges, compatibility and consistency must also be considered.

A multi-zone design within one cloud provider can address some local failures at lower complexity than multicloud. A multi-region design addresses a broader category of disruptions but can still share provider-level dependencies. Select diversity according to the threat and business requirements, not the number of vendors.

## 7. Recovery objectives and continuity of operations

A business impact analysis (BIA) identifies critical processes, dependencies and the consequences of disruption. It helps establish recovery priorities before an incident occurs.

**Recovery time objective (RTO)** is the target maximum elapsed time for restoring a defined service or function after disruption. **Recovery point objective (RPO)** is the target maximum tolerable data loss expressed as time. Neither is a guarantee of actual performance.

Suppose a database fails at 14:00. Its latest usable recovery point is 13:45; service is restored at 15:30. The actual recovery point age is 15 minutes and elapsed recovery time is 90 minutes. If the objectives were RPO 30 minutes and RTO 60 minutes, the data-loss target was met but the recovery-time target was missed.

```text
13:45                 14:00                         15:30
Usable recovery point Failure                       Service restored
       |-- 15 min --|       |------- 90 min -------|
        Recovery point age      Elapsed recovery time
```

🧠 **Memory hook:** RPO looks backward to recoverable data; RTO looks forward to restored service.

Continuity of operations planning (COOP) defines how essential functions continue through disruption. A hospital might use numbered paper forms and controlled manual identification procedures while its systems recover. Staff must protect the forms, record who made changes and reconcile transactions into the restored system. Manual processes must also be tested; they can introduce privacy, integrity and safety risks.

### Capacity planning

Resilient infrastructure needs enough resources to operate during both routine peaks and failure conditions. Consider people, processing, memory, storage, network bandwidth, physical space and power/cooling capacity.

Suppose three servers can each process 100 requests per second and peak demand is 240. Normal total capacity is 300. After losing one server, capacity falls to 200, which is insufficient. Adding one equivalent server gives 300 remaining capacity after a single failure, assuming the load balancer and dependencies can support it.

An **N+1** design provides the required N units plus one additional unit. A **2N** design duplicates the required capacity. Actual redundancy depends on shared dependencies and how resources are distributed. Staff must also have capacity: a recovery plan requiring one administrator who may be unavailable is a human SPOF.

## 8. Backups and trustworthy recovery

A backup is a recoverable copy of data or configuration. A successful backup job does not prove that a complete restoration will succeed. Recovery requires intelligible data, compatible software, keys, permissions and tested procedures.

| Approach | Mechanism | Main trade-off |
| --- | --- | --- |
| Full backup | Copy all selected data | Simple recovery chain; larger storage and backup window |
| Incremental | Copy changes since the preceding backup | Efficient backups; recovery may require several copies |
| Differential | Copy changes since the last full backup | Growing backup size; generally shorter recovery chain |
| Snapshot | Capture a point-in-time representation | Fast rollback in supported systems; may share underlying storage |
| Replication | Copy or synchronize state to another system | Rapid access to recent data; can propagate damaging changes |
| Journaling | Record changes or transactions for consistency/replay | Supports recovery to supported points; requires retained logs and a valid base |

Snapshots are implementation-dependent. Some are independent copies; others rely on the original storage. A filesystem snapshot may not capture application-consistent database state unless coordinated with the application. Replication can be synchronous (acknowledgment depends on remote persistence according to configuration) or asynchronous (remote updates may lag). Synchronous replication usually adds latency and may affect availability if the remote path fails. Asynchronous replication can lose recent writes during failover.

Journaling is also not automatically a backup. A filesystem journal may primarily help restore metadata consistency after a crash. Database transaction logs can support point-in-time recovery when combined with appropriate backups and retention.

### Protecting the recovery environment

Onsite backups may offer fast restoration but can be destroyed by the same fire or theft as production. Offsite backups reduce location risk. Offline or immutable copies reduce the ability of compromised production credentials to modify or delete all recovery points. Immutability means that alteration or deletion is restricted according to configured retention controls; verify that privileged users cannot trivially bypass those protections.

The 3-2-1 heuristic recommends three copies of data including production, two different media types or appropriately independent storage approaches, and one offsite copy. Additional offline or immutable protection and verified restores strengthen the design. This is a heuristic, not a compliance guarantee.

Backup frequency should follow RPO and the rate of data change. Daily backups cannot reliably meet a 15-minute RPO without additional recovery mechanisms. Retention should account for delayed detection: if corruption began weeks ago, yesterday's backup may already contain it.

Encrypt backups in transit and at rest. Restrict access using separate privileges, protect recovery keys and maintain authorized key recovery procedures. Losing the only decryption key can make an otherwise intact backup useless. Audit restore actions and validate restored application behavior, not just archive checksums.

**Exam trap:** RAID (redundant array of independent disks), replication and snapshots do not by themselves replace independent recoverable backups. They can preserve or propagate unwanted changes.

## 9. Physical, environmental and power security

Infrastructure security extends beyond software. Someone with physical access might disconnect network cables, steal storage, connect unauthorized equipment or damage essential systems. A fire or water leak can cause similar availability consequences without malicious intent.

| Control | Purpose | Limitation |
| --- | --- | --- |
| Bollards and barriers | Prevent or deter unauthorized vehicle access | Do not authenticate people |
| Access badges and guards | Restrict and monitor entry | Require revocation and visitor procedures |
| Access control vestibule | Control movement through successive doors | Must meet emergency egress and safety requirements |
| Cameras | Deter, detect and document events | Do not independently prevent every intrusion |
| Infrared, pressure, microwave or ultrasonic sensors | Detect specified motion or presence conditions | Placement and false alarms affect effectiveness |
| Locked racks | Limit access to equipment | Do not protect against facility-wide outages |
| Environmental monitoring | Detect heat, humidity, smoke or leaks | Requires alert handling and appropriate response |
| Fire detection and suppression | Reduce fire-related loss | Must be designed for occupancy and equipment safety |

Physical controls follow the same layered design principles as network controls. A camera may provide evidence but cannot replace a locked rack. An access control vestibule may reduce unauthorized entry but emergency exit requirements take precedence over ordinary access restrictions. Consult qualified facilities and safety professionals rather than improvising electrical or fire-protection installations.

### UPS and generators

An uninterruptible power supply (UPS) uses stored energy to provide temporary power when utility power fails. Depending on design, it may also condition incoming power. It provides time for a generator to start, for service to transfer, or for controlled shutdown.

A generator supplies power for longer outages while fuel, maintenance and environmental conditions permit. Starting and transferring the electrical load takes time; a generator does not automatically provide seamless power. An automatic transfer switch can transfer supported loads between power sources. The UPS bridges the interruption when sized appropriately.

For an illustrative 600 Wh usable battery and 300 W load, ideal runtime is `600 / 300 = 2 hours`. Actual runtime may be lower due to losses, battery condition, discharge rates, temperature and equipment reserve requirements. Use manufacturer runtime data for real sizing. UPS watt and volt-ampere ratings also constrain connected load.

Redundant power supplies connected to the same power distribution unit (PDU) remain dependent on that unit. Independent circuits and power paths reduce that dependency when the facility supports them. Include switches, storage, DNS infrastructure and cooling in critical power planning; a powered server without networking or safe temperature may still be unavailable.

**Memory hook:** UPS bridges; generator sustains; backups restore.

## 10. Recovery testing and secure operation during failure

A recovery procedure needs evidence that it works. Testing methods differ in realism, cost and operational risk.

| Test | What participants do | What it can reveal |
| --- | --- | --- |
| Tabletop | Discuss a hypothetical incident and decisions | Missing roles, escalation paths and unclear priorities |
| Simulation | Exercise actions in a modeled or isolated scenario | Technical or procedural weaknesses without full production transfer |
| Parallel processing | Run alternate processing alongside normal operations | Output differences and readiness of alternate environment |
| Failover test | Transfer a defined service to alternate infrastructure | Real switching behavior and recovery performance |

A tabletop cannot prove database restoration speed. A successful isolated restore cannot prove production network failover. Choose tests that address specific uncertainties and expand scope gradually with authorized change windows, rollback plans and safety controls.

A useful test record contains the objective, scope, participants, start and finish times, expected outcome, actual observations, deviations, corrective actions and retest owner. Record whether actual recovery met RTO and RPO, whether permissions remained intact, and whether logs and monitoring operated during the outage.

### Fail-open and fail-closed behavior

A fail-open control permits a defined operation when its control mechanism fails; a fail-closed control denies the operation. The choice affects security and availability. For example, a failed network inspection device might bypass inspection to preserve connectivity or block traffic to prevent uninspected access. Neither choice is universally appropriate.

Life-safety requirements complicate physical access decisions: people must be able to exit safely during emergencies. Design entry restrictions and emergency egress according to applicable requirements rather than assuming every door should lock or unlock under every failure.

Emergency access (sometimes called break-glass access) should be narrowly scoped, independently protected, monitored and reviewed. An outage is not a justification for sharing administrator credentials or disabling all authentication. Recovery operations often use powerful privileges; separate recovery accounts and audit their use.

## 11. Worked case study: Keeping a diagnostic laboratory operational

A regional diagnostic laboratory operates a results portal used by physicians. The portal has two web servers behind one load balancer, a single database, one internet circuit and one server-room UPS rated for ten minutes under current load. Its nightly backup is stored on a network share using production administrator credentials. The laboratory requires RTO of 60 minutes and RPO of 15 minutes for the portal.

During a utility outage, the UPS runs down and the portal becomes unavailable. The database restarts successfully afterward, but the organization realizes that a destructive database failure would require restoring a backup potentially almost 24 hours old. The two web servers did nothing to address the power outage.

### Step 1 — Map dependencies and priorities

The web tier has limited redundancy. The database, load balancer, network circuit and power path remain single dependencies. Clinicians also depend on working authentication and name resolution. The laboratory must prioritize access to validated results rather than treating every internal application as equally critical.

### Step 2 — Select complementary controls

| Weakness | Recommended design | Remaining risk |
| --- | --- | --- |
| Database failure | Replicated database with monitored promotion and tested restoration | Replication may propagate corruption |
| Nightly-only backup | Frequent recoverable transaction logs and independent encrypted backups | Recovery depends on valid logs, keys and base backup |
| Shared backup privileges | Separate backup identity and protected retention | Privileged recovery accounts still need oversight |
| Short power reserve | UPS supporting critical equipment during generator transfer | Fuel, cooling and prolonged outage risks |
| Single load balancer and circuit | Redundant traffic distribution and independently routed connectivity | Shared regional or provider failure |
| Facility loss | Warm or hot alternate environment chosen through timed recovery tests | Cost, staffing and synchronization complexity |

### Step 3 — Validate the recovery target

In a fictional authorized recovery exercise, the alternate portal becomes operational 42 minutes after the simulated disruption. Its newest validated recovery point is eight minutes before the simulated failure. Both objectives are met in this exercise, but one test does not guarantee future performance. A later test should include loss of the identity service and a corrupted recent recovery point.

During the interruption, authorized staff can use a documented telephone verification procedure for urgent results. They record disclosures, confirm recipient identity and reconcile actions afterward. This preserves essential operations without treating confidentiality as optional.

**Takeaway:** Resilience is a property of the complete service and its operating procedures, not the number of servers in the web tier.

## 12. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Distinguish redundancy and HA. | _Write here._ |
| Explain a shared failure domain. | _Write here._ |
| Compare load balancing and clustering. | _Write here._ |
| Compare hot, warm and cold sites. | _Write here._ |
| Explain RTO and RPO. | _Write here._ |
| Explain why replication is not sufficient backup protection. | _Write here._ |
| Compare UPS and generator functions. | _Write here._ |
| Compare tabletop and failover tests. | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Distinguish redundancy and HA. | Redundancy supplies alternatives; HA combines alternatives, monitoring and switching mechanisms to minimize interruptions. |
| Explain a shared failure domain. | Multiple resources depend on a common element or are exposed to the same event, so they can fail together. |
| Compare load balancing and clustering. | Load balancing distributes requests; clustering coordinates nodes to deliver a service. They can coexist. |
| Compare hot, warm and cold sites. | Hot sites are highly prepared, warm sites require additional preparation, and cold sites primarily provide facilities. |
| Explain RTO and RPO. | RTO targets restoration time; RPO targets tolerable loss of recent data measured in time. |
| Explain why replication is not sufficient backup protection. | Replicas can receive malicious or accidental changes; independent historical recovery points preserve alternatives. |
| Compare UPS and generator functions. | UPS provides temporary stored power and bridges transitions; a generator supplies longer-duration power when supported. |
| Compare tabletop and failover tests. | Tabletop tests decisions through discussion; failover tests actual transfer to an alternate system. |

</details>

## 13. Hands-on lab: Design an outage-resistant community clinic

**Time:** 20–25 minutes  
**Tools:** Paper or a text editor and calculator.  
**Safety and scope:** All data and events are fictional. Do not disconnect real equipment, modify production services or test live failover.

### Scenario and constraints

A clinic requires its appointment and patient-record application to recover within 60 minutes, with no more than 15 minutes of lost data. Peak demand is 150 requests per second. Its two web servers each handle 100 requests per second. The servers share a switch, firewall, database and electrical circuit. Backups run nightly onto local storage. The clinic has an approved manual registration procedure for short outages but has never exercised it.

You have 12 fictional budget credits. Costs are illustrative and do not represent market prices.

| Upgrade | Credits |
| --- | ---: |
| Additional web server | 2 |
| Redundant switch and firewall paths | 3 |
| Database standby with monitored failover | 3 |
| Independent encrypted backup and recoverable logs every five minutes | 2 |
| UPS for critical network and server equipment | 2 |
| Tested generator service | 4 |
| Warm alternate site | 5 |
| Tabletop and isolated restore exercise | 1 |

### Step 1 — Draw the baseline

Draw the client, firewall, switch, two web servers and database. Add the shared electrical circuit and backup storage. Circle at least four single dependencies. Label the data path and recovery path separately.

### Step 2 — Evaluate capacity

Calculate normal web capacity and remaining capacity if one server fails. Compare it with peak demand. Determine how an additional equivalent server changes the result. Assume the network and database are not bottlenecks for this calculation only.

### Step 3 — Select an architecture

Choose upgrades within 12 credits. For each, write the failure addressed, the security benefit and the residual risk. You cannot eliminate every risk. Prioritize the stated recovery objectives and explain any trade-off. A backup upgrade alone does not prove that the RTO is achievable.

### Step 4 — Interpret the simulated exercise

Assume the latest usable recovery point precedes a simulated 10:00 failure by 12 minutes. The application becomes usable at 10:50. Determine whether the RTO and RPO are met. Then assume a second exercise restores the application at 11:20 using a recovery point from 09:30. Reevaluate both targets.

### Step 5 — Prepare continuity and testing

Write three actions staff should take during an outage, including how to protect manually recorded information. Select a recovery test to evaluate actual switching and another to evaluate role clarity. Identify one risk your budget does not solve and specify what evidence would justify funding it next.

### Interpretation questions

1. Why do the original two web servers not provide sufficient capacity during one server failure?
2. Which shared dependencies can disable both servers?
3. What package did you choose, and what did you defer?
4. Which objectives are met in each simulated exercise?
5. Why should the clinic test restoration instead of relying on the backup schedule?
6. How should the clinic protect information during manual operation?

<details markdown="1">
<summary>Reveal lab answers</summary>

1. Normal capacity is 200 requests per second; after one failure it is 100, below the 150 requirement. Three servers provide 200 after one failure, assuming other dependencies support that load.
2. The switch, firewall, database and shared electrical circuit are single dependencies. Local backup storage also shares location risk.
3. One defensible package is additional web server (2), redundant switch/firewall paths (3), database standby (3), independent backup/logs (2), and UPS (2), totaling 12. This addresses local equipment and short power failures while improving recoverability. It does not address prolonged power loss or complete site loss. Another defensible package might prioritize an alternate site or testing and explicitly accept more local failure risk. No package should be described as meeting objectives without evidence. Independent backup/log recovery and controlled failover must be tested before reliance; the clinic should allocate staff time for validation or seek additional funding.
4. First test: recovery takes 50 minutes and the recovery point age is 12 minutes; both objectives are met. Second test: recovery takes 80 minutes and point age is 30 minutes; neither is met.
5. A completed backup may be inconsistent, inaccessible, corrupted or too slow to restore. Restoration exercises validate keys, dependencies, integrity and elapsed time.
6. Use approved numbered forms, verify identities, limit data collection and access, store forms securely, and reconcile transactions with audit records. A tabletop checks role clarity; an authorized failover test checks actual switching. A prolonged outage is a reasonable residual risk to evaluate using power history, runtime estimates and the cost of disrupted care.

</details>

## 14. Eight original Security+-style questions

These are original practice questions, not actual CompTIA exam items.

### Q1. A company keeps a spare firewall in a warehouse. Which statement BEST describes the arrangement?

A. It guarantees fault tolerance.  
B. It provides redundancy without necessarily providing HA.  
C. It provides active-active clustering.  
D. It guarantees zero data loss.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** A spare provides an alternative but requires deployment. A assumes uninterrupted operation; C requires concurrently active infrastructure; D concerns recoverable data, not spare hardware.

</details>

### Q2. Two application servers remain healthy, but users lose access when their shared firewall fails. What explains the outage?

A. Geographic dispersion  
B. Journaling  
C. Platform diversity  
D. A single point of failure

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** The shared firewall is required by both paths. A and C describe diversity strategies; B records changes for recovery or consistency and does not explain the common network dependency.

</details>

### Q3. A service must distribute HTTP requests among healthy web instances. Which control MOST directly performs this function?

A. Load balancer  
B. Cold site  
C. UPS  
D. Backup journal

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** A load balancer selects backend targets and can use health checks. B provides alternate facilities, C temporary power and D recovery information; none directly distributes requests.

</details>

### Q4. A company's recovery facility has installed equipment but requires application restoration and additional configuration before use. Which site type BEST fits?

A. Cold  
B. Hot  
C. Warm  
D. Active-active production region

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** A warm site has intermediate readiness. A normally requires more provisioning; B is more fully prepared; D describes a live production arrangement rather than the stated partially prepared site.

</details>

### Q5. A database has an RTO of two hours and RPO of 15 minutes. Recovery takes 90 minutes using data from 25 minutes before failure. What is the result?

A. Both objectives are met.  
B. RTO is met; RPO is missed.  
C. RPO is met; RTO is missed.  
D. Neither objective is met.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Ninety minutes is within two hours, while 25 minutes exceeds the permitted 15-minute recovery point age. A, C and D incorrectly compare at least one measurement with its target.

</details>

### Q6. Ransomware encrypts production files and their continuously synchronized replicas. Which additional control BEST supports restoration of earlier clean data?

A. Another writable replica using the same credentials  
B. An additional load balancer  
C. A larger UPS  
D. Independent protected historical backups

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Protected historical recovery points can offer pre-infection data if retained and validated. A risks propagating the same changes; B and C address traffic and power rather than historical restoration.

</details>

### Q7. A building generator requires time to start after utility power fails. Which control bridges that interval for critical equipment?

A. UPS  
B. Access badge  
C. Database cluster  
D. Offsite backup

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** A correctly sized UPS supplies temporary power. B controls entry, C coordinates database services, and D protects recovery data; none provides the required electrical energy.

</details>

### Q8. A team has completed a tabletop and now needs evidence that its standby application can accept live service traffic within the recovery target. Which test is MOST appropriate?

A. Another discussion-only tabletop  
B. Review the purchase invoice for standby servers  
C. Authorized failover test with measured recovery time  
D. Verify that cameras retain recordings

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** A controlled failover exercise measures the actual transfer and recovery performance. A clarifies decisions but does not demonstrate switching; B proves acquisition, not readiness; D tests an unrelated physical control.

</details>

**Score guide:** 7–8 correct: strong understanding; 5–6: revisit Sections 3–6 for architectural distinctions, Section 7 for metrics, and Sections 8–10 for recovery controls; 0–4: repeat Sections 12–13 and retry.

## 15. Discussion challenge

A municipality operates an emergency notification platform. It can afford either a geographically separate recovery environment or substantial upgrades to its primary facility, but not both this year. The primary facility already has a generator, although its identity provider and application database remain shared dependencies. Local flooding and credential compromise are both credible threats.

Recommend a strategy. Explain which failures it addresses, what it leaves unresolved, how you would measure success and which compensating controls should be introduced immediately. Consider the costs of maintaining the alternate environment, staffing during emergencies and the consequences of replicating compromised data.

**Bonus:** Explain how an emergency access procedure could preserve availability without unnecessarily sacrificing confidentiality and integrity.

## 16. Study from the Treadmill 🏃

**Direct links to specific videos:** These Professor Messer video pages were checked against search results.

- [Resiliency — SY0-701, Objective 3.4](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/resiliency-sy0-701/) — Focus on distinguishing service distribution, coordinated nodes and recovery-site readiness.
- [Infrastructure Considerations — SY0-701, Objective 3.1](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/infrastructure-considerations-sy0-701/) — Focus on the relationship between cost, scalability, availability and recovery.

**Active-listening prompts:**

- Why can a redundant design still contain a single point of failure?
- Which architectural choice would help with local equipment failure but not a regional disaster?
- How would you justify additional resilience spending using measurable business requirements?

## 17. One-minute recap

- **Redundancy** provides alternatives; **HA** minimizes interruption through coordinated operation or switching.
- **Load balancing** distributes traffic; **clustering** coordinates service nodes. Inspect shared failure domains in both designs.
- **Hot, warm and cold sites** trade readiness against expense; geographic and platform diversity can reduce correlated failures.
- **RTO** measures targeted restoration time; **RPO** expresses tolerable loss of recent data in time units.
- **Independent backups** protect historical recovery options that replication alone may not preserve.
- **UPS, generators and physical controls** protect different aspects of infrastructure availability and security.
- **Continuity and recovery testing** validate that people, systems and procedures can deliver essential functions securely.

**Golden rule:** A resilient design is not proven by the equipment it contains, but by the failures it has been demonstrated to survive.

**Next:** Day 41 — Topic pending curriculum confirmation.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
