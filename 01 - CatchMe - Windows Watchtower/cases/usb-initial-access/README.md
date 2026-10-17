# Case Track — Initial Access via Removable Media

## Evidence summary

The supplied notes and Sysmon screenshots describe a removable-media training case:

- Executable launched from USB: `E:\Open Sandisk 4GB USB.exe`.
- Dropped file: `C:\Users\Public\Documents\winupdate.exe`.
- Sysmon Event ID `13` shows a Run-key value named `WinUpdate` pointing to `C:\Users\Public\Documents\winupdate.exe` under a user hive path.
- Another file-creation screenshot shows a copy on a second drive: `F:\Open Data Traveler 32 GB USB.exe`.

The evidence supports investigating execution from removable media, a registry autorun value, and copying to another removable drive. It does not by itself prove successful reboot persistence or that the second USB infected another computer.

## Investigation steps

1. Review Sysmon Event ID `1` to confirm process image and parent process for the executable launched from `E:`.
2. Review Event ID `11` for creation of `winupdate.exe` and the file written to `F:`.
3. Review Event ID `13` for the exact registry path, value name, data, process, user and timestamp.
4. Correlate with system/device-install logs if available to establish removable-device insertion and device identity.
5. Hash files and preserve copies for isolated offline analysis. Do not execute them.

## MITRE ATT&CK mapping (working hypotheses)

- **T1091 — Replication Through Removable Media:** consistent with the reported copying to another USB drive.
- **T1204.002 — User Execution: Malicious File:** relevant if a user launched the USB executable.
- **T1547.001 — Registry Run Keys / Startup Folder:** relevant to the reported `WinUpdate` Run-key value.

## Response priorities

Isolate the affected host from the network if authorized, quarantine involved removable media, preserve event logs and files, check other systems that used the drives, and review endpoint protection telemetry. These are recommendations, not confirmed completed actions.
