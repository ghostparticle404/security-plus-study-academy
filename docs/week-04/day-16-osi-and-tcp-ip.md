# Day 16 — OSI and TCP/IP: How Data Travels Across a Network

> **CompTIA Security+ SY0-701** · **Week 04, Lesson 1 of 5** · Networking foundations supporting Domain 3 (3.2: secure enterprise infrastructure) and Domain 4 (4.5: enterprise security capabilities). The OSI model is foundational knowledge rather than a standalone SY0-701 objective.  
> **Level:** Beginner to intermediate · **Study time:** 75–100 minutes  
> **Prerequisites:** Days 01–03 · **Previous:** Day 15 — Week 03 final lesson  
> **Next:** Day 17 — IP Addressing and Subnetting

## 1. Learning objectives

By the end of this lesson, you should be able to:

1. Name the seven OSI layers and explain their responsibilities.
2. Match the four TCP/IP layers to the OSI model.
3. Distinguish data, segments, datagrams, packets, and frames.
4. Trace encapsulation and decapsulation from sender to receiver.
5. Identify where common protocols and security controls operate.
6. Use layer-based reasoning to troubleshoot a network problem.

## 2. The big idea: Why do network models exist?

Imagine ordering a book online. You choose the title, a warehouse packs it, a courier sorts it, and a driver delivers it. Each participant needs different information, but no single participant must perform every job.

Networking works similarly. Your browser does not need to know how electrical signals travel through a cable; a switch does not need to understand the web page you requested. **Layering separates responsibilities**, making protocols interoperable and troubleshooting more systematic.

**Important distinction:** The OSI model is a **conceptual seven-layer reference model**. TCP/IP is the **practical protocol suite and layered model** used by the Internet. Real protocols do not always fit neatly into a single layer.

🧠 **Memory hook:** Think of layers as separate jobs, not separate physical machines.

**CIA connection:** Layered controls protect confidentiality (TLS), integrity (authenticated encryption), and availability (redundant links and resilient services). A failure at one layer can affect the entire stack.

## 3. The seven OSI layers 

| Layer | Name | Main question | Examples and clues |
|---|---|---|---|
| **7** | Application | What network service does the software need? | HTTP, DNS, SMTP |
| **6** | Presentation | How is data represented or transformed? | Encoding, compression, encryption functions |
| **5** | Session | How is a communication session managed? | Session establishment, coordination, checkpoints |
| **4** | Transport | Which process receives the data, and how is delivery handled? | TCP, UDP, port numbers |
| **3** | Network | Which IP network and host should receive the packet? | IPv4, IPv6, routers |
| **2** | Data Link | How does a frame move across the local link? | Ethernet, Wi-Fi MAC addressing, switches |
| **1** | Physical | How are bits transmitted over the medium? | Copper, fiber, radio signals |

**Mnemonic (top to bottom):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

### Layer 7 — Application {#application-layer}

The application layer provides network services to software. A browser uses HTTP or HTTPS to request web content; a mail client might use SMTP to send mail; DNS resolves names into addresses.

**Security connection:** An attacker might exploit a vulnerable web application or send a malicious HTTP request. A web application firewall (WAF) examines application-layer traffic, subject to what it can decrypt and inspect.

### Layer 6 — Presentation 

This conceptual layer describes how information is encoded, formatted, compressed, or encrypted so the recipient can interpret it. Examples include character encoding and data serialization.

**Exam nuance:** TLS is often associated with presentation-layer security in OSI teaching, but actual TLS implementations sit between application protocols and transport, rather than mapping perfectly to one OSI layer.

### Layer 5 — Session 

The session layer describes establishing, coordinating, maintaining, and ending communication sessions.

**Don't confuse:** An application login session, a TLS session, and a TCP connection are related ideas but are **not the same thing**.

### Layer 4 — Transport {#transport-layer}

The transport layer delivers data between processes using **port numbers**.

- **TCP:** Connection-oriented, ordered byte stream with acknowledgments and retransmission. Useful for web browsing, email, and file transfer.
- **UDP:** Connectionless datagrams without built-in delivery or ordering guarantees. Useful for DNS queries, real-time traffic, and protocols such as QUIC that implement reliability above UDP when needed.

**Security connection:** A firewall can filter traffic based on transport protocol and source/destination ports. A TCP SYN flood targets connection-handling resources.

### Layer 3 — Network {#network-layer}

The network layer uses **IP addresses** and routing to move packets between networks. Routers select a next hop based on routing information; the path can involve many routers.

**Security connection:** IP-based access control lists, network segmentation, and routing decisions are important here. IP spoofing involves forging a packet's source IP address.

### Layer 2 — Data Link {#data-link-layer}

The data link layer packages packets into **frames** for a particular local link. Ethernet and Wi-Fi use link-layer addressing; switches normally forward Ethernet frames based on MAC addresses.

**Security connection:** ARP spoofing can misdirect IPv4 traffic on a local network. VLANs logically separate Layer 2 broadcast domains.

### Layer 1 — Physical 

The physical layer transmits bits as electrical, optical, or radio signals.

**Security connection:** A disconnected cable, damaged fiber, radio interference, or a physically attached unauthorized device can disrupt or expose communication.

### How adjacent layers cooperate

A protocol defines rules for communication between peers. A layer uses services from the layer below and provides services to the layer above. For example, HTTP can use TCP to carry bytes; TCP uses IP to reach the destination; IP uses a local link to reach the next hop. TCP reliability does not guarantee that an HTTP request succeeds: the server can still return an error.

A MAC address identifies a link-layer interface for local delivery, an IP address provides logical addressing across networks, and a port identifies a transport endpoint. A socket endpoint combines an IP address and port with a transport protocol; a TCP conversation is distinguished by both endpoints. Ports are not physical connectors.

## 4. OSI versus TCP/IP {#osi-vs-tcp-ip}

The commonly taught **four-layer TCP/IP model** groups several OSI functions together.

| OSI model | TCP/IP model | Typical protocols or technologies |
|---|---|---|
| 7 Application | **Application** | HTTP, DNS, SMTP |
| 6 Presentation | **Application** | Encoding and TLS-related functions |
| 5 Session | **Application** | Application/session management |
| 4 Transport | **Transport** | TCP, UDP |
| 3 Network | **Internet** | IPv4, IPv6, ICMP |
| 2 Data Link | **Network Access / Link** | Ethernet, Wi-Fi |
| 1 Physical | **Network Access / Link** | Copper, fiber, radio |

Some references use a **five-layer Internet model** that separates the physical and data link layers. For Security+, be able to recognize both conventions without assuming one-to-one mapping.

**Quick analogy:** OSI is a detailed seven-department organizational chart; TCP/IP is the practical four-team workflow. Both describe the same communication from different levels of detail.

## 5. Encapsulation: wrapping the message {#encapsulation}

When a sender transmits data, each lower layer adds information required for its job. This is called **encapsulation**.

```text
Sender: your browser requests a web page

Application        HTTP request (application data)
       ↓
Transport          [TCP header | application data]          = segment
       ↓
Network            [IP header | TCP segment]                = packet
       ↓
Data Link          [Ethernet header | IP packet | trailer]  = frame
       ↓
Physical           Bits transmitted over cable / radio
```

The headers are **not interchangeable**:

- **TCP header:** Source/destination ports, sequence numbers, and other transport fields.
- **IP header:** Source/destination IP addresses and information used for forwarding.
- **Ethernet header:** Source/destination MAC addresses for the **current local link**.
- **Ethernet trailer:** Usually includes a frame check sequence (FCS) for error detection.

**Protocol data units (PDUs):**

| Layer | Common PDU term |
|---|---|
| Application / Presentation / Session | Data |
| Transport | TCP **segment** or UDP **datagram** |
| Network | IP **packet** |
| Data Link | **Frame** |
| Physical | **Bits** |

> **Exam trap:** Not every transport-layer unit is a TCP segment. UDP carries **datagrams**. Also, an IP packet can carry protocols other than TCP or UDP, including ICMP.

### Reading the nested headers

A header carries protocol metadata; a payload is the content carried by that protocol. What counts as payload depends on the layer: a TCP segment is the payload of an IP packet. Ethernet FCS detects accidental transmission errors, not deliberate tampering by an attacker who can recompute it. TLS provides cryptographic integrity when properly configured. Encapsulation alone is not encryption.

TCP can split an application byte stream into multiple segments; a single HTTP request need not correspond to one packet. UDP preserves datagram boundaries, while IP fragmentation is a separate network-layer mechanism.

## 6. Decapsulation: opening the message 

At the destination, the process is reversed:

1. The network interface receives physical signals and reconstructs a frame.
2. The data link layer checks and processes the frame, then passes its payload upward.
3. The network layer examines the IP packet and passes its payload to the appropriate upper-layer protocol.
4. The transport layer identifies the destination process by port (when applicable) and handles TCP/UDP processing.
5. The application interprets the resulting data.

**Think of nested envelopes:** The outer envelope gets the package across the local link; the next envelope identifies the IP destination; the inner transport information directs it to the correct service.

## 7. Follow a packet through a real network {#packet-path}

Suppose your laptop at `192.168.1.25` opens an HTTPS connection to a website at `203.0.113.10`. These are **illustrative addresses**, not a live network trace. Assume the laptop uses a gateway at `192.168.1.1`, and the site uses TCP port `443` for this example.

**Step A — Application:** Your browser prepares a request. In a typical HTTPS connection, application data is protected with TLS before TCP carries it. DNS resolution may have occurred earlier.

**Step B — Transport:** TCP uses a temporary source port (for example, `51514`) and destination port `443`. A connection is established before HTTP data is sent in this TCP example.

**Step C — Network:** The IP packet has source `192.168.1.25` and destination `203.0.113.10` before any possible NAT translation.

**Step D — Local link:** Because the destination is outside the local subnet, the laptop sends its Ethernet or Wi-Fi frame toward the **default gateway's link-layer address**, not directly to the remote website's MAC address.

**Step E — Router:** The router removes the incoming link-layer framing, checks the IP forwarding information, and creates **new link-layer framing** for the next hop. On a typical home network, NAT may also replace the private source IP and transport port.

**Step F — Destination:** The server receives and processes the packet, and its transport/application software handles the request. The response travels back using the applicable routing and connection state.

### How the next hop is found

The host uses its subnet configuration and routing table to decide whether the destination is on-link or requires a gateway. On Ethernet IPv4 networks, Address Resolution Protocol (ARP) resolves the next-hop IPv4 address to a MAC address; IPv6 uses Neighbor Discovery (NDP). A switch forwards frames within the local network; a router forwards packets between networks. DNS resolves names to IP addresses, not MAC addresses.

### What changes at each hop? {#hop-by-hop}

| Field | Across a routed hop |
|---|---|
| Source/destination MAC addresses | **Usually change** as new link-layer frames are created |
| Source/destination IP addresses | **Usually remain the same** end to end, except with NAT or other translation |
| IPv4 TTL / IPv6 Hop Limit | **Decreases** at each router |
| TCP/UDP ports | **Usually remain the same**, except when NAT or a middlebox changes them |
| Application data | Normally forwarded without alteration by ordinary IP routers; proxies can terminate and create separate connections |

**Security takeaway:** If you capture traffic on two sides of a router, you may see different MAC addresses for the same logical IP conversation. That is expected, not necessarily suspicious.

## 8. Where security tools fit 

| Tool or attack | Useful layer perspective | Why it matters |
|---|---|---|
| Network cable test | Layer 1 | Confirms physical connectivity |
| Switch / VLAN | Layer 2 | Controls local forwarding and segmentation |
| ARP spoofing | Layer 2 | Can redirect local IPv4 traffic |
| Router / IP ACL | Layer 3 | Controls paths and IP-based access |
| TCP/UDP port firewall rule | Layers 3–4 | Filters network and transport traffic |
| TCP SYN flood | Layer 4 | Abuses TCP connection establishment |
| HTTP request inspection / WAF | Layer 7 | Examines web application requests |
| TLS | Cross-layer in practice | Protects application traffic in transit when correctly configured |

**Important:** A device may operate across multiple layers. A next-generation firewall can inspect IPs, ports, protocols, and some application content. Do not assume every security product fits in exactly one box.

## 9. Worked case study: A law firm's inaccessible client portal

A law firm's workstation can reach its gateway and resolve the portal hostname, but its browser cannot establish a TCP connection to the portal on port 443. The firewall log records a denied outbound TCP connection to that destination. These are synthetic observations.

| Evidence | Layer perspective | Interpretation |
|---|---|---|
| Wi-Fi connected | 1–2 | Local connectivity is plausible, not proof of end-to-end access |
| Gateway responds | 3 | Local IP reachability works for this test |
| DNS returns an address | 7 | Name resolution succeeded; the portal may still be unavailable |
| TCP connection denied | 3–4 | Firewall policy is a likely cause |
| No HTTP response | 7 | Application symptom, not proof of an application fault |

The administrator checks the intended egress policy, confirms the destination and business requirement, and uses the approved change process if the rule is incorrect. Do not disable the firewall simply to restore access. Retest TCP, TLS certificate validation and HTTP after the authorized change.

**Lesson:** The symptom appears in the browser, but the evidence points to a lower-layer control. A successful test narrows the investigation; it does not prove every service at that layer is working.

## 10. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| Name the seven OSI layers from top to bottom. | _Write here._ |
| Map OSI to the four TCP/IP layers. | _Write here._ |
| Compare MAC addresses, IP addresses and ports. | _Write here._ |
| List the PDUs from application to physical. | _Write here._ |
| Explain encapsulation and decapsulation. | _Write here._ |
| What changes when a router forwards a packet? | _Write here._ |
| Compare TCP and UDP. | _Write here._ |
| How do ARP and DNS differ? | _Write here._ |
| Why can a browser symptom originate below Layer 7? | _Write here._ |
| Does encapsulation or Ethernet FCS provide cryptographic confidentiality? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| Name the seven OSI layers from top to bottom. | Application, Presentation, Session, Transport, Network, Data Link, Physical. |
| Map OSI to the four TCP/IP layers. | 5–7 Application; 4 Transport; 3 Internet; 1–2 Link. |
| Compare MAC addresses, IP addresses and ports. | Local link delivery, logical network delivery, and transport endpoints respectively. |
| List the PDUs from application to physical. | Data, TCP segment or UDP datagram, IP packet, frame, bits. |
| Explain encapsulation and decapsulation. | Add protocol metadata down the stack; process and remove it as appropriate up the stack. |
| What changes when a router forwards a packet? | New link framing; TTL/Hop Limit decreases; NAT may alter IP addresses and ports. |
| Compare TCP and UDP. | TCP provides an ordered reliable byte stream; UDP provides datagrams without built-in reliability. |
| How do ARP and DNS differ? | ARP resolves local IPv4 next-hop addresses to MAC addresses; DNS resolves names to records such as IP addresses. |
| Why can a browser symptom originate below Layer 7? | Application services depend on transport, routing and local connectivity. |
| Does encapsulation or Ethernet FCS provide cryptographic confidentiality? | No. Encapsulation organizes data; FCS detects errors. Encryption is a separate protection. |

</details>

## 11. Hands-on lab: Troubleshoot, annotate and inspect network traffic

**Time:** 25–35 minutes · **Tools:** Terminal, notebook, optional Wireshark · **Scope:** Your own device and authorized traffic only; no configuration changes.

### Part A — Troubleshooting from the bottom up 

**Scenario:** A laptop cannot load a website.

1. **Layer 1:** Is the cable connected? Is Wi-Fi radio connectivity present?
2. **Layer 2:** Is the device connected to the correct network/VLAN? Is local link communication working?
3. **Layer 3:** Does it have a valid IP address, subnet configuration, and default gateway? Can it reach the gateway?
4. **Layer 4:** Is TCP connectivity to the required service port possible?
5. **Application:** Does DNS resolve the name? Does the web server respond? Are TLS certificates and HTTP responses valid?

This is a **diagnostic framework**, not a rule that every incident must be investigated in exactly this order. For example, if other websites work but one hostname fails, checking DNS early is sensible.

#### Useful beginner commands 

On macOS or Linux, try these **read-only** commands:

```bash
ping -c 4 1.1.1.1
```

Tests whether ICMP echo replies are received. **No reply does not prove the host is down**; ICMP may be filtered.

```bash
nslookup example.com
```

Checks DNS resolution. DNS is an application-layer service, even though it uses lower-layer protocols to communicate.

```bash
netstat -an
```

Shows local network endpoints and connection states (output and options vary by operating system).

**Safety:** Run these only on your own device or an authorized lab. No scanning of other systems is required for this lesson.

#### Record and interpret your results

Check the local network connection in system settings, then run the commands above. Use `curl -I --connect-timeout 10 https://example.com` to request HTTP headers with a ten-second connection timeout. Record whether DNS resolves, ICMP replies arrive, and an HTTP response is returned. Do not bypass TLS verification. If offline, use the synthetic case in Section 9 instead.

<details markdown="1">
<summary>Reveal Part A lab answers</summary>

- `ping -c 4` sends four echo requests; replies indicate ICMP reachability, not TCP service availability.
- `nslookup` should return DNS records if resolution works; a failure may indicate a resolver or connectivity problem.
- `netstat -an` displays numeric endpoints and connection states; TCP may show LISTEN or ESTABLISHED.
- An HTTP status from curl indicates an HTTP exchange occurred; certificate errors or timeouts require separate investigation. Results vary by network.
- In the worked case, inspect the denied firewall rule rather than assuming the web server is broken.

</details>

### Part B — Practical exercise: Annotate a packet path 

**Goal:** Identify what each layer contributes as a browser sends a request to a remote web server.

**Scenario:** A workstation `192.168.10.50` sends an HTTPS request over TCP to `198.51.100.20:443` through gateway `192.168.10.1`. Assume the client source port is `53000`. The public destination is a documentation address; the private addresses are illustrative. Assume a /24 client subnet.

Copy and complete this worksheet in your notes:

| Question | Your answer |
|---|---|
| Which OSI layer handles the HTTP request? | __________ |
| Which transport protocol is used in this scenario? | __________ |
| What is the destination transport port? | __________ |
| What is the source IP before NAT? | __________ |
| What is the destination IP? | __________ |
| Whose MAC address does the client use as the next-hop destination on its local link? | __________ |
| What PDU is transmitted at Layer 2? | __________ |
| What happens to link-layer framing when a router forwards the packet? | __________ |
| Which IP header field is reduced at each IPv4 router? | __________ |

**Packet capture preparation:** On a device and network you are authorized to capture, inspect one of your own TCP conversations. Expand **Frame**, **Ethernet II** (or another link-layer header), **Internet Protocol**, and **Transmission Control Protocol**. Locate the MAC addresses, IP addresses, and ports. The precise capture layers depend on the interface and capture method; loopback captures may not show Ethernet headers.

<details markdown="1">
<summary>Exercise answer key</summary>

1. Layer 7 — Application.
2. TCP.
3. `443`.
4. `192.168.10.50`.
5. `198.51.100.20`.
6. The local default gateway's MAC address (or the next-hop link-layer address appropriate to the link).
7. A frame.
8. The router processes the incoming frame and creates new framing for the outgoing link.
9. IPv4 **Time to Live (TTL)**.

</details>

### Part C — Observe encapsulation in Wireshark

1. Open Wireshark on an authorized interface and start a short capture of your own traffic. Avoid capturing on shared or sensitive networks.
2. Run `curl -I https://example.com` and stop the capture.
3. Apply the display filter `tcp.port == 443`. If there is no matching traffic, check the selected interface or use a provided authorized sample capture.
4. Select a packet and expand the link-layer header (if present), IP header and TCP header. Record source/destination addresses, ports and TTL or Hop Limit. Look for SYN, SYN-ACK and ACK in a new TCP connection if captured.
5. Explain why encrypted HTTPS application content may not be readable. Do not attempt to decrypt other people's traffic.

**Interpretation challenges:** A firewall blocks inbound TCP port 22: which layer supplies the port? Two captures of the same routed connection show different MAC addresses but the same destination IP: why?

<details markdown="1">
<summary>Reveal Part C lab answers</summary>

The link header describes local delivery, IP identifies logical endpoints, and TCP supplies ports and connection metadata. TLS protects application content; visible metadata does not imply plaintext is available. Port 22 is Layer 4 information. Different MAC addresses across routed links are expected because routers create new link framing. Capture results vary with interface, NAT, VPNs and connection reuse.

</details>

## 12. Eight original Security+-style questions {#knowledge-check}

Choose the **BEST** answer before revealing each explanation. These are original practice questions, not actual CompTIA items.

### Q1. Which OSI layer is primarily responsible for routing IP packets between networks?

A. Data Link  
B. Network  
C. Transport  
D. Application  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B — Network.** Layer 3 provides logical addressing and routing. See [Layer 3](#network-layer).

</details>

### Q2. Which PDU is most closely associated with Ethernet at Layer 2?

A. Segment  
B. Packet  
C. Frame  
D. Datagram  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C — Frame.** Ethernet carries link-layer frames. See [Encapsulation](#encapsulation).

</details>

### Q3. A switch forwards Ethernet traffic using MAC addresses. Which layer is most directly involved?

A. Physical  
B. Data Link  
C. Network  
D. Session  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B — Data Link.** Ethernet switches commonly use Layer 2 MAC addresses. See [Layer 2](#data-link-layer).

</details>

### Q4. Which pairing is correct?

A. TCP — Internet layer  
B. IP — Transport layer  
C. HTTP — Application layer  
D. Ethernet — Session layer  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C — HTTP / Application.** HTTP is an application-layer protocol. See [Layer 7](#application-layer).

</details>

### Q5. A laptop sends a packet to a server on a different network. Which MAC address is typically placed in the outgoing frame's destination field?

A. The remote server's MAC address  
B. The default gateway's MAC address  
C. The DNS server's MAC address  
D. The laptop's own MAC address  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B — Default gateway.** The first link targets the next hop, not the remote server's MAC. See [Packet path](#packet-path).

</details>

### Q6. Which statement best describes encapsulation?

A. Each router encrypts the entire application payload  
B. The receiver adds headers while delivering the data upward  
C. Lower layers add protocol information as data moves toward transmission  
D. Every packet always uses TCP  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C — Lower layers add information.** Encapsulation wraps data in protocol headers (and sometimes trailers). See [Encapsulation](#encapsulation).

</details>

### Q7. Which value normally decreases when an IPv4 packet passes through a router?

A. Destination TCP port  
B. Source IP address  
C. TTL  
D. HTTP method  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C — TTL.** Routers reduce the IPv4 TTL as they forward packets. See [Hop-by-hop changes](#hop-by-hop).

</details>

### Q8. Which statement about TCP/IP and OSI is most accurate?

A. Both always contain exactly seven layers  
B. TCP/IP's application layer combines functions described by OSI Layers 5–7  
C. TCP/IP has no transport layer  
D. Ethernet is an application-layer protocol  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B — Combined upper-layer functions.** The four-layer TCP/IP model groups OSI 5–7 into Application. See [OSI versus TCP/IP](#osi-vs-tcp-ip).

</details>

**Score guide:** 7–8: strong understanding; 5–6: revisit Sections 3–9; 0–4: redraw the stack and repeat the lab.

## 13. Discussion challenge

A company deploys a firewall that permits TCP port 443 and assumes all permitted traffic is safe. Explain why port-based filtering cannot establish whether an application request is malicious. Recommend two complementary controls, explain their layer perspectives and limitations, and describe how encrypted traffic affects inspection.

**Bonus:** Which CIA objectives could be affected by a malicious request or a mistaken firewall rule?

## 14. Study from the Treadmill 🏃

**Supplementary video:** [Professor Messer — The OSI Model (Network+ N10-009)](https://www.professormesser.com/network-plus/n10-009/n10-009-video/the-osi-model-n10-009/) — foundational networking material relevant to this lesson, not a standalone Security+ objective. The direct link has not been independently verified in this revision.

**Active-listening prompts:** Which layers use MAC addresses, IP addresses and ports? Why do routers replace link-layer framing? How does the TCP/IP model group the upper OSI layers?

## 15. One-minute recap

- **OSI:** Seven conceptual layers; TCP/IP commonly uses four.
- **MAC / IP / port:** Local delivery / network routing / transport endpoint.
- **Encapsulation:** Data → segment or datagram → packet → frame → bits.
- **Decapsulation:** Process protocol information in reverse at the receiver.
- **Routing:** New link framing, reduced TTL or Hop Limit, possible NAT.
- **Troubleshooting:** Use evidence to isolate the failing layer; security controls can span layers.

### Exam-day quick reference 

| If you see… | Think… |
|---|---|
| Cable, radio, signals | Layer 1 — Physical |
| MAC, Ethernet, frames, switches | Layer 2 — Data Link |
| IP, routers, TTL | Layer 3 — Network |
| TCP, UDP, ports | Layer 4 — Transport |
| HTTP, DNS, SMTP | Layer 7 — Application |
| Adding headers on the sender | Encapsulation |
| Removing headers at the receiver | Decapsulation |
| MAC changes across routers | New link-layer framing per hop |
| OSI 7 layers vs TCP/IP 4 | Conceptual reference vs practical Internet model |

**Three things to remember:**

1. **MAC addresses move frames across a local link; IP addresses guide packets between networks; ports identify services/processes.**
2. **Encapsulation adds protocol information as data moves down the stack; decapsulation processes it on the way up.**
3. **The layer where a symptom appears is a clue, not necessarily the layer where its root cause lies.**

**Next:** Day 17 — IP Addressing and Subnetting: IPv4, IPv6, CIDR notation, and determining which addresses belong to the same network.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
