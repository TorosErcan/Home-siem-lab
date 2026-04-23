# Lab Setup

## Overview
Setting up the virtual machine environment for the home SIEM lab.

## Environment
- Host OS: [Windows 11]
- Virtualisation: VirtualBox
- VMs: Wazuh Server (Ubuntu 22.04), Windows 10 Agent

## Network Design
- Internal Network name: `siem-lab`
- Wazuh Server IP: `192.168.100.10`
- Windows Agent IP: `192.168.100.20`

## Steps Completed

### 1. Downloaded ISOs
- Ubuntu 22.04 LTS Server
- Windows 10

### 2. Created VMs
| VM | RAM | Disk | Network |
|---|---|---|---|
| Wazuh-Server | 4GB | 50GB | NAT + siem-lab |
| Windows-Agent | 2GB | 40GB | NAT + siem-lab |

### 3. Setup of internal network IP address - Server VM 
Network connectivity : netplan, IP a , IP ping

## Screenshots
1. <img width="1163" height="728" alt="VMs built" src="https://github.com/user-attachments/assets/0cdb9e6a-8a32-4e30-85cd-d3e14f99d723" />
2. <img width="1272" height="862" alt="No IP found for internal network in ubuntu setup" src="https://github.com/user-attachments/assets/7a32db28-596f-4905-9156-30df4102e273" />
3. <img width="1272" height="862" alt="Ubuntu login successful on wazuh server" src="https://github.com/user-attachments/assets/17d4d0c6-ee83-4377-a3e9-c727b193b982" />
4. <img width="1272" height="862" alt="testing connection of server to internet" src="https://github.com/user-attachments/assets/2948367e-6c01-4afd-bd42-9ab60800229b" />
5. <img width="1272" height="862" alt="Finding ip addresses for adapters" src="https://github.com/user-attachments/assets/c4a8f35e-4428-43bd-899a-e81a51bd3fe7" />
6. <img width="840" height="1050" alt="Netplan config IP file" src="https://github.com/user-attachments/assets/8e73afa5-cb58-4a88-bdb9-90021852fa45" />
7. <img width="840" height="1050" alt="Static IP set for internal network " src="https://github.com/user-attachments/assets/d9b7d140-663e-4b94-87bd-0a5c3b836321" />
8. <img width="840" height="1050" alt="IPs confirmed" src="https://github.com/user-attachments/assets/3bb44e63-7b7a-4466-ad6e-b97412d55b2d" />



## Issues Encountered
No IP address for internal network:
Set up 2 seperate networks on the VMs: NAT - for connecting to the internet for downloading of Wazuh server(Wazuh-server VM) and host(Windows10-Target VM).
Needed to configure the internal network to set an IP as wont be assigned automatically due to no router to perform DHCP.
Opened a Netplan file "sudo nano /etc/netplan/00-installer-config.yaml" and "sudo netplan apply"
set to static IP so its known exactly where target sends logs to, creating a relaible connection



