# Day 23 — IDS, IPS, and Network Monitoring

**Week 05: Network Security**  
**Canonical path:** `docs/week-05/day-23-ids-ips-and-network-monitoring.md`  
**Prerequisites:** [Day 20 — Routing, Switching, and Packet Flow](../week-04/day-20-routing-switching-and-packet-flow.md), [Day 21 — Segmentation and VLANs](day-21-segmentation-and-vlans.md), and [Day 22 — Firewalls and Filtering](day-22-firewalls-and-filtering.md)  
**Security+ SY0-701 alignment:** Domain 3.0, **Objective 3.2** (security infrastructure, including IDS/IPS); Domain 4.0, **Objective 4.4** (security alerting and monitoring). Related concepts support **Objective 4.3** (vulnerability management) and **Objective 4.9** (data sources for investigations). These are topical alignments, not quotations of CompTIA's objectives.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain why firewalls alone do not provide sufficient visibility into suspicious network behavior.
2. Distinguish **intrusion detection systems (IDS)** from **intrusion prevention systems (IPS)**, including their placement and responses.
3. Compare **network-based** and **host-based** monitoring and **signature-based**, **anomaly-based**, and **behavior-based** detection.
4. Identify the roles of packet captures, flow records, firewall logs, DNS logs, endpoint telemetry, and SIEM correlation.
5. Interpret a sample IDS/IPS alert using time, source, destination, signature, severity, action, and corroborating evidence.
6. Recognize false positives, false negatives, tuning trade-offs, and safe escalation procedures.

## Why network monitoring matters

On Day 21, you separated a small office into VLANs to reduce unnecessary connectivity. On Day 22, you used firewall rules to control which traffic can cross those boundaries. But what happens when traffic is **allowed** by policy and still turns out to be dangerous?

An employee's workstation might contact an approved web service while also sending an unusual volume of data. A compromised device might repeatedly try to reach other machines in its own VLAN, never crossing the perimeter firewall. A malicious request may travel over an otherwise legitimate TCP 443 connection.

Security teams therefore need both **controls that enforce policy** and **telemetry that helps explain what happened**. Monitoring does not automatically prove an intrusion; it provides evidence for investigating one.

> **Core distinction:** A firewall decides whether traffic is permitted under policy. An IDS detects potentially suspicious activity and alerts. An IPS can detect and actively block or disrupt traffic, subject to its configuration and visibility.

## 1. Detection versus prevention

An **intrusion detection system (IDS)** examines events or traffic for patterns that may indicate attacks, policy violations, or suspicious behavior. Its primary output is an **alert** or logged finding. An IDS is generally **out of band**: it observes a copy of traffic or host activity rather than sitting in the forwarding path.

An **intrusion prevention system (IPS)** performs similar analysis but can take **enforcement actions**—for example, dropping packets, rejecting a connection, or blocking traffic associated with a rule. A network IPS is commonly **inline**, meaning traffic passes through it before reaching its destination.

| Characteristic | IDS | IPS |
|---|---|---|
| Primary purpose | Detect and report | Detect and prevent or disrupt |
| Typical network placement | Out of band, monitoring a copy | Inline with traffic |
| Typical action | Alert, log, notify | Drop, reject, reset, block, and/or alert |
| Effect on legitimate traffic | Does not normally block the observed flow | Can interrupt legitimate traffic if detection is wrong |
| Key operational risk | Missed or ignored alerts | False-positive blocking; throughput and availability impact |

**Important nuance:** Product names do not guarantee behavior. An IPS can be configured to alert without blocking, and a detection platform can trigger a separate firewall or endpoint action through automation. On an exam, prioritize the **described function and deployment**, not just the marketing label.

### Passive versus inline placement

```text
Passive monitoring (IDS)

Internet ---- Firewall ---- Switch ---- Workstations
                              |
                       mirrored traffic
                              |
                         IDS sensor

Inline inspection (IPS)

Internet ---- Firewall ---- IPS ---- Switch ---- Workstations
                         |
                    allow or block
```

A **SPAN/mirror port** copies selected switch traffic to a sensor. A **network TAP** provides a dedicated way to observe traffic. Both can feed an IDS without making it the forwarding device. Mirroring can lose packets when oversubscribed, and encrypted sessions limit payload inspection.

An inline IPS can prevent traffic before delivery, but it must be sized and deployed with care. If it fails, the organization's **fail-open** or **fail-closed** design determines whether traffic is permitted or interrupted. Neither is universally correct: the decision depends on the availability and security requirements of that link.

<details markdown="1">
<summary>Check your understanding: IDS or IPS?</summary>

A sensor receives mirrored packets and sends an alert about repeated login attempts. It does not stop the attempts. This is **IDS behavior**. If an inline device detects the same pattern and drops matching traffic, that is **IPS behavior**.

</details>

## 2. Network-based versus host-based detection

**Network-based IDS/IPS (NIDS/NIPS)** inspects traffic visible at a network observation or enforcement point. It can identify scanning, suspicious protocol patterns, known exploit attempts, and some command-and-control behavior, depending on rules and visibility.

**Host-based IDS/IPS (HIDS/HIPS)** runs on or monitors a particular endpoint or server. It may inspect operating-system events, processes, file integrity, local logs, and host network activity. A host prevention component can stop certain actions locally.

| Question | Network-based monitoring | Host-based monitoring |
|---|---|---|
| What does it observe? | Traffic crossing a monitored network point | Events and activity on a specific system |
| Can it see inside an encrypted application session? | Usually not without appropriate decryption/visibility | Sometimes, through endpoint/application telemetry |
| Can it see traffic that never crosses its sensor? | No | Potentially, if it occurs on the monitored host |
| Can it identify local process activity? | Not directly | Often yes, if the agent collects it |
| Main limitation | Network blind spots, encryption, packet loss | Requires host coverage, agent health, and appropriate permissions |

A modern **endpoint detection and response (EDR)** platform overlaps with some host-based detection functions but typically adds richer endpoint telemetry, investigation, and response capabilities. **NDR (network detection and response)** similarly extends network monitoring with analytics and response workflows. These are related categories, not synonyms for every IDS product.

### Example: traffic inside the same VLAN

If two employee workstations communicate through a switch, their traffic may never pass the perimeter firewall or a sensor placed only at the internet edge. An endpoint agent or an appropriately positioned internal sensor could detect suspicious activity that the perimeter sensor misses.

**Design principle:** Sensor placement determines visibility. A perfectly configured signature cannot detect traffic that the sensor never sees.

## 3. Detection methods: signatures, anomalies, and behavior

### Signature-based detection

A **signature** is a defined pattern associated with known malicious or suspicious activity. A rule might match a distinctive request pattern, a protocol anomaly, or a sequence of bytes.

**Strength:** Can be precise for well-understood activity.  
**Limitation:** May miss new, modified, or evasive attacks; requires current, relevant rules.

### Anomaly-based detection

An **anomaly detector** compares observations with an expected baseline and flags meaningful deviations. For example, a printer that usually contacts one print server suddenly initiates hundreds of outbound connections.

**Strength:** Can highlight unfamiliar or unexpected activity.  
**Limitation:** Legitimate changes can look abnormal; baselines must reflect normal variation.

### Behavior-based detection

**Behavior-based detection** looks for suspicious actions or sequences, such as repeated authentication failures followed by a successful login and unusual data access. Products may combine behavior analytics, signatures, statistics, and threat intelligence.

| Method | Illustrative trigger | What an analyst must ask |
|---|---|---|
| Signature | Request matches a known exploit pattern | Was it a real exploit, a harmless test, or an imprecise match? |
| Anomaly | Unusual outbound data volume | Is this a scheduled backup or unauthorized transfer? |
| Behavior | Host scans many internal ports after a suspicious process starts | Is this approved inventory software or lateral-movement activity? |

**Exam trap:** “Anomaly” means deviation from a baseline, **not proof of maliciousness**. A signature match also requires context; alerts are hypotheses to investigate.

## 4. What is network telemetry?

**Telemetry** is recorded information about system or network activity. It can be collected, retained, searched, and correlated to investigate events and monitor security controls.

| Data source | What it can tell you | What it generally cannot prove alone |
|---|---|---|
| **Packet capture (PCAP)** | Packet headers, timing, protocol details, and unencrypted payloads | User intent; encrypted application contents without decryption |
| **NetFlow/IPFIX or other flow records** | Who communicated with whom, when, duration, bytes, ports (fields vary) | Full payload contents |
| **Firewall logs** | Rule/action, source/destination, ports, sometimes session metadata | That an allowed connection was benign |
| **DNS logs** | Domain queries, clients, timestamps, responses if collected | That a queried domain was actually visited or a connection succeeded |
| **IDS/IPS alerts** | Detection rule, event metadata, possible attack indicator, enforcement action | That a compromise occurred |
| **Endpoint/EDR telemetry** | Processes, file activity, host connections, security events | Everything happening on unmonitored devices |
| **Authentication logs** | Login attempts, account, result, and sometimes device/location | That a login was authorized by the account owner |

### Packet capture versus flow records

A **packet capture** is detailed but can be large, sensitive, and difficult to retain at scale. **Flow records** are more compact summaries of communications, useful for spotting trends such as repeated connections or unusually large transfers. Flow data is not a substitute for payload inspection.

### Time synchronization matters

A firewall may record an event at `10:02:05`, while an endpoint records it at `10:07:05` because its clock is wrong. Correlation becomes misleading. Use consistent timestamps and **NTP** or another appropriate time-synchronization mechanism, and record the time zone when reporting.

### SIEM: bringing the evidence together

A **security information and event management (SIEM)** platform ingests logs and alerts from multiple sources, normalizes or parses fields, supports searches, and correlates events. It may generate new alerts based on combinations of evidence.

```text
Firewall logs ------\
DNS logs ------------+--> Collection / parsing --> SIEM --> Alert / triage
IDS alerts ----------+
Endpoint telemetry --/
```

A SIEM does **not** automatically make its inputs accurate or complete. Missing logs, poor parsing, unsynchronized clocks, and weak correlation rules can all affect the result.

<details markdown="1">
<summary>Reveal: Which data source should you use?</summary>

If you need to determine **how many bytes moved between two IP addresses**, flow records may be enough. If you need to inspect the **actual unencrypted HTTP request**, a packet capture may be more useful. If you need to know **which local process initiated a connection**, endpoint telemetry is usually the stronger source.

</details>

## 5. Anatomy of an IDS/IPS alert

Consider this **fictional training alert** from our small-office network. It is illustrative, not actual sensor output.

```text
Timestamp:   2026-11-23T14:22:08Z
Sensor:      internal-monitor-01
Signature:   Possible internal TCP port scan
Severity:    Medium
Source:      192.168.10.45:51544    (Employee VLAN)
Destination: 192.168.20.10:445      (Finance VLAN)
Protocol:    TCP
Action:      Alert (not blocked)
Count:       34 related connection attempts in 60 seconds
```

Read the alert in stages:

1. **When?** The timestamp ends in `Z`, indicating UTC. Check other systems' clocks before correlating events.
2. **Where?** The source is in the employee network; the destination is in the finance network. Refer to the VLAN boundaries from Day 21.
3. **What?** The sensor reports repeated TCP connection attempts associated with a possible scan. Port `445/TCP` is commonly used for SMB file sharing.
4. **How certain?** “Possible” and “Medium” are classifications from a detection rule, not proof of a successful attack.
5. **Was it stopped?** **Action: Alert** means this particular sensor did not block the traffic. A separate firewall may still have denied it.
6. **What next?** Correlate firewall decisions, endpoint process activity, authorized scan schedules, and other destination attempts.

### Severity versus priority

**Severity** often describes a rule's estimated impact or threat level. **Priority** is an operational decision about which case to investigate first. A medium-severity alert involving a critical finance server might deserve prompt attention; a high-severity signature from a sanctioned test may be lower priority after verification.

### An alert is not an incident verdict

The same alert could result from a compromised workstation, an authorized vulnerability scan, misconfigured inventory software, or noisy benign traffic. Investigation distinguishes these possibilities.

## 6. Alert interpretation: a guided classification exercise

Use the six-zone office design from Day 21: Employees (`192.168.10.0/24`), Finance (`192.168.20.0/24`), Printers (`192.168.30.0/24`), Guests (`192.168.40.0/24`), Cameras (`192.168.50.0/24`), and Management (`192.168.60.0/24`). Assume the Day 22 firewall policy denies unnecessary inter-VLAN connections.

The following alerts are **fictional training examples**. For each, classify the detection, identify what it does and does not establish, and choose a sensible next step.

| ID | Observation | Initial question |
|---|---|---|
| A | Guest device `192.168.40.22` repeatedly attempts TCP 445 to Finance `192.168.20.10`; firewall denies each attempt | Is this misconfiguration, scanning, or compromise? |
| B | Printer `192.168.30.25` uploads far more data than its usual baseline to an external address | Is this an approved service or anomalous exfiltration? |
| C | Employee workstation `192.168.10.45` triggers a known exploit signature; IPS action is `drop` | Did prevention occur, and was there any successful activity? |
| D | Management laptop `192.168.60.12` scans multiple internal hosts during a documented maintenance window | Does authorized context explain the alert? |

<details markdown="1">
<summary>Reveal classification and investigation guidance</summary>

**A — Suspicious denied attempts.** The firewall's denial shows the specified attempts were blocked at that enforcement point, not that the source is clean. Check whether the guest device is authorized and whether other attempts occurred.

**B — Anomaly-based finding.** Unusual volume merits checking destination reputation, vendor update schedules, flow history, and the device's permitted network behavior. Volume alone does not prove exfiltration.

**C — Signature-based detection with prevention.** `Drop` indicates the IPS reports blocking matching traffic. Verify the action and check endpoint and surrounding traffic for any other successful exploit path.

**D — Likely authorized activity, pending verification.** Confirm the maintenance ticket, operator, timing, source, and scan scope before closing or tuning the alert. Do not suppress all future scans blindly.

</details>

### Example investigation workflow

```text
Alert received
      |
Verify time, asset, rule, and sensor action
      |
Correlate firewall + DNS/flow + endpoint evidence
      |
Is there a credible security concern?
   /                     \
 Yes                     No / insufficient evidence
  |                             |
Escalate under IR plan     Document, tune if justified,
  |                        or continue monitoring
Preserve evidence and record decisions
```

The goal is **triage**: decide what the evidence supports, what is missing, and what action is justified. Follow organizational incident-response procedures rather than improvising intrusive tests on production systems.

## 7. False positives, false negatives, and tuning

A **true positive** is an alert on genuinely malicious or policy-violating activity. A **false positive** is an alert on benign activity. A **false negative** is suspicious or malicious activity that the detection fails to flag. A **true negative** is benign activity that is correctly not flagged.

| Reality / Detection | Alert generated | No alert generated |
|---|---|---|
| Malicious activity | True positive | False negative |
| Benign activity | False positive | True negative |

**Tuning** changes rules, thresholds, exceptions, or baselines to improve usefulness. It must balance missed attacks against analyst overload and accidental blocking.

- Too many false positives create **alert fatigue**, which increases the chance that analysts miss important findings.
- Overly broad suppression may reduce noise but create **blind spots**.
- A prevention rule that blocks valid business traffic can create an **availability incident**.
- An alert threshold should reflect asset criticality, expected activity, and the cost of error.

**Safe tuning practice:** Document why an exception is needed, scope it narrowly (specific asset, behavior, and time when possible), test it, and review it periodically.

<details markdown="1">
<summary>Reveal exam trap: Does fewer alerts mean better security?</summary>

**No.** Fewer alerts could mean improved detection quality, but they could also mean a failed sensor, dropped mirrored traffic, disabled rules, or excessive suppression. Measure **coverage and detection effectiveness**, not just alert count.

</details>

## 8. Monitoring design and operational safeguards

### Where should sensors go?

- **Perimeter:** Observe internet-facing traffic and attempted access across the external boundary.
- **Internal boundaries:** Monitor traffic between sensitive zones, such as Employees and Finance.
- **Critical hosts:** Collect endpoint logs and host telemetry from servers that matter most.
- **Remote and cloud environments:** Ensure relevant virtual networks, workloads, and identity services have suitable telemetry; a physical switch mirror cannot see every cloud flow.

### What can prevent visibility?

**Encryption:** HTTPS/TLS protects payload confidentiality; a passive network sensor can still see some metadata but cannot generally read encrypted application content. TLS inspection, where authorized and supported, introduces privacy, performance, certificate, and operational considerations.

**Asymmetric routing:** Different directions of a connection may pass different sensors, complicating session reconstruction.

**Packet loss:** Oversubscribed mirror ports or undersized sensors can miss packets.

**Unmonitored paths:** East–west traffic within a VLAN or host may bypass perimeter-only monitoring.

**Retention gaps:** Logs that were never collected or were deleted cannot be reconstructed later from the SIEM.

### Privacy, evidence, and access control

Traffic captures and logs may contain credentials, personal data, internal addresses, and confidential content. Collect only what is authorized and necessary. Restrict access, define retention, protect integrity, and document how evidence was acquired. Avoid publishing real client IPs, usernames, or packet contents in lab write-ups.

## 9. Safe technical demonstrations

These examples are for your **own computer or an explicitly authorized lab**. Commands and available flags vary by operating system.

### Inspect existing connections on macOS or Linux

```bash
netstat -an
```

This lists network socket information. It does **not** tell you whether a connection is malicious. On macOS, you can inspect which processes own connections with:

```bash
sudo lsof -nP -iTCP
```

### Capture a small amount of your own traffic

If `tcpdump` is available, first list interfaces:

```bash
sudo tcpdump -D
```

Choose an interface you are authorized to observe, then capture a limited sample:

```bash
sudo tcpdump -i en0 -c 20 -nn
```

`en0` is an **example interface**, common on some Macs but not universal. Replace it with the appropriate interface. `-c 20` stops after 20 packets; `-nn` avoids name and service resolution. Packet captures can expose sensitive data, so do not share raw output without review and redaction.

### Read an alert without attacking anything

Take the fictional alert in Section 5 and create a short triage note with these fields:

```text
Alert ID:
Timestamp / time zone:
Source and destination / network zones:
Detection method (if known):
Sensor action:
What the alert supports:
What it does not prove:
Corroborating evidence needed:
Next authorized action:
```

**Optional enrichment:** In a deliberately isolated training environment, explore a sample Suricata or Snort ruleset and identify which fields represent an alert's message, protocol, source/destination, and signature identifier. Do not run active scans against systems you do not own or administer.

## 10. Common misconceptions and Security+ exam traps

1. **“IDS blocks attacks.”** A conventional passive IDS detects and alerts; an IPS can block when appropriately configured.
2. **“An IPS always blocks every alert.”** An IPS may run in detection-only mode or have rules set to alert rather than drop.
3. **“A signature match proves compromise.”** It signals a possible match requiring validation and context.
4. **“High severity always means investigate first.”** Asset criticality, confidence, business impact, and active exploitation affect triage priority.
5. **“Flow logs contain packet payloads.”** They generally summarize communications rather than record full content.
6. **“The perimeter sensor sees the entire LAN.”** Sensor placement, switched traffic, and routing determine visibility.
7. **“Encrypted traffic is invisible.”** Much payload content is protected, but metadata and endpoint telemetry may still support investigation.
8. **“Turning off noisy rules fixes alert fatigue.”** Broad suppression can hide genuine attacks; tune narrowly and validate coverage.
9. **“A blocked connection means no incident occurred.”** A block is evidence about a specific attempt, not a guarantee about the host or all other attempts.
10. **“SIEM and IDS are the same thing.”** IDS generates detections from monitored activity; a SIEM aggregates and correlates multiple telemetry sources.

## 11. Key terminology and takeaways

| Term | Remember |
|---|---|
| IDS | Detects and alerts on suspicious activity |
| IPS | Can prevent or disrupt matching activity |
| NIDS/NIPS | Network-based detection/prevention |
| HIDS/HIPS | Host-based detection/prevention |
| Signature | Defined pattern associated with suspicious activity |
| Anomaly | Deviation from an expected baseline |
| Telemetry | Collected evidence about activity |
| PCAP | Detailed captured packets |
| Flow record | Summary of network communication metadata |
| SIEM | Central collection, search, and correlation of security data |
| True positive | Correct alert on malicious activity |
| False positive | Incorrect alert on benign activity |
| False negative | Missed malicious activity |
| Triage | Initial assessment, validation, and prioritization |

**Takeaway:** Effective network security combines segmentation (Day 21), enforcement (Day 22), and monitoring (Day 23). Detection quality depends on **what sensors can see**, **how rules interpret it**, and **how analysts validate and respond to findings**.

## 12. Security+ practice questions

**Question 1.** A company wants to observe suspected scanning on an internal switch without placing a new device in the forwarding path. Which deployment BEST meets the requirement?

A. An inline IPS configured to drop suspicious traffic  
B. A passive NIDS connected to a mirrored switch port  
C. A stateful firewall that blocks all east–west traffic  
D. A DNS resolver with query logging disabled

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** A passive NIDS can inspect mirrored traffic and alert without forwarding or blocking it. **A** is inline and may enforce blocks; **C** changes traffic policy; **D** provides no useful detection visibility.

</details>

**Question 2.** A network IPS reports an exploit signature match with action `drop`. What is the MOST defensible conclusion?

A. The endpoint was certainly compromised  
B. All future attempts from that source will be blocked  
C. The IPS reports that it blocked the matching traffic  
D. The event is necessarily a false positive

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: C.** The action field indicates the sensor's reported response to that traffic. It does not prove compromise, guarantee future protection, or establish that the alert was benign.

</details>

**Question 3.** A security analyst wants to determine whether a workstation transferred an unusually large number of bytes to an external IP during the past hour, without requiring application payloads. Which source is MOST appropriate?

A. Network flow records  
B. A file-integrity baseline  
C. A password-policy report  
D. A list of installed printer drivers

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: A.** Flow records commonly include byte counts and communication metadata. The other sources do not directly summarize network transfer volume.

</details>

**Question 4.** An anomaly detector flags a scheduled backup as suspicious because outbound data volume is far above the usual baseline. What classification BEST fits the event after the backup is verified as authorized?

A. True positive  
B. False negative  
C. True negative  
D. False positive

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: D.** The detector generated an alert for benign, authorized activity: a false positive. A false negative would be malicious activity that went undetected.

</details>

**Question 5.** A perimeter IDS detects no suspicious traffic between two workstations in the same employee VLAN. Endpoint logs later show one workstation probing the other. Which explanation BEST accounts for the discrepancy?

A. IDS signatures cannot detect internal scanning  
B. The traffic may not have crossed the perimeter sensor  
C. All traffic within a VLAN is encrypted  
D. SIEM correlation automatically suppresses same-VLAN traffic

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**Correct answer: B.** Traffic switched locally may not traverse the perimeter monitoring point. Detection depends on sensor placement and visibility. **A**, **C**, and **D** make unsupported universal claims.

</details>

## 13. Further reading

- [CompTIA Security+ certification and exam information](https://www.comptia.org/certifications/security) — consult the current official SY0-701 objectives for exact terminology.
- [NIST SP 800-94 — Guide to Intrusion Detection and Prevention Systems (IDPS)](https://csrc.nist.gov/pubs/sp/800/94/final) — foundational detection/prevention concepts; some implementation details are dated.
- [NIST SP 800-61 Rev. 3 — Incident Response Recommendations and Considerations for Cybersecurity Risk Management](https://csrc.nist.gov/pubs/sp/800/61/r3/final) — handling and responding to security events.
- [RFC 7011 — Specification of the IP Flow Information Export (IPFIX) Protocol](https://www.rfc-editor.org/rfc/rfc7011) — flow-export standards background.
- [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html_chunked/) — packet-analysis concepts and authorized capture workflows.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
