# Evidence Register

Evidence consists of screenshots supplied in the conversation and user-provided notes. Original EVTX files, binaries and shortcut files have not been provided for independent parsing. Screenshot files are retained as received; descriptive filenames indicate their topic, not a forensic chain-of-custody guarantee.

| ID | File | Evidence summary | Limitations |
|---|---|---|---|
| EV-01 | `EV-01-supplied-screenshot.png` | Event Viewer Security overview | No full event XML |
| EV-02 | `EV-02-supplied-screenshot.png` | Failed logon details; notes identify `4625`, Logon Type `3`, source `10.10.53.248` | Verify complete fields and case context |
| EV-03 | `EV-03-supplied-screenshot.png` | Security event list and authentication events | Context is partial |
| EV-04 | `EV-04-supplied-screenshot.png` | Authentication details including Logon ID reported as `0x183C36D` in notes | Verify exact source/time/account |
| EV-05 | `EV-05-supplied-screenshot.png` | Account-management events including `4720` and target `svc_sysrestore` | Full event details needed |
| EV-06 | `EV-06-supplied-screenshot.png` | Group membership event; notes report `Backup Operators` | Full group event needed |
| EV-07 | `EV-07-supplied-screenshot.png` | Group membership event; notes report `Remote Desktop Users` | Full group event needed |
| EV-08 | `EV-08-supplied-screenshot.png` | Sysmon Event ID `15` file stream artifact with `ckjg.exe` context | Full record and file analysis needed |
| EV-09 | `EV-09-supplied-screenshot.png` | Sysmon Event ID `3` destination `193.46.217.4:7777` | Confirm process and connection outcome |
| EV-10 | `EV-10-supplied-screenshot.png` | Sysmon Event ID `22` DNS name `hkfasfsafg.click` | DNS result/status requires full record |
| EV-11 | `EV-11-supplied-screenshot.png` | Sysmon Event ID `11` Startup `DeleteApp.url` path | Persistence effectiveness not established |
| EV-12 | `EV-12-supplied-screenshot.png` | Sysmon Event ID `1` process creation | Need full command line/parent details |
| EV-13 | `EV-13-supplied-screenshot.png` | Sysmon event list with process/network/file/registry activity | Partial context |
| EV-14 | `EV-14-supplied-screenshot.png` | Administrator PSReadLine history | History may include operator setup commands |
| EV-15 | `EV-15-supplied-screenshot.png` | PSReadLine history file properties | Does not prove completeness |
| EV-16 | `EV-16-supplied-screenshot.png` | `thm.alex` PowerShell history snippet | Host/session attribution pending |
| EV-17 | `EV-17-supplied-screenshot.png` | `thm.bob` PowerShell history snippet | Host/session attribution pending |
| EV-18 | `EV-18-initial-access-exposed-services-overview.png` | Training diagram: exposed RDP/mail/MS SQL paths | Concept diagram, not host evidence |
| EV-19 | `EV-19-initial-access-user-driven-overview.png` | Training diagram: phishing and removable media | Concept diagram, not host evidence |
| EV-20 | `EV-20-rdp-security-failed-and-successful-logons.png` | RDP Security log with `4625` and `4624` events | Need full event XML/time window |
| EV-21 | `EV-21-rdp-successful-logon-source-and-workstation.png` | RDP event details; notes identify source `203.205.34.107`, workstation `DESKTOP-QNBC4UU` | Confirm exact event and host fields |
| EV-22 | `EV-22-phishing-case1-misleading-extension-executable.png` | `www.skype.com` shown as MS-DOS Application; training flag shown | Does not establish delivery vector or impact |
| EV-23 | `EV-23-phishing-lnk-shortcut-target.png` | `Official Website` shortcut properties and visible target prefix | Target is truncated in screenshot |
| EV-24 | `EV-24-phishing-case3-files-and-sysmon-log.png` | Phishing Case 3 files and Sysmon EVTX file listing | File contents not supplied |
| EV-25 | `EV-25-phishing-case3-sysmon-process-creation.png` | Sysmon Event ID `1`, `best-cat.jpg.exe`, PID `5484` | Full process context needed |
| EV-26 | `EV-26-phishing-case3-sysmon-dns-query.png` | Sysmon Event ID `22`, query `rjj.store`, PID `5484` | DNS success/network connection not established |
| EV-27 | `EV-27-usb-sysmon-registry-persistence.png` | USB case registry Run-key value evidence | Verify exact hive/value details from XML |
| EV-28 | `EV-28-usb-runkey-payload-value.png` | Run value `WinUpdate` points to `C:\Users\Public\Documents\winupdate.exe` | Does not prove execution after reboot |
| EV-29 | `EV-29-usb-sysmon-file-copy.png` | USB executable and file-creation event; notes report second USB path | Does not prove another host was infected |

## Chain of custody — pending

For original exports: record collector, source host, local timezone, export time, original path and SHA-256. Keep originals read-only and separate from redacted/public copies. These screenshot copies are portfolio references, not a substitute for raw event exports.


## EVTX publication note

EVTX files are included by explicit project-owner choice. Review the event contents for identifiers and confirm redistribution is permitted before making the repository public. The presence of a log file in this portfolio is not, by itself, proof that all records have been independently validated.
