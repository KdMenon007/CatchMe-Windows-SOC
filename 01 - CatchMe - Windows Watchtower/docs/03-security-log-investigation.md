# Security Log Investigation — THM-PC Initial Findings

## Evidence boundary

These observations are transcribed from the supplied screenshots and notes. Original EVTX files have not been supplied for independent parsing.

## Reported scenario indicators

- Host: `THM-PC`.
- Source IP reported as brute-forcing the host: `10.10.53.248`.
- Account reported compromised: `Administrator`.
- Logon ID recorded for the malicious RDP login: `0x183C36D`.
- Account reported created afterward: `svc_sysrestore`.
- Groups reported for the new account: `Backup Operators` and `Remote Desktop Users`.

## Event evidence described

Screenshots show failed logons (`4625`), successful logons (`4624`), logoff (`4634`), explicit-credential logon (`4648`), user/account management events (`4720`, `4724`, `4738`, `4798`), group management events (`4728`, `4732`), and a `1102` Security audit log-clear event in the event list.

## Assessment

The notes describe a sequence consistent with repeated authentication failures followed by remote access and subsequent account/group changes. This is a working hypothesis; full records are needed to establish exact ordering and actor attribution. Event `1102` must be reviewed separately using its full subject, timestamp, and surrounding events; its presence alone does not prove who cleared the log or why.

## Analyst next steps

1. Filter `4625` by source address, target account, logon type and a tight time window.
2. Review candidate `4624` events and capture Logon Type, account, source, workstation and Logon ID.
3. Correlate `4720`, `4728` and `4732` with actor, target, group and time.
4. Inspect the complete `1102` record and surrounding activity.
5. Correlate the session to process and network telemetry without assuming all screenshots represent one host/session.
