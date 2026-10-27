# pfSense Network Interface Configuration

## Overview  

Go to the following link: [pfSense CE Download](https://atxfiles.netgate.com/mirror/downloads/)

This project uses **pfSense 2.7.2-RELEASE** as the central firewall and router for a virtualized cybersecurity home lab running in VirtualBox.

The pfSense virtual machine has six network interfaces: one WAN interface and five internal interfaces. Each interface is assigned a dedicated IPv4 subnet to separate lab environments and support controlled communication between network segments.

## Network Interface Configuration

| Interface | Virtual NIC | NAT / Mode | VLAN ID | IPv4 Address | Subnet | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **WAN** | `vtnet0` | NAT | - | `10.0.2.15/24` | `10.0.2.0/24` | External connectivity through VirtualBox NAT |
| **LAN** | `vtnet1` | Internal | VLAN 0 | `10.0.0.1/24` | `10.0.0.0/24` | Primary LAN and pfSense management |
| **CYBER_RANGE** | `vtnet2` | Internal | VLAN 1 | `10.6.6.1/24` | `10.6.6.0/24` | Cybersecurity testing and lab exercises |
| **AD_LAB** | `vtnet3` | Internal | VLAN 2 | `10.80.80.1/24` | `10.80.80.0/24` | Active Directory and Windows client environment |
| **ISOLATED** | `vtnet4` | Internal | VLAN 3 | `10.99.99.1/24` | `10.99.99.0/24` | Isolated environment for controlled testing |
| **SECURITY** | `vtnet5` | Internal | VLAN 4 | `10.10.10.1/24` | `10.10.10.0/24` | Security monitoring and defensive security tools |

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
