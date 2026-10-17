# Investigation Query Plan

These are **query objectives**, not validated executable KQL. Confirm Elastic Agent/integration status, dataset names, timestamp fields and actual field mappings before implementation.

## Security authentication and account changes

- Find Event IDs `4625` and `4624`; group by source IP, target account, Logon Type and time window.
- For RDP, investigate `4624` Logon Type `10`; use source IP and workstation name as corroborating fields.
- Search `4720`, `4723`, `4724`, `4725`, `4726`, `4728`, `4732` and `4738` for account and group changes.
- Review `1102` independently and capture actor, time, host and surrounding events.
- Correlate Security events by Logon ID where present.

## Sysmon

- Event ID `1`: process image, parent image, command line, user, hashes, Process GUID/PID.
- Event ID `3`: outbound connection, source/destination IP and port, process identity.
- Event ID `10`: process access source/target and access context.
- Event ID `11`: downloaded/dropped files and Startup/USB paths.
- Event ID `13`: Run-key value name and data, process, user and registry path.
- Event ID `15`: alternate data stream details if relevant.
- Event ID `22`: DNS query, result and process context.

## Case-specific pivots

- THM-PC: `ckjg.exe`, `DeleteApp.url`, `193.46.217.4:7777`, `hkfasfsafg.click`.
- RDP case: `203.205.34.107`, `WIN-F89VT9IER10`, `DESKTOP-QNBC4UU`, Administrator and Logon Type `10`.
- Phishing LNK: `best-cat.jpg.exe`, PID `5484`, `rjj.store`, LNK target URL.
- USB: `E:\Open Sandisk 4GB USB.exe`, `C:\Users\Public\Documents\winupdate.exe`, Run value `WinUpdate`, `F:\Open Data Traveler 32 GB USB.exe`.

Keep each case in its own time window and host context. Do not cross-correlate indicators between separate training cases without evidence.

## PowerShell

Check availability of `Microsoft-Windows-PowerShell/Operational`, then review script-block/module events and transcripts if enabled. Compare the user-specific PSReadLine histories to the event telemetry; neither source is complete on its own.

## Query implementation gate

1. Confirm Elastic Agent is enrolled and the Windows/Sysmon integrations are healthy.
2. Inspect actual documents in Discover for each dataset.
3. Verify event ID and timestamp field mappings from real events.
4. Test the query against the correct case time range and host.
5. Record test results, false positives, required fields, and validation status.
