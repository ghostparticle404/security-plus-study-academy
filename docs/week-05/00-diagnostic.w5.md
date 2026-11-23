# Week 05 — Network Security: Pre-Instruction Diagnostic

**Security+ Study Academy | CompTIA Security+ SY0-701**

**Purpose:** Identify what you already know before studying Days 21–25. **Why study something you already know?**

**Instructions:** Answer all 20 questions without consulting the lessons. Choose the single BEST answer for each. Each question is worth one point. Record your answers before opening the answer key below. This diagnostic is a screening tool, not proof of mastery.

**Coverage:** Four questions per lesson (20 points total). The answer key follows the questions.

## Questions 1–4: Day 21 — Segmentation and VLANs

### Question 1

An office moves guest laptops to a different IP subnet but leaves them on the same switch VLAN. Which statement is MOST accurate?

A. The change automatically blocks Layer 2 broadcasts between groups  
B. The change creates an encrypted trust boundary  
C. The address change alone does not create separate Layer 2 VLANs or enforce isolation  
D. The change disables all inter-subnet routing  

### Question 2

A switch link must carry VLAN 10 and VLAN 40 to a VLAN-aware firewall. Which configuration is MOST appropriate?

A. An 802.1Q trunk permitting only the required VLANs  
B. An access port assigned to VLAN 10  
C. An unmanaged hub  
D. A port configured to discard all tagged frames  

### Question 3

Employees need to print to printers in another VLAN, but guests must not access printers. What should the administrator implement?

A. Place every device in the same VLAN  
B. Disable the printers’ IP addresses  
C. Allow every VLAN to communicate freely  
D. Use inter-VLAN routing with narrowly scoped firewall or ACL rules  

### Question 4

An attacker compromises a camera in the IoT VLAN. Which measure BEST reduces access to finance systems?

A. Rename the camera VLAN  
B. Deny IoT-to-finance traffic at the trust boundary  
C. Give cameras public IP addresses  
D. Increase the size of the IoT subnet  

## Questions 5–8: Day 22 — Firewalls and Filtering

### Question 5

A firewall allows an outbound TCP connection and automatically permits matching response packets. Which capability is being used?

A. DNS filtering  
B. Stateless packet matching only  
C. Stateful connection tracking  
D. Port mirroring  

### Question 6

A firewall evaluates rules from top to bottom using first-match processing. Rule 1 allows all traffic from the guest subnet; Rule 2 denies guest access to finance. What happens?

A. The broad allow can match first, preventing Rule 2 from enforcing the intended denial  
B. Rule 2 always overrides Rule 1  
C. Both rules must match before traffic is forwarded  
D. The firewall automatically reorders the rules by specificity  

### Question 7

A company needs application-aware filtering and integrated intrusion prevention. Which firewall category is the BEST fit?

A. Basic stateless ACL  
B. Unmanaged Ethernet switch  
C. Passive network tap  
D. Next-generation firewall (NGFW)  

### Question 8

A policy says unapproved inter-zone connections must be blocked unless explicitly authorized. Which baseline rule best supports this?

A. Permit all outbound traffic from every zone  
B. Default deny with documented least-privilege exceptions  
C. Allow traffic based solely on source port  
D. Disable firewall logging  

## Questions 9–12: Day 23 — IDS, IPS, and Network Monitoring

### Question 9

A security sensor must automatically block known malicious traffic as packets pass through it. Which deployment BEST meets the requirement?

A. Passive NIDS  
B. Inline NIPS  
C. Read-only flow collector  
D. Log archive  

### Question 10

An analyst needs to inspect the payload of an authorized captured connection, not merely source/destination and byte counts. Which telemetry is MOST useful?

A. NetFlow summary  
B. Interface utilization counter  
C. DNS query count  
D. Packet capture  

### Question 11

An IDS flags a connection as malicious, but investigation confirms legitimate approved activity. How should this alert be classified?

A. False positive  
B. True positive  
C. False negative  
D. Confirmed compromise  

### Question 12

An alert shows a high severity score but comes from an isolated test device with no sensitive access. What should the analyst do NEXT?

A. Automatically declare a critical incident  
B. Ignore the alert without review  
C. Assess asset context, supporting telemetry, and business impact to set priority  
D. Delete the detection rule  

## Questions 13–16: Day 24 — VPNs and Secure Connectivity

### Question 13

A remote employee connects from home to an organization’s internal resources using an encrypted tunnel. Which VPN architecture is this?

A. Site-to-site VPN  
B. Layer 2 trunk  
C. Remote-access VPN  
D. Open guest WLAN  

### Question 14

Which IPsec component is primarily used to negotiate security associations and keying material?

A. ESP  
B. IKEv2  
C. 802.1Q  
D. RADIUS accounting  

### Question 15

A company routes ALL remote-user internet traffic through its corporate VPN gateway for centralized inspection. Which model is in use?

A. Split tunneling  
B. SSID isolation  
C. Port mirroring  
D. Full tunneling  

### Question 16

A VPN authenticates a contractor successfully, but the contractor must reach only one approved application. What additional control is MOST important?

A. Resource-specific authorization and least-privilege access rules  
B. A longer Wi-Fi SSID  
C. Disabling VPN logs  
D. Allowing access to every internal subnet  

## Questions 17–20: Day 25 — Wireless and Network Access Control

### Question 17

An organization wants individual employee authentication on corporate Wi-Fi using certificates. Which combination is MOST appropriate?

A. WPA2-Personal with shared passphrase  
B. Captive portal only  
C. SSID hiding with MAC filtering  
D. WPA-Enterprise with 802.1X and EAP-TLS  

### Question 18

A guest network uses a captive portal. Which statement is MOST accurate?

A. The portal automatically encrypts all wireless frames  
B. The portal replaces the need for network isolation  
C. The portal may manage guest access but does not itself provide Wi-Fi link encryption  
D. The portal guarantees protection from evil-twin APs  

### Question 19

A managed laptop authenticates successfully but fails the organization’s required security posture check. Which NAC action BEST supports policy?

A. Quarantine or limit access until remediation  
B. Give it unrestricted production access  
C. Disable device posture checks permanently  
D. Move all other laptops to the guest VLAN  

### Question 20

A malicious actor sets up an AP that imitates the organization’s legitimate wireless network to lure users. What is this BEST described as?

A. An 802.1Q trunk  
B. An evil twin  
C. A stateful firewall  
D. A VPN concentrator  

## Diagnostic scorecard

| Lesson / topic | Questions | Correct / 4 | Percentage | Recommended study action |
|---|---|---|---|---|
| Day 21: Segmentation and VLANs | 1–4 | ___ / 4 | ___% | __________________ |
| Day 22: Firewalls and Filtering | 5–8 | ___ / 4 | ___% | __________________ |
| Day 23: IDS, IPS, and Network Monitoring | 9–12 | ___ / 4 | ___% | __________________ |
| Day 24: VPNs and Secure Connectivity | 13–16 | ___ / 4 | ___% | __________________ |
| Day 25: Wireless and Network Access Control | 17–20 | ___ / 4 | ___% | __________________ |

**Topic-level interpretation:** 4/4 (100%): strong baseline—review briefly or proceed; 3/4 (75%): mostly understood—review missed concepts; 2/4 (50%): partial understanding—study the lesson; 0–1/4 (0–25%): priority study—complete the lesson carefully. Four questions per topic provide a screening result, not a definitive measure of mastery.

**Total diagnostic score:** ___ / 20 (___%).

## Answer key and explanations

Complete the diagnostic before reading this section.

### Day 21 — Segmentation and VLANs

**1. C — The address change alone does not create separate Layer 2 VLANs or enforce isolation.** Changing IP addressing does not itself create a new VLAN or enforce traffic policy. See [Day 21: Segmentation and VLANs — relevant section](day-21-segmentation-and-vlans.md#2-segmentation-methods-what-is-actually-being-separated).

**2. A — An 802.1Q trunk permitting only the required VLANs.** An 802.1Q trunk carries multiple VLANs; limiting allowed VLANs reduces unnecessary exposure. See [Day 21: Segmentation and VLANs — relevant section](day-21-segmentation-and-vlans.md#trunk-ports).

**3. D — Use inter-VLAN routing with narrowly scoped firewall or ACL rules.** Layer 3 forwarding enables the required path, while explicit filtering restricts who can use it. See [Day 21: Segmentation and VLANs — relevant section](day-21-segmentation-and-vlans.md#4-how-traffic-crosses-vlans).

**4. B — Deny IoT-to-finance traffic at the trust boundary.** An enforced deny policy between zones limits lateral movement regardless of VLAN names or subnet size. See [Day 21: Segmentation and VLANs — relevant section](day-21-segmentation-and-vlans.md#1-start-with-the-trust-boundary).

### Day 22 — Firewalls and Filtering

**5. C — Stateful connection tracking.** A stateful firewall tracks connection state to recognize permitted return traffic. See [Day 22: Firewalls and Filtering — relevant section](day-22-firewalls-and-filtering.md#3-stateful-inspection-and-connection-tracking).

**6. A — The broad allow can match first, preventing Rule 2 from enforcing the intended denial.** In a first-match policy, an earlier broad allow can shadow a later deny. See [Day 22: Firewalls and Filtering — relevant section](day-22-firewalls-and-filtering.md#shadowed-rules-and-overly-broad-allows).

**7. D — Next-generation firewall (NGFW).** NGFWs commonly provide application awareness and integrated security inspection beyond basic packet fields. See [Day 22: Firewalls and Filtering — relevant section](day-22-firewalls-and-filtering.md#4-next-generation-firewalls-ngfws).

**8. B — Default deny with documented least-privilege exceptions.** Default deny ensures only expressly authorized flows are permitted. See [Day 22: Firewalls and Filtering — relevant section](day-22-firewalls-and-filtering.md#7-default-deny-least-privilege-and-rule-hygiene).

### Day 23 — IDS, IPS, and Network Monitoring

**9. B — Inline NIPS.** An inline network IPS can inspect and block traffic; a passive IDS generally alerts rather than blocks. See [Day 23: IDS, IPS, and Network Monitoring — relevant section](day-23-ids-ips-and-network-monitoring.md#passive-versus-inline-placement).

**10. D — Packet capture.** Packet captures can preserve packet-level detail, while flow records generally summarize communications. See [Day 23: IDS, IPS, and Network Monitoring — relevant section](day-23-ids-ips-and-network-monitoring.md#packet-capture-versus-flow-records).

**11. A — False positive.** A false positive is an alert for activity that is not actually malicious. See [Day 23: IDS, IPS, and Network Monitoring — relevant section](day-23-ids-ips-and-network-monitoring.md#7-false-positives-false-negatives-and-tuning).

**12. C — Assess asset context, supporting telemetry, and business impact to set priority.** Alert severity is not the same as investigation priority; contextual validation matters. See [Day 23: IDS, IPS, and Network Monitoring — relevant section](day-23-ids-ips-and-network-monitoring.md#severity-versus-priority).

### Day 24 — VPNs and Secure Connectivity

**13. C — Remote-access VPN.** Remote-access VPNs connect individual users or devices to organization resources. See [Day 24: VPNs and Secure Connectivity — relevant section](day-24-vpns-and-secure-connectivity.md#remote-access-vpn-one-user-or-device-connects).

**14. B — IKEv2.** IKEv2 negotiates IPsec security associations; ESP protects traffic data. See [Day 24: VPNs and Secure Connectivity — relevant section](day-24-vpns-and-secure-connectivity.md#esp-ah-and-ikev2-different-jobs).

**15. D — Full tunneling.** Full tunneling routes all configured client traffic through the VPN rather than sending ordinary internet traffic directly. See [Day 24: VPNs and Secure Connectivity — relevant section](day-24-vpns-and-secure-connectivity.md#full-tunnel-configuration).

**16. A — Resource-specific authorization and least-privilege access rules.** Successful tunnel authentication does not grant unrestricted authorization; policy must constrain resource access. See [Day 24: VPNs and Secure Connectivity — relevant section](day-24-vpns-and-secure-connectivity.md#6-vpns-zero-trust-and-least-privilege).

### Day 25 — Wireless and Network Access Control

**17. D — WPA-Enterprise with 802.1X and EAP-TLS.** Enterprise 802.1X with EAP-TLS supports certificate-based authentication rather than a shared Wi-Fi password. See [Day 25: Wireless and Network Access Control — relevant section](day-25-wireless-and-network-access-control.md#eap-tls-and-certificates).

**18. C — The portal may manage guest access but does not itself provide Wi-Fi link encryption.** Captive portals manage access workflows; wireless encryption and segmentation are separate controls. See [Day 25: Wireless and Network Access Control — relevant section](day-25-wireless-and-network-access-control.md#what-a-captive-portal-doesand-does-not-do).

**19. A — Quarantine or limit access until remediation.** NAC can enforce restricted or remediation access when a device fails posture requirements. See [Day 25: Wireless and Network Access Control — relevant section](day-25-wireless-and-network-access-control.md#4-network-access-control-nac-identity-plus-device-posture).

**20. B — An evil twin.** An evil twin impersonates a legitimate wireless network to attract client connections. See [Day 25: Wireless and Network Access Control — relevant section](day-25-wireless-and-network-access-control.md#rogue-ap-versus-evil-twin).

## How to use your results

1. Score each four-question topic separately; use the scorecard to identify priority lessons.
2. Study 0–1/4 topics carefully first; study 2/4 topics next; review missed concepts for 3/4 topics.
3. For 4/4 topics, briefly review the lesson and test yourself on a practical scenario rather than assuming complete mastery.
4. After completing the five lessons, take the separate Week 05 post-instruction knowledge check. Compare only its 20 weekly questions with this 20-question diagnostic; report the five cumulative retention questions separately.

---

© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).  
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
