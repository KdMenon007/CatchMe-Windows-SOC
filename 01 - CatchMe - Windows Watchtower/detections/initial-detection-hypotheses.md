# Initial Detection Hypotheses

All rules below are **hypotheses to validate**, not production-ready or tested detections.

## DET-01 — Failed logon burst followed by remote logon
**Data:** Security `4625`, `4624`. Detect a short-window cluster of failures by source/account followed by successful Logon Type `10`. Triage source, account, workstation, status/substatus, time and Logon ID. Tune thresholds to the environment.

## DET-02 — New account added to sensitive groups
**Data:** `4720`, `4728`, `4732`, `4738`. Investigate new-account creation followed by membership changes to groups such as Backup Operators or Remote Desktop Users. Confirm actor, target, group scope and ordering.

## DET-03 — Browser/download to executable execution
**Data:** Sysmon `1`, `11`, and optionally `3`/`22`. Correlate browser/file creation and subsequent execution of suspicious executable paths or double-extension names. Inspect parent, command line, hashes, signature and network activity.

## DET-04 — Unexpected Startup or Run-key persistence
**Data:** Sysmon `11`, `13`, and nearby process events. Investigate new Startup-folder artifacts or registry Run values; inspect the stored target as text and confirm whether it executed.

## DET-05 — DNS query correlated with network connection
**Data:** Sysmon `22`, `3`. Correlate process, domain, DNS result, destination IP/port and timestamps. DNS lookup alone does not prove a successful connection or C2.

## DET-06 — Execution from removable media and propagation
**Data:** Sysmon `1`, `11`, `13`, plus System/device-install logs where available. Investigate executables launched from removable volumes, suspicious files written to system paths, Run-key changes, and copies to other removable drives.

## DET-07 — PowerShell visibility gap
**Data:** PowerShell Operational, PSReadLine, process telemetry. Alerting depends on logging configuration; first document coverage, retention and gaps.

## Validation criteria

For every detection, capture the query, data source, test time range, matched event IDs, expected versus observed result, false-positive review, and status (`not tested`, `tested in lab`, or `validated`). Do not label a rule validated until evidence of a test exists.
