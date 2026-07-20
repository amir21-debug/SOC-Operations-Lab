## MITRE ATT&CK Technique

**T1059.001 – Command and Scripting Interpreter: PowerShell**

---

## Atomic Red Team Test

**T1059.001-12 – PowerShell Session Creation and Use**

---

## Command Executed

```powershell
Invoke-AtomicTest T1059.001 -TestNumbers 12 -Path "C:\Users\azureuser\Desktop\AtomicRedTeam\atomics"
```

---

## Purpose

Simulate PowerShell execution activity to validate Microsoft Sentinel detections and analytics rules for suspicious PowerShell execution.
