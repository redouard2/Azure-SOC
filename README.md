# Azure-SOC + Honeynet (Live Cyber Attacks)
![image](https://github.com/redouard2/Azure-SOC/assets/73624384/575fbe98-aa7d-4973-8a2f-49ad266bdf7a)



## Introduction

In this project, a mini honeynet was constructed within the Azure platform. This project aimed to capture and analyze logs from several sources subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a seven-day period. Following this phase, security controls were implemented to fortify the virtual environment. Following that, another seven-day metric measurement phase was conducted, and the results obtained from these endeavors are presented below. Lastly, a final seven-day metric measurement phase was conducted to see the differences between the first week of controls introduced and the second week afterward, and the results obtained from these endeavors are also presented below. The metrics analyzed were:

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
![image](https://github.com/redouard2/Azure-SOC/assets/73624384/78297901-db52-4f5b-8110-8cd58ef5c234)

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
Start Time 2023-10-01 12:00
Stop Time 2023-10-07 12:00

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 292830
| Syslog                   | 152543
| SecurityAlert            | 39
| SecurityIncident         | 1843
| AzureNetworkAnalytics_CL | 28580

## Architecture After Week 1 of Hardening / Security Controls
![image](https://github.com/redouard2/Azure-SOC/assets/73624384/e79a4e43-40cf-4298-98e4-ed9d7dce9d93)


The environment was hardened for the "AFTER" stage of the project, and security controls were implemented to comply with NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:
- <b>Network Security Groups (NSGs)</b>: NSGs were hardened by blocking all inbound and outbound traffic except for designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source could access the virtual machines.

- <b>Built-in Firewalls</b>: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM, which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints</b>: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that these sensitive resources were limited to the virtual network, not the public internet.

- <b>Subnetting</b>: To further enhance security, a subnet was created for Azure Key Vault and Storage Containers to separate traffic further and make an extra layer of protection for those endpoints.

## Attack Maps After Week 1 Hardening / Security Controls

<b>This attack map shows the reduction of traffic allowed by an NSG after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/dc420564-2920-454b-8af8-3d83b05c3cc7)

<b>This attack map shows the reduction of attempts threat actors trying to access the Linux virtual machine after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/b2c87bea-c7ae-4c0d-93ac-837f727652b0)

<b>This attack map shows the elimination of attempts from threat actors trying to access the Database Server after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/f803179e-de6e-4145-ba3d-d6d4e45c8692)


<b>This attack map shows the reduction of attempts threat actors trying to access the Windows virtual machine after applying NIST 800-53 controls</b>

![image](https://github.com/redouard2/Azure-SOC/assets/73624384/589ab541-4380-464f-a0c6-4e9a2b67f941)

## Metrics After Week 1 of Hardening / Security Controls

The following table shows the metrics we measured in our environment for another week, but after I applied security controls:
Start Time 2023-10-08 12:00
Stop Time	2023-10-14 12:00

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 108065
| Syslog                   | 22888
| SecurityAlert            | 7
| SecurityIncident         | 1253
| AzureNetworkAnalytics_CL | 2699

## Architecture After Week 2 of Hardening / Security Controls

```The environment has not changed from the controls set in place in Week 1. No changes were necessary.```

## Attack Maps After Week 2 of Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for this seven-day period phase after hardening.```

## Metrics After Week 2 of Hardening / Security Controls

The following table shows the metrics we measured in our environment for another week, but after I applied security controls:
Start Time 2023-10-15 12:00
Stop Time	2023-10-21 12:00

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 50724
| Syslog                   | 2
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Reflection

This project has opened my eyes to the necessity of security, and I had massive fun creating it. It is truly imperative to have proper security controls and configurations in place to protect your resources. The before and after metrics highlight the drastic difference between an insecure and a secure environment, and the map data illustrate the changes. Firewall rules, private endpoints, and not allowing public internet access must be implemented to prevent disastrous consequences caused by attacks from threat actors and unauthorized access to critical assets and resources.

## Conclusion

In this project, a mini but effective honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. The metrics below show the change percentage after applying security controls in week 1 and week 2.


### Impact of Security Controls Week 1

| Metric                                       | Change post-hardening
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 63.1%
| Syslog (Linux VMs)                           | 85%
| SecurityAlert (Microsoft Defender for Cloud) | 82.05%
| SecurityIncident (Sentinel Incidents)        | 32.01%
| AzureNetworkAnalytics_CL                     | 92.56%

### Impact of Security Controls Week 2

| Metric                                       | Change post-hardening
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 53.06%
| Syslog (Linux VMs)                           | 99.99%
| SecurityAlert (Microsoft Defender for Cloud) | 100%
| SecurityIncident (Sentinel Incidents)        | 100%
| AzureNetworkAnalytics_CL                     | 100%
