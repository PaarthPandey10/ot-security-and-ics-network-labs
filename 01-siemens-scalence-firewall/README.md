# 01 – Siemens SCALANCE SC646-2C ICS Segmentation

*Provision an industrial firewall, configure Inter-VLAN bridging for Modbus TCP, and engineer a native Syslog telemetry pipeline.*

---

## Overview

This lab involves the bare-metal provisioning and network configuration of a Siemens SCALANCE SC646-2C Industrial Firewall. It simulates the deployment of "FW5," a critical security boundary facilitating controlled Modbus TCP communication between an internal Plant Bus and Third-Party External Systems.

📄 **Full Walkthrough:** For the complete, step-by-step visual configuration and packet validation, see the Infrastructure Build Guide.

---

## Key Highlights

* **Physically provisioned** the firewall using a Weidmüller 24V power supply.
* **Discovered and initialized** factory-default hardware (0.0.0.0) via Siemens PRONETA.
* **Resolved APIPA subnet mismatches** to establish static IPv4 communication with engineering workstations.
* **Configured Port-Based VLANs** (INT/EXT) and enabled Inter-VLAN bridging.
* **Secured ICS protocol routing** specifically for Modbus TCP (Port 502) and S7 (Port 102).
* **Validated stateful firewall rules** (ACCEPT, DROP, REJECT) via live ICMP packet state testing.
* **Engineered a "Living off the Land" local SOC telemetry pipeline** using native Windows PowerShell to capture real-time Syslog events.

---

## Contact

For any questions or feedback, reach out:

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> OT Security Labs: Industrial Network Defense
