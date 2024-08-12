# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/d6e3a6d7-9418-40c1-ba94-69e1c9be6296)<br>
![image](https://github.com/user-attachments/assets/8fedae24-a7c0-4752-9fc4-98b89c8219fa)<br>
![image](https://github.com/user-attachments/assets/993d00f2-ef9a-4bbe-b2c6-1e1fe8d12f12)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-06-18 13:54
Stop Time 2024-06-19 13:54

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 47884
| Syslog                   | 2399
| SecurityAlert            | 4
| SecurityIncident         | 288
| NSG Inbound Malicious Flows Allowed | 1808

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-19 23:02
Stop Time	2024-06-20 23:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 16534
| Syslog                   | 6
| SecurityAlert            | 0
| SecurityIncident         | 0
| NSG Inbound Malicious Flows Allowed | 0

![image](https://github.com/user-attachments/assets/e940e227-2039-4286-8920-63122bd0c3a9)


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
