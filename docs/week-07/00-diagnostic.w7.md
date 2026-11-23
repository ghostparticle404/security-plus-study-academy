# Week 07 — Diagnostic Assessment

**Vulnerability Management | Security+ SY0-701**

## Before you begin

**Why study something you already know?**

Complete these 20 questions before studying Days 31–35. Choose one answer per question without consulting the lessons or answer key. Award one point per correct answer; there is no penalty for guessing. Record your answers and retain your scores for comparison with the weekly knowledge check.

## Diagnostic questions

### Day 31 — Vulnerability Sources and Misconfiguration

**1. A vulnerable administrative service is reachable from the internet. Which term describes that reachability?**

A. Exploit  
B. Exposure  
C. Threat actor  
D. Impact  

**2. A fully patched storage service allows anonymous users to read private records because of its access policy. What is the primary weakness?**

A. Unsupported firmware  
B. Missing software patch  
C. Race condition  
D. Security misconfiguration  

**3. A vendor has ended security support for an operating system used in production. Which action best addresses the underlying long-term weakness?**

A. Migrate to a supported platform  
B. Increase log retention  
C. Change the hostname  
D. Install a new monitoring dashboard  

**4. An inventory reveals an unused network service running on every workstation. Which action most directly reduces attack surface?**

A. Increase password length  
B. Enable additional audit events  
C. Disable the unnecessary service  
D. Increase backup frequency  

### Day 32 — Web Application Vulnerabilities

**5. An application builds database queries by concatenating user-supplied search text. Which control most directly separates query instructions from data?**

A. TLS encryption  
B. Cookie expiration  
C. Parameterized queries  
D. Network segmentation  

**6. A malicious script saved in a product review executes in the browsers of later visitors. Which weakness does this illustrate?**

A. Stored XSS  
B. Reflected XSS  
C. SQL injection  
D. Session fixation  

**7. A signed-in employee changes a document identifier in a request and receives another department’s restricted file. Which control is missing?**

A. Password complexity  
B. Transport encryption  
C. Session expiration  
D. Server-side object authorization  

**8. A service accepts a stolen, still-valid session token from another browser. Which weakness is illustrated by the attacker using that token to impersonate the user?**

A. Command injection  
B. Session hijacking  
C. Stored XSS  
D. SQL injection  

### Day 33 — Software, Firmware and Supply Chain

**9. Application A imports library B, which imports library C. What is C in relation to A?**

A. Direct dependency  
B. Firmware component  
C. Transitive dependency  
D. Build signature  

**10. A supplier distributes an update with a digital signature. What does successful verification using a trusted supplier key establish?**

A. The update contains no vulnerabilities  
B. The update will work on every device  
C. The supplier has passed every audit  
D. The signed content is intact and associated with the signing key  

**11. A new vulnerability is announced in a library. Which artifact helps identify products containing that library?**

A. Software bill of materials  
B. Firewall rule table  
C. Incident timeline  
D. Network packet capture  

**12. A company plans to grant a maintenance vendor access to production systems. Which action best addresses supplier risk before granting access?**

A. Rely on the vendor’s marketing claims  
B. Assess the vendor’s security controls and define access requirements  
C. Disable internal logging  
D. Grant unrestricted access to simplify support  

### Day 34 — Vulnerability Assessment and Scanning

**13. A team needs to inspect installed package versions and local security settings on authorized servers. Which approach is most suitable?**

A. Passive traffic observation only  
B. External non-credentialed scanning only  
C. Public DNS lookup  
D. Credentialed vulnerability scanning  

**14. A scanner reports a CVSS base score of 9.8. What does this score represent?**

A. Standardized technical severity  
B. A 98% probability of exploitation  
C. Confirmed compromise of the asset  
D. The exact financial loss  

**15. A scanner flags an outdated package, but authoritative vendor and installation evidence confirms the relevant fix is already present. How should the finding be classified?**

A. False negative  
B. Accepted risk  
C. False positive  
D. Confirmed exploitation  

**16. Two findings have similar technical severity. One affects a public payment service and is known to be exploited in the wild; the other affects an isolated test system. What is the best initial prioritization?**

A. Treat both as identical because scores match  
B. Prioritize the public payment service  
C. Prioritize the test system because it is easier to patch  
D. Ignore both until compromise is confirmed  

### Day 35 — Penetration Testing and Remediation

**17. During an authorized penetration test, a tester discovers a server outside the agreed scope. What should the tester do?**

A. Test it because it belongs to the same company  
B. Test it only after business hours  
C. Seek explicit authorization before testing it  
D. Ask another tester to test it without approval  

**18. A legacy application cannot be patched immediately. The team restricts network access to it while preparing a replacement. What does this restriction represent?**

A. A compensating control  
B. A verified permanent fix  
C. Risk transfer  
D. A false positive  

**19. A critical update may disrupt a production service. Which preparation best supports safe deployment?**

A. Disable all monitoring  
B. Close the finding before installation  
C. Assume the update cannot fail  
D. Test in staging and prepare a rollback plan  

**20. Developers report that an authorization defect has been corrected. Which evidence best supports closure?**

A. A developer’s email alone  
B. Retests showing authorized access works and prohibited access is denied  
C. A scan of an unrelated server  
D. A scheduled future patch  

## Topic-level scorecard

| Lesson | Questions | Correct / 4 | Percentage | Recommended action |
| --- | --- | --- | --- | --- |
| Day 31 — Vulnerability Sources and Misconfiguration | 1–4 | ___ / 4 | ___% | ___ |
| Day 32 — Web Application Vulnerabilities | 5–8 | ___ / 4 | ___% | ___ |
| Day 33 — Software, Firmware and Supply Chain | 9–12 | ___ / 4 | ___% | ___ |
| Day 34 — Vulnerability Assessment and Scanning | 13–16 | ___ / 4 | ___% | ___ |
| Day 35 — Penetration Testing and Remediation | 17–20 | ___ / 4 | ___% | ___ |


**Total:** ___ / 20  
**Percentage:** ___% (correct answers × 5)

### Interpreting each topic score

- **4/4 (100%):** Strong baseline — review briefly or proceed to the next topic.
- **3/4 (75%):** Mostly understood — review missed concepts.
- **2/4 (50%):** Partial understanding — study the lesson.
- **0–1/4 (0–25%):** Priority study — complete the lesson carefully.

Four questions per topic provide a screening tool, not proof of mastery.

## Answer key

Open the explanations only after completing all questions.

<details markdown="1">
<summary>Question 1 — Reveal answer and explanation</summary>

**Correct answer: B. Exposure**

Exposure describes accessibility to potential harm; an exploit takes advantage of a weakness.

**Review:** [Day 31 — 1. Vulnerabilities, threats, exploits, exposure, and risk](day-31-vulnerability-sources-and-misconfiguration.md#1-vulnerabilities-threats-exploits-exposure-and-risk)

</details>

<details markdown="1">
<summary>Question 2 — Reveal answer and explanation</summary>

**Correct answer: D. Security misconfiguration**

The access policy is insecure even though the software is patched.

**Review:** [Day 31 — 3. Security misconfiguration](day-31-vulnerability-sources-and-misconfiguration.md#3-security-misconfiguration)

</details>

<details markdown="1">
<summary>Question 3 — Reveal answer and explanation</summary>

**Correct answer: A. Migrate to a supported platform**

Migration addresses the lack of ongoing vendor security fixes; monitoring does not restore support.

**Review:** [Day 31 — Unsupported and legacy systems](day-31-vulnerability-sources-and-misconfiguration.md#unsupported-and-legacy-systems)

</details>

<details markdown="1">
<summary>Question 4 — Reveal answer and explanation</summary>

**Correct answer: C. Disable the unnecessary service**

Removing unnecessary services reduces the number of reachable or usable entry points.

**Review:** [Day 31 — 5. Attack surface and security hardening](day-31-vulnerability-sources-and-misconfiguration.md#5-attack-surface-and-security-hardening)

</details>

<details markdown="1">
<summary>Question 5 — Reveal answer and explanation</summary>

**Correct answer: C. Parameterized queries**

Parameterized queries bind input as data rather than incorporating it into SQL syntax.

**Review:** [Day 32 — SQL injection](day-32-web-application-vulnerabilities.md#sql-injection)

</details>

<details markdown="1">
<summary>Question 6 — Reveal answer and explanation</summary>

**Correct answer: A. Stored XSS**

Stored XSS persists in application data and is delivered to later visitors.

**Review:** [Day 32 — Stored XSS](day-32-web-application-vulnerabilities.md#stored-xss)

</details>

<details markdown="1">
<summary>Question 7 — Reveal answer and explanation</summary>

**Correct answer: D. Server-side object authorization**

Authentication establishes identity; the server must separately check permission for each requested object.

**Review:** [Day 32 — Broken access control](day-32-web-application-vulnerabilities.md#broken-access-control)

</details>

<details markdown="1">
<summary>Question 8 — Reveal answer and explanation</summary>

**Correct answer: B. Session hijacking**

A stolen valid session token can allow impersonation without entering the original password.

**Review:** [Day 32 — Session hijacking, fixation, and replay](day-32-web-application-vulnerabilities.md#session-hijacking-fixation-and-replay)

</details>

<details markdown="1">
<summary>Question 9 — Reveal answer and explanation</summary>

**Correct answer: C. Transitive dependency**

C is inherited through B rather than imported directly by A.

**Review:** [Day 33 — Software components and dependencies](day-33-software-firmware-and-supply-chain.md#software-components-and-dependencies)

</details>

<details markdown="1">
<summary>Question 10 — Reveal answer and explanation</summary>

**Correct answer: D. The signed content is intact and associated with the signing key**

Signature verification supports integrity and authenticity, not proof that software is safe or compatible.

**Review:** [Day 33 — Integrity, authenticity, and provenance](day-33-software-firmware-and-supply-chain.md#integrity-authenticity-and-provenance)

</details>

<details markdown="1">
<summary>Question 11 — Reveal answer and explanation</summary>

**Correct answer: A. Software bill of materials**

An SBOM inventories software components and can help identify affected products; deployment and applicability still require verification.

**Review:** [Day 33 — Software bills of materials (SBOMs)](day-33-software-firmware-and-supply-chain.md#software-bills-of-materials-sboms)

</details>

<details markdown="1">
<summary>Question 12 — Reveal answer and explanation</summary>

**Correct answer: B. Assess the vendor’s security controls and define access requirements**

Supplier assessment and defined access requirements help manage third-party exposure before onboarding.

**Review:** [Day 33 — Third-party and supplier risk](day-33-software-firmware-and-supply-chain.md#third-party-and-supplier-risk)

</details>

<details markdown="1">
<summary>Question 13 — Reveal answer and explanation</summary>

**Correct answer: D. Credentialed vulnerability scanning**

Credentialed scanning can inspect local information that may not be visible remotely.

**Review:** [Day 34 — Credentialed and non-credentialed scanning](day-34-vulnerability-assessment-and-scanning.md#credentialed-and-non-credentialed-scanning)

</details>

<details markdown="1">
<summary>Question 14 — Reveal answer and explanation</summary>

**Correct answer: A. Standardized technical severity**

CVSS measures technical severity; it does not establish probability, compromise, or financial loss.

**Review:** [Day 34 — CVSS and technical severity](day-34-vulnerability-assessment-and-scanning.md#cvss-and-technical-severity)

</details>

<details markdown="1">
<summary>Question 15 — Reveal answer and explanation</summary>

**Correct answer: C. False positive**

The scanner reported a weakness that evidence establishes is absent.

**Review:** [Day 34 — False positives and false negatives](day-34-vulnerability-assessment-and-scanning.md#false-positives-and-false-negatives)

</details>

<details markdown="1">
<summary>Question 16 — Reveal answer and explanation</summary>

**Correct answer: B. Prioritize the public payment service**

Exposure, exploitation evidence and business criticality inform prioritization beyond severity.

**Review:** [Day 34 — Risk-based prioritization](day-34-vulnerability-assessment-and-scanning.md#risk-based-prioritization)

</details>

<details markdown="1">
<summary>Question 17 — Reveal answer and explanation</summary>

**Correct answer: C. Seek explicit authorization before testing it**

Testing must remain within authorized scope unless an approved change extends it.

**Review:** [Day 35 — Authorization, scope, and rules of engagement](day-35-penetration-testing-and-remediation.md#authorization-scope-and-rules-of-engagement)

</details>

<details markdown="1">
<summary>Question 18 — Reveal answer and explanation</summary>

**Correct answer: A. A compensating control**

The restriction reduces exposure without necessarily removing the underlying vulnerability.

**Review:** [Day 35 — Remediation and mitigation](day-35-penetration-testing-and-remediation.md#remediation-and-mitigation)

</details>

<details markdown="1">
<summary>Question 19 — Reveal answer and explanation</summary>

**Correct answer: D. Test in staging and prepare a rollback plan**

Representative testing and rollback planning reduce operational risk during patch deployment.

**Review:** [Day 35 — Patch management and change control](day-35-penetration-testing-and-remediation.md#patch-management-and-change-control)

</details>

<details markdown="1">
<summary>Question 20 — Reveal answer and explanation</summary>

**Correct answer: B. Retests showing authorized access works and prohibited access is denied**

Positive and negative functional tests verify the corrected behavior while preserving legitimate access.

**Review:** [Day 35 — Validation and closure](day-35-penetration-testing-and-remediation.md#validation-and-closure)

</details>

## Plan your study

Start with topics scoring 0–2, then review missed concepts in topics scoring 3. For topics scoring 4, briefly review the learning objectives and practical activities before moving on. Complete the weekly knowledge check even if your baseline is strong. Compare the first 20 post-check questions with this diagnostic; report cumulative retention separately.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
