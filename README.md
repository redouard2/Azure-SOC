# Azure-SOC + Honeynet (Live Cyber Attacks)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, a mini honeynet was constructed within the Azure platform. The objective was to capture and analyze logs from several sources, subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a seven-day period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another seven-day metric measurement phase was conducted, and the results obtained from these endeavors are presented below. The metrics analyzed were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies, Azure Components, and Regulations Employed
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-10-01 03:26:23
Stop Time 2023-10-07 15:00:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 475834
| Syslog                   | 194710
| SecurityAlert            | 54
| SecurityIncident         | 3327
| AzureNetworkAnalytics_CL | 40207

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the one week period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another week, but after I applied security controls:
Start Time 2023-10-07 15:37
Stop Time	2023-10-14 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 206675
| Syslog                   | 39347
| SecurityAlert            | 14
| SecurityIncident         | 1438
| AzureNetworkAnalytics_CL | 9197

## Conclusion

In this project, a mini but effective honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. After implementing more robust security controls, there was a 56.5% reduction in Windows Security Events, a 79.7% reduction in Linux Events, and a 70% reduction in security alerts, incidents, and malicious inbound network traffic.
