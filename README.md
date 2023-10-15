# Azure-SOC + Honeynet (Live Cyber Attacks)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, a mini honeynet was constructed within the Azure platform. This project aimed to capture and analyze logs from several sources subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a seven-day period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another seven-day metric measurement phase was conducted, and the results obtained from these endeavors are presented below. The metrics analyzed were:

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

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In this project's "BEFORE" Stage, a virtual environment was deployed and exposed to the public Internet for threat actors to discover and attempt to break into the machine. This aimed to analyze these actors' attack patterns by attracting them to vulnerable-looking machines. With this plan in mind, I created a Windows virtual machine hosting a SQL database as well as a Linux server to deploy openly. I had both of the VM's network security groups (NSGs) configurations set to "Allow All." A storage account and key vault were deployed to further entice these attackers, with public endpoints visible on the open internet. In this stage, Microsoft Sentinel monitored the unsecured environment using logs aggregated by the Log Analytics workspace.

## Attack Maps Before Hardening / Security Controls

<b>This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/228e66fc-0588-4854-a73a-4c1568c4c9ca)

<b>This attack map shows all the attempts threat actors trying to access the Linux virtual machine via SSH</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/beb48b46-eb48-486d-9c55-129e96a2d0b6)

<b>This attack map shows all the attempts threat actors trying to access the Microsoft SQL Database Server within the Windows virtual machine</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/36c73c5f-2c24-415d-8a2b-148e489b1625)

<b>This attack map shows all the attempts threat actors trying to access the Windows virtual machine via RDP</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/3a4bce02-03d6-449b-bc87-d4118ea551f0)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-10-01 13:30
Stop Time 2023-10-07 13:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 263048
| Syslog                   | 150660
| SecurityAlert            | 39
| SecurityIncident         | 1679
| AzureNetworkAnalytics_CL | 26752

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The environment was hardened for the "AFTER" stage of the project, and security controls were implemented to comply with NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:
- <b>Network Security Groups (NSGs)</b>: NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM, which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints</b>: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network, not the public internet.

- <b>Subnetting</b>: To further enhance security, a subnet was created for Azure Key Vault and Storage Containers, to further separate traffic and create an extra layer of security for those endpoints.

## Attack Maps After Hardening / Security Controls

<b>This attack map shows the reduction of traffic allowed by an NSG after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/dc420564-2920-454b-8af8-3d83b05c3cc7)

<b>This attack map shows the reduction of attempts threat actors trying to access the Linux virtual machine after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/b2c87bea-c7ae-4c0d-93ac-837f727652b0)

<b>This attack map shows the elimination of attempts from threat actors trying to access the Database Server after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/f803179e-de6e-4145-ba3d-d6d4e45c8692)


<b>This attack map shows the reduction of attempts threat actors trying to access the Windows virtual machine after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/589ab541-4380-464f-a0c6-4e9a2b67f941)

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another week, but after I applied security controls:
Start Time 2023-10-08 13:30
Stop Time	2023-10-15 13:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 174890
| Syslog                   | 34252
| SecurityAlert            | 10
| SecurityIncident         | 1393
| AzureNetworkAnalytics_CL | 7609

## Conclusion

In this project, a mini but effective honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. After implementing more robust security controls, there was a 34% reduction in Windows Security Events, a 77% reduction in Linux Events, and a 54% reduction in security alerts, incidents, and malicious inbound network traffic.


