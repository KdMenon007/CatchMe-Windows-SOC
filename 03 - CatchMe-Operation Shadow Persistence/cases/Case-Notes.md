# Consolidated Case Notes

## Account persistence
Supplied notes report six failed Administrator logons, creation of `support`, and addition to `Administrators`. Validate Security 4625, 4624, 4720, 4724, and 4732 with all subject/target fields.

## Service persistence
Lead: `Data Protection Service`, image path `C:\Windows\Help\nessie.exe`. Validate 4697/7045, service configuration, process lineage, and file hash.

## Scheduled task
Lead: `AmazonSync`; supplied notes include learning flag `THM{c2_is_on_schedule!}`. Validate task XML, action, trigger, account, and event 4698.

## Startup and Run key
Examples show `malware.exe` in Startup and `BadRunKey` pointing to `C:\malware.exe`. Validate Sysmon 11/13 and relevant process lineage. Supplied notes mention `Odin` parent `C:\Windows\explorer.exe`, output `Done doing bad stuff!`, and flag `THM{persisting_in_basket!}` for `Kitten`.

## C2
Lead: `update.exe` and `route.m365officesync.workers.dev`; validate process, DNS, and network records. Treat all leads as unconfirmed until raw evidence supports them.

## Final Run-key screenshot (additional evidence)
The latest screenshot shows Sysmon Event ID 13 (`Registry value set`) with RuleName `T1060,RunKey`, process `C:\Windows\System32\reg.exe`, and target Run-key path under the SID ending in `-500`. The value data shown is `C:\Users\Public\kitten.exe`. This supports investigating per-user Run-key persistence; verify the full registry value name, timestamps, and related process events from the raw event log.
