# Implementing a SOC and Honeynet in Azure

![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)



## Introduction and Architecture 
In this project, I created a SOC and honeynet using multiple resources in Microsoft Azure and ingested their logs into a Log Analytics Workspace, which was then connected to Microsoft Sentinel acting as a SIEM. 

The architecture of the honeynet in Azure consists of the following components:

-Virtual Network (VNet)

-Network Security Groups (NSG)

-Virtual Machines (1 windows, 1 linux)

-MSSQL Server (running on the windows-vm)

-Log Analytics Workspace

-Azure Key Vault

-Azure Storage Account

-Microsoft Sentinel

-An additional windows "attack-vm" separate from the Log Analytics Workspace in order to practice ingesting logs and generating alerts

After ingesting the logs from all of these resources into my Log Analytics workspace using various data collection rules (DCR) and KQL syntax, I was able to generate attack maps, alerts, and incidents using Microsoft Sentinel. I originally ran this environment on the live internet without proper security and measured some security metrics in the insecure environment for 24 hours. I then implemented some security controls listed in NIST 800-53 to harden the environment and ran the properly secured environment on the live internet for another 24 hours.

## Logs Examined
The metrics include:

-SecurityEvent (Windows Event Logs)

-Syslog (Linux Event Logs)

-SecurityAlert (Log Analytics Alerts Triggered)

-SecurityIncident (Incidents created by Sentinel)

-AzureNetworkAnalytics_CL (Malicious Flows allowed into honeynet)


For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my IP address, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps (Before Hardening Environment)


![linux ssh 24 hour before](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/f466c245-c346-431f-8ef8-c5148b56a15a)



![mssql 24 hour map before](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/30876d12-732c-4dde-ac8e-056bde3bef35)


![nsg-malicious-allowed-in 24 hour before](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/53ef32ef-06b0-4b96-9626-a87826aa979a)


![windows rdp 24 hour before](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/6381d519-7db5-471b-877c-43d216476f8b)


## Metrics (Before Hardening Environment)

The following table shows the metrics I measured in my insecure environment for 24 hours:

Start Time 2024-03-27T22:13:26.8024321Z

Stop Time 2024-03-28T22:13:26.8024321Z

| Metric                                                         | Count
| ---------------------------------------------------------------| -----
| SecurityEvent (Windows VM)                                     | 99212
| Syslog (Linux VM)                                              | 19983
| SecurityAlert (Microsoft Defender for Cloud)                   | 7
| SecurityIncident (Sentinel Incidents)                          | 245
| AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows Allowed) | 2611


## Attack Maps (After Hardening Environment)


All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.

![image](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/1e74f04d-e84c-4f31-84fc-37970b0070d0)


## Metrics (After Hardening Environment)

The following table shows the metrics I measured in my environment for 24 hours after applying proper controls:

Start Time 2024-03-31T02:58:32.0333992Z

Stop Time 2024-04-01T02:58:32.0333992Z

| Metric                                                         | Count
| ---------------------------------------------------------------| -----
| SecurityEvent (Windows VM)                                     | 24010
| Syslog (Linux VM)                                              | 1
| SecurityAlert (Microsoft Defender for Cloud)                   | 0
| SecurityIncident (Sentinel Incidents)                          | 0
| AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows Allowed) | 0

## Results

The following table shows change percentages from before and after hardening:

| Metric                                                         | Change after securing environment
| ---------------------------------------------------------------| -----
| SecurityEvent (Windows VM)                                     | -75.80%
| Syslog (Linux VM)                                              | -99.99%
| SecurityAlert (Microsoft Defender for Cloud)                   | -100.00%
| SecurityIncident (Sentinel Incidents)                          | -100.00%
| AzureNetworkAnalytics_CL (NSG Inbound Malicious Flows Allowed) | -100.00%

## Incidents + Incident Response

Numerous incidents were triggered after leaving the environment open to traffic, most of which were brute force attempts. I was able to practice triaging alerts and investigating such incidents in Microsoft Sentinel, assigning them to myself, and discovering more about them. Upon diving into a generated incident and completing investigation into it, I could discover the attacker IP addresses, where they were coming from, and other related incidents/alerts coming from the same entities. After, I could then close out the alert and move onto another.

![image](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/7fd41886-d311-4bbe-80f8-e851dfdf7c7c)

![image](https://github.com/gradygolden/Cybersecurity-Projects/assets/157150281/b39b89ec-a252-4203-b7c5-1c96340fb6fe)





