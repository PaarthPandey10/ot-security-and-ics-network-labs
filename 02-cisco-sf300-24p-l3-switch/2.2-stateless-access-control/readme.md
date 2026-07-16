# 2.2 – L3 Security & Stateless Access Control (ACL) Deployment

*Transitioning the open L3 routing fabric into a segmented Zero-Trust environment using stateless ASIC-level packet filtering.*

---

## Overview

This lab focuses on the implementation of a Unidirectional Isolation Policy between the Trusted Home (VLAN 10) and Untrusted Work (VLAN 20) network boundaries. Unlike stateful firewalls, the Cisco SF300 utilizes stateless IPv4-Based Access Control Lists (ACLs). This project explores the critical pitfalls of stateless filtering—specifically the "Default Action" trap—and validates hardware-level traffic drops via deep packet forensics.

📄 **Full Walkthrough:** For the complete CLI methodology, packet-level hex dumps, and policy binding configurations, see the [Infrastructure Build Guide](./2.2-build-guide.md).

---

## Key Highlights

* **Zero-Trust Architectural Design:** Implemented a unidirectional traffic policy, allowing the Home Laptop to initiate traffic to the Work Laptop while strictly prohibiting the Work Laptop from initiating inbound connections.
* **Host-OS Routing Remediation:** Diagnosed a common Windows kernel conflict where Wi-Fi interface metrics overrode the physical Ethernet connection. Remediated by forcing absolute gateway prioritization to the switch SVI.
* **Stateless ACL Trap Diagnosis:** Identified a catastrophic "Default Action: Deny Any" bug during port binding, which blackholed legitimate asymmetric return traffic (Echo Replies). Remediated by transitioning to a surgical "Permit Any" default with specific ICMP drops.
* **Deep Packet Forensics:** Validated policy enforcement using raw Ethernet hex dump analysis. Confirmed successful transit of Type 0 (Echo Reply) frames while verifying that forbidden Type 8 (Echo Request) initiations were dropped at the ASIC level with Type 3 Code 0 (Destination Unreachable).

---

## Technical Specifications

* **Core L3 Engine:** Cisco SF300-24P (ASIC Filtering: ENABLED)
* **VLAN 10 (Trusted Home):** SVI `192.168.10.1/24` | Host: `192.168.10.150`
* **VLAN 20 (Untrusted Work):** SVI `192.168.20.1/24` | Host: `192.168.20.100`
* **Ingress Boundary (Port FE24):** ACL `isolate-work` bound.
* **Primary Filter Rule:** Priority 10 | Deny ICMP Type 8 | Source: `192.168.20.0/24` | Dest: `192.168.10.0/24`

---

## Contact

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio