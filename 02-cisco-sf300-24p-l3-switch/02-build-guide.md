# Infrastructure Build Guide: Cisco SF300 L3 Multilayer Architecture

> A visual narrative documenting the bare-metal provisioning, control plane debugging, and definitive hardware routing validation of a legacy Cisco multilayer switch.

---

## Overview

This guide provides a comprehensive breakdown of configuring a Cisco SF300-24P switch in a pure Layer 3 architecture. It details the transition from initial hardware reset to overcoming legacy OpenSSH deprecation, deploying Out-of-Band (OOB) serial management, resolving firmware SVI Autostate logic, and securing Host-OS firewall rules to validate true hardware-accelerated Inter-VLAN routing.

---

## Workflow Steps

### 1. Act I & II: Initialization and The OpenSSH Deprecation Trifecta
The objective was to deploy a professional-grade L3 routing environment from scratch. We initiated a factory reset, purged the NVRAM, and forced the Cisco SF300 into Layer 3 system mode. Baseline hardening commenced: hostname configuration (`cisco-lab-01`), time synchronization, and the establishment of a Priv 15 local admin account (`paarth`) while nuking the default `admin`.

**Physical Infrastructure & Out-of-Band Setup:**
![](../screenshots/CiscoSW1_Page_01.png)

**System Mode Pivot & Hostname Definition:**
![](../screenshots/CiscoSW1_Page_02.png)

**System RTC Time Synchronization:**
![](../screenshots/CiscoSW1_Page_03.png)

**Establishing Priv 15 User Credentials:**
![](../screenshots/CiscoSW1_Page_04.png)
![](../screenshots/CiscoSW1_Page_05.png)

Attempting to establish an SSH session from a modern Windows PowerShell environment triggered an immediate rejection. The SF300 relies on 2010-era cryptography, which OpenSSH has aggressively deprecated. We engaged in a systematic, four-stage cryptographic bypass using CLI override flags, sequentially forcing the terminal to accept legacy Key Exchanges (`diffie-hellman-group1-sha1`), Host Keys (`ssh-rsa`), Ciphers (`aes256-cbc`), and finally, Message Authentication Codes (`hmac-sha1`). We successfully reclaimed the remote management plane.

### 2. Act III: Control Plane Exhaustion and the OOB Pivot
With SSH established, we executed CLI provisioning for VLAN 10 and 20. Upon binding the first SVI IP (`192.168.10.1`), the switch suffered a catastrophic control plane failure resulting in a `client_loop: send disconnect: Connection reset`. The underpowered CPU could not simultaneously process the heavy AES-256 cryptographic SSH overhead and the ASIC routing table interrupts. The switch triggered a watchdog timeout, kernel panicked, and rebooted (drifting the RTC clock back to 2014).

**The Pivot:** We abandoned SSH entirely for provisioning. You procured a physical DB9-to-RJ45 Cisco Serial Cable and a USB adapter, located the OS COM port map via Device Manager, and established a zero-crypto Out-of-Band (OOB) console session via PuTTY at 115200 baud. This bypassed the CPU bottleneck entirely, allowing the routing engine config to be injected safely. Basic connectivity was restored and verified.

**Restored Management Plane Connectivity Verification:**
![](../screenshots/CiscoSW1_Page_07.png)

### 3. Act IV: SVI Autostate and the GUI "Ghost" Bug
Returning to the Web UI for port assignments, you executed a brilliant piece of physical reverse-engineering. You observed that the VLAN 10 SVI refused to transition to an "UP" state in the routing table despite configuration. By manipulating the physical topology (plugging DP into `gi19`), you deduced the Cisco SVI Autostate mechanism—the virtual routing interface remains dormant until physical Layer 1/2 electrical signals are detected on an assigned access port.

**VLAN Database Activation Status:**
![](../screenshots/CiscoSW1_Page_08.png)

**Port-to-VLAN Membership Mapping:**
![](../screenshots/CiscoSW1_Page_09.png)

Simultaneously, we discovered a fatal SbOS firmware bug: provisioning a new IPv4 interface occasionally flushes the default VLAN 1 management IP (`192.168.1.254`). You diagnosed the lockout, dual-homed your access, and manually restored the gateway to stabilize the control plane. With L2 established and physical links hot, we verified the Switch Virtual Interfaces (SVIs).

**SVI Gateway Provisioning Status:**
![](../screenshots/CiscoSW1_Page_10.png)

The ultimate proof of the control plane configuration was verifying the switch's internal brain. The Routing Information Base (RIB) successfully processed the SVIs and injected the subnets as "Directly Connected" routes.

**ASIC Routing Table (RIB) Verification:**
![](../screenshots/CiscoSW1_Page_11.png)

### 4. Act V: Policy Walls and the Multi-Homed Probe
With routing tables showing "Directly Connected," we attempted Inter-VLAN ICMP verification. Traffic failed. `tracert` revealed the packets were leaking out to the ISP due to conflicting Wi-Fi/Ethernet metrics. After isolating the adapters, ICMP still failed due to the Work Laptop (WL) designating the subnet as a "Public" network, dropping cross-subnet ICMP requests. 

**Initial ICMP Firewall Block / Rule Inspection:**
![](../screenshots/CiscoSW1_Page_06.png)

Lacking Admin permissions on the WL to modify the Windows Defender Firewall scope, you initiated a high-level lateral pivot: The Multi-Homed Probe. You connected both `gi19` (VLAN 10 via Ethernet) and `gi24` (VLAN 20 via Type-C to LAN adapter) to a single machine (DP). You configured static IPs but deliberately left the VLAN 20 gateway blank to prevent routing table poisoning.

**Multi-Homed Probe Interface Configuration:**
![](../screenshots/CiscoSW1_Page_12.png)

Once isolated, we successfully expanded the ICMPv4 Inbound Rule Scope on the primary machine to explicitly trust the `192.168.20.0/24` block, remediating the OS firewall barrier.

**Windows Firewall Subnet Scope Remediation:**
![](../screenshots/CiscoSW1_Page_13.png)

### 5. Act VI: The Weak Host Model and Final ASIC Validation
Initial tests of the multi-homed setup yielded a 1-hop `<1ms` traceroute. We diagnosed this as the **Windows "Weak Host" Model**—the OS kernel was acting as the router, identifying both subnets as "On-link" and looping the traffic internally via CPU memory, completely bypassing the switch.

To achieve true physical validation, we reverted to a distinct two-host physical topology, utilizing the Work Laptop strictly as a destination endpoint. The final execution of `tracert 192.168.20.100` from the Home Laptop produced the definitive proof of concept:

* **Hop 1:** `192.168.10.1` (Traffic hit the Cisco Switch ASIC Gateway).
* **Hop 2:** `192.168.20.100` (Traffic was successfully routed across the backplane to the isolated broadcast domain).

**The Definitive 2-Hop Hardware Trace Validation:**
![](../screenshots/CiscoSW1_Page_14.png)

---

## Contact

For any questions or feedback, reach out:  
**Paarth Pandey** [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: [Paarth Pandey](https://github.com)
>   
> OT Security Labs: Industrial Network Defense
