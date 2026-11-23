# Week 06 Review — Threats, Attacks, and Threat Intelligence

**Security+ Study Academy | Week 06 | Days 26–30**

**Purpose:** Measure progress after completing Week 06, then revisit foundational concepts from earlier weeks. This post-check uses new scenarios to assess the same five competencies as the [Week 06 diagnostic](00-diagnostic.w6.md).

**Suggested time:** 35–45 minutes  
**Scoring:** 20 post-check questions + 5 cumulative questions = 25 points  
**Instructions:** Answer every question before revealing the answer key. Record A, B, C, or D for each item.

## How to use this review

1. Complete the 20-question post-check without notes.
2. Answer the five cumulative questions from previous weeks.
3. Use the answer sheet and scorecards to identify strengths and gaps.
4. Expand the answer key only after completing the questions.
5. Follow the lesson links to review missed or guessed answers.

---

## Post-check topic 1 — Threat Actors and Motivations (Day 26)

### Question 1

An intrusion combines months of stealthy access, specialized tooling, and targeted theft of diplomatic documents. Which actor is MOST consistent with this pattern?

A. A negligent employee  
B. A state-sponsored actor  
C. A casual website visitor  
D. An authorized auditor  

### Question 2

A contractor with valid access deliberately copies proprietary designs to sell. What is the primary threat classification?

A. Malicious insider  
B. External hacktivist  
C. Unskilled attacker  
D. Accidental insider  

### Question 3

A group defaces a company website and posts a manifesto protesting the company’s environmental policies. Which motivation best fits?

A. Financial extortion  
B. Industrial maintenance  
C. Ideological activism  
D. Accidental exposure  

### Question 4

A threat intelligence report names a specific group because its malware resembles a known sample. What should an analyst do FIRST?

A. Publish the attribution as confirmed  
B. Assume the attacker’s nationality  
C. Ignore all technical evidence  
D. Corroborate with independent behavioral and contextual evidence  

## Post-check topic 2 — Attack Surfaces and Vectors (Day 27)

### Question 5

Which item belongs in a company’s attack surface inventory?

A. Only publicly reachable servers  
B. Public applications, cloud identities, employee devices, and vendor connections  
C. Only confirmed vulnerabilities  
D. Only malware samples  

### Question 6

An attacker sends a fake payroll message containing a link to a credential-harvesting site. What is the initial attack vector?

A. Phishing message  
B. The payroll database  
C. A security patch  
D. The final stolen record  

### Question 7

A vendor’s shared administrator account can access customer files beyond its billing role. Which change most directly reduces exposure?

A. Increase log retention only  
B. Rename the account  
C. Enforce least privilege and separate vendor identities  
D. Publish the account password policy  

### Question 8

An attacker compromises a laptop, steals a cloud token, and uses it to download confidential files. What does the full sequence represent?

A. A single IP address  
B. A patch management cycle  
C. A firewall rule  
D. An attack path  

## Post-check topic 3 — Social Engineering and Phishing (Day 28)

### Question 9

A fraudster researches a CFO and sends a personalized invoice request using a supplier’s name. Which classification is MOST precise?

A. Random spam  
B. Spear phishing supporting business email compromise  
C. Worm propagation  
D. Wireless jamming  

### Question 10

A caller claiming to be IT support demands that an employee approve an unexpected MFA prompt immediately. What combination is being used?

A. Authority pretext and MFA fatigue  
B. DNS poisoning and encryption  
C. Network segmentation and backup  
D. Authorized vulnerability scanning  

### Question 11

An email displays a familiar company logo, but its sign-in link points to login.example-support.invalid. What is the BEST response?

A. Trust the logo  
B. Open the link in private browsing  
C. Verify the request through an established independent channel  
D. Reply with a password to test the sender  

### Question 12

A message claiming to be from a managing partner asks for an urgent bank-account change. What control is MOST effective?

A. Automatic forwarding  
B. Disabling all spam filtering  
C. Trusting the sender display name  
D. Out-of-band confirmation using a known contact method  

## Post-check topic 4 — Malware and Ransomware (Day 29)

### Question 13

Malicious code spreads automatically to vulnerable hosts without requiring users to run an infected file. What type is MOST likely?

A. Trojan  
B. Worm  
C. Logic bomb  
D. Spyware  

### Question 14

A suspicious executable installs a startup service so it launches after every reboot. Which behavior does this illustrate?

A. Persistence  
B. Data classification  
C. Nonrepudiation  
D. Backup verification  

### Question 15

An attacker copies sensitive client records before encrypting a firm’s file shares and threatens to publish the records. What is this pattern?

A. Cryptojacking  
B. Simple adware  
C. Double extortion ransomware  
D. DNS tunneling only  

### Question 16

An endpoint shows unusual script execution, new scheduled tasks, and outbound connections. What should an analyst conclude?

A. A named malware family is proven  
B. Data theft is certain  
C. The alerts can be ignored individually  
D. The correlated activity warrants investigation and evidence preservation  

## Post-check topic 5 — Indicators and Threat Intelligence (Day 30)

### Question 17

A SIEM alert matches a file hash reported in a threat feed. What kind of evidence is the hash?

A. A behavioral attack path  
B. An indicator of compromise  
C. Proof of the attacker’s identity  
D. A strategic policy objective  

### Question 18

An account signs in from an unusual location, grants an unfamiliar OAuth app access, then downloads many files. Why is the sequence valuable?

A. It captures a potentially malicious behavior pattern (IoA)  
B. It confirms the user’s nationality  
C. It makes logs unnecessary  
D. It is always benign  

### Question 19

Which pairing correctly describes threat intelligence sharing standards?

A. TAXII describes malware behavior; STIX is a firewall  
B. STIX is a packet capture; TAXII is a VPN  
C. STIX structures threat intelligence; TAXII transports/exchanges it  
D. Both are encryption algorithms  

### Question 20

A threat feed reports a suspicious domain used two years ago. What is the BEST next step before blocking it?

A. Assume it is malicious forever  
B. Attribute it to a country  
C. Delete all related logs  
D. Validate freshness, relevance, confidence, and potential false positives  

---

## Cumulative review — Weeks 01–05

These five questions revisit earlier networking and security foundations. They do **not** count toward the Week 06 topic scorecards.

### Question 21

A new employee connects a personal router to an office switch, bypassing the approved guest network. Which Week 05 concept is MOST directly relevant?

A. Network access control and rogue devices  
B. File hashing  
C. Public-key certificates only  
D. Data retention periods  

### Question 22

A firewall permits inbound TCP 443 to a web application but blocks inbound TCP 22 from the internet. Which principle is being applied?

A. Open all services  
B. Least privilege for network access  
C. Disable encryption  
D. Trust every source  

### Question 23

An analyst needs to identify the destination service in a packet capture showing TCP port 53. Which protocol is commonly associated with that port?

A. SSH  
B. HTTPS  
C. DNS  
D. SMTP submission  

### Question 24

A workstation receives an IP address, gateway, and DNS server automatically on joining the network. Which service provided these settings?

A. TLS  
B. ARP only  
C. NAT only  
D. DHCP  

### Question 25

A remote employee must securely connect to internal company resources over an untrusted network. Which Week 05 technology is most directly relevant?

A. VPN  
B. Plain HTTP  
C. Open guest Wi-Fi  
D. MAC address spoofing  

---

## Answer sheet

| Question | Your answer | Question | Your answer | Question | Your answer |
|---|---|---|---|---|---|
| 1 | _____ | 9 | _____ | 17 | _____ |
| 2 | _____ | 10 | _____ | 18 | _____ |
| 3 | _____ | 11 | _____ | 19 | _____ |
| 4 | _____ | 12 | _____ | 20 | _____ |
| 5 | _____ | 13 | _____ | 21 | _____ |
| 6 | _____ | 14 | _____ | 22 | _____ |
| 7 | _____ | 15 | _____ | 23 | _____ |
| 8 | _____ | 16 | _____ | 24 | _____ |

## Week 06 topic-level scorecard

| Topic | Questions | Correct | Maximum |
|---|---|---|---|
| Day 26: Threat Actors and Motivations | 1–4 | ___ | 4 |
| Day 27: Attack Surfaces and Vectors | 5–8 | ___ | 4 |
| Day 28: Social Engineering and Phishing | 9–12 | ___ | 4 |
| Day 29: Malware and Ransomware | 13–16 | ___ | 4 |
| Day 30: Indicators and Threat Intelligence | 17–20 | ___ | 4 |
| **Week 06 post-check total** | **1–20** | **___** | **20** |
| Cumulative review | 21–25 | ___ | 5 |
| **Overall review total** | **1–25** | **___** | **25** |

**Compare your results:** Diagnostic ___/20 → Post-check ___/20 = change of ___ points.

**Interpret your topic scores:** 4/4 = strong evidence of mastery; 3/4 = review the missed concept; 2/4 = revisit the lesson and lab; 0–1/4 = restudy the lesson and retry the questions after practice. A single short quiz is not a certification of competence.

---

## Post-check answer key and explanations

Open each topic only after you have completed the questions.

<details markdown="1">
<summary>Reveal answers 1–4 — Threat Actors and Motivations</summary>

**1. B — A state-sponsored actor** The combination suggests a well-resourced espionage operation, but does not prove attribution. [Review the lesson](day-26-threat-actors-and-motivations.md#2-common-threat-actor-categories).

**2. A — Malicious insider** Authorized access combined with intentional misuse makes this a malicious insider threat. [Review the lesson](day-26-threat-actors-and-motivations.md#insider-threats).

**3. C — Ideological activism** Public protest and messaging are consistent with hacktivism; motives should still be corroborated. [Review the lesson](day-26-threat-actors-and-motivations.md#4-motivations-and-objectives).

**4. D — Corroborate with independent behavioral and contextual evidence** Tools can be reused, stolen, or deliberately imitated. Attribution requires multiple independent lines of evidence. [Review the lesson](day-26-threat-actors-and-motivations.md#why-attribution-is-difficult).

</details>

<details markdown="1">
<summary>Reveal answers 5–8 — Attack Surfaces and Vectors</summary>

**5. B — Public applications, cloud identities, employee devices, and vendor connections** Attack surfaces include exposed and potentially reachable assets, services, identities, people, and relationships. [Review the lesson](day-27-attack-surfaces-and-vectors.md#2-what-belongs-on-an-attack-surface).

**6. A — Phishing message** The phishing message is the method used to initiate the attempt; the database is a possible target. [Review the lesson](day-27-attack-surfaces-and-vectors.md#3-attack-vectors-how-attempts-begin).

**7. C — Enforce least privilege and separate vendor identities** Restricting vendor access reduces reachable assets and limits the consequences of credential compromise. [Review the lesson](day-27-attack-surfaces-and-vectors.md#7-prioritize-exposure-reduction).

**8. D — An attack path** An attack path links an initial foothold through intermediate actions to an objective. [Review the lesson](day-27-attack-surfaces-and-vectors.md#5-model-an-attack-path).

</details>

<details markdown="1">
<summary>Reveal answers 9–12 — Social Engineering and Phishing</summary>

**9. B — Spear phishing supporting business email compromise** The tailored targeting makes it spear phishing; the invoice diversion is a BEC-style objective. [Review the lesson](day-28-social-engineering-and-phishing.md#2-phishing-and-its-major-variations).

**10. A — Authority pretext and MFA fatigue** The caller impersonates a trusted role and pressures approval of an unsolicited authentication request. [Review the lesson](day-28-social-engineering-and-phishing.md#mfa-fatigue-and-help-desk-impersonation).

**11. C — Verify the request through an established independent channel** Branding is easy to copy. Independent verification avoids interacting with an untrusted destination. [Review the lesson](day-28-social-engineering-and-phishing.md#safe-handling-principles).

**12. D — Out-of-band confirmation using a known contact method** Payment changes require a trusted verification workflow that is independent of the suspicious message. [Review the lesson](day-28-social-engineering-and-phishing.md#6-defensive-controls-people-process-and-technology).

</details>

<details markdown="1">
<summary>Reveal answers 13–16 — Malware and Ransomware</summary>

**13. B — Worm** Self-propagation between systems is the defining behavior of a worm. [Review the lesson](day-29-malware-and-ransomware.md#virus-versus-worm).

**14. A — Persistence** A startup service helps malicious code maintain execution across reboots. [Review the lesson](day-29-malware-and-ransomware.md#persistence-examples).

**15. C — Double extortion ransomware** The actor combines disruption through encryption with pressure from threatened data disclosure. [Review the lesson](day-29-malware-and-ransomware.md#4-ransomware-and-extortion).

**16. D — The correlated activity warrants investigation and evidence preservation** The sequence may indicate compromise, but family identification and exfiltration need corroboration. [Review the lesson](day-29-malware-and-ransomware.md#5-recognizing-and-classifying-incident-indicators).

</details>

<details markdown="1">
<summary>Reveal answers 17–20 — Indicators and Threat Intelligence</summary>

**17. B — An indicator of compromise** A file hash is an artifact-based IoC; matching it does not by itself establish malicious intent. [Review the lesson](day-30-indicators-and-threat-intelligence.md#iocs-versus-ioas).

**18. A — It captures a potentially malicious behavior pattern (IoA)** Related activities can signal attacker behavior even when individual indicators are inconclusive. [Review the lesson](day-30-indicators-and-threat-intelligence.md#iocs-versus-ioas).

**19. C — STIX structures threat intelligence; TAXII transports/exchanges it** STIX standardizes structured threat information, and TAXII supports automated exchange. [Review the lesson](day-30-indicators-and-threat-intelligence.md#6-threat-intelligence-sharing-and-frameworks).

**20. D — Validate freshness, relevance, confidence, and potential false positives** Intelligence must be contextualized and evaluated before operational action. [Review the lesson](day-30-indicators-and-threat-intelligence.md#evaluating-source-quality).

</details>

## Cumulative review answer key

<details markdown="1">
<summary>Reveal answers 21–25 — Cumulative review</summary>

**21. A — Network access control and rogue devices** NAC and wireless/network access policies help identify or prevent unauthorized devices. [Review the earlier lesson](../week-05/day-25-wireless-and-network-access-control.md).

**22. B — Least privilege for network access** Restricting reachable ports and sources reduces exposure. [Review the earlier lesson](../week-05/day-22-firewalls-ids-ips-and-proxies.md).

**23. C — DNS** DNS commonly uses port 53 over UDP and TCP. [Review the earlier lesson](../week-04/day-18-ports-and-protocols.md).

**24. D — DHCP** DHCP assigns IP configuration to clients. [Review the earlier lesson](../week-04/day-19-dns-dhcp-and-network-services.md).

**25. A — VPN** A correctly configured VPN provides a protected tunnel for authorized remote access. [Review the earlier lesson](../week-05/day-24-vpns-and-secure-remote-access.md).

</details>

---

## Reflection and next steps

- Which topic improved the most from diagnostic to post-check? What changed in your reasoning?
- For every missed item, explain why the correct answer is supported and why your original answer is less appropriate.
- Revisit the practical labs in Days 26–30: actor profiling, attack surface modeling, phishing analysis, malware indicator classification, and intelligence-driven investigation.
- If your cumulative score is low, revisit the linked earlier lessons before moving to Week 07.

### Week 06 lesson index

- [Day 26 — Threat Actors and Motivations](day-26-threat-actors-and-motivations.md)
- [Day 27 — Attack Surfaces and Vectors](day-27-attack-surfaces-and-vectors.md)
- [Day 28 — Social Engineering and Phishing](day-28-social-engineering-and-phishing.md)
- [Day 29 — Malware and Ransomware](day-29-malware-and-ransomware.md)
- [Day 30 — Indicators and Threat Intelligence](day-30-indicators-and-threat-intelligence.md)

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
