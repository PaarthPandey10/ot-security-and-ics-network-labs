# 2.4 – Anti-Spoofing Fabric (DHCP Snooping, IPSG, DAI)

*Enforcing wire-speed hardware security by transitioning from a passive routing fabric to an active, L3-to-L2 cryptographic gatekeeper.*

---

## Overview

This lab documents the transformation of the Cisco SF300 from a simple routing engine into a hardened security fabric for the Work Network (VLAN 20). We implemented a "Hardware Gatekeeper" architecture to prevent Man-in-the-Middle (MITM) attacks, MAC spoofing, and rogue DHCP server injection. The project highlights critical ASIC-level engineering, including resolving TCAM hardware memory conflicts and establishing trust anchors for non-DHCP static endpoints.

📄 **Full Walkthrough:** For the complete configuration steps, static binding database methodology, and inverse-spoofing validation logs, see the [Infrastructure Build Guide](./2.4-build-guide.md).

---

## Key Highlights

* **Static Trust Anchoring:** Since the Work Laptop (WL) utilizes a hardcoded static IP, it bypassed the DHCP DORA process. We successfully established a hardware trust anchor by manually injecting the WL’s MAC address (`00:2b:67:0c:4c:3b`) into the static binding table.
* **TCAM Hardware Remediation:** Encountered a critical Marvell ASIC memory conflict when attempting to run IP Source Guard (IPSG) and legacy ACLs simultaneously. We performed a total purge of legacy IPv4 ACEs to clear volatile memory, prioritizing hardware-level spoofing protection.
* **L2/L3 Cryptographic Binding:** Activated DHCP Snooping and Dynamic ARP Inspection (DAI) globally, enforcing strict trust boundaries on VLAN 20 to prevent ARP poisoning and rogue addressing.
* **Firmware/ASIC Debugging:** Diagnosed an "Inactive/No Snoop VLAN" exception by explicitly mapping the VLAN 20 interface to the DHCP Snooping engine within the IP Configuration hierarchy.
* **Hardware Validation:** Executed an inverse spoofing test; by modifying the binding database expectation to a mismatched IP (`.101`), the switch ASIC instantly blackholed traffic from the host (`.100`) at wire speed, definitively proving the hardware gatekeeper is active.

---

## Technical Specifications

* **Core L3 Engine:** Cisco SF300-24P (Marvell ASIC)
* **Target Boundary:** VLAN 20 (Work Network) | Port FE24
* **Security Fabric:** * DHCP Snooping (Global Daemon + Static Host Binding)
    * IP Source Guard (IPSG)
    * Dynamic ARP Inspection (DAI)
* **Endpoint:** Work Laptop (`00:2b:67:0c:4c:3b` | `192.168.20.100`)

---

## Contact

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio