# Candidate MITRE ATT&CK Mapping

Mappings below are candidates based on the supplied material; confirm against current ATT&CK documentation and raw evidence before publication.

| Behavior | Candidate technique | Evidence to validate |
|---|---|---|
| Phishing attachment | T1566 — Phishing | Delivery artifact, archive provenance, user interaction |
| RDP access | T1021.001 — Remote Services: Remote Desktop Protocol | Logon events, source, logon type |
| Create account | T1136 — Create Account | Security 4720 |
| Add privileged group member | T1098 — Account Manipulation (confirm specific sub-technique) | Security 4732 |
| Windows service persistence | T1543.003 — Create or Modify System Process: Windows Service | Security 4697, System 7045, service config |
| Scheduled task persistence | T1053.005 — Scheduled Task | Security 4698, task XML |
| Startup folder / Run key | T1547.001 — Registry Run Keys / Startup Folder | Sysmon 11/13, file/registry details |
| C2 | Protocol-specific technique to be determined | DNS, network, process correlation |
| File/process discovery | T1083 / T1057 may apply if confirmed | Process command line and telemetry |
