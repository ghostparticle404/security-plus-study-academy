# Week 02 Review — Cryptography & Public Key Infrastructure

> **Security+ Study Academy · CompTIA Security+ SY0-701**  
> **Week 02 · Five themes · 20 review questions + 5 cumulative questions**  
> **Optional · Estimated time:** 35–50 minutes  
> **Prerequisites:** Week 02 lessons or equivalent prior knowledge

## Welcome to your Week 02 Review!

**You've studied the concepts. Now let's see how well you can apply them.**

This optional review helps you evaluate your understanding of the five Week 02 themes, identify any remaining knowledge gaps, and determine which concepts deserve additional practice.

You can complete this review whether you studied all five lessons or skipped some based on your diagnostic results.

**Haven't started the week yet?** Take the [Week 02 Diagnostic](00-diagnostic.md) to identify what you already know and which lessons to prioritize.

### How to complete this review

1. Answer **S1–S20** without consulting your notes or lessons.
2. Record your answers before opening the answer key.
3. Check your results by theme, not just your overall score.
4. Follow the targeted lesson links for questions you missed.
5. Complete **C1–C5**, which combine concepts from multiple lessons.
6. Reflect on your progress and decide whether you're ready for Week 03.

These are original educational practice questions, not actual CompTIA exam items.

## Week 02 Themes

| Questions | Theme | Lesson |
| --- | --- | --- |
| S1–S4 | Hashing vs. Encryption | [Day 06](day-06-hashing-vs-encryption.md) |
| S5–S8 | Symmetric vs. Asymmetric Cryptography | [Day 07](day-07-symmetric-vs-asymmetric.md) |
| S9–S12 | Digital Certificates & PKI | [Day 08](day-08-certificates-pki.md) |
| S13–S16 | Certificate Lifecycle | [Day 09](day-09-certificate-lifecycle.md) |
| S17–S20 | Cryptographic Key Management | [Day 10](day-10-key-management.md) |

---

## Part 1 — End-of-Week Knowledge Check

**Directions:** Choose the single BEST answer to each question. Complete all 20 before revealing the answer key.

### S1

**A developer encrypts a confidential database backup and also calculates its SHA-256 digest. What is the PRIMARY purpose of the digest?**

A. Recover the plaintext if the encryption key is lost  
B. Detect whether the backup bytes have changed when compared with a trusted reference  
C. Prevent unauthorized users from opening the backup  
D. Replace the need to manage encryption keys

### S2

**Two employees use the same password, but the authentication database stores different password hashes for their accounts. Which feature BEST explains the difference?**

A. Both passwords were encrypted with the same public key  
B. The system uses Base64 encoding  
C. Each password was processed with a different random salt  
D. The passwords were automatically converted to digital signatures

### S3

**A company needs to verify stored passwords without retaining recoverable plaintext passwords. Which approach is MOST appropriate?**

A. Store passwords encrypted with a shared, reversible key  
B. Store passwords as Base64 strings  
C. Store passwords using a fast, unsalted SHA-256 digest  
D. Store password verifiers using Argon2id with unique random salts

### S4

**A file is encrypted using AES-GCM. During storage, an attacker changes part of the ciphertext. What should a correctly implemented decryption process do?**

A. Detect the authentication failure and reject the altered ciphertext  
B. Automatically restore the original file  
C. Reveal the attacker's identity  
D. Decrypt the modified file without checking its integrity

### S5

**A law firm encrypts thousands of large case files with a randomly generated AES key. Why is symmetric encryption appropriate for this task?**

A. AES eliminates the need to protect encryption keys  
B. Symmetric encryption is efficient for large volumes of data  
C. Symmetric encryption provides a public certificate for every file  
D. AES uses a different public/private key pair for each file

### S6

**A software vendor signs an application update with its private signing key. What should customers use to verify the signature?**

A. The vendor's authentic public key  
B. The customer's private key  
C. The vendor's private key  
D. The application's encryption password

### S7

**An attacker records encrypted HTTPS traffic today and steals the server's long-term private key next year. Which feature of a properly configured TLS 1.3 connection helps protect the recorded session?**

A. Base64 encoding  
B. RSA key transport  
C. Forward secrecy from ephemeral key agreement  
D. Reusing the same session key indefinitely

### S8

**During a TLS 1.3 connection, the client and server establish shared secrets and then protect application data using a symmetric authenticated cipher. What does this illustrate?**

A. Hashing without encryption  
B. Hybrid cryptography  
C. Password-based authentication only  
D. Key escrow

### S9

**A browser receives a server certificate signed by an intermediate CA. What must the browser establish before trusting the certificate?**

A. That the server uses the longest possible password  
B. That the intermediate CA owns the server's private key  
C. That a valid certificate chain leads to an accepted trust anchor and the certificate meets validation requirements  
D. That every certificate in the chain was issued directly by the root CA

### S10

**An administrator installs a valid certificate for `shop.example.com`, but visitors connecting to `payments.example.com` receive a hostname warning. What should the administrator check FIRST?**

A. Whether the certificate's SAN includes `payments.example.com`  
B. Whether the server has enough RAM  
C. Whether the website uses compression  
D. Whether the root CA is stored on the web server

### S11

**A user sees a padlock and HTTPS in the browser while visiting a fraudulent online store. Which statement is MOST accurate?**

A. The certificate proves the merchant is trustworthy  
B. HTTPS guarantees the store cannot steal information  
C. The CA guarantees all purchases are legitimate  
D. HTTPS can protect the connection without establishing that the business is honest

### S12

**Which component of PKI is responsible for issuing and digitally signing certificates after the applicable validation requirements are satisfied?**

A. Certificate Authority (CA)  
B. Data Encryption Key (DEK)  
C. Trusted Platform Module (TPM)  
D. Intrusion Detection System (IDS)

### S13

**An administrator generates a new TLS private key and submits a CSR to a CA. Which statement BEST describes the CSR?**

A. It contains the private key so the CA can back it up  
B. It requests a certificate for the associated public key and identity information  
C. It automatically revokes all earlier certificates  
D. It replaces the need for domain validation

### S14

**A company's website certificate expires during a holiday weekend, causing browsers to display security warnings. Which preventive measure would BEST reduce this risk?**

A. Disable browser certificate checks  
B. Use the same certificate indefinitely  
C. Monitor expiration dates and automate renewal and deployment where appropriate  
D. Publish the private key to simplify troubleshooting

### S15

**A certificate is revoked after a private-key compromise. Which mechanisms are commonly used to communicate revocation status?**

A. CRLs and OCSP  
B. AES and RSA  
C. HSMs and TPMs  
D. SHA-256 and Argon2id

### S16

**A web server's TLS private key is accidentally committed to a public GitHub repository. What is the BEST response?**

A. Delete the GitHub commit and continue using the same key  
B. Wait for the certificate to expire naturally  
C. Change the certificate's display name  
D. Treat the key as compromised, replace the key and certificate, and revoke the affected certificate as appropriate

### S17

**A cloud service encrypts customer records using DEKs and protects those DEKs with a centrally managed KEK. What is the PRIMARY advantage of this arrangement?**

A. It eliminates the need for access controls  
B. It allows centralized protection and rotation of key-wrapping material without necessarily re-encrypting all stored records  
C. It makes encryption mathematically irreversible  
D. It guarantees recovery of deleted plaintext

### S18

**A financial institution needs to protect high-value cryptographic keys and perform sensitive cryptographic operations inside dedicated tamper-resistant hardware. Which solution is MOST appropriate?**

A. SIEM  
B. WAF  
C. HSM  
D. DNS resolver

### S19

**A company plans to destroy encryption keys for retired confidential archives. Which issue must it evaluate FIRST?**

A. Whether the keys are stored in Base64  
B. Whether employees prefer shorter passwords  
C. Whether the archive filenames are descriptive  
D. Whether the data must remain recoverable for legal, regulatory, or business purposes

### S20

**A company discovers that an attacker obtained a DEK used to encrypt confidential customer records. Why is rotating only the KEK insufficient?**

A. The attacker may already possess the DEK needed to decrypt the affected data  
B. KEKs are never used in envelope encryption  
C. Rotating a KEK automatically makes all ciphertext public  
D. A DEK cannot be replaced

---

## Part 2 — Answer Key & Targeted Review

**Do not open this section until you have answered all 20 questions.**

<details markdown="1">
<summary><strong>Reveal Week 02 answer key and explanations — S1–S20</strong></summary>

Questions are grouped by theme in sets of four. Count your correct answers within each group, then use the targeted lesson links to review any concepts you missed.

| Question | Answer | Targeted Lesson Section | Why |
| --- | --- | --- | --- |
| S1 | **B** | [Day 06 — Hashing](day-06-hashing-vs-encryption.md#4-under-the-hood-hashing) | A trusted digest comparison helps detect changes to data. Hashing does not provide confidentiality or key recovery. |
| S2 | **C** | [Day 06 — Salting](day-06-hashing-vs-encryption.md#salt) | Unique random salts cause identical passwords to produce different stored password verifiers. |
| S3 | **D** | [Day 06 — Slow Password Hashing](day-06-hashing-vs-encryption.md#slow-password-hashing) | Argon2id with unique random salts is designed to resist offline password guessing more effectively than fast general-purpose hashes. |
| S4 | **A** | [Day 06 — Encryption](day-06-hashing-vs-encryption.md#3-under-the-hood-encryption) | AES-GCM authenticates ciphertext and associated data; modifications should cause authentication failure. |
| S5 | **B** | [Day 07 — Symmetric Encryption](day-07-symmetric-vs-asymmetric.md#3-under-the-hood-symmetric-encryption) | Symmetric algorithms such as AES are efficient for bulk encryption. |
| S6 | **A** | [Day 07 — Digital Signatures](day-07-symmetric-vs-asymmetric.md#b-digital-signatures) | The signer uses a private key, and recipients verify using the corresponding authentic public key. |
| S7 | **C** | [Day 07 — Key Agreement](day-07-symmetric-vs-asymmetric.md#c-key-agreement-establish-a-shared-secret) | Ephemeral key agreement provides forward secrecy, protecting recorded sessions against later compromise of the server's long-term private key under the relevant assumptions. |
| S8 | **B** | [Day 07 — Hybrid Cryptography](day-07-symmetric-vs-asymmetric.md#5-hybrid-cryptography-how-modern-https-works) | Modern TLS combines public-key authentication/key establishment with efficient symmetric authenticated encryption. |
| S9 | **C** | [Day 08 — Chain of Trust](day-08-certificates-pki.md#chain-of-trust) | Certificate validation requires a valid chain to a trusted root, appropriate hostname, validity period, and other applicable checks. |
| S10 | **A** | [Day 08 — SAN](day-08-certificates-pki.md#the-san-trap) | The requested hostname must be covered by the certificate's SAN. |
| S11 | **D** | [Day 08 — Certificate Validation](day-08-certificates-pki.md#what-does-a-ca-validate) | HTTPS protects the connection but does not guarantee the honesty or legitimacy of the website operator. |
| S12 | **A** | [Day 08 — PKI Roles](day-08-certificates-pki.md#3-the-pki-cast-of-characters) | A CA issues and signs certificates after the required validation process. |
| S13 | **B** | [Day 09 — CSRs](day-09-certificate-lifecycle.md#3-generate-keys-and-create-a-csr) | A CSR requests a certificate containing a public key and identity information. The private key is not submitted. |
| S14 | **C** | [Day 09 — Renewal](day-09-certificate-lifecycle.md#5-monitor-renew-replace-revoke) | Expiration monitoring and tested renewal/deployment processes help prevent certificate-related outages. |
| S15 | **A** | [Day 09 — Revocation](day-09-certificate-lifecycle.md#6-revocation-crl-ocsp-and-stapling) | CRLs and OCSP communicate certificate revocation status, subject to client support and validation behavior. |
| S16 | **D** | [Day 09 — Key Compromise](day-09-certificate-lifecycle.md#7-real-world-case-private-key-on-github) | Deleting an exposed secret from a repository does not restore its secrecy. Replacement and revocation are appropriate responses. |
| S17 | **B** | [Day 10 — Envelope Encryption](day-10-key-management.md#5-envelope-encryption-dek-and-kek) | Envelope encryption separates bulk data encryption from centralized protection of DEKs using KEKs. |
| S18 | **C** | [Day 10 — HSM vs. TPM vs. KMS](day-10-key-management.md#4-hsm-vs-tpm-vs-kms) | An HSM is dedicated hardware designed for secure key storage and sensitive cryptographic operations. |
| S19 | **D** | [Day 10 — Recovery and Destruction](day-10-key-management.md#6-key-escrow-recovery-and-destruction) | Key destruction can make encrypted data permanently inaccessible, so retention and recovery obligations must be considered first. |
| S20 | **A** | [Day 10 — Envelope Encryption](day-10-key-management.md#5-envelope-encryption-dek-and-kek) | If an attacker already has the DEK, changing its wrapping key does not remove the attacker's ability to use that DEK. |

</details>

## Your Week 02 Scorecard

Record your results by theme.

| Theme | Questions | Your Score |
| --- | --- | --- |
| [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md) | S1–S4 | ____ / 4 |
| [Day 07 — Symmetric vs. Asymmetric Cryptography](day-07-symmetric-vs-asymmetric.md) | S5–S8 | ____ / 4 |
| [Day 08 — Certificates & PKI](day-08-certificates-pki.md) | S9–S12 | ____ / 4 |
| [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md) | S13–S16 | ____ / 4 |
| [Day 10 — Key Management](day-10-key-management.md) | S17–S20 | ____ / 4 |
| **TOTAL** | **20 questions** | **____ / 20** |

### Interpreting your results

| Score per theme | Interpretation | Recommended Action |
| --- | --- | --- |
| **4/4** | Strong understanding demonstrated | Continue to cumulative questions and revisit the theme during later spaced practice. |
| **3/4** | Mostly understood | Review the specific concept you missed using the targeted lesson link. |
| **0–2/4** | Knowledge gaps remain | Revisit the full lesson, worked examples, and optional lab before moving on. |

**Remember:** A perfect score on four questions is encouraging, but it does not establish complete mastery. Use the cumulative scenarios below to test whether you can connect and apply what you've learned.

### Compare Your Progress

If you completed the [Week 02 Diagnostic](00-diagnostic.md), compare your results.

| Theme | Diagnostic Score | Review Score |
| --- | --- | --- |
| Hashing vs. Encryption | ____ / 4 | ____ / 4 |
| Symmetric vs. Asymmetric Cryptography | ____ / 4 | ____ / 4 |
| Certificates & PKI | ____ / 4 | ____ / 4 |
| Certificate Lifecycle | ____ / 4 | ____ / 4 |
| Key Management | ____ / 4 | ____ / 4 |
| **TOTAL** | **____ / 20** | **____ / 20** |

**Reflection:** Where did your understanding improve? Which concepts still need practice? Did you learn something you thought you already understood?

---

## Part 3 — Cumulative Application

These five questions combine concepts from multiple Week 02 lessons.

They are designed to assess your ability to **apply and connect security concepts**, rather than simply recognize definitions.

Choose the single BEST answer for each question.

### C1

**A law firm encrypts confidential client documents and publishes SHA-256 hashes so recipients can verify file integrity. An attacker compromises the website hosting both the files and their published hashes. What is the MOST important limitation of the integrity check?**

A. SHA-256 automatically decrypts encrypted documents  
B. The attacker may replace both the file and its expected hash, making an untrusted comparison appear valid  
C. Hashing guarantees that the recipient knows who created the document  
D. Encryption prevents all modifications to ciphertext

### C2

**An administrator discovers that the private key for a law firm's HTTPS certificate was published in a public repository. The certificate is still valid for another six months. What should the organization do FIRST?**

A. Continue using the certificate because it has not expired  
B. Delete the repository and assume the key is safe  
C. Treat the key as compromised and begin replacing the key and certificate while assessing exposure  
D. Change the website's DNS name without replacing the key

### C3

**A company uses envelope encryption for cloud backups. An attacker steals the KEK but does not obtain the encrypted DEKs or the backups themselves. Which statement is MOST accurate?**

A. Every backup is automatically exposed as plaintext  
B. The compromise is irrelevant because KEKs are public  
C. The KEK compromise threatens the protection of wrapped DEKs; administrators should assess access paths, rotate affected key material, and investigate possible exposure  
D. The company must immediately destroy every backup

### C4

**A browser receives a certificate with a valid signature from a trusted CA, but the certificate's SAN does not include the requested hostname. What is the BEST conclusion?**

A. The browser should accept it because the CA signature is valid  
B. The certificate fails hostname validation even if its signature and chain are valid  
C. The server's symmetric encryption key is necessarily compromised  
D. The certificate must be expired

### C5

**A software company signs its updates using a private key stored in an HSM. Customers verify updates using the company's public key. Which statement BEST explains the security benefit and remaining limitation?**

A. The HSM guarantees that all signed software is free of vulnerabilities  
B. Digital signatures encrypt the update and prevent anyone from reading it  
C. The HSM protects signing-key operations, and signatures support integrity and source assurance; neither guarantees the software itself is safe  
D. Public-key verification eliminates the need for software testing

### Cumulative Answer Key

<details markdown="1">
<summary><strong>Reveal cumulative answers and connected lessons — C1–C5</strong></summary>

| Question | Answer | Explanation | Connected Lessons |
| --- | --- | --- | --- |
| C1 | **B** | A hash comparison provides useful integrity evidence only when the expected digest comes from a trustworthy source. If an attacker controls both the file and reference hash, the comparison can be misleading. | [Day 06](day-06-hashing-vs-encryption.md), [Day 07](day-07-symmetric-vs-asymmetric.md) |
| C2 | **C** | A publicly exposed private key must be treated as compromised. Certificate validity dates do not make an exposed key safe. Replace affected credentials and evaluate the scope of exposure. | [Day 08](day-08-certificates-pki.md), [Day 09](day-09-certificate-lifecycle.md), [Day 10](day-10-key-management.md) |
| C3 | **C** | A stolen KEK can threaten wrapped DEKs if the attacker can obtain them. The organization must investigate access, rotate affected keys, and assess whether data was exposed. | [Day 06](day-06-hashing-vs-encryption.md), [Day 10](day-10-key-management.md) |
| C4 | **B** | Certificate validation includes hostname matching in addition to signature and chain validation. A trusted CA signature alone is insufficient. | [Day 07](day-07-symmetric-vs-asymmetric.md), [Day 08](day-08-certificates-pki.md) |
| C5 | **C** | An HSM can reduce the risk of signing-key exposure, while signatures provide integrity and source assurance. Neither proves the signed software is free of malicious code or vulnerabilities. | [Day 07](day-07-symmetric-vs-asymmetric.md), [Day 08](day-08-certificates-pki.md), [Day 10](day-10-key-management.md) |

</details>

### Cumulative Reflection

Choose **one scenario** above and explain your reasoning in 3–5 sentences.

1. What facts are directly observed?
2. Which cryptographic mechanisms or trust relationships are involved?
3. What additional evidence would you need before making a stronger claim?
4. Which control would you recommend, and what limitations would it have?

### Optional Hands-on Reinforcement

If you'd like to apply what you've learned, revisit one of these lessons and complete its practical activities:

- [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md)
- [Day 07 — Symmetric vs. Asymmetric Cryptography](day-07-symmetric-vs-asymmetric.md)
- [Day 08 — Certificates & PKI](day-08-certificates-pki.md)
- [Day 09 — Certificate Lifecycle](day-09-certificate-lifecycle.md)
- [Day 10 — Key Management](day-10-key-management.md)

These activities develop practical skills that multiple-choice questions alone cannot fully assess.

---

## Week 02 — Final Reflection

Before continuing, ask yourself:

- Can I explain the difference between hashing, encryption, and encoding?
- Can I identify when symmetric and asymmetric cryptography are appropriate?
- Can I explain how digital signatures and certificates establish trust?
- Can I describe the certificate lifecycle, including CSRs, renewal, expiration, and revocation?
- Can I distinguish HSMs, TPMs, and KMS solutions?
- Can I explain envelope encryption and the roles of DEKs and KEKs?
- Can I recognize the consequences of compromised cryptographic keys?

**If you can confidently explain and apply these concepts, you're ready to move forward.**

If some concepts still feel uncertain, use your answer-key links to revisit the relevant sections. You don't necessarily need to repeat an entire lesson.

## What's Next?

**Congratulations on completing Week 02 — Cryptography & Public Key Infrastructure!**

You've explored how cryptographic mechanisms protect confidentiality and integrity, how digital certificates establish trust, and how key management supports secure systems.

These concepts are foundational to secure communications, authentication, cloud security, and incident response.

**Ready to continue?** Move on to Week 03 and begin with its diagnostic assessment.

Remember: The goal isn't simply to finish the lessons. It's to understand the concepts well enough to recognize, explain, and apply them in real-world situations.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
