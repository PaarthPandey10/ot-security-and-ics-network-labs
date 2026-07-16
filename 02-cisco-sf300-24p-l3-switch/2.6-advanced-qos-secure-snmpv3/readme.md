# 2.6 – Advanced QoS (DSCP Trust) & Secure SNMPv3 Telemetry Engine

*Deploying wire-speed hardware-level DSCP trust boundaries and implementing encrypted network telemetry via the SNMPv3 USM cryptographic model.*

---

## Overview

This lab addresses two critical infrastructure requirements: maintaining traffic priority in a restrictive Layer 3 hardware environment and establishing a secure, encrypted monitoring pipeline. Due to physical ASIC memory limitations in Layer 3 mode, we abandoned bandwidth policing in favor of a CoS/DSCP Trust architecture. Concurrently, we transitioned switch monitoring from unencrypted SNMP to a robust SNMPv3 (AuthPriv) implementation to protect management traffic from interception.

📄 **Full Walkthrough:** For the complete configuration steps, cryptographic parameter setup, and verification logs, see the [Infrastructure Build Guide](./2.6-build-guide.md).

---

## Key Highlights

* **Layer 3 QoS Constraint Engineering:** Circumvented hardware ASIC limitations by implementing a strict DSCP/CoS Trust Model. The ingress boundary (FE24) now natively trusts and forwards endpoint-defined QoS markings without the overhead of aggregate rate policers.
* **ASIC TCAM Optimization:** Executed a surgical purge of legacy Project 2 stateless ACLs to free up physical TCAM memory, allowing for the simultaneous operation of IP Source Guard (IPSG) and the new QoS trust policies.
* **Cryptographic Telemetry Enclave:** Transitioned the monitoring plane to SNMPv3, utilizing the User Security Model (USM). This enforced an "AuthPriv" (Authentication and Privacy) model using SHA hashing and DES encryption to secure all telemetry traffic.
* **Granular Visibility Control:** Provisioned a custom SNMP `telemetry-view` restricted to the root OID `1`, granting the monitoring server full read access to interface and CPU metrics while natively sandboxing the switch against unauthorized remote write modifications.
* **Hardware-Level Validation:** Utilized Wireshark deep packet inspection to confirm DSCP tags (`CS0`) remained intact across the ASIC routing backplane. Validated SNMPv3 cryptographic success via Paessler SNMP Tester, confirming successful MIB tree polling (sysUpTime) via the encrypted tunnel.

---

## Technical Specifications

* **Core L3 Engine:** Cisco SF300-24P (ASIC Filtering: ENABLED)
* **QoS Fabric:** Trust CoS/802.1p-DSCP | Ingress: Port FE24
* **Telemetry Fabric:** SNMPv3 (Auth: SHA | Priv: DES)
* **Monitoring Scope:** Read-Only (`telemetry-view` / OID `1`)
* **Persistence:** NVRAM-committed configuration

---

## Contact

**Paarth Pandey** | [LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

---

> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio