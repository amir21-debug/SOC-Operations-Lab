# Incident 002 - Local Administrator Account Creation

## Objective

Detect unauthorized creation of local user accounts that could be used by an attacker for persistence or privilege escalation.

## MITRE ATT&CK Technique

T1136.001 - Create Account: Local Account

## Attack Simulation

Atomic Red Team was used to simulate an attacker creating a new local administrator account on the Windows endpoint.

Atomic Test:

T1136.001-8 - Create a new Windows admin user

The simulation created the following account:

Account Name:
T1136.001_Admin

The account was added to the local Administrators group.

## Evidence Collected

Hostname:

SOC-Windows-VM

User Performing Action:

SOC-Windows-VM\azureuser

Created Account:

T1136.001_Admin

Event Source:

Windows Security Auditing

Event IDs:

4720 - A user account was created

4732 - A member was added to a security-enabled local group

Log Source:

Microsoft Sentinel

## Detection Logic

The detection rule identifies Windows Security events associated with local account creation.

The analytics rule triggers when Event ID 4720 is detected.

## KQL Query

See detection.kql

## Investigation

The investigation identified that the azureuser account created a new local administrator account.

The activity was correlated with Atomic Red Team execution.

The created account:

T1136.001_Admin

was added to the Administrators group, giving it elevated privileges.

Creating unauthorized administrator accounts is a common attacker persistence technique.

## Response

1. Validate whether the account creation was authorized.
2. Identify the user responsible for creating the account.
3. Review command-line activity related to account creation.
4. Remove unauthorized accounts.
5. Review additional persistence mechanisms.

## Lessons Learned

Local account creation is a common persistence technique used by attackers.

Monitoring Windows Security events such as Event ID 4720 and 4732 provides visibility into unauthorized account creation.

Microsoft Sentinel can correlate endpoint telemetry and generate alerts when suspicious account activity occurs.
