# Windows Logging Fundamentals

## Core idea

Logs are digital footprints of normal and potentially malicious activity. They help an analyst reconstruct events, identify how activity occurred, investigate incidents, and support operational monitoring.

## Why logs matter

- **Security event monitoring:** detect unusual authentication, account, process, or network activity.
- **Incident investigation and forensics:** build an event timeline and identify possible root cause.
- **Troubleshooting:** diagnose OS, application, and service failures.
- **Performance monitoring:** understand system and application behavior.
- **Auditing and compliance:** retain an activity trail for review.

## Common log categories

- **System:** startup/shutdown, driver loading, OS errors, and hardware/service events.
- **Security:** authentication, authorization, account changes, policy changes, and other audited activity.
- **Application:** application errors, updates, compatibility and application-specific activity.
- **Audit:** data access, system changes, user activity and policy enforcement (often recorded in the Security log or application-specific audit stores).
- **Network:** connection and traffic telemetry from Windows, network devices, or monitoring tools.
- **Access:** resource access records such as web-server, database, or application access logs.

Categories vary by service and product. On Windows, common Event Viewer channels include **Application**, **System**, **Security**, **Setup**, and **Forwarded Events**, plus channels under Applications and Services Logs.

## Event Viewer and event anatomy

Event Viewer is the built-in GUI used to inspect, filter, search, and export Windows events. Important fields include log/channel, provider/source, timestamp, Event ID, level, description, account/subject, target/object, status/reason, process, IP address, and event-specific details. Use the **Details/XML** view when the General tab does not show enough context.

Native EVTX files are normally stored under `C:\Windows\System32\winevt\Logs`.

## Security Event IDs relevant to the supplied cases

| Event ID | Meaning |
|---|---|
| `4624` | Successful logon |
| `4625` | Failed logon |
| `4634` | Logoff |
| `4648` | Logon attempted using explicit credentials |
| `4720` | User account created |
| `4722` | User account enabled |
| `4723` | Attempt to change an account password |
| `4724` | Attempt to reset an account password |
| `4725` | User account disabled |
| `4726` | User account deleted |
| `4728` | Member added to a security-enabled global group |
| `4732` | Member added to a security-enabled local group |
| `4738` | User account changed |
| `4798` | A user's local group membership was enumerated |
| `1102` | Security audit log cleared; investigate context before attribution |

For logons, inspect **Logon Type**, account, source network address, workstation name, status/substatus, and Logon ID. In the supplied RDP case, Logon Type `10` is associated with remote interactive logon. Logon Type `3` is a network logon and is not, by itself, proof of RDP.

## Sysmon Event IDs visible in the screenshots

| Event ID | Meaning |
|---|---|
| `1` | Process creation |
| `3` | Network connection |
| `10` | Process access |
| `11` | File creation |
| `13` | Registry value set |
| `15` | File stream created |
| `22` | DNS query |

Collection depends on Sysmon configuration. Missing events do not prove that an activity did not happen.

## PowerShell visibility

PSReadLine history can preserve commands entered interactively, for example:

`C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`

History is not a complete or tamper-proof audit trail. Check the PowerShell Operational channel and whether Script Block Logging, module logging, and transcription were enabled. Process creation alone may not record each command run inside an already-running PowerShell process.

## Correlation and caveats

Use Logon ID to connect relevant Security events when available. Use Process GUID/PID, parent process, host, user and timestamps to correlate Sysmon process/file/network/registry events. Some Sysmon events lack Logon ID. Confirm timezone and clock accuracy. The supplied screenshots show several different scenarios, so their timestamps must not be merged into one timeline.
