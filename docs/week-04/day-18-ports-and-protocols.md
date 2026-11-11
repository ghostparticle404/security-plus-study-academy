# Day 18 — Ports and Protocols: Finding the Right Door

**Security+ Study Academy | Week 04 · Day 18**  
**Topic:** Transport protocols, service ports, and secure alternatives  
**Prerequisite:** [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md)  
**Estimated study time:** 60–75 minutes  
**Learning outcome:** Associate network services with ports and identify secure alternatives  
**Practical application:** Inspect local network connections without changing system settings  
**Assessment focus:** Protocol selection

> **The big idea:** An IP address gets traffic to the right device. A **port number** helps the receiving device deliver that traffic to the right network service. But a port number is a clue, not a guarantee of what is actually running behind it.

## Learning objectives

By the end of this lesson, you should be able to:

1. Explain how IP addresses, transport protocols, and ports work together.
2. Distinguish TCP from UDP and select an appropriate transport for a scenario.
3. Recognize frequently tested ports and their associated services.
4. Identify safer alternatives to legacy plaintext services.
5. Interpret listening sockets and established connections on your own device.
6. Explain why open ports, encryption, and network exposure matter to defenders.

## 1. The apartment-building analogy 

Imagine a building with one street address and many apartments. The **IP address** identifies the building; the **port** identifies the apartment door. The **transport protocol** determines the rules for delivering a message.

A network conversation is commonly identified by a **five-tuple**:

```text
Source IP        192.168.1.25
Source port      53124
Destination IP   203.0.113.10
Destination port 443
Transport        TCP
```

The addresses above are documentation examples, not real destinations for an exercise. The source port `53124` is an illustrative temporary client port. Port `443` is the usual destination for HTTPS.

**Why the five-tuple matters:** A firewall, packet capture, or security log can use it to distinguish one connection or flow from another. The same host can have many simultaneous connections to the same service.

Recall [Day 16](day-16-osi-and-tcp-ip.md#osi-layers): TCP and UDP are usually discussed at **OSI Layer 4 (Transport)**. IP belongs to Layer 3. HTTP and DNS are application-layer protocols. **A port number is not an IP address or a protocol by itself.**

## 2. What is a port? 

TCP and UDP port numbers are **16-bit integers**, from `0` to `65535`. TCP port 53 and UDP port 53 are distinct transport endpoints.

| Range | Name | What it generally means |
|---|---|---|
| `0–1023` | Well-known / system ports | Commonly assigned to widely used services |
| `1024–49151` | Registered ports | Assigned to applications and vendors; not automatically trusted |
| `49152–65535` | Dynamic / private ports | Common range for temporary client-side ports; actual ephemeral ranges vary by operating system |

Port `0` is reserved, not an ordinary service port. A server commonly **listens** on a predictable destination port; a client commonly selects a temporary **source** port.

```text
Laptop                         Web server
192.168.1.25:53124  ------->   203.0.113.10:443
         client source port      service destination port
```

**Security connection:** Attackers may look for accessible listening services. Defenders limit exposure using host firewalls, network firewalls, access controls, secure configurations, and service hardening. An open port alone does **not** prove a vulnerability.

## 3. TCP versus UDP 

| Characteristic | TCP | UDP |
|---|---|---|
| Connection setup | Three-way handshake | No transport handshake |
| Reliability | Acknowledgments and retransmission | No built-in delivery guarantee |
| Ordering | In-order byte stream delivered to applications | No built-in ordering |
| Data boundaries | Byte stream, not preserved messages | Preserves datagram boundaries |
| Typical uses | HTTPS over TCP, SSH, SMTP, file transfers | Traditional DNS queries, DHCP, voice/video, QUIC |
| Security by default | **Not encrypted by itself** | **Not encrypted by itself** |

### TCP: The tracked delivery 

TCP starts a typical connection with:

```text
Client -> Server: SYN
Server -> Client: SYN-ACK
Client -> Server: ACK
```

Afterward, TCP manages sequence numbers, acknowledgments, flow control, and retransmission. TCP reliability is useful when missing or out-of-order bytes would break an application. **TCP does not provide confidentiality or authentication on its own**; protocols such as TLS supply those protections.

### UDP: The fast postcard 

UDP sends datagrams without a transport-layer connection setup or built-in retransmission. This makes it useful for low-latency applications and for protocols that implement their own reliability when needed.

**Exam trap:** UDP does not mean “always faster” or “always insecure.” **QUIC**, used by HTTP/3, runs over UDP and incorporates encryption and transport reliability mechanisms. DNS can use **UDP or TCP**, and encrypted DNS can use additional transports.

## 4. Ports and protocols to recognize 

The following are **common defaults**, not hard rules. Applications can use nonstandard ports, and the actual service must be verified from configuration or traffic.

| Port(s) | Transport | Common service | What to remember |
|---|---|---|---|
| `20/21` | TCP | FTP | Control typically `21`; classic active-mode data uses `20`; FTP is plaintext |
| `22` | TCP | SSH, SFTP, SCP | Encrypted remote administration and file transfer |
| `23` | TCP | Telnet | Plaintext remote terminal; avoid for administration |
| `25` | TCP | SMTP | Server-to-server mail transfer; STARTTLS may be negotiated |
| `53` | UDP/TCP | DNS | Name resolution; TCP also used for large responses and other operations |
| `67/68` | UDP | DHCPv4 | Server `67`, client `68` |
| `69` | UDP | TFTP | Simple file transfer without built-in authentication/encryption |
| `80` | TCP | HTTP | Plaintext web traffic unless separately protected |
| `88` | TCP/UDP | Kerberos | Network authentication |
| `110` | TCP | POP3 | Legacy mailbox retrieval; plaintext unless protected |
| `123` | UDP | NTP | Time synchronization |
| `135` | TCP | Microsoft RPC endpoint mapper | Windows RPC service discovery |
| `137/138` | UDP | NetBIOS name/datagram | Legacy Windows networking |
| `139` | TCP | NetBIOS session | Legacy file-sharing transport |
| `143` | TCP | IMAP | Mailbox access; plaintext unless protected |
| `161/162` | UDP | SNMP | Queries `161`, traps/informs commonly `162` |
| `389` | TCP/UDP | LDAP | Directory access; encryption depends on configuration |
| `443` | TCP | HTTPS (HTTP/1.1, HTTP/2) | HTTP over TLS |
| `443` | UDP | HTTP/3 over QUIC | Secure web transport over UDP |
| `445` | TCP | SMB | Windows file sharing; use modern SMB security controls |
| `465` | TCP | SMTP submission with implicit TLS | Common encrypted mail submission option |
| `514` | UDP (common) | Traditional syslog | Typically plaintext; secure syslog has other options |
| `587` | TCP | SMTP message submission | Commonly upgraded with STARTTLS |
| `636` | TCP | LDAPS | LDAP over TLS (implicit TLS) |
| `993` | TCP | IMAPS | IMAP over implicit TLS |
| `995` | TCP | POP3S | POP3 over implicit TLS |
| `1433` | TCP | Microsoft SQL Server | Common database listener |
| `1521` | TCP | Oracle database listener | Common database listener |
| `1812/1813` | UDP | RADIUS | Authentication/authorization and accounting |
| `2049` | TCP/UDP | NFS | Network file system; version and configuration matter |
| `3306` | TCP | MySQL | Common database listener |
| `3389` | TCP/UDP | RDP | Remote Desktop; protect access carefully |
| `5432` | TCP | PostgreSQL | Common database listener |
| `5900` | TCP | VNC | Remote graphical access; security varies by implementation |
| `5985/5986` | TCP | WinRM | HTTP / HTTPS-based management endpoints |

**Do not memorize blindly:** `443` usually suggests HTTPS, but an attacker can run a different application there. Conversely, HTTPS may run on a nonstandard port such as `8443`.

### Service families that exam questions mix up 

- **Web:** HTTP `80/TCP`; HTTPS `443/TCP`, or HTTP/3 over `443/UDP`.
- **Remote access:** Telnet `23/TCP` is plaintext; SSH `22/TCP` is the typical encrypted alternative. RDP `3389` is a different remote desktop protocol, not a replacement for SSH.
- **File transfer:** FTP `21/TCP` is generally plaintext; **SFTP over SSH** usually uses `22/TCP`. **FTPS** is FTP protected with TLS and is **not** the same protocol as SFTP.
- **Email:** SMTP transfers/sends mail; IMAP and POP3 retrieve/access it. Submission often uses `587/TCP` with STARTTLS or `465/TCP` with implicit TLS.
- **Directory and identity:** LDAP `389`, LDAPS `636`, Kerberos `88`, RADIUS `1812/1813`.
- **Infrastructure:** DNS `53`, DHCPv4 `67/68`, NTP `123`, SNMP `161/162`.

## 5. Choosing secure alternatives 

| Less secure or legacy option | Prefer, when supported | Important qualification |
|---|---|---|
| Telnet (`23`) | SSH (`22`) | Use strong authentication and restrict management access |
| FTP (`21`) | SFTP (`22`) or properly configured FTPS | SFTP and FTPS are different technologies |
| HTTP (`80`) for sensitive data | HTTPS (`443`) | Validate certificates; TLS version and configuration matter |
| POP3 (`110`) | POP3S (`995`) or POP3 with STARTTLS | Prefer modern mail authentication where available |
| IMAP (`143`) without TLS | IMAPS (`993`) or IMAP with STARTTLS | Reject insecure fallback when policy requires encryption |
| LDAP without TLS (`389`) | LDAPS (`636`) or LDAP with StartTLS | Protect credentials and directory traffic |
| SNMPv1/v2c | SNMPv3 with authentication and privacy | SNMPv3 security depends on enabled modes |
| SMBv1 | Modern SMB with signing/encryption as appropriate | SMBv1 is obsolete; modern SMB still needs correct configuration |
| Unprotected management exposed publicly | VPN/ZTNA plus restricted administration | A VPN is an access control layer, not a fix for every vulnerable service |

**STARTTLS versus implicit TLS:** With STARTTLS, a client initially connects using the service's conventional protocol and negotiates an upgrade to TLS. With implicit TLS, TLS begins at connection establishment. Both can be secure when correctly configured and validated.

**Security+ reasoning:** The question is often not “Which port is encrypted?” but **“Which protocol and configuration best protects the communication?”** Port `443` alone does not prove TLS is enabled or correctly configured.

## 6. Reading connection states 

A **socket** is an operating-system endpoint used by a network application. A listening TCP socket waits for incoming connection attempts; an established TCP connection has completed its handshake.

| State | Meaning | Defender interpretation |
|---|---|---|
| `LISTEN` / `LISTENING` | Service waiting for TCP connections | Determine whether it needs to be reachable and from where |
| `ESTABLISHED` | Active TCP connection | Check peer address, port, and owning process |
| `TIME_WAIT` | Recently closed TCP connection waiting before full reuse | Often normal; not automatically suspicious |
| `CLOSE_WAIT` | Peer closed; local application has not fully closed | Persistent accumulation can indicate an application issue |

UDP is connectionless at the transport layer, so UDP sockets generally do **not** have the same TCP connection-state lifecycle. Tools may display UDP endpoints without a `LISTEN` state.

### Example: Interpret a socket listing 

```text
Proto  Local Address          Foreign Address        State
TCP    127.0.0.1:5432         0.0.0.0:0              LISTEN
TCP    192.168.1.25:53124    203.0.113.10:443       ESTABLISHED
UDP    0.0.0.0:5353           *:*
```

What can we infer?

1. PostgreSQL commonly uses `5432`, but the port alone does not prove that PostgreSQL is running. This example binds to **loopback** (`127.0.0.1`), not every interface.
2. A local client has a TCP connection to remote port `443`. It **might** be HTTPS; confirmation requires more evidence.
3. UDP `5353` commonly indicates multicast DNS (mDNS). The wildcard address does not by itself establish whether traffic is allowed through a firewall.

**Critical distinction:** A service can be **listening locally** yet **not reachable remotely**, due to its bind address, firewall policy, network routing, or other controls.

## 7. Practical lab: Inspect your own connections 

**Goal:** Identify local listening ports, active connections, and possible services **without scanning anyone else's network**.

**Safety and scope:** Run these read-only commands on a computer you own or are authorized to administer. Do not disable a firewall or install software for this lesson. Results will vary by OS and current activity.

### macOS (including Intel Macs) 

Open Terminal and run **one command at a time**:

```bash
lsof -nP -iTCP -sTCP:LISTEN
```

This lists processes with listening TCP sockets. `-nP` avoids DNS and service-name lookup, so you see numeric addresses and ports.

Then inspect active TCP connections:

```bash
lsof -nP -iTCP -sTCP:ESTABLISHED
```

If you receive a permissions error or see incomplete results, some processes may require elevated permissions; **do not escalate privileges for this exercise**.

### Windows (PowerShell) 

```powershell
Get-NetTCPConnection -State Listen
```

```powershell
Get-NetTCPConnection -State Established
```

Look at `LocalAddress`, `LocalPort`, `RemoteAddress`, `RemotePort`, and `State`. Some process metadata may require administrator rights; that is not necessary here.

### Linux 

```bash
ss -ltn
```

```bash
ss -tn state established
```

If `ss` is unavailable, use your distribution's existing network tools rather than installing packages solely for this exercise.

### Record your observations 

Fill in **three** rows from your own results. You may redact public IP addresses before sharing notes.

| Observation | Local IP:port | Remote IP:port (if any) | State | Possible service | What you can actually confirm |
|---|---|---|---|---|---|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

**Reflection questions:**

1. Did you find a port in the well-known range? Which one?
2. Did you see a high-numbered client source port connected to a familiar service port?
3. Was a listener bound to `127.0.0.1`, a specific LAN address, or `0.0.0.0`?
4. What **additional evidence** would you need before claiming the connection is malicious?

**Success criterion:** You can identify a local endpoint, distinguish listening from established TCP sockets, and explain why a port number is only a starting hypothesis.

## 8. Security scenarios: Choose the right protocol 

### Scenario A: Remote server administration

A technician must manage a Linux server from an approved workstation. **SSH** is the typical choice over Telnet because it supports encrypted communication and authenticated sessions. Also limit source access and use approved authentication.

### Scenario B: Uploading confidential documents

A team needs a secure file transfer. **SFTP** is a common choice when SSH is available. Do not confuse it with **FTPS**, which secures FTP with TLS. Both can be suitable if configured properly.

### Scenario C: Mail client setup

A user must send messages through an authenticated submission service. **SMTP submission on `587/TCP` with required STARTTLS** is common; `465/TCP` with implicit TLS is another supported option. **Port 25** is usually associated with server-to-server SMTP delivery.

### Scenario D: Monitoring network equipment

A network team wants to poll switches without exposing community strings and monitoring data. Prefer **SNMPv3 with authentication and privacy**, not legacy SNMPv1/v2c community-string security.

### Scenario E: Suspicious outbound traffic

An endpoint makes an unexpected connection to an unfamiliar IP on `443/TCP`. You **cannot conclude** it is safe because it uses `443`, or malicious because the IP is unfamiliar. Correlate the owning process, destination reputation, DNS history, TLS details (when available), user activity, and endpoint alerts.

## 9. Common exam traps {#exam-traps}

- **“TCP = secure.”** False. TCP is reliable, not encrypted by default.
- **“UDP = no reliability anywhere.”** False. Applications and protocols such as QUIC can implement reliability above UDP.
- **“Port 443 guarantees HTTPS.”** False. Ports are conventions, not proof.
- **“SFTP = FTPS.”** False. SFTP uses SSH; FTPS uses TLS-protected FTP.
- **“DNS is only UDP.”** False. DNS uses both UDP and TCP, depending on operation and conditions.
- **“A listening port is Internet-exposed.”** False. Bind address and access controls matter.
- **“Any high port is malicious.”** False. Temporary client ports are normal.
- **“STARTTLS means plaintext forever.”** False. Correctly negotiated and validated STARTTLS encrypts subsequent communication.

## 10. Day 18 knowledge check 

Choose the **single best answer** for each question. Try without consulting the tables first.

**1. A web client connects from `192.168.1.25:53001` to `203.0.113.10:443` over TCP. Which is the client's source port?**

A. `443`  
B. `53001`  
C. `192.168.1.25`  
D. `203.0.113.10`

**2. Which transport protocol provides built-in sequencing, acknowledgments, and retransmission?**

A. UDP  
B. IP  
C. TCP  
D. ICMP

**3. A company wants encrypted command-line administration instead of Telnet. Which service is the best fit?**

A. SSH on `22/TCP`  
B. FTP on `21/TCP`  
C. HTTP on `80/TCP`  
D. TFTP on `69/UDP`

**4. Which statement about DNS transport is correct?**

A. DNS uses only UDP `53`.  
B. DNS uses only TCP `53`.  
C. DNS never uses port `53`.  
D. Traditional DNS commonly uses UDP and TCP `53`.

**5. Which pairing is correct for DHCPv4?**

A. Server UDP `68`, client UDP `67`  
B. Server UDP `67`, client UDP `68`  
C. Server TCP `443`, client TCP `80`  
D. Server TCP `22`, client TCP `23`

**6. Which is the most accurate description of SFTP?**

A. An unencrypted version of FTP  
B. A synonym for FTPS  
C. A file-transfer protocol carried over SSH  
D. A file-transfer protocol that always uses UDP

**7. A service listens only on `127.0.0.1:5432`. What is the strongest supported conclusion?**

A. It is bound to the local loopback interface.  
B. It is publicly accessible from the Internet.  
C. It is confirmed to be PostgreSQL.  
D. It is malicious.

**8. Which configuration best protects SNMP management traffic?**

A. SNMPv1 with a public community string  
B. SNMPv2c over an untrusted network  
C. Telnet tunneling without authentication  
D. SNMPv3 with authentication and privacy

**9. An analyst sees an unexpected outbound connection to TCP port `443`. What should they do next?**

A. Assume it is safe because `443` is HTTPS.  
B. Investigate the process, destination, and supporting telemetry.  
C. Assume all encrypted traffic is malicious.  
D. Disable all TCP traffic immediately.

**10. Which statement about HTTP/3 is accurate?**

A. It cannot provide reliability because it uses UDP.  
B. It uses Telnet as its transport.  
C. It commonly runs over QUIC on UDP `443`.  
D. It is always unencrypted.

### Knowledge check answers and explanations 

<details markdown="1">
<summary>Reveal answers and explanations</summary>

| Q | Answer | Explanation | Review |
|---|---|---|---|
| 1 | **B** | `53001` is the temporary source port; `443` is the destination service port. | [Address and port](#address-and-port) |
| 2 | **C** | TCP provides transport-layer reliability and ordered delivery. | [TCP versus UDP](#tcp-versus-udp) |
| 3 | **A** | SSH is the common encrypted replacement for Telnet remote administration. | [Secure alternatives](#secure-alternatives) |
| 4 | **D** | Traditional DNS can use UDP or TCP on port 53. | [Common ports](#common-ports) |
| 5 | **B** | DHCPv4 servers typically use UDP 67; clients use UDP 68. | [Common ports](#common-ports) |
| 6 | **C** | SFTP is the SSH File Transfer Protocol, not FTP with TLS. | [Service families](#service-families) |
| 7 | **A** | Loopback binding means the socket is local-only on that interface; a port cannot identify a process with certainty. | [Socket example](#socket-example) |
| 8 | **D** | SNMPv3 supports authenticated and encrypted management when configured with privacy. | [Secure alternatives](#secure-alternatives) |
| 9 | **B** | A port is not proof of application identity or trustworthiness; correlate additional evidence. | [Security scenarios](#security-scenarios) |
| 10 | **C** | HTTP/3 uses QUIC, which commonly runs over UDP port 443 and implements security and reliability. | [UDP behavior](#udp-behavior) |

</details>

**Score:** ____ / 10

- **9–10:** Strong protocol-selection skills. Explain three secure alternatives without checking the table.
- **7–8:** Good progress. Review the service families you mixed up.
- **0–6:** Revisit [TCP versus UDP](#tcp-versus-udp) and [common ports](#common-ports), then retry the questions.

## 11. Exam-day quick reference 

| Clue in the question | Likely concept |
|---|---|
| Reliable ordered byte stream | TCP |
| Connectionless datagrams | UDP |
| Secure shell / secure file transfer | SSH / SFTP, commonly `22/TCP` |
| Unencrypted remote terminal | Telnet, `23/TCP` |
| Name resolution | DNS, `53/UDP` and `53/TCP` |
| Automatic IPv4 assignment | DHCPv4, UDP `67/68` |
| Plain web / secure web | HTTP `80/TCP` / HTTPS `443/TCP` (HTTP/3 `443/UDP`) |
| Time synchronization | NTP, `123/UDP` |
| Directory service | LDAP `389`; LDAPS `636` |
| Windows file sharing | SMB, `445/TCP` |
| Remote desktop | RDP, `3389/TCP` and sometimes UDP |
| Network device monitoring | SNMP `161/162/UDP`; prefer SNMPv3 auth/privacy |
| Email submission | SMTP `587` STARTTLS or `465` implicit TLS |
| Secure mailbox access | IMAPS `993`, POP3S `995` |
| High client source port | Often a normal ephemeral port |
| Listener on loopback | Not the same as Internet-facing exposure |

**Three things to remember:**

1. **IP address = device/interface; port = service endpoint; TCP/UDP = transport rules.**
2. **Choose secure protocols and secure configurations, not merely familiar port numbers.**
3. **A port suggests a service, but only further evidence can confirm what is running and whether it is dangerous.**

### Coming next

**Day 19 — DNS, DHCP and Network Services:** Follow a DNS lookup and DHCP address assignment from beginning to end, then use these concepts to troubleshoot common network failures.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
