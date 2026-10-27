# Active Directory Lab Setup

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

**Lab Result:** One Windows Server 2022 Domain Controller and two Windows clients joined to the `ad.lab` domain, ready for further security monitoring and detection exercises.   
