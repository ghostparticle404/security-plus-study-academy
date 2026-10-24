[SecurityPlus_Lesson_04_Audit_Logs.md](https://github.com/user-attachments/files/33914981/SecurityPlus_Lesson_04_Audit_Logs.md)
# Day 04 — Audit Logs, Accountability & Incident Timelines

> **CompTIA Security+ SY0-701** · Domain 4: Security Operations · Objectives 4.4 and 4.9  
> **Level:** Beginner · **Study time:** 55–75 minutes  
> **Previous:** [Day 03 — Authentication & Authorization](day-03-authentication-authorization.md)

Yesterday, we learned who may access a system. Today we learn how investigators determine **what happened afterward**. Logs are the receipts of cybersecurity—but a receipt does not necessarily tell you who held the wallet.

## 1. Learning objectives

By the end, you should be able to:

1. Distinguish **events, alerts, and incidents**.
2. Interpret common fields in security logs.
3. Explain the roles of **audit trails, NTP, SIEM, and log correlation**.
4. Build a basic timeline from several log entries.
5. Protect logs from alteration and inappropriate disclosure.
6. Explain why an account name in a log does **not** prove a person's identity.

## 2. The big idea: Security systems keep receipts

The third **A** in AAA is **accounting**: recording activity for review and accountability. An **event** is an observable occurrence (a login). An **alert** flags activity for investigation (many failed logins). An **incident** is an occurrence that actually or potentially jeopardizes security and requires response under the organization's criteria.

| Term | Example | What it means |
|---|---|---|
| Event | One successful login | Something happened |
| Alert | Failed logins followed by success | Something may need investigation |
| Incident | Confirmed unauthorized file export | Respond under incident procedures |

**Memory hook:** Authentication says *“You got in.”* Authorization says *“You may open this.”* Accounting says *“Here's what we recorded.”* 🧾

## 3. Under the hood: Read a log entry

Here is a fictional event:

```text
2026-10-24T02:06:17Z host=fileserver-02 user=paralegal07
src_ip=203.0.113.25 action=DOWNLOAD
resource=/cases/1042/evidence.zip result=SUCCESS
bytes=734003200 session_id=s-8821
```

| Field | Meaning | Investigation question |
|---|---|---|
| Timestamp | Event time; `Z` indicates UTC | Are clocks synchronized? |
| Host | Reporting computer/service | Which system observed this? |
| User | Account associated with event | Was the account compromised? |
| Source IP | Network address observed | Could it be a VPN, proxy, or NAT? |
| Action | Attempted operation | Read, edit, delete, or export? |
| Resource | Target of operation | Is the target sensitive? |
| Result | Outcome | Did the operation succeed? |
| Bytes | Data volume | Is this unusual? |
| Session ID | Correlation identifier | What other actions share this session? |

The example IP is reserved for documentation. A source IP may identify a network connection but **not necessarily an individual**.

### Common log sources

- **Identity provider:** Login attempts, MFA challenges, token activity.
- **VPN:** Remote connections, users, assigned addresses.
- **Operating system:** Account changes, service activity, logons.
- **EDR/Sysmon:** Processes, command lines, parent-child relationships.
- **Firewall:** Allowed/denied traffic, ports, network endpoints.
- **DNS:** Domain queries.
- **Cloud applications:** File downloads, sharing, permission changes.
- **Email security:** Message delivery and filtering decisions.

**Technical detail:** Windows Security Event ID **4624** commonly records a successful logon, and **4625** a failed logon when auditing is enabled. **Sysmon Event ID 1** is process creation when configured. These are *different event providers*; don't mix their IDs.

## 4. Time, NTP, and correlation

Imagine these records from different systems:

```text
02:03:10Z  VPN     admin LOGIN_SUCCESS
02:05:02Z  FILES   admin EXPORT_STARTED
02:06:17Z  FILES   admin DOWNLOAD 734003200 bytes
02:08:41Z  VPN     admin DISCONNECT
```

This suggests a file transfer during a VPN session, but does **not** prove the administrator personally performed it.

**NTP (Network Time Protocol)** helps synchronize clocks. Without it, a five-minute drift can make a later event appear to occur before its cause. Preserve timestamps with their time zones, and note clock uncertainty when constructing timelines.

**Correlation** connects events using shared timestamps, identities, devices, source addresses, or session IDs. A **SIEM (Security Information and Event Management)** system aggregates, searches, and correlates logs and can generate alerts. It cannot magically repair missing evidence or identify a human from a username.

**Memory hook:** A timeline with unsynchronized clocks is a mystery novel with the chapters shuffled. 📚

## 5. Protecting audit evidence

A useful log must be available, sufficiently complete, and reasonably trustworthy.

- **Centralize logs** so a compromised endpoint cannot easily erase the only copy.
- **Restrict access** to read, edit, and delete records.
- **Protect integrity** with immutable or append-only storage where appropriate.
- **Synchronize clocks** and document time zones.
- **Set retention rules** based on investigation needs, law, and privacy requirements.
- **Monitor logging failures** and test retrieval.
- **Avoid recording secrets** such as raw passwords or session tokens.

Logs are not perfect proof. Stolen credentials, shared accounts, session hijacking, automation, inaccurate clocks, and tampering can all mislead investigators.

### CIA connection

- **Confidentiality:** Logs themselves may contain sensitive information and need access controls.
- **Integrity:** Tampering can invalidate evidence.
- **Availability:** Logs must remain retrievable during an investigation.

## 6. Case study: The midnight law-firm export

A legal clinic receives an alert: **600 case files exported at 02:06 UTC** under the `admin` account. The administrator says they were asleep.

**Don't accuse anyone based on one log.** Investigate systematically:

1. Confirm the export occurred and identify the affected files.
2. Check identity-provider login and MFA records.
3. Examine VPN/session logs and endpoint process telemetry.
4. Determine whether a scheduled job, backup, or approved migration explains the activity.
5. Preserve relevant evidence according to incident-response procedures.
6. Escalate and contain based on verified risk, not guesswork.

| Clue | Possible explanation | Evidence to seek |
|---|---|---|
| Overnight login | Legitimate late work or compromise | Schedule, login device, MFA logs |
| Large export | Backup, migration, or theft | Change ticket, destination, files |
| `admin` account | Human, automation, or stolen session | Process and session records |

**Exam tip:** If a question asks for the **BEST next step**, choose the action that validates the suspicious event and preserves evidence rather than assuming the conclusion.

## 7. Guided notes: Write first, reveal later

Complete these prompts from memory:

| Prompt | Your answer |
|---|---|
| Event vs. alert vs. incident? | _Write here_ |
| Six useful log fields? | _Write here_ |
| Why synchronize clocks? | _Write here_ |
| What does a SIEM do? | _Write here_ |
| Why centralize logs? | _Write here_ |
| Why can't a username prove a person's identity? | _Write here_ |

<details>
<summary><strong>Reveal completed notes table (sample answers)</strong></summary>

| Prompt | Model answer |
|---|---|
| Event / alert / incident | An event is an occurrence; an alert flags activity to examine; an incident meets criteria for a security response. |
| Six log fields | Timestamp, account, source IP, action, resource, result. Session ID and host are also useful. |
| Clock synchronization | Keeps events comparable across systems and reduces timeline errors. |
| SIEM | Aggregates, searches, analyzes, and correlates events; supports alerting and investigations. |
| Centralization | Reduces evidence loss if an endpoint is compromised and helps enforce access and retention. |
| Username vs. person | Accounts may be stolen, shared, automated, or associated with hijacked sessions. Corroboration is required. |

</details>

## 8. Hands-on lab: Investigate a tiny log file

**Time:** 15–20 minutes. Works in macOS Terminal or Linux. This is fictional, safe data.

```bash
cat > security-day04.log <<'EOF'
2026-10-24T02:01:00Z user=admin action=LOGIN result=FAIL src=198.51.100.8
2026-10-24T02:02:00Z user=admin action=LOGIN result=FAIL src=198.51.100.8
2026-10-24T02:03:00Z user=admin action=LOGIN result=SUCCESS src=198.51.100.8
2026-10-24T02:06:00Z user=admin action=EXPORT result=SUCCESS files=600
2026-10-24T02:07:00Z user=analyst action=LOGIN result=SUCCESS src=192.0.2.44
EOF
```

Try these commands, one at a time:

```bash
cat security-day04.log                   # show all events
grep 'result=FAIL' security-day04.log    # failed logins
grep 'user=admin' security-day04.log    # admin activity
grep -c 'result=FAIL' security-day04.log # count failed logins
nl -ba security-day04.log               # add line numbers
```

**Questions:** How many failures preceded success? How many minutes passed between login and export? What other logs would you request?

<details>
<summary><strong>Reveal lab answers</strong></summary>

**Two** failures; **three minutes** from the successful login at 02:03 to the export at 02:06 UTC. Request identity/MFA logs, VPN events, endpoint/process telemetry, and any approved export or backup schedule. Nothing here proves the named administrator personally acted.

</details>

## 9. Eight original exam-style questions

These are educational practice items, not real CompTIA exam questions. Choose the **BEST** answer before expanding each explanation.

### Q1
Two servers record the same incident with conflicting times. What MOST improves future timeline accuracy?

A. Full-disk encryption  
B. NTP synchronization  
C. Load balancing  
D. Password rotation

<details><summary>Reveal answer and explanation</summary>

**B — NTP.** It synchronizes clocks. Encryption protects confidentiality; load balancing distributes traffic; password rotation does not correct timestamps.
</details>

### Q2
Analysts must correlate VPN logins with file-server downloads. Which tool BEST supports this?

A. SIEM  
B. UPS  
C. Password manager  
D. DLP classification label

<details><summary>Reveal answer and explanation</summary>

**A — SIEM.** It centralizes and correlates events. A UPS supplies backup power; a password manager manages credentials; a DLP label classifies data.
</details>

### Q3
What BEST reduces the risk that an attacker who compromises a server can erase its only audit records?

A. Disable failed-login logging  
B. Keep logs only on that server  
C. Forward logs to restricted central storage  
D. Give everyone log-delete rights

<details><summary>Reveal answer and explanation</summary>

**C.** Centralized restricted storage reduces dependence on the compromised host. Immutable storage can add protection. A, B, and D weaken evidence preservation.
</details>

### Q4
A record states `user=admin action=DELETE result=SUCCESS`. What can be concluded MOST defensibly?

A. The administrator personally deleted the file  
B. The event was associated with the admin account/session  
C. The action was authorized  
D. The record is useless

<details><summary>Reveal answer and explanation</summary>

**B.** Account attribution is not human attribution. A and C assume facts not established; D ignores useful evidence.
</details>

### Q5
Which source BEST helps identify a suspicious process's parent process and command line?

A. EDR or Sysmon telemetry  
B. Door access log  
C. UPS status  
D. Certificate calendar

<details><summary>Reveal answer and explanation</summary>

**A.** Endpoint process telemetry may capture process trees and command lines. The other sources do not directly describe process execution.
</details>

### Q6
Twenty failed logins followed by a success trigger a notification. That notification is BEST described as:

A. Confirmed breach  
B. Alert  
C. Backup  
D. Encryption key

<details><summary>Reveal answer and explanation</summary>

**B — Alert.** The pattern warrants investigation but does not by itself confirm a compromise.
</details>

### Q7
Which field MOST directly links actions within the same authenticated session?

A. Session ID  
B. File extension  
C. Screen resolution  
D. CPU temperature

<details><summary>Reveal answer and explanation</summary>

**A — Session ID.** It can connect related events, although it may be missing or stolen. Other fields are not session correlation identifiers.
</details>

### Q8
An organization keeps detailed authentication logs indefinitely, including sensitive tokens. What is the BEST improvement?

A. Publish logs publicly  
B. Stop all logging  
C. Apply retention limits, access controls, and sensitive-data minimization  
D. Email all logs to employees

<details><summary>Reveal answer and explanation</summary>

**C.** Appropriate retention and minimization balance security, privacy, and investigation needs. The other choices increase exposure or eliminate visibility.
</details>

### Score guide

- **7–8 correct:** Explain why the distractors were wrong.
- **5–6 correct:** Review correlation, time, and attribution.
- **0–4 correct:** Revisit Sections 2–5 and repeat tomorrow.

## 10. Discussion challenge

A law firm's logs show an `admin` login at 02:03 UTC and a 600-file export at 02:06 UTC. The administrator says they were asleep.

**What three independent sources would you check next?** What findings might suggest a legitimate backup rather than theft? Why is it unsafe to name a culprit from the account name alone?

## 11. Study from the Treadmill 🏃

**Recommended:** [Professor Messer's free Security+ SY0-701 course](https://www.professormesser.com/security-plus/sy0-701/). Look for lessons on **log data**, **security monitoring**, and **SIEM**.

Optional video search: [Professor Messer SY0-701 log data](https://www.youtube.com/results?search_query=Professor+Messer+SY0-701+log+data).

**Listen for:** (1) Which sources generate security logs? (2) How does a SIEM correlate events? (3) Why does time synchronization matter?

## 12. One-minute recap

- **Event:** Something happened.
- **Alert:** Something deserves investigation.
- **Incident:** A security occurrence requiring response.
- **NTP:** Keeps clocks aligned.
- **SIEM:** Correlates records from many sources.
- **Centralized, protected logs:** Support investigation and reduce tampering risk.
- **Attribution:** A username is a clue, not proof of who acted.

**Next:** [Day 05 — Non-repudiation & Digital Signatures](day-05-nonrepudiation-digital-signatures.md).

---

*Independent educational resource. Not affiliated with or endorsed by CompTIA. Security+ is a trademark of CompTIA.*
