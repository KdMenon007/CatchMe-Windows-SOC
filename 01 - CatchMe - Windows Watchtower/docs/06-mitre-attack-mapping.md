# MITRE ATT&CK Mapping — Working Hypotheses

Mappings below are based on the supplied training scenario descriptions and are provisional. Map a technique only when the underlying behavior is supported by evidence; a conceptual diagram or domain name alone is insufficient.

| Case / behavior | Candidate technique | Rationale and caveat |
|---|---|---|
| RDP brute force | **T1110 — Brute Force** | Repeated failed logons followed by a success are consistent with brute force; verify event clustering and source. |
| Exposed RDP access | **T1133 — External Remote Services** | Candidate if RDP was exposed externally and used for access. |
| Successful account use | **T1078 — Valid Accounts** | Candidate where a valid account is used; do not assume the password was guessed solely from one successful logon. |
| Phishing delivery | **T1566 — Phishing** | Apply only if delivery through a phishing message is established. |
| User launches executable | **T1204.002 — User Execution: Malicious File** | Candidate for user-launched executable samples. |
| Double-extension payload | **T1036.007 — Double File Extension** | Applies to `best-cat.jpg.exe` if the file name is confirmed; not automatically applicable to `www.skype.com`. |
| Registry Run key | **T1547.001 — Registry Run Keys / Startup Folder** | Candidate for the USB case's `WinUpdate` Run value; persistence success after reboot remains unproven. |
| Removable media propagation | **T1091 — Replication Through Removable Media** | Candidate for reported copy to the second USB drive; infection of another host is not shown. |
| PowerShell commands | **T1059.001 — PowerShell** | Use only where command execution is attributable to the activity under investigation, not merely present in an operator history. |
| Tool/payload download | **T1105 — Ingress Tool Transfer** | Candidate if full records confirm transfer of a payload into the host. |

## Mapping status

`Provisional — not validated against original EVTX exports`. Update the mapping after reviewing complete event records and documenting the evidence supporting each technique.
