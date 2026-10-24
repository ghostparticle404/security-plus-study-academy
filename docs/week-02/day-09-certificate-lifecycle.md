[SecurityPlus_Lesson_09_Certificate_Lifecycle.md](https://github.com/user-attachments/files/33915234/SecurityPlus_Lesson_09_Certificate_Lifecycle.md)
# Day 09 — Certificate Lifecycle: CSRs, Renewal, Revocation & Key Compromise

> **Security+ SY0-701** · Domain 1 · Objective 1.4 · **Week 02, Lesson 4 of 5**  
> **Time:** 60–80 minutes · **Previous:** [Day 08 — Certificates and PKI](day-08-certificates-pki.md)  
> **Next:** Day 10 — Key Management, HSMs & Key Escrow

Yesterday we learned how certificates bind public keys to identities. Today: how do administrators request, deploy, maintain, and revoke them?

## 1. Learning objectives

By the end, explain the certificate lifecycle, inspect a CSR, distinguish expiration from revocation, compare CRLs and OCSP, and respond to private-key compromise.

## 2. Big idea: A certificate is a passport

A certificate is not permanent. It has a validity window, can be replaced, and can be **revoked early** if trust is compromised. Think of a passport: *issued, used, renewed, or canceled if stolen*. 🛂

**Lifecycle:** Generate key pair → Create CSR → CA validation → Issue certificate → Deploy → Monitor → Replace/renew → Revoke when necessary.

**Why it matters:** An expired certificate can stop clients from connecting to a working portal. A stolen private key can permit impersonation despite a certificate that still appears current.

## 3. Generate keys and create a CSR

A server first generates a **public/private key pair**. The private key must remain protected; the public key may be shared.

A **certificate signing request (CSR)**, commonly in **PKCS #10** format, contains the public key, requested identity information, optional requested extensions such as SAN, and a signature made using the corresponding private key.

**It never needs to contain the private key.** The CSR signature proves possession of that key, but does not prove control of a requested domain. That is why the CA must validate the request.

| Term | Meaning |
| --- | --- |
| **CSR** | Request to certify a public key and identity |
| **CA** | Certificate authority that validates and issues certificates |
| **SAN** | Subject Alternative Name: domain names/IPs the certificate covers |
| **Leaf certificate** | End-entity certificate used by a website or service |
| **Intermediate CA** | Signs leaf certificates under authority delegated from a root |

**Exam trap:** Sending a private key to a CA is not a normal part of requesting a TLS certificate.

## 4. Validate, issue, and deploy

For a **domain-validated (DV)** TLS certificate, the CA checks that the applicant controls the requested domain. This is not a background check guaranteeing that the website is honest.

After issuance, the server needs its leaf certificate, the corresponding private key, and appropriate intermediate certificates. The browser verifies the chain to a trusted root, hostname, dates, allowed usage, and other relevant conditions.

A certificate covering `portal.example.com` does **not** automatically cover `files.example.com`. A server missing a required intermediate certificate may also fail validation.

## 5. Monitor, renew, replace, revoke

**Not Before** and **Not After** specify a certificate's validity window. Administrators should inventory certificates, monitor expiry, renew early, and test the deployed replacement.

**ACME (Automatic Certificate Management Environment)** can automate domain validation, issuance, and renewal. Automation still needs alerts: failed DNS challenges, misconfigured scripts, and unsuccessful deployments can cause outages.

| Action | What happens | Typical reason |
| --- | --- | --- |
| **Renew/replace** | Obtain and deploy a new certificate | Expiration approaching |
| **Rekey** | Generate a new key pair | Rotation or suspected compromise |
| **Revoke** | Invalidate certificate before expiry | Key compromise or misissuance |
| **Expire** | Certificate passes its Not After date | Time elapsed |

**Important:** Replacing a certificate does not automatically revoke the old one. If its private key is exposed, generate a **new key pair**, replace the certificate, request revocation, and investigate.

## 6. Revocation: CRL, OCSP, and stapling

**CRL (Certificate Revocation List):** A CA-signed published list of revoked certificates. Clients can consult it, but lists may be large or updated periodically.

**OCSP (Online Certificate Status Protocol):** A client asks a responder about a certificate. Responses include *good*, *revoked*, or *unknown*. “Good” does not guarantee the site is trustworthy; it is a status response.

**OCSP stapling:** The TLS server includes a signed, time-limited OCSP response in the handshake, reducing separate client lookups.

**Limit:** Revocation enforcement varies by client and environment. Don't assume every browser will immediately reject a revoked certificate, particularly when status services are unavailable.

🧠 **Memory hook:** CRL = published no-fly list; OCSP = ask the desk about one passport; stapling = bring a signed status slip. ✈️

## 7. Real-world case: Private key on GitHub

A law firm's portal private key is accidentally pushed to a public repository. Its certificate doesn't expire for three months.

**Best response:** Treat the key as compromised. Activate incident response; identify affected systems; generate a fresh key pair; obtain and deploy a replacement certificate; request revocation of the exposed certificate; investigate access and possible misuse; and improve secret scanning and key storage.

Deleting the commit does not make the key secret again. Deleting the only working key before a tested replacement is ready can also cause an outage, so coordinate containment and recovery.

**CIA connection:** Impersonation threatens confidentiality and integrity. A botched certificate replacement threatens availability.

## 8. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| What belongs in a CSR? | _Write here._ |
| Why must the private key stay out of the CSR? | _Write here._ |
| What does a DV CA validate? | _Write here._ |
| Expiration vs. revocation? | _Write here._ |
| CRL vs. OCSP vs. stapling? | _Write here._ |
| Why isn't replacement alone enough after key compromise? | _Write here._ |

<details>
<summary><strong>Reveal completed notes table (model answers)</strong></summary>

| Prompt | Model answer |
| --- | --- |
| CSR | Public key, requested identity/extensions, and signature proving private-key possession. |
| Private key | Secret key stays protected; it signs the CSR without being embedded. |
| DV validation | Control of the requested domain, not the operator's trustworthiness. |
| Expiration/revocation | Validity window ended / CA invalidated certificate early. |
| CRL/OCSP/stapling | Published signed list / individual status query / server-provided signed status response. |
| Compromise | Old certificate may still be usable; rekey, revoke, investigate, and deploy safely. |

</details>

## 9. Hands-on OpenSSL lab — Inspect a CSR

**Time:** 20–25 minutes · **Local-only** · OpenSSL required. Never publish your private key or use this test key in production.

```bash
mkdir -p secplus-day09
cd secplus-day09
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out lab-private.pem
openssl req -new -key lab-private.pem -out lab.csr \
  -subj '/CN=lab.example.test' \
  -addext 'subjectAltName=DNS:lab.example.test'
openssl req -in lab.csr -noout -text -verify
```

Look for **Subject**, **Subject Public Key Info**, **SAN**, and the request's signature. The private key should not appear inside the CSR.

Create a **self-signed lab certificate** (modern OpenSSL):

```bash
openssl x509 -req -in lab.csr -signkey lab-private.pem \
  -out lab-cert.pem -days 7 -sha256 -copy_extensions copy
openssl x509 -in lab-cert.pem -noout -subject -issuer -dates -ext subjectAltName
```

`-addext` and `-copy_extensions` may be unavailable in older OpenSSL builds; if so, use an extension configuration file or a newer OpenSSL installation. This lab is not CA issuance.

**Questions:** Is this publicly trusted? What is its expiry? Did any CA verify your domain? Is the SAN present?

<details>
<summary><strong>Reveal lab answers</strong></summary>

No public CA was involved; this is self-signed and not automatically trusted. It expires roughly seven days after creation. No domain validation occurred. SAN should appear if the requested extension was copied. Never add disposable lab certificates to your normal trust store merely to bypass warnings.

</details>

## 10. Eight original Security+-style questions

Choose the **BEST** answer before expanding. These are original study questions, not real CompTIA items.

### Q1. Which item must NEVER be included in a CSR sent to a CA?

A. Public key
B. Requested domain
C. Private key
D. Requested SAN

<details><summary>Reveal answer and explanation</summary>

**C — Private key.** A CSR includes the public key and requested identity details and is signed using the private key. A, B, and D can be legitimate CSR information.

</details>

### Q2. A certificate expires tomorrow. What should the administrator do FIRST to avoid disruption?

A. Disable certificate validation
B. Obtain, deploy, and test a valid replacement
C. Publish the private key
D. Revoke the root CA

<details><summary>Reveal answer and explanation</summary>

**B.** Replace and test before expiry. A and C undermine security; D is unrelated and potentially disruptive.

</details>

### Q3. Which protocol requests certificate revocation status from a responder?

A. NTP
B. DNS
C. OCSP
D. AES

<details><summary>Reveal answer and explanation</summary>

**C — OCSP.** NTP synchronizes clocks, DNS resolves names, and AES encrypts data.

</details>

### Q4. A server's private key is leaked. The team installs a replacement certificate. What else is MOST important?

A. Revoke the old certificate and investigate
B. Keep using the old key for backups
C. Ignore the exposure until expiry
D. Share the key to warn users

<details><summary>Reveal answer and explanation</summary>

**A.** Replacement doesn't automatically revoke the old certificate. The compromised key must not be reused; investigate and revoke.

</details>

### Q5. What is the MAIN operational benefit of ACME?

A. Guarantees every website is honest
B. Automates certificate issuance and renewal workflows
C. Replaces TLS encryption
D. Stores plaintext passwords

<details><summary>Reveal answer and explanation</summary>

**B.** ACME automates certificate management. It does not certify a site's intentions or replace encryption.

</details>

### Q6. A certificate is still within its validity dates, but its CA invalidated it after a key compromise. It is:

A. Expired
B. Revoked
C. Encoded
D. Rehashed

<details><summary>Reveal answer and explanation</summary>

**B — Revoked.** Revocation happens before scheduled expiration. The other terms don't fit.

</details>

### Q7. What is OCSP stapling?

A. A server sends a signed status response during the TLS handshake
B. A server sends its private key to the client
C. A browser automatically trusts every status response
D. A technique that prevents all certificate theft

<details><summary>Reveal answer and explanation</summary>

**A.** Stapling supplies a signed, time-bounded status response. It does not reveal private keys or guarantee safety.

</details>

### Q8. A valid certificate's SAN lists only `portal.example.com`. Users connect to `files.example.com` and receive a warning. What is MOST likely?

A. Hash collision
B. Hostname mismatch
C. Password reuse
D. Key escrow failure

<details><summary>Reveal answer and explanation</summary>

**B — Hostname mismatch.** The requested DNS name is not covered by SAN. The other choices do not explain this warning.

</details>

**Score:** 7–8 correct: strong. 5–6: review lifecycle and revocation. 0–4: revisit Sections 3–6 and retry.

## 11. Discussion challenge

> A firm's TLS private key is accidentally published. A colleague suggests deleting the GitHub commit and renewing the certificate next month. What's wrong with that plan? Propose an ordered response that protects both security and service availability.

**Bonus:** Why might a revoked certificate still work for some clients?

## 12. Study from the Treadmill 🏃

**Exact lesson link:** [Professor Messer — Certificates (Security+ SY0-701, Objective 1.4)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/certificates-sy0-701/)

Focus on **CSRs, certificate authorities, certificate revocation, and OCSP**. While watching, answer: Why is a CSR not a certificate? Why isn't expiration the same as revocation? What happens when a private key is exposed?

## 13. One-minute recap

- **CSR:** Public key + requested identity + signature; never includes private key.
- **CA:** Validates and issues signed certificates.
- **Renewal/replacement:** Prevents expiry-related disruption.
- **Revocation:** Invalidates a certificate early.
- **CRL/OCSP:** Revocation-status mechanisms; client enforcement varies.
- **Compromised key:** Generate a new key, replace certificate, request revocation, investigate.

**Next:** Day 10 — Cryptographic Key Management, HSMs & Key Escrow.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.

*Independent educational material; not affiliated with or endorsed by CompTIA.*
