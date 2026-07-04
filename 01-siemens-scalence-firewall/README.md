01-siemens-scalence-firewall: Infrastructure Build Guide: Bare-Metal Siemens SCALANCE ICS Firewall

A visual guide documenting the hardware inspection, network segmentation, and native SOC telemetry integration of an industrial perimeter firewall.

Overview

This guide provides a comprehensive visual narrative of configuring a Siemens SCALANCE SC646-2C Industrial Firewall. It follows the end-to-end engineering workflow, starting from analyzing physical enterprise OT hardware, utilizing Layer 2 discovery for initial provisioning, configuring Inter-VLAN bridging for Modbus TCP, and culminating in a custom "Living off the Land" PowerShell SIEM pipeline.

Workflow Steps

1. OT Hardware Immersion & Architecture

Before deploying the network logic, a physical inspection of the surrounding enterprise infrastructure was conducted to understand hardware-level fault tolerance. This included analyzing Cisco Secure Firewalls, Cisco SF300 Managed Switches, Fortinet gateways, and extracting modular components like cooling fans and solid-state drives from the chassis.

Cisco & Fortinet Enterprise Hardware Inspection:


Extracting Modular Controller Assemblies:


The firewall itself was powered via a bare-metal Weidmüller PROeco 480W 24V power supply, physically wired directly into the SCALANCE L1+ and M1 terminals.

Weidmüller PROeco 24V Power Delivery:


2. Layer 2 Discovery and Provisioning

Out of the box, the SCALANCE firewall lacked a routable IP address (defaulted to 0.0.0.0), rendering standard web/SSH access impossible. We utilized Siemens PRONETA, a proprietary Layer 2 network analysis tool, to discover the device's MAC address directly on the wire and assign its initial IPv4 address (192.168.70.203/24).

Siemens PRONETA Initial IP Provisioning:


3. Engineering Workstation & Subnet Troubleshooting

To configure the firewall, two engineering laptops were connected via ethernet to ports P1 and P4. Initially, connectivity failed due to a subnet mismatch; the Windows NIC self-assigned an APIPA address (169.254.x.x).

We resolved this by manually accessing the Windows Network Adapters and statically assigning an IPv4 address (192.168.70.93/24) to place the workstation in the exact broadcast domain as the firewall.

Static IPv4 Assignment for OT Subnet Sync:


Live Lab Configuration Environment:


4. Network Segmentation (VLANs & Bridging)

With connectivity established, we accessed the SCALANCE Web GUI to architect the ICS boundary. We segmented the network into VLAN 1 (INT) representing the internal Plant Bus, and VLAN 2 (EXT) representing external third-party systems.

Port-Based VLAN Configuration:


To allow controlled, stateful routing between the internal and external segments, Inter-VLAN bridging was configured and assigned to the respective interfaces.

Inter-VLAN Bridge Activation:


5. Industrial Protocol Security Policies

The primary role of "FW5" is to secure specific industrial communications while blocking everything else. We configured specific IP Services to authorize Modbus-TCP (Port 502) and S7 (Port 102) traffic, along with ICMP for diagnostics.

Defining Modbus TCP & S7 IP Services:


Enforcing Stateful IP Rules Across VLANs:


6. Packet-State Validation (ACCEPT vs. DROP vs. REJECT)

To validate the security boundaries, we manipulated the IP rules and observed the exact packet-level responses via the Windows Command Prompt.

When the rule was set to ACCEPT, ICMP echoes passed seamlessly across the VLANs.


When the rule was changed to DROP, the firewall silently discarded the packets, resulting in standard timeouts. This is the ICS best practice (stealth mode) to prevent network reconnaissance.


When the rule was changed to REJECT, the firewall actively denied the connection, sending a Destination port unreachable ICMP error back to the workstation.


7. Local SOC Telemetry & Native SIEM Integration

Because corporate restrictions prevented the installation of third-party packet analyzers (like Wireshark or Tftpd64), we engineered a "Living off the Land" telemetry pipeline. We configured the SCALANCE Syslog Client to forward all security and fault logs directly to the engineering laptop (UDP Port 5514).

SCALANCE Syslog Client Configuration:


We then wrote a native Windows PowerShell script to act as a UDP listener. This allowed us to capture, parse, and monitor raw intrusion events (such as firewall DROP actions) in real-time natively on the workstation.

Real-Time Native PowerShell Syslog Capture:


Contact

For any questions or feedback, reach out:

Paarth Pandey LinkedIn | GitHub | paarthdxb@gmail.com

Author: Paarth Pandey

OT Security Labs: Industrial Network Defense
