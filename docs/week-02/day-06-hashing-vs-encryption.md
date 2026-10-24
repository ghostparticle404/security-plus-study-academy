[day-06-hashing-vs-encryption.md](https://github.com/user-attachments/files/33915166/day-06-hashing-vs-encryption.md)
# Day 06 — Hashing vs. Encryption: Fingerprints, Locked Boxes & Password Storage

> **Security+ SY0-701** · Domain 1 · Objective 1.4 · Beginner–intermediate · 60–75 minutes  
> Previous: [Day 05 — Digital Signatures](day-05-non-repudiation-digital-signatures.md)

Yesterday we explored digital signatures. Today: what makes **hashing**, **encryption**, and **encoding** different—and why password storage needs special care.

## 1. Learning objectives

By the end, you should be able to distinguish encryption, hashing, and encoding; define keys, ciphertext, and digests; explain AES and SHA-256; describe salts and password hashing; and select suitable protections in realistic scenarios.

## 2. Big idea: Three different tools

| Operation | Purpose | Reversible? | Example |
| --- | --- | --- | --- |
| **Encryption** | Confidentiality | Yes, with the correct key | Encrypted laptop drive |
| **Hashing** | Fixed-length digest, often for integrity checks | No decryption operation | SHA-256 file hash |
| **Encoding** | Change representation | Yes, without a secret key | Base64 |

**Memory hook:** Encryption is a **locked box** 🔐; hashing is a **fingerprint** 🫆; encoding is a **different alphabet**. Base64 is not encryption—it's data wearing sunglasses. 😎

**CIA connection:** Encryption helps confidentiality; trusted hash comparisons help integrity. Neither automatically ensures availability.

## 3. Under the hood: Encryption

**Plaintext** is readable information. An encryption algorithm transforms it using a **key** into **ciphertext**. Decryption with the appropriate key recovers the plaintext.

```text
plaintext + algorithm + key → ciphertext
ciphertext + algorithm + correct key → plaintext
```

**AES** is a widely used *symmetric* encryption algorithm: the same secret key is used for encryption and decryption. AES has a **128-bit block size** and supports **128-, 192-, and 256-bit keys**. “AES-256” refers to its key length, not its block size.

Encryption protects disk contents, backup archives, and network traffic. But **implementation matters**:

- **ECB** mode can reveal patterns and is generally unsuitable for bulk application-data encryption.
- **AES-GCM** provides *authenticated encryption*: confidentiality plus integrity/authenticity checks when used correctly.
- Reusing a nonce with the same key in GCM can seriously compromise security.

**Real-world limitation:** Full-disk encryption helps protect a *powered-off* stolen laptop if its unlock secrets remain protected. It may not protect files on an already-unlocked device.

## 4. Under the hood: Hashing

A cryptographic hash maps arbitrary-length input to a **fixed-length digest**. **SHA-256** outputs **256 bits**, commonly displayed as **64 hexadecimal characters**.

A secure cryptographic hash aims for:

- **Determinism:** Same bytes, same digest.
- **Preimage resistance:** Infeasible to recover a suitable input from its digest.
- **Collision resistance:** Infeasible to find two different inputs with the same digest.
- **Avalanche effect:** Tiny changes usually produce very different digests.

If a downloaded file matches a **trusted** vendor SHA-256 reference, you have strong evidence the bytes match. But if an attacker controls both the file and the posted hash, they can replace both. **A bare hash doesn't prove authorship.**

A simple checksum such as CRC32 catches accidental errors but is not designed to resist intentional manipulation like a cryptographic hash.

## 5. Passwords: Why SHA-256 alone isn't enough

You might think a one-way hash makes passwords safe. But an attacker with a stolen password database can **guess candidate passwords**, hash each guess, and compare outputs. SHA-256 is *fast*, which makes mass guessing easier.

### Salt

A **salt** is a unique, random, non-secret value incorporated into each password's hashing process.

- Identical passwords with different salts yield different stored outputs.
- Salts defeat reusable precomputed lookup tables, including **rainbow tables**.
- Salts are generally stored with the password hash; they are not encryption keys.

### Slow password hashing

Use a deliberately expensive password-hashing function:

| Algorithm | Why it matters |
| --- | --- |
| **Argon2id** | Modern, memory-hard password hashing; strong default where supported |
| **scrypt** | Memory-hard, increases offline guessing cost |
| **bcrypt** | Widely used; tune work factor and mind input-length limitations |
| **PBKDF2** | Iteration-based; common in compatibility/compliance settings |
| **SHA-256 alone** | Too fast for direct password storage |

An optional **pepper** is a separate secret kept outside the password database. It adds a layer but does not replace proper salting and slow hashing.

**Memory hook:** Salt makes every recipe unique; slow hashing makes every guess expensive. 🧂

## 6. Real-world case study: A law firm

| Requirement | Appropriate solution | Main reason |
| --- | --- | --- |
| Keep stolen laptop's case files unreadable | Full-disk encryption | Confidentiality |
| Check whether an evidence image changed | SHA-256 comparison against trusted digest | Integrity |
| Store employee portal passwords | Unique salts + Argon2id (or suitable alternative) | Resist offline guessing |
| Verify publisher of software update | Digital signature + trusted verification key | Authenticity and integrity |

A hash match doesn't establish a full chain of custody. A signature doesn't automatically hide data. An encrypted file isn't necessarily available if its key is lost.

## 7. Guided notes — Write first, reveal later

Complete the table from memory.

| Prompt | Your answer |
| --- | --- |
| Encryption vs. hashing vs. encoding? | _Write here._ |
| Define plaintext, ciphertext, and digest. | _Write here._ |
| SHA-256 digest size? | _Write here._ |
| Why not use unsalted SHA-256 for passwords? | _Write here._ |
| What do salts and slow hashing accomplish? | _Write here._ |
| What can AES-GCM provide beyond confidentiality? | _Write here._ |

<details>
<summary><strong>Reveal completed notes table (model answers)</strong></summary>

| Prompt | Model answer |
| --- | --- |
| Three operations | Encryption hides content and is reversible with a key; hashing makes a one-way fixed-length digest; encoding changes representation and is readily reversible. |
| Three terms | Plaintext = readable input; ciphertext = encrypted output; digest = hash output. |
| SHA-256 | 256 bits / 64 hexadecimal characters. |
| Unsalted SHA-256 | Too fast for guessing; identical passwords produce identical digests; precomputed tables are reusable. |
| Salt + slow hashing | Unique salts prevent reusable precomputation; Argon2id/scrypt/bcrypt/PBKDF2 increase guessing cost. |
| AES-GCM | Authenticated encryption can also detect tampering when implemented correctly. |

</details>

## 8. Hands-on lab — Hash and decode

**Time:** 15–20 minutes. Use macOS Terminal or a Linux shell. This is safe fictional data.

**Create a file:**

```bash
printf 'Invoice total: 100\n' > invoice.txt
```

**Compute SHA-256:**

macOS:
```bash
shasum -a 256 invoice.txt
```

Linux:
```bash
sha256sum invoice.txt
```

Record the digest. Now change the file:

```bash
printf 'Invoice total: 900\n' > invoice.txt
```

Compute the hash again. The file length is unchanged, but the digest changes dramatically.

**Decode Base64 (no secret key):**

macOS:
```bash
printf 'U2VjcmV0IG1lc3NhZ2U=' | base64 -D
```

Linux:
```bash
printf 'U2VjcmV0IG1lc3NhZ2U=' | base64 -d
```

The result is `Secret message`.

**Think like an analyst:** What did the changed digest demonstrate? Could an attacker replace both a file and its reference hash? Why isn't Base64 appropriate for protecting passwords?

<details>
<summary><strong>Reveal lab explanations</strong></summary>

The changed digest demonstrates different file bytes. An attacker controlling both a file and reference hash can substitute both. Base64 is reversible encoding without a secret key. Trustworthy hash distribution and cryptographic signatures provide additional assurance.

</details>

## 9. Eight original Security+-style questions

Try each before opening its answer. These are original practice items, not actual CompTIA questions.

### Q1
Which BEST protects confidential files on a powered-off stolen laptop?

A. SHA-256  
B. Full-disk encryption  
C. Base64  
D. Checksum  

<details><summary>Reveal answer and explanation</summary>

**B.** Encryption protects confidentiality when keys remain secure. Hashes/checksums detect changes; Base64 merely encodes.

</details>

### Q2
A download's SHA-256 matches a trusted vendor reference. What was MOST directly checked?

A. Confidentiality  
B. Availability  
C. Integrity  
D. Authorization  

<details><summary>Reveal answer and explanation</summary>

**C.** Matching trusted digests supports integrity. It does not hide data, provide uptime, or assign permissions.

</details>

### Q3
Which BEST protects stored passwords against rainbow tables and expensive offline guessing?

A. Unsalted SHA-256  
B. Base64  
C. Unique salts with Argon2id  
D. Plaintext  

<details><summary>Reveal answer and explanation</summary>

**C.** Salts defeat reusable precomputed tables, while Argon2id makes guesses costly. The others lack adequate password protection.

</details>

### Q4
What is the PRIMARY weakness of unsalted SHA-256 password storage?

A. It decrypts easily  
B. It is fast and identical passwords yield identical hashes  
C. It reveals plaintext automatically  
D. Hashes cannot be compared  

<details><summary>Reveal answer and explanation</summary>

**B.** Fast hashes enable high-volume guessing, and missing salts permit reusable precomputation. Hashing is not decryption.

</details>

### Q5
Which statement about Base64 is MOST accurate?

A. Symmetric encryption  
B. Cryptographic hash  
C. Reversible encoding  
D. Requires a private key  

<details><summary>Reveal answer and explanation</summary>

**C.** Base64 is a public representation format. It is not encryption or hashing and needs no secret key to decode.

</details>

### Q6
Which is MOST appropriate for efficient encryption of a large backup archive?

A. AES  
B. SHA-256  
C. Base64  
D. CRC32  

<details><summary>Reveal answer and explanation</summary>

**A.** AES is symmetric encryption for bulk data. SHA-256 and CRC32 are not encryption; Base64 is encoding.

</details>

### Q7
When used correctly, what additional protection does AES-GCM provide?

A. Guaranteed availability  
B. Integrity/authenticity checking  
C. Proof of human approval  
D. Automatic backup recovery  

<details><summary>Reveal answer and explanation</summary>

**B.** Authenticated encryption can detect tampering, provided keys, nonces, and tags are handled correctly. It doesn't provide the other guarantees.

</details>

### Q8
Two employees choose the same password but their stored password hashes differ. What BEST explains this?

A. Different random salts  
B. Base64  
C. Hash collision  
D. Identical unsalted SHA-256  

<details><summary>Reveal answer and explanation</summary>

**A.** Unique random salts produce different stored results even for identical passwords. The other answers don't explain this desirable outcome.

</details>

**Score guide:** 7–8 correct: strong understanding. 5–6: review salts and encryption modes. 0–4: revisit Sections 3–5 and retry.

## 10. Discussion challenge

> A law firm encrypts client PDFs, publishes SHA-256 hashes for evidence downloads, and stores employee passwords as unsalted SHA-256. Which practices are appropriate? Which is dangerous? What would you change, and why might hashes posted on a compromised website be misleading?

Use the CIA triad in your reasoning.

## 11. Study from the Treadmill 🏃

**Watch these exact lessons:**

1. [Professor Messer — Hashing and Digital Signatures (SY0-701, Objective 1.4)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/hashing-and-digital-signatures-sy0-701/) — hashing and integrity.
2. [Professor Messer — Encrypting Data (SY0-701, Objective 1.4)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/encrypting-data-sy0-701/) — practical encryption use cases.
3. [Professor Messer — Protecting Data (SY0-701, Objective 3.3)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/protecting-data-sy0-701/) — compare encryption, hashing, and related protections.

**Active listening:** Which operation is reversible with a key? Why must a reference hash be trusted? Why does password storage require more than fast SHA-256?

## 12. One-minute recap

- **Encryption:** Confidentiality; reversible with appropriate key.
- **Hashing:** Fixed-length digest; useful for integrity.
- **Encoding:** Reversible format conversion; no secrecy.
- **AES:** Symmetric encryption; 128-bit blocks, 128/192/256-bit keys.
- **SHA-256:** 256-bit digest; too fast for direct password storage.
- **Salt:** Unique random value that prevents reusable precomputed tables.
- **Argon2id and alternatives:** Deliberately costly password hashing.
- **AES-GCM:** Authenticated encryption can protect confidentiality and integrity.

**Next:** Day 07 — Symmetric vs. Asymmetric Encryption: One Key or Two?

---

*Independent educational resource; not affiliated with or endorsed by CompTIA.*
