# Executive Summary — Operation Shadow Persistence

The parsed Windows Security and Sysmon exports record several suspicious behaviors requiring investigation: Security audit-log clearing, installation of an automatically starting service, registration of a boot-triggered scheduled task, PowerShell-driven download and execution of `update.exe`, remote Administrator logons, creation and elevation of a `support` account, and Run-key persistence followed by execution of `kitten.exe`.

The account-management events share a subject logon ID with a recorded remote-interactive Administrator session, supporting session-level correlation. This does not identify the human operator. DNS queries reviewed for the suspicious processes have status `9003` and no results; successful command-and-control is not established by those records.

## Priority actions

1. Preserve original EVTX and suspicious artifacts unchanged.
2. Verify event XML, timestamps, host provenance, and collection windows.
3. Investigate the service, scheduled task, Run key, and account changes.
4. Review remote access and credentials associated with the correlated session.
5. Validate detection coverage and hunt for related indicators.
6. Confirm ATT&CK mappings and sanitize screenshots before public release.

## Evidence caveats

Six EVTX files produced 256 parsed records without parser errors. This does not establish log completeness or authenticity. The extra `Sysmon (After Reboot) (copy 1).evtx` has a different SHA-256 from the other after-reboot file and must remain a distinct evidence source until provenance is resolved.

The original EVTX, ZIP, LNK, and parsed-log files are not included in this public review build.
