# Splunk SIEM Setup & Configuration

### 1. Ubuntu VM Setup
Create an Ubuntu Desktop VM in VirtualBox to host Splunk Enterprise.

- **OS:** Ubuntu Desktop LTS (use a currently supported version)
- **RAM:** 4 GB
- **Disk:** 100 GB
- **Network:** Internal Network connected to the `SECURITY` subnet
- **Subnet:** `10.10.10.0/24`
- **Purpose:** Centralized log collection, searching, and security event monitoring

Install VirtualBox Guest Additions, update the system, and take a snapshot.

```bash
sudo apt update && sudo apt full-upgrade
```

### 2. Install Splunk Enterprise

Download the Linux `.deb` package from the official Splunk website: [Splunk Enterprise Free Trial | Splunk](https://www.splunk.com/en_us/download/splunk-enterprise.html)

Install the downloaded package. Replace the filename with the actual package name:

```bash
sudo apt install curl
cd ~/Downloads
sudo dpkg -i <splunk-package>.deb
```
<img width="1049" height="238" alt="1" src="https://github.com/user-attachments/assets/e3bdcc8a-978b-4f55-84e6-2be1e1837e7c" />


Start Splunk and accept the license:

```bash
sudo /opt/splunk/bin/splunk start --accept-license --answer-yes
```

Configure Splunk to start automatically at boot:

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

Access the Splunk Web interface:

`http://<SPLUNK_IP>:8000`

### 3. Configure Splunk Receiving Port

In Splunk Web:

1. Navigate to **Settings → Forwarding and receiving**.
2. Under **Receive data**, select **Add new**.
3. Configure the receiving port as `9997`.
4. Save the configuration.

Port `9997` receives forwarded log data from Splunk Universal Forwarders.
<img width="1062" height="188" alt="1" src="https://github.com/user-attachments/assets/37feaec7-be4f-4874-9a65-7753c042af25" />



### 4. Install Splunk Universal Forwarder on Windows Server

Install the Windows 64-bit Splunk Universal Forwarder on the Windows Server 2022 Domain Controller.

Download: [Download Universal Forwarder for Remote Data Collection | Splunk](https://www.splunk.com/en_us/download/universal-forwarder.html)

During installation, configure:

- **Deployment Server / Splunk management endpoint:** `<SPLUNK_IP>:8089` if prompted
- **Receiving Indexer:** `<SPLUNK_IP>:9997`
- Use appropriate credentials and complete the installation.

Find the Splunk server IP address on Ubuntu:

```bash
ip a
```
<img width="906" height="619" alt="ip a" src="https://github.com/user-attachments/assets/5e74805f-df2e-4b62-8a64-6294170bb234" />


Use the IP assigned to the Ubuntu VM's active network interface.

### 5. Configure Windows Event Log Ingestion

In Splunk Web:

1. Navigate to **Settings → Add Data → Forward**.
2. Select the Windows Server Universal Forwarder.
3. Select the Windows Event Logs to collect.
4. Create an index named `windows`.
5. Review the configuration and submit.

### 6. Verify Log Ingestion

Open **Apps → Search & Reporting** and run:

```spl
index="windows"
```
<img width="1657" height="1023" alt="2" src="https://github.com/user-attachments/assets/d6498005-32c9-42eb-a586-609405788340" />


If events do not appear immediately, wait briefly and check the Universal Forwarder connection, Windows Event Log inputs, firewall rules, and receiving port configuration.

### 7. Lab Integration

- **Log source:** Windows Server 2019 Domain Controller
- **Log collector:** Splunk Universal Forwarder
- **SIEM:** Splunk Enterprise on Ubuntu
- **Network:** SECURITY — `10.10.10.0/24`
- **Data flow:** Windows Event Logs → Universal Forwarder → Splunk Enterprise
- **Purpose:** Practice centralized logging, security event investigation, and SOC monitoring.

Take a VM snapshot after completing the setup.
