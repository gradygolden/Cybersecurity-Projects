# Implementing a SOC and Honeynet in Azure

*NOTE- this page is under construction and being updated with images.

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

After ingesting the logs from all of these resources into my Log Analytics workspace using various data collection rules (DCR) and KQL syntax, I was able to generate attack maps, alerts, and incidents using Microsoft Sentinel. I originally ran this environment on the live internet without proper security and measured some security metrics in the insecure environment for 24 hours. I then implemented some security controls listed in NIST 800-53 to harden the environment and ran the properly secured environment on the live internet for another 24 hours. The metrics include:

-SecurityEvent (Windows Event Logs)

-Syslog (Linux Event Logs)

-SecurityAlert (Log Analytics Alerts Triggered)

-SecurityIncident (Incidents created by Sentinel)

-AzureNetworkAnalytics_CL (Malicious Flows allowed into honeynet)


For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my IP address, and all other resources were protected by their built-in firewalls as well as Private Endpoint.




