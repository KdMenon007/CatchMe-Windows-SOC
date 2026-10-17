# Case Track — LNK Delivery and Double-Extension Payload

## Evidence summary

The supplied screenshots and notes describe a separate phishing case:

- Shortcut display name: `Official Website`.
- Shortcut target includes `http://wp16.hqywlqpa.thm:8000/cgi-bin/f` (the screenshot truncates the target; verify the full target in shortcut metadata if available).
- Downloaded executable reported in the notes: `C:\Users\Administrator\Downloads\best-cat.jpg.exe`.
- The suspicious file is also shown in the `C:\Users\Administrator\Pictures` case folder.
- Sysmon process ID reported for the launched payload: `5484`.
- DNS name the malware tried to contact: `rjj.store`.
- A supplied screenshot shows Sysmon Event ID `1` for process image `C:\Users\Administrator\Pictures\best-cat.jpg.exe`; another shows Event ID `22` with `QueryName` `rjj.store` and the same process ID.

The notes distinguish the download path from the later case-folder path. Confirm file movement/extraction and timestamps using the original logs rather than assuming both paths represent one event.

## Investigation steps

1. Inspect the LNK's Target and arguments as inert metadata; do not launch it.
2. Correlate browser/file creation events with the LNK execution and the payload process.
3. Review process parent, command line, hash, signature and user context.
4. Correlate Sysmon Event IDs `1`, `11`, and `22`; check Event ID `3` if network connection telemetry is available.
5. Preserve the executable and shortcut for offline analysis in an isolated lab.

## MITRE ATT&CK mapping (working hypotheses)

- **T1566 — Phishing:** if the email/delivery vector is confirmed.
- **T1204.002 — User Execution: Malicious File:** if user launch is corroborated.
- **T1036.007 — Double File Extension:** `best-cat.jpg.exe` is consistent with a double-extension disguise.
- **T1105 — Ingress Tool Transfer:** only if the network transfer behavior is confirmed by event/command evidence.

## Limitations

The supplied evidence does not independently establish whether the DNS lookup succeeded, whether a network connection completed, whether the payload achieved persistence, or what impact occurred.
