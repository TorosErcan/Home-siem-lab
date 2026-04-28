# Lab Setup

## Overview
Setting up the virtual machine environment for the home SIEM lab.

## Environment
- Host OS: [Windows 11]
- Virtualisation: VirtualBox
-  Host RAM: 16GB | Storage available: 94GB

## VMs Created
### Wazuh Server
- OS: Ubuntu 22.04 LTS Server
- RAM: 4GB | CPUs: 2 | Disk: 50GB (dynamically allocated)
- Adapter 1: NAT (internet access for updates and installs)
- Adapter 2: Internal Network — `siem-lab`
- Adapter 3: Host-only 


### Windows10 Agent
- OS: Windows 10
- RAM: 2GB | CPUs: 2 | Disk: 40GB (dynamically allocated)
- Adapter 1: Internal Network — `siem-lab`
- Adapter 2: NAT (internet access for agent install)



## Network Design
- Wazuh Server | enp0s3 | 10.0.2.15/24 (DHCP) | Internet/updates via NAT |
- Wazuh Server | enp0s8 | 192.168.100.10/24 | SIEM internal communications |
- Wazuh Server | enp0s9 | 192.168.56.111/24 (DHCP) | Host machine dashboard access |
- Windows Agent | — | 192.168.100.20/24 | Agent to SIEM communications |




### Testing network connectivity 
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
Set up 2 seperate networks on the VMs:
NAT - for connecting to the internet for downloading of Wazuh server(Wazuh-server VM) and host(Windows10-Target VM).
Internal network - for Vms to communicate with each other.
Needed to configure the internal network to set an IP as wont be assigned automatically due to no router to perform DHCP.
Opened a Netplan file "sudo nano /etc/netplan/00-installer-config.yaml" and "sudo netplan apply"
set to static IP so its known exactly where target sends logs to, creating a relaible connection.




