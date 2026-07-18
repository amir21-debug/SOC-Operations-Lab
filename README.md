# Enterprise SOC Lab using Microsoft Sentinel

## Overview

Built a cloud-based SOC environment using Microsoft Sentinel,
Azure Monitor Agent, Sysmon, and Atomic Red Team.

## Architecture

Azure VM
↓
Sysmon
↓
Azure Monitor Agent
↓
Log Analytics
↓
Microsoft Sentinel


## Attack Simulation

Simulated MITRE ATT&CK techniques:

1: T1059.001 PowerShell
2: T1136 Create Local User 
3: T1053 Scheduled Task
4: T1546 Image File Execution Option
5: T1021 Remote Desktop


## Detection Engineering

Created KQL detections for:

- Suspicious PowerShell
- Failed authentication
- Account creation
- Registry modification


## Tools

- Microsoft Sentinel
- KQL
- Sysmon
- Atomic Red Team
- MITRE ATT&CK
