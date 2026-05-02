# 4 Log Ingestion and Custom Detection Rules

## Goal
Understand the logs flowing into Wazuh from the Windows endpoint, and understand and write custom detection rules to identify specific threat behaviours.

## Detection Rules
A detection rule is logic that tells the SIEM what consititues a security event worth alerting on. Rules evaluate incoming log data against conditions - when conditions are met an alert is generated.

Wazuh rules are written in XML and live in:
- Built in rules: '/var/ossec/ruleset/rules/' (read only)
- Custom rules: '/var/ossec/etc/rules/local_rules.xml'


## The Process for Writing Any New Rule
### Step 1 — Identify what you want to detect
Example: "I want to detect when someone enters the incorrect password when logging into the machine"


### Step 2 — Find the Windows Event ID
Google: "windows event id failed logon"
Answer: Event ID 4625

### Step 3 — Find the Parent SID in Wazuh
#### Method A - Dashboard
Trigger the event on the Windows VM and proceed to the Wazuh dashboard > security events and find the rule ID; thats the parent SID.

#### Method B - Grep
- bash sudo grep -r "4625" /var/ossec/ruleset/rules/

If the event id is too broad such as the case for failed login then:

- bash sudo grep -A 10 "4625" /var/ossec/ruleset/rules/0580-win-security_rules.xml

Find the rule ID that matches — that's your if_sid
Answer: 60122

Always verify the parent SID by triggering the event and confirming the rule fires in the dashboard

Which rules file to search:
Logons, accounts, privileges : `0580-win-security_rules.xml` 
Application events : `0585-win-application_rules.xml` 
System events, services : `0590-win-system_rules.xml` 
PowerShell : `0915-win-powershell_rules.xml` 
Policy changes : `0575-win-policy_rules.xml` 

### Step 4 — Decide the severity
Ask yourself — how serious is this event?
The severity of the alert on a scale of 1-15:
0-3 - Informational(no alert)
4-6 - Low (Single occurence, could be legitimate e.g. Failed login)
7-9 - Medium (Suspicious but not conifrmed attack)
10-12 - High (Strong indicator of attack e.g. Multiple failed login)
13-15 - Critical (malware detection)


### Step 5 — Find the MITRE technique
Google: "MITRE ATT&CK PowerShell"
Answer: T1110

### Step 6 — Group Names
Used for organisation and filtering in the dashboard
Always include two groups: the first is custom_rules that marks its your own rule and the second describes the category of the attack:

authentication : Logons, failed logins, password changes
persistence : New services, scheduled tasks, registry changes
privilege_escalation : Admin account changes, UAC bypass
defence_evasion : Defender disabled, logs cleared, AV killed
lateral_movement : Remote logons, pass the hash
execution : PowerShell, scripts, new processes
account_management : New users created, group changes
network : Suspicious connections, port scanning

### Step 7 — Write the rule
In the wazuh manager VM:
sudo nano /var/ossec/etc/rules/local_rules.xml

Every rule needs a unique ID number
Always start your custom rules at 100000 and increment by 1 for each new rule.

xml
<group name="custom_rules,execution,">
  <rule id="100001" level="5"> 
    <if_sid>60122</if_sid>
    <description>Failed login attempt</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>



#### Additional Rule Conditions
Beyond if_sid you can add extra conditions to make rules more specific:

#### Using Frequency and Timeframe
<rule id="" level="" frequency=""  timeframe="" > 
<if_matched_sid></if_matched_sid>


#### Match specific text in the log
xml<match>mimikatz</match>
Only fires if the word "mimikatz" appears in the log

##### Match a specific user
xml<user>administrator</user>
Only fires for a specific username

##### Match a specific source IP
xml<srcip>192.168.100.20</srcip>
Only fires for traffic from a specific IP

#### Example
Combine conditions
xml<rule id="100005" level="14">
  <if_sid>60122</if_sid>
  <user>administrator</user>
  <description>Failed logon attempt on administrator account</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>
This only fires when a failed logon happens specifically on the administrator account — much more targeted than firing on every failed logon.


### Step 8 — Restart Wazuh
sudo systemctl restart wazuh-manager

### Step 9 — Test and verify
Deliberatly trigger the condition 
Verify security event in Wazuh dashboard
