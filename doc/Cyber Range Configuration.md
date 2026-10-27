# Cyber Range Configuration

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
