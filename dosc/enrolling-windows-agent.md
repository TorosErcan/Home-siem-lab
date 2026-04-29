# Enrolling Windows Agent

## Goal
Enrol the Windows 10 VM as a monitored endpoint in Wazuh so logs and secuirty events flow into the SIEM dashboard.

## Wazuh Agent
A software installed on endpoints that:
- Collects security events and logs
- Monitors file integrity
- Detects vulnerabilities
- Ships all data back to the Wazuh manager over the internal network

## Environment
Agent

## Setup

### 1. Windows Installation
Installed Windows 10 pro on VM

### 2. Static IP Configuration
Navigate to 'Network and Internet Settings' > 'Change adapter options' > internal network adapter 'Properties' > 'Internet Protocol version 4(TCP/IPv4)' > 'Use the following IP addresses'
Add fill with:
IP Address: 192.168.100.20 (Device is set to the same network as Wazuh-Server) 
Subnet Mask: 255.255.255.0 (Range of devices addresses in the same network)
Default Gateway: 192.168.100.1 (Traffic destined for outside the local network. Internet access handled by NAT adapter)
Preferred DNS Server: 8.8.8.8 (Google's Primary DNS)
Alternative DNS Server: 8.8.4.4 (Google's Secondary DNS)

### 3. Connectivity Verification
ping 192.168.100.10 (Wazuh Server)

Confirms Windows Agent can reach Wazuh Server over internal network.

### 4. Agent Installation
Open the browser and go to https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.0-1.msi to download the wazuh agent and install.
or
1. In Powershell as administrator Invoke-WebRequest -Uri "https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.0-1.msi" -OutFile "C:\Users\user\Downloads\wazuh-agent-4.7.0-1.msi"
Invoke-WebRequest downloads directly to the path specified

2. msiexec.exe /i "C:\Users\user\Downloads\wazuh-agent-4.7.0-1.msi" WAZUH_MANAGER="192.168.100.10" WAZUH_AGENT_NAME="Windows-Target" /l*v "C:\Users\user\Downloads\install.log"

Then NAT START WazuhSvc

Navigate to the Wazuh dashboard on the host machine and the Agent will appear as active


## Issues Encountered


## Screenshots
