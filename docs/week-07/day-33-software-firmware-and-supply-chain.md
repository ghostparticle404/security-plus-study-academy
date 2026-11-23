# Day 33 — Software, Firmware and Supply Chain

**Week 07 | Vulnerability Management**

When an organization installs an application, it rarely installs only code written by the application's developer. It may also install libraries maintained by volunteers, commercial components, and software produced by automated build systems. Even the computer running the application depends on firmware supplied by hardware manufacturers. Today we examine how those relationships create security risks and how defenders manage them.

> **Security+ exam alignment**
>
> **SY0-701 — Domain 2: Threats, Vulnerabilities, and Mitigations**
>
> - **2.2:** Explain common threat vectors and attack surfaces.
> - **2.3:** Explain various types of vulnerabilities, including malicious updates, firmware, legacy platforms, and supply chain vulnerabilities involving service providers, hardware providers, and software providers.
> - **2.5:** Explain the purpose of mitigation techniques used to secure the enterprise.
> - **4.3 (supporting):** Explain various activities associated with vulnerability management, including package monitoring.
> - **5.3 (supporting):** Explain the processes associated with third-party risk assessment and management.
>
> An SBOM is a practical enrichment topic that supports dependency identification and vulnerability management; memorizing SBOM formats is not the primary exam goal.
>
> **Prerequisite:** [Day 31 — Vulnerability Sources and Misconfiguration](day-31-vulnerability-sources-and-misconfiguration.md).
>
> **Level:** Beginner–intermediate | **Estimated study time:** 80–105 minutes
>
> **Previous:** [Day 32](day-32-web-application-vulnerabilities.md) | **Next:** [Day 34](day-34-vulnerability-assessment-and-scanning.md)

<a id="3-learning-objectives"></a>

## 1. Learning objectives

By the end of this lesson, you should be able to:

- Distinguish software, firmware, and supplier-related vulnerabilities.
- Explain direct and transitive software dependencies.
- Describe how compromised suppliers and updates can affect downstream customers.
- Explain what hashes, digital signatures, and secure boot can and cannot establish.
- Interpret a sample software bill of materials (SBOM).
- Recommend technical and organizational mitigations for supply-chain risks.

<a id="4-why-this-topic-matters"></a>

## 2. The big idea: Trust extends beyond your organization

Imagine a bakery purchasing flour from a wholesaler. Even excellent hygiene at the bakery cannot compensate completely for contaminated flour arriving from a supplier. The bakery needs controls within its own kitchen and confidence in the materials it receives.

Cybersecurity works similarly. An organization can harden its servers, enforce MFA, and deploy firewalls while still inheriting weaknesses from a trusted vendor. Supply-chain security extends the security boundary beyond the organization's own systems.

The potential impact reaches all three elements of the CIA triad. A compromised component might disclose confidential information, modify records, or interrupt an essential service. A shared component can also create correlated risk: many organizations may become vulnerable at the same time.

**Memory hook:** Inventory tells you what you have; verification tells you what you received; controls limit what it can do.

<a id="5-detailed-instruction"></a>

## 3. Software, firmware, and supply-chain security

### Software components and dependencies

Software developers reuse existing code because implementing every feature independently would be expensive and error-prone. A library is reusable code that provides functionality to another program. A package is a distributable unit of software that may include a library, executable, configuration, and dependency metadata. A dependency is a component another component requires to work.

Consider a fictional document portal:

```text
DocumentPortal 3.0
├── RenderKit 2.1 (direct dependency)
│   └── ImageCore 1.4 (transitive dependency)
└── AuthKit 4.2 (direct dependency)
```

The portal directly requests RenderKit and AuthKit. RenderKit itself requires ImageCore. ImageCore is therefore a **transitive dependency** of the portal. The portal developers may never explicitly import ImageCore, but its code may still be present and executed.

See the [comparison tables](#7-comparison-tables) for a summary.


#### Manifests and lockfiles

A manifest describes the packages a project requires, sometimes using version ranges. A lockfile records resolved dependency versions and may include integrity information. A version range may allow a package manager to select a newer release automatically; a lockfile helps make installations reproducible when the relevant tooling honors it.

Pinning a version reduces unexpected changes but does not make that version safe forever. Teams must monitor pinned versions for newly disclosed weaknesses and update them through a controlled process.

A dependency can be risky without being malicious: it might contain a programming error, lack maintenance, or rely on another vulnerable package. Conversely, a malicious package may deliberately introduce unauthorized behavior even when no conventional coding defect is present.

### Software supply chain and malicious updates

The software supply chain includes the people, systems, and processes that create, package, distribute, and maintain software.

```text
Source code + dependencies
          |
          v
Build and test environment
          |
          v
Packaged release and signing
          |
          v
Distribution repository
          |
          v
Customer installation and updates
```

Each stage is a trust boundary. Source repositories may be modified without authorization. Build systems may be compromised. Release credentials may be stolen. Distribution infrastructure may deliver unauthorized files. Customer systems may accept packages from unapproved sources.

A **malicious update** is an update intentionally containing harmful behavior. This differs from a defective update that accidentally introduces a vulnerability. Both can harm customers, but the investigation and response may differ.

#### Common supply-chain weaknesses

See the [comparison tables](#7-comparison-tables) for a summary.


These are defensive recognition examples. The goal is to identify where trust can fail, not to reproduce attacks against real repositories.

### Firmware, hardware, and legacy platforms

**Firmware** is software closely associated with hardware operation. It may initialize components, control a device, or provide an embedded environment for its functions. Routers, network cameras, storage controllers, and computer motherboards all commonly contain firmware.

Firmware is not synonymous with an operating system. Some firmware provides an embedded operating environment, while other firmware performs narrow hardware-control tasks. It is typically updated through mechanisms provided by the manufacturer.

See the [comparison tables](#7-comparison-tables) for a summary.


#### Why firmware weaknesses matter

Firmware can operate at a highly privileged layer. A flaw may affect device initialization, expose device-management functions, or allow unauthorized modification of persistent device state. Reinstalling an operating system does not necessarily rewrite the relevant firmware.

A network appliance might appear to operate normally while running firmware with a known vulnerability. An inventory must therefore record the exact device model, hardware revision where relevant, firmware version, and support status. Installing a firmware image intended for a different model may render a device unusable.

#### End of life and end of support

A legacy platform is an older system still in use. End-of-life and end-of-support milestones are defined by the manufacturer and do not always occur together. Verify the actual security update policy rather than relying on the label alone.

If a device cannot receive necessary security fixes, replacement is generally preferable. Where immediate replacement is impossible, restrict management access, isolate the device, disable unnecessary features, monitor its activity, and document an approved migration deadline. These compensating controls reduce exposure; they do not repair the firmware defect.

#### Hardware suppliers

Hardware may be counterfeit, altered in transit, or sourced through unauthorized channels. Purchasing from approved suppliers, tracking serial numbers, inspecting deliveries, and checking authenticity using manufacturer guidance help reduce procurement risk. A convincing logo or functional device is not sufficient assurance of provenance.

### Integrity, authenticity, and provenance

A **cryptographic hash** produces a fixed-length digest from data. Comparing a downloaded file's digest with an independently trusted expected digest can detect unexpected changes. If an attacker replaces both the download and the published hash, the comparison alone cannot establish authenticity.

A **digital signature** allows a verifier to check whether data was signed using the corresponding private key and whether the signed data was modified. Trust also depends on the identity associated with the verification key and the security of the signing process.

**Provenance** describes the origin and production history of an artifact, such as the source revision and build process associated with a release. Signed provenance can provide useful evidence about how an artifact was produced, subject to the trustworthiness of the system making the claim.

See the [comparison tables](#7-comparison-tables) for a summary.


#### Secure boot and firmware updates

Secure boot validates boot components against a configured trust policy before permitting them to run. Signed firmware update mechanisms similarly help restrict installation to authorized images. These controls are complementary, but neither proves that all authorized code is safe.

Firmware updates should be obtained from trusted manufacturer channels, validated using supported procedures, tested where feasible, and installed with a recovery plan. A rollback to an older release may restore functionality yet reintroduce a known vulnerability, so recovery decisions require security review.

### Software bills of materials (SBOMs)

An **SBOM**, or software bill of materials, is a structured inventory describing software components and their relationships. Think of an ingredients list for an application: it tells defenders what is present, not whether every ingredient is safe.

An SBOM may include component names, versions, suppliers, unique identifiers, dependency relationships, author information, and timestamps. Coverage varies with tooling and the point at which the SBOM is generated.

Common machine-readable formats include SPDX and CycloneDX. The lesson uses a simplified table rather than requiring knowledge of either schema.

#### How an SBOM supports vulnerability management

1. A vulnerability advisory identifies an affected component and version range.
2. The security team searches available SBOMs and deployment inventories for matching components.
3. The team checks whether affected products are actually deployed and whether vulnerable functionality is reachable.
4. Owners select remediation or compensating controls based on risk.
5. Updated artifacts and inventory records are checked to confirm remediation.

An SBOM can dramatically narrow the investigation when a widely used component becomes vulnerable. It is not itself a vulnerability scan, patch, or security certificate. A stale or incomplete SBOM can omit important components. A version match also requires interpretation: vendor backports and configuration differences can affect applicability.

See the [comparison tables](#7-comparison-tables) for a summary.


### Third-party and supplier risk

A third party is an external organization providing products or services. A managed service provider (MSP), for example, may administer endpoints or networks for multiple customers. Its privileged access makes it important to assess and limit the consequences of a supplier compromise.

Supplier risks are not restricted to software defects. A payroll provider handles sensitive data; a remote support vendor may possess administrative access; a hardware reseller influences the authenticity of equipment. Subcontractors may introduce additional fourth-party dependencies.

#### Supplier lifecycle

See the [comparison tables](#7-comparison-tables) for a summary.


Contractual requirements establish obligations but do not technically enforce them. Combine governance with network restrictions, identity controls, and monitoring.

### Supply-chain mitigation and security operations

Supply-chain security is a continuous process, not a one-time purchasing decision.

- **Inventory:** Track software components, firmware versions, suppliers, and deployment locations.
- **Approved sourcing:** Use authorized suppliers and approved package repositories.
- **Access control:** Restrict publishing credentials, build permissions, and supplier access.
- **Release assurance:** Verify signatures and relevant provenance; review changes and require approvals.
- **Patch management:** Monitor advisories, test updates, deploy them according to risk, and verify successful installation.
- **Segmentation:** Limit the systems and resources that a compromised component can reach.
- **Monitoring:** Investigate unexpected package changes, new outbound connections, unusual privileges, and changes to release metadata.
- **Recovery:** Maintain known-good artifacts, backups, and documented response procedures.

#### Choosing the right response

If a vulnerable library is present, upgrade or replace it when an appropriate fix exists. If a supplier is compromised, simply patching the application may not revoke stolen credentials or remove unauthorized access. Incident responders must consider isolating affected systems, preserving evidence, rotating relevant credentials, and rebuilding from trusted sources as appropriate.

A valid signature on a suspicious release is evidence to investigate, not grounds to dismiss an alert. A new hash after an approved upgrade is expected; a mismatch against the trusted hash for that exact release requires investigation.

<a id="6-real-world-cybersecurity-examples"></a>

## 4. Real-world cybersecurity examples

### Example: inherited vulnerabilities

Suppose ImageCore 1.4 has a known parsing vulnerability. The portal accepts uploaded images and passes them to RenderKit for previews. The vulnerable code may be reachable through this workflow, even though ImageCore is not named in the portal's primary dependency manifest. A developer must determine whether the affected code is present, reachable, and used under vulnerable conditions before selecting remediation.

### Real-world example: SolarWinds Orion

In 2020, the SolarWinds Orion compromise demonstrated that a trusted software release process could distribute malicious code downstream. The case illustrates why installing a legitimately signed update is not equivalent to establishing that every part of the development and release process was secure.

Defenders need layered controls: supplier assessment, release integrity checks, restricted application privileges, network segmentation, and monitoring for unexpected behavior. See the CISA advisory in the references for historical context.

### Scenario: outsourced administration

A retailer grants its support provider a shared administrator account with unrestricted access to every branch. If that provider is compromised, the same credentials may put every branch at risk.

A better design uses individually attributable accounts, MFA, least privilege, time-limited access, segmentation, and centralized logs. The retailer should retain the ability to revoke vendor access quickly. This limits the potential blast radius without assuming the supplier will never experience an incident.

<a id="7-comparison-tables"></a>

## 5. Comparison tables

### Dependency relationships

| Component relationship | Meaning | Security implication |
| --- | --- | --- |
| Direct dependency | Explicitly required by the application | Usually visible in project manifests |
| Transitive dependency | Required through another dependency | May be overlooked during manual reviews |
| Development dependency | Used during building or testing | May compromise build outputs even if absent from production |
| Runtime dependency | Needed during execution | Can affect production behavior directly |

### Supply-chain weaknesses

| Weakness | Explanation | Defensive measure |
| --- | --- | --- |
| Compromised maintainer account | An unauthorized party gains control of publishing privileges | MFA, scoped credentials, release approvals |
| Typosquatting | A package name imitates a legitimate package | Approved repositories and dependency review |
| Dependency confusion | Resolution selects an unintended package from another source | Registry scoping and explicit source policies |
| Compromised build pipeline | Unauthorized code enters the release during compilation or packaging | Isolated builds, restricted access, provenance checks |
| Unmaintained dependency | Security fixes are no longer reliably produced | Replace or maintain the component with appropriate oversight |
| Compromised update channel | Customers receive altered or unauthorized releases | Signature verification and controlled deployment |

### Technology layers

| Layer | Example | Typical maintenance |
| --- | --- | --- |
| Application | Document editor | Application update |
| Operating system | Linux or Windows | OS security updates |
| Firmware | UEFI or router firmware | Device-specific firmware update |
| Hardware | Network adapter or motherboard | Repair or replacement where necessary |

### Integrity and authenticity mechanisms

| Mechanism | What it helps establish | What it does not guarantee |
| --- | --- | --- |
| Trusted hash comparison | Download matches expected bytes | Publisher identity or absence of malicious code |
| Digital signature | Integrity and authenticity relative to a trusted key | Bug-free software or uncompromised development |
| Provenance attestation | Recorded origin and build information | Complete security of every dependency |
| Secure boot | Boot components satisfy configured signature or trust policy | Absence of all firmware and runtime vulnerabilities |

### Inventory and vulnerability artifacts

| Artifact | Primary question answered |
| --- | --- |
| SBOM | What components are in this software? |
| Asset inventory | Where is the software or device deployed? |
| Vulnerability advisory | Which versions or conditions are affected? |
| Lockfile | Which dependency versions were resolved? |
| VEX statement (enrichment) | What is the reported applicability of a vulnerability to a particular product? |

### Supplier lifecycle

| Stage | Security considerations |
| --- | --- |
| Selection | Due diligence, security practices, support lifecycle, independent assessment |
| Contracting | Incident notification, access restrictions, audit rights, security responsibilities |
| Onboarding | Approved accounts, MFA, limited privileges, logging, verified products |
| Operation | Periodic reviews, vulnerability notifications, access recertification |
| Offboarding | Revoke credentials, remove integrations, return or delete data as agreed |

<a id="9-common-misconceptions-and-exam-traps"></a>

## 6. Common misconceptions and exam traps

| Misconception | Correct interpretation |
| --- | --- |
| A direct dependency list shows all components. | Transitive dependencies may also be present. |
| An SBOM removes vulnerabilities. | It supports identification and investigation; remediation is separate. |
| A valid signature means the software is safe. | Authorized signing can occur after a compromised build. |
| Reinstalling an OS always removes firmware compromise. | Firmware is a separate layer and may persist. |
| Unsupported means definitely compromised. | Support status is a risk factor, not incident evidence. |
| Open-source software is inherently unsafe or safe. | Assess maintenance, provenance, vulnerabilities, and usage regardless of licensing model. |
| A supplier contract prevents unauthorized access. | Technical controls must enforce access boundaries. |

<a id="10-key-terminology-and-takeaways"></a>

## 7. Key terminology and takeaways

| Term | Meaning |
| --- | --- |
| Dependency | Component required by another component |
| Transitive dependency | Indirect requirement inherited through another component |
| Malicious update | Release deliberately containing harmful behavior |
| Firmware | Software closely associated with hardware operation |
| SBOM | Structured software component inventory |
| Provenance | Evidence describing an artifact's origin and production history |
| Digital signature | Mechanism supporting integrity and authenticity relative to a trusted key |
| Compensating control | Alternative measure that reduces risk when a preferred control cannot be implemented |
| Third-party risk | Risk arising from external products, services, or relationships |

The central lesson is that trust must be managed across the entire lifecycle. Inventory establishes visibility, verification improves confidence, and layered controls limit the consequences when a trusted component fails.

## 8. Worked case study: A signed release with unexpected behavior

A fictional legal services firm installs a signed portal update. Monitoring subsequently detects unexpected outbound connections. The signature is valid, but the release provenance has not yet been reviewed.

| Step | Analyst reasoning |
| --- | --- |
| Establish facts | Record the installed version, signature result, connection times, and affected systems. |
| Evaluate hypotheses | A compromised build, legitimate new feature, or unrelated activity could explain the observations. |
| Limit exposure | Restrict unnecessary outbound access and escalate through approved response procedures. |
| Verify | Compare release metadata and trusted artifacts, consult the supplier, and correlate logs. |
| Recover | If compromise is confirmed, use trusted artifacts and assess credentials and downstream exposure. |

**Takeaway:** A valid signature establishes integrity and authenticity relative to a trusted key, not the safety of the build.

## 9. Guided notes — Write first, reveal later

Complete the table from memory before revealing the model answers.

| Prompt | Your notes |
| --- | --- |
| Direct versus transitive dependency | |
| Manifest versus lockfile | |
| Malicious versus defective update | |
| Firmware versus OS | |
| Hash versus signature | |
| Secure boot limitation | |
| SBOM versus asset inventory | |
| Supplier access controls | |

<details markdown="1">
<summary>Reveal model guided notes</summary>

- **Direct versus transitive dependency:** Direct is explicitly required; transitive is inherited through another component.
- **Manifest versus lockfile:** A manifest declares requirements; a lockfile records resolved versions.
- **Malicious versus defective update:** Malicious updates intentionally contain harmful behavior; defective updates introduce weaknesses accidentally.
- **Firmware versus OS:** Firmware controls or initializes hardware; reinstalling an OS does not necessarily replace firmware.
- **Hash versus signature:** A trusted hash checks expected bytes; a signature also supports authenticity relative to a trusted key.
- **Secure boot limitation:** It checks boot components against a trust policy, not whether all authorized code is safe.
- **SBOM versus asset inventory:** An SBOM lists components; an asset inventory identifies deployment locations.
- **Supplier access controls:** Individual accounts, MFA, least privilege, time limits, logging and revocation limit exposure.

</details>

<a id="8-technical-demonstrations-and-practical-activity"></a>

## 10. Hands-on lab (20–25 minutes)

### Part A — Inspect local artifacts

The following commands are read-only. Use them on your own computer or an authorized laboratory system. Output depends on the files and tools installed; no sample output is represented as an actual execution result.

#### Inspect installed Python packages

```bash
python3 -m pip list
python3 -m pip show requests
```

The first command lists packages in the selected Python environment. The second displays metadata for the named package if installed, including dependency information where available. A package list is not a complete SBOM of a deployed application, and it does not determine whether packages are vulnerable.

#### Calculate a local file hash

On macOS:

```bash
shasum -a 256 package.zip
```

On Linux:

```bash
sha256sum package.zip
```

On Windows PowerShell:

```powershell
Get-FileHash -Path .\package.zip -Algorithm SHA256
```

Replace `package.zip` with the path to an existing authorized local file. Compare the resulting digest with a trusted reference for that exact release. These commands calculate a hash; they do not automatically verify a signature or determine whether a file contains malware.

Record one package version and dependency, if available, and explain what a hash comparison would establish. If you cannot run the commands, explain their purpose from the examples.

<details markdown="1">
<summary>Reveal Part A interpretation</summary>

Package metadata identifies installed components in the selected environment, not all deployed software. A matching trusted hash supports integrity; it does not prove safety.

</details>

### Part B — Review a sample SBOM



You are a junior security analyst at a fictional legal services company. Its internet-facing document portal uses the following components. All product names, versions, and advisories in this exercise are fictional and are not claims about real vulnerabilities.

#### Sample SBOM and deployment information

| Component | Version | Relationship | Supplier |
| --- | --- | --- | --- |
| CasePortal | 3.0 | Primary application | Example Legal Systems |
| RenderKit | 2.1 | CasePortal direct dependency | Example Rendering |
| ImageCore | 1.4 | RenderKit dependency | Example Imaging |
| AuthKit | 4.2 | CasePortal direct dependency | Example Identity |
| BuildHelper | 1.0 | Build-time dependency | Example Tools |

The portal accepts image uploads. The build environment is separate from production. The security team receives these fictional notices:

- ImageCore versions below 1.6 contain a parsing weakness affecting uploaded images. A fixed version is available.
- BuildHelper 1.0 is no longer supported. No specific vulnerability has been reported in the notice.
- The AuthKit supplier reports a suspected unauthorized release; the affected version range is not yet confirmed.

#### Your tasks

1. Identify the direct and transitive dependencies.
2. Determine which confirmed weakness deserves immediate production investigation and explain why.
3. Recommend a response to the unsupported build dependency.
4. Explain why the AuthKit notice requires investigation even though its affected version range is unknown.
5. Identify two pieces of information missing from the SBOM that would help establish organizational exposure.

<details markdown="1">
<summary>Reveal analysis and recommended actions</summary>

1. RenderKit and AuthKit are direct dependencies; ImageCore is transitive. BuildHelper is identified as a build-time dependency, but the table does not specify whether its relationship is direct or transitive.
2. ImageCore 1.4 is within the affected range and the portal handles the relevant input. Confirm applicability and reachability, identify a compatible release containing the fix, test promptly, and verify the resolved version after deployment. Consider restricting uploads if necessary while remediation proceeds.
3. Evaluate a supported replacement for BuildHelper and secure the build environment. Lack of support is a risk, not proof that a specific vulnerability or compromise exists.
4. Obtain authoritative details from the supplier, identify the exact installed artifact and release history, check relevant indicators, and consider temporarily holding suspect updates. If compromise is indicated, escalate through incident response.
5. Deployment locations, artifact hashes, build dates, unique component identifiers, and runtime configuration can help. An SBOM alone cannot identify every exposed server or determine whether affected functionality is enabled.

</details>

#### Extension: supplier controls

The portal vendor asks for permanent administrative access to resolve the ImageCore issue. Recommend a safer arrangement.

<details markdown="1">
<summary>Reveal recommended approach</summary>

Use approved, individually attributable, time-limited access with MFA and only the permissions required for the task. Restrict reachable systems, log activity, supervise the change as appropriate, and revoke access when work is complete. Require evidence of the updated component and validate remediation independently.

</details>

<a id="11-practice-questions"></a>

## 11. Eight original Security+-style questions

Choose the best answer before revealing each explanation.

### Question 1

An application requires Package A, which requires Package B. A vulnerability is discovered in Package B. How should Package B be classified relative to the application?

A. Firmware  
B. Transitive dependency  
C. Configuration baseline  
D. Digital signature  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Package B is required indirectly through Package A. Its vulnerabilities may affect the application even if it is not explicitly listed in the primary manifest.

</details>

### Question 2

A release passes signature verification, but investigators discover that malicious code entered the build before signing. Which conclusion is correct?

A. The signature necessarily used an invalid key.  
B. The malicious code must have entered after distribution.  
C. Signature verification proves the source code was reviewed.  
D. A valid signature does not guarantee that the signed content is safe.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** The signing process may legitimately sign a compromised build. Signature verification does not establish that upstream development was secure.

</details>

### Question 3

A router has a known firmware vulnerability and no supported update. Replacement requires several weeks. Which action best reduces exposure during that period?

A. Restrict management access, isolate the device, and monitor it.  
B. Reinstall the operating systems on nearby workstations.  
C. Disable logging to reduce processing load.  
D. Assume the device is safe because it still operates normally.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** Compensating controls reduce reachable attack surface while an approved replacement is arranged. They do not remove the underlying vulnerability.

</details>

### Question 4

A widely used library has a newly disclosed vulnerability. Which artifact most directly helps identify software products containing the library?

A. Firewall rule set  
B. Incident notification agreement  
C. Software bill of materials  
D. Password policy  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** An SBOM identifies software components and versions. Deployment records and advisory details are then needed to determine actual exposure.

</details>

### Question 5

A service provider needs temporary access to maintain one server. Which approach best limits supplier-related risk?

A. Provide a shared permanent domain administrator account.  
B. Grant individual, time-limited access with MFA and least privilege.  
C. Rely exclusively on a confidentiality agreement.  
D. Disable monitoring during maintenance.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Narrow privileges, authentication, attribution, and expiration reduce the potential blast radius. Logging should remain enabled.

</details>

### Question 6

An analyst calculates a downloaded package's SHA-256 hash and finds it matches an independently trusted reference for that release. What does this establish?

A. Every dependency is supported.  
B. The developer's build environment was uncompromised.  
C. The package contains no exploitable flaws.  
D. The downloaded bytes match the expected digest, providing integrity assurance.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** A trusted hash comparison helps detect changes to the artifact. It does not establish the absence of malicious code or vulnerabilities.

</details>

### Question 7

A package manager resolves an internal package name to an unintended public repository. Which weakness is most directly described?

A. Typosquatting  
B. Dependency confusion  
C. Firmware rollback  
D. Configuration drift  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Dependency confusion concerns unintended package resolution across sources. Explicit registry scoping and source policies reduce this risk.

</details>

### Question 8

A supplier contract ends. Which action most directly reduces remaining technical access risk?

A. Retain the shared administrator account  
B. Review only the original contract  
C. Revoke supplier credentials and remove unused integrations  
D. Disable audit logs  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Offboarding must remove actual access, not merely end contractual obligations.

</details>

**Score guide:** 7–8: ready to advance; 5–6: review missed concepts; 0–4: revisit the comparison tables and lab.

## 12. Discussion challenge

A vendor offers a signed emergency update but cannot provide a current SBOM or build provenance. Your organization depends on the affected service. How would you balance the risk of delaying the update against the risk of installing it? Explain what evidence, temporary controls, testing, and approval you would require.

<a id="12-study-from-the-treadmill"></a>

## 13. Study from the Treadmill 🏃

Use these free video lessons to reinforce today's concepts while walking, exercising, or taking a break from your desk.

| Video | Instructor | Duration | Focus |
| --- | --- | --- | --- |
| [Hardware Vulnerabilities](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/hardware-vulnerabilities-sy0-701/) | Professor Messer | 6:27 | Firmware and legacy platforms |
| [Supply Chain Vulnerabilities](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/supply-chain-vulnerabilities-sy0-701/) | Professor Messer | 9:12 | Hardware, software, and service-provider risks |
| [Third-party Risk Assessment](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/third-party-risk-assessment-sy0-701/) | Professor Messer | — | Supplier assessment and oversight |

### Treadmill Study Challenge

- Why can a trusted software update contain malicious code?
- How can an indirect dependency affect your application?
- Why might a firmware weakness survive an operating system reinstall?
- What can an SBOM tell you, and what can it not tell you?
- How would you limit the impact of a compromised service provider?

After your workout, explain each answer without consulting your notes.

**Keep moving. Keep learning.**

## 14. One-minute recap

- Direct and transitive dependencies can both introduce weaknesses.
- Trusted suppliers and signed updates can still be compromised.
- Firmware requires device-specific maintenance and lifecycle planning.
- SBOMs support identification; deployment context determines exposure.
- Verify integrity, limit privileges, monitor behavior, and plan recovery.

**Next:** [Day 34 — Vulnerability Assessment and Scanning](day-34-vulnerability-assessment-and-scanning.md).

## References and further reading

- [Professor Messer — SY0-701 video course](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/sy0-701-comptia-security-plus-course/)
- [CISA — SolarWinds and Active Exploitation of SolarWinds Software (AA20-352A)](https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-352a)
- [NTIA — Minimum Elements for an SBOM](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)
- [CycloneDX](https://cyclonedx.org/)
- [SPDX](https://spdx.dev/)
- [NIST SP 800-218 — Secure Software Development Framework](https://doi.org/10.6028/NIST.SP.800-218)
- [NIST SP 800-161 Rev. 1 — Cybersecurity Supply Chain Risk Management Practices](https://doi.org/10.6028/NIST.SP.800-161r1)

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
