# MITRE ATT&CK Mapping

Mappings below are investigative hypotheses based on the training behavior described in the supplied notes. Confirm applicability against the exact technique definitions and source telemetry before final publication.

| Observed/reported behavior | Candidate ATT&CK technique | Rationale / validation |
|---|---|---|
| `whoami`, `net user`, user/group and privilege checks | T1033 — System Owner/User Discovery; T1069 — Permission Groups Discovery | Validate command lines and parent-child process chain in Sysmon Event ID 1. |
| `systeminfo`, `ipconfig`, process/service and network checks | T1082 — System Information Discovery; T1016 — System Network Configuration Discovery; T1057 — Process Discovery; T1007 — System Service Discovery | Confirm each command was observed, not merely listed in the training narrative. |
| Checking for a security product process | T1518.001 — Security Software Discovery | A process-name search is an indicator of discovery intent, but does not prove product absence. |
| Searching for documents and sensitive file locations | T1083 — File and Directory Discovery | Correlate search commands, paths, and subsequent staging activity. |
| Accessing clipboard contents | T1115 — Clipboard Data | Confirm the PowerShell command and process lineage; do not infer clipboard contents from the command alone. |
| Staging collected data in a named directory | T1074.001 — Local Data Staging | Confirm file creation/copy events and whether files actually entered the directory. |
| Downloading tools using browser or command-line utilities | T1105 — Ingress Tool Transfer | Correlate process creation with file creation and network/DNS telemetry. |
| DNS queries to lab-provided domains | Candidate C2/exfiltration behavior; validate against ATT&CK technique definitions | A DNS query alone does not establish C2 or successful exfiltration. Investigate process, response, destination IP, volume, and timing. |

Reference: https://attack.mitre.org/
