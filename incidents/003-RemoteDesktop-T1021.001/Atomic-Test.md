# Atomic Red Team Simulation

---

## Technique

**T1021.001 – Remote Desktop Protocol (RDP)**

---

## Objective

Simulate an attacker remotely accessing a compromised Windows system using Remote Desktop Protocol (RDP).

---

## Test Environment

| Role                 | System                  |
| -------------------- | ----------------------- |
| **Attacker Machine** | Windows VirtualBox VM   |
| **Target Machine**   | Azure Windows Server VM |

---

## Commands Executed

```powershell
$Server = "<Azure VM Public IP>"
$User = "<Local Administrator Account>"
$Password = "<Password Redacted>"

cmdkey /generic:TERMSRV/$Server /user:$User /pass:$Password
mstsc /v:$Server

echo "RDP connection established"
```

---

## Expected Behavior

* Credentials are securely stored using `cmdkey`.
* Remote Desktop Connection (`mstsc.exe`) initiates an RDP session to the Azure VM.
* Windows generates Remote Desktop authentication events.
* Azure Monitor ingests the security telemetry.
* Microsoft Sentinel processes the events.
* The custom analytics rule generates an alert for investigation.

---

## Validation

The RDP connection was successfully established from the Windows VirtualBox attacker VM to the Azure Windows Server.

Microsoft Sentinel successfully detected the activity and generated an alert using the custom analytics rule.

---

## Cleanup

```powershell
cmdkey /delete:TERMSRV/$Server
```

