# Incident Response Report — Operation Shadow Persistence

## 1. Case details

- **Case:** CatchMe Project 03 — Operation Shadow Persistence
- **Type:** Windows persistence, account management, and suspicious process activity
- **Severity:** High-priority investigation; final incident severity requires environment-specific impact assessment
- **Status:** Findings documented; final publication review pending
- **Evidence:** Six Windows Security/Sysmon EVTX exports, a ZIP archive, a LNK artifact, and supplied lab notes/screenshots
- **Time basis:** UTC timestamps normalized from parsed event records; verify against original EVTX XML before formal use

## 2. Executive summary

The parsed Windows telemetry records several suspicious behaviors: Security audit-log clearing, automatic service installation, scheduled-task registration, PowerShell-driven executable download and execution, failed and successful remote logons, creation and elevation of a `support` account, and Run-key persistence followed by execution of `kitten.exe`.

A successful remote-interactive Administrator logon and later account-management events share a subject logon ID, supporting session-level correlation. The available evidence does not establish the identity of the human operator. Reviewed DNS events show query attempts with status `9003` and no results; successful DNS resolution or command-and-control has not been established from those records.

## 3. Scope and evidence handling

Six original EVTX files were parsed using `python-evtx` 0.8.1 in an isolated Python environment. The exports yielded 256 parsed records without parser errors. Parsing success does not prove completeness, authenticity, or full coverage of endpoint activity.

SHA-256 values were recorded for all six EVTX files, the suspicious ZIP, and the LNK artifact. The raw evidence and suspicious artifacts are intentionally excluded from this public review package and should remain unchanged in the local evidence store.

An additional `Sysmon (After Reboot) (copy 1).evtx` differs in SHA-256 from `Sysmon (After Reboot).evtx`. Preserve and analyze these as separate sources until provenance is established.

## 4. Findings

### 4.1 Audit-log clearing

Security Event ID 1102 is present in two separate exports: `Security (All Time).evtx` at 2025-07-02 19:08:23.061308 UTC and `Security.evtx` at 2025-07-02 20:57:30.028061 UTC. Treat them as records from separate source files; verify host and collection context before deciding whether they represent separate actions or overlapping views.

### 4.2 Service installation

Security Event ID 4697, Record 12, at 2025-07-02 19:09:16.288832 UTC records installation of `Data Protection Service`, with image path `C:\Windows\Help\nessie.exe`, automatic start, and LocalSystem service account.

### 4.3 Scheduled task

Security Event ID 4698, Record 18, at 2025-07-02 19:10:09.332184 UTC records task `\AmazonSync`, with a boot trigger and action `C:\Program Files\Common Files\troy.exe`. The task principal is LocalSystem. A distinct after-reboot Sysmon export records `troy.exe -d` running as SYSTEM with the Task Scheduler service as parent. Confirm source provenance before making stronger cross-log claims.

### 4.4 PowerShell download and execution

Sysmon Event ID 1, Record 18, at 2025-07-02 20:14:12.680626 UTC records a PowerShell command downloading `http://10.14.97.15/update.exe` to the Administrator AppData Roaming directory and launching it. Sysmon Event ID 11, Record 20, records creation of `C:\Users\Administrator\AppData\Roaming\update.exe`. Sysmon Event ID 1, Record 22, records execution with SHA-256 `B99076499729D423B209C2D434FDB62839D0DF2524A1669085E3C053D8F70C79`.

Sysmon Event ID 22, Record 23, records a DNS query for `route.m365officesync.workers.dev` with status `9003` and no results. This does not establish successful resolution or C2.

### 4.5 Remote logons and account changes

The Security export records six failed NTLM network logons (Event ID 4625, Records 3–8) for `Administrator` from `10.14.97.15`, with substatus `0xc000006a` indicating an incorrect password. Event ID 4624 Records 9–10 then record successful NTLM network logons from that source.

Event ID 4624, Record 20, at 2025-07-02 21:00:27.486475 UTC records a remote-interactive Administrator logon (Logon Type 10) from `10.14.97.15`, target logon ID `0x00000000001cbc41`. Subsequent account-management events share the subject logon ID:
- Event ID 4720, Record 24: `support` created.
- Event ID 4722, Record 25: `support` enabled.
- Event ID 4724, Record 27: password-reset operation recorded for `support`.
- Event ID 4732, Record 30: `support` added to Users.
- Event ID 4732, Record 31: `support` added to Administrators.

This supports a relationship between the remote-interactive session and the account changes. It does not independently identify the operator or prove intent.

Event ID 4648, Record 19, records explicit-credentials activity involving target account `Administrator` and subject `THM-DFIR-VM-3$`. Review full event XML and neighboring telemetry before attributing it to a process or operator.

### 4.6 Run-key persistence and `kitten.exe`

`Sysmon (Before Reboot).evtx.jsonl` records a Run-key command and registry event for value `Basket`, with data `C:\Users\Public\kitten.exe` (Event IDs 1 and 13, Records 15–16, around 2025-07-04 22:09:44–22:09:47 UTC). A separate `Sysmon (After Reboot).evtx.jsonl` records `C:\Users\Public\kitten.exe -d` executing as Administrator at 2025-07-04 22:16:49.013077 UTC, followed by process termination.

A subsequent DNS query for `kitten-c2.apt057.thm` is recorded with status `9003` and no results. The records support a Run-key configuration and later execution in the supplied exports, but do not independently prove the exact reboot mechanism or successful C2.

### 4.7 Additional after-reboot processes

`Sysmon (After Reboot) (copy 1).evtx.jsonl` records `troy.exe -d` executing as SYSTEM with the Task Scheduler service as parent, and `nessie.exe` executing as SYSTEM with `services.exe` as parent. Their SHA-256 values differ, although the records show the same IMPHASH. Similarity alone does not prove identical binaries or a common malware family.

## 5. Response recommendations

If these observations represented a real environment:
1. Preserve logs, disk artifacts, and relevant volatile evidence before remediation where feasible.
2. Isolate the affected endpoint according to incident-response policy.
3. Review remote-access exposure, successful logons, and account activity around the correlated logon session.
4. Validate the `support` account, privileged group memberships, service, scheduled task, Run key, and referenced executables before changing them.
5. Hunt across the environment for the observed file hashes, service/task names, paths, and DNS indicators; confirm scope before blocking.
6. Reset affected credentials and revoke unauthorized access after evidence preservation.
7. Validate detection coverage for audit-log clearing, suspicious service/task creation, suspicious PowerShell download-and-execute behavior, new privileged accounts, and Run-key changes.
8. Document remediation and monitor for recurrence.

## 6. Candidate ATT&CK coverage

Candidate behaviors include Windows service persistence, scheduled-task persistence, registry Run-key persistence, account creation/manipulation, and remote services. Verify technique identifiers and names against current official MITRE ATT&CK documentation before publishing mappings.

## 7. Limitations

- Separate EVTX files may represent different collection windows or host states.
- DNS status `9003` with no results does not establish successful C2.
- Similar IMPHASH values with different SHA-256 hashes do not establish identical binaries.
- The logs do not identify a human operator.
- Scenario flags and lab notes are not independent telemetry.
- The suspicious ZIP/LNK content and screenshot redaction remain outside the scope of this review build.

## 8. Conclusion

The supplied parsed telemetry supports several high-priority persistence and account-management findings. The clearest account-activity correlation is the shared logon ID between the remote-interactive Administrator session and later `support` account changes. Preserve the original evidence, validate fields against raw EVTX/XML, resolve source provenance, and keep hypotheses separate from recorded observations.
