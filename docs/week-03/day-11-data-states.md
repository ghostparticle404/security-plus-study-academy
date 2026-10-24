# Day 11 — Protecting Data at Rest, in Transit, and in Use

> **CompTIA Security+ SY0-701** · **Week 03, Lesson 1 of 5** · Domain 3, Objective **3.3: Data protection** (related: Domain 1, Objective 1.4)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 10 — Key Management](../week-02/day-10-key-management.md)  
> **Next:** Day 12 — Data Classification, Labeling, and Handling

## 1. Learning objectives

By the end, you should be able to distinguish the three data states; choose controls for each; explain encryption's limitations; compare DLP, masking, and tokenization; and map a real-world data flow.

## 2. The big idea: A file takes a journey

Imagine a confidential client document at a law firm. It sits on a server, travels over HTTPS to a laptop, and opens in a document editor. The document is the same, but its exposure changes.

| State | Meaning | Example | Safeguards |
| --- | --- | --- | --- |
| **At rest** | Stored on media | PDF on an SSD or in cloud storage | Full-disk/storage encryption, permissions, backups |
| **In transit** | Moving between endpoints | PDF uploaded over HTTPS | TLS, certificate validation, IPsec when appropriate |
| **In use** | Actively processed | PDF open in an editor | Authorization, process isolation, endpoint security |

🧠 **Memory hook:** Data is a traveler. 🧳 **Rest** = hotel safe; **transit** = armored car; **use** = papers open on a conference table. Protecting the safe and car doesn't automatically protect the table.

**CIA connection:** Confidentiality matters in all three states, but integrity and availability matter too. Encryption is not a substitute for backups or authorization.

## 3. At rest: Securing stored data

**Full-disk encryption (FDE)** protects stored volumes against many offline attacks, especially when the device is powered off and keys remain secure. Examples: **FileVault** (macOS) and **BitLocker** (Windows). **File-level encryption** targets selected files; database and cloud-storage encryption have different boundaries.

Other controls include **least-privilege permissions**, tested and protected **backups**, retention rules, secure disposal, monitoring, and cryptographic key management.

**Important limitation:** Once a device is unlocked, applications may access decrypted content. A compromised user session may expose files despite FDE. Similarly, cloud encryption at rest does not prevent an authorized cloud service from delivering files through an accidentally public link.

**Exam trap:** “Encrypted at rest” does not mean “safe from every attacker.” Ask **who can decrypt and access the content**.

## 4. In transit: Securing moving data

**TLS (Transport Layer Security)** helps authenticate the server and protects traffic with encryption and integrity checks when properly configured. **HTTPS = HTTP over TLS.** Modern TLS typically combines asymmetric authentication/key establishment with efficient symmetric authenticated encryption.

```text
Browser -- TLS negotiation and authentication --> Portal
Browser <==== encrypted application traffic ====> Portal
```

Other secure protocols include **SSH/SFTP** for remote access and file transfer, and **IPsec** for certain VPN configurations. Plain HTTP, Telnet, and FTP generally lack suitable confidentiality protections for sensitive traffic.

**Boundary:** TLS protects traffic *between its endpoints*. A reverse proxy may terminate TLS and forward decrypted data. TLS does not guarantee the application is secure or that a user chose the correct recipient.

🧠 An armored truck is useful—but if it unloads into an unlocked warehouse, the journey wasn't the only risk. 🚚

## 5. In use: Securing active processing

An application often needs plaintext to display or edit an encrypted document. Sensitive content can reside in RAM, application buffers, temporary files, or screenshots.

| Control | Benefit | Limitation |
| --- | --- | --- |
| Least privilege | Restricts access to necessary identities/processes | Stolen sessions may retain authorized access |
| Process/memory isolation | Separates workloads | OS or application compromise may bypass boundaries |
| EDR | Detects some malicious behavior | Cannot guarantee detection |
| Screen locking | Reduces unattended access | Doesn't stop all malware |
| Trusted execution environment (TEE) | Hardware-backed isolation for supported workloads | Threat model and hardware support vary |
| Redaction/minimization | Reduces exposed content | May reduce utility |

**Confidential computing** can use TEEs to protect some data during computation. It is not a universal guarantee that every application is protected.

**Technical hook:** Disk encryption protects the *stored* file, not necessarily the plaintext an editor has already loaded into memory.

## 6. DLP, masking, tokenization, and redaction

**Data loss prevention (DLP)** tools identify, alert on, or block sensitive-data movement, such as emailing client identifiers to a personal account or uploading case files to unapproved storage. Endpoint, network, and cloud DLP have different visibility and limitations.

**Masking** obscures values in a display or substitute dataset (for example `**** **** **** 4821`). **Redaction**, when correctly performed, removes sensitive content from a released copy. A black rectangle drawn over a PDF might leave the underlying text extractable.

**Tokenization** substitutes a sensitive value with a token resolved by a separate protected system, such as a payment-card token vault. **Encryption** instead mathematically transforms data using a key. Tokenization and encryption are not interchangeable.

| Requirement | Best starting point |
| --- | --- |
| Stolen powered-off laptop | FDE and secure unlock credentials |
| Browser-to-portal transfer | TLS and certificate checks |
| Active sensitive document | Authorization and process isolation |
| Unapproved sensitive upload | DLP |
| Display only last four digits | Masking |
| Substitute payment-card numbers | Tokenization |

**Exam tip:** Identify the **data state**, **threat**, and **control boundary** before choosing an answer.

## 7. Worked case study: A legal evidence file

A law firm receives an evidence archive, analyzes it, and sends a report to outside counsel.

| Stage | State | Risk | Control |
| --- | --- | --- | --- |
| Stored in portal | Rest | Overbroad access | Permissions, encryption, monitoring |
| Downloaded via HTTPS | Transit | Interception | TLS, certificate validation |
| Saved to laptop | Rest | Theft | FDE, device security |
| Opened in editor | Use | Malware reads plaintext | Trusted software, EDR, isolation |
| Sent to outside counsel | Transit | Wrong recipient | Recipient verification, secure sharing |
| Archived after case | Rest | Excessive retention | Retention policy, protected backups |

**Scenario:** The laptop is stolen while unlocked. FDE may not stop immediate access to open files. Endpoint management, session controls, least privilege, and incident response become essential.

**Lesson:** TLS cannot stop an employee sending to the wrong person; FDE cannot stop all malware running in an unlocked account. Layer your safeguards.

## 8. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Define rest, transit, and use. | _Write here._ |
| Name two controls for each state. | _Write here._ |
| Why doesn't FDE secure every active process? | _Write here._ |
| Where does TLS protection end? | _Write here._ |
| Compare masking, tokenization, and encryption. | _Write here._ |
| What is DLP intended to do? | _Write here._ |
| Why minimize retained data? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Three states | Rest = stored; transit = moving; use = actively processed. |
| Controls | Rest: FDE, permissions. Transit: TLS, IPsec. Use: isolation, authorization. |
| FDE limitation | Unlocked volumes can supply decrypted content to applications. |
| TLS boundary | Between TLS endpoints; not after decryption or on compromised endpoints. |
| Three techniques | Masking hides displayed values; tokenization substitutes tokens; encryption uses cryptographic keys. |
| DLP | Detects or restricts unauthorized movement of sensitive data. |
| Minimization | Less data stored means less to lose and protect. |

</details>

## 9. Hands-on lab: Map the flow and inspect HTTPS

**Time:** 15–25 minutes · **Tools:** Terminal and notebook · **Safe, read-only**

Draw and label each state:

```text
Client portal → HTTPS download → Laptop SSD → Document editor
             → HTTPS upload → Outside counsel portal
```

Inspect a TLS connection (the output varies with your curl build and network):

```bash
curl -I https://example.com
curl -v -o /dev/null https://example.com
```

Avoid `-k` / `--insecure`, which suppresses certificate verification. On macOS, optionally check FileVault without changing settings:

```bash
fdesetup status
```

**Questions:** Does TLS prove a website is honest? Can malware in an unlocked session read decrypted files? Does HTTPS prevent wrong-recipient sharing?

<details>
<summary><strong>Reveal lab answers</strong></summary>

**No; potentially yes; no.** TLS secures transport to an authenticated endpoint, not a site's intentions. Unlocked volumes can supply decrypted data to compromised processes. HTTPS cannot validate your intended recipient. Portal/laptop storage = rest; HTTPS arrows = transit; editor = use.

</details>

## 10. Eight original Security+-style questions

Choose the **BEST** answer before opening each explanation. These are original practice questions, not actual CompTIA items.

### Q1. Which BEST protects files on a powered-off stolen laptop?

A. Full-disk encryption
B. Load balancing
C. Base64 encoding
D. Screen brightness controls

<details><summary>Reveal answer and explanation</summary>

**A.** FDE protects stored data when keys remain secure. The others do not provide confidentiality.

</details>

### Q2. An employee uploads a file over HTTPS. Which state is TLS protecting during transfer?

A. At rest
B. In transit
C. In use
D. Archived only

<details><summary>Reveal answer and explanation</summary>

**B.** TLS protects data moving between its endpoints; storage and processing need additional controls.

</details>

### Q3. Malware reads documents on an unlocked encrypted laptop. Why did FDE not stop it?

A. FDE is hashing
B. FDE only protects packets
C. Authorized sessions may access decrypted files
D. FDE disables antivirus

<details><summary>Reveal answer and explanation</summary>

**C.** An unlocked system may provide plaintext to processes with sufficient permissions. FDE still helps against offline theft.

</details>

### Q4. A payment service replaces card numbers with references resolved through a protected vault. What is this?

A. Tokenization
B. Base64
C. Load balancing
D. Defragmentation

<details><summary>Reveal answer and explanation</summary>

**A.** Tokenization substitutes sensitive values with tokens; the other answers do not describe this technique.

</details>

### Q5. Which tool BEST helps block sensitive files uploaded to unapproved cloud storage?

A. DLP
B. NTP
C. RAID
D. DHCP

<details><summary>Reveal answer and explanation</summary>

**A.** DLP can identify and restrict sensitive-data movement. NTP synchronizes clocks, RAID supports storage redundancy, and DHCP assigns network configuration.

</details>

### Q6. Cloud files are encrypted at rest but exposed by a public sharing link. What failed?

A. Cloud encryption cannot work
B. Sharing permissions and access configuration
C. SHA-256 digest length
D. AES block size

<details><summary>Reveal answer and explanation</summary>

**B.** The service can decrypt and serve content to identities permitted by a public link. Encryption doesn't replace authorization.

</details>

### Q7. Which technology helps isolate computation for supported workloads in hardware-backed environments?

A. Trusted execution environment (TEE)
B. FTP
C. Plain HTTP
D. Simple checksum

<details><summary>Reveal answer and explanation</summary>

**A.** TEEs can isolate some data-in-use workloads, subject to implementation and threat-model limits.

</details>

### Q8. A confidential report travels over TLS to the wrong recipient. Which is MOST accurate?

A. TLS prevents delivery to unintended people
B. Confidentiality cannot be compromised with TLS
C. TLS protects transport, but recipient verification is still needed
D. Hashing corrects recipient mistakes

<details><summary>Reveal answer and explanation</summary>

**C.** TLS protects the connection, not whether the recipient was appropriate.

</details>

**Score guide:** 7–8 correct: strong understanding. 5–6: revisit Sections 3–6. 0–4: redraw the data flow and retry.

## 11. Discussion challenge

> A law firm encrypts laptops and cloud storage and uses HTTPS for all transfers. An employee nevertheless exposes a confidential case file. Describe **two ways** this could happen without breaking encryption and recommend **one additional control for each**.

**Bonus:** Which CIA triad objectives could be affected?

## 12. Study from the Treadmill 🏃

**Direct links to specific videos:**

1. [Professor Messer — Protecting Data (SY0-701, Objective 3.3)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/protecting-data-sy0-701/) — compare protection mechanisms and data states.
2. [Professor Messer — Encrypting Data (SY0-701, Objective 1.4)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/encrypting-data-sy0-701/) — revisit encryption use cases and boundaries.

**Active-listening prompts:** When is plaintext exposed? Why can't FDE secure every active process? How does DLP complement encryption?

## 13. One-minute recap

- **Rest:** Stored → encryption, permissions, backups.
- **Transit:** Moving → TLS, authenticated protocols.
- **Use:** Processed → authorization, isolation, endpoint protection; TEEs for supported workloads.
- **DLP:** Controls sensitive-data movement.
- **Masking/tokenization:** Reduce exposure differently from encryption.
- **Golden rule:** Protect the **entire data lifecycle**.

**Next:** Day 12 — Data Classification, Labeling, and Handling.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
