# Incident 003 - Scheduled Task Creation

## Objective

Detect scheduled task creation activity that may indicate attacker persistence using Microsoft Sentinel and Sysmon telemetry.

## MITRE ATT&CK Technique

T1053.005 - Scheduled Task/Job: Scheduled Task

## Attack Simulation

Atomic Red Team test:

T1053.005-1 Scheduled Task Startup Script

The simulation created two scheduled tasks using `schtasks.exe`:

- T1053_005_OnLogon
- T1053_005_OnStartup

These tasks were configured to launch `calc.exe` upon user logon or system startup, simulating a persistence mechanism commonly used by attackers.

## Evidence Collected

Hostname:
SOC-Windows-VM

User:
SOC-Windows-VM\azureuser

Process:
schtasks.exe

Event Source:
Microsoft-Windows-Sysmon

Event ID:
1 - Process Creation

Scheduled Tasks Created:
- T1053_005_OnLogon
- T1053_005_OnStartup

## Detection Logic

Microsoft Sentinel monitored Sysmon process creation events and detected execution of `schtasks.exe` with task creation arguments.

## KQL Query

See `detection.kql`

## Investigation

The alert was generated after Atomic Red Team created scheduled tasks using the Windows `schtasks.exe` utility.

Sysmon Event ID 1 recorded the full command line, including the scheduled task names and execution parameters. The activity was traced to the `azureuser` account on `SOC-Windows-VM`.

The scheduled tasks were configured to execute `calc.exe` during user logon and system startup to simulate an attacker establishing persistence.

## Recommended Response

1. Validate whether the scheduled task is authorized.
2. Review the command executed by the task.
3. Remove unauthorized scheduled tasks.
4. Investigate additional persistence mechanisms.
5. Review recent account activity for signs of compromise.

## Lessons Learned

Scheduled tasks are a common persistence technique used by attackers to execute malicious code automatically after login or system startup.

Sysmon Event ID 1 provides detailed visibility into task creation by capturing the executed command line, allowing analysts to identify newly created scheduled tasks and investigate potential persistence activity.
