# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC]

## Introduction

For this undertaking, I constructed a compact honeynet within Azure and gathered log data from diverse sources into a Log Analytics workspace. These logs were subsequently utilized by Microsoft Sentinel to generate attack visualizations, activate alerts, and establish incidents. In the initial phase, I assessed security metrics within the vulnerable environment over a span of 24 hours. Following that, I implemented several security measures to fortify the environment, and once again evaluated metrics for an additional 24-hour period. The outcomes of these assessments are presented below.The metrics used in this lab are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imgur.com/eDiNvmW.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/jtiYKDD.jpg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/by8K1jf.jpg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/UsHgAlN.jpg)<br>

## Metrics Before Hardening / Security Controls

The table below shows the metrics measured in an insecure environment for 24 hours:
Start Time 2023-06-01 20:53:30
Stop Time  2023-06-02 20:53:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18929
| Syslog                   | 2775
| SecurityAlert            | 10
| SecurityIncident         | 231
| AzureNetworkAnalytics_CL | 719

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The table below shows the metrics measured in an environment for another 24 hours, after security controls were applied :
Start Time 2023-06-07 03:36:58
Stop  Time 2023-06-08 03:36:58

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 696
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 3
| AzureNetworkAnalytics_CL | 0

## Results of hardening the enviorment (BEFORE and AFTER Metrics) 
|	                                          |Change after security environment
|--------------------------------------------|---------------------------------
|Security Events (Windows VMs)	             |    -96.32%
|Syslog (Linux VMs)	                         |    -99.17%
|SecurityAlert (Microsoft Defender for Cloud)|	   -100.00%
|Security Incident (Sentinel Incidents)      |    -98.70%
|NSG Inbound Malicious Flows Allowed	       |    -100.00%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
