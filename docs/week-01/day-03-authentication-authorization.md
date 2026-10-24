[day-03-authentication-authorization.md](https://github.com/user-attachments/files/33915124/day-03-authentication-authorization.md)
# Day 03 — Authentication, Authorization & Least Privilege

> **CompTIA Security+ SY0-701** · Domain 4, Objective 4.6 (Identity and Access Management) · Related: Domain 1, Objective 1.2  
> **Level:** Beginner · **Time:** 50–70 minutes  
> **Previous:** [Day 02 — Security Controls](day-02-security-controls.md)

Welcome back! Day 1 explored **what** security protects (confidentiality, integrity, availability). Day 2 introduced **how** organizations protect it (security controls). Today: **Who gets access, and what can they do?**

## 1. Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish **identification, authentication, authorization, and accounting**.
2. Identify authentication factors and explain what makes authentication *multifactor*.
3. Apply **least privilege**, **need to know**, and **separation of duties**.
4. Compare introductory **RBAC, ABAC, DAC, and MAC** access models.
5. Interpret realistic access-control scenarios and answer exam-style questions.

## 2. The big idea: You're going to a concert 🎸

Imagine arriving at a concert venue:

- **Identification:** You claim, “I'm Alex.”
- **Authentication (AuthN):** You prove that claim with an acceptable credential.
- **Authorization (AuthZ):** Your ticket grants entry to general admission, **not** the backstage area.
- **Accounting:** The ticket scan and security logs record the activity.

The classic **AAA framework** stands for **Authentication, Authorization, Accounting**. Identification happens before or during authentication but is not an additional “A.”

**Memory hook:** *Who are you? What can you do? What happened?*

**Why it matters:** A user can be genuinely authenticated and still be forbidden from opening a particular file. A successful login is not a master key.

## 3. Authentication: Proving identity

An identity claim alone isn't trustworthy. Authentication checks evidence supporting the claim.

### The three classic factor categories

| Factor | Meaning | Examples |
| --- | --- | --- |
| **Something you know** | A memorized secret | Password, PIN |
| **Something you have** | A possessed authenticator | Hardware security key, registered authenticator device |
| **Something you are** | A biometric characteristic | Fingerprint, facial recognition |

**Multifactor authentication (MFA)** uses **at least two different factor categories**.

| Combination | MFA? | Why? |
| --- | --- | --- |
| Password + PIN | **No** | Know + know |
| Password + hardware security key | **Yes** | Know + have |
| Fingerprint + face scan | **No** | Are + are |
| Password + code generated on a registered authenticator device | **Typically yes** | Know + have, when device possession is validated |

Two prompts do **not** automatically mean two factors. That's an exam favorite.

### Under the hood: Passkeys and phishing resistance

Passkeys commonly use **public-key cryptography**. The authenticator holds a private key; the service verifies a signed challenge with the corresponding public key. A local PIN or biometric check may unlock use of the credential. The website does not need to receive your fingerprint.

When correctly implemented, FIDO2/WebAuthn authentication binds credentials to the legitimate service, making it resistant to many phishing attacks.

**MFA is powerful, not magical.** Some methods remain vulnerable to repeated push approval requests (*MFA fatigue*), adversary-in-the-middle phishing, or stolen authenticated session tokens. Prefer phishing-resistant authentication for high-risk accounts.

## 4. Authorization: What are you allowed to do?

Once identity is verified, the system checks permissions for a **subject** (user or service) requesting an **action** (read, write, delete) on an **object** (file, application, database record).

Consider a law firm:

- A **receptionist** manages appointments.
- A **paralegal** reads assigned case files.
- A **partner** approves certain filings.
- A **payroll specialist** accesses compensation records.

Logging in successfully should not grant all four roles' privileges.

### Least privilege

**Least privilege** means granting only the permissions needed to complete an assigned task, for no longer than needed. If a receptionist's account is stolen, it shouldn't allow the attacker to administer servers.

Related ideas:

- **Need to know:** A legitimate business reason is required to access particular sensitive information.
- **Separation of duties:** Split sensitive tasks across people—for example, one creates a vendor payment and another approves it.
- **Privileged Access Management (PAM):** Controls and monitors elevated administrative access.
- **Just-in-time (JIT) access:** Provides temporary privileges for a specific approved task, then removes them.

**Memory hook:** Give people keys to the rooms they work in, not a master key to the entire building. Even the office plant doesn't need Domain Admin. 🪴

## 5. Four access-control models

| Model | Who/what determines access? | Example |
| --- | --- | --- |
| **RBAC** — Role-Based | Assigned job roles | Paralegals have a defined case-management role |
| **ABAC** — Attribute-Based | Attributes of users, resources, actions, or context | Allow if assigned to case **and** using managed device |
| **DAC** — Discretionary | Resource owner grants access | Document owner shares a folder |
| **MAC** — Mandatory | Central security labels and rules | Classified records controlled by clearance and labels |

### Under the hood: An ABAC-style policy

```text
Request: READ case-1042.pdf
User role: paralegal
Assigned cases: [1042, 1088]
Device managed: true

ALLOW if:
  role == paralegal
  AND requested_case in assigned_cases
  AND device_managed == true
  AND action == READ
```

This uses several **attributes**. A simple RBAC decision might check only whether the user belongs to the paralegal role.

**Exam tip:** If the question emphasizes *job title*, think RBAC. If it evaluates *contextual attributes* such as case assignment, location, device state, or time, think ABAC.

## 6. Accounting: What happened?

Accounting provides records of activity for investigation, troubleshooting, and accountability.

```text
2026-10-24T15:02:11Z user=paralegal07
source=10.0.4.21 action=READ
resource=case-1042.pdf result=ALLOW
```

Useful fields include **timestamp/time zone**, account, source, action, target, and result. Centralized logs and synchronized clocks help investigators correlate events.

**Important limitation:** A username in a log does not prove that a particular human performed an action. Credentials can be stolen, sessions hijacked, accounts shared, and logs altered. Corroboration matters.

## 7. Real-world case: The overprivileged contractor

A contractor is hired to update a law firm's website for two weeks. To save time, IT grants permanent domain administrator rights. The contractor later falls for a phishing message, and an attacker uses the account to export unrelated client records.

| Problem | Better safeguard | Security benefit |
| --- | --- | --- |
| Password theft | Phishing-resistant MFA | Stronger authentication |
| Excessive permissions | Scoped website-management role | Least privilege |
| Permanent admin access | JIT privilege with expiration | Reduced exposure window |
| Unnoticed file exports | Centralized audit logs and alerts | Detection and investigation |
| Access after contract ends | Scheduled account deprovisioning | Removes unnecessary access |

**Connect the dots:** The attacker threatened **confidentiality** (Day 1). MFA, restricted permissions, and logging are **security controls** (Day 2). The underlying failure involved **authorization** (today).

## 8. Guided notes — Write first, reveal later

Fill in your own answers before expanding the model table.

| Prompt | Your answer |
| --- | --- |
| Difference between identification, authentication, and authorization? | _Write here._ |
| What does AAA stand for? | _Write here._ |
| Give one valid and one invalid MFA combination. | _Write here._ |
| Define least privilege and need to know. | _Write here._ |
| RBAC vs. ABAC? | _Write here._ |
| Why can't a username in a log identify the human with certainty? | _Write here._ |

<details>
<summary><strong>Reveal completed notes table — sample answers</strong></summary>

| Prompt | Model answer |
| --- | --- |
| Identity vs. AuthN vs. AuthZ | Identification claims an identity; authentication verifies it; authorization decides allowed actions. |
| AAA | Authentication, Authorization, Accounting. |
| MFA | Valid: password + hardware security key (know + have). Invalid: password + PIN (know + know). |
| Least privilege / need to know | Grant only necessary permissions; allow sensitive information access only for legitimate job requirements. |
| RBAC / ABAC | RBAC grants permissions based on roles; ABAC evaluates attributes and context. |
| Limits of logs | Credentials or sessions may be stolen, accounts shared, and logs inaccurate or altered. |

</details>

## 9. Hands-on exercise: Design a permissions matrix

**Time:** 15–20 minutes · **Tools:** Notebook or spreadsheet

A fictional firm has four roles: **Receptionist**, **Paralegal**, **Partner**, and **IT Administrator**. Resources: **Calendar**, **Assigned Case Files**, **Payroll**, and **Server Configuration**.

Create a matrix using **Read**, **Edit**, or **None**. Explain which access should be temporary, which needs approval, and what should be logged.

<details>
<summary><strong>Reveal example permissions matrix</strong></summary>

| Role | Calendar | Assigned Case Files | Payroll | Server Configuration |
| --- | --- | --- | --- | --- |
| Receptionist | Edit | None | None | None |
| Paralegal | Read | Edit (assigned only) | None | None |
| Partner | Read | Edit (authorized cases) | None by default | None |
| IT Administrator | Limited support | None by default | None | Edit via approved privileged workflow |

This is an illustration, not a universal policy. Real firms may have separate payroll staff, emergency access, and audited IT support exceptions.

</details>

## 10. Eight original Security+-style questions

These are **original practice questions**, not actual CompTIA exam items. Choose the **BEST** answer before revealing each explanation.

### Q1
A user successfully logs in but receives “Access denied” when opening the finance folder. What MOST directly explains the denial?

A. Identification  
B. Authorization  
C. Authentication  
D. Accounting

<details><summary>Reveal answer and explanation</summary>

**B — Authorization.** Login already succeeded. The denial concerns permissions. Identification is the claim, authentication verifies it, and accounting records activity.

</details>

### Q2
Which combination is MFA?

A. Password + PIN  
B. Fingerprint + face scan  
C. Password + hardware security key  
D. Two different passwords

<details><summary>Reveal answer and explanation</summary>

**C.** Knowledge + possession are different factor categories. A and D use knowledge twice; B uses inherence twice.

</details>

### Q3
A contractor needs administrator privileges for one approved maintenance task. What BEST limits unnecessary exposure?

A. Permanent administrator membership  
B. Shared administrator password  
C. Just-in-time privileged access  
D. Disabling audit logs

<details><summary>Reveal answer and explanation</summary>

**C.** JIT grants elevation when needed and removes it afterward. The alternatives expand exposure or weaken accountability.

</details>

### Q4
A system grants access based on role, case assignment, and whether the device is managed. Which model BEST fits?

A. DAC  
B. MAC  
C. ABAC  
D. RBAC only

<details><summary>Reveal answer and explanation</summary>

**C — ABAC.** It evaluates several attributes, not just the user's role. DAC is owner-controlled and MAC relies on centrally enforced labels.

</details>

### Q5
Which demonstrates separation of duties?

A. One employee creates and approves payments  
B. A second authorized employee approves payments created by another  
C. All employees have admin rights  
D. Payroll processing disables logging

<details><summary>Reveal answer and explanation</summary>

**B.** Separating initiation and approval reduces opportunities for unilateral fraud or error. The other choices weaken controls.

</details>

### Q6
What principle BEST reduces damage if a receptionist's account is compromised?

A. Least privilege  
B. Non-repudiation  
C. Maximum availability  
D. Key escrow

<details><summary>Reveal answer and explanation</summary>

**A.** Restricting the account to necessary actions limits what an attacker can do. The other concepts do not directly reduce excessive permissions.

</details>

### Q7
A login system asks for a password followed by a second password. What is MOST accurate?

A. It is phishing-resistant MFA  
B. It uses two steps but one factor category  
C. It combines possession and inherence  
D. It eliminates credential theft

<details><summary>Reveal answer and explanation</summary>

**B.** Both passwords are knowledge factors. Multiple prompts are not necessarily multifactor authentication.

</details>

### Q8
A log records an export under an administrator's username. What conclusion is MOST defensible?

A. The administrator personally exported the data  
B. The log is irrelevant  
C. The account/session was associated with the export, but the actor needs corroboration  
D. Authorization was configured correctly

<details><summary>Reveal answer and explanation</summary>

**C.** Logs attribute actions to accounts or sessions; stolen credentials or hijacked sessions may obscure the real actor. Investigate supporting evidence.

</details>

### Score guide

- **7–8 correct:** Strong understanding. Explain why the distractors are wrong.
- **5–6 correct:** Review MFA categories and access-control models.
- **0–4 correct:** Revisit Sections 3–5 and retry tomorrow.

## 11. Discussion challenge

> A hospital contractor successfully authenticates with MFA but can read patient records unrelated to the assigned project. Which part of AAA worked, which part failed, and what **two changes** would you recommend? Does MFA alone solve the problem?

Use the CIA triad and the security-control categories from earlier lessons to support your reasoning.

## 12. Study from the Treadmill 🏃 + one-minute recap

**Watch these exact lessons:**

1. [Professor Messer — Authentication, Authorization, and Accounting (9:03)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/authentication-authorization-and-accounting-sy0-701/) — start with the AAA framework.
2. [Professor Messer — Multifactor Authentication (4:29)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/multifactor-authentication-sy0-701/) — identify different factors.
3. [Professor Messer — Access Controls (7:49)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/access-controls-sy0-701/) — connect least privilege to permissions.

**Active listening:** What is the difference between authentication and authorization? Why aren't two passwords MFA? How do access rules limit a compromised account?

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
