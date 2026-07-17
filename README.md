# Enterprise IT/OT Infrastructure & Network Security Labs

Hands-on hybrid Operational Technology (OT) and Enterprise IT engineering portfolios — configuring enterprise-grade industrial firewalls, managed multilayer switches, security fabrics, and bare-metal hardware.

---

## Folder Structure 
```
enterprise-it-ot-infrastructure-security-labs/
├── screenshots/
├── 01-siemens-scalance-firewall/
│   ├── README.md
│   ├── 01-lab-setup.md
│   ├── 02-firewall-config.md
│   ├── 03-modbus-filtering.md
│   └── 04-s7-filtering.md
├── 02-cisco-sf300-24p-l3-switch
│   ├── README.md
│   ├── 2.1-inter-vlan-routing/
│   │   ├── 2.1-build-guide.md
│   │   └── README.md
│   ├── 2.2-stateless-access-control/
│   │   ├── 2.2-build-guide.md
│   │   └── README.md
│   ├── 2.3-dhcp-infrastructure-automation/
│   │   ├── 2.3-build-guide.md
│   │   └── README.md
│   ├── 2.4-anti-spoofing-fabric/
│   │   ├── 2.4-build-guide.md
│   │   └── README.md
│   ├── 2.5-stp-loop-prevention/
│   │   ├── 2.5-build-guide.md
│   │   └── README.md
│   └── 2.6-advanced-qos-secure-snmpv3/
│       ├── 2.6-build-guide.md
│       └── README.md
├── LICENSE
└── README.md
```
    
---

## Portfolio Architecture

This repository is organized by hardware vendor and functional security domain, ranging from industrial protocol filtering to enterprise-grade L3 routing and telemetry.

## Hardware Portfolio Highlights

* **OT/ICS Boundary Defense:** Provisioned Siemens SCALANCE firewalls to perform deep packet inspection on industrial protocols, specifically filtering Modbus/TCP and S7 traffic to protect PLC environments.
* **Multi-VLAN L3 Core & Routing Engine:** Leveraged physical Cisco silicon to route and segment traffic at wire speed across distinct broadcast domains.
* **Control Plane Hardening & Cryptographic Bypass:** Intercepted legacy 2010-era switch SSH control planes by forcing modern OpenSSH clients to negotiate legacy Key Exchanges and Ciphers. Pivoted to zero-crypto OOB serial debugging to bypass CPU exhaustion locks.
* **Defensive Boundary Fabrics:** Deployed wire-speed hardware validation using IP Source Guard (IPSG), DHCP Snooping, and Dynamic ARP Inspection (DAI) to defeat spoofing and MITM attacks.
* **Secure Industrial Monitoring:** Engineered local "Living off the Land" SNMPv3 User Security Model (USM) profiles implementing SHA/DES authentication and encryption contexts.

---

## Technologies & Equipment Used

* **Hardware:** Cisco SF300-24P, Siemens SCALANCE SC646-2C, Cisco Secure Firewall 3100, Dell Precision 7560 Engineering Workstation
* **Software & Tooling:** Wireshark Foundation (Cryptographic Negotiation & Packet Analysis), Paessler SNMP Tester, Siemens PRONETA, PuTTY (OOB Serial Console), Windows PowerShell
* **Key Protocols:** SNMPv3 (AuthPriv), DSCP/CoS, RSTP (802.1w), DHCP Snooping, Dynamic ARP Inspection (DAI), IP Source Guard (IPSG), IPv4 Static/Inter-VLAN Routing, Modbus/TCP, S7

---

## Contact & Credits

**Paarth Pandey**
[LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com


> Author: Paarth Pandey
> 
> Enterprise IT/OT Infrastructure Security Lab Portfolio


*Note: All portfolios are compiled from actual hands-on engineering lab environments utilizing physical hardware. Visual documentation, topology diagrams, and command-line outputs are provided for learning and portfolio demonstration purposes.*
