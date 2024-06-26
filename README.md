
# Building a SOC + Honeynet in Azure (Live Traffic)

## Introduction and Objective

In this project, I created a mini honeynet and SOC in the Microsoft Azure platform. The honeynet consisted of various resources completely open and vulnerable to the public internet. The goal was to capture and analyze logs from these sources and aggregate them to a Log Analytics Workspace. Microsoft Sentinel was then deployed to leverage these logs to build attack maps, trigger alerts, and create incidents. Security metrics were measured in this insecure environment for 24 hours. I then applied security controls to harden the environment, including NIST 800-53. The same security metrics were measured for another 24 hours, within the hardened environment. The objective was to compare the results and understand the effectiveness and importance of proper controls and regulatory standards. The metrics analyzed were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/f230e3f4-76a0-4ad5-a895-9a90e096b636)

The architecture of the virtual environment consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Architecture Before Hardening / Security Controls
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/2389228e-054c-4b84-a6ac-5715e5c4df57)

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/2c87dc78-fd19-4f69-b345-cb940d1b928a)

This map demonstrates the consequences of leaving the Network Security Group (NSG) open, as it allowed for malicious traffic to flow unimpeded.

![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/65948389-4d70-4153-a3c1-91e0a84ced4d)

This map shows the numerous syslog authentication failures experienced by the Linux server that was deployed. 

![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/b9c75901-b04d-4bb0-bbe1-44b56ae5acb3)

This map shows the numerous RDP and SMB failures experienced by the Windows virtual machines.

## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:

Start Time 2024-05-29 19:45:24

Stop Time 2024-05-30 19:45:24

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 103996
| Syslog                   | 3444
| SecurityAlert            | 3
| SecurityIncident         | 298
| AzureNetworkAnalytics_CL | 2416

## Architecture After Hardening / Security Controls
![image](https://github.com/wadegamache/Azure-SOC-Honeynet/assets/171600915/14e53716-1a48-488e-9ad7-0c783c631871)

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics measured in the environment for another 24 hours, but after security controls were applied:

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

After reviewing the logs ingested and metrics measured, it's clear to me the importance of security hardening and fortifying an environment's security posture. These security measures are important to mitigate risk and safegaurd valuable data and resources. The effectiveness of even the simplest control can not be understated. The combination of several controls will only strengthen that posture. The metircs measured a 99.99% reduction in events and incidents across the board. It is important to mention that if the network's resources were engaged by regular uers, it's likely that a higher number of secrity events and alerts would have been produced within the 24-hour timeframe post-security control implementation. 
