# Day 07 — Symmetric vs. Asymmetric Cryptography: One Key or Two?

> **CompTIA Security+ SY0-701** · Domain 1 · Objective **1.4: Cryptographic solutions**  
> **Week 02 · Lesson 2 of 5** · Beginner → intermediate · **Estimated study time:** 60–80 minutes  
> **Previous:** [Day 06 — Hashing vs. Encryption](day-06-hashing-vs-encryption.md)  
> **Next:** Day 08 — Certificates and Public Key Infrastructure (PKI)

## 1. Learning objectives

After this lesson, you should be able to:

1. Distinguish **symmetric encryption** from **asymmetric (public-key) cryptography**.
2. Identify the roles of **AES, RSA, elliptic-curve cryptography (ECC), ECDHE, and ECDSA**.
3. Explain **key distribution**, **key agreement**, **digital signatures**, and **hybrid encryption**.
4. Trace a simplified **TLS 1.3** connection without confusing signatures, key exchange, and bulk encryption.
5. Choose an appropriate cryptographic technique in an exam scenario.

## 2. The big idea: One shared house key, or a public mailbox?

Suppose two colleagues need to exchange confidential case files.

**Symmetric encryption:** They share one secret key. The same key encrypts and decrypts. It is fast, but **how do they safely exchange that key?**

**Asymmetric cryptography:** Each person has a **public/private key pair**. A public key may be distributed; the private key must be protected. Public-key systems can support **encryption, signatures, or key agreement**, depending on the algorithm.

| Property | Symmetric | Asymmetric |
| --- | --- | --- |
| Keys | Shared secret key | Public/private pair |
| Speed | Generally fast | Generally slower or more computationally expensive |
| Common purpose | Bulk-data encryption | Authentication, signatures, key establishment; sometimes encrypting small secrets |
| Examples | AES | RSA, ECC-based algorithms |
| Main challenge | Secure key sharing and management | Authenticating public keys and protecting private keys |

🧠 **Memory hook:** Symmetric is **one house key shared by roommates**. Asymmetric is **a public mailbox anyone can drop mail into, but only its owner can open**. The mailbox metaphor illustrates *public-key encryption*, not every asymmetric algorithm.

## 3. Under the hood: Symmetric encryption

**AES (Advanced Encryption Standard)** is a symmetric **block cipher** with a **128-bit block size** and key sizes of **128, 192, or 256 bits**. AES-256 means a 256-bit *key*, not a 256-bit block.

A simplified conceptual flow:

```text
Alice's plaintext -- AES + shared secret key --> ciphertext
Bob's ciphertext -- AES + same secret key ----> plaintext
```

In real systems, encryption also needs a suitable **mode of operation** and appropriate nonce/IV handling. **AES-GCM** is an authenticated-encryption mode that provides confidentiality and integrity/authenticity checks when implemented correctly.

**Strengths:** Efficient for large files, backups, disks, and live network traffic.

**Limitations:** Anyone possessing the shared key may decrypt protected content. Key theft or mishandling compromises security. Encryption alone does not identify the human who used the key.

**Example:** A law firm encrypts a 200 GB evidence archive with AES-based authenticated encryption. Using RSA directly to encrypt that entire archive would be inefficient and generally inappropriate.

## 4. Under the hood: Asymmetric cryptography

A public/private key pair is mathematically related, but knowing the public key should not make recovering the private key computationally feasible.

### A. Public-key encryption

In a suitable public-key encryption scheme:

1. Bob shares his **public key**.
2. Alice encrypts a message or a small secret **for Bob** using Bob's public key.
3. Bob decrypts with his **private key**.

```text
Alice → encrypt with BOB'S PUBLIC key → ciphertext
Bob   → decrypt with BOB'S PRIVATE key → plaintext
```

**Critical condition:** Alice must authenticate Bob's public key. If an attacker substitutes their own key, Alice may unknowingly encrypt for the attacker.

**RSA-OAEP** is an example of a public-key encryption scheme. RSA is not normally used for large bulk files.

### B. Digital signatures

Recall Day 5:

```text
Signer:   sign with SIGNER'S PRIVATE key
Verifier: verify with SIGNER'S PUBLIC key
```

Digital signatures support signed-data integrity and evidence of private-key use. They do **not** encrypt or hide the document.

**RSA-PSS** and **ECDSA** are signature schemes. **ECDSA is not an encryption algorithm.**

### C. Key agreement: Establish a shared secret

**Diffie–Hellman (DH)** and **Elliptic-Curve Diffie–Hellman (ECDH)** allow participants to derive a shared secret over an untrusted network without transmitting the final secret directly.

**ECDHE** means *ephemeral* elliptic-curve Diffie–Hellman. Fresh temporary key material is used for the exchange, enabling **forward secrecy** when the protocol is implemented correctly.

**Forward secrecy:** If a server's long-term authentication key is compromised later, previously captured encrypted sessions should remain protected *provided the ephemeral session secrets were not also compromised*.

**Exam trap:** ECDHE is **key agreement**, ECDSA is **digital signing**, and AES is **symmetric encryption**. All may appear in a single secure connection but do different jobs.

## 5. Hybrid cryptography: How modern HTTPS works

A browser visiting a secure law-firm portal needs both a trustworthy server identity and efficient encrypted communication.

A simplified **TLS 1.3** handshake:

1. **ClientHello:** Browser proposes supported cryptographic parameters and provides key-share information.
2. **ServerHello:** Server selects parameters and provides its key share.
3. **Key agreement:** Client and server derive shared handshake/session secrets, commonly using **(EC)DHE**.
4. **Server authentication:** Server presents a certificate and proves possession of its authentication private key using a signature over handshake data.
5. **Protected traffic:** Both sides derive symmetric traffic keys and use an **AEAD cipher** such as AES-GCM or ChaCha20-Poly1305.

```text
Certificate + signature → authenticate server
(EC)DHE                  → establish shared secrets
Key derivation           → create traffic keys
AES-GCM / ChaCha20-Poly1305 → protect application data
```

**Nuance:** TLS 1.3 does **not** use RSA key transport. RSA may still appear in a certificate/signature, but not as the mechanism that transports the TLS session secret.

**Why hybrid?** Public-key techniques solve authentication and key-establishment problems; symmetric encryption efficiently protects large volumes of traffic.

### What HTTPS does not promise

HTTPS helps secure traffic **between endpoints** and authenticate the server to the extent its certificate and trust checks are valid. It does not guarantee that the website is ethical, malware-free, or safe to give your credit card to.

## 6. Real-world case study: Three law-firm needs

| Requirement | Best-fit mechanism | Reason |
| --- | --- | --- |
| Encrypt nightly 500 GB backups | Symmetric authenticated encryption (e.g., AES-GCM in a suitable system) | Efficient bulk confidentiality and integrity |
| Establish protected browser sessions | TLS 1.3 with (EC)DHE and symmetric traffic encryption | Secure key establishment plus efficient transport |
| Verify an attorney's signed document | Digital signature and trusted verification key | Signed-data integrity and source-authentication evidence |
| Let an outside consultant encrypt a small secret to the firm | Suitable public-key encryption or hybrid file-encryption tool | No need to pre-share a symmetric key |

**Threat model:** What if the consultant downloads a fake public key from an attacker-controlled page? The encryption can be mathematically sound while the intended recipient is wrong. **Public-key authenticity matters.**

## 7. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| How many keys are involved in symmetric encryption? | _Write here._ |
| What does AES-256 mean? | _Write here._ |
| Which key encrypts *for Bob* in public-key encryption? | _Write here._ |
| Which keys sign and verify a digital signature? | _Write here._ |
| What is ECDHE used for? | _Write here._ |
| Why does TLS combine asymmetric and symmetric techniques? | _Write here._ |
| What is forward secrecy? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Symmetric keys | One shared secret key (or keys derived from a common secret for different directions/purposes). |
| AES-256 | AES with a **256-bit key** and a **128-bit block size**. |
| Encrypt for Bob | Bob's **public** encryption key; Bob decrypts using his **private** key. |
| Signing | Signer's **private** key signs; matching **public** key verifies. |
| ECDHE | Ephemeral elliptic-curve key agreement to establish shared secrets. |
| Hybrid TLS | Asymmetric authentication/key establishment + fast symmetric traffic encryption. |
| Forward secrecy | Later compromise of long-term authentication keys does not, by itself, reveal earlier session traffic. |

</details>

## 8. Hands-on lab: Inspect a local key pair

**Time:** 15–20 minutes. Requires **OpenSSL** on macOS or Linux. This is a **local learning key only**; never use it for production or share its private key.

Check installation:

```bash
openssl version
```

Create a temporary folder and generate an elliptic-curve key pair:

```bash
mkdir -p secplus-day07
cd secplus-day07
openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-256 -out demo-private.pem
openssl pkey -in demo-private.pem -pubout -out demo-public.pem
```

Inspect **only the public key**:

```bash
openssl pkey -pubin -in demo-public.pem -text -noout
```

Create and sign a fictional message:

```bash
printf 'Approve training invoice: 100\n' > message.txt
openssl dgst -sha256 -sign demo-private.pem -out signature.bin message.txt
openssl dgst -sha256 -verify demo-public.pem -signature signature.bin message.txt
```

You should see `Verified OK`. Now modify the message:

```bash
printf 'Approve training invoice: 900\n' > message.txt
openssl dgst -sha256 -verify demo-public.pem -signature signature.bin message.txt
```

Verification should fail.

**What did you demonstrate?** A private key created a signature and its public key verified it. **You did not encrypt the message**, and you did not perform ECDHE key agreement.

<details>
<summary><strong>Reveal lab analysis</strong></summary>

1. **Why did verification fail?** The signed content changed.
2. **Does this prove a named human signed it?** No. The public key was generated locally without an identity certificate.
3. **Would sharing the public key be safe?** Yes, provided its authenticity can be established where needed.
4. **Would sharing the private key be safe?** No. Anyone possessing it could create signatures as that key.
5. **Why isn't this a TLS handshake?** We demonstrated ECDSA signing, not ephemeral key agreement or authenticated encrypted transport.

</details>

**Clean up:** Delete the practice directory when finished; don't reuse the training private key.

## 9. Practice: Eight original Security+-style questions

These are **original** practice items, not real CompTIA exam questions. Choose the **BEST** answer before expanding.

### Q1
Which algorithm is MOST suitable for efficiently encrypting a multi-terabyte backup?

A. ECDSA

B. AES

C. SHA-256

D. RSA-PSS

<details><summary>Reveal answer and explanation</summary>

**B — AES.** AES is efficient symmetric encryption. ECDSA and RSA-PSS are signature schemes; SHA-256 is a hash.

</details>

### Q2
Alice wants to encrypt a small secret so only Bob can decrypt it using a suitable public-key encryption scheme. Which key does Alice use?

A. Alice's private

B. Bob's private

C. Bob's public

D. Alice's public

<details><summary>Reveal answer and explanation</summary>

**C — Bob's public key.** Bob uses his private key to decrypt. The other keys do not achieve the intended recipient confidentiality.

</details>

### Q3
Which mechanism is commonly used in TLS 1.3 to establish shared secrets and provide forward secrecy?

A. ECDHE

B. Base64

C. CRC32

D. RSA key transport

<details><summary>Reveal answer and explanation</summary>

**A — ECDHE.** Ephemeral key agreement supports forward secrecy. Base64 is encoding; CRC32 is a checksum; TLS 1.3 does not use RSA key transport.

</details>

### Q4
A vendor digitally signs a software update. Which key should customers use to verify its signature?

A. Customer's private

B. Vendor's authenticated public

C. Vendor's private

D. Shared AES key

<details><summary>Reveal answer and explanation</summary>

**B.** The signer's public key verifies a signature. Its authenticity matters; the private key must remain secret.

</details>

### Q5
What is the MAIN reason modern HTTPS uses symmetric encryption after key establishment?

A. Symmetric encryption requires no keys

B. Symmetric encryption is efficient for bulk traffic

C. Signatures encrypt every packet

D. TLS disables authentication after login

<details><summary>Reveal answer and explanation</summary>

**B.** Symmetric AEAD ciphers efficiently protect application data. They still use keys, signatures do not encrypt traffic, and TLS authentication is not simply disabled.

</details>

### Q6
An attacker replaces Bob's public key with the attacker's own. Alice encrypts a secret using the substituted key. Which security failure BEST explains the problem?

A. Weak SHA-256 digest length

B. Unauthenticated public-key distribution

C. Excessive disk redundancy

D. Incorrect Base64 padding

<details><summary>Reveal answer and explanation</summary>

**B.** Alice failed to verify whose key she received. Even strong encryption can protect data for the wrong recipient.

</details>

### Q7
Which statement about AES-256 is correct?

A. It has a 256-bit block size

B. It is an asymmetric signature algorithm

C. It uses a 256-bit key and 128-bit blocks

D. It hashes passwords by default

<details><summary>Reveal answer and explanation</summary>

**C.** AES has 128-bit blocks and 128/192/256-bit keys. It is a symmetric block cipher, not a signature or password-hashing algorithm.

</details>

### Q8
A server's long-term authentication private key is stolen next year. Properly configured TLS 1.3 sessions from last year used ephemeral key agreement, and their ephemeral secrets were not compromised. Which property helps protect the recorded old sessions?

A. Forward secrecy

B. Non-repudiation alone

C. Key escrow

D. Encoding

<details><summary>Reveal answer and explanation</summary>

**A — Forward secrecy.** Later loss of a long-term authentication key does not alone reveal past ephemeral session keys. The other choices do not provide this protection.

</details>

**Score guide:** 7–8 correct: explain key agreement versus signing. 5–6: revisit Sections 4–5. 0–4: redraw the three flows (encrypt, sign, agree) and retry.

## 10. Discussion challenge

> Your organization needs to encrypt backups, exchange confidential files with an outside expert, and authenticate a public-facing client portal. Which cryptographic mechanisms would you choose for each? Where does **trust in a public key** become critical?

**Bonus:** Why would it be inaccurate to say, “TLS encrypts all website traffic with RSA”?

## 11. Study from the Treadmill 🏃

**Watch these exact lessons from Professor Messer's free SY0-701 course:**

1. **[Public Key Infrastructure — 9:08](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/public-key-infrastructure-sy0-701/)** — start here for symmetric vs. asymmetric encryption and key pairs.
2. **[Key Exchange — 3:39](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/key-exchange-sy0-701/)** — see why two systems need a shared secret and how they can establish one.
3. **[Encrypting Data — 9:48](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/encrypting-data-sy0-701/)** — connect encryption to disks, files, and network traffic.

**Optional longer treadmill session:** Watch all three (approximately 23 minutes).

**Active-listening prompts:**

1. Why is securely distributing a shared symmetric key difficult?
2. How is *key agreement* different from *encrypting a session key*?
3. Why is AES preferable to RSA for large files?
4. Where does trust in a public key enter the process?

> These links open the individual lesson pages containing the videos—not search-result pages.

## 12. One-minute recap

- **Symmetric:** Shared secret; efficient bulk encryption; **AES**.
- **Asymmetric:** Public/private key pair; encryption, signatures, or key agreement depending on algorithm.
- **Public-key encryption:** Encrypt to **recipient's public** key; recipient decrypts with **private** key.
- **Digital signatures:** Sign with **signer's private** key; verify with **public** key.
- **ECDHE:** Ephemeral key agreement; supports **forward secrecy**.
- **TLS 1.3:** Server authentication + shared-secret establishment + symmetric authenticated encryption.
- **Trust matters:** A public key must be reliably linked to the intended identity.

**Next:** Day 08 — Certificates and Public Key Infrastructure: Who Can You Trust Online?

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.

*Independent educational material. Not affiliated with or endorsed by CompTIA.*
