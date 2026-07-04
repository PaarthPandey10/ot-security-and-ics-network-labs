# OT Security & ICS Network Defense Labs

Hands-on Operational Technology (OT) assignments — configuring enterprise-grade industrial firewalls, switches, and bare-metal ICS infrastructure.

## Table of Contents

* [About](https://www.google.com/search?q=%23about)
* [Project Structure](https://www.google.com/search?q=%23project-structure)
* [Usage / How to Use](https://www.google.com/search?q=%23usage--how-to-use)
* [Features / Highlights](https://www.google.com/search?q=%23features--highlights)
* [Technologies Used](https://www.google.com/search?q=%23technologies-used)
* [Contributing](https://www.google.com/search?q=%23contributing)
* [License](https://www.google.com/search?q=%23license)
* [Contact](https://www.google.com/search?q=%23contact)

---

## About

This repository contains real-world hardware and network security assignments completed while working with physical enterprise infrastructure.

Each lab explores the configuration, segmentation, and defense of industrial network boundaries using equipment from Siemens, Cisco, Fortinet, and Advantech.

---

## Project Structure

```text
ot-security-labs/
├── screenshots/
├── 01-siemens-scalance-firewall/
├───├─ 01-build-guide.md
│   └── README.md
└── LICENSE
```

---

## Usage / How to Use

Each folder contains a lab with:

* Summary of the objective
* Key concepts or OT services used
* PowerShell scripts, or configuration notes

> **Note:** All visual documentation, topology diagrams, and command-line outputs are stored in the centralized `screenshots/` directory.

Start from **Lab 01** to follow the progression from basic industrial boundary setup to advanced deep packet inspection and network switching.

---

## Features / Highlights

* Provisioned bare-metal firewall hardware and established power delivery.
* Configured Industrial Port-Based VLANs (Plant Bus vs. External Systems).
* Routed and secured OT protocols (Modbus TCP, S7).
* Conducted live packet-state validation (ACCEPT vs. DROP vs. REJECT).
* Engineered local "Living off the Land" SOC telemetry pipelines using Syslog and native PowerShell.

---

## Technologies Used

* **Hardware:** Siemens SCALANCE SC646-2C, Cisco Secure Firewall 3100, Cisco SF300-24P Switch, Weidmüller PROeco Power Supply
* **Software & Tools:** Siemens PRONETA, Windows PowerShell, Windows Command Prompt
* **Protocols:** Modbus TCP, S7, ICMP, Syslog, SNMP, IPv4

---

## Contributing

Not open for contributions — personal practice lab and hardware portfolio showcase.

---

## License

This project is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

**You are free to:**

* **Share** — copy and redistribute the material in any medium or format
* **Adapt** — remix, transform, and build upon the material for any purpose, even commercially

**Under the following terms:**

* **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made.

🔗 [View License](https://www.google.com/search?q=https://creativecommons.org/licenses/by/4.0/)

---

## Contact

**Paarth Pandey**
[LinkedIn](https://www.google.com/search?q=https://www.linkedin.com) | [GitHub](https://www.google.com/search?q=https://github.com) | paarthdxb@gmail.com

---

*Note: All labs in this repository are based on hands-on engineering work with physical Operational Technology (OT) hardware.
Screenshots and command flows are reused respectfully for learning and documentation purposes only.*

---
> Author: Paarth Pandey
>
> OT Security Labs: Industrial Network Defense


