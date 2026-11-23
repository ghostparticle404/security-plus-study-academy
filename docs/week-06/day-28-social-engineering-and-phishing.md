# Day 28 — Social Engineering and Phishing

**Security+ Study Academy | Week 06: Threats, Vulnerabilities, and Mitigations**  
**File:** `docs/week-06/day-28-social-engineering-and-phishing.md`  
**Prerequisites:** [Day 26 — Threat Actors and Motivations](day-26-threat-actors-and-motivations.md) and [Day 27 — Attack Surfaces and Vectors](day-27-attack-surfaces-and-vectors.md)  
**Estimated study time:** 60–80 minutes (allow additional time for the lab if needed)

**Security+ SY0-701 alignment:** Objective 2.2 (threat vectors and attack surfaces, including human vectors); related Objective 2.1 (threat actors and motivations) and 5.6 (security awareness).

## 1. Learning objectives {#learning-objectives}

By the end of this lesson, you should be able to:

1. Explain social engineering as an attack method that exploits human decisions and organizational processes.
2. Distinguish phishing, spear phishing, whaling, smishing, vishing, and business email compromise (BEC).
3. Recognize pretexting, impersonation, authority, urgency, scarcity, familiarity, and other influence techniques.
4. Analyze a suspicious message using sender information, URLs, context, and independently verified evidence.
5. Recommend practical prevention, verification, reporting, and response controls.

## 2. The big idea: Trust is an attack surface

A locked door is only useful if people verify who is asking to enter. Social engineering targets the decision to open that door, whether it is a payment approval, an account recovery request, or a physical entrance. Security controls must protect both technology and the decisions people make.

**Memory hook:** Stop, verify independently, then act.

## 3. Social engineering: exploiting trust rather than code {#1-social-engineering-exploiting-trust-rather-than-code}

**Social engineering** is the use of deception or manipulation to persuade someone to disclose information, grant access, transfer money, install software, or take another action that benefits an attacker. It can be carried out through email, text messages, phone calls, collaboration platforms, social networks, or face-to-face interactions.

A social engineering attempt may succeed even when an organization has patched its systems. The attacker may persuade a legitimate user to perform an otherwise authorized action, such as approving an MFA request or sending a sensitive document.

**Connect to earlier lessons:** Day 26 explains who might conduct an attack and why; Day 27 explains the entry point and vector. This lesson explains how an attacker uses a message or interaction to turn an exposed human or business process into an opportunity.

| Element | Example |
| --- | --- |
| Threat actor | Financially motivated criminal group |
| Objective | Divert a vendor payment |
| Entry point | Accounts payable employee's inbox |
| Attack vector | Impersonated vendor email |
| Influence technique | Urgency and apparent authority |
| Requested action | Replace bank account details |
| Potential impact | Financial loss and compromised business records |

> **Exam distinction:** A persuasive email is an *attack attempt*, not proof that the recipient was compromised. Evidence of clicking, credential entry, account access, or money movement is needed to determine what happened.

<details markdown="1">
<summary>Reveal quick check: identify the method</summary>

A caller claims to be from IT and asks an employee to approve a login notification. This is **social engineering**, using **impersonation** and **authority**. If the attacker calls by telephone, the channel is **vishing**.

</details>

## 4. Phishing and its major variations {#2-phishing-and-its-major-variations}

**Phishing** uses deceptive communications to induce an action. The message may link to a counterfeit sign-in page, deliver a malicious attachment, solicit confidential information, or request a fraudulent payment. Not every phishing message contains malware.

| Term | Meaning | Typical example |
| --- | --- | --- |
| **Phishing** | Broad deceptive-message campaign | Generic “Your mailbox is full” email |
| **Spear phishing** | Targeted message tailored to a person or group | Fake project document referencing a real team |
| **Whaling** | Targeting senior executives or other high-value leaders | Fraudulent legal notice sent to a CEO |
| **Smishing** | Phishing by SMS or text messaging | “Package delivery failed; update your address” |
| **Vishing** | Voice-based phishing | Caller impersonates bank fraud staff |
| **Quishing** | Phishing that uses a QR code as the lure or link | QR code on a fake parking-payment notice |
| **BEC** | Business email compromise: abuse of business identity or communications to facilitate fraud | Impersonated CFO requests an urgent wire transfer |
| **Clone phishing** | A convincing copy of a legitimate message with altered malicious content | Resent invoice email with a substituted link |

**Important:** These categories overlap. A carefully personalized SMS can be both *smishing* and *spear phishing*. BEC may involve a spoofed address, a lookalike domain, or a genuinely compromised business account.

### Phishing versus spear phishing

A generic message sent to thousands of users is phishing. A message mentioning a particular employee's manager, current project, and upcoming meeting is more likely spear phishing. Personalization increases credibility but does not establish the sender's legitimacy.

### Whaling and executive impersonation

Whaling targets people with substantial access, influence, or financial authority. An attacker may also impersonate an executive to pressure a subordinate; that is executive impersonation and may form part of BEC, but it does not automatically mean the executive was the target.

### MFA fatigue and help-desk impersonation

Attackers may repeatedly trigger authentication prompts, hoping someone approves one to stop the interruptions. Others impersonate IT staff to persuade an employee to disclose a one-time code, reset MFA, or register a new authenticator. A legitimate help desk should follow an established identity-verification procedure rather than asking for a user's password or authentication code.

<details markdown="1">
<summary>Reveal classification exercise</summary>

**Scenario:** A text message references a real employee's upcoming conference and directs them to a counterfeit event-registration page.

**Answer:** **Smishing** describes the text-message channel; **spear phishing** describes its targeted personalization. The counterfeit registration page is a possible credential-harvesting entry point.

</details>

## 5. Influence techniques and psychological triggers {#3-influence-techniques-and-psychological-triggers}

Attackers often combine several techniques in one message.

| Technique | How it works | Warning sign |
| --- | --- | --- |
| **Authority** | Claims instructions come from a powerful or trusted role | “The CEO approved this—do not delay” |
| **Urgency** | Creates time pressure to bypass normal checks | “You have 10 minutes before access is suspended” |
| **Scarcity** | Suggests an opportunity or resource is running out | “Only two places remain” |
| **Fear or intimidation** | Threatens punishment, exposure, or financial loss | “Pay immediately or face legal action” |
| **Familiarity or liking** | Uses a friendly tone or shared context | “Great talking at yesterday's conference” |
| **Social proof** | Claims other people have already complied | “Everyone else has completed the verification” |
| **Reciprocity** | Offers a favor or reward to create obligation | “I helped you; now send me the access code” |
| **Curiosity** | Promises surprising or exclusive information | “Confidential salary list attached” |

These are **indicators**, not automatic proof of malicious intent. Legitimate communications can be urgent, personalized, or authoritative. The safest response is to verify the requested action through a known, independent channel.

### Pretexting

**Pretexting** means constructing a believable story or identity to justify a request. A caller might claim to be a new employee who cannot access the company portal, or a vendor who urgently needs banking information updated.

A pretext usually answers three questions: **Who am I? Why am I contacting you? Why should you act now?**

### Impersonation and identity spoofing

An attacker can pretend to be a manager, supplier, recruiter, colleague, or IT technician. The displayed sender name, caller ID, or social profile is not conclusive proof of identity. Email spoofing and lookalike domains are technical methods that can reinforce impersonation.

### Other social engineering methods

- **Baiting:** Enticing someone with a promised reward, useful download, or attractive item, such as an unknown USB drive labeled “Payroll.”
- **Quid pro quo:** Offering a supposed service or benefit in exchange for information or access, such as fake technical support.
- **Tailgating:** Following an authorized person through a controlled entrance without independently authenticating.
- **Shoulder surfing:** Observing sensitive information, such as passwords, on another person's screen or keyboard.
- **Dumpster diving:** Recovering discarded documents or devices that reveal sensitive information.
- **Watering-hole attack:** Compromising a site commonly visited by a targeted group; this can be part of a broader social engineering campaign but often relies on technical exploitation.

> **Exam tip:** Distinguish the *story* (pretexting), the *claimed identity* (impersonation), the *psychological pressure* (urgency/authority), and the *delivery channel* (email, SMS, voice). A single incident can involve all four.

## 6. Links, domains, and message authentication {#5-links-domains-and-message-authentication}

A phishing message may look polished and contain no spelling mistakes. Instead of judging by appearance alone, inspect its technical and business context.

### Reading a URL safely

Consider this fictional URL:

`https://login.northstar.example.security-check.example/session`

The **registered-domain portion** here is `security-check.example`, not `northstar.example`. A familiar company name placed earlier in a hostname does not make the site belong to that company.

Other warning signs include lookalike spelling, unexpected shortened links, misleading hyperlink text, and unexpected redirects. **HTTPS indicates an encrypted connection, not that a website is trustworthy.**

For suspicious messages, use a known bookmark or manually navigate to the official service instead of opening the supplied link. Do not paste sensitive tokens or private links into public URL-analysis services.

### Email headers and authentication

| Mechanism | What it helps establish | What it does **not** prove |
| --- | --- | --- |
| **SPF** | Whether a sending server is authorized for an envelope sender domain | That the visible sender is honest or the message is safe |
| **DKIM** | Whether a message has a valid cryptographic signature associated with a signing domain | That the signer is trustworthy |
| **DMARC** | Whether SPF or DKIM authentication aligns with the visible From domain, subject to the domain's policy | That a legitimate or compromised sender cannot send malicious content |

A message can pass SPF, DKIM, and DMARC and still be phishing—especially when sent from an attacker-controlled domain or a compromised legitimate account. Conversely, forwarding and configuration problems can sometimes affect authentication results.

### Safe handling principles

- Use the email client's **Report phishing** function or the organization's designated security channel.
- Preserve the original message and headers when the incident-response process requires them.
- Do not open suspicious attachments or enable macros to “test” a message.
- Avoid clicking links simply to see where they lead; inspect available link information without visiting the destination.
- Record what is **observed** separately from what is **inferred**.

## 7. Defensive controls: people, process, and technology {#6-defensive-controls-people-process-and-technology}

| Layer | Controls | Why they help |
| --- | --- | --- |
| **People** | Scenario-based training, simple reporting, verification habits | Improves recognition and early escalation |
| **Process** | Out-of-band payment confirmation, dual approval, verified vendor change workflow | Prevents one deceptive message from authorizing a high-impact action |
| **Identity** | Phishing-resistant MFA, least privilege, secure recovery procedures | Reduces credential theft and account-takeover risk |
| **Email** | SPF, DKIM, DMARC, attachment scanning, anti-phishing filters | Makes some spoofing and malicious content easier to block or detect |
| **Endpoint** | Patch management, application controls, endpoint detection and response | Limits damage from malicious attachments or downloads |
| **Monitoring** | Login anomaly detection, mailbox-rule alerts, unusual payment-change alerts | Helps discover account misuse and fraudulent workflows |

**Phishing-resistant MFA**, such as appropriately configured FIDO2/WebAuthn security keys or passkeys, offers stronger protection against credential-harvesting attacks than one-time codes. No single control eliminates all forms of social engineering, particularly fraudulent payment instructions made by an authorized user.

### If someone clicked or submitted information

1. **Report promptly** through the approved security channel; early reporting can reduce harm.
2. **Describe exactly what happened:** link opened, file downloaded, credentials entered, MFA approved, or payment sent.
3. **Follow incident-response instructions:** the appropriate containment differs by event. For suspected credential exposure, use a trusted route to reset credentials and revoke sessions as directed.
4. **Preserve evidence:** message, time, destination, relevant screenshots, and any available logs. Do not delete or alter evidence unnecessarily.
5. **Escalate payment fraud immediately:** notify the financial institution and internal fraud-response contacts using verified channels; recovery can be time-sensitive.

**Avoid blaming the reporter.** A reporting culture that punishes honest mistakes can delay detection and increase impact.

## 8. Key terminology and self-assessment

Before moving to Day 29, confirm that you can:

- [ ] Explain social engineering and its relationship to attack vectors.
- [ ] Distinguish phishing, spear phishing, whaling, smishing, vishing, quishing, and BEC.
- [ ] Recognize pretexting, impersonation, urgency, authority, and scarcity.
- [ ] Explain baiting, quid pro quo, tailgating, shoulder surfing, and dumpster diving.
- [ ] Analyze a mock email without assuming that suspicious indicators prove compromise.
- [ ] Explain why SPF, DKIM, DMARC, and HTTPS do not guarantee safety.
- [ ] Recommend independent verification, phishing-resistant MFA, reporting, and payment controls.


### Key terms

**Social engineering:** Manipulation of people to induce actions that benefit an attacker.  
**Phishing:** Deceptive communication intended to obtain information or prompt an unsafe action.  
**Spear phishing:** Targeted phishing tailored to a particular recipient or group.  
**Whaling:** Phishing targeting executives or other high-value leaders.  
**Smishing:** SMS/text-based phishing.  
**Vishing:** Voice-based phishing.  
**Quishing:** QR-code-based phishing.  
**BEC:** Business email compromise; fraud exploiting business communications or identities.  
**Pretexting:** Creating a plausible story to justify a deceptive request.  
**Impersonation:** Pretending to be another person or trusted role.  
**MFA fatigue:** Repeated authentication prompts intended to elicit an unintended approval.  
**Out-of-band verification:** Confirmation through a separate, independently established channel.  
**SPF / DKIM / DMARC:** Email authentication and domain-alignment mechanisms.  
**Phishing-resistant MFA:** Authentication designed to resist credential capture and relay through counterfeit sign-in pages.
## 9. Worked case study: Suspected vendor-payment fraud {#4-anatomy-of-a-suspicious-email}

Use this fictional example for analysis. All names, addresses, and domains below are illustrative; `example.com` and `.example` domains are reserved for documentation.

**Displayed sender:** Morgan Ellis — Finance Director  
**From:** `Morgan Ellis <morgan.ellis@firm-payments.example>`  
**Reply-To:** `billing-update@external-invoices.example`  
**To:** `accounts-payable@northstar.example`  
**Subject:** `URGENT: Updated bank details for today's payment`

> Hi team,
>
> I'm in meetings and can't take calls. Our legal vendor changed banks this morning, so the invoice due today must go to the new account. Please use the updated details in the attached document and confirm the transfer before 3:00 p.m. The CEO has already approved it. Keep this between Finance and AP until we finalize the paperwork.
>
> Thanks,  
> Morgan

**Attachment:** `Updated_Bank_Details.pdf` (fictional; no file is supplied)

### Analyze the clues

1. **Sender identity:** The display name says “Finance Director,” but the address uses a separate payments-themed domain. Compare it with the organization's established domain.
2. **Reply-To mismatch:** A different reply destination may be suspicious, though legitimate systems can also use separate reply addresses.
3. **Urgency:** The deadline pressures the recipient to act before checking the request.
4. **Authority:** “The CEO approved it” attempts to substitute a claim for verifiable approval.
5. **Isolation:** “Keep this between Finance and AP” discourages normal review.
6. **Process change:** A new bank account for an existing vendor is a high-risk payment change that warrants independent confirmation.
7. **Attachment:** A PDF is not automatically malicious. The risk is the requested financial action and any potentially unsafe content or links.

**Provisional assessment:** This message has multiple indicators of **possible BEC / vendor-payment redirection**. It is not enough to conclude that an account was compromised or that the attachment contains malware.

### What would you do?

Do **not** reply to the suspicious message to verify it. Use the finance director's known number or an established internal messaging channel, and confirm vendor banking changes using a previously verified vendor contact. Follow the organization's approval and incident-reporting process.

<details markdown="1">
<summary>Reveal model analyst conclusion</summary>

**Classification:** Suspected business email compromise or vendor impersonation.  
**Likely objective:** Redirect an authorized payment.  
**Influence methods:** Authority, urgency, impersonation, and discouraging independent verification.  
**Evidence available:** Message text and metadata presented in the scenario.  
**Unknowns:** Whether the sender account is compromised, whether the attachment is harmful, whether the recipient acted, and whether any funds moved.  
**Recommended action:** Preserve the message, report it, verify through known contacts, and pause changes to payment details until approved.

</details>

## 10. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| What distinguishes social engineering from technical exploitation? | _Write here._ |
| Compare phishing, spear phishing, and whaling. | _Write here._ |
| Distinguish smishing, vishing, and quishing. | _Write here._ |
| Compare pretexting, impersonation, and urgency. | _Write here._ |
| Why can a message pass DMARC and still be dangerous? | _Write here._ |
| What does HTTPS establish, and what does it not establish? | _Write here._ |
| How does independent verification prevent payment fraud? | _Write here._ |
| What should you do after suspected credential disclosure? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Social engineering | Manipulates human decisions; it can succeed without exploiting a software flaw. |
| Phishing variants | General phishing is broad; spear phishing is tailored; whaling targets high-value leaders. |
| Channels | SMS, voice, and QR codes respectively. |
| Influence | Pretexting supplies a story; impersonation supplies a claimed identity; urgency creates time pressure. |
| DMARC | A malicious or compromised legitimate sender can pass authentication. |
| HTTPS | Protects transport to the authenticated endpoint; it does not prove the site's intentions. |
| Verification | Confirm changes through a previously established contact and approval workflow. |
| Disclosure | Report promptly, preserve evidence, and follow approved credential reset and session revocation procedures. |

</details>

## 11. Hands-on lab: Analyze a mock phishing email {#7-practical-lab-analyze-a-mock-phishing-email}

**Time:** 15–25 minutes.

**Lab objective:** Identify social engineering indicators, distinguish observations from hypotheses, classify the vector, and recommend a safe response.

**Environment:** This is a paper-based or Markdown-only exercise. Do not send the example message, visit suspicious websites, or interact with real accounts.

### Scenario

You work on the security team for a 25-person law firm. An administrative assistant forwards the following message:

**From:** `IT Helpdesk <helpdesk@northstar-support.example>`  
**To:** `assistant@northstar.example`  
**Subject:** `ACTION REQUIRED: Your account will be disabled in 20 minutes`

> Our monitoring system detected a sign-in from a new location. To keep your access active, verify your identity at the link below and enter the six-digit security code from your authenticator. Do not call the help desk because our phone queue is currently offline.
>
> `https://verify-account.northstar-support.example/login`
>
> IT Security Team

The assistant reports the message **without opening the link**.

### Part A — Analyze the evidence

#### Task A — Extract observable evidence

Create a table with three columns: **Observation**, **Possible interpretation**, and **What would verify it?** Include at least five entries.

#### Task B — Classify the attack

Identify the delivery channel, suspected phishing subtype, pretext, influence techniques, requested action, likely attacker objective, and targeted asset.

#### Task C — Build a plausible attack path

Write a possible sequence from message delivery to unauthorized account access. Label each step as **observed**, **possible**, or **not established**.

### Part B — Recommend controls and report

#### Task D — Recommend controls

Choose one control each from the **people**, **process**, and **technical** categories. Explain why each would help.

#### Task E — Write an analyst summary

In 75–125 words, explain the evidence, your confidence level, remaining unknowns, and the next safe action.

<details markdown="1">
<summary>Reveal lab answer guide and model analyst summary</summary>

**Task A examples:** The subject threatens a 20-minute deadline (**observed**; urgency is a reasonable interpretation); the sender uses a support-themed domain (**observed**; ownership must be verified); the message asks for an authenticator code (**observed**; possible MFA interception); it discourages calls (**observed**; may be attempting to prevent independent verification); the link points to a login page on the same support-themed domain (**observed**; legitimacy unverified).

**Task B:** Email phishing; possibly spear phishing if the recipient was intentionally selected (not proven from this message alone). Pretext: emergency account-security verification. Techniques: urgency, fear, authority, and help-desk impersonation. Requested action: visit the link and enter an authenticator code; credential entry is a possible further request, not established. Likely objective: obtain account access. Targeted asset: employee identity and associated firm data.

**Task C:** Message received (**observed**) → recipient visits link (**not observed**) → submits credentials/code (**not observed**) → attacker attempts authentication (**possible**) → attacker accesses account (**not established**).

**Task D:** People: reporting practice. Process: verification through a known help-desk channel. Technical: phishing-resistant MFA and email filtering.

**Task E model:** “An employee received an email claiming that account access would be disabled within 20 minutes. The message uses a support-themed sender domain, requests an authenticator code, and discourages calling the help desk. These are strong indicators of a phishing attempt using IT impersonation and urgency. The employee reported the message without opening the link, and there is no evidence in this scenario of credential disclosure or account compromise. We recommend preserving the message, reporting it to the security team, verifying the account status through the established IT channel, and checking for similar messages across the organization.”

</details>

## 12. Eight original Security+-style questions {#8-knowledge-check-10-security-style-questions}

Choose the BEST answer before revealing each explanation. These are original practice questions, not actual CompTIA items.

### Question 1

An attacker sends the same fake password-expiration email to thousands of employees at unrelated organizations. Which technique is MOST directly described?

A. Whaling  
B. General phishing  
C. Tailgating  
D. Shoulder surfing

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B. General phishing.** A broad deceptive email campaign is phishing. Whaling targets high-value leaders, while tailgating and shoulder surfing are physical-world techniques. See [Phishing and its major variations](#2-phishing-and-its-major-variations).

</details>

### Question 2

A text message names an employee's real supervisor and asks the employee to visit a fake company portal. Which classification is BEST?

A. Smishing and spear phishing  
B. Vishing and tailgating  
C. Whaling and dumpster diving  
D. Baiting and shoulder surfing

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A. Smishing and spear phishing.** SMS is the channel, while the tailored content suggests targeted phishing. See [Phishing and its major variations](#2-phishing-and-its-major-variations).

</details>

### Question 3

A caller claims to be the company's IT administrator and says the employee must approve an MFA prompt immediately. Which combination BEST describes the attempt?

A. Vishing, impersonation, and authority  
B. DNS poisoning and lateral movement  
C. Tailgating and scarcity  
D. Credential stuffing and patch management

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A. Vishing, impersonation, and authority.** The attacker uses a voice channel, claims a trusted identity, and pressures the employee to act. See [Influence techniques and psychological triggers](#3-influence-techniques-and-psychological-triggers).

</details>

### Question 4

An accounts payable clerk receives a message from a purported vendor asking to change bank details for an invoice. What is the BEST immediate action?

A. Reply to the email asking whether the request is genuine  
B. Send a small test payment to the new account  
C. Verify the change through a previously established vendor contact and approval workflow  
D. Trust the request if the email uses HTTPS links

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Independent, out-of-band verification and established payment controls reduce the risk of BEC. Replying to the suspect message may reach the attacker. See [Anatomy of a suspicious email](#4-anatomy-of-a-suspicious-email).

</details>

### Question 5

A suspicious email passes SPF, DKIM, and DMARC. Which conclusion is MOST accurate?

A. The message cannot be phishing  
B. The sender is guaranteed to be an employee  
C. The email's authentication results do not establish that its content is safe  
D. The attachment has been scanned and found harmless

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Authentication helps establish sending-domain authorization and alignment, not intent or safety. See [Links, domains, and message authentication](#5-links-domains-and-message-authentication).

</details>

### Question 6

A message claims that a confidential report is available only to the first five employees who click a link. Which influence technique is MOST prominent?

A. Scarcity  
B. Tailgating  
C. Lateral movement  
D. Data masking

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A. Scarcity.** Limiting apparent availability creates pressure to act. See [Influence techniques and psychological triggers](#3-influence-techniques-and-psychological-triggers).

</details>

### Question 7

A visitor follows an employee through a badge-controlled door without presenting credentials. What is this called?

A. Smishing  
B. Tailgating  
C. Clone phishing  
D. Quishing

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B. Tailgating.** The attacker exploits another person's authorized physical entry. See [Other social engineering methods](#other-social-engineering-methods).

</details>

### Question 8

Which control MOST directly reduces the risk of credential theft through a counterfeit sign-in website?

A. Requiring longer email signatures  
B. Disabling all email attachments  
C. Using phishing-resistant FIDO2/WebAuthn authentication  
D. Changing the office Wi-Fi name weekly

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** Properly configured phishing-resistant authentication binds the authentication ceremony to the legitimate service, making ordinary credential-harvesting sites less effective. See [Defensive controls: people, process, and technology](#6-defensive-controls-people-process-and-technology).

</details>

**Score guide:** 7–8: strong understanding; 5–6: review missed concepts; 0–4: revisit the lesson and repeat the lab.

## 13. Discussion challenge

A finance employee receives a payment instruction from a genuinely compromised executive mailbox. The message passes email authentication. A colleague also reports a suspicious link but has not opened it. Explain why authentication does not establish safety, what independent payment controls could prevent fraud, and how you would report the second event without claiming a confirmed compromise.

## 14. Study from the Treadmill 🏃

- [Professor Messer — Security+ SY0-701 video course](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/sy0-701-comptia-security-plus-course/) — look for the lessons on phishing and human vectors. Specific video links require verification before publication.

**Active-listening prompts:** How do delivery channels differ from influence techniques? Why is independent verification important even when email authentication passes?

## 15. One-minute recap

- Social engineering exploits human decisions and business processes.
- Phishing can be broad, targeted, executive-focused, or delivered through SMS, voice, or QR codes.
- Pretexting, impersonation, authority, and urgency are distinct but overlapping techniques.
- SPF, DKIM, DMARC, and HTTPS do not guarantee trustworthy content.
- Verify high-risk requests independently; report suspicious messages promptly.
- Separate observed evidence from suspected objectives and unconfirmed compromise.

**Next:** [Day 29 — Malware and Ransomware](day-29-malware-and-ransomware.md).

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.  

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
