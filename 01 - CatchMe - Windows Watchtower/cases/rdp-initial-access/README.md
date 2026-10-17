# Case Track — Initial Access via Exposed RDP

## Scenario

The training scenario describes an Internet-exposed RDP service with weak credentials and repeated login attempts. The supplied screenshots show a Security log with a large number of events and failed/successful logons.

## Reported findings

- Most frequently targeted account in the notes: `Administrator`.
- Source IP reported to have breached the host via RDP: `203.205.34.107`.
- Target host shown in the event details: `WIN-F89VT9IER10`.
- Remote workstation name reported in the notes: `DESKTOP-QNBC4UU`.
- A successful `4624` event shows Logon Type `10` in the screenshot; separate `4625` failures are also visible.

These values come from the supplied case notes/screenshots. Confirm exact event fields, time window and target account from the original EVTX before finalizing attribution.

## Detection workflow

1. Filter Security events for `4625` and group by source IP, target account and short time window.
2. Inspect Logon Type and status/substatus. Type `3` is network logon; type `10` is remote interactive.
3. Search for `4624` after the failed-logon burst and compare account, source IP, workstation and Logon Type.
4. Record Logon ID and use it to investigate subsequent activity where the relevant event exposes the same identifier.
5. Review new accounts, group membership changes, service/task creation, PowerShell and process telemetry after the successful session.

## MITRE ATT&CK mapping (working hypotheses)

- **T1110 — Brute Force:** repeated authentication failures followed by a successful login are consistent with this technique.
- **T1133 — External Remote Services:** exposed RDP may provide initial access.
- **T1078 — Valid Accounts:** successful use of an account may be relevant; confirm credential use and context.

## Response priorities

Restrict RDP exposure to trusted access paths, enforce MFA where supported, use unique strong credentials, review privileged accounts, invalidate compromised credentials, preserve Security logs, and investigate post-logon activity. These are recommended controls, not actions confirmed as performed in this lab.
