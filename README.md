# Building a SOC + Honeynet in Azure (Live Traffic)
![Honeynet   SOC Lab](https://github.com/user-attachments/assets/59b115ad-2c45-4c82-ba8c-05ed1882ec2d)


## Introduction

In this lab I built a honeynet in Microsoft Azure. It starts with multiple resources being created and exposed to global internet traffic via poor network configuration and open firewall rules. Each resource has logs sent to a central repository using the Logs Analytics Workspace. I provisionsed a SIEM using Microsoft Sentinnel and quiried LAW based on using KQL. The rules and queries projected data in the form of SOC incidents and heatmaps. I used Microsoft Defender to features to rank my security posture and respond to harden my environment based on NIST 800-53 standards. More details will be provided below.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Vulnerable assets2](https://github.com/user-attachments/assets/95918535-d934-4e6b-8df1-38a5bd8d5aed)

## Architecture After Hardening / Security Controls
![Secured Assets2](https://github.com/user-attachments/assets/5c949101-bb64-4c46-a19e-8c713e7748d9)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open to incoming web traffic, and all other Azure Key Vault, Blob Storage, and Activity Logs are deployed visible to public internet traffic.

The "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint for the Azure Key Vault, Blob Storage, and Activity Logs.

## Attack Maps Before Hardening / Security Controls
![24hr vulnerable NSG map](https://github.com/user-attachments/assets/afafc90d-28f4-4e48-a241-5346547aa579)<br>
![24hr vulnerable windows map](https://github.com/user-attachments/assets/c6022461-520a-42b6-b684-f6b0de2df19e)<br>
![24hr vulnerable linux map](https://github.com/user-attachments/assets/13625599-9566-4aea-b303-d6e84ec9b738)<br>
![24hr vulnerable MSSQL map](https://github.com/user-attachments/assets/2bf2f208-e393-496c-83f9-4b0cc1bd0e6d)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 12/21/2024, 00:23:46
Stop Time 12/22/2024, 00:23:46

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17347
| Syslog                   | 9782
| SecurityAlert            | 313
| SecurityIncident         | 317
| AzureNetworkAnalytics_CL | 3564

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening. This was suprising to see how much firewalls and perimeter security protect our assets.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 12/25/2024, 23:04:53
Stop Time	12/26/2024, 23:04:53

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 609
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is daunting to see the staggering amount of threats existing globally on the open internet. I will use the critical incidents generated in Microsoft Sentinel to review Incident Response procedures in accordance with NIST 800-61 Incedent Response Framework in a future lab exercise.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
Thanks for checking out my lab.
