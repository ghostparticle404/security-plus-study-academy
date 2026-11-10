# Day 20 — Routing, Switching and Packet Flow

**Security+ Study Academy | Week 04 | Day 20**

**Prerequisites:** [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md), [Day 17 — IP Addressing and Subnetting](day-17-ip-addressing-and-subnetting.md), [Day 18 — Ports and Protocols](day-18-ports-and-protocols.md), and [Day 19 — DNS, DHCP and Network Services](day-19-dns-dhcp-and-network-services.md).  
**Learning outcome:** Explain routing, switching, ARP, and packet forwarding.  
**Practical application:** Inspect packets in Wireshark.  
**Assessment focus:** Traffic-path reasoning.

## Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish Layer 2 switching from Layer 3 routing.
2. Explain how switches learn MAC addresses and forward Ethernet frames.
3. Describe ARP in IPv4 networks and Neighbor Discovery in IPv6 networks.
4. Determine when a host sends traffic directly to a peer or to its default gateway.
5. Trace packet forwarding across subnets, including changing Ethernet headers and IP TTL.
6. Interpret a basic Wireshark capture and connect network behavior to security controls.

## 1. Two different delivery jobs

Imagine a university campus. **A switch** is like the mailroom inside one building: it knows which desk receives mail on which hallway. **A router** is like the campus delivery service: it decides which building or campus the package should go to next.

The distinction matters because a device can have the correct IP address and still fail to communicate if it cannot reach the right switch port, resolve the next-hop MAC address, or find a route.

| Feature | Switch (typical Layer 2) | Router (Layer 3) |
|---|---|---|
| Primary forwarding information | Destination MAC address | Destination IP address |
| Forwards | Ethernet frames | IP packets |
| Main lookup | MAC/CAM table | Routing table/FIB |
| Typical boundary | VLAN / broadcast domain | Between IP networks |
| Key security features | VLANs, port security, DHCP snooping | ACLs, route filtering, segmentation |

**Nuance:** Multilayer switches can perform Layer 3 routing, and some devices combine switching, routing, firewall, and wireless functions. The exam often asks about the *function*, not the box's marketing name.

## 2. How Ethernet switching works

An Ethernet frame includes a **source MAC address**, **destination MAC address**, an EtherType or length field, a payload, and a frame check sequence (FCS). A switch normally learns which MAC addresses are reachable on its ports by inspecting **source** MAC addresses on incoming frames.

### The MAC address table

Suppose three computers connect to a switch:

| Port | Learned MAC | Device |
|---|---|---|
| 1 | `02:00:00:00:00:11` | Alice |
| 2 | `02:00:00:00:00:22` | Bob |
| 3 | `02:00:00:00:00:33` | Carol |

When Alice sends a frame to Bob, the switch checks its forwarding table and sends the frame out port 2—not every port. If Bob's destination MAC is **unknown**, the switch ordinarily floods the frame out other eligible ports in the same VLAN. Once Bob responds, the switch can learn his source MAC.

### Three important traffic types

- **Unicast:** One sender to one destination MAC; a learned unicast is forwarded to the relevant port.
- **Broadcast:** One sender to all devices in the local broadcast domain, using `ff:ff:ff:ff:ff:ff` in Ethernet.
- **Multicast:** One sender to a group; forwarding depends on multicast membership and switch configuration.

**Security+ connection:** A **MAC flooding** attack attempts to exhaust or disrupt a switch's forwarding table, potentially increasing flooding. Defenses may include port security, switch hardening, and monitoring. A switch is not automatically a confidentiality boundary for all traffic.

## 3. VLANs: One switch, multiple logical networks

A **virtual LAN (VLAN)** separates Layer 2 broadcast domains even when devices share physical switching hardware. For example:

| VLAN | Purpose | Example subnet |
|---|---|---|
| 10 | Employees | `192.168.10.0/24` |
| 20 | Guests | `192.168.20.0/24` |
| 30 | Management | `192.168.30.0/24` |

An **access port** typically carries traffic for one VLAN to an end device. A **trunk port** can carry multiple VLANs between network devices, commonly using IEEE 802.1Q tags. Traffic between VLANs requires a Layer 3 forwarding function, such as a router or multilayer switch, and may be restricted by ACLs or firewall policy.

**Exam trap:** VLANs provide segmentation, but a VLAN alone does not encrypt traffic or guarantee that every policy between segments is secure.

## 4. ARP: Finding the next-hop MAC address

IPv4 hosts use the **Address Resolution Protocol (ARP)** to discover the MAC address associated with an on-link IPv4 address.

Suppose Alice has:

```text
IP address:      192.168.10.25/24
Default gateway: 192.168.10.1
Destination:     192.168.10.40
```

Alice determines that `192.168.10.40` is in her local `/24` network. If she does not already know its MAC address, she broadcasts an ARP request: *Who has 192.168.10.40?* The destination host replies with its MAC address. Alice can then send an Ethernet frame directly to that MAC address.

Now change the destination to `192.168.20.40`. That IP is **off-link**. Alice does **not** ARP for the remote host's MAC. She resolves the **default gateway's** MAC address (`192.168.10.1`) and sends the frame to the gateway, while the IP packet's destination remains `192.168.20.40`.

### ARP cache and security

Operating systems cache recently learned IP-to-MAC mappings. On a machine you control, you can inspect the cache with:

```bash
arp -a
```

**ARP spoofing/poisoning** occurs when an attacker causes hosts to associate an IP address (often the gateway) with the wrong MAC address. It can enable interception or disruption on a local network. Possible defenses include **Dynamic ARP Inspection (DAI)**, properly configured DHCP snooping bindings, segmentation, and endpoint protections.

**IPv6 distinction:** IPv6 does **not** use ARP. It uses **Neighbor Discovery Protocol (NDP)** through ICMPv6 Neighbor Solicitation and Neighbor Advertisement messages. IPv6 Router Advertisements also support router discovery and address configuration. Do not block all ICMPv6 indiscriminately; some messages are essential to IPv6 operation.

## 5. Routing: Choosing the next network

Routers examine the destination IP address and consult routing information to select a next hop or outgoing interface. A route generally contains a destination prefix, a next hop or interface, and associated selection information.

### Longest-prefix match

A router has these illustrative routes:

| Destination prefix | Next hop |
|---|---|
| `10.0.0.0/8` | Router A |
| `10.20.0.0/16` | Router B |
| `10.20.30.0/24` | Router C |
| `0.0.0.0/0` | Default route |

For destination `10.20.30.45`, the router chooses `10.20.30.0/24` because it is the **most specific matching prefix**. For `10.20.99.7`, it chooses `/16`. For an otherwise unmatched IPv4 destination, it uses the default route if one exists.

A **default route** (`0.0.0.0/0` for IPv4; `::/0` for IPv6) is the fallback route—not proof that the destination is reachable.

### Static versus dynamic routing

- **Static routes** are manually configured and can be appropriate for simple, predictable paths.
- **Dynamic routing protocols** exchange reachability information and adapt to changes. Examples include **OSPF** (link-state interior routing), **BGP** (interdomain path-vector routing), and **RIP** (distance-vector, mainly of historical/legacy relevance).

Routing protocols do not automatically provide end-to-end encryption. Misconfiguration or malicious route announcements can redirect or blackhole traffic; routing authentication, filtering, and monitoring help reduce risk.

## 6. Follow a packet across two subnets

Let's trace a simplified TCP connection from Alice to a web server. Assume the two LANs are connected by one router and there is no NAT.

```text
Alice                    Router                    Web server
192.168.10.25/24         LAN 1: 192.168.10.1       192.168.20.40/24
MAC AA                   LAN 2: 192.168.20.1       MAC DD
                         MAC BB / MAC CC
     |                         |                          |
     | Ethernet: AA -> BB      |                          |
     | IP: .10.25 -> .20.40   |                          |
     |------------------------>|                          |
     |                         | Ethernet: CC -> DD       |
     |                         | IP: .10.25 -> .20.40    |
     |                         |------------------------->|
```

### Step-by-step packet path

1. **Application/transport:** Alice's browser prepares an HTTPS connection to the server on TCP port 443. TCP constructs a segment; IP encapsulates it in a packet.
2. **Subnet decision:** Alice compares the destination IP to her local network prefix and determines it is remote.
3. **Next-hop resolution:** Alice looks up or learns the gateway's MAC address (MAC BB) using ARP.
4. **First Ethernet frame:** Alice sends an Ethernet frame with source MAC AA and destination MAC BB. Inside it, the IP packet has source `192.168.10.25` and destination `192.168.20.40`.
5. **Router forwarding:** The router removes the incoming Ethernet framing, examines the IP destination, selects the outgoing interface, and decrements the IPv4 TTL by one. It updates the IPv4 header checksum accordingly.
6. **Second next-hop resolution:** The router resolves the web server's MAC (MAC DD) on the second LAN if needed.
7. **New Ethernet frame:** The router encapsulates the forwarded IP packet in a *new* Ethernet frame with source MAC CC and destination MAC DD.
8. **Delivery:** The server receives the frame, processes the IP packet and TCP segment, and replies along an appropriate return path.

### What changes at the router?

| Field | Before router | After router | Why |
|---|---|---|---|
| Ethernet source MAC | AA | CC | New local link |
| Ethernet destination MAC | BB | DD | New next hop |
| Source IPv4 address | `192.168.10.25` | Same | No NAT in this example |
| Destination IPv4 address | `192.168.20.40` | Same | Still the final destination |
| IPv4 TTL | e.g., 64 | 63 | Each forwarding router decrements TTL |
| TCP destination port | 443 | 443 | Routing does not normally change ports |

**Important exceptions:** NAT may rewrite IP addresses and sometimes transport ports; tunneling adds outer headers; and security devices may filter or otherwise process traffic. The table describes ordinary forwarding without those features. In IPv6, the equivalent field is **Hop Limit**, and routers do not update an IPv6 header checksum because there is none.

## 7. Why loops, missing routes, and bad gateways matter

Routing and switching failures often look like application problems:

| Symptom | Plausible cause | First useful check |
|---|---|---|
| Can reach same-subnet peers, not other subnets | Missing/wrong gateway or route | IP settings and routing table |
| ARP request repeats, no reply | Peer absent, wrong VLAN, filtering, or L2 issue | ARP cache and capture |
| Ping reaches gateway, but not remote network | Routing or downstream filtering | Route table, traceroute, ACLs |
| Intermittent LAN disruption | Layer 2 loop, link issue, or instability | Switch logs, STP state |
| Only one VLAN cannot reach a service | Inter-VLAN ACL, firewall, or route | VLAN and policy configuration |

### Spanning Tree Protocol (STP)

Redundant Ethernet links can create Layer 2 loops, which may cause broadcast storms and unstable MAC learning. **STP** and its faster variants, such as **RSTP**, prevent loops by placing selected redundant paths into non-forwarding states. A blocked STP port is not necessarily broken; it may be doing its job.

### Traceroute and TTL

`traceroute` (macOS/Linux) or `tracert` (Windows) can reveal some intermediate hops by sending probes with increasing TTL/Hop Limit values. Routers may respond with ICMP Time Exceeded messages. Missing or starred hops do **not** automatically mean the route is broken: filtering, rate limiting, or nonresponsive devices can hide hops.

## 8. Practical lab: Inspect packet flow in Wireshark

**Goal:** Identify local versus routed traffic, ARP behavior, IP endpoints, and the layers visible in a packet capture.

**Safety:** Capture only traffic from your own device or a network where you have permission. Avoid sharing captures containing credentials, private addresses tied to identifiable users, or personal data. If live capture is unavailable, use an instructor-provided sanitized PCAP.

### Part A — Record your network configuration

On macOS or Linux, inspect your network settings using tools available on your system:

```bash
ifconfig
netstat -rn
arp -a
```

On Windows:

```powershell
ipconfig /all
route print
arp -a
```

Write down your device's IP address, prefix or subnet mask, default gateway, and one ARP cache entry if available. Do not change network settings.

### Part B — Capture or open a PCAP

1. Open Wireshark and choose the interface carrying your permitted traffic, or open a sanitized PCAP.
2. If using live capture, generate ordinary browsing traffic to a site you trust; your system may already have cached DNS and ARP information.
3. Apply these **display filters**, one at a time:

```text
arp
```

```text
icmp || icmpv6
```

```text
ip && tcp.port == 443
```

```text
eth.addr == 02:00:00:00:00:11
```

The last filter uses a **fictional example MAC**. Replace it with a MAC visible in your own authorized capture.

4. Select an Ethernet frame and expand **Ethernet II**, **Internet Protocol**, and **Transmission Control Protocol** (where present).
5. Compare source and destination MAC addresses with source and destination IP addresses.

**Note:** On loopback, VPN, Wi-Fi monitor-mode, or some virtual interfaces, the capture may not show ordinary Ethernet headers. That is expected and does not invalidate the IP-level analysis.

### Part C — Packet-path annotation worksheet

For each packet you choose, fill in:

| Field | Your observation |
|---|---|
| Frame number | |
| Source MAC | |
| Destination MAC | |
| Source IP | |
| Destination IP | |
| Protocol (ARP, ICMP, TCP, etc.) | |
| Source and destination ports, if present | |
| Local or off-subnet destination? | |
| Expected next hop | |
| Evidence supporting your conclusion | |

**Interpretation prompts:**

1. If the destination IP is remote but the destination MAC is your gateway, is the frame incorrectly addressed?
2. If an ARP request has no TCP port, is that an error?
3. If the same IP packet is forwarded through a router, should its Ethernet MAC addresses remain unchanged?
4. Can one capture point always show both sides of a router?

### Worked example

Alice (`192.168.10.25/24`, MAC AA) sends a TCP packet to `192.168.20.40:443` via gateway `192.168.10.1` (MAC BB).

- **Local or remote?** Remote: the destination is outside `192.168.10.0/24`.
- **First-hop destination MAC?** BB, the gateway's MAC—not the server's MAC.
- **IP destination?** Still `192.168.20.40`.
- **Transport destination?** TCP 443.
- **Can ARP resolve the remote server directly?** Not in this ordinary routed setup; Alice ARPs for the local gateway.

### Lab answers

1. **No.** Ethernet delivers to the next hop, while IP identifies the final destination.
2. **No.** ARP is not TCP or UDP and has no transport-layer ports.
3. **No.** A router builds a new Layer 2 frame on the outgoing link.
4. **No.** A capture generally shows only traffic visible at its observation point; capturing both sides requires suitable access or multiple captures.

## 9. Security scenarios: Choose the right layer

**Scenario A — Guest Wi-Fi can reach the management subnet.** The networks have separate VLANs but no effective Layer 3 restriction. **Think:** inter-VLAN routing policy, firewall rules, and ACLs.

**Scenario B — A workstation suddenly associates the gateway IP with an unfamiliar MAC.** **Think:** possible ARP poisoning, but verify against authorized network changes and switch data before concluding there is an attack.

**Scenario C — The switch reports a flood of broadcasts after a redundant cable is connected.** **Think:** a possible Layer 2 loop and STP/RSTP configuration.

**Scenario D — The route to `10.20.30.45` unexpectedly uses a `/24` route instead of a `/16` route.** **Think:** longest-prefix match; the more specific `/24` wins when both match.

**Scenario E — HTTPS works to a local server but fails to a remote server.** **Think:** gateway, routes, firewall policies, DNS if using a hostname, and service reachability. Do not assume port 443 itself is the cause.

## 10. Common Security+ exam traps

- **MAC versus IP:** A switch normally forwards using MAC addresses; a router selects routes using IP addresses.
- **Next hop versus final destination:** The Ethernet destination can be the gateway while the IP destination is the remote server.
- **ARP versus DNS:** ARP resolves an on-link IPv4 address to a MAC address; DNS resolves names to records such as A or AAAA.
- **ARP versus IPv6 NDP:** IPv6 uses Neighbor Discovery, not ARP.
- **VLAN versus subnet:** They often align in designs, but they are different concepts at different layers.
- **STP versus routing:** STP prevents Layer 2 loops; routing determines Layer 3 paths.
- **TTL versus TCP port:** A router decrements TTL/Hop Limit; it does not normally change the TCP destination port.
- **Default route versus guaranteed delivery:** A default route supplies a next hop, not an assurance of end-to-end connectivity.

## 11. Knowledge check — 10 questions

Choose the **best** answer. Try the questions before consulting the answer key.

**1. A switch receives a unicast Ethernet frame whose destination MAC is not in its forwarding table. What will it typically do?**
A. Drop every unknown unicast frame by definition
B. Flood the frame to other eligible ports in the same VLAN
C. Route the frame to the internet
D. Convert it into an ARP reply

**2. A workstation at `192.168.10.25/24` needs to send a packet to `192.168.20.40`. Which MAC address should it normally use as the Ethernet destination?**
A. The remote server's MAC
B. The broadcast MAC
C. The local default gateway's MAC
D. Its own MAC

**3. Which protocol resolves an on-link IPv4 address to a MAC address?**
A. DNS
B. ARP
C. DHCP
D. NTP

**4. Which mechanism replaces ARP's neighbor-resolution role in IPv6?**
A. ICMPv6 Neighbor Discovery
B. TCP three-way handshake
C. DNSSEC
D. OSPF

**5. A router has matching routes `10.0.0.0/8`, `10.20.0.0/16`, and `10.20.30.0/24`. Which route wins for `10.20.30.45`?**
A. `/8` because it covers the most addresses
B. `/16` because it is in the middle
C. `/24` because it is the longest matching prefix
D. None; the router must use the default route

**6. What normally changes when a router forwards an IPv4 packet without NAT?**
A. The IP destination becomes the router's IP
B. The Ethernet source/destination MAC addresses and IPv4 TTL
C. The TCP destination port always becomes 80
D. The source and destination IP addresses both change

**7. What is the main purpose of STP/RSTP in a switched network?**
A. Encrypt Ethernet frames
B. Assign IPv4 addresses
C. Prevent Layer 2 forwarding loops
D. Resolve domain names

**8. Which feature commonly carries traffic for multiple VLANs between switches?**
A. An 802.1Q trunk
B. A DNS MX record
C. An ARP cache entry
D. A default IPv4 route

**9. Which control is most directly associated with checking ARP messages against trusted bindings?**
A. DNSSEC
B. Dynamic ARP Inspection
C. NTP authentication
D. TLS certificate validation

**10. A traceroute displays `* * *` at one intermediate hop, but later hops respond. What is the best conclusion?**
A. The network is definitely compromised
B. The packet could not have reached any later hop
C. The intermediate device may not answer probes; traffic can still pass through
D. The DNS server has failed

### Answer key and explanations

| Q | Answer | Explanation |
|---|---|---|
| 1 | **B** | Unknown unicast frames are ordinarily flooded within the VLAN. Review [switching](#2-how-ethernet-switching-works). |
| 2 | **C** | The host sends off-subnet traffic to its local gateway's MAC. Review [ARP](#4-arp-finding-the-next-hop-mac-address). |
| 3 | **B** | ARP maps on-link IPv4 addresses to MAC addresses. Review [ARP](#4-arp-finding-the-next-hop-mac-address). |
| 4 | **A** | IPv6 uses ICMPv6 Neighbor Discovery. Review [IPv6 distinction](#4-arp-finding-the-next-hop-mac-address). |
| 5 | **C** | The `/24` is the most specific matching route. Review [longest-prefix match](#longest-prefix-match). |
| 6 | **B** | Routing changes the link-layer frame and decrements TTL, without normally rewriting IP endpoints. Review [packet path](#6-follow-a-packet-across-two-subnets). |
| 7 | **C** | STP/RSTP prevents Layer 2 loops over redundant paths. Review [STP](#spanning-tree-protocol-stp). |
| 8 | **A** | 802.1Q trunks commonly transport multiple VLANs. Review [VLANs](#3-vlans-one-switch-multiple-logical-networks). |
| 9 | **B** | DAI can validate ARP traffic against trusted bindings. Review [ARP security](#arp-cache-and-security). |
| 10 | **C** | Missing traceroute replies do not necessarily indicate packet loss at that hop. Review [traceroute](#traceroute-and-ttl). |

## 12. Exam-day quick reference

| Clue | Think |
|---|---|
| Frame delivered within one VLAN | Layer 2 switching, MAC table |
| Packet crossing subnets | Layer 3 routing, gateway |
| Unknown destination MAC | Unknown-unicast flooding |
| IPv4 IP-to-MAC lookup | ARP |
| IPv6 neighbor lookup | ICMPv6 NDP |
| Unexpected gateway MAC | Possible ARP spoofing; investigate |
| Multiple VLANs on one uplink | 802.1Q trunk |
| Redundant links causing broadcast storm | STP/RSTP |
| Multiple matching IP routes | Longest-prefix match |
| No specific route | Default route, if configured |
| Router hop | New Layer 2 header; TTL/Hop Limit decreases |
| Separate VLANs need communication | Inter-VLAN routing plus policy |

**Takeaway:** The **IP destination** tells the network where a packet ultimately belongs. The **destination MAC** tells the current link where to deliver its frame next. Switching handles local frame delivery; routing moves packets between networks. Security analysts need both views to understand where traffic actually went.

**Next:** Return to the [Week 04 review](review.w4.md) after all five lessons are complete. Compare your diagnostic and post-check results, and revisit any lesson sections that need reinforcement.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
