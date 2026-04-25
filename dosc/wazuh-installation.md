# Wazuh Installation

## Goal
Deploy the Wazuh SIEM on the Ubuntu server VM.

##Components installed:
Wazuh Manager: Core engine — processes logs and triggers alerts 
Wazuh Indexer: Stores and indexes all log data (based on OpenSearch) 
Wazuh Dashboard: Web UI for visualising alerts and managing the platform


## Wazuh installation
Deployed the Wazuh install on the Ubuntu server VM:
bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash wazuh-install.sh -a -i
Then confirmed Wazuh services are running: sudo systemctl status wazuh-manager

### Flags explained
`-sO`  curl: silent mode, save with original filename 
`-a`  Install all components (manager, indexer, dashboard) 
`-i`  Ignore OS compatibility check 

### Connecting to the Wazuh Siem:
Given credentials to log in to SIEM through the host machine

## Screenshots
9. <img width="1031" height="1050" alt="Running Wazuh installer" src="https://github.com/user-attachments/assets/364302f5-bc14-43a0-88b7-c196974bfbca" />
2. <img width="1282" height="872" alt="Checking Wazuh services are running" src="https://github.com/user-attachments/assets/8f81b258-e454-465f-8827-f5246e9781fc" />
3. <img width="1458" height="736" alt="wazuh dashboard login" src="https://github.com/user-attachments/assets/d1c086b9-3a8c-418e-9b1d-f0705b52ed8a" />



## Issues Encountered
Couldnt connect to Wazuh SIEM:
Got a connection timed out error when trying to access the Wazuh dashboard.
The IP we allocated for the wazuh server is within the internal network so only VMs on that netowrk can reach each other - the host machine sits outside of the network. So a host only adapter was needed and to be cofigured(Netplan config) for them to communicate and for the dashboard to be accessed from the host. 

Forgot credentials for wazuh dashboard login:
Changed the password to something easier to write down:
Ran sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh -u admin -p NewPassword
