## MITRE ATT&CK Techniques

**T1053.005 – Scheduled Task**

**T1546.012 – Image File Execution Options (IFEO) Injection**

---

## Atomic Red Team Tests

**T1053.005-1 – Scheduled Task Startup Script**

**T1546.012-1 – IFEO Injection**

---

## Commands Executed

### Scheduled Task

```powershell id="q6xjlwm"
Invoke-AtomicTest T1053.005 -TestNumbers 1 -Path "C:\Users\azureuser\Desktop\AtomicRedTeam\atomics"
```

### IFEO Injection

```powershell id="r0g0q2o"
Invoke-AtomicTest T1546.012 -TestNumbers 1 -Path "C:\Users\azureuser\Desktop\AtomicRedTeam\atomics"
```

---

## Attack Chain

```text id="zljw3w"
User Logon
      ↓
Scheduled Task Executes
      ↓
calc.exe Launch Attempt
      ↓
IFEO Registry Intercepts Execution
      ↓
cmd.exe Executes Instead
```

---

## Result

The Scheduled Task and IFEO Injection techniques were successfully chained together to simulate an advanced persistence scenario.

When a user logged on, the scheduled task attempted to launch **calc.exe**. The previously configured **Image File Execution Options (IFEO)** registry key intercepted the execution request and redirected it to **cmd.exe**, demonstrating how attackers can hijack legitimate application launches to execute arbitrary programs.

Microsoft Sentinel successfully detected both the scheduled task activity and the IFEO registry modification. A custom correlation analytics rule generated an alert when the Scheduled Task and IFEO events occurred on the same host within a 10-minute window, simulating detection of a multi-stage persistence attack.

