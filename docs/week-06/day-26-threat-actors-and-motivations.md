# Day 26 — Threat Actors and Motivations

**Security+ Study Academy | Week 06: Threats, Vulnerabilities, and Mitigations**  
**File:** `docs/week-06/day-26-threat-actors-and-motivations.md`  
**Prerequisites:** Weeks 01–05  
**Security+ SY0-701 alignment:** Domain 2, Objective 2.1 (threat actors and motivations).  
**Level:** Beginner to intermediate  
**Estimated study time:** 60–80 minutes

## 1. Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish common threat actor categories by resources, capabilities, access, and typical behavior.
2. Compare motivations such as financial gain, espionage, disruption, ideology, revenge, and curiosity.
3. Explain why an actor's apparent sophistication does not establish their identity or motive.
4. Use evidence from an incident to develop a defensible threat actor profile.
5. Recommend defensive priorities based on the likely actor, their objectives, and the organization's exposure.

## 2. The big idea: Why threat actors matter

Imagine two people accessing the same confidential folder: one wants to sell its contents, while the other accidentally shares it publicly. The exposure may look similar, but the motivation, access and appropriate response differ.

🧠 **Memory hook:** Motive = why; capability = how; resources = with what; access = from where.

Threat profiling helps defenders prioritize controls and investigations without confusing a plausible explanation with proven attribution.

<a id="1-what-is-a-threat-actor"></a>

## 3. What is a threat actor?

A **threat actor** is a person, group, or organization that may cause harm to systems, data, people, or operations, whether deliberately or through negligent activity. Threat actors differ in what they want, what they can access, and what resources they can bring to an attack.

A useful way to assess an actor is to separate four dimensions:

| Dimension | Question | Examples |
| --- | --- | --- |
| **Motivation** | Why might they act? | Profit, espionage, ideology, revenge |
| **Capability** | What can they do? | Phishing, malware development, credential theft |
| **Resources** | What can they sustain or obtain? | Funding, personnel, infrastructure, time |
| **Access** | Where can they start? | Internet-facing systems, stolen accounts, legitimate internal permissions |

**Important:** These dimensions are not interchangeable. A highly motivated actor may have limited skills; an insider may have substantial access without sophisticated tools. A successful attack does not automatically imply a nation-state adversary.

<details markdown="1">
<summary>Reveal quick check: motivation versus capability</summary>

An attacker who wants to steal money has a **financial motivation**. Their ability to write custom malware is a **capability**. Neither fact alone identifies the actor.

</details>

<a id="2-common-threat-actor-categories"></a>

## 4. Common threat actor categories

### Nation-state and state-sponsored actors 

**Nation-state actors** operate on behalf of, or in alignment with, government interests. Some have substantial funding, specialized personnel, intelligence support, and long-term operational capacity. Common objectives include espionage, strategic advantage, intellectual property theft, and disruption of critical infrastructure.

**Example:** A government research organization experiences repeated credential attacks followed by targeted attempts to access restricted project files.

**Defensive priorities:** Strong identity controls, phishing-resistant MFA, privileged access management, network segmentation, logging, and incident response readiness.

**Exam caution:** Advanced techniques or persistence can be *consistent with* a state-sponsored campaign, but are not proof of sponsorship.

### Organized cybercriminals 

**Organized cybercriminals** conduct coordinated activity for profit. They may operate ransomware programs, sell stolen credentials, commit business email compromise (BEC), or provide criminal services to other attackers. Their skill and funding vary considerably.

**Example:** A company receives a fraudulent invoice after a criminal gains access to an employee's email account.

**Defensive priorities:** MFA, payment verification procedures, email protections, backups, endpoint detection, and user reporting channels.

### Hacktivists 

**Hacktivists** act in support of a political, social, or ideological cause. Tactics may include website defacement, distributed denial-of-service (DDoS) attacks, or unauthorized disclosure of information. Their motivations do not make unauthorized activity lawful.

**Example:** An activist group claims responsibility for disrupting an organization's public website after a controversial announcement.

**Defensive priorities:** DDoS protection, web application security, public-facing asset monitoring, and crisis communications planning.

### Insider threats 

An **insider threat** arises from a person with authorized access, such as an employee, contractor, or business partner. Insider incidents can be **malicious** (intentional theft or sabotage), **negligent** (careless handling of information), or **compromised** (an external attacker uses the insider's account).

**Example:** An employee exports confidential customer data to a personal account before leaving the company.

**Defensive priorities:** Least privilege, access reviews, data loss prevention (DLP), audit logs, offboarding controls, and clear data-handling policies.

**Critical distinction:** Suspicious activity from an employee account does not prove the employee acted maliciously. The account may be compromised.

### Unskilled attackers and script kiddies 

An **unskilled attacker**, sometimes called a *script kiddie*, often uses readily available tools or instructions without fully understanding their operation. Limited expertise does not mean limited impact, especially when an exposed system is poorly secured.

**Example:** Someone runs a publicly available scanning or exploitation tool against an unpatched service.

**Defensive priorities:** Patch management, attack surface reduction, secure configurations, and monitoring of exposed services.

### Shadow IT and unauthorized technology 

**Shadow IT** refers to technology used without organizational approval or oversight. It is **not inherently a threat actor category**: an employee using an unapproved cloud service may be well-intentioned. However, shadow IT can introduce vulnerabilities, data exposure, and gaps in monitoring.

**Example:** A team uploads confidential client documents to an unapproved file-sharing platform.

**Defensive priorities:** Approved alternatives, asset discovery, SaaS governance, data classification, and practical security awareness.

<a id="3-compare-actors-without-stereotyping"></a>

## 5. Compare actors without stereotyping

| Actor type | Common motivations | Possible resources | Typical access starting point | Important qualification |
| --- | --- | --- | --- | --- |
| Nation-state/state-sponsored | Espionage, strategic advantage, disruption | Sometimes extensive | External or compromised internal | Attribution requires corroboration |
| Organized cybercrime | Financial gain | Variable; sometimes extensive | External, stolen credentials | Can use sophisticated tools |
| Hacktivist | Ideology, publicity, protest | Variable | Public-facing services | May also seek data exposure |
| Malicious insider | Revenge, profit, grievance | Existing organizational knowledge | Authorized access | Not every insider incident is malicious |
| Negligent insider | Usually no malicious objective | Existing access | Authorized access | Risk can arise without hostile intent |
| Unskilled attacker | Curiosity, recognition, opportunism | Often limited | Publicly exposed services | Simple attacks can still be damaging |

**Remember:** These are patterns, not rules. Attackers can collaborate, change objectives, or imitate another group's tactics.

<a id="4-motivations-and-objectives"></a>

## 6. Motivations and objectives

A **motivation** explains *why* an actor attacks; an **objective** describes the specific result they seek.

| Motivation | Possible objective | Illustrative indicator |
| --- | --- | --- |
| Financial gain | Extort payment or steal funds | Ransom demand, fraudulent transfer request |
| Espionage | Obtain confidential intelligence | Targeted collection of sensitive documents |
| Ideology | Draw attention to a cause | Public claim of responsibility |
| Disruption | Interrupt services or operations | Destructive changes or traffic flooding |
| Revenge | Harm a former employer or colleague | Targeted deletion after a dispute |
| Blackmail or coercion | Force a person or organization to comply | Threat to disclose sensitive information |
| Philosophical or ethical beliefs | Act on a perceived moral obligation | Stated justification for unauthorized disclosure |
| War or strategic conflict | Degrade an adversary’s operations | Targeting strategically important services |
| Curiosity or recognition | Demonstrate access or gain status | Public boasting about unauthorized access |

An **indicator** is evidence worth investigating, not automatic proof of motive. For example, data exfiltration might support espionage, extortion, resale, or several motives at once.

<a id="5-capability-resources-and-attack-sophistication"></a>

## 7. Capability, resources, and attack sophistication

Consider the following distinctions:

- **Technical capability:** Skill to discover, exploit, or maintain access.
- **Operational resources:** Time, money, personnel, infrastructure, and tooling.
- **Opportunity and access:** Exposed systems, stolen credentials, or existing privileges.
- **Persistence:** Ability or willingness to continue despite defensive changes.

A resource-rich actor can purchase access instead of exploiting a system directly. Conversely, a low-skill actor can cause a major incident by abusing weak passwords or an unpatched public service.

### Why attribution is difficult 

**Attribution** is the process of assessing who is responsible for an activity. Attackers may reuse tools, route traffic through compromised infrastructure, plant misleading clues, or borrow tactics associated with other groups.

Useful evidence includes authenticated logs, account activity, infrastructure relationships, malware analysis, incident timelines, and independently corroborated intelligence. A single IP address, language string, or malware family rarely establishes a person's identity or state affiliation.

Use calibrated language:

- **Observed:** “The account downloaded 2 GB of files.”
- **Inferred:** “The pattern is consistent with attempted data theft.”
- **Unconfirmed:** “The activity may be linked to a financially motivated group.”

<a id="7-defensive-decision-making"></a>

## 8. Defensive decision-making

Threat profiling should inform controls, not replace foundational security practices. A practical order of operations is:

1. **Protect identities:** MFA, least privilege, secure recovery, and session management.
2. **Reduce exposure:** Patch known vulnerabilities, inventory assets, and limit unnecessary internet-facing services.
3. **Detect misuse:** Collect identity, endpoint, network, and cloud audit logs.
4. **Limit impact:** Segment networks, protect backups, and restrict sensitive-data access.
5. **Prepare response:** Define escalation paths, evidence preservation procedures, and business verification processes.

**Exam tip:** When a question asks which threat actor is *most likely*, use the actor's stated **motive**, **access**, and **resources** together. Do not select “nation-state” simply because an attack is complex.

<a id="9-review-and-self-assessment"></a>

## 9. Key terminology and self-assessment

Before moving to Day 27, confirm that you can:

- [ ] Explain the difference between motivation, capability, resources, and access.
- [ ] Compare nation-state actors, organized cybercriminals, hacktivists, insiders, and unskilled attackers.
- [ ] Distinguish malicious, negligent, and compromised insider scenarios.
- [ ] Explain why shadow IT is a risk condition rather than a threat actor identity.
- [ ] Build an evidence-based threat actor profile with an explicit confidence level.
- [ ] Recommend controls without overclaiming who conducted an attack.

### Key terms 

**Threat actor:** Entity capable of carrying out harmful activity.  
**Motivation:** Reason for acting.  
**Capability:** Ability to execute techniques.  
**Resources:** Funding, time, personnel, tools, and infrastructure.  
**Insider threat:** Security risk arising through an individual with authorized access.  
**Hacktivism:** Ideologically motivated cyber activity.  
**Shadow IT:** Technology used without organizational approval or oversight.  
**Attribution:** Assessment of who is responsible for observed activity.  
**Indicator:** Observable information relevant to an investigation, not necessarily proof of identity or intent.


<a id="6-practical-lab-build-a-threat-actor-profile"></a>

## 10. Worked case study: Northstar Legal Services

A midsize law firm reports the following events:

1. A staff member receives an email appearing to come from the firm's document-sharing provider.
2. A successful sign-in to that staff member's cloud account follows shortly afterward from an unfamiliar device.
3. Audit logs show access to several confidential client folders and a large download.
4. The firm's finance team receives a message requesting a change to a vendor's payment details.
5. Investigators have not confirmed whether the employee entered credentials into a phishing site or whether the sign-in resulted from another form of account compromise.



**Worked reasoning:** The payment-change request supports a financial-fraud hypothesis, but the unfamiliar sign-in and download do not establish the actor’s identity. A compromised account is plausible; employee complicity and the precise entry method remain unconfirmed. Prioritize account containment, evidence preservation and independent payment verification.

## 11. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Four actor dimensions | _Write here._ |
| Motivation versus objective | _Write here._ |
| State-sponsored versus cybercrime | _Write here._ |
| Insider incident types | _Write here._ |
| Shadow IT | _Write here._ |
| Attribution | _Write here._ |
| Evidence versus inference | _Write here._ |
| Defensive priorities | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Four actor dimensions | Motivation, capability, resources, access. |
| Motivation versus objective | Why an actor acts versus the result sought. |
| State-sponsored versus cybercrime | Strategic interests versus typically financial gain; neither is proven by sophistication. |
| Insider incident types | Malicious, negligent, or activity through a compromised account. |
| Shadow IT | Unapproved technology that can bypass governance and monitoring. |
| Attribution | Assessing responsibility using corroborated evidence. |
| Evidence versus inference | A recorded download is an observation; theft is a hypothesis until supported. |
| Defensive priorities | Protect identities, reduce exposure, detect misuse, limit impact and prepare response. |

</details>

## 12. Hands-on lab: Build and revise a threat actor profile

**Time:** 15–25 minutes · **Tools:** Notebook or text editor · **Safe, fictional evidence only**

### Part A — Build the profile

Use the Northstar case above. **Task:** Complete the profile below using only the available evidence.

| Profile field | Your assessment |
| --- | --- |
| Most plausible actor category | |
| Primary suspected motivation | |
| Observed capabilities | |
| Observed or inferred access | |
| Evidence supporting the assessment | |
| Alternative explanations | |
| Confidence (low/medium/high) and why | |
| Three immediate defensive actions | |
| Additional evidence needed | |

<details markdown="1">
<summary>Reveal model threat profile and reasoning</summary>

| Profile field | Model response |
| --- | --- |
| Most plausible actor category | External attacker, possibly financially motivated cybercriminal |
| Primary suspected motivation | Financial fraud and/or theft of sensitive data |
| Observed capabilities | Use of a convincing email pretext and unauthorized account activity; the exact compromise method is unconfirmed |
| Observed or inferred access | Access to the employee's cloud account and documents, based on audit logs |
| Supporting evidence | Unfamiliar-device sign-in, large document download, payment-change request |
| Alternative explanations | Account compromise unrelated to the email; unrelated fraudulent invoice attempt; legitimate download requiring verification |
| Confidence | Medium that the account was misused; low that a specific group or method has been identified |
| Immediate actions | Revoke active sessions and secure the account; preserve logs and investigate scope; independently verify and pause the payment change |
| Additional evidence | Email headers, identity-provider logs, MFA events, session details, download records, message provenance, endpoint telemetry |

**Why this works:** It distinguishes what the logs show from the analyst's hypotheses. It also avoids accusing the employee or naming a criminal group without evidence.

</details>

### Part B — Reassess when evidence changes 

Suppose the same account had legitimately downloaded the files for a scheduled client migration. Which part of your profile would change? Explain how independent verification affects your confidence and response priorities.

<details markdown="1">
<summary>Reveal stretch-task discussion</summary>

The download would no longer support the data-theft hypothesis as strongly, assuming the migration and file access are independently verified. The unfamiliar sign-in and payment-change request still require investigation. An analyst should update the assessment as evidence changes rather than defend the first hypothesis.

</details>

**Additional checks:** Explain how purchased credentials can substitute for technical skill, and why a confidential download does not establish state espionage.

<details markdown="1">
<summary>Reveal additional reasoning</summary>

Purchased access can reduce the technical work needed to compromise an account. A download can support several motives; attribution requires independent corroboration.

</details>

<a id="8-knowledge-check"></a>

## 13. Eight original Security+-style questions

Choose the **BEST** answer before opening each explanation. These are original practice questions, not actual CompTIA items.

### Q1.
A group steals proprietary defense research to support a government's strategic objectives. Which actor category is most consistent with the scenario?

A. Hacktivist  
B. Nation-state or state-sponsored actor  
C. Negligent insider  
D. Unskilled attacker  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** The strategic intelligence objective is most consistent with state-sponsored activity. The scenario does not independently prove attribution.

</details>

### Q2.
An employee accidentally shares a confidential folder publicly. Which description best fits the event?

A. Malicious insider attack  
B. Nation-state espionage  
C. Negligent insider incident  
D. Hacktivism  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** The employee had authorized access but made an accidental security mistake. Intentional harm has not been established.

</details>

### Q3.
Which factor most directly describes a threat actor's **motivation**?

A. Their ability to develop malware  
B. Their number of servers  
C. Their desire to obtain ransom payments  
D. Their access to an administrator account  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Seeking ransom payments explains *why* the actor attacks. The other options describe capability, resources, and access.

</details>

### Q4.
An attacker uses a freely available exploit against an exposed, unpatched server. What is the best conclusion?

A. The attacker must be highly sophisticated  
B. The attacker must be a current employee  
C. A relatively unskilled attacker may still cause serious damage  
D. The attack necessarily involves a nation-state  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Public tools and weak defenses can allow serious compromise without advanced expertise.

</details>

### Q5.
A group floods a government website with traffic to protest a new policy and publicly claims responsibility. Which actor category is most consistent?

A. Hacktivist  
B. Negligent insider  
C. Authorized penetration tester  
D. Vendor support technician  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** The ideological motivation and public protest are characteristic of hacktivism.

</details>

### Q6.
An investigator sees a successful login from a foreign IP address. What should they conclude?

A. The user is a foreign intelligence operative  
B. The IP address proves the attacker's physical location  
C. The login is irrelevant to the investigation  
D. The event warrants correlation with additional evidence  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** VPNs, proxies, cloud infrastructure, and compromised hosts make IP-only attribution unreliable.

</details>

### Q7.
A departing contractor intentionally deletes project files using access they still possess. Which factor most distinguishes this threat?

A. Access through authorized credentials  
B. Use of a public DDoS service  
C. Government funding  
D. A purely accidental mistake  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** The contractor's legitimate access makes this a potential malicious insider incident. Authorization to access a system is not authorization to destroy its data.

</details>

### Q8.
A team adopts an unapproved cloud storage tool for convenience. Which statement is most accurate?

A. The team is automatically a cybercriminal organization  
B. Shadow IT creates potential security and governance gaps  
C. Shadow IT is always safer than approved services  
D. The service is secure if employees have good intentions  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Unauthorized technology can bypass security review, monitoring, and data-handling requirements. Shadow IT is not itself a threat actor.

</details>

**Score guide:** 7–8 correct: strong understanding. 5–6: revisit actor comparisons and attribution. 0–4: review the lesson and repeat the lab.

## 14. Discussion challenge

A company discovers that an employee account accessed sensitive files after hours. The manager wants to blame the employee immediately. Explain two alternative explanations, identify evidence that would distinguish them, and recommend a proportionate response that protects the organization without making unsupported accusations.

## 15. Study from the Treadmill 🏃

- [Professor Messer — Security+ SY0-701 video course](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/sy0-701-comptia-security-plus-course/) — locate the Objective 2.1 lesson on threat actors.

**Active-listening prompts:** How do actor resources differ from capability? Why is insider access important? What evidence would change your attribution assessment?

## 16. One-minute recap

- **Motivation:** Why an actor acts.
- **Capability, resources, access:** Different dimensions of what an actor can accomplish.
- **Actor categories:** State-sponsored, criminal, hacktivist, insider and unskilled actors.
- **Shadow IT:** Governance risk, not proof of hostile intent.
- **Attribution:** Corroborate evidence and state confidence.
- **Defense:** Match controls to exposure and likely objectives, not stereotypes.

**Next:** Day 27 — Attack Vectors and Attack Surfaces.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.  

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
