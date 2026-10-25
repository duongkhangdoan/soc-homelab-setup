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
## <span style="color: red;">1. pfSense Configuration
pfSense is the gateway (router) and firewall for the lab. pfSense should always be the first VM that is booted when using the lab. If pfSense is not booted the machines in the lab will not be able to access the internet. Once pfSense is up other VMs can be launched.  
### Download pfSense

Go to the following link: [pfSense CE Download](https://filippo.io/mirror/downloads/)
As of writing the latest version of pfSense is `2.7.2`
Download the `amd64` version `ISO` of the latest version available


