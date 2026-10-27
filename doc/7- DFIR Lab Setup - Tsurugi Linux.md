# DFIR Lab Setup — Tsurugi Linux  

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

<img width="746" height="713" alt="1" src="https://github.com/user-attachments/assets/f14e2733-8b73-4dd3-a7e5-9fe78dc9c799" />  
<img width="801" height="721" alt="1" src="https://github.com/user-attachments/assets/58927030-441f-4c3d-8f4d-0a12dd19f1af" />
<img width="995" height="834" alt="Tsu" src="https://github.com/user-attachments/assets/ad991eef-170c-4194-aba1-a13e46012093" />
