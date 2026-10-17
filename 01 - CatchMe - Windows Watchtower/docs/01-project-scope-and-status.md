# Project Scope and Status

**Project:** CatchMe Project 01 — Windows Watchtower  
**Subtitle:** Windows Security Monitoring & Threat Detection

## Scope

Document the supplied Windows Event Viewer and Sysmon screenshots, PowerShell history snippets, and initial-access case notes. The work emphasizes analyst workflow: evidence preservation, event interpretation, correlation, detection hypotheses, MITRE ATT&CK mapping, and response priorities.

## Case separation

The source material describes several distinct exercises:

- **THM-PC log review:** Security/Sysmon/PowerShell notes associated with `THM-PC` and user `sarah.miller` in the Sysmon artifacts.
- **RDP case:** a separate host shown as `WIN-F89VT9IER10`, with source `203.205.34.107` and workstation `DESKTOP-QNBC4UU` in the notes/screenshots.
- **Phishing binary case:** `www.skype.com` is an executable in a training folder, not a confirmed legitimate website artifact.
- **Phishing LNK case:** `Official Website` shortcut and `best-cat.jpg.exe`; separate from the previous binary case.
- **USB case:** removable-drive executable, dropped file, registry Run value, and a second removable-drive copy.

Do not combine these into a single attack chain unless original event records establish that relationship.

## Current status

- Security log screenshots reviewed; event IDs and several event fields recorded.
- Sysmon screenshots reviewed for process, file, registry, network and DNS events.
- PowerShell PSReadLine history snippets reviewed; attribution remains incomplete.
- Initial-access cases documented as independent tracks.
- Original EVTX exports, full XML, complete timelines, host/timezone confirmation, Elastic ingestion, and tested detections remain pending.

## Investigation principles

Preserve originals; keep screenshot copies unchanged; record source and limitations; correlate by time, host, account, Logon ID, PID/Process GUID and parent process; label observations separately from interpretations; and avoid overclaiming persistence, execution, compromise or live C2 without corroboration.
