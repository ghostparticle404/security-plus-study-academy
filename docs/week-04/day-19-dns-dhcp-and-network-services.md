# Day 19 — DNS, DHCP and Network Services

**Security+ Study Academy | Week 04 | Day 19**

**Prerequisites:** [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md) and [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md)  
**Learning outcome:** Explain name resolution and address assignment.  
**Practical application:** Analyze a DNS query.  
**Assessment focus:** Service troubleshooting.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain how recursive and authoritative DNS servers resolve a name.
2. Interpret common DNS record types and recognize DNS security threats.
3. Describe the DHCP DORA exchange, leases, and relay agents.
4. Identify the ports and transport protocols used by DNS, DHCP, NTP, and directory services.
5. Troubleshoot name-resolution and address-assignment failures with safe, read-only commands.

## 1. Why these services matter

Imagine entering a friend's name in your phone rather than memorizing their phone number. **DNS** is the internet's contact list: it maps names such as `example.com` to information including IP addresses. **DHCP** is the hotel front desk: it assigns your device a temporary room number (an IP address) and tells it where the exits are (its default gateway). Both services help clients communicate, but they solve different problems.

**Security+ connection:** When a user says, “The Wi-Fi works, but the website won't load,” determine whether the failure involves local connectivity, an IP configuration, DNS resolution, routing, or the application itself. Avoid assuming every website failure is DNS.

## 2. DNS: From a name to an address

The **Domain Name System (DNS)** is a distributed, hierarchical naming system. A client typically asks a configured **recursive resolver** to find the records for a domain. The resolver can answer from cache or query other DNS servers.

### A typical recursive lookup

1. Your browser needs the address of `www.example.com`.
2. The operating system checks available local caches and configuration, then sends a DNS query to its configured resolver if needed.
3. If the recursive resolver has no usable cached answer, it may ask a **root name server** which servers know the relevant top-level domain (TLD).
4. The resolver asks a **TLD server** which authoritative name servers serve `example.com`.
5. It asks an **authoritative server** for the requested record (or follows referrals/aliases as necessary).
6. The resolver returns the answer and generally caches it according to its TTL and caching policy.
7. The client can then connect to the returned address using the appropriate transport and application protocols.

```text
Client ──query──> Recursive resolver
                    │ (cache miss)
                    ├──> Root: Where is .com?
                    ├──> .com TLD: Who serves example.com?
                    └──> Authoritative server: A for www.example.com?
Client <──answer── Recursive resolver
```

**Important:** This is a conceptual cache-miss path, not a guarantee that every lookup sends all three queries. Resolvers cache referrals and records; CNAMEs, DNSSEC validation, split-horizon DNS, and other factors can change the flow.

### DNS roles

| Component | What it does | Security relevance |
|---|---|---|
| Stub resolver | Client-side DNS lookup functionality | May rely on OS caches and configured resolvers |
| Recursive resolver | Retrieves and caches answers for clients | Can enforce filtering and validate DNSSEC |
| Root name server | Directs resolvers toward TLD infrastructure | Part of DNS hierarchy, not a directory of all website IPs |
| TLD name server | Refers queries to a domain's authoritative servers | Delegation integrity matters |
| Authoritative server | Publishes records for zones it serves | Unauthorized changes can redirect users |
| DNS cache | Stores answers temporarily | Poisoned or stale entries can misdirect traffic |

## 3. DNS records you need to recognize

| Record | Purpose | Example use |
|---|---|---|
| **A** | Name → IPv4 address | `www.example.com` → `192.0.2.20` |
| **AAAA** | Name → IPv6 address | `www.example.com` → `2001:db8::20` |
| **CNAME** | Alias from one name to another | `shop.example.com` → `store.example.net` |
| **MX** | Mail exchange servers, with priority | Deliver mail for a domain |
| **NS** | Nameservers for a zone/delegation | Identify authoritative DNS servers |
| **TXT** | Arbitrary text; often email policy and verification | SPF policy, domain verification |
| **PTR** | Reverse DNS: IP-related name lookup | Mail-server checks and troubleshooting |
| **SOA** | Zone authority and administrative metadata | Zone serial and timing values |
| **SRV** | Service location, including host and port | Directory/service discovery |
| **CAA** | Which certificate authorities may issue certificates | Reduce unauthorized certificate issuance |

**Don't confuse:** An A record points to an IPv4 address; an AAAA record points to an IPv6 address. An MX record specifies mail routing, not a user's inbox. A PTR record supports reverse lookup; it does not prove that an IP address is trustworthy.

### TTL and caching

**Time to Live (TTL)** tells caching resolvers how long they may retain a DNS record. A short TTL can help changes propagate more quickly but increases lookup frequency. A long TTL reduces repeated queries but may keep outdated answers longer. TTL is not a promise that every device updates instantly.

## 4. DNS ports, encryption, and trust

- **Traditional DNS:** UDP port **53** for many queries; TCP port **53** for cases such as larger responses, retries, and zone transfers.
- **DNS over TLS (DoT):** typically TCP port **853**.
- **DNS over HTTPS (DoH):** DNS carried over HTTPS, commonly TCP **443** (and potentially HTTP/3 over UDP 443).
- **DNSSEC:** Adds cryptographic signatures and a chain of trust to authenticate DNS data. It does **not** encrypt DNS queries or guarantee that a website itself is safe.

**Exam distinction:** DNSSEC = authenticity/integrity of DNS data; DoH/DoT = encryption of DNS traffic between participating endpoints. These protections address different problems and can be combined.

### DNS threats and defenses

| Threat | What happens | Defensive approach |
|---|---|---|
| DNS cache poisoning | Resolver stores a forged answer | DNSSEC validation, patched resolvers, secure configuration |
| DNS hijacking | DNS settings or records are changed maliciously | Account MFA, registrar locks, configuration monitoring |
| DNS tunneling | Data is encoded into DNS queries/responses | Query-pattern monitoring, egress controls, anomaly detection |
| DNS amplification | Spoofed queries trigger large responses toward a victim | Anti-spoofing controls, rate limiting, restricted recursion |
| Malicious domains | Attackers use deceptive names for phishing/C2 | DNS filtering, threat intelligence, user education |

**Practical judgment:** A valid DNSSEC signature authenticates the record's origin within DNS trust assumptions; it does not mean the domain owner is benevolent.

## 5. DHCP: Automatic address assignment

The **Dynamic Host Configuration Protocol (DHCP)** helps devices obtain network settings without manual configuration. For IPv4, a DHCP server commonly provides:

- An IPv4 address and subnet mask.
- A default gateway (router option).
- DNS resolver addresses.
- A lease duration and other optional settings.

### DORA: Discover, Offer, Request, Acknowledge

```text
New client                        DHCP server
    |                                  |
    |------ DHCPDISCOVER ------------->|
    |<----- DHCPOFFER -----------------|
    |------ DHCPREQUEST -------------->|
    |<----- DHCPACK -------------------|
    |                                  |
```

1. **Discover:** A client seeks available DHCP servers.
2. **Offer:** A server proposes an address and configuration.
3. **Request:** The client requests an offered configuration.
4. **Acknowledge:** The server confirms the lease.

**Ports:** DHCPv4 servers use UDP **67**; DHCPv4 clients use UDP **68**. Initial discovery is typically broadcast because the client does not yet have an address. A **DHCP relay agent** can forward DHCP messages across subnet boundaries; routers generally do not forward broadcasts by default.

### Leases, reservations, and scope

- **Lease:** Time-limited assignment; clients usually attempt renewal before expiration.
- **Scope/pool:** Addresses a DHCP server is permitted to assign.
- **Exclusion:** Address intentionally kept out of the dynamic pool.
- **Reservation:** An address consistently assigned to a particular client identity, often using its MAC address.
- **Static IP:** Address configured manually on a device; not the same as a DHCP reservation.

### DHCPv6 and IPv6

DHCPv6 uses UDP **546** (client) and **547** (server). IPv6 hosts may also use **SLAAC** (Stateless Address Autoconfiguration), based on Router Advertisements, rather than DHCPv6 for address assignment. IPv6 default-router information normally comes from Router Advertisements, not DHCPv6. Don't apply IPv4 DORA terminology mechanically to DHCPv6.

### DHCP security problems

**Rogue DHCP server:** An unauthorized server offers malicious settings, such as an attacker-controlled DNS resolver or gateway. **DHCP starvation:** An attacker attempts to exhaust the available IPv4 address pool. Switch features such as **DHCP snooping**, port security, and network access controls can help mitigate these attacks. DHCP snooping distinguishes trusted server-facing ports from untrusted client-facing ports.

## 6. Other network services to recognize

| Service | Typical port(s) | Main job | Troubleshooting/security note |
|---|---|---|---|
| NTP | UDP 123 | Synchronizes clocks | Bad time can disrupt logs and authentication |
| LDAP | TCP/UDP 389 | Directory queries | Prefer appropriate TLS protection for sensitive operations |
| LDAPS | TCP 636 | LDAP over TLS | Check certificates and directory configuration |
| Kerberos | TCP/UDP 88 | Ticket-based authentication | Requires reasonably synchronized clocks |
| SNMP | UDP 161; traps often UDP 162 | Device monitoring and alerts | Prefer SNMPv3 with authentication/privacy |
| SMB | TCP 445 | File/printer sharing | Restrict exposure and use modern SMB security |

These are common defaults, not universal guarantees. Network services may be reconfigured to different ports.

## 7. Troubleshooting: Ask the right question first

A useful order is **link → address → gateway → DNS → service**.

| Symptom | Possible cause | First check |
|---|---|---|
| No IPv4 address; self-assigned `169.254.x.x` | DHCP unavailable or unreachable | Interface status and DHCP lease |
| IP address exists, but local gateway unreachable | Wrong subnet, VLAN, Wi-Fi isolation, link issue | Address, mask, route, gateway reachability |
| IP connectivity works, but names fail | DNS resolver, configuration, filtering, record issue | Resolver settings and targeted DNS query |
| One hostname fails, others work | Missing/stale record, domain issue, application issue | Query that hostname and record type |
| Name resolves, but HTTPS fails | Routing, firewall, TLS, or application failure | Connection and certificate error |
| Login failures with inconsistent timestamps | Clock synchronization problem | Time/NTP configuration |

**APIPA note:** `169.254.0.0/16` is the IPv4 link-local range. It often appears when automatic configuration cannot obtain a DHCP lease, but it does not alone prove the DHCP server is broken. IPv6 link-local addresses are a separate concept.

## 8. Practical lab: Analyze a DNS query

**Goal:** Identify the resolver, record type, answer, and whether a failure is likely DNS-related. This is a read-only exercise; use your own machine and public example domains.

### Step A — Query an A record

On macOS or Linux (where `dig` is available):

```bash
dig example.com A
```

Look for:

- **QUESTION SECTION:** The name and record type requested.
- **ANSWER SECTION:** Returned A record(s), if any.
- **SERVER:** Which resolver answered.
- **Query time:** How long the lookup took.
- **Status:** `NOERROR`, `NXDOMAIN`, `SERVFAIL`, etc.

On Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type A
```

If `dig` is unavailable on macOS, try the built-in command:

```bash
nslookup example.com
```

### Step B — Compare record types

```bash
dig example.com AAAA
dig example.com MX
dig example.com NS
```

**Question:** Why might a successful A lookup not imply that an MX record exists? Because each record type answers a different question; a domain can have web-address records without publishing mail-exchange records.

### Step C — Query a specific resolver (optional)

```bash
dig @1.1.1.1 example.com A
```

This asks Cloudflare's public resolver rather than your default resolver. **Privacy note:** It sends your query to a third party; skip this step if your environment restricts external DNS or if you prefer not to share queries. A failure here may reflect firewall policy rather than a broken domain.

### Step D — Interpret response codes

| Response | Interpretation | Next step |
|---|---|---|
| `NOERROR` with A answer | Name resolved for that query | Check application connectivity if the website still fails |
| `NOERROR` without requested record | Name may exist, but requested type has no data | Check another record type |
| `NXDOMAIN` | Queried domain name does not exist according to the response | Verify spelling, delegation, and resolver behavior |
| `SERVFAIL` | Resolver could not complete the query | Check resolver health, upstream issues, DNSSEC validation |
| Timeout | No response within allowed time | Check network path, resolver address, firewall |

### Step E — Annotate the packet path

For a traditional DNS query over UDP, label this simplified journey:

```text
Laptop (stub resolver)
  ↓ UDP source ephemeral port → destination 53
Local gateway / network
  ↓
Recursive DNS resolver
  ↓ (if cache miss: additional DNS lookups)
Authoritative DNS server
  ↑
Recursive resolver returns answer
  ↑ UDP source 53 → destination client ephemeral port
Laptop uses returned IP for subsequent connection
```

**Layer mapping:** DNS is an application-layer protocol (OSI 7); UDP is transport (OSI 4); IP is network (OSI 3); Ethernet/Wi-Fi carry traffic on local links (OSI 2/1). Routers forward IP packets; they do not normally perform recursive DNS resolution merely because they are forwarding traffic.

### Optional Wireshark observation

If you have Wireshark and permission to capture your own traffic, apply the display filter:

```text
dns
```

Generate a lookup with `nslookup example.com` and identify query/response IDs, names, types, and response codes. If your system uses encrypted DNS, ordinary DNS messages may not be visible as plaintext; this is expected. Capture only traffic you are authorized to inspect.

## 9. Security+ scenarios

**Scenario 1:** A laptop receives `169.254.23.8/16` and cannot reach the corporate network. Start with DHCP reachability, VLAN, and interface configuration; don't begin by changing DNS records.

**Scenario 2:** A browser reports a DNS failure, but `dig example.com A` returns a valid address. The DNS lookup succeeded; next inspect application behavior, proxy settings, routing, filtering, and TLS.

**Scenario 3:** Users are redirected to a fake banking page after a network change. Check DNS settings, rogue DHCP offers, DNS record integrity, and resolver caches. A malicious gateway or proxy is also possible.

**Scenario 4:** A company wants to authenticate DNS records but also keep queries private on untrusted Wi-Fi. Consider DNSSEC validation **and** encrypted transport (DoH/DoT), while understanding the trust placed in the chosen resolver.

**Scenario 5:** A remote subnet's clients never obtain DHCPv4 leases, while local clients do. Investigate the DHCP relay configuration and any filtering between relay and server.

## 10. Knowledge check

Choose the **best** answer. The weekly diagnostic and post-check will be written after all five lessons are finalized.

**1. Which server role normally performs lookups on behalf of a client and caches answers?**
A. Authoritative server
B. Recursive resolver
C. Default gateway
D. DHCP relay

**2. Which DNS record maps a hostname to an IPv6 address?**
A. A
B. PTR
C. AAAA
D. MX

**3. A defender wants DNS answer authentication, not query encryption. Which mechanism is most directly relevant?**
A. DNSSEC
B. DHCP snooping
C. NAT
D. DoH

**4. Which DHCPv4 message generally follows DHCPOFFER in the DORA exchange?**
A. DHCPACK
B. DHCPREQUEST
C. DHCPDISCOVER
D. DHCPRELEASE

**5. A client has an automatically assigned `169.254.18.4` IPv4 address. What should you investigate first?**
A. MX record priorities
B. DHCP availability and local connectivity
C. TLS certificate expiration
D. Kerberos tickets

**6. Which port pair is correct for DHCPv4?**
A. UDP 53/853
B. TCP 67/68
C. UDP 67/68
D. UDP 546/547

**7. A client resolves a domain name but cannot establish an HTTPS connection. What is the best conclusion?**
A. DNS must be down
B. The DHCP server is necessarily rogue
C. Name resolution worked; investigate the connection or service
D. The A record must be absent

**8. Which switch security feature helps block unauthorized DHCP server responses on client-facing ports?**
A. DNSSEC
B. DHCP snooping
C. DNS over HTTPS
D. NTP

**9. Which response indicates that a queried name does not exist, according to the resolver's answer?**
A. `NOERROR`
B. `SERVFAIL`
C. `NXDOMAIN`
D. `REFUSED` only

**10. Which statement about DHCPv6 is correct?**
A. It always uses IPv4 DORA broadcasts
B. It assigns IPv6 default gateways in place of Router Advertisements
C. It uses UDP 546/547, and SLAAC is another IPv6 address-configuration method
D. It uses TCP port 445

### Answer key and explanations

| Q | Answer | Explanation |
|---|---|---|
| 1 | **B** | Recursive resolvers answer client requests and commonly cache results. Review [DNS lookup](#2-dns-from-a-name-to-an-address). |
| 2 | **C** | AAAA stores IPv6 address mappings. Review [record types](#3-dns-records-you-need-to-recognize). |
| 3 | **A** | DNSSEC authenticates DNS data; DoH encrypts DNS transport. Review [DNS trust](#4-dns-ports-encryption-and-trust). |
| 4 | **B** | Discover → Offer → Request → Acknowledge. Review [DORA](#dora-discover-offer-request-acknowledge). |
| 5 | **B** | IPv4 link-local addressing often signals DHCP trouble. Review [troubleshooting](#7-troubleshooting-ask-the-right-question-first). |
| 6 | **C** | DHCPv4 uses UDP server 67 and client 68. Review [DHCP](#5-dhcp-automatic-address-assignment). |
| 7 | **C** | A successful DNS answer does not establish that HTTPS works. Review [troubleshooting](#7-troubleshooting-ask-the-right-question-first). |
| 8 | **B** | DHCP snooping limits untrusted DHCP messages. Review [DHCP security](#dhcp-security-problems). |
| 9 | **C** | NXDOMAIN reports a nonexistent queried name. Review [response codes](#step-d-interpret-response-codes). |
| 10 | **C** | DHCPv6 uses UDP 546/547; SLAAC can configure IPv6 addresses. Review [IPv6](#dhcpv6-and-ipv6). |

## 11. Exam-day quick reference

| If the question mentions… | Think… |
|---|---|
| Name → IPv4 | DNS A record |
| Name → IPv6 | DNS AAAA record |
| Mail routing | DNS MX record |
| Reverse lookup | DNS PTR record |
| DNS answer authenticity | DNSSEC |
| Encrypted DNS transport | DoT or DoH |
| Automatic IPv4 settings | DHCPv4, DORA, UDP 67/68 |
| DHCP across subnets | DHCP relay agent |
| Rogue DHCP offers | DHCP snooping |
| IPv4 `169.254.x.x` | Link-local/APIPA; check DHCP/connectivity |
| IPv6 automatic configuration | SLAAC and/or DHCPv6 |
| Incorrect log timestamps | NTP/time synchronization |

**Takeaway:** DNS answers *“Where does this name point?”* DHCP answers *“What network settings should this device use?”* When troubleshooting, determine which question is failing before choosing a fix.

**Next:** [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md).

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
