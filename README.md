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

##*Attack Lifecycle*

Attacker executes PowerShell
          ↓
Creates a local administrator account
          ↓
Uses RDP to remotely access the system
          ↓
Creates a scheduled task for persistence
          ↓
Abuses IFEO registry injection to hijack execution

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
- Account creation
- Scheduled Task
- Image File Execution Option
- Remote Desktop


## Tools

- Microsoft Sentinel
- KQL
- Sysmon
- Atomic Red Team
- MITRE ATT&CK
