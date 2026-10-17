# Initial Findings Report

## Executive summary

The supplied evidence contains several independent Windows training investigations. The THM-PC review includes reported failed logons, a successful remote session, subsequent account/group changes, a suspicious executable, a Startup-folder artifact and DNS/network indicators. Separate case materials cover an RDP brute-force scenario, a misleading-name executable, a phishing LNK/double-extension payload, and removable-media execution/persistence.

These are initial observations and hypotheses, not a single confirmed incident. Original EVTX records and artifacts have not been provided for independent validation.

## Finding set A — THM-PC Security/Sysmon

- Notes report `10.10.53.248` brute-forcing `THM-PC`, with Administrator described as compromised and Logon ID `0x183C36D` associated with the RDP session.
- Notes identify `svc_sysrestore` and group membership involving `Backup Operators` and `Remote Desktop Users`.
- Sysmon screenshots/notes identify `ckjg.exe`, `DeleteApp.url`, `193.46.217.4:7777`, and `hkfasfsafg.click`.
- PSReadLine history is available for review, but some commands may be lab setup/operator activity and attribution is unresolved.

## Finding set B — RDP case

- Notes identify Administrator as the most-targeted account, source `203.205.34.107`, target host `WIN-F89VT9IER10`, and remote workstation `DESKTOP-QNBC4UU`.
- Screenshots show failed logons and a successful logon with Logon Type `10`.
- Confirm all fields from full EVTX records before concluding the compromise sequence.

## Finding set C — Phishing binary

- A training executable named `www.skype.com` is shown as an MS-DOS Application and produces the flag `THM{misleading_extension}` when run in the exercise.
- The screenshot supports a misleading-name demonstration, not the real-world origin or impact of the file.

## Finding set D — Phishing LNK

- `Official Website` shortcut target includes `http://wp16.hqywlqpa.thm:8000/cgi-bin/f` (visible target is truncated in the screenshot).
- Notes report `best-cat.jpg.exe`, process ID `5484`, and DNS name `rjj.store`.
- Confirm the download/extraction path and network outcome from full records.

## Finding set E — USB

- Notes identify `E:\Open Sandisk 4GB USB.exe`, dropped file `C:\Users\Public\Documents\winupdate.exe`, Run-key value `WinUpdate`, and copy `F:\Open Data Traveler 32 GB USB.exe`.
- This supports investigation of removable-media execution, autorun persistence, and propagation, but not proof of successful reboot persistence or infection of another host.

## Priority actions

1. Obtain original EVTX files and record host/timezone/collection metadata.
2. Hash and preserve available artifacts without executing them.
3. Reconstruct a separate timeline for each case.
4. Correlate logon sessions, account/group changes, process identifiers, registry values, file paths and network events.
5. Inspect PowerShell Operational coverage and each user-specific history context.
6. Verify Elastic ingestion and actual field mappings before writing KQL.
7. Test detection hypotheses safely and record validation evidence.

## Confidence statement

Screenshots support the existence of the displayed event types and selected fields. Attack-chain interpretations are derived from user notes and the training scenario context. The evidence does not independently establish current C2 status, payload impact, successful persistence after reboot, or propagation to another endpoint.
