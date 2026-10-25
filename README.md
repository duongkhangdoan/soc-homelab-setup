
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

Go to the following link: [pfSense CE Download](https://atxfiles.netgate.com/mirror/downloads/)

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
<img width="998" height="289" alt="1" src="https://github.com/user-attachments/assets/1439411d-01ce-4c9c-bd8e-406aefb171d8" />

### c. CYBER_RANGE — Cybersecurity Testing

- **Interface:** `vtnet2`
- **IP address:** `10.6.6.1/24`

This network is intended for cybersecurity exercises, controlled testing, and simulated attack-and-defense scenarios. Firewall policies can restrict access to other lab networks.  
<img width="998" height="273" alt="cyyber" src="https://github.com/user-attachments/assets/3f87dbde-2be1-4294-9056-adfae1eb801d" />


### d. AD_LAB — Active Directory Environment

- **Interface:** `vtnet3`
- **IP address:** `10.80.80.1/24`

This network is intended to host a Windows Server domain controller and Windows client machines for practicing domain administration, authentication, Group Policy, and security event monitoring.  
<img width="1002" height="238" alt="ad" src="https://github.com/user-attachments/assets/2f27d6ac-18ce-4347-8b86-d8cff935726c" />


### e. ISOLATED — Isolated Testing Environment

- **Interface:** `vtnet4`
- **IP address:** `10.99.99.1/24`

This network is intended for systems that require additional network isolation. Firewall rules should restrict communication with other lab segments according to testing requirements.   
<img width="998" height="170" alt="iso" src="https://github.com/user-attachments/assets/77dd7836-3d05-4043-b388-7444aec3d811" />


### f. SECURITY — Security Monitoring

- **Interface:** `vtnet5`
- **IP address:** `10.10.10.1/24`

This network is intended for defensive security infrastructure, such as log collection, monitoring tools, and security analysis systems. Access should be limited to required communication paths.  
<img width="1007" height="239" alt="sec" src="https://github.com/user-attachments/assets/1baa08d7-e9f1-473f-a7a0-0d86866bbf16" />


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
---  


# Kali Linux Installation Guide

## a. Download Kali Linux

 Visit the official website: https://www.kali.org/get-kali/
 Download the **64-bit Installer ISO**.

## b. Create a Virtual Machine

Open VirtualBox and create a new virtual machine with the following settings:

| Setting | Configuration |
|---|---|
| Name | Kali Linux |
| Type | Linux |
| Version | Debian (64-bit) |
| Virtual Hard Disk | 80 GB |
| ISO Image | Leave empty initially |

## c. Configure the Virtual Machine

Before starting the VM, configure the following settings.

### System → Motherboard
- Set **Hard Disk** before **Optical** in the boot order.
- Disable **Floppy**.

### System → Processor
- Enable **PAE/NX** if available.

### Display
- Set Video Memory to **128 MB**.

### Storage
- Attach the downloaded Kali Linux ISO file.

## d. Configure Network

Under **Settings → Network → Adapter 1**:

| Setting | Configuration |
|---|---|
| Enable Network Adapter | Enabled |
| Attached to | Internal Network |
| Network Name | Select the intended internal network |
| Adapter Type | Paravirtualized Network (virtio-net) |

**Note:** The VirtualBox Internal Network name must match the network used by the other intended lab machines. Internal Network does not automatically provide Internet access or DHCP.

## e. Install Kali Linux

1. Start the VM.
2. Select **Graphical Install**.
3. Choose your language, location, and keyboard layout.
4. Configure the hostname.
5. Create a user account and password.
6. Select the virtual disk (`sda`).
7. Choose **Guided — use entire disk**.
8. Select **All files in one partition**.
9. Confirm the partition changes and continue the installation.
10. Select **XFCE** as the desktop environment. It is a lightweight option suitable for a lab VM.
11. Complete the installation and reboot.

## f. Post-Installation Setup

After logging in, open a terminal.

### Check Network Interfaces

```bash
ip a
```

### Update the System

```bash
sudo apt update && sudo apt full-upgrade
```

### Remove Unnecessary Packages

```bash
sudo apt autoremove
```

## g. Purpose in the Cybersecurity Lab

Kali Linux is a Linux distribution designed for penetration testing, vulnerability assessment, and security research.

In this lab, Kali Linux can be used for:

- Network reconnaissance and scanning.
- Authorized security testing.
- Controlled penetration-testing exercises.
- Generating security events to practice detection and monitoring.

**Important:** Perform scanning and security testing only on systems and networks you own or have explicit permission to test.
<img width="794" height="719" alt="1" src="https://github.com/user-attachments/assets/1334a823-ca89-4901-baf5-61e7873638c4" />





 



