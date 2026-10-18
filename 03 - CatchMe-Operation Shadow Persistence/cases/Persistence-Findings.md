# Persistence Findings — Operation Shadow Persistence

## Scope and confidence

Findings below are based on parsed records from the supplied EVTX exports. Event IDs, record IDs, timestamps, and field values should be checked against the original EVTX/XML before formal external use. The different after-reboot exports are retained as distinct evidence sources.

## Finding 1 — Data Protection Service

- **Source:** `Security (All Time).evtx.jsonl`
- **Event:** Security Event ID 4697, Record ID 12
- **Time:** 2025-07-02 19:09:16.288832 UTC
- **Service:** `Data Protection Service`
- **Image path:** `C:\Windows\Help\nessie.exe`
- **Start type:** Automatic (`2`)
- **Account:** LocalSystem
- **Assessment:** Service installation and configuration are supported by the parsed Security event. Validate service configuration and any available System 7045/Sysmon process events before attributing the action to a specific operator.

## Finding 2 — AmazonSync scheduled task

- **Source:** `Security (All Time).evtx.jsonl`
- **Event:** Security Event ID 4698, Record ID 18
- **Time:** 2025-07-02 19:10:09.332184 UTC
- **Task:** `\AmazonSync`
- **Trigger:** Boot trigger
- **Action:** `C:\Program Files\Common Files\troy.exe`
- **Principal:** LocalSystem (`S-1-5-18`)
- **Assessment:** Task registration is supported by the event. A later process-creation record shows `troy.exe` launched by the Task Scheduler service, supporting a relationship between the task and execution. Confirm the task XML and host/time provenance.

## Finding 3 — PowerShell download and DNS query

- **Source:** `Sysmon.evtx.jsonl`
- **Process event:** Sysmon Event ID 1, Record ID 18, 2025-07-02 20:14:12.680626 UTC
- **Command:** PowerShell downloads `http://10.14.97.15/update.exe` to the Administrator AppData Roaming directory and launches it.
- **File event:** Sysmon Event ID 11, Record ID 20, 2025-07-02 20:14:13.456669 UTC; creates `C:\Users\Administrator\AppData\Roaming\update.exe`.
- **Execution:** Sysmon Event ID 1, Record ID 22, 2025-07-02 20:14:28.351181 UTC; SHA-256 `B99076499729D423B209C2D434FDB62839D0DF2524A1669085E3C053D8F70C79`.
- **DNS:** Sysmon Event ID 22, Record ID 23, 2025-07-02 20:14:30.581535 UTC; query `route.m365officesync.workers.dev`, status `9003`, no results.
- **Assessment:** The command, file creation, process execution, and DNS query attempt are recorded. The reviewed DNS event does not establish successful resolution or C2 communication.

## Finding 4 — Support account creation and administrator membership

- **Source:** `Security.evtx.jsonl`
- **Log clearing:** Event ID 1102, Record ID 1, 2025-07-02 20:57:30.028061 UTC.
- **Failed logons:** Six Event ID 4625 records, Records 3–8, from `10.14.97.15` targeting `Administrator`; NTLM network logons with substatus `0xc000006a` (incorrect password).
- **Successful logons:** Event ID 4624 Records 9–10 are successful NTLM network logons from the same source.
- **Remote-interactive logon:** Event ID 4624, Record ID 20, 2025-07-02 21:00:27.486475 UTC; Logon Type 10, target account `Administrator`, source `10.14.97.15`, target logon ID `0x00000000001cbc41`.
- **Account activity:** Event ID 4720 Record 24 creates `support`; Event ID 4722 Record 25 enables it; Event ID 4724 Record 27 records a password-reset operation; Event ID 4732 Record 30 adds it to Users; Event ID 4732 Record 31 adds it to Administrators.
- **Correlation:** The account-management actions share the subject logon ID associated with the remote-interactive session.
- **Assessment:** The session-level correlation is strong. The event records do not independently establish the identity or intent of the human operator.

## Finding 5 — Basket Run-key persistence and kitten.exe execution

- **Source:** `Sysmon (Before Reboot).evtx.jsonl`
- **Events:** Event ID 1, Record ID 15, at 2025-07-04 22:09:44.493685 UTC; Event ID 13, Record ID 16, at 2025-07-04 22:09:47.424625 UTC.
- **Registry value:** `Basket` under the Administrator user's Run key.
- **Value data:** `C:\Users\Public\kitten.exe`.
- **Follow-up:** `Sysmon (After Reboot).evtx.jsonl` records `C:\Users\Public\kitten.exe -d` executing as Administrator at 2025-07-04 22:16:49.013077 UTC, followed by a process-termination event.
- **DNS:** A query for `kitten-c2.apt057.thm` is recorded with status `9003` and no results.
- **Assessment:** The Run-key configuration and subsequent process execution are recorded in the supplied sources. The logs alone do not independently prove the exact reboot mechanism or successful C2.

## Finding 6 — Post-reboot service-associated processes

- **Source:** `Sysmon (After Reboot) (copy 1).evtx.jsonl`
- **Troy:** Event ID 1, Record ID 64, 2025-07-04 22:14:07.192060 UTC; `troy.exe -d` running as SYSTEM with parent Task Scheduler service (`svchost.exe ... -s Schedule`); SHA-256 `B7BC2BC792E164D019B705CFE6EF99F3C50004E85FD2791B47223F534DC47E92`.
- **Nessie:** Event ID 1, Record ID 69, 2025-07-04 22:14:07.196959 UTC; `nessie.exe` running as SYSTEM with parent `services.exe`; SHA-256 `07F4C2B083282F1BBF2F0F2F4BF06750DE7D8B3B23AB1DB3D3B3F8F3A9352FDD`.
- **Hash note:** Both records show the same recorded IMPHASH but different SHA-256 hashes. Similarity alone does not prove identical binaries or a shared malware family.
- **DNS:** `troy.exe` queried `troy-c2.apt057.thm`; status `9003`, no results.
- **Assessment:** The process records are consistent with execution through the registered task and service. Keep the additional after-reboot EVTX distinct until provenance is confirmed.

## Limitations

- A DNS query attempt is not proof of successful resolution, beaconing, or C2.
- Event ordering across separate exports does not prove a single uninterrupted attack chain.
- Scenario flags and learning notes are not independent telemetry.
- The evidence does not establish the identity of an operator.
- Preserve original EVTX, ZIP, and LNK artifacts unchanged in the local evidence store.
