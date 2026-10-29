# Cybersecurity Home Lab & SOC Environment

A segmented cybersecurity home lab built with VirtualBox and pfSense to practice network security, Active Directory administration, SIEM monitoring, vulnerability assessment, malware analysis, and digital forensics in a controlled environment.

The goal of this project is to simulate an enterprise-style network and develop practical skills in security monitoring, threat detection, system administration, and incident investigation.

## Project Overview

This lab contains five network segments designed to separate general network access, security testing, Active Directory systems, malware analysis, and security monitoring.

Key components include:

- **VirtualBox** — Virtualization and lab infrastructure
- **pfSense** — Firewall, routing, and network segmentation
- **Windows Server 2019 & Windows 10** — Active Directory domain and client systems
- **Splunk Enterprise** — SIEM and centralized log monitoring
- **Kali Linux** — Authorized security testing
- **Metasploitable 2 & Chronos** — Vulnerable targets for controlled testing
- **FLARE-VM & REMnux** — Malware analysis environment
- **Tsurugi Linux** — Digital forensics and incident response (DFIR)

## Network Architecture

The lab uses pfSense to route and control traffic between separate VirtualBox networks. Firewall rules determine which systems and subnets can communicate.

<img width="1774" height="887" alt="Neon Cybersecurity Home Lab Network (1)" src="https://github.com/user-attachments/assets/93721baa-74da-4303-922d-6685f3924dbe" />


### Network Segmentation

| Network | Subnet | Gateway | Purpose |
|---|---|---|---|
| LAN (VLAN 0) | `10.0.0.0/24` | `10.0.0.1` | General lab network and Kali Linux |
| CYBER_RANGE (VLAN 1) | `10.6.6.0/24` | `10.6.6.1` | Vulnerable targets and security testing |
| AD_LAB (VLAN 2) | `10.80.80.0/24` | `10.80.80.1` | Active Directory and Windows clients |
| ISOLATED (VLAN 3) | `10.99.99.0/24` | `10.99.99.1` | Isolated malware analysis environment |
| SECURITY (VLAN 4) | `10.10.10.0/24` | `10.10.10.1` | Security monitoring and DFIR tools |

> **Note:** These are separate subnets connected through pfSense. Actual traffic isolation depends on the configured firewall rules. The VLAN labels describe the logical segments in this diagram; VirtualBox Internal Networks are used to connect the VMs.

## Lab Components

### 1. Network Infrastructure

**Technologies:** VirtualBox, pfSense

- Configure network interfaces and internal networks.
- Create separate subnets for different lab environments.
- Manage routing and firewall rules between network segments.
- Practice network isolation and controlled connectivity.

### 2. Security Testing — CYBER_RANGE

**Technologies:** Kali Linux, Metasploitable 2, Chronos

| System | IP Address | Role |
|---|---|---|
| Metasploitable 2 | `10.6.6.11` | Vulnerable target |
| Chronos 1 | `10.6.6.12` | Vulnerable target |

Kali Linux is used for authorized reconnaissance and security testing against intentionally vulnerable systems in the lab.

### 3. Active Directory Lab — AD_LAB

**Technologies:** Windows Server 2022, Windows 10 Enterprise, Active Directory Domain Services, DNS

| System | IP Address | Role |
|---|---|---|
| Windows Server 2022 | `10.80.80.2` | Domain Controller |
| Windows 10 Client 1 | `10.80.80.11` | Domain client |
| Windows 10 Client 2 | `10.80.80.12` | Domain client |

This environment is designed to practice domain administration, user and group management, DNS, Group Policy, and Windows security event generation.

### 4. Malware Analysis — ISOLATED

**Technologies:** FLARE-VM, REMnux

| System | IP Address | Role |
|---|---|---|
| FLARE-VM | `10.99.99.11` | Windows-based malware analysis |
| REMnux | `10.99.99.12` | Linux-based malware analysis |

This segment is intended for controlled malware analysis and examination of suspicious files. It should remain isolated from production systems and untrusted networks.

### 5. Security Monitoring & DFIR — SECURITY

**Technologies:** Splunk Enterprise, Tsurugi Linux

| System | IP Address | Role |
|---|---|---|
| Tsurugi Linux | `10.10.10.2` | Digital forensics and incident response |
| Splunk Enterprise | DHCP / assigned IP | SIEM and log analysis |

Splunk is intended to collect and analyze security logs, while Tsurugi provides tools for digital forensics and investigation.

## SIEM Log Collection

The planned Windows log collection pipeline is:

```text
Windows Server 2019
        |
        v
Splunk Universal Forwarder
        |
        v
Splunk Enterprise
        |
        v
Search, Monitor & Investigate
```

The objective is to ingest Windows Event Logs into Splunk and practice searching, reviewing, and investigating security-related events.

## Learning Objectives

- Understand network segmentation, routing, and firewall rules.
- Configure and administer a Windows Active Directory domain.
- Generate and investigate Windows security events.
- Configure SIEM log collection with Splunk.
- Practice authorized vulnerability assessment in a controlled lab.
- Explore malware analysis and digital forensics workflows.
- Develop foundational blue team, threat detection, and incident investigation skills.

## Documentation

Detailed setup instructions and configuration notes are organized in the `docs/` directory.

- [pfSense](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/2-%20pfSense%20Network%20Interface%20Configuration.md)
- [Kali Linux](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/3-%20Kali%20Linux%20Installation.md)
- [Cyber Range](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/4-%20Cyber%20Range%20Configuration.md)
- [Active Directory Lab](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/5-%20Active%20Directory%20Lab%20Setup.md)
- [Malware Analysis](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/6-%20Malware%20Analysis%20Lab%20Setup.md)
- [DFIR — Tsurugi Linux](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/7-%20DFIR%20Lab%20Setup%20-%20Tsurugi%20Linux.md)
- [Splunk SIEM](https://github.com/duongkhangdoan/soc-homelab-setup/blob/main/doc/Splunk%20SIEM%20Setup%20%26%20Configuration.md)
  
*Documentation will be updated as each component is configured and tested.*

## Project Status

**Status:** In Progress

The lab is being developed in stages, from network infrastructure and virtual machines to SIEM integration and security investigation exercises.

Future improvements may include:

- Splunk dashboards and detection searches.
- Windows security event investigation scenarios.
- Simulated attack and detection exercises.
- Incident response and forensic investigation write-ups.

## Safety & Responsible Use

This environment is intended for education and authorized security testing only.

- Perform security testing only against systems you own or have permission to test.
- Keep intentionally vulnerable machines isolated from production networks.
- Use VM snapshots before making significant changes or running suspicious files.
- Review firewall rules and verify network isolation before conducting security experiments.
- Never use real sensitive data in the lab.
