# Enterprise SOC Lab using Microsoft Sentinel

## Overview

Built a cloud-based Security Operations Center (SOC) lab in Microsoft Azure to simulate real-world cyberattacks, collect endpoint telemetry using Sysmon, develop Kusto Query Language (KQL) detections, and create Microsoft Sentinel analytics rules for automated threat detection and investigation.

---

## Technologies Used

* Microsoft Azure
* Microsoft Sentinel
* Microsoft Defender Portal
* Azure Monitor Agent (AMA)
* Log Analytics Workspace
* Sysmon
* Virtual Machines
* Atomic Red Team
* Kusto Query Language (KQL)
* Windows Event Logs
* MITRE ATT&CK Framework

---

## Architecture

```text
Azure Virtual Machine
        ↓
      Sysmon
        ↓
Azure Monitor Agent
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
        ↓
Analytics Rules
        ↓
Microsoft Defender Portal Alerts
```

---

## Attack Lifecycle

```text
PowerShell Execution
        ↓
Local Administrator Account Creation
        ↓
Remote Desktop Access
        ↓
Scheduled Task Persistence
        ↓
Scheduled Task Persistence + IFEO Execution Hijacking
```

This lab demonstrates how multiple MITRE ATT&CK techniques can be chained together to simulate a realistic Windows intrusion, from initial code execution through persistence and remote access.

---

## MITRE ATT&CK Techniques Simulated

| Technique                                     | ATT&CK ID |
| --------------------------------------------- | --------- |
| PowerShell Execution                          | T1059.001 |
| Create Local Account                          | T1136.001 |
| Remote Desktop Protocol (RDP)                 | T1021.001 |
| Scheduled Task                                | T1053.005 |
| Image File Execution Options (IFEO) Injection | T1546.012 + T1053.005 |

---

## Detection Engineering

Developed Microsoft Sentinel KQL detections for:

* Suspicious PowerShell execution
* Local administrator account creation
* Remote Desktop (RDP) activity
* Scheduled task creation
* Correlated Scheduled Task + IFEO persistence activity

Each detection was validated by executing the corresponding Atomic Red Team simulation and confirming the expected telemetry was collected.

---

## Analytics Rules

Implemented Microsoft Sentinel analytics rules to automatically generate alerts for:

* PowerShell execution
* Local administrator account creation
* Remote Desktop activity
* Scheduled task creation
* Scheduled Task + IFEO correlation rule

The correlation rule detects an attacker creating a scheduled task followed by an IFEO registry modification on the same endpoint within a 10-minute window, significantly increasing confidence that a persistence attack is being established.

---

## Incident Investigations

Each incident folder contains:

* MITRE ATT&CK mapping
* Atomic Red Team simulation
* KQL detection query
* Microsoft Sentinel analytics rule
* Investigation summary
* Recommended response actions
* Screenshots of attack execution, telemetry, and generated alerts

---

## Repository Structure

```text
SOC-Operations-Lab/
│
├── README.md
├── architecture/
├── incidents/
│   ├── 001-PowerShell-T1059.001/
│   ├── 002-Create-Local-User-T1136.001/
│   ├── 003-RemoteDesktop-T1021.001/
│   ├── 004-Scheduled-Task-T1053.005/
│   └── 004-Attack-Chain-ScheduledTask-IFEO/
│
├── analytics-rules/
│   ├── 001-PowerShell-Rule.png/
│   ├── 002-Account-Creation-Rule.png/
│   ├── 003-Remote-Desktop-Rule.png/
│   ├── 004-ScheduleTask-Rule.png/
│   └── 005-AAttack-Chain-ScheduledTask-IFEO-Rule/
├── workbooks/
```

---

## Skills Demonstrated

* Microsoft Sentinel
* Microsoft Defender Portal
* Detection Engineering
* Threat Hunting
* Kusto Query Language (KQL)
* Windows Event Analysis
* Sysmon
* Incident Investigation
* Azure Monitor Agent
* Log Analytics Workspace
* MITRE ATT&CK Framework
* Atomic Red Team
* Security Monitoring
* Endpoint Telemetry Analysis
* Alert Development
* Correlation Rule Development
