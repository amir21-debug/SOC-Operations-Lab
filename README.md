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

- T1059.001 PowerShell
- T1087 Account Discovery
- T1136 Create Account
- T1053 Scheduled Task


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
