# 2.3 – DHCP Infrastructure Automation (VLAN 10/20)

*Transitioning from static manual addressing to automated lease management while maintaining cryptographically and logically isolated broadcast domains.*

---

## Overview

This lab documents the migration of the L3 routing fabric from static address provisioning to automated DHCP lease management. We activated the Cisco SF300’s native DHCP server daemon to manage addressing for VLAN 10 (Home) and VLAN 20 (Work). The project focuses on scope provisioning, infrastructure IP exclusions to prevent gateway conflicts, and validating the L3 air-gap between internal segments and external internet traffic.

📄 **Full Walkthrough:** For the complete configuration steps, pool parameter definitions, and connectivity audit logs, see the [Infrastructure Build Guide](./2.3-build-guide.md).

---

## Key Highlights

* **Service Activation:** Successfully instantiated the global DHCP server daemon on the Cisco ASIC, transitioning the network from manual static assignment to automated lease management.
* **Logical Isolation:** Provisioned discrete, non-overlapping address scopes (`HOME-POOL` and `WORK-POOL`), ensuring that internal broadcast domains remain isolated and predictable.
* **Infrastructure Protection:** Applied strict static address exclusions (`192.168.x.1-20`) to the DHCP engine. This prevents the daemon from issuing leases to critical infrastructure—specifically the SVI gateways—ensuring the routing plane remains stable.
* **Client Migration:** Migrated the Work Laptop (WL) and Home Laptop (DP) endpoints from static configuration to dynamic lease acquisition, validating successful delivery of correct gateway and DNS (8.8.8.8) parameters.
* **Connectivity Audit:** Validated the success of the L3 air-gap. Confirmation of "Destination net unreachable" errors when attempting to resolve public IPs proved the lab successfully prevents unauthorized egress to the production office network.

---

## Technical Specifications

* **Core L3 Engine:** Cisco SF300-24P (DHCP Server: ENABLED)
* **VLAN 10 (Home):** Scope `192.168.10.0/24` | Gateway `192.168.10.1`
* **VLAN 20 (Work):** Scope `192.168.20.0/24` | Gateway `192.168.20.1`
* **Infrastructure Exclusions:** `192.168.x.1` through `192.168.x.20`

---

## Contact

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio