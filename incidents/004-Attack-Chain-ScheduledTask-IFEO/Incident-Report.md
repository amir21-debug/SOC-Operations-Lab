# Incident 004 - Scheduled Task and IFEO Persistence Chain

## Objective

Detect a persistence attack chain by correlating scheduled task creation with Image File Execution Options (IFEO) registry modification using Microsoft Sentinel and Sysmon telemetry.

## MITRE ATT&CK Techniques

- T1053.005 - Scheduled Task
- T1546.012 - Image File Execution Options (IFEO) Injection

## Attack Simulation

Atomic Red Team tests:

- T1053.005-1 Scheduled Task Startup Script
- T1546.012-1 IFEO Injection

The simulation demonstrated a multi-stage persistence attack.

First, a scheduled task was created to execute `calc.exe` whenever a user logged on. An IFEO registry modification was then configured so that any attempt to launch `calc.exe` was redirected to `cmd.exe`.

This simulated how an attacker could combine multiple persistence techniques to automatically execute malicious code during user logon without requiring manual execution.

## Evidence Collected

Hostname:
SOC-Windows-VM

User:
SOC-Windows-VM\azureuser

Processes:

- schtasks.exe
- cmd.exe
- calc.exe (intercepted through IFEO)

Registry Path:
```cmd
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\calc.exe" /v Debugger /d "C:\Windows\System32\cmd.exe
```

Event Sources:

Microsoft-Windows-Sysmon

Event IDs:

- 1 - Process Creation
- 13 - Registry Value Set

## Detection Logic

## Detection Logic
Two KQL queries were developed for this scenario:

1. **Hunting query (`detection.kql`)** — surfaces any scheduled task creation event (Sysmon Event ID 1, containing `schtasks.exe`) or any IFEO registry modification (Sysmon Event ID 13, containing "Image File Execution Options") independently, for broad visibility during investigation.

2. **Analytics Rule (`analyticalrule.kql`)** — a higher-confidence correlation rule that joins scheduled task creation events with IFEO registry modification events occurring on the same host within a 10-minute window following the task creation. This rule is what triggers an actual Sentinel incident, since it requires both conditions to occur together rather than alerting on either technique in isolation.

## KQL Query

See `detection.kql`

## Investigation

Investigation identified execution of `schtasks.exe` followed by an IFEO registry modification on the same endpoint.

The scheduled task was configured to execute `calc.exe` during user logon. The IFEO registry key redirected execution of `calc.exe` to `cmd.exe`, demonstrating execution hijacking.

Correlating both events significantly reduced false positives compared to detecting either technique independently.

## Recommended Response

1. Validate the scheduled task and its configured action.
2. Review IFEO registry entries for unauthorized debugger values.
3. Remove unauthorized scheduled tasks and IFEO registry modifications.
4. Review additional persistence mechanisms on the endpoint.
5. Reset affected credentials if compromise is confirmed.
6. Monitor for additional attacker activity following persistence establishment.

## Lessons Learned

Attackers frequently combine multiple MITRE ATT&CK techniques rather than relying on a single persistence mechanism.

Correlating related security events greatly improves detection fidelity and provides higher confidence than monitoring isolated events alone.
