# AlienVault OSSIM Implementation

A comprehensive guide to deploying an open-source SIEM solution with distributed architecture

<p align="center">
  <img src="Images/LOGO.jpg" width="600" alt="AlienVault OSSIM Logo" />
</p>

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Installation Guide](#installation-guide)
  - [Network Setup](#network-setup)
  - [OSSIM Server Deployment](#ossim-server-deployment)
  - [OSSIM Sensor Deployment](#ossim-sensor-deployment)
  - [Target Systems Setup](#target-systems-setup)
- [Configuration](#configuration)
  - [Server Configuration](#server-configuration)
  - [Sensor Configuration](#sensor-configuration)
  - [Log Forwarding Setup](#log-forwarding-setup)
  - [Detection Rules](#detection-rules)
- [Use Cases](#use-cases)
- [Resources](#resources)

## 🔍 Overview

This project documents the implementation of AlienVault OSSIM (Open Source Security Information and Event Management), a comprehensive SIEM solution for threat detection, incident response, and compliance management. The implementation follows a distributed architecture with separate server and sensor components to demonstrate enterprise-grade deployment scenarios.

## 🏗️ Architecture

```mermaid
graph LR
    B[OSSIM Sensor<br>192.168.100.151] -->|Forwards Events| A[OSSIM Server<br>192.168.100.150]
    C[Web Server<br>192.168.100.200] -->|Logs| A
    D[Kali Linux<br>192.168.100.102] -->|Attacks| C
    C -->|Network Monitoring| B
```
**How It Works**
- **Sensor**: Watches traffic, detects attacks (e.g., scans from Kali), sends alerts to Server.
- **Server**: Analyzes alerts and logs to identify threats.
- **Web Server**: Sends logs (e.g., SSH, HTTP) to Server; monitored by Sensor.
- **Kali**: Tests the system with attacks.

**Key Features**
- **Monitoring**: Sensor flags suspicious traffic.
- **Logs**: Web Server forwards logs to Server.
- **Visibility**: Server combines data for threat detection.

**Software Requirements**:
- VirtualBox (latest version)
- AlienVault OSSIM ISO 
  - Official Download: [AT&T Cybersecurity](https://cybersecurity.att.com/products/ossim/download)  
  > ⚠️ Note: Versions 5.8.14 and later of the official ISO are known to hang or fail during the `alienvault-gvm11-feed` step, leading to incomplete installs and missing configuration files :contentReference[oaicite:0]{index=0}.  
  - **Use this stably tested in our lab ISO** : [Download alienvault-ossim-custom.iso](https://github.com/aymenmarjan/Implementing-AlienVault-OSSIM-with-Distributed-Architecture/releases/download/v1.0/alienvault-ossim-custom.iso)
- Ubuntu Server ISO ([Download Link](https://ubuntu.com/download/server))
- Kali Linux VirtualBox VM ([Download Link](https://www.kali.org/get-kali/#kali-virtual-machines))

## 🚀 Installation Guide

### Network Setup

The lab environment uses a host-only network (`192.168.100.0/24`) for isolation and security.

| VM | IP Address | Function |
|----|------------|----------|
| OSSIM Server | 192.168.100.150 | Central management |
| OSSIM Sensor | 192.168.100.151 | Distributed monitoring |
| Ubuntu Web Server | 192.168.100.200 | Target system |
| Kali Linux | 192.168.100.102 | Attack simulation |

#### Steps to create Host-Only Network in VirtualBox:

1. Open VirtualBox
2. Navigate to File → Host Network Manager
3. Create a new network with the following settings:
   - IPv4 Address: 192.168.100.1
   - Network Mask: 255.255.255.0

<p align="center">
  <img src="Images/image1.png" alt="host only" width="600"/>
</p>

   - Don't forget to configure DHCP Server

<p align="center">
  <img src="Images/image2.png" alt="dhcp" width="600"/>
</p>

### OSSIM Server Deployment

Detailed steps to install and configure the OSSIM server VM:

1. **Create the VM**:
   - Name: ossim-server
   - OS: Debian (64-bit)
   - Memory: 4GB+ RAM
   - CPU: 2-3 processors
   - Storage: 25GB
   
   > 💡 **Tip**: Skip adding the ISO when creating the VM. We'll mount it manually later.

2. **Installation Process**:

   Power on, Then follow:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Boot from ISO Image** | <img src="Images/image3.png" alt="iso" width="400"/> |
   | **Select Server Deployment Mode** | <img src="Images/image4.png" alt="server" width="400"/> |
   | **Configure System Language** | <img src="Images/image5.png" alt="language" width="400"/> |
   | **Set Geographic Location** | <img src="Images/image6.png" alt="location" width="400"/> |
   | **Configure Keyboard Layout** | <img src="Images/image7.png" alt="keyboard" width="400"/> |
   | **Configure Network: IP Address** (Use `192.168.100.150`) | <img src="Images/image8.png" alt="network ip" width="400"/> |
   | **Configure Network: Subnet Mask** (Leave default) | <img src="Images/image9.png" alt="network mask" width="400"/> |
   | **Configure Network: Gateway** (Use `192.168.100.1`) | <img src="Images/image10.png" alt="network gateway" width="400"/> |
   | **Configure Network: DNS Server** (Use `192.168.100.1`) | <img src="Images/image11.png" alt="network dns" width="400"/> |
   | **Set Root Password** | <img src="Images/image12.png" alt="password" width="400"/> |
   | **Select Time Zone** | <img src="Images/image13.png" alt="timezone" width="400"/> |
   | **Wait..** | <img src="Images/image14.png" alt="progress1" width="400"/> |
   | **Wait more...** | <img src="Images/image15.png" alt="progress2" width="400"/> |
   | **Installation Complete** | <img src="Images/image16.png" alt="complete" width="400"/> |
   
   </div>

3. **Post-Installation Configuration**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Login and Shutdown** (as we set before: `root:root`) | <img src="Images/image18.png" alt="shutdown" width="400"/> |
   | **Configure VM Network Adapter** (Change to `Host-Only Adapter`) | <img src="Images/image19.png" alt="network-config" width="400"/> |
   
   </div>

### OSSIM Sensor Deployment

Steps to install and configure the OSSIM sensor VM:

1. **Create the VM**:
   - Name: ossim-sensor
   - OS: Debian (64-bit)
   - Memory: 4GB+ RAM
   - CPU: 2 processors
   - Storage: 15GB
   
   > 💡 **Tip**: Skip adding the ISO when creating the VM. We'll mount it manually later.

2. **Installation Process**:

   Power on, Then follow:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Boot from ISO Image** | <img src="Images/image20.png" alt="boot-sensor" width="400"/> |
   | **Select Sensor Installation Mode** | <img src="Images/image21.png" alt="sensor-mode" width="400"/> |
   | **Configure System Language** | <img src="Images/image22.png" alt="sensor-language" width="400"/> |
   | **Set Geographic Location** | <img src="Images/image23.png" alt="sensor-location" width="400"/> |
   | **Configure Keyboard Layout** | <img src="Images/image24.png" alt="sensor-keyboard" width="400"/> |
   | **Configure Network: IP Address** (Use `192.168.100.151`) | <img src="Images/image25.png" alt="sensor-ip" width="400"/> |
   | **Configure Network: Subnet Mask** (Leave default) | <img src="Images/image26.png" alt="sensor-mask" width="400"/> |
   | **Configure Network: Gateway** (Use `192.168.100.1`) | <img src="Images/image27.png" alt="sensor-gateway" width="400"/> |
   | **Configure Network: DNS Server** (Use `192.168.100.1`) | <img src="Images/image28.png" alt="sensor-dns" width="400"/> |
   | **Set Root Password** | <img src="Images/image29.png" alt="sensor-password" width="400"/> |
   | **Select Time Zone** | <img src="Images/image30.png" alt="sensor-timezone" width="400"/> |
   | **Wait..** | <img src="Images/image31.png" alt="sensor-progress1" width="400"/> |
   | **Wait more...** | <img src="Images/image15.png" alt="progress2" width="400"/> |
   | **Installation Complete** | <img src="Images/image32.png" alt="sensor-complete" width="400"/> |
   
   </div>

4. **Network Interface Configuration**:

   After you shut it down, you should configure 2 interfaces as:

   <div align="center">
   
   | Configuration | Screenshot |
   |---------------|------------|
   | **NIC1: Host-only Adapter** | <img src="Images/image34.png" alt="host only" width="400"/> |
   | **NIC2: Host-only Adapter with Promiscuous Mode set to "Allow All"** | <img src="Images/image35.png" alt="host only" width="400"/> |
   
   </div>

### Additional Server And Sensor Configuration

Run both machines together.

<p align="center">
  <img src="Images/image36.png" alt="both VMs running" width="600"/>
</p>

1. **Server-Sensor Connectivity Test**:
   
   From The `Ossim-Server` machine (or the opposite):

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Access System Shell** (Select "Jailbreak System") | <img src="Images/image37.png" alt="jailbreak" width="400"/> |
   | **Confirm Shell Access** | <img src="Images/image38.png" alt="shell-confirm" width="400"/> |
   | **Test Network Connectivity** (Ping on Sensor) | <img src="Images/image39.png" alt="ping-test" width="400"/> |
   
   </div>

2. **Server Configuration**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Boot Server VM** | <img src="Images/image40.png" alt="boot-server" width="400"/> |
   | **Access Hostname Configuration** | <img src="Images/image41.png" alt="hostname-menu" width="400"/> |
   | **Set System Hostname** (Use `Ossimserver`) | <img src="Images/image42.png" alt="set-hostname" width="400"/> |
   | **Confirm Hostname Change** | <img src="Images/image43.png" alt="hostname-confirm" width="400"/> |
   | **Return to Main Menu** | <img src="Images/image44.png" alt="main-menu" width="400"/> |
   | **Access Sensor Configuration** | <img src="Images/image45.png" alt="sensor-config" width="400"/> |
   | **Configure Data Sources** | <img src="Images/image46.png" alt="data-sources" width="400"/> |
   | **Enable Syslog Collection** | <img src="Images/image47.png" alt="syslog" width="400"/> |
   | **Return to Main Menu** | <img src="Images/image48.png" alt="return-menu" width="400"/> |
   | **Apply Configuration Changes** | <img src="Images/image49.png" alt="apply-config" width="400"/> |
   | **Confirm Configuration Update** | <img src="Images/image50.png" alt="config-confirm" width="400"/> |
   | **Wait for Configuration Process** | <img src="Images/image51.png" alt="config-wait" width="400"/> |
   | **Complete Configuration** | <img src="Images/image52.png" alt="config-complete" width="400"/> |
   
   </div>

3. **Sensor Configuration**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Boot Sensor VM** | <img src="Images/image66(52+1).png" alt="sensor-boot" width="400"/> |
   | **Access Hostname Configuration** | <img src="Images/image67(52+2).png" alt="hostname-menu" width="400"/> |
   | **Set the Hostname as `OssimSensor`** | <img src="Images/image68(52+3).png" alt="set-sensor-hostname" width="400"/> |
   | **Confirm Hostname Change** | <img src="Images/image69(52+4).png" alt="hostname-confirm" width="400"/> |
   | **Access Sensor Configuration Menu** | <img src="Images/image53.png" alt="sensor-config" width="400"/> |
   | **Configure Data Source Plugins** | <img src="Images/image54.png" alt="data-plugins" width="400"/> |
   | **Enable Syslog Collection** | <img src="Images/image55.png" alt="syslog-enable" width="400"/> |
   | **Configure AlienVault Server IP** | <img src="Images/image56.png" alt="server-ip-option" width="400"/> |
   | **Set Server IP Address** (Enter `192.168.100.150`) | <img src="Images/image57.png" alt="set-server-ip" width="400"/> |
   | **Configure AlienVault Framework IP** | <img src="Images/image58.png" alt="framework-ip-option" width="400"/> |
   | **Set Framework IP Address** (Enter `192.168.100.150`) | <img src="Images/image59.png" alt="set-framework-ip" width="400"/> |
   | **Configure Network Monitoring** | <img src="Images/image60.png" alt="network-monitoring" width="400"/> |
   | **Select Monitoring Interface** (Choose `eth1` with Promiscuous Mode) | <img src="Images/image61.png" alt="select-interface" width="400"/> |
   | **Return to Main Menu** | <img src="Images/image62.png" alt="main-menu" width="400"/> |
   | **Apply Configuration Changes** | <img src="Images/image63.png" alt="apply-changes" width="400"/> |
   | **Confirm Configuration Update** | <img src="Images/image64.png" alt="confirm-update" width="400"/> |
   | **Wait for Configuration Process** | <img src="Images/image65.png" alt="config-process" width="400"/> |
   
   </div>

### Target Systems Setup

#### Ubuntu Web Server

1. **Create the VM**:
   - Name: web-server
   - OS: Ubuntu Server (64-bit)
   - Memory: 4GB RAM
   - CPU: 2 processors
   - Storage: 15GB fixed disk

2. **Installation Process**:
> Follow default steps

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Configure User Profile and Credentials** | <img src="Images/image70.png" alt="profile-setup" width="400"/> |
   | **Installation Complete and System Ready** | <img src="Images/image71.png" alt="system-ready" width="400"/> |
   
   </div>

4. **Install and Configure Apache**:

   ```bash
   sudo apt update
   sudo apt install apache2
   sudo ufw allow 'Apache'
   sudo ufw allow 'OpenSSH'
   sudo ufw enable
   ```

5. **Change network mode to Host-Only Adapter**:
   
   From now on, we won't need the Internet on this virtual machine. Power it off and change its networking mode to Host-Only Adapter.

6. **Configure Static IP**:

   ```bash
   # Edit Netplan configuration
   sudo nano /etc/netplan/00-installer-config.yaml
   ```
   
   Example configuration:
   
   ```yaml
   network:
     ethernets:
       enp0s3:
         addresses: [192.168.100.200/24]
         gateway4: 192.168.100.1
     version: 2
   ```

#### Kali Linux VM

1. **Import Pre-built Image**:
   - Add the Kali .vbox file to VirtualBox
   - Rename to "Kali-2023-VM"

2. **Update the System**:

   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

3. **Network Configuration**:
   
   After you updated the system, You can change network mode to Host-Only Adapter.

## ⚙️ Configuration

Turn on all the VMs

<p align="center">
  <img src="Images/image72.png" alt="All VMs running" width="600"/>
</p>

### Server Configuration

1. **Access the Web Interface**:
   - Navigate to https://192.168.100.150
   - Create admin account

   <p align="center">
     <img src="Images/image73.png" alt="Web interface" width="600"/>
   </p>

2. **Environment Setup via Wizard**:
   - Add hosts:
     - OSSIM server
     - Sensor
     - Kali machine
     - Web server
   - Assign correct OS type
   - Deploy HIDS agents
  
3. **Steps**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Start Configuration Wizard** | <img src="Images/image74.png" alt="start-wizard" width="400"/> |
   | **Proceed to Next Step** | <img src="Images/image75.png" alt="next-step" width="400"/> |
   | **Add the missing machines, Select every machine's OS** | <img src="Images/image76.png" alt="asset-discovery" width="400"/> |
   | **OSSIM Server & Sensor Credentials** (`root:root`) | <img src="Images/image77.png" alt="credentials" width="400"/> |
   | **Web Server Credentials** (`marjan:123`) | <img src="Images/image78.png" alt="Web-credentials" width="400"/> |
   | **Skip LOG MANAGEMENT** (Internet access required) | <img src="Images/image79.png" alt="skip-log-management" width="400"/> |
   | **Skip JOIN OTX** (Internet access required) | <img src="Images/image80.png" alt="skip-additional" width="400"/> |
   | **Complete the Configuration Wizard** | <img src="Images/image81.png" alt="finish-setup" width="400"/> |
   | **Launch OSSIM Dashboard** | <img src="Images/image82.png" alt="launch-dashboard" width="400"/> |
   | **OSSIM Web Interface Ready for Use** | <img src="Images/image83.png" alt="dashboard-ready" width="400"/> |
   
   </div>

### Sensor Configuration

1. **Verify Sensor Connection**:
   
   - On server, click `Insert` under **Configuration** → **Deployment** → **Components** → **SENSORS**

   <p align="center">
     <img src="Images/image84.png" alt="Insert sensor" width="600"/>
   </p>

   - Configure the sensor (192.168.100.151) to be added.

   <p align="center">
     <img src="Images/image85.png" alt="Configure sensor" width="600"/>
   </p>

   <p align="center">
     <img src="Images/image86.png" alt="Sensor added" width="600"/>
   </p>

2. **Configure Detection Settings**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **Click on `system detail` icon** | <img src="Images/image87.png" alt="System detail" width="400"/> |
   | **Click `Sensor Configuration`** | <img src="Images/image88.png" alt="Sensor config" width="400"/> |
   | **Click `Detection`** | <img src="Images/image89.png" alt="Detection" width="400"/> |
   | **Verify Detection Settings** (Make sure you have the same) | <img src="Images/image90.png" alt="Detection settings" width="400"/> |
   
   </div>

3. **Detection Test**:

   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | Search `nikto` under **Configuration** → **THREAT INTELLIGENCE** → **DIRECTIVES** | <img src="Images/image91.png" alt="Search nikto" width="400"/> |
   | **Clone Directive** | <img src="Images/image92.png" alt="Clone directive" width="400"/> |
   | **Click on `+` button beside `!HOME_NET` (`FROM` column)** | <img src="Images/image93.png" alt="Modify directive" width="400"/> |
   | **In source section replace `!HOME_NET` with `HOME_NET`, Then click `MODIFY`** | <img src="Images/image94.png" alt="Replace network" width="400"/> |
   | **Reload Directive to save** | <img src="Images/image95.png" alt="Reload directive" width="400"/> |
   
   </div>

**On OssimSensor terminal (`Jailbreak System` option)**

 ```bash
  nano /etc/suricata/suricata.yaml
 ```
**Set `EXTERNAL_NET: any`, Save and exit.**

   <p align="center">
     <img src="Images/image96.png" alt="Suricata file" width="600"/>
   </p>

**Restart suricata service, Exit terminal.**

 ```bash
  service suricata restart
 ```

**On Kali VM**

  ```bash
   sudo nikto -h 192.168.100.200
  ```

**And There we have it! We found our sercurity events under **ANALYSIS** → **ALARMS****

   <p align="center">
     <img src="Images/image97.png" alt="events" width="600"/>
   </p>

### Log Forwarding Setup

1. **Configure Syslog on Web Server**:

   ```bash
   # Edit rsyslog configuration
   sudo nano /etc/rsyslog.d/50-default.conf
   ```

   Add the following line:
   
   ```
   *.* @192.168.100.150:514
   ```

2. **Restart Service**:

   ```bash
   sudo systemctl restart rsyslog
   ```

3. **Verify Log Forwarding**:

   **On the server**:
   
   ```bash
   sudo tcpdump -i eth0 port 514
   ```
   
   <div align="center">
   
   | Step | Screenshot |
   |------|------------|
   | **SSH connection attempt from Kali VM** | <img src="Images/image98.png" alt="SSH attempt" width="400"/> |
   | **Logs received via syslog** | <img src="Images/image99.png" alt="Logs received" width="400"/> |
   | **Logs recieved Also on our SIEM interface (Under `ANALYSIS` → `SECURITY EVENTS (SIEM)`)** | <img src="Images/image100.png" alt="SIEM alerts" width="400"/> |
   
   </div>

## 📊 Use Cases
OSSIM supports multiple security monitoring use cases:

- **Real-time Monitoring:** Tracks network and system events, provides alerts for suspicious activity, and helps manage incident response.
- **Compliance Management:** Creates compliance reports, maintains audit logs, and documents security control performance.
- **Threat Intelligence Integration:** Uses OTX feeds to correlate events with global threats and detect new vulnerabilities.

## 📚 Resources

- [Official AlienVault Documentation](https://cybersecurity.att.com/documentation)
- [Youtube Playlist Comprehensive Tutorial](https://www.youtube.com/playlist?list=PLxTwjzMO9Zf5VdowToEnM_DdHDgAVtCem)
