# Incident 005 – Remote Desktop Protocol (RDP) Detection

## Objective

Detect unauthorized Remote Desktop Protocol (RDP) activity using Microsoft Sentinel and Windows Security event telemetry.

---

## MITRE ATT&CK Technique

**Technique:** T1021.001 – Remote Desktop Protocol

**Tactic:** Lateral Movement

---

## Attack Simulation

An Atomic Red Team simulation was used to emulate an attacker remotely accessing the target system over Remote Desktop Protocol (RDP). Unlike previous simulations that executed locally on the Azure VM, this attack originated from a separate Windows VirtualBox virtual machine to represent an external attacker connecting to the Azure Windows Server.

The attacker authenticated using valid credentials and successfully established an RDP session with the target endpoint.

This simulated the third stage of the attack lifecycle, where an attacker uses previously obtained credentials to move laterally into a compromised system.

---

## Evidence Collected

| Field              | Value                                        |
| ------------------ | -------------------------------------------- |
| Attacker Host      | Windows VirtualBox VM                        |
| Target Host        | SOC-Windows-VM                               |
| Protocol           | Remote Desktop Protocol (RDP)                |
| Event Source       | Windows Security Event Logs                  |
| Relevant Event IDs | 4624 (Successful Logon), 4625 (Failed Logon) |
| Logon Type         | 10 (RemoteInteractive)                       |

---

## Detection Logic

Microsoft Sentinel monitored Windows Security event logs for Event IDs **4624** (successful logon) and **4625** (failed logon). The detection identified Remote Desktop authentication by searching for **Logon Type 10 (RemoteInteractive)** within the event details, allowing the analytics rule to detect both successful and failed RDP authentication attempts.

Whenever a matching event was identified, Microsoft Sentinel automatically generated an alert for analyst investigation.

---

## KQL Query

See `detection.kql`

---

## Investigation

Investigation confirmed that a remote system initiated an RDP connection to the Azure Windows Server.

The security logs showed a successful **RemoteInteractive (Logon Type 10)** authentication corresponding to the simulated attack. The source workstation and network information matched the Windows VirtualBox attacker machine used during testing.

The activity was verified as authorized because it originated from the controlled Atomic Red Team simulation environment.

---

## Recommended Response

1. Verify whether the remote connection was authorized.
2. Confirm the source IP address, workstation, and user account.
3. Review recent account creation or privilege escalation activity to determine whether the account was legitimately created.
4. Investigate additional lateral movement originating from the same source system.
5. Disable unauthorized accounts and terminate active RDP sessions if malicious activity is confirmed.
6. Reset credentials and review authentication logs for additional suspicious logon attempts.

---

## Lessons Learned

Remote Desktop Protocol is one of the most common techniques used by attackers for lateral movement after obtaining valid credentials.

Monitoring Windows Security events for **Logon Type 10** provides defenders with visibility into remote authentication activity and enables rapid identification of unauthorized access attempts.

While this detection alerts on every successful or failed RDP authentication, a production SOC would typically enhance the rule by incorporating contextual filtering, such as trusted source IP ranges, privileged account monitoring, business-hour baselines, geographic anomalies, or repeated failed authentication attempts to reduce false positives and improve detection fidelity.

This incident also demonstrates how individual detections become more valuable when correlated with previous attacker activity. In this lab, the RDP session leveraged an account created during the earlier **Local Account Creation (T1136.001)** simulation, illustrating how multiple MITRE ATT&CK techniques can be chained together to model a realistic intrusion.
