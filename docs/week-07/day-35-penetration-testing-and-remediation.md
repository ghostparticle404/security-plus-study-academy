# Day 35 — Penetration Testing and Remediation

**Week 07 | Vulnerability Management**

A vulnerability scanner reports that a customer portal may expose confidential information. An authorized tester confirms that a test account can view another test account's records. The development team must now correct the weakness, and the security team must verify that the correction works. Today's lesson follows that journey from suspected vulnerability to documented resolution.

## 2. Security+ exam alignment

- **Domain 4 — Security Operations, Objective 4.3:** Explain various activities associated with vulnerability management, including penetration testing, responsible disclosure, remediation, validation, and reporting.
- **Domain 5 — Security Program Management and Oversight, Objective 5.5:** Explain types and purposes of audits and assessments, including penetration testing perspectives and reconnaissance.
- **Domain 2 — Threats, Vulnerabilities, and Mitigations, Objective 2.5:** Explain the purpose of mitigation techniques used to secure the enterprise.

**Prerequisite:** [Day 34 — Vulnerability Assessment and Scanning](day-34-vulnerability-assessment-and-scanning.md).

## 3. Learning objectives

By the end of this lesson, you should be able to:

- Distinguish vulnerability scanning, vulnerability assessment, penetration testing, and remediation validation.
- Identify the purpose of written authorization, assessment scope, and rules of engagement.
- Compare known, partially known, and unknown testing environments and active and passive reconnaissance.
- Explain responsible disclosure and bug bounty programs.
- Select appropriate remediation or mitigation techniques based on evidence and organizational risk.
- Draft a remediation plan with accountable owners, deadlines, and measurable validation criteria.

## 4. Why this topic matters

Imagine a building inspector discovers a door with a damaged lock. The observation suggests a vulnerability. An authorized locksmith confirms that the lock fails to prevent entry. A maintenance worker replaces the lock, and a subsequent inspection verifies that it functions correctly. Each person contributes different evidence; none of these stages alone establishes that the building is secure.

Cybersecurity follows similar reasoning. A scanner can identify thousands of possible weaknesses, but organizations need more than a list of alerts. They need to understand whether a finding is accurate, what its consequences could be, who will address it, and whether the response actually reduces risk.

Penetration testing can reveal interactions between weaknesses that individual scans miss. Remediation transforms findings into security improvements. Without ownership and validation, even an excellent assessment can become a report that nobody acts upon.

## 5. Detailed instruction

### From assessment to remediation

A vulnerability scan checks for indicators of known weaknesses. A vulnerability assessment interprets those indicators alongside configuration evidence, asset importance, and threat information. A penetration test is an authorized attempt to demonstrate whether security weaknesses can be exploited within agreed limits. Remediation addresses the weaknesses, while validation determines whether the response achieved its intended effect.

These activities complement each other rather than forming a compulsory sequence for every finding. An organization can install an urgent vendor patch without first attempting to exploit the vulnerability. Conversely, a complicated authorization defect may require manual testing because version-based scanning cannot establish whether access controls work correctly.

```text
Discover and assess weaknesses
             |
             v
Validate findings and determine risk
             |
             v
Assign owner and select response
             |
             v
Implement approved remediation or mitigation
             |
             v
Verify effectiveness and document results
             |
             v
Close, monitor, or reopen the finding
```

Penetration testing is one possible source of validation and evidence. It is not a prerequisite for all remediation, and a successful test does not establish that every other vulnerability has been discovered.

### Authorization, scope, and rules of engagement

Written authorization establishes permission to conduct an assessment. Scope defines the assets, accounts, locations, and activities covered by that permission. Rules of engagement (RoE) specify how testing may take place.

A useful RoE identifies the authorized systems, excluded systems, testing window, permitted techniques, prohibited actions, emergency contacts, stop conditions, evidence-handling requirements, and reporting expectations. It also identifies who can approve changes to the scope. Permission from an employee who lacks authority over the system is not sufficient.

For example, an organization might authorize testing its staging portal using synthetic customer records, while prohibiting production testing, denial-of-service experiments, and access to third-party payment services. A tester who discovers a link from staging to production must not assume that the linked system is authorized. The appropriate response is to stop that activity and ask the designated contact for clarification.

Testing can cause downtime, expose sensitive information, or trigger defensive alerts. The RoE reduces these risks and provides a shared understanding between testers, system owners, and defenders. It does not eliminate all operational risk; unexpected effects still require escalation.

### Testing perspectives and reconnaissance

The testing perspective describes how much information is available to the tester before the assessment begins.

- **Known environment:** The tester receives detailed internal information, potentially including architecture, source code, and designated credentials. Often called white-box testing.
- **Partially known environment:** The tester receives limited information, such as a user account or selected architecture diagrams. Often called gray-box testing.
- **Unknown environment:** The tester receives little or no internal technical information. Often called black-box testing.

An unknown environment does not mean an unauthorized environment. All three approaches require permission. Known testing often supports deeper coverage within a limited time, while unknown testing can help evaluate exposure from an outsider's perspective. Partially known testing is useful when the objective is to assess what an ordinary user or limited partner account can access.

Reconnaissance is information gathering. Passive reconnaissance gathers information without directly probing the target systems, such as reviewing publicly available documentation. Active reconnaissance interacts directly with target systems, such as an authorized network discovery scan. Active interaction may create log entries and alerts. Passive techniques are not automatically unrestricted: privacy, contractual restrictions, and scope still matter.

Internal and external testing describe the tester's starting position, not the amount of information supplied. A known-environment test can be external; an unknown-environment test can begin inside a network. Physical penetration testing evaluates protections such as controlled entrances and secure areas, and requires explicit authorization for physical activities.

### Penetration testing process and evidence

A typical engagement includes planning, discovery, analysis, controlled validation, reporting, and cleanup. NIST SP 800-115 provides a broader technical assessment framework.

During planning, the parties establish objectives and constraints. Discovery identifies relevant assets and possible weaknesses. Analysis determines which findings warrant further investigation. Controlled validation gathers the minimum evidence necessary to demonstrate a security impact without exceeding authorization.

Security+ students should recognize the conceptual purpose of exploitation (demonstrating that a weakness can be used), privilege escalation (obtaining greater permissions), lateral movement (accessing additional systems from an existing foothold), and persistence (maintaining access). These activities are not automatically permitted in a penetration test. A narrowly scoped engagement may stop after a single access-control failure is confirmed.

A finding should explain what was observed, the affected asset, prerequisites, demonstrated impact, limitations, supporting evidence, and recommended corrective action. Test artifacts should be removed as agreed, and evidence should be retained or destroyed according to the engagement requirements.

A report must distinguish demonstrated impact from possible impact. If a tester accessed one synthetic record, that is evidence of an access-control weakness, but not proof that every production record was exposed or that a criminal intrusion occurred.

### Red, blue, and purple teams

Red teams emulate adversary behavior to evaluate security controls and organizational response. Blue teams defend systems, monitor activity, investigate alerts, and respond to incidents. Purple teaming emphasizes cooperation and knowledge sharing between offensive and defensive functions.

A penetration test generally has a defined assessment objective, such as identifying weaknesses in a particular application. A red-team exercise may instead evaluate whether an organization can detect and respond to an adversary pursuing a broader objective. The two can overlap, but they are not interchangeable.

For example, a controlled exercise may reveal that an application generates an access-denied event but that the event never reaches the central monitoring platform. The blue team can improve log collection and alerting, while developers address the underlying application weakness. Both preventive and detective controls benefit from the exercise.

### Responsible disclosure and bug bounties

Responsible or coordinated vulnerability disclosure is a process for communicating a discovered weakness to the organization capable of addressing it. A vulnerability disclosure policy (VDP) describes acceptable research and the reporting channel. A bug bounty program may additionally offer rewards for eligible findings.

A reward is not guaranteed, and a public program does not authorize testing every system operated by the organization. Researchers must follow program scope, evidence-handling requirements, and restrictions. Disclosure timelines vary and may involve coordination among vendors, researchers, and affected organizations.

A report should contain sufficient information for the organization to understand and reproduce the issue safely. Sensitive information should be minimized and transmitted through approved channels. Finding a weakness is not permission to collect real customer data or publish secrets.

### Remediation and mitigation

Remediation corrects a weakness or removes the vulnerable condition. Mitigation reduces the likelihood or impact of exploitation. The terms sometimes overlap in operational practice; a vulnerability management process may group both under remediation activities.

Examples include patching software, updating firmware, correcting access-control logic, removing unnecessary services, changing insecure defaults, replacing unsupported products, and enforcing secure configurations. A compensating control reduces risk when the preferred control cannot presently be implemented. Examples include restricting access to a legacy server, introducing network segmentation, or using a narrowly configured application firewall while developers prepare a permanent fix.

A compensating control does not necessarily eliminate the underlying weakness. An application firewall might block some malicious requests without correcting the application's authorization logic. Track the underlying issue and document what the temporary control does and does not protect against.

### Risk response and exceptions

Risk mitigation reduces risk; avoidance stops the activity that creates the risk; transfer shifts some financial or contractual consequences to another party; acceptance acknowledges residual risk through an authorized decision.

Cyber insurance may transfer some financial exposure, but it does not repair a vulnerability or prevent an attack. Decommissioning an unnecessary vulnerable system can avoid the associated exposure. Accepting a legacy system's residual risk requires an appropriate decision-maker, documented rationale, review date, and any necessary safeguards.

An exception or exemption records an approved departure from a policy or remediation requirement. It should not be a convenient way to make overdue findings disappear. A time-limited exception should identify the affected system, reason, residual risk, approving authority, temporary safeguards, and expiration or review date.

### Patch management and change control

Patching is a managed process rather than simply installing updates. Identify affected assets and versions, obtain updates from trusted sources, evaluate applicability, test representative systems, obtain required approval, deploy, and verify the result.

A staging environment provides an opportunity to discover compatibility problems before changing production systems. A rollback plan defines how service can be restored if a change fails. Emergency changes may use an expedited approval process, but still require documentation and validation.

Some updates require restarting a service or device before the corrected code becomes active. A management console showing that a patch was downloaded is not the same as proof that the running application is protected. Configuration management also helps prevent a corrected setting from being overwritten by an outdated deployment template.

### Prioritization, ownership, and deadlines

As discussed in Day 34, CVSS describes technical severity, not the exact risk to an organization. Combine severity with exposure, known exploitation, asset importance, business consequences, and existing controls. A publicly reachable weakness affecting confidential records may warrant more urgent action than a technically severe weakness on an isolated laboratory system.

A service-level agreement (SLA) or internal remediation policy may define target response times. These depend on the organization's requirements; Security+ does not prescribe a universal deadline for every critical vulnerability.

Every actionable finding should have a named accountable role or team, a due date, and an expected validation method. Security teams may coordinate and verify fixes, while developers, system administrators, or suppliers implement them. Assigning responsibility to an unspecified group such as IT can leave important work unclaimed.

### Validation and closure

Validation includes rescanning, auditing, and verification. A rescan checks whether a previously detected condition remains visible to the assessment tool. An audit may compare current settings or records against requirements. Manual verification can test the actual security behavior, such as whether a server now denies prohibited access.

Choose validation methods that match the original weakness. A configuration defect may require inspecting effective configuration; an authorization defect requires positive and negative functional tests; a vulnerable dependency may require checking the deployed artifact rather than only the source repository.

A positive functional test confirms permitted behavior still works. A negative security test confirms prohibited behavior is rejected. Both matter: disabling an entire service might prevent unauthorized access but also prevent legitimate business activity.

A follow-up scan with missing coverage cannot establish closure. Record the scope, date, method, outcome, and evidence of retesting. If the weakness persists, reopen the finding. If risk is formally accepted, record that disposition separately from a verified permanent fix.

### Reporting and continuous improvement

Technical reports provide sufficient evidence for remediation teams; executive summaries describe business impact, risk trends, and decisions requiring leadership attention. A remediation tracker connects assessment evidence to owners, actions, deadlines, exceptions, and validation results.

Useful measures include overdue findings, time to remediation, recurrence rates, and the proportion of findings with verified closure. Metrics require context: fewer findings could mean improved security or reduced assessment coverage. A report should communicate limitations rather than presenting absence of evidence as evidence of safety.

Root cause analysis asks why the weakness existed and how recurrence can be prevented. Correcting one insecure server is valuable; correcting the provisioning template that created twenty insecure servers may be more valuable still.

**Further reading:** [NIST SP 800-115](https://csrc.nist.gov/pubs/sp/800/115/final) on technical security testing and assessment; [NIST SP 800-40 Rev. 4](https://csrc.nist.gov/pubs/sp/800/40/r4/final) on enterprise patch management.

## 6. Real-world cybersecurity examples

### Customer portal authorization weakness

In a fictional customer portal, a controlled assessment confirms that one synthetic user can access a record belonging to another synthetic user. The development team adds server-side authorization checks and tests each supported role. The retest confirms that legitimate access succeeds while cross-account access is denied. The team also reviews similar endpoints to reduce recurrence. Hiding record identifiers alone would not correct the authorization defect.

### Unsupported equipment in a manufacturing network

A production controller runs unsupported firmware and cannot immediately be replaced without disrupting operations. The owner restricts network access, monitors permitted connections, and schedules replacement. Management approves a temporary risk exception with an expiration date. The security team verifies the access restrictions but records the firmware weakness as unresolved until replacement or an effective permanent correction is verified.

### Patch deployment with incomplete coverage

A company deploys a critical update to 100 servers. Its management platform confirms success on 94, failure on four, and no recent contact with two. The team investigates the six unverified systems rather than recording all 100 as remediated. It verifies the running version after necessary restarts and repeats assessment checks before closure.

## 7. Comparison tables

| Activity | Main purpose | Typical evidence |
| --- | --- | --- |
| Vulnerability scanning | Identify potential weaknesses | Scanner observations |
| Vulnerability assessment | Interpret weaknesses and risk | Validated findings and context |
| Penetration testing | Demonstrate security impact within scope | Controlled test evidence |
| Audit | Evaluate against requirements | Records and control evidence |
| Remediation validation | Verify corrective action | Retest and configuration evidence |

| Testing perspective | Information provided | Typical advantage |
| --- | --- | --- |
| Known | Extensive technical information | Efficient, detailed coverage |
| Partially known | Selected information | Evaluates limited-access perspectives |
| Unknown | Little internal information | Evaluates outsider discovery and exposure |

| Response | Example | Underlying weakness necessarily removed? |
| --- | --- | --- |
| Remediation | Install corrective patch | Yes, if effective |
| Compensating control | Restrict access to vulnerable service | No |
| Risk transfer | Purchase applicable insurance | No |
| Risk acceptance | Approve documented residual risk | No |
| Avoidance | Retire unnecessary vulnerable service | Exposure removed if retirement is complete |

## 8. Technical demonstrations and practical activity

### Demonstration A: Verify local configuration evidence

On a system you own or administer with permission, use the applicable read-only commands to inspect local software and listening services. These commands do not establish whether a particular product is vulnerable; compare the evidence against the correct vendor advisory and expected baseline.

Linux:

```bash
uname -r
ss -lnt
```

macOS:

```bash
sw_vers
lsof -nP -iTCP -sTCP:LISTEN
```

Windows PowerShell:

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
Get-NetTCPConnection -State Listen
```

Record the operating system version and visible listening ports. A listening port is not automatically a vulnerability, and local output may not reveal external firewall restrictions. Do not disable services merely because they appear in these results.

### Demonstration B: Interpret a synthetic retest

The following is fictional evidence from a staging application using two approved test accounts. No real system or customer records are involved.

| Test | Before correction | After correction |
| --- | --- | --- |
| User views own record | Allowed | Allowed |
| User views another user's record | Allowed | Denied |
| Administrator views authorized record | Allowed | Allowed |

The negative security test now passes, and the positive functional tests show the expected functionality remains available. This supports closure for the tested behavior, provided the corrected version is deployed to the relevant environment and the assessment covers affected endpoints.

### Practical activity: Draft a remediation plan

You are a security analyst at fictional Harbor Services. An assessment identified the following findings. All data and deadlines are illustrative, not official exam requirements.

| ID | Finding | Context |
| --- | --- | --- |
| F-01 | Broken record-level authorization | Public customer portal; validated with synthetic accounts |
| F-02 | Unsupported firmware | Internal operational device; replacement requires downtime |
| F-03 | Unnecessary remote management service | Internet-facing server; no business requirement |
| F-04 | Possible outdated software | Scanner version inference; vendor patch status not verified |
| F-05 | Patch marked deployed | Host unreachable during follow-up scan |

For each finding, propose a priority, owner, immediate action, permanent response where appropriate, deadline or review date, and validation evidence. Distinguish confirmed vulnerabilities from unverified findings and approved exceptions. Assume any production change requires the organization's change approval process.

<details markdown="1">
<summary>Reveal sample remediation plan</summary>

| ID | Priority and owner | Response and illustrative target | Validation |
| --- | --- | --- | --- |
| F-01 | Urgent; application team | Restrict affected functionality as appropriate immediately; correct authorization within 48 hours | Approved cross-account negative tests and legitimate-access positive tests |
| F-02 | High; operations owner | Restrict access immediately; request time-limited exception and replacement plan within 30 days | Verify segmentation; confirm supported replacement before permanent closure |
| F-03 | Urgent; infrastructure team | Disable unnecessary service through approved change within 24 hours | Check effective configuration and authorized external reachability |
| F-04 | Pending verification; vulnerability analyst | Investigate package and vendor advisory within one business day; assign remediation if confirmed | Record authoritative package and patch evidence |
| F-05 | Pending validation; infrastructure team | Restore authorized assessment coverage and verify within one business day | Confirm running version and successful follow-up scan |

The exact deadline depends on the real organization's risk policy and operational constraints. F-04 is not automatically a false positive, and F-05 cannot be closed merely because deployment was reported. F-02 remains tracked while temporary protections are in place.

</details>

## 9. Common misconceptions and exam traps

- A vulnerability scan is not equivalent to a penetration test. Controlled demonstration of impact is a distinct objective.
- Unknown-environment testing is not unauthorized testing.
- Scope and rules of engagement are not interchangeable: scope defines boundaries, while rules specify how work proceeds.
- A bug bounty does not grant unlimited permission.
- A critical CVSS score does not automatically establish the highest business priority.
- Segmentation can reduce exposure without removing the underlying vulnerability.
- Installing a patch does not establish that it is effective or active.
- A failed or incomplete rescan is not proof of successful remediation.
- Risk acceptance is an authorized decision, not an undocumented decision to ignore a finding.

## 10. Key terminology and takeaways

| Term | Meaning |
| --- | --- |
| Penetration test | Authorized assessment demonstrating security weaknesses within constraints |
| Scope | Defined assessment boundaries |
| Rules of engagement | Agreed operational requirements for testing |
| Reconnaissance | Gathering information relevant to an assessment |
| Known environment | Assessment with extensive internal information |
| Responsible disclosure | Coordinated reporting of vulnerability information |
| Bug bounty | Program offering potential rewards for eligible findings |
| Remediation | Correcting or removing an identified weakness |
| Compensating control | Alternative safeguard reducing risk when preferred control is unavailable |
| Residual risk | Risk remaining after safeguards or responses |
| Exception | Approved departure from a requirement |
| Validation | Evidence-based verification of corrective action |

Remember that the goal is not to produce the longest list of vulnerabilities. It is to reduce meaningful risk through authorized assessment, accountable action, and reliable evidence of improvement.

## 11. Practice questions

**1.** A scanner identifies a possible authorization weakness, and an approved test demonstrates that one synthetic account can access another account's record. Which activity specifically demonstrates the security impact?

- A. Asset inventory
- B. Penetration testing
- C. Patch deployment
- D. Risk transfer

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Penetration testing demonstrates the impact of a weakness through controlled, authorized testing. A scanner may identify a potential problem without establishing the same impact.

</details>

**2.** During a staging assessment, a tester encounters a production system excluded from the written scope. What should the tester do next?

- A. Continue if no data is downloaded
- B. Test only during off-hours
- C. Treat the linked system as implicitly authorized
- D. Stop that activity and request authorization before expanding scope

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Testing must remain within authorized boundaries. Low-impact techniques and off-hours timing do not create permission.

</details>

**3.** A tester receives an ordinary employee account and selected network diagrams, but no additional internal information. Which testing perspective best describes this arrangement?

- A. Partially known environment
- B. Unknown environment
- C. Fully known environment
- D. Passive monitoring only

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** Limited supplied information defines a partially known environment. Reconnaissance techniques and starting network position are separate characteristics.

</details>

**4.** A vendor patch is not available for an important legacy application. The company restricts access to a small authorized network segment while awaiting a replacement. What is this restriction?

- A. Risk transfer
- B. Permanent software correction
- C. Compensating control
- D. False positive

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Segmentation reduces exposure while the underlying weakness remains. The organization must continue tracking residual risk.

</details>

**5.** A patch console reports successful installation, but the affected service still uses the old version until a restart. What is the best next step?

- A. Close the finding based on the console
- B. Arrange an approved restart and verify the running version and security condition
- C. Reclassify the weakness as a false positive
- D. Transfer the vulnerability to the vendor

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Deployment status alone is insufficient when the vulnerable version is still running. Verification must establish that the effective correction is active.

</details>

**6.** Management approves a temporary exception for an unsupported device. Which record best supports accountable risk management?

- A. A statement that no further review is required
- B. A scanner screenshot without an owner
- C. A lower CVSS score assigned by the asset owner
- D. Documented residual risk, approving authority, safeguards, owner, and review date

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** An exception should be traceable, authorized, and subject to review. It does not change the technical severity of the original vulnerability or establish permanent remediation.

</details>

## 12. Study from the Treadmill

Use these free Professor Messer SY0-701 video lessons while walking, exercising, or away from your desk.

| Video | Duration | Focus |
| --- | --- | --- |
| [Penetration Testing](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/penetration-testing-sy0-701/) | 6:21 | Engagement rules, controlled testing, vulnerability disclosure |
| [Vulnerability Remediation](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/vulnerability-remediation-sy0-701/) | 8:45 | Corrective actions, exceptions, and verification |
| [Penetration Tests](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/penetration-tests-sy0-701/) | Additional study | Testing perspectives and reconnaissance |

### Treadmill study challenge

- How would you explain the difference between scanning and penetration testing to a business owner?
- Why does an unknown-environment test still require authorization?
- What separates a permanent correction from a compensating control?
- Why should remediation plans contain owners and deadlines?
- What evidence would convince you that a reported vulnerability has actually been addressed?

After exercising, answer these questions without consulting the lesson. Revisit any concept you cannot explain using a practical example.

**Keep moving. Keep learning.**

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
