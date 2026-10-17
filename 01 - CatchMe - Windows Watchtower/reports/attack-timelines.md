# Case-Specific Timelines (Preliminary)

The supplied screenshots contain different dates, hosts and training cases. These timelines are intentionally separate; exact timestamps must be confirmed from full event records.

## A. THM-PC Security/Sysmon

1. Repeated failed logons reported from `10.10.53.248`.
2. Successful remote logon reported for `Administrator`, Logon ID `0x183C36D`.
3. `svc_sysrestore` reportedly created and added to `Backup Operators` and `Remote Desktop Users`.
4. Sysmon evidence shows `ckjg.exe` activity, a Startup-folder `.url` artifact, DNS query `hkfasfsafg.click`, and destination `193.46.217.4:7777`.
5. PowerShell history review began; first command in the Administrator history screenshot is `Get-ComputerInfo`.

**Ordering caveat:** Steps above are from supplied notes and should not be treated as a verified timestamp-ordered chain until the original records are correlated.

## B. RDP case

1. Repeated `4625` failed-logon events appear in the RDP Security log screenshot.
2. A `4624` successful logon with Logon Type `10` is shown.
3. Notes identify source `203.205.34.107`, target `WIN-F89VT9IER10`, and workstation `DESKTOP-QNBC4UU`.

## C. Misleading-extension binary case

1. `www.skype.com` appears in a training folder as an MS-DOS Application.
2. The sample is launched in the exercise and prints a message plus `THM{misleading_extension}`.

## D. Phishing LNK case

1. `Official Website` shortcut is present; visible target prefix points to a training URL.
2. Notes report download to `C:\Users\Administrator\Downloads\best-cat.jpg.exe` and a case-folder copy/path under `Pictures`.
3. Sysmon Event ID `1` is shown for PID `5484` and the payload path.
4. Sysmon Event ID `22` shows query name `rjj.store` for PID `5484`.

## E. USB case

1. Executable launched from `E:\Open Sandisk 4GB USB.exe`.
2. Sysmon records show a Run-key value `WinUpdate` pointing to `C:\Users\Public\Documents\winupdate.exe`.
3. A file-creation event shows `F:\Open Data Traveler 32 GB USB.exe`.

**Caution:** The timelines above are reconstructed from notes/screenshots, not raw event exports. Validate each event timestamp, host and process identity before publishing as a definitive chronology.
