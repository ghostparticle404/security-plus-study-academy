# Week 02 Diagnostic — Cryptography & Public Key Infrastructure

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 02 · Five themes · 20 questions**  
> **Optional · Estimated time:** 25–35 minutes  
> **Prerequisites:** Week 01 concepts or equivalent prior knowledge

## Welcome to your weekly diagnostic!

**You don't have to study something you already know.**

This optional assessment helps you identify your strengths and knowledge gaps before beginning the week's lessons. It isn't graded, and your score doesn't determine whether you can continue.

Answer **20 questions without consulting your notes**. Afterward, review your results by theme—not just your overall score.

If you answer all four questions correctly for a theme and can explain your reasoning, you may choose to skip that lesson. If you miss questions, follow the links to the specific sections that address those concepts.

Remember: **a diagnostic is a starting point, not proof of complete mastery.** You'll have another opportunity to demonstrate your learning in the end-of-week review.

## What does this diagnostic cover?

| Questions | Theme | Lesson |
| --- | --- | --- |
| P1–P4 | Hashing vs. Encryption | [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md) |
| P5–P8 | Symmetric vs. Asymmetric Cryptography | [Day 07 — Symmetric vs. Asymmetric Cryptography](day-07-symmetric-vs-asymmetric.md) |
| P9–P12 | Digital Certificates & PKI | [Day 08 — Certificates & PKI](day-08-certificates-pki.md) |
| P13–P16 | Certificate Lifecycle | [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md) |
| P17–P20 | Cryptographic Key Management | [Day 10 — Key Management](day-10-key-management.md) |

## How to take the diagnostic

1. Answer all 20 questions without consulting your lessons, notes, or the answer key.
2. Record your answers on paper or in a separate note.
3. After finishing, expand the answer key.
4. Count your correct answers separately for each theme.
5. Use the targeted lesson-section links to identify knowledge gaps.
6. Decide which lessons to study, review, or potentially skip.

These are original Security+-style educational questions, not actual CompTIA exam questions.

---

## Pre-diagnostic — 20 Questions

Choose the **single BEST answer** for each question.

### P1

**A law firm needs to keep confidential documents unreadable if a laptop is stolen while powered off. Which protection BEST meets this requirement?**

A. SHA-256 hashing  
B. Full-disk encryption  
C. Base64 encoding  
D. CRC32 checksum

### P2

**An analyst calculates the SHA-256 digest of a downloaded application and compares it with a trusted vendor reference. The values differ. What is the BEST conclusion?**

A. The application is definitely malicious  
B. The application was encrypted incorrectly  
C. The downloaded bytes do not match the trusted reference  
D. The vendor's private key has been compromised

### P3

**An organization stores passwords using unsalted SHA-256. Which change BEST protects against rainbow tables and large-scale offline password guessing?**

A. Encode passwords with Base64 before hashing  
B. Use unique random salts with Argon2id  
C. Replace SHA-256 with CRC32  
D. Store two SHA-256 hashes for each password

### P4

**Which statement about AES-GCM is MOST accurate when it is implemented correctly?**

A. It provides confidentiality and integrity/authenticity checks  
B. It guarantees that encrypted files remain available  
C. It proves which human approved a transaction  
D. It uses a public/private key pair to encrypt data

### P5

**A company must efficiently encrypt a multi-terabyte backup archive. Which algorithm is MOST appropriate?**

A. ECDSA  
B. RSA-PSS  
C. AES  
D. SHA-256

### P6

**Alice wants to encrypt a small secret so that only Bob can decrypt it using a suitable public-key encryption scheme. Which key should Alice use?**

A. Alice's private key  
B. Bob's public key  
C. Bob's private key  
D. Alice's public key

### P7

**Which mechanism is commonly used in TLS 1.3 to establish shared secrets while supporting forward secrecy?**

A. RSA key transport  
B. ECDSA  
C. Base64  
D. ECDHE

### P8

**Why does modern HTTPS combine asymmetric cryptography with symmetric encryption?**

A. Symmetric encryption authenticates certificates without public keys  
B. Asymmetric cryptography encrypts all application traffic more efficiently  
C. Public-key techniques support authentication and key establishment, while symmetric encryption efficiently protects traffic  
D. Hashing eliminates the need for encryption keys

### P9

**A browser visits `files.example.com`, but the server presents a certificate whose SAN lists only `portal.example.com`. What is the MOST likely problem?**

A. The certificate uses the wrong hash length  
B. The requested hostname does not match the certificate  
C. The server has insufficient storage  
D. The user's password has expired

### P10

**What is the PRIMARY purpose of a trusted root certificate authority (CA) in PKI?**

A. Encrypt all application data  
B. Store website private keys  
C. Serve as a trust anchor for certificate validation  
D. Guarantee that every certified website is safe

### P11

**An attacker creates a phishing website and obtains a valid domain-validated TLS certificate for a domain they control. Which statement is MOST accurate?**

A. The certificate guarantees the website is legitimate  
B. HTTPS prevents the website from collecting stolen credentials  
C. The CA has verified the website operator's honesty  
D. The certificate may be valid even though the website is malicious

### P12

**Which field in an X.509 certificate is MOST relevant when checking whether the certificate covers a requested DNS hostname?**

A. Subject Alternative Name (SAN)  
B. Serial number  
C. Signature algorithm  
D. Issuer

### P13

**A server administrator generates a certificate signing request (CSR). Which item must NOT be included in the CSR sent to the CA?**

A. Requested domain name  
B. Public key  
C. Private key  
D. Requested SAN entries

### P14

**A certificate is still within its validity period, but the CA invalidates it after a private-key compromise. What is its status?**

A. Expired  
B. Revoked  
C. Re-encrypted  
D. Rehashed

### P15

**What is the PRIMARY purpose of OCSP stapling?**

A. Allow a server to provide a signed, time-limited certificate-status response during the TLS handshake  
B. Transfer the server's private key securely to the browser  
C. Replace the need for certificate validation  
D. Automatically renew expired certificates

### P16

**An organization's TLS certificate will expire tomorrow. What should administrators do to BEST prevent service disruption?**

A. Disable certificate validation for clients  
B. Publish the private key so users can verify it  
C. Wait until the certificate expires before acting  
D. Obtain, deploy, and test a valid replacement certificate

### P17

**A laptop uses a hardware security component to protect disk-unlock key material and support measured boot. Which technology is MOST likely involved?**

A. Hardware Security Module (HSM)  
B. Trusted Platform Module (TPM)  
C. Security Information and Event Management (SIEM)  
D. Web Application Firewall (WAF)

### P18

**A company uses a data encryption key (DEK) to encrypt files and a key encryption key (KEK) to protect the DEK. If the KEK is rotated and the DEK remains unchanged, what has MOST likely changed?**

A. The contents of every encrypted file  
B. The underlying encryption algorithm  
C. The protective wrapping around the DEK  
D. The identity of the file owner

### P19

**A law firm must recover encrypted archives after the employee who originally managed the encryption leaves. Which solution BEST supports authorized recovery?**

A. Controlled key escrow  
B. Publicly shared encryption keys  
C. Base64 encoding  
D. Destruction of all recovery keys

### P20

**A data encryption key (DEK) used to protect confidential archives is confirmed compromised. Which response BEST addresses the continuing confidentiality risk?**

A. Only rotate the KEK while continuing to use the compromised DEK  
B. Assess the exposure and re-encrypt affected data using new protected key material as appropriate  
C. Delete the encryption logs  
D. Assume replacing the key eliminates all previous exposure

---

## Answer Key & Targeted Study Recommendations

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal diagnostic answer key and explanations — P1–P20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the links below to review the exact concepts you need to strengthen.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| P1 | **B** | [Day 06 — Encryption](day-06-hashing-vs-encryption.md#3-under-the-hood-encryption) | Full-disk encryption helps protect confidential files on a stolen, powered-off laptop when the keys remain protected. Hashing and encoding do not conceal data. |
| P2 | **C** | [Day 06 — Hashing](day-06-hashing-vs-encryption.md#4-under-the-hood-hashing) | Different SHA-256 digests indicate that the downloaded bytes do not match the trusted reference. A mismatch does not automatically prove maliciousness. |
| P3 | **B** | [Day 06 — Password Hashing](day-06-hashing-vs-encryption.md#5-passwords-why-sha-256-alone-isnt-enough) | Unique random salts prevent reusable precomputed lookups, while Argon2id increases the cost of offline password guessing. |
| P4 | **A** | [Day 06 — Encryption Modes](day-06-hashing-vs-encryption.md#3-under-the-hood-encryption) | AES-GCM provides authenticated encryption, combining confidentiality with integrity/authenticity checks when implemented correctly. |
| P5 | **C** | [Day 07 — Symmetric Encryption](day-07-symmetric-vs-asymmetric.md#3-under-the-hood-symmetric-encryption) | AES is efficient for encrypting large amounts of data. ECDSA and RSA-PSS are signature schemes; SHA-256 is a hash function. |
| P6 | **B** | [Day 07 — Public-Key Encryption](day-07-symmetric-vs-asymmetric.md#4-under-the-hood-asymmetric-cryptography) | Alice encrypts using Bob's authentic public key. Bob decrypts using his corresponding private key. |
| P7 | **D** | [Day 07 — Key Agreement](day-07-symmetric-vs-asymmetric.md#c-key-agreement-establish-a-shared-secret) | ECDHE establishes shared secrets using ephemeral key material, supporting forward secrecy. TLS 1.3 does not use RSA key transport. |
| P8 | **C** | [Day 07 — Hybrid Cryptography](day-07-symmetric-vs-asymmetric.md#5-hybrid-cryptography-how-modern-https-works) | Public-key techniques support authentication and key establishment, while symmetric authenticated encryption efficiently protects application traffic. |
| P9 | **B** | [Day 08 — Certificate SAN](day-08-certificates-pki.md#the-san-trap) | The certificate's SAN must cover the hostname being requested. A certificate for one hostname does not automatically cover another. |
| P10 | **C** | [Day 08 — PKI and Chain of Trust](day-08-certificates-pki.md#3-the-pki-cast-of-characters) | A trusted root CA provides the trust anchor used to validate a certificate chain. It does not encrypt application traffic or guarantee website safety. |
| P11 | **D** | [Day 08 — Domain Validation](day-08-certificates-pki.md#what-does-a-ca-validate) | A DV certificate establishes control of a domain, not the trustworthiness of the website operator. Malicious sites can use valid HTTPS certificates. |
| P12 | **A** | [Day 08 — X.509 Certificate Fields](day-08-certificates-pki.md#4-under-the-hood-whats-inside-an-x509-certificate) | The Subject Alternative Name field identifies the DNS names and IP addresses covered by the certificate. |
| P13 | **C** | [Day 09 — Creating a CSR](day-09-certificate-lifecycle.md#3-generate-keys-and-create-a-csr) | A CSR contains the public key, requested identity information, and a signature. The private key must remain protected and is not included. |
| P14 | **B** | [Day 09 — Revocation](day-09-certificate-lifecycle.md#5-monitor-renew-replace-revoke) | Revocation invalidates a certificate before its scheduled expiration. A certificate can be revoked while its validity dates remain current. |
| P15 | **A** | [Day 09 — OCSP Stapling](day-09-certificate-lifecycle.md#6-revocation-crl-ocsp-and-stapling) | OCSP stapling allows the server to include a signed, time-limited certificate-status response in the TLS handshake. |
| P16 | **D** | [Day 09 — Renewal and Replacement](day-09-certificate-lifecycle.md#5-monitor-renew-replace-revoke) | Obtaining, deploying, and testing a valid replacement before expiration helps prevent certificate-related outages. |
| P17 | **B** | [Day 10 — TPM](day-10-key-management.md#under-the-hood-tpm) | A TPM provides device-rooted key protection and can support measured boot and disk-unlock security. |
| P18 | **C** | [Day 10 — Envelope Encryption](day-10-key-management.md#5-envelope-encryption-dek-and-kek) | Rewrapping changes the protection around a DEK. It does not necessarily re-encrypt the underlying data. |
| P19 | **A** | [Day 10 — Key Escrow](day-10-key-management.md#6-key-escrow-recovery-and-destruction) | Controlled key escrow provides authorized recovery while requiring strong access restrictions, auditing, and approval procedures. |
| P20 | **B** | [Day 10 — Key Compromise](day-10-key-management.md#why-compromise-changes-the-plan) | A compromised DEK may require replacement and re-encryption of affected data. Merely rewrapping the same compromised DEK does not eliminate exposure. |

</details>

---

## Your Diagnostic Scorecard

Record your results by theme, not just your overall score.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md) | P1–P4 | ____ / 4 |
| [Day 07 — Symmetric vs. Asymmetric Cryptography](day-07-symmetric-vs-asymmetric.md) | P5–P8 | ____ / 4 |
| [Day 08 — Certificates & PKI](day-08-certificates-pki.md) | P9–P12 | ____ / 4 |
| [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md) | P13–P16 | ____ / 4 |
| [Day 10 — Key Management](day-10-key-management.md) | P17–P20 | ____ / 4 |
| **TOTAL** | **20 questions** | **____ / 20** |

### What do your results mean?

| Score per theme | Interpretation | Recommended Action |
| --- | --- | --- |
| **4/4** | Strong initial understanding | You may consider skipping the introductory lesson if you can explain your answers and apply the concepts in a new scenario. |
| **3/4** | Mostly familiar | Review the targeted section for the missed question and complete the lesson's exit ticket. |
| **0–2/4** | Knowledge gaps identified | Study the full lesson, including worked examples, practice questions, and optional labs. |

**Important:** Four correct answers do not prove complete mastery. They provide an initial indication of your understanding. Continue checking your knowledge through cumulative practice and later assessments.

### Reflection

Before moving forward, consider:

- Which questions did you answer confidently?
- Which answers were educated guesses?
- Which cryptographic concepts were unfamiliar?
- Could you explain why the incorrect options were wrong?
- Which lessons should you prioritize?

Treat lucky guesses as opportunities for review, even if your answer was correct.

---

## Build Your Personalized Study Plan

Use your diagnostic results to decide which lessons to complete.

- **Need to distinguish hashing, encryption, and encoding?** Start with [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md).
- **Confused about symmetric and asymmetric cryptography?** Study [Day 07 — One Key or Two?](day-07-symmetric-vs-asymmetric.md).
- **Unsure how browsers trust certificates?** Review [Day 08 — Digital Certificates & PKI](day-08-certificates-pki.md).
- **Need practice with CSRs, expiration, and revocation?** Complete [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md).
- **Want to strengthen key-management knowledge?** Study [Day 10 — HSMs, TPMs, Escrow & Recovery](day-10-key-management.md).

## What Comes Next?

**Ready to start learning?**

Begin with [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md), or jump directly to the lessons identified by your diagnostic results.

**Already confident in all five themes?**

You can move directly to the [Week 02 End-of-Week Review](review.w2.md), which will contain a different set of questions and cumulative scenarios.

The review gives you another opportunity to demonstrate what you know and identify any remaining knowledge gaps.

**Remember: The goal isn't to complete every lesson. The goal is to develop the knowledge and skills needed to succeed in cybersecurity and on the Security+ exam.**

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
