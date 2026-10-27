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
