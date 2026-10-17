# Lab Architecture and Evidence Sources

## Environment

The screenshots show a Windows Server training VM identified in the payload output as `THM-DFIR-VM-2`, with an Administrator session. The exercise uses Sysmon Operational telemetry viewed in Windows Event Viewer.

## Evidence sources shown

| Source | Relevant telemetry | Purpose |
|---|---|---|
| Sysmon Operational | Event ID 1 — Process Create | Identify commands, executable paths, command lines, parent processes, users, PIDs, and process GUIDs. |
| Sysmon Operational | Event ID 22 — DNS Query | Identify queried domains and correlate them to process IDs and the activity timeline. |
| Windows user interface | File listings and browser UI | Corroborate the existence of target files and demonstrate a credential-manager view; sensitive values are redacted in the publication screenshot. |
| Training payload console | Simulated malware output | Contextualize the behaviors the lab payload claims to perform; treat as narrative evidence, not as a substitute for telemetry. |

## Evidence-handling rules

- Preserve original source files separately from publication copies.
- Record host, timezone, source log, event ID, UTC timestamp, process GUID, PID, parent PID, image, command line, user, and DNS query/result where present.
- Do not execute suspicious binaries to reproduce the behavior.
- Never publish credential values, private-key contents, browser databases, stolen archives, or unredacted secrets.
- This package contains screenshot copies only; no raw EVTX or PCAP was provided in this request.
