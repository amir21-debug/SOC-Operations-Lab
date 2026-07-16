# Incident 001 – PowerShell Session Creation Detected

## Objective

Validate Microsoft Sentinel's ability to detect PowerShell execution by simulating adversary behavior with Atomic Red Team and analyzing Sysmon telemetry.

---

## MITRE ATT&CK

| Technique | Description |
|-----------|-------------|
| T1059.001 | Command and Scripting Interpreter: PowerShell |

---

## Attack Simulation

**Tool:** Atomic Red Team

**Atomic Test:**
T1059.001-12 – PowerShell Session Creation and Use

**Command Executed**

```powershell
Invoke-AtomicTest T1059.001 -TestNumbers 12 -Path "C:\Users\azureuser\Desktop\AtomicRedTeam\atomics"
```

The Atomic Red Team simulation launched a PowerShell process that created a PowerShell session on the Windows endpoint.

---

## Detection Method

**Data Source**

- Microsoft Sentinel
- Azure Monitor Agent
- Sysmon

**Log Source**

Microsoft-Windows-Sysmon/Operational

**Event ID**

- Sysmon Event ID 1 – Process Creation

---

## Evidence Collected

**Hostname**

SOC-Windows-VM

**User**

SOC-Windows-VM\azureuser

**Process**

powershell.exe

**Parent Process**

powershell.exe

**Observed Command**

New-PSSession

**Detection Query**

See `detection.kql`

---

## Investigation

A PowerShell process creation event was generated after executing the Atomic Red Team simulation.

The Sysmon event identified:

- The executing user
- The PowerShell executable
- The command line used during execution
- The parent PowerShell process

The event matched the analytics rule and generated an alert in the Microsoft Defender unified SOC experience.

No additional suspicious child processes or persistence mechanisms were observed during this controlled simulation.

---

## Analyst Assessment

This activity was expected because it was generated during an authorized Atomic Red Team test.

In a production environment, unexpected PowerShell session creation should be reviewed to determine whether it represents administrative activity or malicious execution.

---

## Recommended Response

If this activity occurred outside of an authorized test:

1. Verify the user initiated the PowerShell session.
2. Review the complete command line.
3. Examine parent and child processes.
4. Check for additional MITRE ATT&CK techniques.
5. Determine whether lateral movement or persistence occurred.

---

## Lessons Learned

- Sysmon Event ID 1 provides detailed process creation telemetry.
- Microsoft Sentinel successfully ingested the Sysmon event.
- A scheduled analytics rule detected the activity and generated an alert.
- Atomic Red Team is an effective tool for validating endpoint detections.

---
