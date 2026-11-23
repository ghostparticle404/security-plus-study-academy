# Day 17 — IP Addressing and Subnetting: Giving Every Device a Place on the Network

**Security+ Study Academy | Week 04 · Day 17**  
**Topic:** IP addressing, CIDR, and network segmentation  
**Prerequisite:** [Day 16 — OSI and TCP/IP](day-16-osi-and-tcp-ip.md)  
**Estimated study time:** 90–120 minutes
**CompTIA Security+ SY0-701 alignment:** Domain 3, Objective 3.2 (secure enterprise infrastructure); related Objective 2.5 (segmentation and mitigation). IP addressing and subnet arithmetic are supporting networking foundations, not separately enumerated exam objectives.  
**Learning outcome:** Interpret IPv4, IPv6, CIDR notation, and network ranges  
**Practical application:** Work through CIDR and subnet calculations  
**Assessment focus:** Addressing decisions

## 1. Learning objectives

By the end of this lesson, you should be able to:

1. Distinguish IPv4 from IPv6 and recognize common address types.
2. Explain the network and host portions of an IP address.
3. Translate common CIDR prefixes into IPv4 subnet masks.
4. Determine IPv4 network addresses, broadcast addresses, and usable host ranges.
5. Decide whether two IPv4 addresses belong to the same subnet.
6. Explain how addressing and subnetting support network segmentation and security.
7. Inspect local IP configuration without changing any settings.

## 2. The big idea: Why do IP addresses matter? {#why-ip-addresses-matter}

> **The big idea:** An IP address tells the network *where* a device is reachable. A subnet mask or prefix tells it which addresses are neighbors and which require a router. Subnetting is how we turn one enormous digital neighborhood into manageable, securable streets.

Imagine a large apartment building. The building's street address gets a delivery truck to the right property; the apartment number tells the courier which door to visit. Networks need a similar hierarchy.

- An **IP address** identifies a network interface's logical address.
- A **network prefix** identifies the network to which an address belongs.
- A **host/interface portion** identifies an address within that network.
- A **router** forwards traffic between IP networks.

Recall [Day 16](day-16-osi-and-tcp-ip.md#osi-layers): IP operates at **OSI Layer 3 (Network)**. Ethernet MAC addresses operate at **Layer 2 (Data Link)**. A MAC address is not a substitute for an IP address, and an IP address is not a port number.

**Security connection:** If an analyst sees traffic from `192.168.50.23` to `192.168.60.10`, knowing the subnet boundaries helps identify whether that communication crosses a routed security boundary.

## 3. IPv4: Four octets, 32 bits 

An IPv4 address has **32 bits**, conventionally written as four decimal **octets** separated by dots:

```text
192 . 168 . 10 . 42
 8     8    8    8 bits = 32 bits
```

Each octet represents a value from **0 to 255**. A valid example is `10.20.30.40`; `10.20.300.40` is invalid because `300` exceeds `255`.

Each bit has a place value:

```text
Bit position:  7   6   5   4   3   2   1   0
Place value: 128  64  32  16   8   4   2   1
Example 192:  1   1   0   0   0   0   0   0
              128 + 64 = 192
```

You do **not** need to convert every address to binary by hand. But understanding the bit positions makes CIDR calculations much less mysterious.

### Binary practice: read an octet

Each bit is either 0 or 1. Add only the place values whose bits are 1. For example, `10101010` = 128 + 32 + 8 + 2 = 170. Conversely, to encode 42, choose 32 + 8 + 2, giving `00101010`. Leading zeros are essential when writing an eight-bit octet.

🧠 **Memory hook:** Every step left doubles the value; every step right halves it.

### Public and private IPv4 {#public-private-ipv4}

**Private IPv4 addresses** are intended for internal networks and are not globally routed on the public Internet:

| Private block | Range | Common use |
|---|---|---|
| `10.0.0.0/8` | `10.0.0.0`–`10.255.255.255` | Large internal networks |
| `172.16.0.0/12` | `172.16.0.0`–`172.31.255.255` | Internal networks |
| `192.168.0.0/16` | `192.168.0.0`–`192.168.255.255` | Home and small-business networks |

**Watch the trap:** `172.20.4.8` is private, but `172.32.4.8` is **not** in the RFC 1918 private block. Likewise, `192.169.1.5` is not private merely because it starts with `192`.

A **public IPv4 address** may be globally routable, provided it is actually allocated and not part of another reserved or special-use range. Not every address outside RFC 1918 is usable on the public Internet.

### Other IPv4 addresses worth recognizing {#special-ipv4}

| Address or range | Meaning |
|---|---|
| `127.0.0.0/8` | Loopback; `127.0.0.1` refers to the local machine |
| `169.254.0.0/16` | IPv4 link-local; often associated with automatic addressing when DHCP is unavailable |
| `0.0.0.0` | Unspecified address; `0.0.0.0/0` denotes the default route |
| `255.255.255.255` | Limited broadcast address |
| `100.64.0.0/10` | Shared address space, often used for carrier-grade NAT; **not** RFC 1918 private space |

**Troubleshooting clue:** A workstation unexpectedly using `169.254.x.x` may have failed to obtain an address from DHCP. This is a clue, not proof: link-local addressing can also be intentional.

## 4. IPv6: More space, different conventions {#ipv6-basics}

IPv6 addresses contain **128 bits** and are written as eight groups of hexadecimal digits (0–9 and a–f):

```text
2001:0db8:0000:0000:0000:0000:0000:0042
```

IPv6 allows two useful shortening rules:

1. Omit **leading zeros** within each group: `0db8` becomes `db8`.
2. Replace **one contiguous run** of all-zero groups with `::` (only once in an address).

Thus the example becomes:

```text
2001:db8::42
```

`2001:db8::/32` is reserved for **documentation**, so it is safe for textbook examples.

### Expand an IPv6 address

To expand `2001:db8::42`, count the three explicitly written groups. Insert five zero groups to reach eight, then pad each group to four hexadecimal digits. A hexadecimal digit represents four bits, so eight groups of four digits contain 128 bits. An address such as `2001::db8::42` is invalid because two compression markers make expansion ambiguous.

IPv6 prefixes also identify network bits. In `2001:db8:1234:10::42/64`, the first four groups form the 64-bit prefix. The remaining bits identify an interface within that prefix. A device can hold multiple IPv6 addresses simultaneously, including link-local and global addresses. Do not apply IPv4 broadcast or minus-two host arithmetic to IPv6.

### Common IPv6 address categories {#ipv6-address-types}

| IPv6 address or prefix | Purpose |
|---|---|
| `2000::/3` | Global unicast allocation space |
| `fe80::/10` | Link-local unicast; used on local links |
| `fc00::/7` | Unique local addresses (ULA), commonly `fd...` in practice |
| `::1/128` | Loopback |
| `::/128` | Unspecified address |
| `ff00::/8` | Multicast |

IPv6 has **no broadcast**. It uses multicast and other mechanisms instead. Neighbor Discovery Protocol (NDP), which uses ICMPv6, replaces several IPv4-era local discovery functions, including ARP's address-resolution role.

### IPv4 vs. IPv6 at a glance 

| Feature | IPv4 | IPv6 |
|---|---|---|
| Address size | 32 bits | 128 bits |
| Notation | Dotted decimal | Colon-separated hexadecimal |
| Example | `192.168.10.42` | `2001:db8::42` |
| Local address resolution | ARP | NDP (ICMPv6) |
| Broadcast | Yes | No |
| Common LAN prefix | `/24` (varies) | `/64` (common) |
| Address assignment | Static, DHCP, other methods | Static, SLAAC, DHCPv6, other methods |

**Exam caution:** IPv6 support for IPsec does **not** mean every IPv6 connection is automatically encrypted. Security depends on configuration and the protocols actually used.

## 5. The network and host portions 

Consider `192.168.10.42/24`:

```text
192 . 168 . 10 . 42
|  network  | host |
       /24 = first 24 bits are the network prefix
```

The `/24` is **CIDR notation** (Classless Inter-Domain Routing). It says that the first 24 bits belong to the network prefix, leaving **8 bits** for the host portion.

For IPv4:

```text
Host bits = 32 − prefix length
Total addresses = 2^(host bits)
```

For an ordinary IPv4 subnet with at least two host bits, the typical usable-host calculation is:

```text
Usable host addresses = 2^(host bits) − 2
```

Why subtract two? Conventionally, the all-zero host value is the **network address**, and the all-one host value is the **directed broadcast address**.

**Important exceptions:** `/31` networks can support two addresses on point-to-point links (RFC 3021), and a `/32` identifies one IPv4 address. Do not blindly apply the “minus two” rule to those cases.

## 6. CIDR prefixes and subnet masks {#cidr-and-masks}

An IPv4 **subnet mask** is a 32-bit pattern of leading `1` bits followed by `0` bits. Its decimal form tells you which address bits represent the network.

```text
/24 mask:
11111111.11111111.11111111.00000000
255     .255     .255     .0
```

| Prefix | Subnet mask | Total IPv4 addresses | Typical usable hosts |
|---|---|---:|---:|
| `/16` | `255.255.0.0` | 65,536 | 65,534 |
| `/24` | `255.255.255.0` | 256 | 254 |
| `/25` | `255.255.255.128` | 128 | 126 |
| `/26` | `255.255.255.192` | 64 | 62 |
| `/27` | `255.255.255.224` | 32 | 30 |
| `/28` | `255.255.255.240` | 16 | 14 |
| `/29` | `255.255.255.248` | 8 | 6 |
| `/30` | `255.255.255.252` | 4 | 2 |
| `/31` | `255.255.255.254` | 2 | 2 on point-to-point links |
| `/32` | `255.255.255.255` | 1 | One address/host route |

**Pattern to notice:** Increasing a prefix by one bit **halves** the address block. A `/26` is half of a `/25`; a `/27` is half of a `/26`.

### How a subnet mask actually works: binary AND

A host obtains its network address by applying a bitwise AND to its IPv4 address and subnet mask. AND returns 1 only when both input bits are 1. Mask bits of 1 preserve network bits; mask bits of 0 clear host bits.

For `192.168.10.77/26`, the first three octets are unchanged. Examine the fourth:

```text
Address:    01001101  (77)
Mask:       11000000  (192)
AND result: 01000000  (64)
```

The network is `192.168.10.64/26`. Set all six host bits to 1 (`01111111`) to obtain broadcast `.127`. This is why the block-size shortcut works: it finds the boundaries imposed by the fixed prefix bits.

### The block-size shortcut 

For a subnet boundary within an octet, subtract the mask's value in that octet from **256**:

```text
/26 = 255.255.255.192
256 − 192 = 64
```

Therefore the last-octet network boundaries are:

```text
0, 64, 128, 192
```

Each `/26` block contains 64 addresses. The boundary is the first address of a block, not its first usable host.

## 7. Calculate a subnet, step by step {#subnet-calculation}

**Question:** What network contains `192.168.10.77/26`?

**Step 1 — Find the mask.** `/26` is `255.255.255.192`.

**Step 2 — Find the block size.** `256 − 192 = 64`.

**Step 3 — List the boundaries.** `0, 64, 128, 192`.

**Step 4 — Locate the address.** `77` falls in the `64–127` block.

**Step 5 — Identify the endpoints.**

```text
Network:          192.168.10.64
First usable:     192.168.10.65
Last usable:      192.168.10.126
Broadcast:        192.168.10.127
Total addresses:  64
Usable hosts:     62
```

### A second example: `/27` 

**Question:** Find the subnet for `10.5.8.142/27`.

- Mask: `255.255.255.224`
- Block size: `256 − 224 = 32`
- Boundaries: `0, 32, 64, 96, 128, 160, 192, 224`
- `142` falls in `128–159`.

**Answer:** Network `10.5.8.128`, broadcast `10.5.8.159`, usable range `10.5.8.129–10.5.8.158`, with **30** typical usable host addresses.

### A third example: boundaries in the third octet 

**Question:** Find the network for `172.20.5.200/23`.

A `/23` mask is `255.255.254.0`. The block size is `256 − 254 = 2` in the **third octet**.

```text
Third-octet blocks: 0–1, 2–3, 4–5, 6–7, ...
5 falls in 4–5.

Network:          172.20.4.0
Broadcast:        172.20.5.255
First usable:     172.20.4.1
Last usable:      172.20.5.254
Typical hosts:    510
```

**Exam trap:** Not every subnet boundary is in the fourth octet. Read the mask before doing arithmetic.

## 8. Are two addresses on the same subnet? 

For ordinary IPv4 unicast communication, a host uses its configured prefix to decide whether the destination is **on-link** or should be sent toward a router. The comparison is based on the network prefix, not simply the first three decimal octets.

| Source | Destination | Prefix | Same subnet? | Why? |
|---|---|---|---|---|
| `192.168.10.10` | `192.168.10.200` | `/24` | Yes | Both in `192.168.10.0/24` |
| `192.168.10.10` | `192.168.11.10` | `/24` | No | Different `/24` networks |
| `192.168.10.10` | `192.168.10.200` | `/25` | No | First is in `.0–.127`; second in `.128–.255` |
| `10.5.8.142` | `10.5.8.158` | `/27` | Yes | Both in `.128–.159` |

**Security implication:** Separate subnets commonly communicate through routers or Layer 3 switches, where administrators can apply access control lists (ACLs), firewall rules, and monitoring. **A subnet by itself is not a firewall.** Proper segmentation requires enforcement, not just different address ranges.

## 9. NAT, gateways, and segmentation 

A **default gateway** is the next-hop router a host normally uses for destinations that are not on its local network and for which no more-specific route exists.

**Network Address Translation (NAT)** can rewrite IP addresses as traffic crosses a boundary. A common variant, **PAT** (Port Address Translation), also uses port mappings so multiple private hosts can share one public IPv4 address.

```text
Laptop 192.168.10.42 ──┐
                       ├── Router/NAT ── Internet
Phone  192.168.10.43 ──┘   public IPv4
```

NAT conserves IPv4 addresses, but **NAT is not a substitute for a firewall** or a comprehensive security policy.

### Segmentation scenario {#segmentation-scenario}

A small law firm has three groups:

| Network | Example prefix | Intended access |
|---|---|---|
| Staff devices | `192.168.10.0/24` | Authorized business systems |
| Guest Wi-Fi | `192.168.20.0/24` | Internet only |
| Case-file servers | `192.168.30.0/24` | Approved staff/services only |

The addresses make the groups distinguishable. VLANs, routing policy, firewall rules, identity controls, and logging help make the separation meaningful.

**Ask yourself:** If a guest laptop can still reach the case-file server, has segmentation achieved its security goal? **No.** Different subnet labels alone do not prevent access.

## 10. Worked case study: Address planning for a law firm

A firm has one available block, `192.168.40.0/24`, and needs separate staff (50 hosts), guest (25 hosts), and evidence-server (10 hosts) networks. Include gateway addresses in capacity planning; allow growth when possible.

Allocate the largest requirements first using variable-length subnet masks (VLSM):

| Group | Prefix | Usable range | Broadcast | Capacity |
|---|---|---|---|---:|
| Staff | `192.168.40.0/26` | `.1–.62` | `.63` | 62 |
| Guests | `192.168.40.64/27` | `.65–.94` | `.95` | 30 |
| Evidence | `192.168.40.96/28` | `.97–.110` | `.111` | 14 |

These blocks do not overlap. Addresses `.112–.255` remain available for future allocations, subject to valid prefix boundaries. Reserve a usable address in each subnet for its gateway. Assign VLANs and enforce a default-deny inter-segment policy with explicit approved exceptions. Permit guests to reach the Internet, not evidence servers; allow only authorized staff services to reach evidence systems; log denied attempts. Apply equivalent IPv6 policy if IPv6 is enabled.

**CIA connection:** Segmentation reduces unauthorized disclosure and modification and can limit the spread of incidents that disrupt availability. It does not replace endpoint security or identity controls.

**Exam trap:** NAT changes addressing, not authorization. Overlapping networks can also cause ambiguous routing and VPN connectivity problems.

## 11. Guided notes — Write first, reveal later

| Prompt | Your answer |
|---|---|
| IPv4 and IPv6 lengths and notation | _Write here._ |
| Three RFC 1918 ranges | _Write here._ |
| IPv4 and IPv6 link-local and loopback examples | _Write here._ |
| CIDR prefix meaning and host formula | _Write here._ |
| How does binary AND identify a network? | _Write here._ |
| Mask, block size and usable hosts for /26 | _Write here._ |
| How do you decide whether a destination is on-link? | _Write here._ |
| NAT versus PAT | _Write here._ |
| Why are separate subnets not sufficient security? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
|---|---|
| Address lengths | IPv4: 32 bits, dotted decimal; IPv6: 128 bits, hexadecimal. |
| Private ranges | `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`. |
| Special addresses | IPv4 link-local `169.254.0.0/16`, loopback `127.0.0.1`; IPv6 link-local `fe80::/10`, loopback `::1`. |
| CIDR | Prefix counts network bits; ordinary IPv4 usable hosts = 2^(32 − prefix) − 2, with /31 and /32 exceptions. |
| AND | Preserve bits where mask is 1; clear host bits where mask is 0. |
| /26 | `255.255.255.192`, 64 addresses, 62 typical usable hosts. |
| On-link | Compare network prefixes using the configured mask and routing information. |
| NAT/PAT | NAT translates addresses; PAT also maps ports. |
| Segmentation | Routing, ACLs and firewalls must enforce permitted flows. |

</details>

## 12. Hands-on lab: Work through CIDR examples {#practical-exercise} 

**Goal:** Calculate network boundaries and make safe addressing decisions. No special software or administrator privileges required.

### Part A — Pencil-and-paper subnetting 

For each address, determine the network address, broadcast address, typical usable-host range, and number of usable hosts.

1. `192.168.50.201/25`
2. `10.10.10.99/28`
3. `172.16.18.4/22`

Then decide:

4. Are `192.168.50.20/25` and `192.168.50.140/25` in the same subnet?
5. A department needs **50 usable IPv4 host addresses** in a conventional subnet. Which is the smallest suitable prefix: `/27`, `/26`, or `/25`?

### Part B — Inspect your own configuration 

Use **one** of these read-only commands on a device you control:

**macOS:**

```bash
ifconfig
```

**Windows (Command Prompt):**

```cmd
ipconfig /all
```

**Linux:**

```bash
ip addr
```

Find an active interface and record **locally**:

- Its IPv4 address, if present.
- Its subnet mask or CIDR prefix.
- Whether it has an IPv6 link-local address.

**Privacy note:** Do not post your full interface output publicly. It can include device identifiers, local addresses, and network configuration details. Do not modify your network settings for this exercise.

### Part C — Security decision

Before starting, recall that IP addressing belongs to OSI Layer 3 and IPv6 uses multicast rather than broadcast. Explain both facts in your own words. 

A guest workstation uses `192.168.20.45/24`. A confidential file server uses `192.168.30.10/24`.

- Are they on the same subnet?
- What kind of device normally forwards traffic between them?
- What **additional control** is needed if guests must never reach the file server?

### Part D — Validate calculations with Python

**Tools:** Python 3, terminal; no packages or network access required. Run `python3` (or `py` on Windows), then enter:

```python
import ipaddress
for value in ["192.168.50.201/25", "10.10.10.99/28", "172.16.18.4/22"]:
    net = ipaddress.ip_network(value, strict=False)
    print(net, net.netmask, net.broadcast_address, net.num_addresses)
```

`strict=False` permits a host address as input and derives its network. The printed count includes network and broadcast addresses. For these ordinary IPv4 subnets, subtract two for usable hosts. Compare the output with Part A before revealing answers.

<details markdown="1">
<summary>Reveal Part D expected output</summary>

```text
192.168.50.128/25 255.255.255.128 192.168.50.255 128
10.10.10.96/28 255.255.255.240 10.10.10.111 16
172.16.16.0/22 255.255.252.0 172.16.19.255 1024
```

The usable counts are 126, 14 and 1,022. Python validates arithmetic; it does not tell you whether a real network is securely configured.

</details>

### Practical exercise answer key 

<details markdown="1">
<summary>Reveal practical exercise answers</summary>

| Item | Answer |
|---|---|
| 1 | Network `192.168.50.128`; broadcast `.255`; usable `.129–.254`; **126 hosts** |
| 2 | Network `10.10.10.96`; broadcast `.111`; usable `.97–.110`; **14 hosts** |
| 3 | Network `172.16.16.0`; broadcast `172.16.19.255`; usable `172.16.16.1–172.16.19.254`; **1,022 hosts** |
| 4 | **No.** They are in the `.0/25` and `.128/25` subnets respectively. |
| 5 | **`/26`**, which provides 62 typical usable addresses. |
| Part C | Different `/24` networks; a router or Layer 3 switch forwards between them. Use enforced ACL/firewall policy to block guest-to-server access. |

For Part B, your answers depend on your own device and network.

</details>

## 13. Eight original Security+-style questions {#knowledge-check}

Choose the **BEST** answer before revealing each explanation. These are original practice questions, not actual CompTIA items.

### Q1. Which address belongs to an RFC 1918 private IPv4 block?

A. `172.32.10.5`  
B. `192.169.1.5`  
C. `172.20.10.5`  
D. `169.254.10.5`  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** The private `172.16.0.0/12` block ends at `172.31.255.255`. Review: [Public and private IPv4](#public-private-ipv4).

</details>

### Q2. What subnet mask corresponds to `/26`?

A. `255.255.255.0`  
B. `255.255.255.128`  
C. `255.255.255.192`  
D. `255.255.255.224`  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** `/26` sets 26 mask bits to 1, giving `255.255.255.192`. Review: [CIDR and masks](#cidr-and-masks).

</details>

### Q3. How many typical usable IPv4 host addresses does a `/27` subnet provide?

A. 14  
B. 30  
C. 32  
D. 62  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** `/27` leaves five host bits: 32 total addresses minus network and broadcast = 30. Review: [CIDR and masks](#cidr-and-masks).

</details>

### Q4. What is the network address for `192.168.5.70/26`?

A. `192.168.5.0`  
B. `192.168.5.64`  
C. `192.168.5.70`  
D. `192.168.5.128`  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** `/26` blocks start at `.0`, `.64`, `.128`, and `.192`; `.70` falls in `.64–.127`. Review: [Subnet calculation](#subnet-calculation).

</details>

### Q5. Which IPv6 address is the loopback address?

A. `fe80::1`  
B. `ff02::1`  
C. `::1`  
D. `2001:db8::1`  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** `::1/128` is IPv6 loopback. Review: [IPv6 types](#ipv6-address-types).

</details>

### Q6. A host unexpectedly receives `169.254.25.8`. What should you investigate first?

A. Whether DHCP address assignment failed  
B. Whether its public DNS domain expired  
C. Whether the server's TLS certificate expired  
D. Whether a router changed the destination port  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** An IPv4 link-local address can indicate failure to obtain a DHCP lease. Review: [Special IPv4](#special-ipv4).

</details>

### Q7. A company needs 100 typical usable IPv4 host addresses in one subnet. What is the smallest suitable prefix?

A. `/27`  
B. `/26`  
C. `/25`  
D. `/28`  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** `/25` supports 126 typical usable hosts; `/26` supports only 62. Review: [CIDR and masks](#cidr-and-masks).

</details>

### Q8. Two departments use different IPv4 subnets. Which statement is most accurate?

A. Their traffic is automatically encrypted.  
B. They cannot communicate under any circumstances.  
C. They are isolated without needing security rules.  
D. Routing and enforced security policy determine whether communication is permitted.  

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Subnets identify network boundaries; ACLs, firewalls, and routing policies enforce access. Review: [Segmentation scenario](#segmentation-scenario).

</details>

**Score guide:** 7–8: strong understanding; 5–6: revisit missed concepts; 0–4: redo the guided notes and lab.

## 14. Discussion challenge

A firm separates guest and staff networks using different IPv4 prefixes, but guests can still access a confidential server. Explain why this is possible, propose two enforcement controls, and describe how you would verify the intended policy without scanning unauthorized systems. Consider IPv6 as well as IPv4.

## 15. Study from the Treadmill 🏃

- [Professor Messer — Network Segmentation (SY0-701)](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/network-segmentation-sy0-701/) — connect subnet boundaries with enforced security policy.

**Active-listening prompts:** Why is a subnet not a firewall? Where can inter-network access be enforced? How can segmentation reduce incident impact?

## 16. One-minute recap

### Exam-day quick reference 

| See this clue | Think this |
|---|---|
| `192.168.x.x`, `10.x.x.x`, or `172.16–31.x.x` | RFC 1918 private IPv4 (within exact boundaries) |
| `169.254.x.x` | IPv4 link-local; check address assignment |
| `127.0.0.1` or `::1` | Loopback |
| `fe80::/10` | IPv6 link-local |
| `ff00::/8` | IPv6 multicast |
| `/24` | 256 IPv4 addresses; 254 typical usable hosts |
| `/26` | 64 IPv4 addresses; 62 typical usable hosts |
| `/27` | 32 IPv4 addresses; 30 typical usable hosts |
| `/28` | 16 IPv4 addresses; 14 typical usable hosts |
| Different subnets | Routing needed for ordinary inter-subnet communication |
| Subnets without ACLs/firewall rules | Not necessarily secure isolation |

**Three things to remember:**

1. **A CIDR prefix tells you how many bits belong to the network.** Longer prefixes mean smaller address blocks.
2. **A host's network is determined by the address *and* the prefix.** Do not judge by matching decimal octets alone.
3. **Subnetting organizes networks; security controls enforce segmentation.** Different address ranges are not a security policy.

**Next:**

**Day 18 — Ports and Protocols:** Learn which services use which ports, how TCP and UDP affect communication, and when secure alternatives matter.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
