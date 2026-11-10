# Week 04 — Pre-Instruction Diagnostic: Networking Fundamentals

**Security+ Study Academy | Week 04 · Days 16–20**  
**Assessment:** Pre-instruction diagnostic (20 questions)  
**Points:** 20 (1 point per question)  
**Suggested time:** 25–35 minutes

> **Why study something you already know?** Use this screening assessment to identify the networking topics you already understand and the ones that deserve your study time.

**Instructions:** Complete the questions *before* reading the Week 04 lessons. Choose the single best answer for each question. Record your choices, then consult the answer key at the end. No outside tools are needed. Each lesson contributes four questions. This is a baseline, not a certification practice-exam score.

## Questions 1–4: Day 16 — OSI and TCP/IP

**1. A switch forwards Ethernet frames using destination MAC addresses. Which OSI layer is most directly involved?**
A. Physical
B. Data Link
C. Network
D. Transport

**2. A technician captures a TCP segment being wrapped with an IP header before transmission. What is this process called?**
A. Decapsulation
B. Name resolution
C. Encapsulation
D. Port translation

**3. A packet travels through a router between two Ethernet networks. Which description is most accurate?**
A. The original Ethernet frame remains unchanged end to end.
B. The router changes the destination TCP port for every packet.
C. The router replaces the application payload with a new one.
D. The router forwards the IP packet in a new link-layer frame.

**4. A network cable is disconnected. Which OSI layer should a troubleshooter investigate first?**
A. Physical
B. Session
C. Application
D. Presentation

## Questions 5–8: Day 17 — IP Addressing and Subnetting

**5. Which address is within the RFC 1918 private IPv4 ranges?**
A. 172.32.5.10
B. 100.64.5.10
C. 172.20.5.10
D. 198.51.100.10

**6. A host uses 192.168.40.70/26. What is its subnet network address?**
A. 192.168.40.0
B. 192.168.40.64
C. 192.168.40.70
D. 192.168.40.128

**7. Which IPv6 address is a link-local unicast address?**
A. 2001:db8::7
B. ff02::1
C. ::1
D. fe80::7

**8. A workstation at 192.168.8.25/24 must reach 192.168.9.30. What does it normally need to send traffic beyond its local subnet?**
A. A default gateway
B. A larger source port
C. A DNS TXT record
D. A different loopback address

## Questions 9–12: Day 18 — Ports and Protocols

**9. An administrator needs encrypted command-line access to a Linux server. Which destination port and protocol is the usual choice?**
A. TCP 23 / Telnet
B. TCP 22 / SSH
C. UDP 69 / TFTP
D. TCP 80 / HTTP

**10. A packet capture shows a client using UDP destination port 53. Which service is the most likely target?**
A. NTP
B. SNMP
C. DNS
D. SMB

**11. A web application uses HTTP/3 over QUIC. Which transport/port combination should a firewall generally allow for that service?**
A. UDP 443
B. TCP 23
C. UDP 161
D. TCP 110

**12. A security analyst sees a TCP connection in ESTABLISHED state. What does that most directly indicate?**
A. The remote host is guaranteed trustworthy.
B. The application payload is encrypted.
C. The server has no open ports.
D. A TCP connection has been established between endpoints.

## Questions 13–16: Day 19 — DNS, DHCP and Network Services

**13. A user can reach a website by IP address but not by its hostname. Which service should be checked first?**
A. DHCP lease duration
B. DNS resolution
C. NTP synchronization
D. SNMP traps

**14. Which DNS record type maps a hostname to an IPv6 address?**
A. MX
B. PTR
C. AAAA
D. A

**15. A new DHCP client has broadcast a Discover message. What normally comes next in the DHCPv4 DORA exchange?**
A. Offer
B. Acknowledge
C. Release
D. Request

**16. A DNS lookup returns NXDOMAIN. What does that response most directly mean?**
A. The resolver provided a valid IPv4 address.
B. The client received a DHCP address.
C. The DNS answer was encrypted.
D. The queried domain name does not exist according to the response.

## Questions 17–20: Day 20 — Routing, Switching and Packet Flow

**17. A switch receives a unicast Ethernet frame for a destination MAC address already in its MAC table. What does it normally do?**
A. Flood the frame to every VLAN.
B. Forward it through the associated port.
C. Send it to the default gateway regardless of subnet.
D. Replace its destination IP address.

**18. A host needs the MAC address of its IPv4 default gateway on the local network. Which protocol is used?**
A. DNS
B. BGP
C. ARP
D. DHCP

**19. A router has routes for 10.0.0.0/8, 10.20.0.0/16, and 10.20.30.0/24. Which route is selected for 10.20.30.55?**
A. 10.20.30.0/24
B. 10.20.0.0/16
C. 10.0.0.0/8
D. 0.0.0.0/0

**20. Two hosts are assigned to different VLANs on the same switch. What is normally required for them to exchange IP traffic?**
A. Only a larger Ethernet frame
B. A DNS CNAME record
C. Disabling MAC learning
D. Inter-VLAN routing

## Record your answers

| Topic | Your choices | Score |
|---|---|---|
| Day 16 — OSI and TCP/IP (1–4) | 1: __ 2: __ 3: __ 4: __ | __ / 4 |
| Day 17 — IP Addressing and Subnetting (5–8) | 5: __ 6: __ 7: __ 8: __ | __ / 4 |
| Day 18 — Ports and Protocols (9–12) | 9: __ 10: __ 11: __ 12: __ | __ / 4 |
| Day 19 — DNS, DHCP and Network Services (13–16) | 13: __ 14: __ 15: __ 16: __ | __ / 4 |
| Day 20 — Routing, Switching and Packet Flow (17–20) | 17: __ 18: __ 19: __ 20: __ | __ / 4 |

**Total:** __ / 20 (multiply by 5 for a percentage).

## Topic-level scorecard

| Lesson / topic | Questions | Correct | Percentage | Recommended study action |
|---|---|---|---|---|
| [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md) | 1–4 | __ / 4 | __% | Apply the scoring guide below |
| [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md) | 5–8 | __ / 4 | __% | Apply the scoring guide below |
| [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md) | 9–12 | __ / 4 | __% | Apply the scoring guide below |
| [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md) | 13–16 | __ / 4 | __% | Apply the scoring guide below |
| [Day 20 — Routing, Switching and Packet Flow](day-20-routing-switching-and-packet-flow.md) | 17–20 | __ / 4 | __% | Apply the scoring guide below |

| Score | Interpretation | Study action |
|---|---|---|
| **4/4 (100%)** | Strong baseline | Review briefly or proceed to the next topic. |
| **3/4 (75%)** | Mostly understood | Review missed concepts. |
| **2/4 (50%)** | Partial understanding | Study the lesson. |
| **0–1/4 (0–25%)** | Priority study | Complete the lesson carefully. |

**Important:** Four questions are a screening tool, **not proof of mastery**. Even a 4/4 cannot assess every skill in a lesson.

---

## Answer key and explanations

*Check only after completing all 20 questions.*

### Day 16 — OSI and TCP/IP

**1. B — Data Link**  
A conventional Ethernet switch makes local forwarding decisions at Layer 2; routers use Layer 3 IP information.  
**Review:** [Day 16 — OSI and TCP/IP — relevant section](day-16-osi-and-tcp-ip.md#osi-layers)

**2. C — Encapsulation**  
Encapsulation adds protocol information as data passes down the stack.  
**Review:** [Day 16 — OSI and TCP/IP — relevant section](day-16-osi-and-tcp-ip.md#encapsulation)

**3. D — The router forwards the IP packet in a new link-layer frame.**  
A router removes the incoming link-layer framing and builds outgoing framing for the next link while forwarding the IP packet.  
**Review:** [Day 16 — OSI and TCP/IP — relevant section](day-16-osi-and-tcp-ip.md#packet-path)

**4. A — Physical**  
Cables and signals belong to Layer 1, making the physical layer the first check.  
**Review:** [Day 16 — OSI and TCP/IP — relevant section](day-16-osi-and-tcp-ip.md#troubleshooting)

### Day 17 — IP Addressing and Subnetting

**5. C — 172.20.5.10**  
172.16.0.0 through 172.31.255.255 is private; 100.64.0.0/10 is shared address space, not RFC 1918.  
**Review:** [Day 17 — IP Addressing and Subnetting — relevant section](day-17-ip-addressing-and-subnetting.md#public-private-ipv4)

**6. B — 192.168.40.64**  
A /26 has 64-address blocks: .0–.63, .64–.127, etc. The host belongs to 192.168.40.64/26.  
**Review:** [Day 17 — IP Addressing and Subnetting — relevant section](day-17-ip-addressing-and-subnetting.md#subnet-calculation)

**7. D — fe80::7**  
IPv6 link-local addresses use fe80::/10; ff00::/8 is multicast and ::1 is loopback.  
**Review:** [Day 17 — IP Addressing and Subnetting — relevant section](day-17-ip-addressing-and-subnetting.md#ipv6-address-types)

**8. A — A default gateway**  
The destination is outside 192.168.8.0/24, so the host normally sends it to a router/default gateway.  
**Review:** [Day 17 — IP Addressing and Subnetting — relevant section](day-17-ip-addressing-and-subnetting.md#nat-gateways-segmentation)

### Day 18 — Ports and Protocols

**9. B — TCP 22 / SSH**  
SSH typically uses TCP 22 and protects remote administration; Telnet sends session data without native encryption.  
**Review:** [Day 18 — Ports and Protocols — relevant section](day-18-ports-and-protocols.md#secure-alternatives)

**10. C — DNS**  
Traditional DNS queries commonly use UDP 53, although DNS also uses TCP 53.  
**Review:** [Day 18 — Ports and Protocols — relevant section](day-18-ports-and-protocols.md#common-ports)

**11. A — UDP 443**  
HTTP/3 over QUIC commonly uses UDP 443, unlike HTTP/1.1 and HTTP/2 over TLS on TCP 443.  
**Review:** [Day 18 — Ports and Protocols — relevant section](day-18-ports-and-protocols.md#common-ports)

**12. D — A TCP connection has been established between endpoints.**  
ESTABLISHED indicates a TCP session exists; it does not prove encryption or trust.  
**Review:** [Day 18 — Ports and Protocols — relevant section](day-18-ports-and-protocols.md#connection-states)

### Day 19 — DNS, DHCP and Network Services

**13. B — DNS resolution**  
Working IP connectivity with failed hostname access points first toward DNS name resolution.  
**Review:** [Day 19 — DNS, DHCP and Network Services — relevant section](day-19-dns-dhcp-and-network-services.md#2-dns-from-a-name-to-an-address)

**14. C — AAAA**  
AAAA records contain IPv6 addresses; A records contain IPv4 addresses.  
**Review:** [Day 19 — DNS, DHCP and Network Services — relevant section](day-19-dns-dhcp-and-network-services.md#3-dns-records-you-need-to-recognize)

**15. A — Offer**  
DORA proceeds Discover, Offer, Request, Acknowledge.  
**Review:** [Day 19 — DNS, DHCP and Network Services — relevant section](day-19-dns-dhcp-and-network-services.md#5-dhcp-automatic-address-assignment)

**16. D — The queried domain name does not exist according to the response.**  
NXDOMAIN indicates the queried name does not exist according to the DNS response; SERVFAIL indicates a resolution failure.  
**Review:** [Day 19 — DNS, DHCP and Network Services — relevant section](day-19-dns-dhcp-and-network-services.md#8-practical-lab-analyze-a-dns-query)

### Day 20 — Routing, Switching and Packet Flow

**17. B — Forward it through the associated port.**  
A learned destination MAC lets the switch forward the frame to its associated port within the VLAN.  
**Review:** [Day 20 — Routing, Switching and Packet Flow — relevant section](day-20-routing-switching-and-packet-flow.md#2-how-ethernet-switching-works)

**18. C — ARP**  
ARP resolves an IPv4 next-hop address to a link-layer MAC address on the local segment.  
**Review:** [Day 20 — Routing, Switching and Packet Flow — relevant section](day-20-routing-switching-and-packet-flow.md#4-arp-finding-the-next-hop-mac-address)

**19. A — 10.20.30.0/24**  
The /24 is the most specific matching prefix, so longest-prefix matching selects it.  
**Review:** [Day 20 — Routing, Switching and Packet Flow — relevant section](day-20-routing-switching-and-packet-flow.md#5-routing-choosing-the-next-network)

**20. D — Inter-VLAN routing**  
Separate VLANs are separate Layer 2 broadcast domains; traffic between their IP networks needs routing.  
**Review:** [Day 20 — Routing, Switching and Packet Flow — relevant section](day-20-routing-switching-and-packet-flow.md#3-vlans-one-switch-multiple-logical-networks)

## Turn your baseline into a study plan

1. **Start with 0–1/4 topics.** Work through the complete lesson and its practical exercise.
2. **Then address 2/4 topics.** Study the lesson and explain the missed concepts in your own words.
3. **For 3/4 topics,** review the linked sections for missed questions and try the lesson's practical activity.
4. **For 4/4 topics,** skim the lesson's quick reference and move ahead if comfortable; a high screening score is not comprehensive mastery.
5. **After studying,** complete the separate Week 04 end-of-week knowledge check. Compare results by topic, not only by total score.

### Security+ SY0-701 alignment

This diagnostic supports networking knowledge applied across **Objective 3.1 (security architecture models and infrastructure)**, **Objective 3.2 (secure enterprise infrastructure)**, and **Objective 4.4 (security alerting and monitoring tools)** where packet and traffic interpretation is relevant. It also reinforces protocol-related concepts used throughout the exam. These are *supporting foundational topics*, not a claim that SY0-701 has a standalone subnetting domain. Refer to CompTIA's official objectives for the authoritative exam scope.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
