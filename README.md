
# Enterprise-Style SOC Home Lab

A virtual enterprise security environment featuring segmented networks, Active Directory, Windows endpoints, Splunk SIEM, malware analysis, digital forensics, and vulnerable systems for security monitoring and incident detection.

---

## Home Lab Overview

- pfSense (Gateway & Firewall)
- Kali Linux (Management VM)
- Active Directory Lab (Domain Controller & 2 Clients)
- Malware Analysis Lab (Windows & Linux)
- Security VMs (DFIR & SIEM)
- Cyber Range (Vulnerable VMs for CTF practice)
<img width="1774" height="887" alt="Cybersecurity Home Lab Network Diagram" src="https://github.com/user-attachments/assets/9a4fb913-9b90-497c-963e-f2811213f408" />

---
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


# 3. Kali Linux Installation Guide

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

---  
---

# 4. Cyber Range Configuration

This section explains how to set up vulnerable virtual machines in VirtualBox for cybersecurity practice.

## 1. Metasploitable 2

### Download
- Download Metasploitable 2 from [VulnHub](https://www.vulnhub.com/entry/metasploitable-2,29/).
- Extract the downloaded `.zip` file.
- Locate the `.vmdk` virtual disk file.

### Create and Configure the VM
 Create a new VM in VirtualBox named **Metasploitable 2**.
 Set the OS type and version according to the image requirements.
 Allocate **1024 MB RAM**.
 Select **Do Not Add a Virtual Hard Disk**.
 Finish creating the VM.
 Move the `.vmdk` file into the VM folder.
 Open **Settings → Storage** and attach the `.vmdk` file.
 Under **System → Motherboard**, place Hard Disk first in the boot order and disable Floppy.
 Under **Network → Adapter 1**, select **Internal Network** and choose the intended network.

### Test Connectivity
a. Start the VM and log in:

   - Username: `msfadmin`
   - Password: `msfadmin`

b. Check the IP address:

   ```bash
   ip a l eth0
   ```
<img width="992" height="543" alt="1" src="https://github.com/user-attachments/assets/0d9b53bf-b178-4db7-9c24-606598a45801" />

c. Test connectivity from Metasploitable 2:

   ```bash
   ping google.com -c 5
   ping 10.0.0.2 -c 5
   ```
<img width="616" height="228" alt="1" src="https://github.com/user-attachments/assets/7d8065f1-0676-4474-ba73-c9955a270c19" />


d. From Kali Linux, test connectivity to Metasploitable 2:

   ```bash
   ping <METASPLOITABLE_IP> -c 5
   ```
<img width="568" height="416" alt="2" src="https://github.com/user-attachments/assets/920cb13d-99a5-446f-8f50-32d63cbb02ec" />

## 2. Chronos

### Download and Import
 Download Chronos from [VulnHub](https://www.vulnhub.com/entry/chronos-1,735/).
 Open VirtualBox and select **Tools → Import**.
 Select the downloaded `.ova` file.
 Set RAM to **1024 MB** if appropriate for your system.
 Select **Generate new MAC addresses for all network adapters**.
 Import the virtual machine.

### Configure the VM
 Move Chronos into the **Home Lab/Cyber Range** group, if using groups.
 Open **Settings → System → Motherboard**.
 Place Hard Disk first in the boot order and disable Floppy.
 Open **Settings → Network → Adapter 1**.
 Set **Attached to: Internal Network**.
 Select the intended network.
 Set **Adapter Type: Paravirtualized Network (virtio-net)**.
 Save the settings and start the VM.

### Test Connectivity
 Open the pfSense web portal from Kali Linux.
 Navigate to **Status → DHCP Leases**.
 Find the IP address assigned to Chronos.
 From Kali Linux, test connectivity:  
 
<img width="1044" height="509" alt="1" src="https://github.com/user-attachments/assets/67f2e897-4cad-4b6a-a252-3e9cbd022709" />


   ```bash
   ping <CHRONOS_IP> -c 5
   ```
<img width="569" height="232" alt="1" src="https://github.com/user-attachments/assets/073031ac-9ea1-45a5-b2f1-6660b78e4a83" />

## 3. Cyber Range Network

- **Kali Linux:** Used for authorized penetration testing.
- **Metasploitable 2:** Vulnerable target machine for security practice.
- **Chronos:** Additional vulnerable target machine.
- **pfSense:** Routes and controls traffic between configured networks.

### Important Notes
- Replace `<METASPLOITABLE_IP>` and `<CHRONOS_IP>` with the actual IP addresses assigned to the VMs.
- The article uses `10.6.6.11` for Metasploitable 2 and `10.6.6.12` for Chronos as examples; your addresses may differ.
- Ensure the target VMs are connected to the intended lab network and that DHCP and firewall rules are configured appropriately.
- Keep vulnerable VMs isolated from untrusted networks and use them only for authorized testing.
---  
---

# 5.Active Directory Lab Setup

## 1. Lab Overview

Build an Active Directory environment using VirtualBox to practice domain administration, authentication, Group Policy, and security monitoring.

### Virtual Machines
Go to the following URL:[Windows Server 2019 | Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022?msockid=14e0f61eb4bf64860e83e1f0b5fc65aa)  

Go to the following URL:[Windows 10 Enterprise | Microsoft Evaluation Center](https://archive.org/details/Windows10EnterpriseLTSC202164Bit)

| VM | Role | RAM | Disk |
|---|---|---:|---:|
| Windows Server 2019 | Domain Controller (DC), AD DS, DNS, DHCP, AD CS | 4 GB | 100 GB | 
| Windows 10 Enterprise - Client 1 | Domain-joined workstation | Default | 100 GB |
| Windows 10 Enterprise - Client 2 | Domain-joined workstation | Default | 100 GB |  
<img width="1628" height="712" alt="1" src="https://github.com/user-attachments/assets/a7229c55-4167-445d-bde8-7f49ff4e2a51" />


## 2. Network Configuration

Connect all three VMs to the VirtualBox Internal Network corresponding to the `AD_LAB` interface on pfSense.

| Setting | Configuration |
|---|---|
| Network | AD_LAB |
| Subnet | `10.80.80.0/24` |
| pfSense Gateway | `10.80.80.1` |
| Domain Controller IP | `10.80.80.2` |
| Subnet Mask | `255.255.255.0` |
| Client DNS | `10.80.80.2` |
| Domain Name | `ad.lab` |  
<img width="423" height="475" alt="2" src="https://github.com/user-attachments/assets/7ef5c52e-e274-43f9-a4b8-9a6943fefdcb" />
<img width="511" height="444" alt="1" src="https://github.com/user-attachments/assets/031fd3d2-9446-46b1-a36b-948538a797bc" />

### DHCP Scope

Configure DHCP on the Domain Controller if it will assign addresses to the clients.

- Start IP: `10.80.80.11`
- End IP: `10.80.80.243`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `10.80.80.1`
- DNS Server: `10.80.80.2`

Ensure that DHCP is enabled on the intended network only and that pfSense firewall rules allow the required traffic.
<img width="441" height="490" alt="1" src="https://github.com/user-attachments/assets/ed3f4009-6904-4df4-a00d-34a08696da36" />
<img width="603" height="489" alt="1" src="https://github.com/user-attachments/assets/ce120347-d688-4d6d-bc64-969136888039" />

## 3. Domain Controller Configuration

On Windows Server 2019:

- Configure a static IP address: `10.80.80.2`.
- Rename the server (for example, `DC1`).
- Install Active Directory Domain Services (AD DS) and DNS Server.
- Promote the server to a Domain Controller.
- Create a new forest named `ad.lab`.
- Configure the DNS forwarder as `10.80.80.1`.
- Install and configure DHCP if the Domain Controller will assign client IP addresses.
- Install Active Directory Certificate Services (AD CS) with the Certificate Authority role.

## 4. Active Directory Users

Open **Active Directory Users and Computers** to create domain accounts.


Create:

- A dedicated domain administrator account and add it to the `Domain Admins` group.
- Two standard domain user accounts for the client machines.

Use the administrator account for domain administration and the standard accounts for regular client logins.  
<img width="552" height="478" alt="1" src="https://github.com/user-attachments/assets/a128f497-898f-4432-9a28-999237dd7566" />  
<img width="716" height="506" alt="2" src="https://github.com/user-attachments/assets/906c2f0f-f7d0-4745-a317-e0febd930e21" />  

## 5. Vulnerable AD Configuration (Optional)

This step configures the Active Directory lab with intentionally vulnerable settings for security testing and attack detection.

a. Open **Windows PowerShell as Administrator** on the Domain Controller.
b. Allow script execution for the current configuration:

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Bypass -Force
   ```
<img width="962" height="191" alt="2" src="https://github.com/user-attachments/assets/2492f8ee-3af0-4152-9551-ce0654225014" />

c. Download and execute the Vulnerable AD script, replacing `ad.lab` with your actual domain name:

   ```powershell
   [System.Net.WebClient]::new().DownloadString('https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1') -replace 'change\.me', 'ad.lab' | Invoke-Expression
   ```
<img width="1121" height="198" alt="1" src="https://github.com/user-attachments/assets/d6ca15c0-efdb-4eb0-87d6-bbdfeed9d747" />

d. Wait for the script to finish. According to the guide, the system waits 30 seconds and then restarts automatically.

**Security warning:** These settings reduce security. Apply them only in an isolated, controlled lab. Do not use them on production systems or expose the lab directly to the Internet. Review scripts before executing them, and take snapshots before making changes.

## 6. Windows Client Configuration

Repeat the following setup for both Windows 10 Enterprise VMs:

 Install Windows 10 Enterprise.
 Install VirtualBox Guest Additions.
 Connect the VM to the `AD_LAB` Internal Network.
 Ensure the client receives an IP address from the intended DHCP server.
 Set the client's DNS server to `10.80.80.2`.
 Rename the client to an identifiable name, such as `CLIENT1` or `CLIENT2`.
 Join the `ad.lab` domain using authorized domain credentials.
 Restart the client.
 Log in using the appropriate domain user account.
<img width="1106" height="554" alt="1" src="https://github.com/user-attachments/assets/7c44d1d9-40bd-4c8b-8b7a-816b50b57403" />
<img width="1122" height="366" alt="1" src="https://github.com/user-attachments/assets/49ae92f9-4928-4e6f-9092-c5a90f65c2b9" />

 

## 7. Verification

### Verify IP Configuration

On each Windows client, open PowerShell or Command Prompt:

```powershell
ipconfig /all
```

Confirm that the client has an IP address in `10.80.80.0/24`, the correct gateway, and DNS set to `10.80.80.2`.

### Verify Domain Login

Run:

```powershell
whoami
```

Confirm that the logged-in account uses the expected domain identity.

### Verify DHCP and DNS

- Open DHCP Manager on the Domain Controller and confirm that client leases exist.
- Open DNS Manager and confirm that client DNS records have been registered.
- Confirm that both clients can resolve the `ad.lab` domain.
<img width="935" height="779" alt="1" src="https://github.com/user-attachments/assets/a2d6d3c1-3b1c-4df7-8536-121c555edc7b" />

### Apply Group Policy

After running the vulnerable AD script and restarting the Domain Controller, configure the following Group Policies using **Group Policy Management**.

#### a. Disable Windows Defender and Firewall

- Create a GPO named `Disable Protections` and link it to the `ad.lab` domain.
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Defender Antivirus`
- Enable **Turn off Windows Defender Antivirus**.
- Under **Real-time Protection**, enable **Turn off real-time protection**.
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → Network → Network Connections → Windows Defender Firewall → Domain Profile`
- Set **Windows Defender Firewall: Protect all network connections** to **Disabled**.
- Mark the `Disable Protections` GPO as **Enforced**.

#### b. Enable Remote Login for Local Administrators

- Create a GPO named `Local Admin Remote Login`.
- Navigate to:
  `Computer Configuration → Preferences → Windows Settings → Registry`
- Create a Registry Item with:
  - Hive: `HKEY_LOCAL_MACHINE`
  - Key Path: `SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
  - Value Name: `LocalAccountTokenFilterPolicy`
  - Value Type: `REG_DWORD`
  - Value Data: `1`

#### c. Enable WinRM Server

- Create a GPO named `Enable WinRM Server`.
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Remote Management (WinRM) → WinRM Service`
- Enable **Allow remote server management through WinRM** and set the IPv4 filter to `*`.
- Enable **Allow Basic authentication**.
- Enable **Allow unencrypted traffic**.
- Navigate to:
  `Computer Configuration → Preferences → Control Panel Settings → Services`
- Create a service configuration for **Windows Remote Management (WS-Management)**:
  - Startup: `Automatic`
  - Action: `Start service`
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → Windows Components → Windows Remote Shell`
- Enable **Allow Remote Shell Access**.

#### d. Enable Remote Desktop Protocol (RDP)

- Create a GPO named `Enable RDP`.
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → Windows Components → Remote Desktop Services → Remote Desktop Session Host → Connections`
- Enable **Allow users to connect remotely using Remote Desktop Services**.

#### e. Enable RPC Configuration

- Create a GPO named `Enable RPC`.
- Navigate to:
  `Computer Configuration → Policies → Administrative Templates → System → Remote Procedure Call`
- Enable **Enable RPC Endpoint Mapper Client Authentication**.

#### f. Apply Group Policies

- Open **Windows PowerShell as Administrator** on the Domain Controller.
- Run:

  ```powershell
  gpupdate /force
  ```
<img width="905" height="519" alt="1" src="https://github.com/user-attachments/assets/c63cbfd8-244b-46ec-b3cd-a2f5d5339a6f" />  
<img width="774" height="271" alt="1" src="https://github.com/user-attachments/assets/ad9fb89a-d4ee-40e1-8e1c-3d48477ad172" />


- Confirm that the GPOs are linked to the domain and applied to the intended computers.
- Keep the Domain Controller powered on before starting the Windows client VMs.

**Security Warning:** These policies intentionally weaken security, including disabling endpoint protections and enabling less-secure remote management options. Use them only in an isolated lab, never on production systems. Take snapshots before applying changes.

## 8. Snapshots and Lab Startup

After confirming that the environment works:

- Power off each VM before taking a clean snapshot.
- Create snapshots of the Domain Controller and both clients.
- Keep the Domain Controller running before starting the client VMs.
- Retain the Windows 10 ISO if it will be used later to build a FlareVM.

## 9. Learning Objectives

This lab provides practice with:

- Active Directory domain administration.
- User and group management.
- DNS and DHCP configuration.
- Domain authentication and domain-joined endpoints.
- Group Policy management.
- Certificate Authority fundamentals.
- Windows security configuration and remote administration.
- Generating Windows security events for SOC monitoring and detection.

**Lab Result:** One Windows Server 2019 Domain Controller and two Windows clients joined to the `ad.lab` domain, ready for further security monitoring and detection exercises.   
---
---

# 6. Malware Analysis Lab Setup

## 1. Flare VM Setup

**Purpose:** Analyze suspicious files and malware in a dedicated Windows environment.

### VM Configuration

- OS: Windows 10 Enterprise (64-bit)
- RAM: 4 GB
- Disk: 100 GB
- Initial Network: NAT (required for installation and downloads)
- Final Network: Internal Network connected to `ISOLATED` (`LAN 3` in the reference guide)

### Installation

1. Download the Windows 10 Enterprise ISO from Microsoft Evaluation Center.
2. Create a VirtualBox VM and install Windows 10.
3. Install VirtualBox Guest Additions.
4. Take **Snapshot 1** before modifying Windows security settings.
5. Pause Windows Updates and configure Windows Defender as required by the Flare VM installation guide.
6. Rename the VM using a hostname without spaces or special characters.
7. Take **Snapshot 2**.
8. Download and run the official Flare VM installation script from the [Mandiant Flare VM repository](https://github.com/mandiant/flare-vm).
9. Select the `debloat.vm` package during customization, as specified in the reference guide.
10. Restart the VM after installation completes.
11. Install the Windows **OpenSSH Server** feature for transferring files from other lab VMs.
12. Shut down the VM and change its network from NAT to the intended `ISOLATED` Internal Network.
13. Take **Snapshot 3** after completing the setup.

**Security Note:** Disabling Defender and Windows Updates reduces protection. Use Flare VM only in an isolated lab, and review scripts before executing them.
<img width="1324" height="721" alt="1" src="https://github.com/user-attachments/assets/cd73e713-0b37-4fb0-b925-b88b9a350344" />


## 2. REMnux VM Setup

**Purpose:** Analyze malware, inspect suspicious files, and perform Linux-based malware analysis.

### VM Configuration

- OS: REMnux virtual appliance
- RAM: 4 GB
- Import Format: `.ova`
- Network: Internal Network connected to `ISOLATED` (`LAN 3` in the reference guide)

### Installation

1. Download the official REMnux virtual appliance from the [REMnux documentation](https://docs.remnux.org/install-distro/get-virtual-appliance).
2. Import the `.ova` file into VirtualBox.
3. Assign 4 GB RAM and generate new MAC addresses during import.
4. Move the VM into the `Home Lab/Malware Analysis` group.
5. Verify the boot order and update VirtualBox Guest Additions.
6. Open a terminal and update REMnux packages:

   ```bash
   remnux upgrade
   ```
   <img width="802" height="724" alt="2" src="https://github.com/user-attachments/assets/0e4c6fe2-27a5-4b3b-8c60-de041263c7f8" />


7. Restart the VM.
8. Shut down the VM and connect it to the `ISOLATED` Internal Network.
9. Take a snapshot of the configured VM.

## 3. Network Isolation

Both Flare VM and REMnux are placed on the isolated malware-analysis network after installation.

- Use NAT temporarily when Internet access is needed for initial setup.
- Switch to the intended `ISOLATED` network after installation.
- Verify the VirtualBox Internal Network name matches the network connected to the pfSense `ISOLATED` interface.
- Confirm pfSense firewall rules prevent unwanted access to other lab networks and the Internet.
- Use snapshots to restore clean states after analysis.

## 4. Learning Objectives

- Windows-based malware analysis with Flare VM.
- Linux-based malware analysis with REMnux.
- Safe handling and transfer of suspicious files.
- Network isolation and virtual machine snapshot management.
- Preparation for malware-analysis and SOC investigation exercises.
---
---

# 7. DFIR Lab Setup — Tsurugi Linux  

## 1. Tsurugi Linux VM Setup

**Purpose:** Digital Forensics and Incident Response (DFIR).

### VM Configuration

- OS: Tsurugi Linux ISO
- RAM: 4 GB
- Disk: 150 GB (the reference guide states that at least 110 GB is required)
- Network: Internal Network — `LAN 4`
- VirtualBox group: `Home Lab/Security`

### Installation

1. Download the ISO from the [official Tsurugi Linux website](https://tsurugi-linux.org/).
2. Create a new VirtualBox VM and attach the ISO.
3. Configure 4 GB RAM and a 150 GB virtual disk.
4. Set the boot order to Hard Disk first, Optical second; disable Floppy.
5. For Tsurugi Linux 2024.1 or later, enable **System → Motherboard → Enable EFI**.
6. Connect Adapter 1 to the `LAN 4` Internal Network.
7. Start the VM and select GUI mode.
8. Set the display resolution to `1600x1050` before starting the installer.
9. Install Tsurugi Linux, configure the user account, and restart.

### Post-Installation

Install VirtualBox Guest Additions:

```bash
sudo ./VBoxLinuxAdditions.run
```

Update the system:

```bash
sudo apt update && sudo apt full-upgrade
```

Restart the VM after updates and verify that the system works correctly.

## 2. Snapshot and Verification

- Shut down Tsurugi Linux and create a VirtualBox snapshot.
- Verify that the VM receives an IP address in `10.10.10.0/24`.
- Confirm that its default gateway is `10.10.10.1`.
- Test only the network connectivity permitted by your pfSense rules.

## 3. Learning Objectives

- Digital forensics and incident response (DFIR).
- Forensic investigation using Tsurugi's preconfigured tools.
- Network segmentation and firewall rule management.
- Preparing a dedicated security subnet for future SOC tools, including Splunk.

**Next Step:** Set up Ubuntu and Splunk SIEM, then install the Splunk Universal Forwarder on the Active Directory Domain Controller to collect Windows security events.
<img width="746" height="713" alt="1" src="https://github.com/user-attachments/assets/f14e2733-8b73-4dd3-a7e5-9fe78dc9c799" />  
<img width="801" height="721" alt="1" src="https://github.com/user-attachments/assets/58927030-441f-4c3d-8f4d-0a12dd19f1af" />
---
---










 



