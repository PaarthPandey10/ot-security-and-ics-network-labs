# 01 – Siemens SCALANCE SC646-2C ICS Segmentation

*Provision an industrial firewall, configure Inter-VLAN bridging for Modbus TCP, and engineer a native Syslog telemetry pipeline.*

---

## Overview

This lab documents the hands-on inspection of enterprise Operational Technology (OT) hardware and the end-to-end network configuration of a Siemens SCALANCE SC646-2C Industrial Firewall.

The configuration simulates the role of "FW5" in a segmented Industrial Control System (ICS) architecture. FW5 acts as the security boundary between an internal Plant Bus (core process automation network) and external/Third-Party systems, specifically configured to facilitate secure Modbus TCP communications.

---

## 🛠️ Phase 1: OT Hardware Immersion & Tinkering

Before deploying the network logic, I conducted a physical inspection and teardown of surrounding enterprise infrastructure in the lab to understand the physical resilience and architecture of OT devices.

**Hardware Analyzed:**

* **Power Supply:** Weidmüller PROeco (Model 1469510000 | 480W 24V 20A) - Physically wired to the Siemens Firewall (L1+ Red / M1 Black).
* **Switches:** Cisco SF300-24P (24-Port 10/100 PoE Managed Switch) & Advantech EKI-9226G Industrial Switch.
* **Firewalls:** Cisco Secure Firewall (w/ NM1 & NM2 modules) & Fortinet enterprise gateways.
* **Peripherals:** Black Box 4K HDMI Dual-Head KVM Ext Receiver (KVXLCH-200-RX).

> **Note:** Tinkered with the physical chassis of these devices, extracting and inspecting modular cooling fan assemblies, solid-state drives (SSDs), and redundant power rails to understand hardware-level fault tolerance.

---

## 🏗️ Phase 2: Bare-Metal Provisioning & Discovery

The core firewall for this deployment was the Siemens SCALANCE SC646-2C.

* **Initial Boot:** Out of the box, the firewall possessed a factory default IP of 0.0.0.0.
* **Layer 2 Discovery:** Because the device had no routable IP, standard web/SSH access was impossible. I utilized Siemens PRONETA (a proprietary Layer 2 network analysis tool) to detect the MAC address (74-FC-45-47-84-62) on the local wire and assign an initial IP address of 192.168.70.203 with a /24 subnet mask.

---

## ⚙️ Phase 3: Subnet Troubleshooting & Connectivity

To configure the firewall, two engineering laptops were connected via ethernet to ports P1 and P4.

**The Subnet Mismatch Issue:**
Initially, my laptop failed to establish a connection. `ipconfig` revealed my NIC had self-assigned an APIPA address (169.254.x.x).

**Resolution:**
Navigated to Windows Control Panel > Network Adapters and statically assigned my IPv4 address to 192.168.70.93 (Subnet 255.255.255.0). This placed my machine in the same broadcast domain as the firewall, immediately resolving the ICMP ping failures.

---

## 🛡️ Phase 4: Network Segmentation & IP Services

Logged into the SCALANCE Web GUI to architect the ICS boundary:

* **SNMP Configuration:** Enabled SNMP V1/V2/V3 for diagnostic monitoring across the plant bus.
* **VLAN Segmentation:**
* Created **VLAN 1 (INT)** for the internal automation network.
* Created **VLAN 2 (EXT)** for third-party external networks.


* **Inter-VLAN Bridge:** Enabled bridging to allow controlled, stateful routing between the internal and external segments.
* **Industrial Protocol Rules:**
* Authorized **Modbus-TCP** (Transport: TCP, Dest Port: 502) to allow external systems to read/write specific PLC registers.
* Authorized **S7** communications (Port 102).
* Enabled **ICMP** (Echo Request Type 8) for network diagnostic testing.



---

## 🧪 Phase 5: Stateful Firewall Rule Testing

To validate the security boundaries, we wrote specific IPv4 rules between VLAN 1 and VLAN 2 and observed the packet-level behavior via Windows Command Prompt.

### Test 1: Action = ACCEPT

Traffic passes freely between the engineering station and the firewall.

```text
C:\Users\Intern>ping 192.168.70.93
Reply from 192.168.70.93: bytes=32 time=2ms TTL=128
Reply from 192.168.70.93: bytes=32 time=1ms TTL=128

```

### Test 2: Action = DROP (Stealth Mode)

The firewall rule was changed to DROP. The SCALANCE firewall silently discards the ICMP packets without notifying the sender. This is the ICS best practice to prevent network scanning/reconnaissance.

```text
C:\Users\Intern>ping 192.168.70.22 -t
Request timed out.
Request timed out.

```

### Test 3: Action = REJECT (Active Refusal)

The firewall rule was changed to REJECT. Instead of silently dropping the packet, the firewall actively sends an ICMP error message back to the sender, confirming the host exists but is actively blocking the port.

```text
C:\Users\Intern>ping 192.168.70.22 -t
Reply from 192.168.70.203: Destination port unreachable.

```

---

## 📡 Phase 6: Local SOC Telemetry & Native SIEM Integration

A firewall is only effective if its logs are actively monitored. Due to corporate restrictions preventing the installation of third-party software (like Wireshark or Tftpd64), I engineered a "Living off the Land" SIEM solution to capture live telemetry.

### 1. Firewall Syslog Configuration

* Navigated to the SCALANCE Web Manager (192.168.2.205).
* Enabled the Syslog Client under **System > Events**.
* Configured the firewall to forward Security, Firewall, and Fault logs to the engineering laptop's IP (192.168.2.200) over UDP Port 5514.

### 2. The Native PowerShell Listener

Using native Windows PowerShell, I built a UDP listener to capture, parse, and display the raw Syslog events in real-time.

```powershell
$Port = 5514
$Endpoint = New-Object System.Net.IPEndPoint([System.Net.IPAddress]::Any, $Port)
$UDPClient = New-Object System.Net.Sockets.UdpClient($Port)
Write-Host "Listening on UDP $Port"

while ($true) {
    $Bytes = $UDPClient.Receive([ref]$Endpoint)
    $Message = [Text.Encoding]::UTF8.GetString($Bytes)
    Write-Host "$(Get-Date) | $($Endpoint.Address)"
    Write-Host $Message
}

```

### 3. Validating the Telemetry

By generating unauthorized traffic that triggered the firewall's DROP rules, the PowerShell script successfully captured the raw intrusion events, verifying that the OT telemetry pipeline was fully operational.

```text
07/03/2026 17:06:19 | 192.168.2.205
<132>1 2000-09-09T13:44:27+00:00 - 6GK5646-2GS00-2AC2 11 - [meta sysUpTime="2383200"] DROP(3) in:vlan2 out:vlan1 len:64 
s-mac:E4:B9:7A:72:97:42 d-mac:01:00:5E:7F:FF:FA 
s-ip:192.168.1.100 d-ip:239.255.255.250 
udp:50711->3702

```

---

## Contact

**Paarth Pandey**
[LinkedIn](https://www.linkedin.com) | [GitHub](https://github.com) | paarthdxb@gmail.com

> Author: Paarth Pandey
>
> Open-Source Contributions Portfolio
