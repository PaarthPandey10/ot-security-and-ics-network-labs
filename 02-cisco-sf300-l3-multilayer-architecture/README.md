# 02 – Cisco SF300 L3 Multilayer Architecture & Control Plane Debugging

*Provision a multilayer switch from factory reset, negotiate legacy cryptography, establish Out-of-Band (OOB) serial management, and definitively validate hardware-level Inter-VLAN ASIC routing.*

---

## Overview

This lab involves the end-to-end bare-metal provisioning of a Cisco SF300-24P in Layer 3 System Mode. The objective extends beyond standard VLAN configuration to include severe control plane debugging, OpenSSH cryptographic bypass, firmware anomaly resolution, and Host-OS routing isolation to prove authentic hardware-level transit.

📄 **Full Walkthrough:** For the complete, step-by-step visual configuration, CLI methodology, and physical traceroute validation, see the [Infrastructure Build Guide](./02-build-guide.md)

---

## Key Highlights

* **System Initialization & Legacy Crypto Negotiation:** Forced the switch into L3 mode and manually bypassed OpenSSH deprecation blocks using CLI override flags for legacy KEX, Host Keys, Ciphers, and MACs.
* **Control Plane Diagnostics & OOB Pivot:** Identified severe CPU exhaustion causing SSH daemon crashes during ASIC routing updates. Pivoted to zero-crypto Out-of-Band (OOB) serial management via DB9/RJ45 connection to safely inject the configuration.
* **SVI Autostate Reverse-Engineering:** Diagnosed dormant routing interfaces, proving that Switch Virtual Interfaces (SVIs) require physical L1/L2 continuity to transition to an "UP" state.
* **L2 Segmentation & L3 Routing Engine:** Deployed Port-Based VLANs (10/20) and configured IP routing interfaces. Validated the Routing Information Base (RIB) to ensure networks were "Directly Connected."
* **Host Firewall Remediation:** Modified Windows Defender Advanced Security scopes to explicitly permit cross-subnet ICMP requests across isolated broadcast domains.
* **OS Kernel Bypass & Data Plane Validation:** Diagnosed the Windows "Weak Host" loopback mechanism during a multi-homed probe test. Re-architected a strict 2-host topology to force a 2-hop traceroute, definitively proving packet transit through the Cisco ASIC.

---

## Contact

For any questions or feedback, reach out:

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> OT Security Labs: Industrial Network Defense
