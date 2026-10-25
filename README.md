# Soc-Homelab-Setup

A segmented Cybersecurity Home Lab built on VirtualBox and pfSense/OPNsense, featuring dedicated subnets for Active Directory, SOC monitoring (Splunk), malware analysis (Flare VM & REMnux), forensics (Tsurugi), and vulnerable attack targets.</sub>

---

## <span style="color: red;"> Home Lab Overview</span>

- pfSense (Gateway & Firewall)
- Kali Linux (Management VM)
- Active Directory Lab (Domain Controller & 2 Clients)
- Malware Analysis Lab (Windows & Linux)
- Security VMs (DFIR & SIEM)
- Cyber Range (Vulnerable VMs for CTF practice)
<img width="1774" height="887" alt="Cybersecurity Home Lab Network Diagram" src="https://github.com/user-attachments/assets/9a4fb913-9b90-497c-963e-f2811213f408" />

---  
# 1. Installing VirtualBox

Go to the following URL: [Downloads — Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Download VirtualBox and VirtualBox Extension Pack.

As of the writing of this module, the latest version of VirtualBox is `7.2.22`.  
<img width="440" height="196" alt="2" src="https://github.com/user-attachments/assets/2b9555fd-accf-4add-8af8-729e23704aca" />  
---


# 2. pfSense Network Interface Configuration

## Overview

This project uses **pfSense 2.7.2-RELEASE** as the central firewall and router for a virtualized cybersecurity home lab running in VirtualBox.

The pfSense virtual machine has six network interfaces: one WAN interface and five internal interfaces. Each interface is assigned a dedicated IPv4 subnet to separate lab environments and support controlled communication between network segments.

## Network Interface Configuration

| Interface | Virtual NIC | IPv4 Address | Subnet | Purpose |
|---|---|---|---|---|
| WAN | `vtnet0` | `10.0.2.15/24` | `10.0.2.0/24` | External connectivity through VirtualBox NAT |
| LAN | `vtnet1` | `10.0.0.1/24` | `10.0.0.0/24` | Primary LAN and pfSense management |
| CYBER_RANGE | `vtnet2` | `10.6.6.1/24` | `10.6.6.0/24` | Cybersecurity testing and lab exercises |
| AD_LAB | `vtnet3` | `10.80.80.1/24` | `10.80.80.0/24` | Active Directory and Windows client environment |
| ISOLATED | `vtnet4` | `10.99.99.1/24` | `10.99.99.0/24` | Isolated environment for controlled testing |
| SECURITY | `vtnet5` | `10.10.10.1/24` | `10.10.10.0/24` | Security monitoring and defensive security tools |

## Interface Descriptions

### a. WAN — External Network

- **Interface:** `vtnet0`
- **IP address:** `10.0.2.15/24`
- **VirtualBox network mode:** NAT

The WAN interface connects pfSense to the external network through VirtualBox NAT. It provides a path for outbound connectivity, subject to the WAN configuration, firewall rules, and upstream network settings.

### b. LAN — Primary Network

- **Interface:** `vtnet1`
- **IP address:** `10.0.0.1/24`

The LAN interface serves as the primary internal network and can provide access to the pfSense web management interface. It can also host administrative or general-purpose lab systems.

### c. CYBER_RANGE — Cybersecurity Testing

- **Interface:** `vtnet2`
- **IP address:** `10.6.6.1/24`

This network is intended for cybersecurity exercises, controlled testing, and simulated attack-and-defense scenarios. Firewall policies can restrict access to other lab networks.

### d. AD_LAB — Active Directory Environment

- **Interface:** `vtnet3`
- **IP address:** `10.80.80.1/24`

This network is intended to host a Windows Server domain controller and Windows client machines for practicing domain administration, authentication, Group Policy, and security event monitoring.

### e. ISOLATED — Isolated Testing Environment

- **Interface:** `vtnet4`
- **IP address:** `10.99.99.1/24`

This network is intended for systems that require additional network isolation. Firewall rules should restrict communication with other lab segments according to testing requirements.

### f. SECURITY — Security Monitoring

- **Interface:** `vtnet5`
- **IP address:** `10.10.10.1/24`

This network is intended for defensive security infrastructure, such as log collection, monitoring tools, and security analysis systems. Access should be limited to required communication paths.

## Network Segmentation and Security

The lab uses separate IPv4 subnets to organize systems by function. pfSense acts as the central routing and firewall device between connected networks.

Key design goals:

- Separate Active Directory, cybersecurity testing, and security monitoring environments.
- Control inter-network traffic using firewall rules.
- Limit access to administrative interfaces.
- Isolate designated testing systems when required.
- Build a foundation for SOC monitoring, log analysis, and incident response exercises.

**Important:** Assigning separate subnets does not automatically block all communication between them. Inter-network access depends on pfSense routing, firewall rules, and the configuration of the connected VirtualBox networks.

## Current Status

The six interfaces and their IPv4 addresses are configured and visible in the pfSense console. DHCP scopes, firewall policies, client connectivity, and inter-network access should be configured and tested separately.  
<img width="1275" height="743" alt="1" src="https://github.com/user-attachments/assets/422ac6ef-a3a5-44c4-94a5-7b481e1da0e6" />




 



