## MITRE ATT&CK Technique

**T1053.005 – Scheduled Task/Job: Scheduled Task**

---

## Atomic Red Team Test

**T1053.005-1 – Scheduled Task Startup Script**

---

## Commands Executed

```cmd
schtasks /create /tn "T1053_005_OnLogon" /sc onlogon /tr "cmd.exe /c calc.exe"

schtasks /create /tn "T1053_005_OnStartup" /sc onstart /ru system /tr "cmd.exe /c calc.exe"
```

---

## Cleanup Commands

```cmd
schtasks /delete /tn "T1053_005_OnLogon" /f >nul 2>&1

schtasks /delete /tn "T1053_005_OnStartup" /f >nul 2>&1
```

---

## Result

Two scheduled tasks were successfully created:

* **T1053_005_OnLogon** — Executes `calc.exe` whenever a user logs on.
* **T1053_005_OnStartup** — Executes `calc.exe` during system startup under the **SYSTEM** account.

This simulated an attacker establishing persistence by creating scheduled tasks that automatically execute after user logon or system startup. Microsoft Sentinel successfully detected the activity using the custom KQL detection and analytics rule.

