# Day 10 — Cryptographic Key Management: HSMs, TPMs, Escrow & Recovery

> **CompTIA Security+ SY0-701** · Domain 1 · Objective 1.4  
> **Week 02 · Lesson 5 of 5** · Beginner to intermediate · 65–85 minutes  
> Previous: [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md)

## 1. Learning objectives

By the end, explain the key lifecycle; compare HSM, TPM, and KMS; distinguish rotation, rewrapping, re-encryption and escrow; and choose controls for real-world scenarios.

## 2. The big idea: Who protects the keys?

Encryption can be mathematically strong and still fail when a secret key is stolen, lost, or shared carelessly.

Think of encryption as a vault. **Key management** is the locksmith, key register, access policy, spare-key procedure, and retirement plan. A perfect vault with its combination taped to the door is not secure. 🔐

**Cryptographic key management** covers the generation, storage, permitted use, replacement, recovery, and destruction of cryptographic keys.

**CIA connection:** Stolen decryption keys threaten **confidentiality**. Stolen signing keys can undermine **integrity and authenticity**. Lost decryption keys threaten **availability**.

## 3. The key lifecycle

| Stage | What happens | Why it matters |
| --- | --- | --- |
| Generate | Use approved algorithms and cryptographically secure randomness | Predictable keys defeat strong encryption |
| Establish / distribute | Agree on or deliver keys through protected mechanisms | Interception and substitution are threats |
| Store | Protect keys from theft or unauthorized use | Access control, HSMs, TPMs |
| Use | Restrict operations to authorized identities and purposes | Least privilege, logging |
| Rotate / replace | Introduce new key material | Limits exposure and responds to compromise |
| Back up / recover | Maintain authorized recovery where appropriate | Avoid irreversible loss |
| Disable / revoke | Stop use of compromised or obsolete material | Prevent further misuse |
| Retire / destroy | Eliminate unnecessary keys safely | Reduce exposure without losing needed data |

**Important:** Certificate revocation and symmetric-key retirement are not the same. A CA can revoke a certificate, but there is no universal internet-wide revocation list for a symmetric data-encryption key.

### Exam tip

If a question describes strong encryption but poor key storage, the weakness is **key management**, not necessarily the cipher.

## 4. HSM vs. TPM vs. KMS

| Technology | What it is | Common application |
| --- | --- | --- |
| **HSM** (Hardware Security Module) | Dedicated security hardware protecting keys and performing cryptographic operations | Certificate-authority signing keys, payment infrastructure |
| **TPM** (Trusted Platform Module) | Device-associated security component for protected keys, measured boot, attestation | Laptop disk-unlock protection |
| **KMS** (Key Management Service/System) | Central service for key policies, access, rotation, and audit | Cloud workload encryption |
| **Secure enclave / hardware-backed keystore** | Isolated key handling within a device | Mobile and computer credential protection |

### Under the hood: HSM

A certificate authority might use an HSM to perform signing without exporting its most sensitive private key to an application. **Non-exportable** keys can reduce exposure, but administrators, configuration, firmware, and backups still matter.

### Under the hood: TPM

A TPM can protect keys tied to a device and help establish trust in its boot state. A disk-encryption product may use the TPM to protect key material while another component performs bulk encryption. **A TPM is not simply a tiny hard drive containing all your files.**

### Under the hood: KMS

A KMS manages permissions, audit records, key versions, and cryptographic operations. Some services use HSMs underneath.

**Memory hook:** HSM = specialized vault; TPM = device lockbox; KMS = managed key office. 🗝️

## 5. Envelope encryption: DEK and KEK

Large files are often encrypted using a **data encryption key (DEK)**. A separate **key encryption key (KEK)** protects the DEK.

```text
Case files -- encrypt with DEK --> ciphertext
DEK        -- wrap with KEK ----> protected DEK
```

Authorized software can unwrap the DEK and then decrypt the files.

**Rewrapping** replaces the protection around a DEK (for example, a new KEK) without necessarily re-encrypting all the data.

**Re-encryption** changes the encryption protecting the actual data, potentially using a new DEK.

**Key rotation** introduces new key material. Rotation does not automatically re-encrypt every historical file. Older key versions may still be needed for legitimate recovery.

### Why compromise changes the plan

If an attacker stole a **KEK**, the team must determine whether wrapped DEKs were exposed and what the attacker could access. If an attacker stole the **DEK itself**, merely rewrapping that same DEK does not protect ciphertext the attacker already obtained.

**Exam trap:** New keys do not retroactively erase exposure to compromised old keys.

## 6. Key escrow, recovery and destruction

**Key escrow** stores key material or recovery information under controlled conditions so authorized personnel can regain access to encrypted data.

**Example:** A law firm retains encrypted archives for ten years. If an employee leaves, approved administrators can recover records without using the employee's personal account.

**Benefits:** Continuity, recovery, legal retention.

**Risks:** Recovery keys become high-value targets. Require restricted access, approval workflows, auditing, and separation of duties.

**Important distinction:** Escrowing decryption keys can support recovery. Escrowing **private signing keys** can weaken non-repudiation because multiple people or systems could create signatures using that key.

**Cryptographic erasure** destroys access by securely eliminating the necessary encryption keys, assuming the data is correctly encrypted and no usable copies of those keys remain. It can make data permanently unrecoverable.

## 7. Real-world case study: Leaked archive key

A law firm's backup administrator discovers a key used for encrypted case archives in a public code repository.

| Question | Recommended response |
| --- | --- |
| Which key leaked? | Identify whether it is a DEK, KEK, or signing key |
| What could it expose? | Map affected files, backups, and key versions |
| Can we delete it immediately? | Preserve authorized recovery and follow incident-response procedures |
| What should change? | Restrict access, replace affected keys, rewrap/re-encrypt as needed |
| How do we prevent recurrence? | Use managed secret storage, permissions, repository scanning, and audit logs |

**Why it matters:** Deleting the only working key could permanently destroy access to evidence. But leaving a compromised key in service can expose confidential records. Security professionals must manage **confidentiality and availability together**.

## 8. Guided notes — Complete before revealing

| Prompt | Your answer |
| --- | --- |
| Name five key-lifecycle stages. | _Write here._ |
| How do HSM, TPM, and KMS differ? | _Write here._ |
| What are DEK and KEK? | _Write here._ |
| How does rewrapping differ from re-encryption? | _Write here._ |
| What is key escrow, and what is its risk? | _Write here._ |
| Why might signing-key escrow be problematic? | _Write here._ |
| What is cryptographic erasure? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Lifecycle | Generate, establish/distribute, store, use, rotate, recover, disable, retire/destroy |
| HSM / TPM / KMS | HSM is dedicated key-protection hardware; TPM is device-rooted security; KMS manages keys and policies, possibly using HSMs |
| DEK / KEK | DEK encrypts data; KEK protects the DEK |
| Rewrapping / re-encryption | Rewrapping changes the key protection around a DEK; re-encryption changes the key protecting data |
| Escrow | Controlled recovery material; creates a sensitive target |
| Signing-key escrow | Additional holders could create signatures, weakening attribution |
| Erasure | Destroy necessary encryption keys so encrypted data cannot be decrypted, assuming no copies remain |

</details>

## 9. Hands-on lab: Encryption and recovery

**Time:** 15–25 minutes · **Tools:** OpenSSL, macOS/Linux terminal · **Data:** Fictional training text only.

### Create a practice file

```bash
mkdir -p secplus-day10
cd secplus-day10
printf 'Fictional client case notes\n' > notes.txt
```

### Encrypt using a temporary lab passphrase

```bash
openssl enc -aes-256-cbc -pbkdf2 -salt -in notes.txt -out notes.enc
```

Choose a **new disposable passphrase**, not one used for real accounts. The passphrase is used to derive an encryption key.

### Examine the ciphertext

```bash
ls -l notes.txt notes.enc
xxd -l 48 notes.enc
```

### Recover the original text

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -in notes.enc -out recovered.txt
cat recovered.txt
```

**Important limitation:** This educational OpenSSL CBC example does **not** provide authenticated encryption. It is not a recommended production archive design. Successful decryption alone does not reliably establish integrity.

**Think like an analyst:** What if the passphrase is lost? What if it is published in a public repository? What would a controlled recovery plan need?

<details>
<summary><strong>Reveal lab answers</strong></summary>

Without a valid key/passphrase or authorized recovery material, the file may be unrecoverable. Publishing the passphrase can expose the plaintext. Production systems should use reviewed encryption with integrity protection, managed secrets, limited permissions, auditing, and recovery procedures.

This lab demonstrates encryption and recovery; it does not demonstrate an HSM, TPM, or KMS.

</details>

## 10. Eight original Security+-style questions

These are original educational questions, not real CompTIA exam items. Choose the **BEST** answer before revealing the explanation.

### Q1

A certificate authority must protect its signing key in dedicated tamper-resistant hardware. Which is BEST?

A. IDS
B. HSM
C. SIEM
D. DLP

<details><summary>Reveal answer and explanation</summary>

**B — HSM.** HSMs protect high-value cryptographic keys and can perform signing. IDS detects intrusions; SIEM correlates events; DLP helps prevent data leakage.

</details>

### Q2

A laptop uses a platform security component to protect disk-unlock secrets. Which is MOST likely?

A. TPM
B. WAF
C. UPS
D. DNS

<details><summary>Reveal answer and explanation</summary>

**A — TPM.** TPMs support device-rooted key protection. WAF filters web requests, UPS provides power, and DNS resolves names.

</details>

### Q3

An organization must recover encrypted archives after the only employee with access leaves. Which is BEST?

A. Publish the key
B. Controlled key escrow
C. Disable encryption
D. Base64 encoding

<details><summary>Reveal answer and explanation</summary>

**B — Controlled key escrow.** Recovery material supports continuity when governed securely. Publishing keys compromises confidentiality; Base64 is encoding, not encryption.

</details>

### Q4

A firm rotates its KEK but continues using the same DEK. What has MOST likely changed?

A. All underlying data has been re-encrypted
B. The DEK's protective wrapping
C. The file contents
D. The symmetric algorithm automatically changed to RSA

<details><summary>Reveal answer and explanation</summary>

**B.** Rewrapping changes how the DEK is protected. It does not inherently change the DEK or re-encrypt the data.

</details>

### Q5

Which scenario BEST demonstrates cryptographic erasure?

A. Encrypt a file twice
B. Securely destroy the only remaining keys needed to decrypt correctly encrypted data
C. Rotate a public certificate
D. Compress a backup archive

<details><summary>Reveal answer and explanation</summary>

**B.** Destroying the necessary keys can render ciphertext inaccessible, provided no usable copies exist. The other actions do not establish erasure.

</details>

### Q6

Why can escrow of a private digital-signing key weaken non-repudiation?

A. It increases the AES block size
B. Multiple parties may be able to produce signatures using the same key
C. It makes certificates expire immediately
D. It prevents all signature verification

<details><summary>Reveal answer and explanation</summary>

**B.** Additional key holders weaken confidence about which person performed a signing action. The other claims are unrelated.

</details>

### Q7

A DEK used to encrypt files is confirmed compromised. Which response BEST addresses the continuing confidentiality risk?

A. Only change the KEK wrapping the same compromised DEK
B. Assess exposure and re-encrypt affected data with a new protected DEK as appropriate
C. Publish the DEK so users can verify it
D. Delete all audit logs

<details><summary>Reveal answer and explanation</summary>

**B.** Rewrapping a known-compromised DEK alone is insufficient. The organization must assess affected data and replace encryption material appropriately.

</details>

### Q8

Which statement about a KMS is MOST accurate?

A. It is always a chip inside a laptop
B. It manages key lifecycle and access policies and may rely on HSM-backed protection
C. It is a protocol for resolving hostnames
D. It eliminates the need for key permissions

<details><summary>Reveal answer and explanation</summary>

**B.** A KMS centralizes key operations and policies. TPMs are device components; DNS resolves names; KMS access controls remain essential.

</details>

**Score guide:** 7–8 correct: strong grasp. 5–6: revisit envelope encryption and recovery. 0–4: redraw the lifecycle and DEK/KEK diagram, then retry.

## 11. Discussion challenge

> A law firm stores ten years of encrypted client records. Its archive DEK may have leaked. Leadership proposes deleting the key immediately to “stop the breach.”

Explain why this might be dangerous, what additional facts you would investigate, and how you would balance **confidentiality, integrity, availability, and retention requirements**.

## 12. Study from the Treadmill 🏃

**Watch these specific lessons:**

1. [Professor Messer — Encryption Technologies (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/encryption-technologies-sy0-701/) — hardware-backed encryption and key protection.
2. [Professor Messer — Key Exchange (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/key-exchange-sy0-701/) — revisit how keys are established.
3. [Professor Messer — Certificates (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/certificates-sy0-701/) — connect signing-key protection to PKI.

**Active listening:** Which device protects laptop-bound keys? Which protects CA signing keys? Why isn't rotating a KEK necessarily the same as re-encrypting data?

## 13. One-minute recap and week review

- **HSM:** Dedicated hardware for high-value key protection and cryptographic operations.
- **TPM:** Device-rooted key protection and security functions.
- **KMS:** Central key management, permissions, and audit.
- **DEK:** Encrypts data. **KEK:** Wraps/protects a DEK.
- **Rotation:** Introduces new key material; old data may need additional work.
- **Escrow:** Controlled recovery, with substantial security responsibilities.
- **Cryptographic erasure:** Destroy the necessary keys to make encrypted data inaccessible.

### Week 2 cumulative retrieval

Without looking back, explain: (1) hashing vs. encryption, (2) symmetric vs. asymmetric cryptography, (3) what a certificate proves, (4) why revocation differs from expiration, and (5) what happens if an archive's only decryption key is lost.

**Next:** Day 11 — Data at Rest, in Transit & in Use.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
