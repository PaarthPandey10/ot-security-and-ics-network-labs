# 2.5 – STP/RSTP Loop Prevention & Error Recovery

*Implementing sub-second network convergence and automated broadcast storm mitigation to ensure fabric-wide L2 stability and self-healing.*

---

## Overview

This lab documents the transition from legacy Spanning Tree configurations to the Rapid Spanning Tree Protocol (802.1w) to achieve sub-second network convergence. We focused on hardening the network ingress boundaries (Home and Work ports) to prevent Layer 2 broadcast storms—a critical requirement for the switch's CPU stability. The deployment includes BPDU Guard for rogue switch detection and a 300-second automated error recovery mechanism, creating a resilient, self-healing L2 topology.

📄 **Full Walkthrough:** For the complete configuration steps, RSTP convergence methodology, and error recovery logic, see the [Infrastructure Build Guide](./2.5-build-guide.md).

---

## Key Highlights

* **RSTP Migration (802.1w):** Successfully transitioned the fabric to Rapid Spanning Tree Protocol, enabling sub-second convergence during topology changes compared to legacy STP timers.
* **Administrative Edge Hardening:** Promoted ports FE19 and FE24 to "Edge Port" (PortFast) status, eliminating unnecessary listening/learning latency and preventing TCN (Topology Change Notification) propagation during link-up.
* **Rogue Switch Mitigation:** Instantiated BPDU Guard on edge ports, ensuring that any unsolicited Bridge Protocol Data Units—indicative of unauthorized downstream bridge insertion—trigger an immediate port shutdown.
* **Automated Self-Healing:** Configured global Error Recovery for BPDU Guard and Port Security events with a 300-second cooldown. This implements an automated self-healing loop, allowing the ASIC to restore port integrity without manual administrator intervention.
* **ASIC Logic Verification:** Validated the internal switch state, confirming active 802.1w logic, persistent edge flags, and error recovery policy initialization.

---

## Technical Specifications

* **Core L3 Engine:** Cisco SF300-24P (ASIC Hardware-level STP)
* **Target Boundaries:** Port FE19 (Home) & Port FE24 (Work)
* **Security Fabric:** * Rapid Spanning Tree Protocol (802.1w)
    * Administrative Edge Ports (PortFast behavior)
    * BPDU Guard
    * Error Recovery (300s timer)

---

## Contact

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio