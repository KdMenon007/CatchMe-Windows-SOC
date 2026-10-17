# Case 01 — Discovery

## Objective
Reconstruct the discovery phase and identify whether multiple reconnaissance commands originate from a suspicious parent process.

## Reported findings
- First command reported by the lab: `whoami`.
- User/account discovery includes `net user Administrator`; output indicates the built-in Administrator account belongs to the local `Administrators` group.
- Other discovery examples in the notes include `ipconfig`, `whoami /priv`, `dir`, `tasklist /v`, `wmic computersystem get model`, `Get-Service`, and checking Defender-related process presence.
- A discovery-related DNS destination was reported as `exfil.beecz.cafe`.

## Evidence to validate
- Sysmon Event ID 1: `Image`, `CommandLine`, `ParentImage`, `ParentCommandLine`, `ProcessId`, `ParentProcessId`, `ProcessGuid`, `ParentProcessGuid`, `User`, and `UtcTime`.
- Sysmon Event ID 22: `QueryName`, `QueryResults`, `ProcessId`, and `UtcTime`.
- Build a process tree using GUIDs when available; PIDs can be reused.

## Analyst conclusion
The reported sequence is consistent with host and user discovery in a controlled training scenario. Confirm the parent process and time window before assigning malicious intent; these commands can also be used by administrators.
