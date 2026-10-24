[SecurityPlus_Lesson_08_Certificates_PKI.md](https://github.com/user-attachments/files/33915208/SecurityPlus_Lesson_08_Certificates_PKI.md)
# Day 08 — Digital Certificates & PKI: Who Can You Trust Online?

> **CompTIA Security+ SY0-701** · Domain 1 · Objective **1.4: Cryptographic solutions**  
> **Week 02 · Lesson 3 of 5** · Beginner → intermediate · **Estimated time:** 65–85 minutes  
> **Previous:** [Day 07 — Symmetric vs. Asymmetric Cryptography](day-07-symmetric-vs-asymmetric.md)  
> **Next:** Day 09 — Certificate Lifecycle, CSRs & Revocation

## 1. Learning objectives

By the end of this lesson, you should be able to:

1. Explain why an unverified public key is dangerous.
2. Identify the roles of a **certificate authority (CA)**, **registration authority (RA)**, and **public key infrastructure (PKI)**.
3. Read important fields of an **X.509 certificate**, including **SAN**, issuer, validity dates, public key, and key usage.
4. Trace a **root → intermediate → leaf** certificate chain.
5. Distinguish certificate **authentication** from encryption and from a website's trustworthiness.
6. Troubleshoot common browser certificate warnings.

## 2. The big idea: A public key needs an identity

Yesterday we learned that asymmetric cryptography uses a public/private key pair. But there is a catch: **how does your browser know the public key belongs to the website you intended to visit?**

An attacker could substitute their own key and impersonate a server. Strong encryption would then protect the connection **to the wrong party**.

A **digital certificate** is a signed statement binding a public key to an identity, often a domain name. A **certificate authority (CA)** issues that statement after performing required validation. **Public key infrastructure (PKI)** is the broader system of policies, people, technology, certificates, and processes that makes this trust model work.

🧠 **Memory hook:** A public key is like someone handing you a house key and saying, “Trust me, I'm the landlord.” A certificate is an ID issued by an authority your browser recognizes. Of course, even someone with a legitimate ID can still be a terrible landlord. 🏠

## 3. The PKI cast of characters

| Component | Job | Important nuance |
| --- | --- | --- |
| **CA (Certificate Authority)** | Validates and issues signed certificates | Trust in the CA is foundational |
| **RA (Registration Authority)** | May perform identity checks on a CA's behalf | Usually doesn't sign the final certificate |
| **Root CA** | Top-level trust anchor, often self-signed | Trusted because its certificate is installed in a trust store |
| **Intermediate CA** | Issues certificates under a root's authority | Limits routine exposure of root signing keys |
| **Leaf/end-entity certificate** | Identifies a website, service, or other subject | Used by the endpoint, not normally to issue other certificates |
| **Trust store** | Collection of trusted roots/policies | Managed by browsers, operating systems, or organizations |
| **CRL / OCSP** | Mechanisms for checking revocation status | Real-world client behavior varies |

A **self-signed** certificate is not automatically malicious. Root certificates are often self-signed, and organizations may deploy their own internal PKI. The important question is **whether the verifier has an appropriate reason to trust it**.

### Chain of trust

```text
Trusted Root CA
   └── signs Intermediate CA certificate
          └── signs portal.example.com certificate
                 └── server proves possession of its private key
```

A browser verifies signatures along the chain, checks relevant constraints, and determines whether the chain leads to an accepted trust anchor.

**Important:** A certificate's signature alone is not enough. The browser also needs a matching hostname, acceptable validity period, appropriate usage, and other policy checks.

## 4. Under the hood: What's inside an X.509 certificate?

Most HTTPS server certificates use the **X.509** format.

| Field | Meaning | Example |
| --- | --- | --- |
| **Subject** | Entity associated with the certificate | Website identity information |
| **Subject Alternative Name (SAN)** | Hostnames/IP addresses the certificate covers | `portal.example.com` |
| **Issuer** | CA that issued the certificate | An intermediate CA |
| **Not Before / Not After** | Validity window | Start and expiry dates |
| **Subject Public Key Info** | Public key and algorithm information | RSA or elliptic-curve public key |
| **Key Usage / Extended Key Usage** | Permitted cryptographic purposes | TLS server authentication |
| **Signature algorithm/value** | How the issuer signed the certificate | Certificate-signing metadata |
| **Serial number** | Identifier assigned by issuer | Used in management/revocation |

### The SAN trap

A certificate listing only `portal.example.com` does **not** automatically cover `files.example.com`.

A wildcard such as `*.example.com` typically covers one subdomain level, such as `portal.example.com`, but **not** the bare `example.com` or `a.b.example.com`.

**Exam tip:** For modern HTTPS hostname validation, focus on **SAN**, not merely the legacy Common Name field.

### What does a CA validate?

For a typical **domain-validated (DV)** TLS certificate, the CA validates control of the domain, not the moral character or business reputation of the website operator.

A valid HTTPS certificate **does not mean the website is safe, legitimate, or free of phishing**. Attackers can obtain valid certificates for domains they control.

## 5. How the certificate fits into TLS 1.3

Let's connect this lesson to Day 7.

1. The browser contacts `portal.example.com`.
2. Client and server negotiate TLS parameters and establish shared secrets, commonly using **ECDHE**.
3. The server sends its certificate chain.
4. The browser validates the chain, hostname, time, permitted usage, and applicable policies.
5. The server proves possession of its authentication private key by signing handshake data.
6. Both sides use derived **symmetric traffic keys** for authenticated encryption of application data.

```text
Certificate → trustworthy binding between public key and server identity
Private-key signature → proof server controls corresponding key
ECDHE → shared secret establishment
AES-GCM / ChaCha20-Poly1305 → protected application traffic
```

**Do not confuse these jobs.** The certificate does not itself encrypt every web page. TLS combines certificates, asymmetric authentication/key agreement, and symmetric encryption.

### Why a certificate warning matters

A browser warning might mean:

- **Expired/not yet valid:** The current time is outside the certificate's validity window.
- **Hostname mismatch:** The visited name is not covered by SAN.
- **Untrusted issuer:** The chain does not lead to a trusted root.
- **Incomplete chain:** The server omitted an intermediate certificate needed to build a valid path.
- **Revocation or policy failure:** The certificate may be unacceptable under applicable checks.
- **Incorrect device clock:** A bad local clock can make otherwise valid certificates appear invalid.

**Never teach users to routinely click “Proceed anyway.”** Report the warning and verify the destination through a trusted channel.

## 6. Real-world case study: The law firm's broken client portal

A law firm migrates its client portal from `portal.example.com` to `files.example.com`. The new server presents a valid, unexpired certificate whose SAN includes only `portal.example.com`.

Clients see a warning.

**Diagnosis:** This is a **hostname mismatch**, not evidence that AES encryption has failed.

**Correct response:**

1. Confirm the intended domain and server configuration.
2. Obtain/deploy a certificate covering `files.example.com`.
3. Verify the certificate chain and server private-key configuration.
4. Test access from a standard browser.
5. Remind staff and clients not to bypass certificate warnings.

### What if the private key was stolen?

A valid certificate might still be dangerous if its corresponding private key is compromised. The organization should generate a **new key pair**, replace the certificate, request revocation of the compromised certificate, investigate exposure, and follow its incident-response procedures.

**CIA connection:** Server impersonation can expose **confidentiality** and **integrity**. Certificate failures can also disrupt **availability** by preventing clients from connecting.

## 7. Guided notes — Write first, reveal later

Fill in the second column without scrolling up. Then open the completed model table.

| Prompt | Your answer |
| --- | --- |
| Why isn't a public key automatically trustworthy? | _Write here._ |
| What do CA, RA, and PKI mean? | _Write here._ |
| What is a certificate chain? | _Write here._ |
| Which certificate field identifies valid hostnames? | _Write here._ |
| Name three causes of browser certificate warnings. | _Write here._ |
| Does HTTPS prove a website is honest? Why? | _Write here._ |
| What is the difference between a certificate and an encryption key? | _Write here._ |

<details>
<summary><strong>Reveal completed notes table (model answers)</strong></summary>

| Prompt | Model answer |
| --- | --- |
| Public-key trust | An attacker can substitute a key; it must be reliably bound to the intended identity. |
| CA / RA / PKI | CA issues signed certificates; RA may perform validation; PKI is the larger trust-management system. |
| Chain | Signed certificates link an end entity through intermediates to a trusted root. |
| Hostname field | Subject Alternative Name (SAN). |
| Warning causes | Expiry, hostname mismatch, untrusted issuer, incomplete chain, incorrect clock, or applicable revocation/policy failure. |
| HTTPS honesty | No. TLS authenticates the server identity/domain under its trust model and protects transport, not the site's intentions. |
| Certificate vs. key | A certificate binds identity to a public key; a private key remains secret and is used for authentication/signing or other allowed operations. |

</details>

## 8. Hands-on lab: Inspect a real certificate

**Time:** 15–25 minutes · **Tool:** OpenSSL on macOS or Linux · **Network:** Internet connection required.

This lab makes a **read-only TLS connection** to `example.com`, a reserved example domain. It does not scan or exploit a system.

### Step 1 — Check OpenSSL

```bash
openssl version
```

### Step 2 — Retrieve the server certificate

```bash
openssl s_client -connect example.com:443 -servername example.com -showcerts </dev/null
```

Look for certificate-chain details and the `Verify return code`. A successful verification depends on your local trust configuration and network environment.

### Step 3 — Display readable certificate fields

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates -ext subjectAltName
```

You should see the **subject**, **issuer**, **validity dates**, and **SAN** if present.

### Step 4 — Think like an analyst

1. Does the certificate cover the hostname you requested?
2. Which organization issued it?
3. When does it expire?
4. Would a matching SAN alone prove the website is safe?
5. Why did we include `-servername example.com`?

<details>
<summary><strong>Reveal lab explanations</strong></summary>

1. Compare the requested hostname to SAN entries.
2. The issuer identifies the CA named in the certificate; chain validation is still required.
3. The `notAfter` field gives the expiration time.
4. **No.** Hostname coverage is necessary for authentication but not proof of website safety.
5. `-servername` sends **SNI (Server Name Indication)** so a server hosting multiple domains can select the appropriate certificate.

**Important:** The second command displays certificate fields; it is **not** by itself a complete hostname-verification test. To request explicit OpenSSL hostname verification, use a supported command such as:

```bash
openssl s_client -connect example.com:443 -servername example.com -verify_hostname example.com </dev/null
```

Inspect the verification result and any errors.

</details>

### Optional local extension

Generate a self-signed certificate in a disposable lab and observe that a browser will not automatically trust it. This is expected behavior, not proof that self-signed certificates are inherently malicious. Do **not** install a new root CA into your everyday trust store just for this exercise.

## 9. Eight original Security+-style questions

Choose the **BEST** answer before expanding. These are original study questions, not actual CompTIA exam items.

### Q1

A browser visits `files.example.com`, but the server certificate's SAN lists only `portal.example.com`. What is the MOST likely problem?

A. Expired symmetric key

B. Hostname mismatch

C. Hash collision

D. Incorrect password policy

<details>
<summary>Reveal answer and explanation</summary>

**B — Hostname mismatch.** The requested hostname must match a covered certificate name. The other options do not explain this certificate validation failure.

</details>

### Q2

What is the PRIMARY purpose of a trusted root CA certificate?

A. Encrypt every application packet

B. Serve as a trust anchor for certificate validation

C. Store every website's private key

D. Guarantee websites cannot host malware

<details>
<summary>Reveal answer and explanation</summary>

**B.** A trusted root provides an accepted trust anchor. Traffic encryption uses negotiated keys; roots do not store all server private keys or guarantee website safety.

</details>

### Q3

Which certificate field is MOST relevant when checking whether a certificate covers a requested DNS hostname?

A. Serial number

B. Issuer

C. Subject Alternative Name

D. Signature length

<details>
<summary>Reveal answer and explanation</summary>

**C — SAN.** Modern hostname validation relies on SAN entries. Issuer and serial number serve other roles.

</details>

### Q4

A company's HTTPS certificate has expired. What should administrators do?

A. Disable certificate validation for clients

B. Deploy a valid replacement certificate and verify configuration

C. Publish the private key

D. Turn off TLS

<details>
<summary>Reveal answer and explanation</summary>

**B.** Replace the expired certificate and verify the hostname and chain. The other options weaken security or break encrypted transport.

</details>

### Q5

An attacker creates a phishing site and obtains a valid DV certificate for the domain they control. Which statement is MOST accurate?

A. The CA has certified the site is trustworthy

B. HTTPS means the site cannot steal passwords

C. The certificate can be valid while the site is malicious

D. A valid certificate guarantees the business identity was verified

<details>
<summary>Reveal answer and explanation</summary>

**C.** Domain validation establishes control of the domain, not good intentions or full business identity. HTTPS can protect a connection to a malicious site.

</details>

### Q6

What is the MAIN role of an intermediate CA?

A. Replace the need for a root trust anchor

B. Issue certificates under authority delegated through the CA hierarchy

C. Encrypt application data with AES

D. Store users' plaintext passwords

<details>
<summary>Reveal answer and explanation</summary>

**B.** Intermediate CAs issue certificates within a chain of trust. They do not eliminate root trust or perform application-data encryption.

</details>

### Q7

A TLS server's private key is confirmed stolen. What is the BEST response?

A. Wait for the certificate to expire

B. Keep using the key because the certificate remains valid

C. Generate a new key pair, replace the certificate, request revocation, and investigate

D. Publish the private key to warn clients

<details>
<summary>Reveal answer and explanation</summary>

**C.** Key compromise requires replacement and incident response. A certificate can remain within its validity dates while its key is compromised.

</details>

### Q8

During a TLS 1.3 connection, which statement BEST describes the certificate's role?

A. It encrypts every packet using RSA

B. It helps authenticate the server's public key and identity

C. It replaces symmetric encryption

D. It automatically proves the user is authorized to access every file

<details>
<summary>Reveal answer and explanation</summary>

**B.** The certificate helps establish trusted server identity. TLS uses key agreement and symmetric authenticated encryption for application traffic; authorization is separate.

</details>

**Score guide:** 7–8 correct: explain certificate trust without notes. 5–6: revisit Sections 3–5. 0–4: redraw the certificate chain and retry.

## 10. Discussion challenge

> A paralegal receives a certificate warning while accessing the firm's client portal. A coworker says, “It's fine—just click through; we know the website.”

Explain:

1. Why is bypassing the warning risky?
2. Which certificate fields and trust checks would IT investigate?
3. What if the certificate is valid but the website is a phishing site?
4. How does this connect to confidentiality, integrity, and availability?

## 11. Study from the Treadmill 🏃

**Watch these specific Professor Messer SY0-701 lessons:**

1. [Certificates — Objective 1.4](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/certificates-sy0-701/) — X.509 certificates, CAs, and revocation.
2. [Public Key Infrastructure — Objective 1.4](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/public-key-infrastructure-sy0-701/) — certificate hierarchies and trust.

**Active-listening prompts:**

- What is the difference between a root and an intermediate CA?
- Why does a certificate need SAN entries?
- Why can a phishing site have a valid HTTPS certificate?

## 12. One-minute recap

- **PKI:** Infrastructure for managing public-key trust.
- **CA:** Issues signed certificates after appropriate validation.
- **RA:** May verify identities on behalf of a CA.
- **X.509:** Common certificate format for TLS.
- **SAN:** Hostnames/IP addresses covered by a certificate.
- **Chain of trust:** Leaf → intermediate → trusted root.
- **Certificate ≠ encryption:** The certificate binds identity to a public key; TLS uses additional mechanisms to establish and encrypt a session.
- **Valid HTTPS ≠ safe website.** Trust the identity check, not the site's intentions.

**Next:** Day 09 — Certificate Lifecycle: CSRs, Renewal, Revocation & Key Compromise.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.

*Independent educational resource. Not affiliated with or endorsed by CompTIA.*
