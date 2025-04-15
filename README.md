# AlienVault OSSIM Implementation
*A comprehensive guide to deploying an open-source SIEM solution with distributed architecture*

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
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
- [Testing & Validation](#testing--validation)
- [Use Cases](#use-cases)
- [Resources](#resources)

## 🔍 Overview
This project documents the implementation of AlienVault OSSIM (Open Source Security Information and Event Management), a comprehensive SIEM solution for threat detection, incident response, and compliance management. The implementation follows a distributed architecture with separate server and sensor components to demonstrate enterprise-grade deployment scenarios.

## 🏗️ Architecture

![OSSIM Architecture Diagram](images/image0.png)

The implementation consists of:
- **OSSIM Server** (Central management, correlation, and dashboard)
- **OSSIM Sensor** (Distributed monitoring component with NIDS capabilities)
- **Ubuntu Web Server** (Target system generating logs)
- **Kali Linux VM** (Attack simulation platform)

## 📝 Prerequisites
- **Hardware Requirements**:
  - Minimum 16GB RAM recommended for the host system
  - 100GB+ available disk space
  - Quad-core CPU or better
  
- **Software Requirements**:
  - VirtualBox (latest version)
  - AlienVault OSSIM ISO ([Download Link](https://cybersecurity.att.com/products/ossim/download))
  - Ubuntu Server ISO ([Download Link](https://ubuntu.com/download/server))
  - Kali Linux VirtualBox VM ([Download Link](https://www.kali.org/get-kali/))

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

![host only](Images/image1.png)

   - Don't forget to configure DHCP Server

![dhcp](Images/image2.png)


### OSSIM Server Deployment
Detailed steps to install and configure the OSSIM server VM:

1. **Create the VM**:
   - Name: ossim-server
   - OS: Debian (64-bit)
   - Memory: 4GB+ RAM
   - CPU: 2-3 processors
   - Storage: 25GB
> 💡 Tip: Skip adding the ISO when creating the VM. We'll mount it manually later.

2. **Installation Process**:</br>
Power on, Then follow:

- 
![host only](Images/image3.png)

- 
![host only](Images/image4.png)

- 
![host only](Images/image5.png)

- 
![host only](Images/image6.png)

- 
![host only](Images/image7.png)

- 
![host only](Images/image8.png)

- 
![host only](Images/image9.png)

- 
![host only](Images/image10.png)

- 
![host only](Images/image11.png)

- 
![host only](Images/image12.png)

- 
![host only](Images/image13.png)

- 
![host only](Images/image14.png)

- 
![host only](Images/image15.png)

- 
![host only](Images/image16.png)

3. **Post-Installation Configuration**:

- 
![host only](Images/image17.png)

- 
![host only](Images/image18.png)

- 
![host only](Images/image19.png)

### OSSIM Sensor Deployment
Steps to install and configure the OSSIM sensor VM:

1. **Create the VM**:
   - Name: ossim-sensor
   - OS: Debian (64-bit)
   - Memory: 4GB+ RAM
   - CPU: 2 processors
   - Storage: 15GB
> 💡 Tip: Skip adding the ISO when creating the VM. We'll mount it manually later.

2. **Installation Process**:</br>
Power on, Then follow:

- 
![host only](Images/image20.png)

- 
![host only](Images/image21.png)

- 
![host only](Images/image22.png)

- 
![host only](Images/image23.png)

- 
![host only](Images/image24.png)

- 
![host only](Images/image25.png)

- 
![host only](Images/image26.png)

- 
![host only](Images/image27.png)

- 
![host only](Images/image28.png)

- 
![host only](Images/image29.png)

- 
![host only](Images/image30.png)

- 
![host only](Images/image31.png)

- 
![host only](Images/image32.png)

- 
![host only](Images/image33.png)

3. **Network Interface Configuration**:
   - NIC1: Host-only Adapter

![host only](Images/image34.png)

   - NIC2: Host-only Adapter with Promiscuous Mode set to "Allow All"

![host only](Images/image35.png)


### Additional Server And Sensor Configuration
Run both machines together.

![host only](Images/image35.png)


1. **Server-Sensor Connectivity Test**:

- 
![host only](Images/image37.png)

- 
![host only](Images/image38.png)

- 
![host only](Images/image39.png)


2. **Server Configuration**:

- 
![host only](Images/image40.png)

- 
![host only](Images/image41.png)

- 
![host only](Images/image42.png)

- 
![host only](Images/image43.png)

- 
![host only](Images/image44.png)

- 
![host only](Images/image45.png)

- 
![host only](Images/image46.png)

- 
![host only](Images/image47.png)

- 
![host only](Images/image48.png)

- 
![host only](Images/image49.png)

- 
![host only](Images/image50.png)

- 
![host only](Images/image51.png)

- 
![host only](Images/image52.png)


3. **Sensor Configuration**:

- 
![host only](Images/image66(52+1).png)

- 
![host only](Images/image67(52+2).png)

- 
![host only](Images/image68(52+3).png)

- 
![host only](Images/image69(52+4).png)

- 
![host only](Images/image53.png)

- 
![host only](Images/image54.png)

- 
![host only](Images/image55.png)

- 
![host only](Images/image56.png)

- 
![host only](Images/image57.png)

- 
![host only](Images/image58.png)

- 
![host only](Images/image59.png)

- 
![host only](Images/image60.png)

- 
![host only](Images/image61.png)

- 
![host only](Images/image62.png)

- 
![host only](Images/image63.png)

- 
![host only](Images/image64.png)

- 
![host only](Images/image65.png)

### Target Systems Setup

#### Ubuntu Web Server
1. **Create the VM**:
   - Name: web-server
   - OS: Ubuntu Server (64-bit)
   - Memory: 4GB RAM
   - CPU: 2 processors
   - Storage: 15GB fixed disk

2. **Installation Process**:<br>
Follow default steps.

- 
![host only](Images/image70.png)

- 
![host only](Images/image71.png)

3. **Install and Configure Apache**:
   ```bash
   sudo apt update
   sudo apt install apache2
   sudo ufw allow 'Apache'
   sudo ufw allow 'OpenSSH'
   sudo ufw enable
   ```

4. **Change network mode to Host-Only Adapter**

5. **Configure Static IP**:
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
   - Rename to "Kali-OSSIM-Lab"

2. **Update the System**:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

3. **Network Configuration**:
   - After you updated the system, You can change network mode to Host-Only Adapter.

## ⚙️ Configuration
Turn on all the VMs

![host only](Images/image72.png)

### Server Configuration
1. **Access the Web Interface**:
   - Navigate to https://192.168.100.150
   - Create admin account

![host only](Images/image73.png)

2. **Environment Setup via Wizard**:
   - Add hosts:
     - OSSIM server
     - Sensor
     - Kali machine
     - Web server
   - Assign correct OS type
   - Deploy HIDS agents

3. **Screenshots**:

- 
![host only](Images/image74.png)

- 
![host only](Images/image75.png)

- 
![host only](Images/image76.png)

- 
![host only](Images/image77.png)

- 
![host only](Images/image78.png)

- 
![host only](Images/image79.png)

- 
![host only](Images/image80.png)

- 
![host only](Images/image81.png)

- 
![host only](Images/image82.png)

- 
![host only](Images/image83.png)

### Sensor Configuration
1. **Verify Sensor Connection**:
   - On server, click `Insert` under **Configuration** → **Deployment** → **Components** → **SENSORS**

![host only](Images/image84.png)

   - Configure the sensor (192.168.100.151) to be added.

![host only](Images/image85.png)

![host only](Images/image86.png)

2. **Configure Detection Settings**:

- Click on `system detail` icon.

![host only](Images/image87.png)

- Click `Sensor Configuration`.

![host only](Images/image88.png)

- Click `Detection`.

![host only](Images/image89.png)

- Make sure you have the same.

![host only](Images/image90.png)

3. **Detection Test**:

- Search `nikto` under **Configuration** → **THREAT INTELLIGENCE** → **DIRECTIVES**

![host only](Images/image91.png)

- Clone Directive.

![host only](Images/image92.png)

- Clik on `+` button beside `!HOME_NET` (FROM column) to modify:

![host only](Images/image93.png)

- In source section replace `!HOME_NET` with `HOME_NET`, Then click `MODIFY`.

![host only](Images/image94.png)

- `Reload Directive` to save.

![host only](Images/image95.png)

- On OssimSensor terminal (`Jailbreak System` option)
   ```bash
   nano /etc/suricata/suricata.yaml
   ```
   - Set `EXTERNAL_NET: any`.

![host only](Images/image96.png)

   - Save and exit.
   - Restart suricata service
     ```bash
     service suricata restart
     ```
  - Exit terminal
- On Kali VM
  ```bash
  sudo nikto -h 192.168.100.200
  ```
- And There we have it! We found our sercurity events under

![host only](Images/image97.png)

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
   ```bash
   # On the sever
   sudo tcpdump -i eth0 port 514
   ```
  - From Kali VM try to connect to the webserver via SSH:

![host only](Images/image98.png)

  - Ossim is recieving logs from syslog:

![host only](Images/image99.png)

  - Alerts recieved Also on our siem interface

![host only](Images/image100.png)


## 🧪 Testing & Validation

### Basic Connectivity Tests
```bash
# Test connectivity between all systems
ping 192.168.100.150
ping 192.168.100.151
ping 192.168.100.200
ping 192.168.100.102
```

### Simulated Attack Scenarios
1. **Web Scanning Detection**:
   ```bash
   # From Kali Linux
   nikto -h 192.168.100.200
   ```

2. **Brute Force Detection**:
   ```bash
   # From Kali Linux
   hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.100.200 http-post-form
   ```

3. **Alert Verification**:
   - Screenshot of triggered alarms in OSSIM console
   - Analysis of event details

## 📊 Use Cases

### 1. Security Monitoring
Demonstrate how the SIEM solution provides real-time visibility into security events:
- Dashboard configuration
- Alert monitoring
- Incident response workflow

### 2. Compliance Reporting
Show how OSSIM helps meet compliance requirements:
- Available reports
- Custom report creation
- Scheduled reporting

### 3. Threat Intelligence Integration
Document how threat intelligence can be integrated:
- OTX setup (when internet is available)
- Custom threat feeds
- Intelligence correlation

## 📚 Resources
- [Official AlienVault Documentation](https://cybersecurity.att.com/documentation)
- [Community Forums](https://community.alienvault.com/)
- [OSSIM GitHub Repository](https://github.com/ossim/ossim)
