# Day 30: Indicators and Threat Intelligence

**Week 06 — Threats, Vulnerabilities, and Mitigations**  
**Security+ Study Academy**  
**Prerequisites:** Days 26–29  
**Estimated study time:** 75–100 minutes

## Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish indicators of compromise (IoCs) from indicators of attack (IoAs).
2. Interpret common host, network, identity, and cloud indicators without assuming they prove malicious activity.
3. Compare threat intelligence sources and evaluate their reliability, timeliness, and relevance.
4. Explain the intelligence lifecycle, intelligence sharing, and the limits of attribution.
5. Map indicators to proportionate investigation and response steps.
6. Create a short, evidence-based threat intelligence assessment.

## 1. From threat actors to observable evidence

In Days 26–29, you examined who may attack an organization, which surfaces they can reach, how they manipulate people, and how malware behaves. Investigators connect those concepts to **observable evidence**: events, artifacts, and patterns that may indicate hostile activity.

An indicator is a **lead**, not automatically a verdict. For example, an unfamiliar IP address could be an attacker, a legitimate cloud service, or a remote employee. The analyst must test the context.

### IoCs versus IoAs

| Concept | Primary question | Examples | Main limitation |
| --- | --- | --- | --- |
| **Indicator of compromise (IoC)** | What artifact suggests a system *may have been* compromised? | Known-malicious file hash, suspicious domain, ransom note, unauthorized account | Can be changed or reused; may be stale or benign in context |
| **Indicator of attack (IoA)** | What behavior suggests an attack *may be underway*? | Repeated failed logins followed by success, unusual privilege changes, mass file renames | Behaviors can have legitimate explanations; requires correlation |

**Important nuance:** The categories overlap. A suspicious process command line might be both a recorded artifact and evidence of a malicious behavior. IoC and IoA are analytic lenses, not mutually exclusive file types.

<details markdown="1">
<summary>Check your understanding: IoC or IoA?</summary>

A threat feed reports the SHA-256 hash of a confirmed malicious executable. That hash is commonly used as an **IoC**. A detection rule alerts on a sequence of login attempts from many accounts followed by a successful login and a new privileged role assignment. That sequence is an **IoA**. Neither finding alone replaces investigation.

</details>

## 2. Common indicator categories

### Network indicators

- IP addresses contacting a suspicious endpoint.
- Domain names, DNS queries, and URLs associated with a campaign.
- Unusual outbound connections, repeated beacons, or unexpected data transfer volumes.
- Network connections from devices that do not normally communicate.

**Caution:** Shared hosting, content delivery networks, NAT, and dynamically assigned IPs make an address alone a weak basis for attribution or blocking.

### Host and file indicators

- File hashes (often SHA-256) and unexpected executables.
- Unusual parent-child process relationships.
- Newly created services, scheduled tasks, startup entries, or other persistence mechanisms.
- Unexpected security-tool changes or widespread file modifications.

A cryptographic hash identifies file contents with high precision, but a different build or modified file produces a different hash. Hash matching does not detect every variant.

### Identity and cloud indicators

- Impossible or unusual travel patterns, accounting for VPNs and proxy locations.
- MFA fatigue attempts, unusual sign-ins, or suspicious session-token use.
- Newly granted administrator roles or OAuth permissions.
- Unexpected mailbox forwarding rules or mass cloud-storage downloads.

### Email and human-reported indicators

- Display-name impersonation, suspicious reply-to addresses, or lookalike domains.
- Unexpected attachment types, urgent payment requests, or requests to bypass normal verification.
- User reports of an unfamiliar sign-in notification or a sudden ransom message.

**Remember:** A suspicious message is evidence to preserve, not a link to click or an attachment to execute.

## 3. The Pyramid of Pain

The **Pyramid of Pain** illustrates how difficult different types of detection indicators can be for an adversary to change.

| Indicator level | Example | General adversary cost to change |
| --- | --- | --- |
| Hash values | SHA-256 of a malware sample | Very low |
| IP addresses | Command-and-control server IP | Low |
| Domain names | Malicious domain | Moderate |
| Network/host artifacts | Distinctive URI path or registry artifact | Moderate |
| Tools | Particular attacker toolkit | High |
| Tactics, techniques, and procedures (TTPs) | Consistent method of credential access and lateral movement | Very high |

The pyramid is a **general model**, not a universal ranking. Operational constraints can make a specific domain hard to replace or a tool easy to swap. Behavioral detections are often more durable than a single hash or address.

## 4. Threat intelligence versus raw threat data

**Threat data** consists of observations: logs, suspicious hashes, domain lists, and reports. **Threat intelligence** adds analysis and context so someone can make a decision.

A useful intelligence product answers: **What happened? How confident are we? Why does it matter to us? What should we do next?**

### Intelligence levels

| Level | Audience | Typical output |
| --- | --- | --- |
| Strategic | Executives, risk owners | Threat trends, business exposure, investment priorities |
| Operational | Incident responders, security managers | Campaign objectives, actor activity, likely targets |
| Tactical | Detection engineers, SOC analysts | TTPs, detection logic, defensive playbooks |
| Technical | SOC tooling, security operations | IPs, domains, hashes, URLs, signatures |

A domain list may help with an immediate hunt, but a briefing that explains why a campaign targets law firms may better guide strategic decisions.

### Intelligence sources

- **Internal:** SIEM alerts, EDR telemetry, authentication logs, help-desk reports, asset inventories, incident records.
- **Open source (OSINT):** Public advisories, vendor research, security blogs, public repositories, and community reporting.
- **Commercial:** Paid feeds, curated research, and managed intelligence services.
- **Government and sector sharing:** National cyber agencies, information sharing and analysis centers (ISACs), and trusted peer communities.
- **Human sources:** Employee reports, partner notifications, and vetted investigator findings.

No source is inherently correct. Ask whether it is relevant to your environment, current, independently corroborated, and transparent about uncertainty.

### Evaluating source quality

For each indicator, record:

1. **Provenance:** Where did the observation originate?
2. **Time:** When was it first and last seen?
3. **Confidence:** How strong is the evidence, and what remains uncertain?
4. **Context:** What behavior, campaign, or asset is associated with it?
5. **Relevance:** Do we use the affected technology or have the exposed asset?
6. **Actionability:** Can we safely investigate or detect it without excessive false positives?

<details markdown="1">
<summary>Reveal example: a low-confidence feed hit</summary>

A public feed flags `203.0.113.77` as suspicious, but provides no timestamps or supporting behavior. This is **low-confidence intelligence**. It may justify a log search, not an automatic claim that every connection to that address is malicious. (The address is from a documentation-only range.)

</details>

## 5. The threat intelligence lifecycle

The cycle is iterative rather than strictly linear.

1. **Direction:** Define the intelligence question. Example: “Are we being targeted by credential phishing?”
2. **Collection:** Gather relevant logs, user reports, advisories, and other permitted evidence.
3. **Processing:** Normalize timestamps, deduplicate events, and parse artifacts.
4. **Analysis:** Correlate findings, compare hypotheses, assess confidence, and identify gaps.
5. **Dissemination:** Share a concise report with the people who can act on it.
6. **Feedback:** Check whether the intelligence was useful and refine collection or detection.

**Example:** A law firm's security team collects mailbox reports, sign-in events, and endpoint alerts. Analysts correlate a suspicious login with a newly created forwarding rule. The team notifies incident responders, preserves evidence, and reviews affected accounts.

## 6. Threat intelligence sharing and frameworks

### MITRE ATT&CK

**MITRE ATT&CK** is a knowledge base of observed adversary tactics and techniques. It helps describe *behavior*, such as phishing for initial access or creating accounts for persistence. ATT&CK technique labels are not proof that a named threat actor performed an action.

### STIX and TAXII

- **STIX (Structured Threat Information Expression):** A structured format for describing threat information and relationships.
- **TAXII (Trusted Automated Exchange of Intelligence Information):** A protocol for exchanging cyber threat intelligence, often including STIX content.

### TLP and safe sharing

The **Traffic Light Protocol (TLP)** communicates sharing boundaries. TLP 2.0 uses **TLP:RED**, **TLP:AMBER**, **TLP:GREEN**, and **TLP:CLEAR**; **TLP:AMBER+STRICT** is a more restrictive variant. Follow the sender's stated restrictions and your organization's policies.

Before sharing, remove unnecessary personal data, credentials, confidential client material, and other sensitive details. A useful indicator can often be shared without exposing the affected person's identity.

## 7. Map indicators to investigation steps

Use the workflow **observe → validate → scope → preserve → respond → document**.

| Observation | First validation | Next investigation | Possible response (after confirmation and authorization) |
| --- | --- | --- | --- |
| Suspicious domain in DNS logs | Check timestamp, device, source reliability | Review DNS, proxy, browser, and EDR events | Block domain if justified; assess affected host |
| Many failed sign-ins followed by success | Check identity, source, time, and known activity | Review MFA, session, privilege, and device events | Revoke sessions, reset credentials, or restrict account |
| New mailbox forwarding rule | Confirm owner and business purpose | Examine audit logs and mailbox access | Remove unauthorized rule; secure account |
| Mass file renaming | Determine whether authorized software is responsible | Inspect process tree, file events, and backup status | Isolate affected endpoint under response procedures |
| Known malicious file hash | Verify hash, source, and execution status | Search other hosts and related process events | Quarantine or isolate according to policy |

**Do not** execute a suspicious sample, browse a malicious URL, or upload confidential files to a public scanning service merely to verify an indicator. Use approved tools and procedures.

### Evidence and attribution

Keep the **observation**, **interpretation**, and **confidence** separate:

- **Observation:** “A new forwarding rule was created at 09:14 UTC.”
- **Interpretation:** “This may indicate unauthorized mailbox access.”
- **Confidence:** “Moderate, pending user confirmation and sign-in-log review.”

An IP address, malware family, or technique rarely establishes the identity of the actor. Attribution requires multiple independent lines of evidence and careful consideration of alternatives.

## 8. Practical lab: investigate a simulated law firm incident

**Scenario:** A 20-person law firm uses cloud email, shared document storage, managed laptops, and MFA. On Tuesday morning, the help desk receives a report that a staff member clicked a link in an unexpected “document review” email.

All names, domains, addresses, and events below are **fictional training data**. The `.example` domains and documentation IP ranges are not live attack targets.

### Evidence packet

| ID | Time (UTC) | Source | Observation |
| --- | --- | --- | --- |
| E1 | 08:42 | User report | Staff member clicked `https://case-review.example/login` after receiving an urgent document-sharing email |
| E2 | 08:45 | Identity log | Five failed sign-ins followed by a successful sign-in from `198.51.100.24` |
| E3 | 08:47 | Identity log | New session created for the affected account; device not recognized |
| E4 | 08:51 | Mail audit | Inbox rule created to forward selected messages to `review-archive@example.net` |
| E5 | 09:02 | Cloud audit | Unusual download of 140 client documents |
| E6 | 09:08 | Endpoint log | No malware alert on the employee's laptop |
| E7 | 09:15 | Help desk | Employee denies creating the forwarding rule |
| E8 | 09:20 | Threat feed | `198.51.100.24` listed as suspicious, without a cited first-seen date |

### Your tasks

1. Identify **at least four indicators**, labeling each as an IoC, IoA, or both where justified.
2. Rank the **three most urgent investigative questions**.
3. Write a plausible **attack-path hypothesis** without treating it as established fact.
4. Recommend **three immediate actions** that preserve evidence and reduce risk.
5. Write a **five-sentence analyst summary** separating observations from conclusions.
6. Assign an overall confidence level (**low, moderate, or high**) and explain it.

<details markdown="1">
<summary>Reveal model investigation and reasoning</summary>

**Indicators:** E1 is a possible phishing-related artifact and entry point; E2–E3 form a suspicious authentication behavior (IoA); E4 is an account artifact and potential persistence/exfiltration behavior (both); E5 is possible data-exfiltration behavior (IoA). E8 is a weakly contextualized technical indicator. E6 does **not** rule out account compromise.

**Urgent questions:** Was the successful sign-in authorized? Which sessions and permissions were used? Which documents were actually accessed or exported, and by whom?

**Hypothesis:** The user may have encountered a credential-harvesting page, after which an unauthorized party accessed the mailbox, established forwarding, and downloaded files. Alternative explanations remain possible until identity and cloud audit evidence are correlated.

**Actions:** Preserve relevant logs and the original email; use approved procedures to revoke suspicious sessions and secure the account; assess the forwarding rule and downloaded data while following the incident response plan and escalation requirements.

**Sample summary:** “A staff member reported clicking an unexpected document-review link at 08:42 UTC. Authentication records show failed attempts followed by a successful login and an unfamiliar device session. A new forwarding rule and an unusual document download occurred shortly afterward. These correlated events support a working hypothesis of account compromise and possible data exposure, although the session owner and download scope remain under investigation. Confidence is moderate pending session validation, audit review, and confirmation of affected files.”

</details>

## 9. Security+ knowledge check

Choose the **best** answer for each question before opening its explanation.

### Question 1

Which finding is the clearest example of an **IoA**?

A. The SHA-256 hash of a known malware sample  
B. A sequence of password spraying followed by successful privileged access  
C. A domain listed in a year-old feed  
D. A file name included in an incident report

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** A correlated sequence of suspicious actions is a behavioral indicator of attack. The other choices are primarily artifacts or contextual data.

</details>

### Question 2

Why might a detection based on attacker TTPs be more durable than a detection based only on file hashes?

A. TTPs always identify the attacker by name  
B. File hashes never change  
C. Attackers can often modify a file more easily than their established operating methods  
D. Behavioral detections have no false positives

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** The Pyramid of Pain illustrates that changing established behaviors can be more costly than changing low-level artifacts. Behavioral detections still require tuning.

</details>

### Question 3

An analyst receives a suspicious IP address from a feed without timestamps or context. What should the analyst do **first**?

A. Attribute the activity to a nation-state group  
B. Block every connection associated with the IP across all systems  
C. Verify the source and correlate the indicator with internal telemetry  
D. Announce a confirmed breach

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** Validate freshness, provenance, and relevance before taking disruptive action.

</details>

### Question 4

Which intelligence product is most useful for an executive deciding whether to fund additional identity security controls?

A. Strategic intelligence describing business risk and trends  
B. A raw list of 500 IP addresses  
C. A single packet capture without analysis  
D. An isolated SHA-256 hash

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: A.** Strategic intelligence connects threats to organizational risk and investment decisions.

</details>

### Question 5

What is the primary role of **STIX**?

A. Encrypt all SIEM logs  
B. Represent threat intelligence in a structured format  
C. Replace endpoint detection software  
D. Assign threat actor identities automatically

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** STIX provides structured representations of threat information; TAXII supports its exchange.

</details>

### Question 6

An unexpected mailbox forwarding rule appears after a suspicious sign-in. What is the best interpretation?

A. The account is definitely compromised by a specific actor  
B. The rule is harmless because it is not malware  
C. The rule may indicate unauthorized access and should be investigated with audit and sign-in records  
D. The endpoint must contain ransomware

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** Correlation supports investigation, but does not establish attribution or every aspect of compromise.

</details>

### Question 7

Which step of the intelligence lifecycle involves turning collected observations into assessed findings?

A. Direction  
B. Analysis  
C. Dissemination  
D. Feedback

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** Analysis interprets and correlates information to answer the intelligence question.

</details>

### Question 8

Which statement about MITRE ATT&CK is correct?

A. It guarantees attribution to a named threat actor  
B. It is a catalog of only malware hashes  
C. It describes adversary tactics and techniques that can support analysis and detection  
D. It replaces incident response procedures

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** ATT&CK helps analysts describe observed behaviors, not prove actor identity.

</details>

### Question 9

A staff member's laptop shows no malware alert, but cloud logs show suspicious account activity. What is the best conclusion?

A. No incident is possible without malware  
B. Account compromise remains possible and should be investigated  
C. The cloud logs must be false  
D. All cloud accounts must be deleted immediately

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: B.** Credential theft and session misuse can occur without a malware alert on the endpoint.

</details>

### Question 10

Before sharing indicators from a client incident with an external community, an analyst should **first**:

A. Publish the full mailbox contents for transparency  
B. Ignore classification and sharing restrictions  
C. Check sharing permissions, sensitivity, and whether personal or confidential data can be removed  
D. Upload all attachments to an unapproved public scanner

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Answer: C.** Intelligence sharing must respect handling rules, privacy, and confidentiality.

</details>

## 10. Quick-reference glossary

| Term | Meaning |
| --- | --- |
| **IoC** | Artifact that may indicate compromise |
| **IoA** | Pattern or behavior that may indicate an attack |
| **TTP** | Tactic, technique, or procedure used by an adversary |
| **OSINT** | Intelligence gathered from publicly available sources |
| **SIEM** | Security information and event management system |
| **EDR** | Endpoint detection and response |
| **STIX** | Structured Threat Information Expression |
| **TAXII** | Trusted Automated Exchange of Intelligence Information |
| **TLP** | Traffic Light Protocol for information sharing boundaries |
| **Attribution** | Assessment of who is responsible for an activity, based on evidence |

## 11. Self-assessment

- [ ] I can distinguish an IoC from an IoA and recognize overlap.
- [ ] I can explain why indicators require context and validation.
- [ ] I can compare technical, tactical, operational, and strategic intelligence.
- [ ] I can describe the intelligence lifecycle and name common intelligence sources.
- [ ] I can explain the purpose of ATT&CK, STIX, TAXII, and TLP.
- [ ] I can map suspicious observations to safe investigation steps.
- [ ] I can write an assessment that separates evidence, hypotheses, and confidence.

## 12. Review and next steps

**Week 06 progression:**

- [Day 26 — Threat Actors and Motivations](day-26-threat-actors-and-motivations.md)
- [Day 27 — Attack Surfaces and Vectors](day-27-attack-surfaces-and-vectors.md)
- [Day 28 — Social Engineering and Phishing](day-28-social-engineering-and-phishing.md)
- [Day 29 — Malware and Ransomware](day-29-malware-and-ransomware.md)

You have now followed the path from **actor → exposure → social engineering → malware behavior → investigation and intelligence**. Complete the Week 06 diagnostic and review activities when they are published.

### Further reading

- [MITRE ATT&CK](https://attack.mitre.org/)
- [CISA Cybersecurity Advisories](https://www.cisa.gov/news-events/cybersecurity-advisories)
- [FIRST Traffic Light Protocol](https://www.first.org/tlp/)
- [OASIS CTI — STIX and TAXII](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=cti)

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
