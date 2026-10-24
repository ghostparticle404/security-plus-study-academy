[day-05-non-repudiation-digital-signatures.md](https://github.com/user-attachments/files/33915158/day-05-non-repudiation-digital-signatures.md)
# Day 05 — Non-repudiation, Digital Signatures & Trust

> **CompTIA Security+ SY0-701** · Domain 1: General Security Concepts · Objectives **1.2** (fundamental security concepts) and **1.4** (cryptographic solutions)  
> **Level:** Beginner → early intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 04 — Audit Logs, Accountability & Incident Timelines](day-04-audit-logs.md)

Yesterday we learned to reconstruct events from logs—and why a username in an audit record is not proof of which person acted. Today we'll investigate a stronger kind of evidence: **digital signatures**.

## Learning objectives

By the end, you should be able to:

1. Define **non-repudiation** and explain its limitations.
2. Distinguish **hashing, digital signatures, and encryption**.
3. Describe how a **private key signs** and a **public key verifies**.
4. Explain how certificates help bind public keys to identities.
5. Recognize what changes—or does not change—when a signed document is altered.
6. Solve original Security+-style scenarios involving authenticity and integrity.

---

## 1. The big idea: "I never signed that!"

Imagine a law firm emails a contract to a client. Later, someone claims the contract was changed after approval. How could the firm evaluate that claim?

A handwritten signature can be forged. An email address can be spoofed. An audit log can be incomplete or manipulated.

A **digital signature** provides cryptographic evidence about **data integrity** and the use of a particular **private key**. When the public key is reliably associated with a person or organization, that evidence can support **source authentication** and **non-repudiation**.

**Non-repudiation** means having evidence that makes it difficult for a party to credibly deny an action or communication. It is not a magical guarantee that a particular human personally pressed a button.

**Memory hook:** An audit log says, "This account was involved." A valid digital signature says, "The matching private key signed these bytes." Establishing *who controlled that key* is another important question.

---

## 2. A cryptographic fingerprint: Hashing

A **cryptographic hash function** accepts input data and produces a fixed-length **digest**.

For example, SHA-256 outputs a **256-bit** digest, commonly displayed as 64 hexadecimal characters.

Important properties of a secure cryptographic hash include:

- **Deterministic:** Identical inputs produce identical outputs.
- **One-way:** Recovering the original input from its digest is computationally infeasible for appropriately chosen inputs.
- **Collision resistance:** Finding two different inputs with the same digest should be computationally infeasible.
- **Avalanche effect:** A small input change generally produces a very different-looking digest.

A hash helps detect whether bytes changed **when compared against a trusted expected digest**. But anyone can hash a file. A bare SHA-256 digest does **not** establish who authored it.

### Try this mental experiment

You download a software installer and compute its SHA-256 digest. You compare it to a digest from a trustworthy vendor channel.

- **Match:** Strong evidence the downloaded bytes match the reference.
- **Mismatch:** The files differ; investigate before running the installer.
- **Both file and digest from an attacker-controlled page:** A match proves little about authenticity.

**Exam trap:** Hashing is **not encryption**. There is no decryption key that restores a file from its SHA-256 digest.

---

## 3. Under the hood: How digital signatures work

Digital signatures combine cryptographic hashing with **asymmetric cryptography**.

A signer has two mathematically related keys:

- **Private key:** Kept secret and used to create signatures.
- **Public key:** Shared with verifiers and used to check signatures.

### Signing

1. Alice prepares a document.
2. A signature algorithm processes the document (often via a cryptographic digest).
3. Alice's **private key** creates the digital signature.
4. Alice sends the document and its signature.

### Verification

1. Bob receives the document and signature.
2. Bob obtains Alice's authentic **public key**.
3. Verification checks whether the signature is valid for **those exact signed bytes**.
4. Bob evaluates whether he trusts the identity associated with the key.

```text
ALICE (signer)                          BOB (verifier)
Document                               Received document
   |                                          |
   v                                          v
Hash / signature algorithm             Signature verification
   |                                    using Alice's PUBLIC key
   v                                          |
Alice's PRIVATE key --> Signature ------------+
                                                |
                                    VALID or INVALID
```

This is a **conceptual** diagram: actual signing algorithms differ. For example, RSA-PSS, ECDSA, and EdDSA do not all use identical internal steps.

**Do not confuse signing with public-key encryption.** For confidentiality, someone may encrypt to a recipient's public key so the recipient can decrypt with their private key. For a signature, the **signer signs with their private key**, and others **verify with the signer's public key**.

### What a valid signature tells you

Under sound algorithms and correct implementation, verification provides strong evidence that:

- The signed content has not changed since signing.
- The corresponding private key was used to generate the signature.

It **does not, by itself**, prove:

- The key belongs to the named person.
- The key was not stolen.
- The signer read or understood the document.
- The signer was legally authorized to approve it.
- The document is confidential.

---

## 4. Certificates: Whose public key is this?

An attacker could create a key pair and claim, "This is Alice's public key!" Bob needs a trustworthy way to associate a key with an identity.

A **digital certificate**, often using the **X.509** format, can bind a public key to an identity. A **certificate authority (CA)** signs the certificate after performing the validation appropriate to the certificate type.

A verifier may check:

1. The certificate's signature and chain to a trusted authority.
2. The certificate's validity period.
3. The identity or name it covers.
4. Whether revocation or other policy checks apply.
5. Whether the certificate is appropriate for the intended use.

A valid certificate helps establish a **trusted key-to-identity binding**, but it does not guarantee a key was never compromised or that the organization is honest.

We'll study PKI, certificate lifecycles, and revocation more deeply in later lessons.

---

## 5. Real-world example: The signed contract dispute

A client receives a digitally signed contract from a law firm.

**Scenario A — The document is modified after signing.**  
Verification should fail for the changed signed content. This is an **integrity** problem.

**Scenario B — The signature verifies, but the attorney denies signing.**  
The firm must investigate key custody, credential theft, signing-service records, delegated authority, and the document approval workflow. The signature alone does not prove the attorney was physically present.

**Scenario C — The signed contract is emailed without encryption.**  
The signature may verify, but the document may still be readable in transit or by unintended recipients. This is a **confidentiality** concern.

| Goal | Typical mechanism | What it provides |
| --- | --- | --- |
| Detect changed content | Trusted hash comparison | Integrity evidence |
| Verify a signed document | Digital signature | Integrity and evidence of private-key use |
| Bind public key to identity | Validated certificate / trusted key distribution | Identity assurance |
| Hide content from unauthorized readers | Encryption | Confidentiality |
| Record actions and context | Protected audit logs | Accountability and investigative context |

**Memory hook:** A signature is a tamper-evident wax seal, not an invisibility cloak. 🕯️

---

## 6. Digital signatures vs. electronic signatures

These terms are related but not identical.

An **electronic signature** is a broad concept: a typed name, a clicked approval, or a signature drawn on a screen may be an electronic signature depending on the system and applicable law.

A **digital signature** is a specific **cryptographic technique** using public/private keys.

Some electronic-signature platforms use digital signatures internally, together with identity checks, timestamps, consent records, and audit trails.

**Security+ exam tip:** When a question emphasizes **cryptographic integrity and origin verification**, think **digital signature**. When it emphasizes **keeping a message secret**, think **encryption**.

---

## 7. Guided notes — Write first, reveal later

Try completing the table without rereading the lesson. Then expand the model answers.

| Prompt | Your answer |
| --- | --- |
| Define non-repudiation in your own words. | _Write here._ |
| What does a cryptographic hash do? | _Write here._ |
| Which key creates a digital signature? Which verifies it? | _Write here._ |
| What two kinds of evidence does a valid signature provide? | _Write here._ |
| What does a certificate add to signature verification? | _Write here._ |
| Why does a signature not guarantee confidentiality? | _Write here._ |

<details>
<summary><strong>Reveal the completed notes table (model answers)</strong></summary>

| Prompt | Model answer |
| --- | --- |
| Non-repudiation | Evidence that makes it difficult to credibly deny an action or communication; its strength depends on identity, key custody, and supporting processes. |
| Hash | Produces a fixed-length digest useful for checking whether data matches a trusted reference. |
| Signing / verification keys | Sign with the **signer's private key**; verify with the **signer's public key**. |
| Valid signature evidence | Signed content integrity and evidence that the corresponding private key was used. |
| Certificate | Helps associate a public key with a validated identity through a trust framework. |
| No confidentiality | Signing does not encrypt or conceal document contents; separate encryption may be required. |

</details>

---

## 8. Hands-on lab: See integrity checking in action

**Time:** 10–20 minutes · **Tools:** Terminal on macOS or Linux.

This exercise uses **hashing**, not digital signing. It prepares you to understand why a digital signature binds a signature to specific content.

### Step 1 — Create a practice file

```bash
printf 'Payment approved: $100\n' > contract-demo.txt
```

### Step 2 — Compute its SHA-256 hash

**macOS:**

```bash
shasum -a 256 contract-demo.txt
```

**Linux (commonly available):**

```bash
sha256sum contract-demo.txt
```

Record the digest.

### Step 3 — Change one character

```bash
printf 'Payment approved: $900\n' > contract-demo.txt
```

Compute the hash again. It should be very different.

### Step 4 — Explain your finding

1. Did the file size necessarily change?
2. Why did the digest change?
3. Could an attacker edit the file **and** replace a hash stored alongside it?
4. What additional mechanism could help authenticate the expected digest or file?

<details>
<summary><strong>Reveal lab answers</strong></summary>

1. **No.** Both sample lines have the same length.
2. The content changed, so SHA-256 produces a different digest.
3. **Yes.** If both file and reference hash are attacker-controlled, an attacker can substitute both.
4. A **digital signature** or a hash distributed through an independently trusted channel can strengthen authenticity assurance.

**Key distinction:** The experiment demonstrates *hash-based integrity comparison*. It does not create or verify a digital signature.

</details>

### Optional extension

If you want to explore real digital signatures later, OpenSSL supports signing and verification with generated keys. We'll revisit that in the cryptography lab section once we've covered safe key generation and management.

---

## 9. Practice: Eight original Security+-style questions

Choose the **BEST** answer before opening each explanation. These are original practice questions, not CompTIA exam items.

### Q1

Which key should a software publisher use to digitally sign a release?

A. Customer's public key  
B. Publisher's private key  
C. Publisher's public key  
D. Customer's private key

<details><summary>Reveal answer and explanation</summary>

**B — Publisher's private key.** A signer creates the signature using their private key. Verifiers use the matching public key. The customer's keys are not used to sign the publisher's release.

</details>

### Q2

A signed PDF is changed after signing. What should occur when its signature is checked against the modified signed content?

A. Verification fails  
B. Signature automatically updates  
C. Document becomes encrypted  
D. Public key is destroyed

<details><summary>Reveal answer and explanation</summary>

**A.** The signed bytes no longer match the signature. Digital signatures do not automatically update, encrypt content, or destroy keys.

</details>

### Q3

A law firm wants to prevent unauthorized parties from reading a signed contract sent over a network. Which ADDITIONAL protection is needed?

A. Digital signature only  
B. Encryption  
C. File naming convention  
D. Audit log rotation

<details><summary>Reveal answer and explanation</summary>

**B — Encryption.** A signature supports integrity and source authentication, not confidentiality. Encryption protects readable content from unauthorized disclosure.

</details>

### Q4

An administrator downloads a tool and compares its SHA-256 hash with a trusted vendor reference. Which security property is MOST directly checked?

A. Availability  
B. Integrity  
C. Authorization  
D. Physical security

<details><summary>Reveal answer and explanation</summary>

**B — Integrity.** A matching cryptographic digest is strong evidence the bytes match the trusted reference. It does not guarantee availability or grant permissions.

</details>

### Q5

A digital signature verifies correctly, but investigators discover the signer's private key was stolen before the document was signed. What is the BEST conclusion?

A. The named person definitely signed it  
B. The document must be confidential  
C. Verification alone cannot establish which human used the key  
D. The signature is automatically a hash collision

<details><summary>Reveal answer and explanation</summary>

**C.** Verification supports evidence of key use and integrity, but stolen keys weaken attribution. A valid signature does not prove who operated the key or provide confidentiality.

</details>

### Q6

What is the PRIMARY role of a trusted digital certificate in a signature-verification workflow?

A. Bind a public key to an identity under a trust framework  
B. Replace all encryption keys with passwords  
C. Guarantee the signer's honesty  
D. Restore deleted documents

<details><summary>Reveal answer and explanation</summary>

**A.** Certificates help associate a public key with an identity. They do not guarantee honest behavior or perform backup recovery.

</details>

### Q7

Which statement about cryptographic hashes is MOST accurate?

A. They can be decrypted using a private key  
B. They always prove who created a file  
C. They produce fixed-length digests that can help detect changes  
D. They hide document contents

<details><summary>Reveal answer and explanation</summary>

**C.** Secure cryptographic hashes produce fixed-length digests. Hashing is not reversible encryption and does not, by itself, authenticate authorship or conceal content.

</details>

### Q8

A company wants strong evidence that a specific private key signed a document and that the signed data has not changed. Which technology BEST fits?

A. Network address translation  
B. Digital signature  
C. Load balancer  
D. Data compression

<details><summary>Reveal answer and explanation</summary>

**B — Digital signature.** Signature verification checks the signed content and corresponding public key. NAT, load balancing, and compression serve unrelated purposes.

</details>

### Score guide

- **7–8 correct:** Strong understanding. Explain why a valid signature isn't absolute proof of human identity.
- **5–6 correct:** Revisit signing vs. encryption and certificates.
- **0–4 correct:** Redraw the sign/verify workflow and retry tomorrow.

---

## 10. Discussion challenge

> A client receives a digitally signed contract. The signature verifies, but the attorney says they never approved the document. The law firm's audit log shows a successful login to the signing platform at the relevant time.

Discuss:

1. What does the valid signature establish?
2. What does the audit log establish—and what does it *not* establish?
3. Name **three additional pieces of evidence** you would investigate.
4. Which security controls might reduce this risk in the future?

**Bonus:** If the signed contract was accidentally sent to the wrong recipient, which CIA objective was threatened even if the signature remained valid?

---

## 11. Study from the Treadmill 🏃

**Watch these exact lessons:**

1. [Professor Messer — Non-repudiation (7:57)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/non-repudiation-sy0-701/) — evidence of origin and integrity.
2. [Professor Messer — Hashing and Digital Signatures (SY0-701, Objective 1.4)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/hashing-and-digital-signatures-sy0-701/) — hashing and signature verification.

**Active listening:** Which key signs? Why does a hash alone not establish authorship? What happens when signed bytes change?

## 12. One-minute recap and what's next

- **Non-repudiation:** Strong evidence against denying an action; not absolute human attribution.
- **Hash:** Fixed-length digest used for integrity comparisons.
- **Digital signature:** Signer's **private** key creates it; corresponding **public** key verifies it.
- **Certificate:** Helps bind a public key to an identity.
- **Encryption:** Protects confidentiality; signing alone does not.
- **Investigation:** Combine cryptographic evidence with logs, key custody, and context.

**Next: Day 06 — Hashing vs. Encryption: Fingerprints, Locked Boxes & Password Storage.**

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
