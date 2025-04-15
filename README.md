# AlienVault OSSIM Implementation
*A comprehensive guide to deploying an open-source SIEM solution with distributed architecture*

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

- **Boot from ISO Image**:
![iso](Images/image3.png)

- **Select Server Deployment Mode**:
![server](Images/image4.png)

- **Configure System Language**:
![language](Images/image5.png)

- **Set Geographic Location**:
![location](Images/image6.png)

- **Configure Keyboard Layout**:
![keyboard](Images/image7.png)

- **Configure Network: IP Address** (Use `192.168.100.150`):
![network ip](Images/image8.png)

- **Configure Network: Subnet Mask** (Leave default):
![network mask](Images/image9.png)

- **Configure Network: Gateway** (Use `192.168.100.1`):
![network gateway](Images/image10.png)

- **Configure Network: DNS Server** (Use `192.168.100.1`):
![network dns](Images/image11.png)

- **Set Root Password**:
![password](Images/image12.png) 

- **Select Time Zone**:
![timezone](Images/image13.png)

- Wait...
![progress1](Images/image14.png)

- Wait more...
![progress2](Images/image15.png)

- **Installation Complete**:
![complete](Images/image16.png)

3. **Post-Installation Configuration**:

- **Login and Shutdown** (Default credentials: `root:root`):
![shutdown](Images/image18.png)

- **Configure VM Network Adapter** (Change to `Host-Only Adapter`):
![network-config](Images/image19.png)

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

- **Boot from ISO Image**:
![boot-sensor](Images/image20.png)

- **Select Sensor Installation Mode**:
![sensor-mode](Images/image21.png)

- **Configure System Language**:
![sensor-language](Images/image22.png)

- **Set Geographic Location**:
![sensor-location](Images/image23.png)

- **Configure Keyboard Layout**:
![sensor-keyboard](Images/image24.png)

- **Configure Network: IP Address** (Use `192.168.100.151`):
![sensor-ip](Images/image25.png)

- **Configure Network: Subnet Mask** (Leave default):
![sensor-mask](Images/image26.png)

- **Configure Network: Gateway** (Use `192.168.100.1`):
![sensor-gateway](Images/image27.png)

- **Configure Network: DNS Server** (Use `192.168.100.1`):
![sensor-dns](Images/image28.png)

- **Set Root Password**:
![sensor-password](Images/image29.png)

- **Select Time Zone**:
![sensor-timezone](Images/image30.png)

- Wait...
![sensor-progress1](Images/image31.png)

- Wait more...
![progress2](Images/image15.png)

- **Installation Complete**:
![sensor-complete](Images/image32.png)

3. **Network Interface Configuration**:<br>
After you shut it down, you should configure 2 interfaces as:

   - NIC1: Host-only Adapter

![host only](Images/image34.png)

   - NIC2: Host-only Adapter with Promiscuous Mode set to "Allow All"

![host only](Images/image35.png)


### Additional Server And Sensor Configuration
Run both machines together.

![host only](Images/image36.png)


1. **Server-Sensor Connectivity Test**:<br>
From The `Ossim-Server` machine: (or the opposite)

- **Access System Shell** (Select "Jailbreak System"):
![jailbreak](Images/image37.png)

- **Confirm Shell Access**:
![shell-confirm](Images/image38.png)

- **Test Network Connectivity** (Ping on Sensor):
![ping-test](Images/image39.png)


2. **Server Configuration**:

- **Boot from ISO Image**:
![iso](Images/image3.png)

- **Select Server Deployment Mode**:
![server](Images/image4.png)

- **Configure System Language**:
![language](Images/image5.png)

- **Set Geographic Location**:
![location](Images/image6.png)

- **Configure Keyboard Layout**:
![keyboard](Images/image7.png)

- **Configure Network: IP Address** (Use `192.168.100.150`):
![network ip](Images/image8.png)

- **Configure Network: Subnet Mask** (Leave default):
![network mask](Images/image9.png)

- **Configure Network: Gateway** (Use `192.168.100.1`):
![network gateway](Images/image10.png)

- **Configure Network: DNS Server** (Use `192.168.100.1`):
![network dns](Images/image11.png)

- **Set Root Password**:
![password](Images/image12.png) 

- **Select Time Zone**:
![timezone](Images/image13.png)

- **Installation Progress** (First phase):
![progress1](Images/image14.png)

- **Installation Progress** (Final phase):
![progress2](Images/image15.png)

- **Installation Complete**:
![complete](Images/image16.png)

## 3. **Post-Installation Network Configuration**

- **Login and Shutdown** (Default credentials: `root:root`):
![shutdown](Images/image18.png)

- **Configure VM Network Adapter** (Change to `Host-Only Adapter`):
![network-config](Images/image19.png)

## OSSIM Sensor Deployment

### 1. **VM Creation and Resource Allocation**
   - Name: ossim-sensor
   - OS: Debian (64-bit)
   - Memory: 4GB+ RAM
   - CPU: 2 processors
   - Storage: 15GB
> 💡 Tip: Skip adding the ISO when creating the VM. We'll mount it manually later.

### 2. **Sensor Installation Process**

- **Boot from ISO Image**:
![boot-sensor](Images/image20.png)

- **Select Sensor Installation Mode**:
![sensor-mode](Images/image21.png)

- **Configure System Language**:
![sensor-language](Images/image22.png)

- **Set Geographic Location**:
![sensor-location](Images/image23.png)

- **Configure Keyboard Layout**:
![sensor-keyboard](Images/image24.png)

- **Configure Network: IP Address** (Use `192.168.100.151`):
![sensor-ip](Images/image25.png)

- **Configure Network: Subnet Mask** (Leave default):
![sensor-mask](Images/image26.png)

- **Configure Network: Gateway** (Use `192.168.100.1`):
![sensor-gateway](Images/image27.png)

- **Configure Network: DNS Server** (Use `192.168.100.1`):
![sensor-dns](Images/image28.png)

- **Set Root Password**:
![sensor-password](Images/image29.png)

- **Select Time Zone**:
![sensor-timezone](Images/image30.png)

- **Installation Progress** (First phase):
![sensor-progress1](Images/image31.png)

- **Installation Progress** (Final phase):
![sensor-progress2](Images/image32.png)

- **Installation Complete**:
![sensor-complete](Images/image33.png)

### 3. **Network Interface Configuration**

- **Configure Primary Interface** (NIC1: Host-only Adapter):
![primary-nic](Images/image34.png)

- **Configure Monitoring Interface** (NIC2: Host-only Adapter with Promiscuous Mode "Allow All"):
![monitor-nic](Images/image35.png)

## System Integration and Configuration

- **Launch Both VMs**:
![both-vms](Images/image35.png)

### 1. **Connectivity Verification**

- **Access System Shell** (Select "Jailbreak System"):
![jailbreak](Images/image37.png)

- **Confirm Shell Access**:
![shell-confirm](Images/image38.png)

- **Test Network Connectivity** (Ping between server and sensor):
![ping-test](Images/image39.png)

### 2. **OSSIM Server Configuration**

- **Boot Server VM**:
![boot-server](Images/image40.png)

- **Access Hostname Configuration**:
![hostname-menu](Images/image41.png)

- **Set System Hostname** (Use `Ossimserver`):
![set-hostname](Images/image42.png)

- **Confirm Hostname Change**:
![hostname-confirm](Images/image43.png)

- **Return to Main Menu**:
![main-menu](Images/image44.png)

- **Access Sensor Configuration**:
![sensor-config](Images/image45.png)

- **Configure Data Sources**:
![data-sources](Images/image46.png)

- **Enable Syslog Collection**:
![syslog](Images/image47.png)

- **Return to Main Menu**:
![return-menu](Images/image48.png)

- **Apply Configuration Changes**:
![apply-config](Images/image49.png)

- **Confirm Configuration Update**:
![config-confirm](Images/image50.png)

- **Wait for Configuration Process**:
![config-wait](Images/image51.png)

- **Complete Configuration**:
![config-complete](Images/image52.png)


3. **Sensor Configuration**:

- **Boot Sensor VM**:
![sensor-boot](Images/image66(52+1).png)

- **Access Hostname Configuration**:
![hostname-menu](Images/image67(52+2).png)

- **Set the Hostname as `OssimSensor`**:
![set-sensor-hostname](Images/image68(52+3).png)

- **Confirm Hostname Change**:
![hostname-confirm](Images/image69(52+4).png)

- **Access Sensor Configuration Menu**:
![sensor-config](Images/image53.png)

- **Configure Data Source Plugins**:
![data-plugins](Images/image54.png)

- **Enable Syslog Collection**:
![syslog-enable](Images/image55.png)

- **Configure AlienVault Server IP**:
![server-ip-option](Images/image56.png)

- **Set Server IP Address** (Enter `192.168.100.150`):
![set-server-ip](Images/image57.png)

- **Configure AlienVault Framework IP**:
![framework-ip-option](Images/image58.png)

- **Set Framework IP Address** (Enter `192.168.100.150`):
![set-framework-ip](Images/image59.png)

- **Configure Network Monitoring**:
![network-monitoring](Images/image60.png)

- **Select Monitoring Interface** (Choose `eth1` with Promiscuous Mode):
![select-interface](Images/image61.png)

- **Return to Main Menu**:
![main-menu](Images/image62.png)

- **Apply Configuration Changes**:
![apply-changes](Images/image63.png)

- **Confirm Configuration Update**:
![confirm-update](Images/image64.png)

- **Wait for Configuration Process**:
![config-process](Images/image65.png)

### Target Systems Setup

#### Ubuntu Web Server
1. **Create the VM**:
   - Name: web-server
   - OS: Ubuntu Server (64-bit)
   - Memory: 4GB RAM
   - CPU: 2 processors
   - Storage: 15GB fixed disk

2. **Installation Process**:<br>

- **Configure User Profile and Credentials:**:
![profile-setup](Images/image70.png)

- **Installation Complete and System Ready**:
![system-ready](Images/image71.png)

3. **Install and Configure Apache**:
   ```bash
   sudo apt update
   sudo apt install apache2
   sudo ufw allow 'Apache'
   sudo ufw allow 'OpenSSH'
   sudo ufw enable
   ```

4. **Change network mode to Host-Only Adapter**:
From now on we don't need the internet on this VM. Turn it off and change its networking mode to Host-Only Adapter.

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
   - Rename to "Kali-OSSIM-Lab"

2. **Update the System**:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

3. **Network Configuration**:
After you updated the system, You can change network mode to Host-Only Adapter.

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
