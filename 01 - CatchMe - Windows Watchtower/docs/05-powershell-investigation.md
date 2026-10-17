# PowerShell History Investigation — Work in Progress

## History evidence observed

The supplied screenshots show multiple PSReadLine history files. They may belong to different users and sessions, and should not be attributed to an attacker without corroborating event records.

### Administrator profile

Path shown:

`C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`

The first command shown in the history is `Get-ComputerInfo`. Other visible commands include directory navigation, `notepad`, `Get-DnsClientServerAddress`, DNS configuration, `ping google.com`, scheduled-task changes, and RDP/TermService and registry configuration commands. These may be lab setup or operator activity; the history alone does not establish malicious intent.

### `thm.alex` profile

Visible commands include `ls`, `ping 1.1.1.1`, `nslookup google.com`, `ssh root@10.10.5.15`, and `ping 10.10.5.15`. Treat this as a separate profile/session unless host and collection context prove otherwise.

### `thm.bob` profile

Visible commands include navigation into Documents, `get-localuser > users.txt`, `get-smbshare > shares.txt`, `ipconfig /all > network.txt`, a command writing a flag string to `flag.txt`, `get-mppreference > defender.txt`, and `clear-recyclebin`. Preserve the original history and determine the context before attributing these actions.

## Analyst limitations

PSReadLine history is a useful lead, not a complete or tamper-proof record. Commands can be missing, deleted, or run through other mechanisms. The visible screenshots do not establish a reliable full execution timeline.

## Next steps

1. Record which host and account each history file belongs to.
2. Preserve the original history files and compute SHA-256 hashes.
3. Review PowerShell Operational events for the relevant time window.
4. Check Script Block Logging, module logging and transcription status.
5. Correlate commands with process IDs, logon sessions, timestamps and host telemetry.
6. Separate lab-operator setup commands from simulated attacker activity.
