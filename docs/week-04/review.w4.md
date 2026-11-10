# Week 04 — End-of-Week Knowledge Check: Networking Fundamentals

**Security+ Study Academy | Week 04 · Days 16–20**  
**Assessment:** Post-instruction knowledge check (25 questions)  
**Suggested time:** 35–45 minutes

Complete this assessment **after** studying all five Week 04 lessons. Choose the single best answer for each question. Do not consult the answer key until you finish. The first 20 questions parallel the diagnostic by learning objective, but use new scenarios. Questions 21–25 assess earlier learning.

**Scoring:** Weekly learning = questions 1–20 (**/20**); cumulative retention = questions 21–25 (**/5**); combined = **/25**. Only the weekly /20 score should be compared to your diagnostic /20 score.

## Questions 1–4: Day 16 — OSI and TCP/IP

**1. A bridge learns source MAC addresses and forwards frames on a LAN. Which OSI layer describes its forwarding function?**  
A. Transport  
B. Network  
C. Data Link  
D. Physical

**2. A sender adds a TCP header to application data and then adds an IP header. What is the sender performing?**  
A. Encapsulation  
B. Decapsulation  
C. DNS recursion  
D. ARP resolution

**3. A router sends a packet onto a different Ethernet segment. What typically happens to its link-layer information?**  
A. Its source TCP port must change  
B. Its payload is discarded  
C. Its destination IP always changes  
D. A new Ethernet frame is built for the outgoing link

**4. A workstation shows no link light after a cable was damaged. Which layer is the best starting point?**  
A. Presentation  
B. Physical  
C. Session  
D. Application

## Questions 5–8: Day 17 — IP Addressing and Subnetting

**5. Which address is in a private IPv4 range?**  
A. 172.31.200.4  
B. 172.33.200.4  
C. 198.51.100.4  
D. 100.64.200.4

**6. A host is assigned 192.168.50.150/26. What is its network address?**  
A. 192.168.50.64  
B. 192.168.50.128  
C. 192.168.50.150  
D. 192.168.50.192

**7. Which IPv6 address belongs to the link-local unicast range?**  
A. ff02::5  
B. ::1  
C. 2001:db8::5  
D. fe80::5

**8. A host at 10.4.6.15/24 needs to contact 10.4.7.20. Which component normally forwards the traffic off the local subnet?**  
A. Its loopback interface  
B. A DNS MX record  
C. Its default gateway  
D. Its source TCP port

## Questions 9–12: Day 18 — Ports and Protocols

**9. A technician must securely administer a remote Unix host from a terminal. Which service is appropriate?**  
A. Telnet on TCP 23  
B. SSH on TCP 22  
C. TFTP on UDP 69  
D. HTTP on TCP 80

**10. A client sends a conventional DNS lookup over UDP. What destination port is typically used?**  
A. 123  
B. 161  
C. 445  
D. 53

**11. An organization is enabling QUIC-based HTTP/3 through a firewall. Which destination is most relevant?**  
A. UDP 443  
B. TCP 21  
C. UDP 67  
D. TCP 25

**12. A socket listing shows an active TCP connection in ESTABLISHED state. Which conclusion is supported?**  
A. The data is necessarily encrypted  
B. The peer is verified as benign  
C. The TCP connection is established  
D. The process cannot be compromised

## Questions 13–16: Day 19 — DNS, DHCP and Network Services

**13. An internal application loads when accessed by 10.20.30.40 but fails using its hostname. Which component should be investigated first?**  
A. SNMP  
B. DNS  
C. NTP  
D. DHCP lease renewal

**14. A DNS administrator needs a record that returns an IPv6 address for app.example. Which record should be created?**  
A. PTR  
B. MX  
C. A  
D. AAAA

**15. During DHCPv4 address assignment, a server receives a Discover message. What is its next normal response?**  
A. Offer  
B. Request  
C. Release  
D. Acknowledge

**16. A DNS response has status NXDOMAIN. What does that response report?**  
A. The DHCP lease has expired  
B. The requested record was encrypted  
C. The queried domain name does not exist  
D. The host is using an IPv6 address

## Questions 17–20: Day 20 — Routing, Switching and Packet Flow

**17. A switch knows the destination MAC address and its associated interface. How will it usually handle a unicast frame?**  
A. Send it only through the learned interface  
B. Flood it into all VLANs  
C. Replace the destination IP address  
D. Always send it to a router

**18. A computer must send an IPv4 packet to its local default gateway but does not know the gateway MAC. Which mechanism resolves it?**  
A. BGP  
B. DNS  
C. ARP  
D. NTP

**19. Routes 192.168.0.0/16, 192.168.10.0/24, and 192.168.10.128/25 are installed. Which matches 192.168.10.200 most specifically?**  
A. 192.168.0.0/16  
B. 192.168.10.128/25  
C. 192.168.10.0/24  
D. 0.0.0.0/0

**20. Two endpoints reside in separate VLANs and need to exchange IP packets. What network function must be available?**  
A. A larger MTU  
B. A CNAME record  
C. Disabled switching  
D. Inter-VLAN routing

## Questions 21–25: Cumulative retention — Week 01 foundations

These five questions revisit verified foundational concepts from Day 01 and connect them to network security decisions.

**21. A database export is accidentally shared with unauthorized recipients. Which security objective is most directly violated?**  
A. Availability  
B. Integrity  
C. Confidentiality  
D. Redundancy

**22. An attacker silently changes a supplier bank account number in a payment record. Which objective is most directly harmed?**  
A. Integrity  
B. Availability  
C. Confidentiality  
D. Capacity

**23. A service outage prevents authorized clinicians from opening patient records. Which CIA objective is most directly affected?**  
A. Confidentiality  
B. Authenticity  
C. Nonrepudiation  
D. Availability

**24. A company compares a downloaded tool with a SHA-256 value obtained from a trusted source. What is this primarily checking?**  
A. Network reachability  
B. File integrity  
C. User authorization  
D. Server availability

**25. A ransomware incident both steals files and prevents staff from opening their encrypted copies. Which two CIA objectives are directly affected?**  
A. Confidentiality and availability  
B. Integrity and latency  
C. Availability and authentication  
D. Confidentiality and routing

## Record your answers and scores

| Topic | Question numbers | Your answers | Score |
|---|---|---|---|
| Day 16 — OSI and TCP/IP | 1–4 | 1: ___, 2: ___, 3: ___, 4: ___ | ___ / 4 |
| Day 17 — IP Addressing and Subnetting | 5–8 | 5: ___, 6: ___, 7: ___, 8: ___ | ___ / 4 |
| Day 18 — Ports and Protocols | 9–12 | 9: ___, 10: ___, 11: ___, 12: ___ | ___ / 4 |
| Day 19 — DNS, DHCP and Network Services | 13–16 | 13: ___, 14: ___, 15: ___, 16: ___ | ___ / 4 |
| Day 20 — Routing, Switching and Packet Flow | 17–20 | 17: ___, 18: ___, 19: ___, 20: ___ | ___ / 4 |
| Cumulative retention (Week 01) | 21–25 | 21: ___, 22: ___, 23: ___, 24: ___, 25: ___ | ___ / 5 |

**Weekly learning score (1–20):** ___ / 20 = ___%  
**Cumulative retention score (21–25):** ___ / 5 = ___%  
**Combined score:** ___ / 25 = ___%

## Compare your diagnostic and post-check

| Measure | Record |
|---|---|
| Week 04 diagnostic (before lessons) | ___ / 20 |
| Week 04 post-check (questions 1–20) | ___ / 20 |
| Difference in correct answers | Post-check minus diagnostic = ___ |
| Diagnostic percentage | Diagnostic score × 5 = ___% |
| Post-check percentage | Post-check score × 5 = ___% |
| Percentage-point change | (Post-check − diagnostic) × 5 = ___ percentage points |
| Cumulative retention (separate) | ___ / 5 |

A higher post-check score suggests improvement on this sample of objectives. Both assessments are short screening measures, **not** precise measures of mastery or proof of certification readiness.

## Topic-level improvement scorecard

| Lesson / topic | Diagnostic /4 | Post-check /4 | Percentage-point change | Review action |
|---|---:|---:|---:|---|
| Day 16 — OSI and TCP/IP | ___ | ___ | (post − pre) × 25 = ___ pp | ___ |
| Day 17 — IP Addressing and Subnetting | ___ | ___ | (post − pre) × 25 = ___ pp | ___ |
| Day 18 — Ports and Protocols | ___ | ___ | (post − pre) × 25 = ___ pp | ___ |
| Day 19 — DNS, DHCP and Network Services | ___ | ___ | (post − pre) × 25 = ___ pp | ___ |
| Day 20 — Routing, Switching and Packet Flow | ___ | ___ | (post − pre) × 25 = ___ pp | ___ |

**Post-check review actions:** 4/4: strong performance—advance and schedule spaced review. 3/4: review the missed concept. 2/4: revisit the lesson and repeat its practical application. 0–1/4: repeat the lesson carefully and reassess with new questions.

Identify your **two lowest-scoring topics**: 1. __________ 2. __________. Record the concepts you missed and which practical exercises you will redo. A negative change may reflect question sampling or a concept that needs clarification; inspect individual mistakes before drawing conclusions.

## Answer key and remediation

**Stop here until you have answered all 25 questions.** Each entry includes the correct choice, rationale, assessed objective, and a direct link to the teaching section.

### Day 16 — OSI and TCP/IP

**1. C — Layer 2 uses Ethernet MAC addresses for frame forwarding.**  
*Objective:* Identify the layer used for MAC-based forwarding. *Review:* [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md#data-link-layer).

**2. A — Headers are added as data moves down the protocol stack.**  
*Objective:* Recognize encapsulation. *Review:* [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md#encapsulation).

**3. D — A router removes the incoming frame and encapsulates the forwarded IP packet in a frame appropriate to the outgoing link.**  
*Objective:* Trace what changes at a router. *Review:* [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md#hop-by-hop).

**4. B — A damaged cable or link signal is a Layer 1 problem.**  
*Objective:* Select the initial troubleshooting layer. *Review:* [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md#physical-layer).

### Day 17 — IP Addressing and Subnetting

**5. A — 172.16.0.0/12 includes 172.31.200.4; 100.64.0.0/10 is shared address space, not RFC 1918.**  
*Objective:* Identify RFC 1918 private addresses. *Review:* [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md#public-private-ipv4).

**6. B — A /26 has blocks of 64 addresses: 128–191 contains 150.**  
*Objective:* Calculate a CIDR network boundary. *Review:* [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md#subnet-calculation).

**7. D — fe80::/10 is the IPv6 link-local unicast range.**  
*Objective:* Recognize IPv6 address categories. *Review:* [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md#ipv6-address-types).

**8. C — A destination outside the local /24 is normally sent to a router/default gateway.**  
*Objective:* Choose routing for off-subnet traffic. *Review:* [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md#same-subnet-decisions).

### Day 18 — Ports and Protocols

**9. B — SSH provides encrypted remote shell access, normally on TCP 22.**  
*Objective:* Select secure remote administration. *Review:* [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md#secure-alternatives).

**10. D — Conventional DNS queries commonly use UDP destination port 53; DNS can also use TCP 53.**  
*Objective:* Associate DNS with its conventional port. *Review:* [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md#common-ports).

**11. A — HTTP/3 runs over QUIC, which uses UDP; HTTPS service commonly uses UDP 443.**  
*Objective:* Select transport and port for HTTP/3. *Review:* [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md#service-families).

**12. C — ESTABLISHED indicates TCP connection state, not encryption or trust.**  
*Objective:* Interpret a TCP connection state. *Review:* [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md#connection-states).

### Day 19 — DNS, DHCP and Network Services

**13. B — Success by IP but failure by hostname points first to name resolution.**  
*Objective:* Troubleshoot hostname resolution. *Review:* [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md#7-troubleshooting-ask-the-right-question-first).

**14. D — AAAA records associate hostnames with IPv6 addresses.**  
*Objective:* Identify an IPv6 DNS record. *Review:* [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md#3-dns-records-you-need-to-recognize).

**15. A — The DORA sequence is Discover, Offer, Request, Acknowledge.**  
*Objective:* Order DHCP DORA messages. *Review:* [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md#dora-discover-offer-request-acknowledge).

**16. C — NXDOMAIN means the queried domain name does not exist according to the DNS response; it is not a transport or DHCP status.**  
*Objective:* Interpret DNS response codes. *Review:* [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md#step-d-interpret-response-codes).

### Day 20 — Routing, Switching and Packet Flow

**17. A — A switch forwards a known unicast frame out the learned port in its VLAN.**  
*Objective:* Explain known-unicast switching. *Review:* [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md#the-mac-address-table).

**18. C — ARP maps a local IPv4 next-hop address to a MAC address.**  
*Objective:* Choose ARP for next-hop MAC discovery. *Review:* [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md#4-arp-finding-the-next-hop-mac-address).

**19. B — 192.168.10.200 belongs to all three networks, but /25 is the longest matching prefix.**  
*Objective:* Apply longest-prefix routing. *Review:* [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md#longest-prefix-match).

**20. D — VLANs are separate Layer 2 broadcast domains; communication between them normally needs Layer 3 routing.**  
*Objective:* Recognize the need for inter-VLAN routing. *Review:* [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md#3-vlans-one-switch-multiple-logical-networks).

### Cumulative retention — Day 01

**21. C — Unauthorized disclosure violates confidentiality.**  
*Objective:* Recognize confidentiality. *Review:* [Day 01 — CIA Triad](../week-01/day-01-cia-triad.md#2-confidentiality-prevent-unauthorized-disclosure).

**22. A — Unauthorized modification compromises data integrity.**  
*Objective:* Recognize integrity. *Review:* [Day 01 — CIA Triad](../week-01/day-01-cia-triad.md#3-integrity-protect-trustworthiness).

**23. D — Inability to access needed information is an availability failure.**  
*Objective:* Recognize availability. *Review:* [Day 01 — CIA Triad](../week-01/day-01-cia-triad.md#4-availability-keep-systems-usable).

**24. B — A trusted expected hash helps detect whether file contents changed.**  
*Objective:* Connect hashing to integrity. *Review:* [Day 01 — CIA Triad](../week-01/day-01-cia-triad.md#3-integrity-protect-trustworthiness).

**25. A — Theft affects confidentiality; inaccessible encrypted files affect availability.**  
*Objective:* Integrate multiple CIA impacts. *Review:* [Day 01 — CIA Triad](../week-01/day-01-cia-triad.md#5-the-objectives-interactand-sometimes-compete).

## What to do next

1. Compare **only** the two Week 04 scores out of 20 to estimate weekly improvement.
2. Use the topic scorecard to select targeted rereading and practical labs.
3. Review any cumulative errors using the linked Week 01 material; track retention separately.
4. Revisit the same objectives later with **new** questions rather than memorizing this answer key.
5. Continue to the next week when you can explain your decisions and reproduce the relevant practical tasks, not simply when you achieve a high multiple-choice score.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
