# Week 07 — Review and Knowledge Check

**Vulnerability Management | Security+ SY0-701**

Complete the [Week 7 diagnostic](00-diagnostic.w7.md) and Days 31–35 before attempting this assessment. Choose one answer per question without notes. Each correct answer earns one point. Questions 1–20 measure weekly learning; questions 21–25 measure cumulative retention.

## Knowledge check

### Day 31 — Vulnerability Sources and Misconfiguration

**1. A database console is accessible from a partner network rather than only from the administration VLAN. What has increased?**

A. Exposure  
B. Impact  
C. Exploit code  
D. Threat intelligence  

**2. A current web server permits directory listing of confidential files because an administrator enabled it. What is the primary issue?**

A. Missing patch  
B. Security misconfiguration  
C. Firmware defect  
D. Malicious update  

**3. A critical appliance no longer receives vendor security fixes. Which action provides the best durable response?**

A. Extend log retention  
B. Rename the appliance  
C. Replace it with a supported appliance  
D. Increase scan frequency  

**4. A server has an unused file-sharing feature enabled. Which change directly reduces its attack surface?**

A. Add storage capacity  
B. Increase backup retention  
C. Rename the server  
D. Disable the unused feature  

### Day 32 — Web Application Vulnerabilities

**5. A reporting application inserts form values directly into SQL statements. Which development change best addresses the underlying weakness?**

A. Use shorter session lifetimes  
B. Bind values using prepared statements  
C. Add HTTPS  
D. Require longer usernames  

**6. A forum saves unsafe profile text in its database; browsers execute it whenever the profile is viewed. What type of attack is this?**

A. Reflected XSS  
B. CSRF  
C. Stored XSS  
D. Command injection  

**7. A patient portal verifies login but returns any appointment record requested by a signed-in user. What should developers implement?**

A. A longer session token  
B. A new TLS certificate  
C. A stronger password policy  
D. Authorization checks for each requested record  

**8. An attacker impersonates a customer by presenting a copied, valid session cookie. What is this attack?**

A. Session hijacking  
B. SQL injection  
C. CSRF  
D. Reflected XSS  

### Day 33 — Software, Firmware and Supply Chain

**9. A reporting tool imports a chart library that itself requires a compression library. How is the compression library classified relative to the reporting tool?**

A. Direct dependency  
B. Build artifact  
C. Transitive dependency  
D. Firmware  

**10. An administrator verifies a firmware package signature against a trusted vendor key. What can the administrator conclude?**

A. The firmware has no bugs  
B. The device will never fail  
C. The firmware is the newest release  
D. The signed bytes are intact and associated with that key  

**11. A security advisory names an affected component and version. Which document helps a team locate products that include it?**

A. SBOM  
B. Network topology  
C. Access log  
D. Backup schedule  

**12. A payroll provider requests integration access before onboarding. Which step best manages supplier risk?**

A. Approve all requested permissions  
B. Evaluate its controls and specify access limits  
C. Disable audit logging  
D. Accept its sales brochure as evidence  

### Day 34 — Vulnerability Assessment and Scanning

**13. A defender must check local patch inventory on managed endpoints. Which scanning method provides the most relevant visibility?**

A. External port discovery alone  
B. Passive DNS observation  
C. Unauthenticated web scan  
D. Credentialed vulnerability scan  

**14. A report lists a CVSS base score of 8.8. What does that number communicate?**

A. Standardized technical severity  
B. Confirmed exploitation  
C. Exact remediation cost  
D. Probability of an attack this month  

**15. A scanner identifies a missing fix, but verified installation records and vendor documentation establish that the fix is present. How should the alert be classified?**

A. False negative  
B. False positive  
C. Confirmed compromise  
D. Risk transfer  

**16. Two equally severe findings affect an isolated training VM and an internet-facing identity service with documented active exploitation. Which should receive priority?**

A. The training VM because it is easier to fix  
B. Both equally regardless of context  
C. The identity service  
D. Neither until an incident occurs  

### Day 35 — Penetration Testing and Remediation

**17. A staging-only test reveals a link to a production API. What should the tester do before probing that API?**

A. Obtain explicit scope approval  
B. Probe it with a different account  
C. Test it after midnight  
D. Assume the link grants permission  

**18. A vulnerable controller cannot be replaced this month. A firewall restricts access to designated management hosts. What is the firewall restriction?**

A. Permanent removal of the defect  
B. Risk transfer  
C. Compensating control  
D. False negative  

**19. A security patch affects a business-critical database. Which preparation best reduces deployment risk?**

A. Close the ticket immediately  
B. Skip testing to save time  
C. Disable monitoring  
D. Test representative workloads and prepare rollback  

**20. A team fixes a portal permission error. Which result best demonstrates successful remediation?**

A. A ticket marked complete  
B. Retests confirm permitted requests succeed and forbidden requests fail  
C. A new scan of another application  
D. A promise to test next month  

### Cumulative retention — Weeks 1–6

**21. A firewall review finds an unnecessary listening service. Which transport-layer value identifies the service endpoint on a host?**

A. MAC address  
B. Port number  
C. Subnet mask  
D. DNS zone  

**22. A team wants to confirm that a downloaded patch has not changed compared with a trusted digest. Which operation is appropriate?**

A. Calculate and compare its hash  
B. Decrypt its filename  
C. Change its IP address  
D. Compress the file  

**23. A user enters a hostname to reach an internal scanning dashboard. Which service resolves the name to an IP address?**

A. DHCP  
B. NTP  
C. DNS  
D. SMTP  

**24. A company wants to limit network access between a legacy device and ordinary workstations. Which design best supports that goal?**

A. Use the same password everywhere  
B. Increase link speed  
C. Disable logging  
D. Separate network segments and enforce traffic rules  

**25. An email asks an employee to install a supposed urgent security update from an unfamiliar link. Which threat technique is most evident?**

A. Phishing  
B. Packet fragmentation  
C. Load balancing  
D. Hashing  

## Scorecard

| Measure | Score | Percentage |
| --- | --- | --- |
| Diagnostic | ___ / 20 | ___% |
| Weekly knowledge check (1–20) | ___ / 20 | ___% |
| Cumulative retention (21–25) | ___ / 5 | ___% |
| Combined | ___ / 25 | ___% |

## Topic-level comparison

| Topic | Questions | Diagnostic / 4 | Post-check / 4 | Change (percentage points) | Review action |
| --- | --- | --- | --- | --- | --- |
| Day 31 — Vulnerability Sources and Misconfiguration | 1–4 | ___ | ___ | ___ | ___ |
| Day 32 — Web Application Vulnerabilities | 5–8 | ___ | ___ | ___ | ___ |
| Day 33 — Software, Firmware and Supply Chain | 9–12 | ___ | ___ | ___ | ___ |
| Day 34 — Vulnerability Assessment and Scanning | 13–16 | ___ | ___ | ___ | ___ |
| Day 35 — Penetration Testing and Remediation | 17–20 | ___ | ___ | ___ | ___ |

- 4/4: Strong performance — advance, with later spaced review.
- 3/4: Review the missed concept.
- 2/4: Revisit the lesson and practice applications.
- 0–1/4: Repeat the lesson and reassess with new questions.

## Compare your results

Subtract your diagnostic score from your weekly post-check score to obtain the change in correct answers. Multiply that difference by five to obtain the percentage-point change. For each topic, multiply the difference out of four by 25. For example, moving from 2/4 to 3/4 is a gain of 25 percentage points. Record cumulative retention separately; do not include it in weekly improvement. These scores indicate progress, not proof of mastery or exam readiness.

## Answer key and remediation

Open only after completing all 25 questions.

<details markdown="1">
<summary>Question 1 — Reveal answer and explanation</summary>

**Correct answer: A. Exposure**

Reachability increases exposure even if no attack has occurred.

**Learning objective:** Distinguish exposure from vulnerability and risk.

**Review:** [Day 31 — 1. Vulnerabilities, threats, exploits, exposure, and risk](day-31-vulnerability-sources-and-misconfiguration.md#1-vulnerabilities-threats-exploits-exposure-and-risk)

</details>

<details markdown="1">
<summary>Question 2 — Reveal answer and explanation</summary>

**Correct answer: B. Security misconfiguration**

An insecure setting is the weakness; current software can still be misconfigured.

**Learning objective:** Identify configuration weaknesses.

**Review:** [Day 31 — 3. Security misconfiguration](day-31-vulnerability-sources-and-misconfiguration.md#3-security-misconfiguration)

</details>

<details markdown="1">
<summary>Question 3 — Reveal answer and explanation</summary>

**Correct answer: C. Replace it with a supported appliance**

Replacement restores a supported update path; monitoring alone cannot do so.

**Learning objective:** Address unsupported technology.

**Review:** [Day 31 — Unsupported and legacy systems](day-31-vulnerability-sources-and-misconfiguration.md#unsupported-and-legacy-systems)

</details>

<details markdown="1">
<summary>Question 4 — Reveal answer and explanation</summary>

**Correct answer: D. Disable the unused feature**

Removing unnecessary functionality reduces potential entry points.

**Learning objective:** Apply hardening to reduce attack surface.

**Review:** [Day 31 — 5. Attack surface and security hardening](day-31-vulnerability-sources-and-misconfiguration.md#5-attack-surface-and-security-hardening)

</details>

<details markdown="1">
<summary>Question 5 — Reveal answer and explanation</summary>

**Correct answer: B. Bind values using prepared statements**

Prepared statements with bound parameters separate data from SQL instructions.

**Learning objective:** Select SQL injection prevention.

**Review:** [Day 32 — SQL injection](day-32-web-application-vulnerabilities.md#sql-injection)

</details>

<details markdown="1">
<summary>Question 6 — Reveal answer and explanation</summary>

**Correct answer: C. Stored XSS**

The unsafe content persists and reaches subsequent viewers.

**Learning objective:** Recognize stored XSS.

**Review:** [Day 32 — Stored XSS](day-32-web-application-vulnerabilities.md#stored-xss)

</details>

<details markdown="1">
<summary>Question 7 — Reveal answer and explanation</summary>

**Correct answer: D. Authorization checks for each requested record**

The server must verify permission for the specific record, not just identity.

**Learning objective:** Distinguish authentication and object authorization.

**Review:** [Day 32 — Broken access control](day-32-web-application-vulnerabilities.md#broken-access-control)

</details>

<details markdown="1">
<summary>Question 8 — Reveal answer and explanation</summary>

**Correct answer: A. Session hijacking**

Possession of a valid stolen session credential can permit impersonation.

**Learning objective:** Recognize session hijacking.

**Review:** [Day 32 — Session hijacking, fixation, and replay](day-32-web-application-vulnerabilities.md#session-hijacking-fixation-and-replay)

</details>

<details markdown="1">
<summary>Question 9 — Reveal answer and explanation</summary>

**Correct answer: C. Transitive dependency**

The compression library is inherited through another dependency.

**Learning objective:** Classify dependency relationships.

**Review:** [Day 33 — Software components and dependencies](day-33-software-firmware-and-supply-chain.md#software-components-and-dependencies)

</details>

<details markdown="1">
<summary>Question 10 — Reveal answer and explanation</summary>

**Correct answer: D. The signed bytes are intact and associated with that key**

A valid signature supports integrity and authenticity, not safety or freshness.

**Learning objective:** Interpret signature verification.

**Review:** [Day 33 — Integrity, authenticity, and provenance](day-33-software-firmware-and-supply-chain.md#integrity-authenticity-and-provenance)

</details>

<details markdown="1">
<summary>Question 11 — Reveal answer and explanation</summary>

**Correct answer: A. SBOM**

An SBOM lists components and versions; deployment and applicability still need confirmation.

**Learning objective:** Use SBOMs to identify affected products.

**Review:** [Day 33 — Software bills of materials (SBOMs)](day-33-software-firmware-and-supply-chain.md#software-bills-of-materials-sboms)

</details>

<details markdown="1">
<summary>Question 12 — Reveal answer and explanation</summary>

**Correct answer: B. Evaluate its controls and specify access limits**

Due diligence and defined access requirements reduce supplier-related exposure.

**Learning objective:** Apply supplier risk assessment.

**Review:** [Day 33 — Third-party and supplier risk](day-33-software-firmware-and-supply-chain.md#third-party-and-supplier-risk)

</details>

<details markdown="1">
<summary>Question 13 — Reveal answer and explanation</summary>

**Correct answer: D. Credentialed vulnerability scan**

Authorized local access allows inspection of installed patches and packages.

**Learning objective:** Choose credentialed scanning.

**Review:** [Day 34 — Credentialed and non-credentialed scanning](day-34-vulnerability-assessment-and-scanning.md#credentialed-and-non-credentialed-scanning)

</details>

<details markdown="1">
<summary>Question 14 — Reveal answer and explanation</summary>

**Correct answer: A. Standardized technical severity**

CVSS describes technical severity rather than organization-specific risk or probability.

**Learning objective:** Interpret CVSS.

**Review:** [Day 34 — CVSS and technical severity](day-34-vulnerability-assessment-and-scanning.md#cvss-and-technical-severity)

</details>

<details markdown="1">
<summary>Question 15 — Reveal answer and explanation</summary>

**Correct answer: B. False positive**

The reported vulnerability is absent according to validated evidence.

**Learning objective:** Recognize false positives.

**Review:** [Day 34 — False positives and false negatives](day-34-vulnerability-assessment-and-scanning.md#false-positives-and-false-negatives)

</details>

<details markdown="1">
<summary>Question 16 — Reveal answer and explanation</summary>

**Correct answer: C. The identity service**

Exposure, business importance and exploitation evidence increase urgency.

**Learning objective:** Prioritize using risk context.

**Review:** [Day 34 — Risk-based prioritization](day-34-vulnerability-assessment-and-scanning.md#risk-based-prioritization)

</details>

<details markdown="1">
<summary>Question 17 — Reveal answer and explanation</summary>

**Correct answer: A. Obtain explicit scope approval**

Discovery does not expand written authorization.

**Learning objective:** Respect assessment scope.

**Review:** [Day 35 — Authorization, scope, and rules of engagement](day-35-penetration-testing-and-remediation.md#authorization-scope-and-rules-of-engagement)

</details>

<details markdown="1">
<summary>Question 18 — Reveal answer and explanation</summary>

**Correct answer: C. Compensating control**

The restriction reduces exposure while the underlying weakness remains.

**Learning objective:** Distinguish mitigation from remediation.

**Review:** [Day 35 — Remediation and mitigation](day-35-penetration-testing-and-remediation.md#remediation-and-mitigation)

</details>

<details markdown="1">
<summary>Question 19 — Reveal answer and explanation</summary>

**Correct answer: D. Test representative workloads and prepare rollback**

Testing and rollback planning reduce the operational risk of change.

**Learning objective:** Plan safe patch deployment.

**Review:** [Day 35 — Patch management and change control](day-35-penetration-testing-and-remediation.md#patch-management-and-change-control)

</details>

<details markdown="1">
<summary>Question 20 — Reveal answer and explanation</summary>

**Correct answer: B. Retests confirm permitted requests succeed and forbidden requests fail**

Positive and negative retests demonstrate intended access behavior.

**Learning objective:** Validate remediation before closure.

**Review:** [Day 35 — Validation and closure](day-35-penetration-testing-and-remediation.md#validation-and-closure)

</details>

<details markdown="1">
<summary>Question 21 — Reveal answer and explanation</summary>

**Correct answer: B. Port number**

Ports distinguish transport endpoints on a host.

**Learning objective:** Recall ports and protocols.

**Review:** [Day 18 — Ports and Protocols](../week-04/day-18-ports-and-protocols.md)

</details>

<details markdown="1">
<summary>Question 22 — Reveal answer and explanation</summary>

**Correct answer: A. Calculate and compare its hash**

Comparing a cryptographic hash with a trusted reference checks integrity.

**Learning objective:** Recall hashing versus encryption.

**Review:** [Day 6 — Hashing vs Encryption](../week-02/day-06-hashing-vs-encryption.md)

</details>

<details markdown="1">
<summary>Question 23 — Reveal answer and explanation</summary>

**Correct answer: C. DNS**

DNS resolves names to records such as IP addresses.

**Learning objective:** Recall DNS.

**Review:** [Day 19 — DNS, DHCP and Network Services](../week-04/day-19-dns-dhcp-and-network-services.md)

</details>

<details markdown="1">
<summary>Question 24 — Reveal answer and explanation</summary>

**Correct answer: D. Separate network segments and enforce traffic rules**

Segmentation with enforced traffic restrictions limits reachability.

**Learning objective:** Recall network security and segmentation.

**Review:** Week 05 — Network Security (segmentation)

</details>

<details markdown="1">
<summary>Question 25 — Reveal answer and explanation</summary>

**Correct answer: A. Phishing**

The message attempts to persuade the recipient to take an unsafe action.

**Learning objective:** Recall social engineering and phishing.

**Review:** [Day 28 — Social Engineering and Phishing](../week-06/day-28-social-engineering-and-phishing.md)

</details>

## Next steps

Review missed concepts using the lesson references, then explain each correction in your own words. Revisit weak cumulative topics during spaced review.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
