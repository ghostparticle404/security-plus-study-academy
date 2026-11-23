# Week 05 — Network Security: Post-Instruction Knowledge Check

**Security+ Study Academy | CompTIA Security+ SY0-701**

**Purpose:** Assess what you learned from Days 21–25 and revisit foundational network concepts. Complete the Week 05 diagnostic first, study the lessons, then take this assessment without notes.

**Instructions:** Choose the single BEST answer for each question. Questions 1–20 measure Week 05 learning (20 points). Questions 21–25 measure cumulative retention (5 points); score these separately. Each question is worth one point. Do not open the answer key until finished.

## Questions 1–4: Day 21 — Segmentation and VLANs

### Question 1

A clinic separates its medical devices and visitor laptops into VLANs. What must also be configured to prevent visitors from reaching medical devices?

A. Distinct SSID names only  
B. Inter-VLAN filtering that denies guest-to-medical traffic  
C. Larger DHCP pools  
D. A shared switch management password  

### Question 2

An administrator connects an ordinary desktop to a switch port intended for one VLAN. Which port mode is appropriate?

A. 802.1Q trunk  
B. SPAN destination  
C. Access port  
D. Routed WAN interface  

### Question 3

A building switch needs to transport several VLANs to another managed switch. Which configuration is BEST?

A. A trunk with an explicit allowed-VLAN list  
B. An access port for each VLAN on the same cable  
C. An unmanaged repeater  
D. Disable VLAN tagging on both ends  

### Question 4

A compromised conference-room device attempts to contact payroll servers. Which architecture most directly limits the damage?

A. Place payroll on the same VLAN  
B. Change the device hostname  
C. Allow all east–west traffic  
D. Separate trust zones and deny unnecessary inter-zone access  

## Questions 5–8: Day 22 — Firewalls and Filtering

### Question 5

A firewall permits a workstation to initiate an HTTPS session and recognizes its return packets without a separate inbound allow rule. What explains this?

A. Stateful inspection  
B. Static DNS  
C. Port mirroring  
D. A VLAN trunk  

### Question 6

A first-match firewall has an early allow-any rule for a subnet followed by a deny for its access to servers. What is the primary problem?

A. The deny is evaluated first  
B. The rules are automatically merged  
C. The early allow shadows the later deny  
D. All TCP traffic is encrypted  

### Question 7

A security team needs to recognize applications and block exploit attempts at the firewall. Which solution is most suitable?

A. A passive network tap  
B. An NGFW with integrated prevention  
C. A basic unmanaged switch  
D. A DHCP relay  

### Question 8

A new internal network should be reachable only by explicitly approved services. Which approach is strongest?

A. Allow everything until an incident  
B. Trust traffic from private IP addresses  
C. Disable policy logging  
D. Default deny plus narrow, documented allow rules  

## Questions 9–12: Day 23 — IDS, IPS, and Network Monitoring

### Question 9

A sensor must stop matching malicious packets before they reach their target. Which deployment is appropriate?

A. A passive IDS listening to mirrored traffic  
B. An offline log archive  
C. An inline IPS  
D. A flow-only dashboard  

### Question 10

An investigator needs to examine packet-level content from an authorized, unencrypted lab exchange. Which source is most useful?

A. A packet capture  
B. A flow summary only  
C. An interface uptime counter  
D. A device inventory list  

### Question 11

A detection engine reports malware, but a validated investigation shows routine authorized traffic. How is the detection categorized?

A. True negative  
B. False positive  
C. False negative  
D. Confirmed intrusion  

### Question 12

Two identical IDS alerts occur on a production database and a disposable lab host. What should drive triage priority?

A. The alert ID alone  
B. Alphabetical order of hostnames  
C. Only the packet size  
D. Asset criticality, context, and corroborating evidence  

## Questions 13–16: Day 24 — VPNs and Secure Connectivity

### Question 13

A traveling employee needs an encrypted connection to approved internal services from a hotel. Which architecture fits?

A. Site-to-site between offices  
B. Remote-access VPN  
C. VLAN access port  
D. Public guest SSID  

### Question 14

During an IPsec setup, which component establishes security associations and negotiates keys?

A. ESP  
B. 802.1Q  
C. IKEv2  
D. DHCP  

### Question 15

An enterprise wants a remote laptop’s general internet browsing routed through its corporate security gateway. Which setting supports this?

A. Full tunneling  
B. Split tunneling  
C. MAC filtering  
D. Port mirroring  

### Question 16

A vendor connects successfully to the VPN but must be restricted to one ticketing system. Which control enforces this?

A. A hidden SSID  
B. Unrestricted subnet routing  
C. Longer DHCP leases  
D. Application-specific authorization and least-privilege policy  

## Questions 17–20: Day 25 — Wireless and Network Access Control

### Question 17

A business wants unique certificate-based employee Wi-Fi authentication instead of a shared password. Which approach is best?

A. WPA2-Personal  
B. Captive portal alone  
C. WPA-Enterprise with 802.1X/EAP-TLS  
D. SSID hiding  

### Question 18

A coffee-shop guest WLAN shows a login webpage. What does that page NOT establish on its own?

A. A guest-use policy notice  
B. Wireless link-layer encryption  
C. An access workflow  
D. An acceptable-use acknowledgement  

### Question 19

An employee laptop presents valid credentials but fails required patch checks. Which NAC response is appropriate?

A. Unrestricted access to all VLANs  
B. Disable the posture requirement  
C. Place finance servers on guest Wi-Fi  
D. Restricted remediation or quarantine access  

### Question 20

A fake AP advertises the same network name as a company WLAN to attract employees. What is the threat?

A. Evil twin  
B. 802.1X authenticator  
C. Stateful firewall  
D. Site-to-site VPN  

## Questions 21–25: Cumulative retention and integration

These questions revisit earlier foundations such as IP routing, TCP sessions, ports, and DHCP through Week 05 network-security applications.

### Question 21

A laptop in VLAN 10 needs to reach a server in VLAN 30. Which device function is required to forward traffic between the IP networks?

A. A pure Layer 2 hub  
B. Layer 3 routing  
C. SSID broadcasting  
D. An IDS signature  

### Question 22

A technician is diagnosing a permitted outbound TCP session whose reply traffic is accepted. Which concept best explains the firewall decision?

A. 802.1Q tags  
B. A DHCP lease  
C. Connection state associated with the session  
D. DNS caching  

### Question 23

A network monitor records a destination IP, ports, and total bytes but no full packet contents. What type of record is most likely?

A. Flow telemetry  
B. Full packet capture  
C. A WPA3 handshake transcript  
D. An EAP-TLS certificate  

### Question 24

A remote user can reach an internal service through a VPN but cannot reach an unauthorized subnet. Which explanation fits best?

A. Encryption guarantees unrestricted access  
B. VLAN names control all permissions  
C. DNS removes the need for access rules  
D. Routing and authorization rules restrict the allowed destination  

### Question 25

Guests receive valid IP addresses and internet connectivity but cannot reach internal file shares. Which result best demonstrates a secure design?

A. DHCP has been disabled  
B. Guest isolation and inter-zone restrictions are working  
C. The guest SSID is hidden  
D. Every VLAN is bridged together  

## Scoring

| Measure | Questions | Score |
|---|---|---|
| Week 05 diagnostic baseline | Pre-instruction diagnostic, 1–20 | ___ / 20 |
| Week 05 post-check | 1–20 | ___ / 20 |
| Cumulative retention | 21–25 | ___ / 5 |
| Combined post-check | 1–25 | ___ / 25 |

**Weekly improvement:** Post-check correct − diagnostic correct = ___ questions. Percentage-point change = (post-check % − diagnostic %) = ___ percentage points. Do **not** include cumulative questions when measuring Week 05 improvement.

## Topic-level progress scorecard

| Topic | Diagnostic / 4 | Post-check / 4 | Percentage-point change | Recommended action |
|---|---:|---:|---:|---|
| Day 21: Segmentation and VLANs | ___ | ___ | ___ pp | __________________ |
| Day 22: Firewalls and Filtering | ___ | ___ | ___ pp | __________________ |
| Day 23: IDS, IPS, and Network Monitoring | ___ | ___ | ___ pp | __________________ |
| Day 24: VPNs and Secure Connectivity | ___ | ___ | ___ pp | __________________ |
| Day 25: Wireless and Network Access Control | ___ | ___ | ___ pp | __________________ |

**How to interpret each post-check topic score:** 4/4: advance and revisit with spaced practice; 3/4: review the missed concept; 2/4: revisit the lesson and practice applying it; 0–1/4: repeat the lesson and reassess using new questions.

A four-question topic score is a useful indicator, not definitive proof of mastery or exam readiness.

## Answer key, explanations, and remediation

Open this section only after completing all 25 questions. Each link points to an actual heading in a completed Week 05 lesson.

### Day 21 — Segmentation and VLANs

**1. B — Inter-VLAN filtering that denies guest-to-medical traffic.** A VLAN organizes Layer 2 traffic; enforce cross-zone policy at a routed boundary.
**Learning objective:** Enforce trust boundaries. **Review:** [Day 21 — 4. How traffic crosses VLANs](day-21-segmentation-and-vlans.md#4-how-traffic-crosses-vlans).

**2. C — Access port.** An access port places ordinary endpoint traffic into its assigned VLAN.
**Learning objective:** Choose access and trunk ports. **Review:** [Day 21 — Access ports](day-21-segmentation-and-vlans.md#access-ports).

**3. A — A trunk with an explicit allowed-VLAN list.** An 802.1Q trunk carries tagged traffic for multiple VLANs; restrict the allowed list.
**Learning objective:** Configure a limited VLAN trunk. **Review:** [Day 21 — Trunk ports](day-21-segmentation-and-vlans.md#trunk-ports).

**4. D — Separate trust zones and deny unnecessary inter-zone access.** Restrictive policies between trust zones reduce lateral movement.
**Learning objective:** Reduce lateral movement. **Review:** [Day 21 — 1. Start with the trust boundary](day-21-segmentation-and-vlans.md#1-start-with-the-trust-boundary).

### Day 22 — Firewalls and Filtering

**5. A — Stateful inspection.** Connection tracking recognizes return packets associated with an established permitted session.
**Learning objective:** Distinguish stateful filtering. **Review:** [Day 22 — 3. Stateful inspection and connection tracking](day-22-firewalls-and-filtering.md#3-stateful-inspection-and-connection-tracking).

**6. C — The early allow shadows the later deny.** First-match processing can make later restrictions ineffective.
**Learning objective:** Evaluate rule order. **Review:** [Day 22 — Shadowed rules and overly broad allows](day-22-firewalls-and-filtering.md#shadowed-rules-and-overly-broad-allows).

**7. B — An NGFW with integrated prevention.** NGFWs can incorporate application-aware inspection and IPS functions.
**Learning objective:** Compare firewall types. **Review:** [Day 22 — 4. Next-generation firewalls (NGFWs)](day-22-firewalls-and-filtering.md#4-next-generation-firewalls-ngfws).

**8. D — Default deny plus narrow, documented allow rules.** Default deny establishes a least-privilege baseline.
**Learning objective:** Apply default deny. **Review:** [Day 22 — 7. Default deny, least privilege, and rule hygiene](day-22-firewalls-and-filtering.md#7-default-deny-least-privilege-and-rule-hygiene).

### Day 23 — IDS, IPS, and Network Monitoring

**9. C — An inline IPS.** An inline IPS is positioned to block traffic in the path.
**Learning objective:** Distinguish detection from prevention. **Review:** [Day 23 — Passive versus inline placement](day-23-ids-ips-and-network-monitoring.md#passive-versus-inline-placement).

**10. A — A packet capture.** Packet capture offers packet-level evidence, whereas flow records summarize conversations.
**Learning objective:** Choose appropriate telemetry. **Review:** [Day 23 — Packet capture versus flow records](day-23-ids-ips-and-network-monitoring.md#packet-capture-versus-flow-records).

**11. B — False positive.** A false positive is a malicious alert for benign activity.
**Learning objective:** Classify detection outcomes. **Review:** [Day 23 — 7. False positives, false negatives, and tuning](day-23-ids-ips-and-network-monitoring.md#7-false-positives-false-negatives-and-tuning).

**12. D — Asset criticality, context, and corroborating evidence.** Severity alone does not determine operational priority.
**Learning objective:** Interpret alerts in context. **Review:** [Day 23 — Severity versus priority](day-23-ids-ips-and-network-monitoring.md#severity-versus-priority).

### Day 24 — VPNs and Secure Connectivity

**13. B — Remote-access VPN.** Remote-access VPNs connect individual users or endpoints.
**Learning objective:** Select VPN architecture. **Review:** [Day 24 — Remote-access VPN: one user or device connects](day-24-vpns-and-secure-connectivity.md#remote-access-vpn-one-user-or-device-connects).

**14. C — IKEv2.** IKEv2 negotiates SAs and keying; ESP protects payload traffic.
**Learning objective:** Distinguish IPsec components. **Review:** [Day 24 — ESP, AH, and IKEv2: different jobs](day-24-vpns-and-secure-connectivity.md#esp-ah-and-ikev2-different-jobs).

**15. A — Full tunneling.** A full tunnel directs client traffic through the VPN gateway for central policy.
**Learning objective:** Compare tunnel routing choices. **Review:** [Day 24 — Full-tunnel configuration](day-24-vpns-and-secure-connectivity.md#full-tunnel-configuration).

**16. D — Application-specific authorization and least-privilege policy.** VPN authentication alone does not authorize all internal resources.
**Learning objective:** Separate authentication from authorization. **Review:** [Day 24 — 6. VPNs, Zero Trust, and least privilege](day-24-vpns-and-secure-connectivity.md#6-vpns-zero-trust-and-least-privilege).

### Day 25 — Wireless and Network Access Control

**17. C — WPA-Enterprise with 802.1X/EAP-TLS.** 802.1X and EAP-TLS provide enterprise certificate-based authentication.
**Learning objective:** Choose enterprise authentication. **Review:** [Day 25 — EAP-TLS and certificates](day-25-wireless-and-network-access-control.md#eap-tls-and-certificates).

**18. B — Wireless link-layer encryption.** Captive portals do not themselves encrypt Wi-Fi frames.
**Learning objective:** Distinguish portals from encryption. **Review:** [Day 25 — What a captive portal does—and does not do](day-25-wireless-and-network-access-control.md#what-a-captive-portal-doesand-does-not-do).

**19. D — Restricted remediation or quarantine access.** NAC can limit access until posture is remediated.
**Learning objective:** Apply NAC posture enforcement. **Review:** [Day 25 — 4. Network access control (NAC): identity plus device posture](day-25-wireless-and-network-access-control.md#4-network-access-control-nac-identity-plus-device-posture).

**20. A — Evil twin.** An evil twin imitates a legitimate WLAN to lure connections.
**Learning objective:** Identify wireless impersonation. **Review:** [Day 25 — Rogue AP versus evil twin](day-25-wireless-and-network-access-control.md#rogue-ap-versus-evil-twin).

### Cumulative retention — integrated foundations

**21. B — Layer 3 routing.** Different IP networks require routing; cross-zone filtering can then enforce policy.
**Learning objective:** Integrate IP addressing and routing foundations. **Review:** [Day 21 — 4. How traffic crosses VLANs](day-21-segmentation-and-vlans.md#4-how-traffic-crosses-vlans).

**22. C — Connection state associated with the session.** Stateful inspection tracks a TCP session rather than relying only on isolated packet fields.
**Learning objective:** Apply TCP session fundamentals. **Review:** [Day 22 — 3. Stateful inspection and connection tracking](day-22-firewalls-and-filtering.md#3-stateful-inspection-and-connection-tracking).

**23. A — Flow telemetry.** Flow data summarizes communication metadata rather than storing every packet.
**Learning objective:** Connect ports and network monitoring. **Review:** [Day 23 — Packet capture versus flow records](day-23-ids-ips-and-network-monitoring.md#packet-capture-versus-flow-records).

**24. D — Routing and authorization rules restrict the allowed destination.** Encrypted transport and resource authorization are separate controls.
**Learning objective:** Integrate secure transport and access policy. **Review:** [Day 24 — 6. VPNs, Zero Trust, and least privilege](day-24-vpns-and-secure-connectivity.md#6-vpns-zero-trust-and-least-privilege).

**25. B — Guest isolation and inter-zone restrictions are working.** Guest addressing and internet access can coexist with isolation from internal resources.
**Learning objective:** Integrate DHCP, VLANs, and guest security. **Review:** [Day 25 — Step 5 — Test isolation instead of assuming it works](day-25-wireless-and-network-access-control.md#step-5--test-isolation-instead-of-assuming-it-works).

## Diagnostic-to-post-check comparison

1. Enter the Week 05 diagnostic score out of 20 and the post-check score for questions 1–20.
2. Subtract the diagnostic score from the post-check score to calculate the change in correct answers.
3. Convert both scores to percentages (score ÷ 20 × 100) and subtract to calculate percentage-point change.
4. Compare each four-question topic pair and revisit any lesson with missed concepts.
5. Record questions 21–25 as a **separate** retention score out of five. Review those foundational concepts without treating this score as Week 05 improvement.

**Example:** Diagnostic 12/20 (60%) → post-check 17/20 (85%) = +5 correct answers and +25 percentage points. A cumulative score of 3/5 (60%) is reported separately.

## Suggested next steps

- For every missed question, follow its linked lesson heading and explain the concept in your own words.
- Rework the small-office VLAN, firewall, monitoring, VPN, and guest-Wi-Fi design exercises as one integrated network.
- Reassess weak areas using new scenarios rather than memorizing this answer key.
- Retain your diagnostic and post-check scorecards for later spaced review.

**Source note:** The available files include the five Week 05 lessons and the Week 05 diagnostic, but not the full earlier-week lesson files. Consequently, cumulative items use foundational topics in integrated Week 05 scenarios, and their remediation links point to verified Week 05 headings rather than inventing earlier-week anchors.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
