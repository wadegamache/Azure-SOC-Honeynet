
# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/f230e3f4-76a0-4ad5-a895-9a90e096b636)


## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace. Those logs were then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, then showed the results below. The metrics that were measured are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/2389228e-054c-4b84-a6ac-5715e5c4df57)

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
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/2c87dc78-fd19-4f69-b345-cb940d1b928a)
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/65948389-4d70-4153-a3c1-91e0a84ced4d)
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/b9c75901-b04d-4bb0-bbe1-44b56ae5acb3)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-29 19:45:24
Stop Time 2024-05-30 19:45:24

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 103996
| Syslog                   | 3444
| SecurityAlert            | 3
| SecurityIncident         | 298
| AzureNetworkAnalytics_CL | 2416

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-06-03 15:34:37
Stop Time	2024-06-04 15:34:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.
