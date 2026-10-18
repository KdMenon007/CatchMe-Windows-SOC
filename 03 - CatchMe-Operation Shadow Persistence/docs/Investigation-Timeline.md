# Investigation Timeline (UTC)

Times below are normalized from parsed event timestamps. Before formal publication, verify each event's original EVTX XML, host/time provenance, and timestamp conversion. Events from different exports are kept separate where provenance is unresolved.

| UTC timestamp | Source / event | Observation | Confidence / caveat |
|---|---|---|---|
| 2025-07-02 19:08:23.061 | Security (All Time), Event 1102, Record 1 | Security audit log cleared | Event recorded; operator intent not established |
| 2025-07-02 19:09:16.288 | Security (All Time), Event 4697, Record 12 | `Data Protection Service` installed; image `C:\Windows\Help\nessie.exe`; automatic start; LocalSystem | Service-install event supports configuration |
| 2025-07-02 19:10:09.332 | Security (All Time), Event 4698, Record 18 | `\AmazonSync` task registered with boot trigger and `troy.exe` action | Task registration recorded |
| 2025-07-02 20:14:12.680 | Sysmon, Event 1, Record 18 | PowerShell command downloads and launches `update.exe` from `10.14.97.15` | Command line recorded |
| 2025-07-02 20:14:13.456 | Sysmon, Event 11, Record 20 | `update.exe` created in Administrator AppData Roaming | File-creation event recorded |
| 2025-07-02 20:14:28.351 | Sysmon, Event 1, Record 22 | `update.exe` executes | SHA-256 recorded in findings |
| 2025-07-02 20:14:30.581 | Sysmon, Event 22, Record 23 | DNS query for `route.m365officesync.workers.dev` | Status `9003`; no results |
| 2025-07-02 20:57:30.028 | Security, Event 1102, Record 1 | Security audit log cleared | Separate Security export |
| 2025-07-02 20:58:05–21:00:20 | Security, Event 4625, Records 3–8 | Six failed NTLM network logons for Administrator from `10.14.97.15` | Incorrect-password substatus |
| 2025-07-02 21:00:23.847–21:00:25.459 | Security, Event 4624, Records 9–10 | Successful NTLM network logons from `10.14.97.15` | Network logons, Type 3 |
| 2025-07-02 21:00:27.486 | Security, Event 4624, Record 20 | Administrator remote-interactive logon, Type 10; logon ID `0x00000000001cbc41` | Session correlates with later account-management events |
| 2025-07-02 21:01:38.397–21:01:38.484 | Security, Events 4720, 4722, 4724, 4732, Records 24–30 | `support` created, enabled, password-reset operation recorded, added to Users | Shared subject logon ID |
| 2025-07-02 21:02:29.133 | Security, Event 4732, Record 31 | `support` added to Administrators | Same subject logon ID |
| 2025-07-04 22:09:44.493–22:09:47.424 | Sysmon Before Reboot, Events 1 and 13, Records 15–16 | Run-key value `Basket` set to `C:\Users\Public\kitten.exe` | Persistence configuration recorded |
| 2025-07-04 22:14:07.192 | Sysmon After Reboot (copy 1), Event 1, Record 64 | `troy.exe -d` runs as SYSTEM; parent is Task Scheduler service | Distinct EVTX variant |
| 2025-07-04 22:14:07.196 | Sysmon After Reboot (copy 1), Event 1, Record 69 | `nessie.exe` runs as SYSTEM; parent is `services.exe` | Distinct EVTX variant |
| 2025-07-04 22:14:07.501 | Sysmon After Reboot (copy 1), Event 22, Record 91 | DNS query for `troy-c2.apt057.thm` | Status `9003`; no results |
| 2025-07-04 22:16:49.013 | Sysmon After Reboot, Event 1, Record 20 | `kitten.exe -d` executes as Administrator | Separate after-reboot export |
| 2025-07-04 22:16:51.334 | Sysmon After Reboot, Event 22, Record 25 | DNS query for `kitten-c2.apt057.thm` | Status `9003`; no results |

## Interpretation cautions

- Events from different EVTX files are not assumed to form one uninterrupted chain.
- A DNS query with status `9003` and no results does not establish successful resolution or command-and-control.
- The support-account changes share a logon ID with the remote-interactive session, but that does not identify the human operator.
- Validate original XML fields and time normalization before publication.
