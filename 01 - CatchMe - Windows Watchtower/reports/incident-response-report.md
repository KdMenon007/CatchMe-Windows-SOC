# Incident Response Report — Windows Watchtower (Initial / Multi-Case)

## Status

**Preliminary.** This report summarizes multiple independent training cases. It is not a declaration that the evidence represents one real-world incident.

## Objectives

- Establish what the supplied logs/screenshots show.
- Identify evidence gaps and working attack hypotheses.
- Define containment, investigation and detection-validation priorities.

## Case A — THM-PC Security/Sysmon review

**Reported indicators:** `10.10.53.248`, `THM-PC`, `Administrator`, Logon ID `0x183C36D`, `svc_sysrestore`, `Backup Operators`, `Remote Desktop Users`, `ckjg.exe`, `DeleteApp.url`, `193.46.217.4:7777`, `hkfasfsafg.click`.

**Assessment:** Notes describe authentication abuse and subsequent account/group changes, alongside endpoint telemetry that warrants investigation. Correlation with original event XML is required.

## Case B — RDP Initial Access

**Reported indicators:** target `WIN-F89VT9IER10`, source `203.205.34.107`, workstation `DESKTOP-QNBC4UU`, account `Administrator`, successful Logon Type `10`.

**Assessment:** The combination is consistent with a possible remote-access breach after repeated failures. Confirm source/target fields, timestamps and Logon ID from the complete Security records.

## Case C — Misleading-extension binary

**Observed training result:** file `www.skype.com` is displayed as an MS-DOS Application; the exercise output includes `THM{misleading_extension}`.

**Assessment:** Demonstrates a misleading filename/file-type presentation. Delivery vector, persistence and impact are not established by the screenshot alone.

## Case D — Phishing LNK and double-extension payload

**Reported indicators:** `Official Website` shortcut; visible target prefix `http://wp16.hqywlqpa.thm:8000/cgi-bin/f`; `best-cat.jpg.exe`; PID `5484`; DNS query `rjj.store`.

**Assessment:** The evidence is consistent with a shortcut-led payload download/execution scenario. Verify full LNK target, parent process, file movement, DNS result and any subsequent network connection.

## Case E — USB initial access

**Reported indicators:** `E:\Open Sandisk 4GB USB.exe`; `C:\Users\Public\Documents\winupdate.exe`; registry Run value `WinUpdate`; `F:\Open Data Traveler 32 GB USB.exe`.

**Assessment:** The screenshots support reviewing execution from removable media, a dropped executable, a Run-key value and a file written to another removable drive. Successful persistence and cross-host infection are not proven.

## Recommended response lifecycle

1. **Preserve:** collect Security, System, Sysmon and PowerShell Operational EVTX; preserve relevant history files, LNK metadata, suspicious file metadata and registry evidence; hash originals.
2. **Scope:** identify host, user, local timezone, time window and whether evidence tracks are independent. Do not combine unrelated scenario timelines.
3. **Contain (if this were an active authorized environment):** restrict exposed RDP; disable/rotate compromised credentials; isolate affected endpoint; quarantine suspicious files and involved removable media; prevent further execution.
4. **Investigate:** reconstruct authentication, process, file, registry and network sequences. Check account/group changes and potential lateral movement with corroborating logs.
5. **Eradicate/recover:** only after validation, remove unauthorized persistence, reset credentials, reimage where warranted, and restore from known-good sources.
6. **Improve detections:** validate failed-logon burst, sensitive group addition, suspicious download/execution, Run-key/Startup artifact, DNS/network correlation, and removable-media execution hypotheses.

These actions are recommendations; the package does not claim they have been performed.

## Evidence limitations

- Original EVTX files are not included.
- Screenshots may omit fields, timestamps or surrounding events.
- PowerShell history may be incomplete and cannot reliably attribute commands by itself.
- Threat-intelligence/reputation checks were not performed.
- Elastic ingestion, KQL queries and detection rules remain unvalidated.

## Closure criteria

Close each case only after full records are correlated, key indicators and timeline are verified, containment/eradication decisions are documented where applicable, detection tests are recorded, and evidence hashes/chain of custody are captured.
