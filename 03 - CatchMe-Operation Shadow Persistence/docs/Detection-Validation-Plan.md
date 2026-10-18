# Detection Validation Plan

## Event pivots
- Security 4625: failed logon; group by account, source/workstation, logon type, and time.
- Security 4624: successful logon; correlate with failures and later account/group changes.
- Security 4720: account creation.
- Security 4724: password reset attempt.
- Security 4732: member added to local security-enabled group.
- Security 4697 and/or System 7045: service installation.
- Security 4698: scheduled task created.
- Sysmon 1: process creation; inspect image, parent, command line, user, hashes, ProcessGuid.
- Sysmon 3: network connection.
- Sysmon 11: file creation, especially Startup paths.
- Sysmon 13: registry value set, especially Run/RunOnce.
- Sysmon 22: DNS query correlated with process and network activity.

## Candidate detections
- New account added to Administrators or Remote Desktop Users shortly after creation.
- Service installation with an unusual or user-writable image path.
- Scheduled task launching an executable/script from an unusual path or running at startup as SYSTEM.
- Executable created in a user's Startup folder.
- Run-key value points to a newly written or suspicious executable.
- Rare process queries a newly observed domain and makes outbound connections.
- Repeated failed remote logons followed by success and account/group changes.

Validate field availability, baselines, thresholds, and false positives before production use.
