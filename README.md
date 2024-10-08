# Azure Honeynet & SOC Project: Cyber Attacks in Real Time

## Introduction

 In this project, I simulate a small-scale honeynet that attracts real-world traffic from attackers around the world through Microsoft Azure. The goal throughout this project is to demonstrate best security practices, incident response tactics, and the effects of hardening your environment. We'll accomplish this by intentionally deploying virtual machines that have no safeguard from the public internet to attract attackers into our environment. Then after ingesting some log sources into Log Analytics Workspace, Microsoft Sentinel will come in to create attack maps, alerts, and incidents. In order to showcase metrics before and after hardening the environment based off the incidents generated from the 24 hour capture. 

## Azure Components Utilized

- Virtual Machines (1x Linux, 1x Windows)
- Azure Virtual Network (VNet)
- Network Security Groups (NSG)
- Kusto Query Language (KQL)
- Azure Key Vault 
- Azure Storage Account 
- Microsoft Sentinel 
- Microsoft Defender for Cloud 



In the "BEFORE" stage of the project, all resources were initially deployed with the hope that attraction would be gained from the public internet. The Virtual Machines had both their Network Security Groups (NSGs) and built-in firewalls wide open, allowing unrestricted access from any source. Additionally, all other resources, such as storage accounts and databases, were deployed with public endpoints visible to the internet, without utilizing any Private Endpoints for added security. These machines were then left to the public for 24 hours to generate the following attack maps mentioned earler. 
 <br />

 
 <b>This attack map highlights the incidents for syslog authentication failures experienced by the Linux server. </b>
 
![Linux Syslog Auth Failures](https://i.imgur.com/maeWdk1.png)<br>

 <br />
 <br />
 
 <b>This attack map showcases RDP failures against the Window machine.</b>
 
![Windows RDP/SMB Auth Failures](https://i.imgur.com/LBE6JJ0.png)<br>

 <br />
 <br />
 
 
 ## After Hardening Measures and Security Controls

In the "AFTER" stage, based off the incidents created from the "Before" 24 hour capture, I implemented hardening measures and security controls to improve the environment's security from attackers.<br /> 
These improvements included:

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by only allowing my own public IP address to come thrugh otherwise all other traffic would be blocked by the new parameteres created.

- <b>Built-in Firewalls</b>: In my virtual machines I configured the built-in firewalls so that it would deny access from unauthorized users. 

- <b>Private Endpoints</b>: For other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to only the virtual network.

## Attack Maps After Hardening / Security Controls

<br />

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

 <br />
 
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br/>
Start Time 2024-09-12 09:15 AM<br/>
Stop Time 2024-09-13 09:15 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 4858
| Syslog (Linux VM)                   | 2445
| SecurityAlert (Microsoft Defender for Cloud            | 6
| SecurityIncident (Sentinel Incidents)        | 73
| NSG Inbound Malicious Flows Allowed | 103



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our secure environment for 24 hours:<br/>
Start Time 2024-09-13 4:25 PM<br/>
Stop Time	2023-09-14 4:25 PM


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent (Windows VM)            | 2264
| Syslog (Linux VM)                   | 24
| SecurityAlert (Microsoft Defender for Cloud            | 0
| SecurityIncident (Sentinel Incidents)        | 0
| NSG Inbound Malicious Flows Allowed | 0

<br/>


## Utilizing NIST 800.61r2 Computer Incident Handling Guide

For each simulated attack I practiced incident responses following NIST SP 800-61 r2.

![NIST 800.61](https://i.imgur.com/6PTG7c0l.png)

Each organization will have policies related to an incident response that should be followed. This event is just a walkthrough for possible actions to take in the detection of malware on a workstation.  

#### Preparation

- The Azure lab was set up to ingest all of the logs into Log Analytics Workspace, Sentinel and Defender were configured, and alert rules were put in place.

#### Detection & Analysis

- Malware has been detected on a workstation with the potential to compromise the confidentiality, integrity, or availability of the system and data.
- Assigned alert to an owner, set the severity to "High", and the status to "Active"
- Identified the primary user account of the system and all systems affected.
- A full scan of the system was conducted using up-to-date antivirus software to identify the malware.
- Verified the authenticity of the alert as a "True Positive".
- Sent notifications to appropriate personnel as required by the organization's communication policies.

#### Containment, Eradication & Recovery

- The infected system and any additional systems infected by the malware were quarantined.
- If the malware was unable to be removed or the system sustained damage, the system would have been shut down and disconnected from the network.
- Depending on organizational policies the affected systems could be restored known clean state, such as a system image or a clean installation of the operating system and applications. Or an up-to-date anti-virus solution could be used to clean the systems. 

#### Post-Incident Activity

- In this simulated case, an employee had downloaded a game that contained malware. 
- All information was gathered and analyzed to determine the root cause, extent of damage, and effectiveness of the response. 
- Report disseminated to all stakeholders.
- Corrective actions are implemented to remediate the root cause.
- And a lessons-learned review of the incident was conducted.

<br />

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. The significant reduction in security events and incidents following the implementation of security controls highlights their effectiveness in safeguarding the environment.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
