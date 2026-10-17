
---

# High-level Initial Access taxonomy

1. **Exposed services** (internet-reachable): RDP, mail, MSSQL, web apps.
    
    - MITRE examples: **T1133 External Remote Services**, **T1190 Exploit Public-Facing App**.
        
    ![[Pasted image 20251111061330.png]]
    
1. **User-driven** (social engineering & removable media): phishing attachments, LNK shortcuts, USB.
    
    - MITRE examples: **T1566 Phishing**, **T1091 Removable Media**.
        

![[Pasted image 20251111061353.png]]

---

# Exposed services — what to watch for (RDP example)

**Scenario:** RDP exposed, weak creds (`Administrator:Summer2025`), botnet brute-force → successful RDP → actor moves laterally.


### Key Windows events to use (Security log)

- **4625** = failed logon (use to detect brute force).
    
    - Filter by **Logon Type** 3 (network) and 10 (remote/terminal services).
        
    - Filter by **Source Network Address** (external IPs).
        
- **4624** = successful logon.
    
    - Check **Logon Type** 10 for interactive RDP.
        
    - Capture **Account Name**, **Account Domain**, **Logon ID**, **Source Network Address**.
        
- Use **time clustering**: many 4625s in short window => brute force.
    

### Analyst playbook — detect RDP brute force → access → activity

1. In Security logs: search events `EventID=4625` with LogonType IN (3,10) and source IP ≠ internal ranges.
    
    - Look for high frequency (dozens/ hundreds in minutes).
        
2. On success: search `EventID=4624` for same account + source IP + LogonType=10 → identify successful compromise.
    
3. Copy **Logon ID** from the 4624 event.
    
4. Correlate with **Sysmon** logs (or Process Creation logs) searching for processes where **SubjectLogonId** or CreatorLogonId matches the Logon ID — this shows processes spawned by the remote session.
    
5. Hunt for suspicious binaries, PowerShell usage, lateral tools, scheduled tasks, service installs after that Logon ID.
    

---

# Phishing (attachments) — binary & LNK attack chains

Phishing commonly arrives as: zipped archives → double extensions (.pdf.exe), uncommon exes (.com, .scr), or LNK shortcuts pointing to scripts.

### Sysmon event chain to detect malicious download & execution (what to look for)

- **Sysmon Event ID 1** — Process creation (browser launched): `Image` = browser (Edge/Chrome).
    
- **Sysmon Event ID 11** — File creation/created in Downloads (archive or shortcut).
    
    - `TargetFilename` shows `C:\Users\<user>\Downloads\invoice.zip`, `*.lnk`, or `invoice.pdf.exe`.
        
- **Sysmon Event ID 1** — Process creation for unpacker/extractor (`7zG.exe`, `explorer.exe`) when archive extracted.
    
- **Sysmon Event ID 1** — Process creation for the malicious payload: `C:\Users\<user>\Downloads\invoice.pdf.exe` or `C:\ProgramData\r.exe`.
    
- Look for browser → download → unpack → execution chain within a short window.
    
![[Pasted image 20251111061509.png]]

### LNK-based payloads (special case)

- LNK execution often shows `explorer.exe` as parent and **PowerShell/cmd.exe** in the commandline. LNK itself leaves a FileCreate event when downloaded.
    
- **Detection:** find `FileCreated` events for `*.lnk` in Downloads followed shortly by `ProcessCreate` where `ParentImage` = `explorer.exe` and `Image` = `powershell.exe` (or `cmd.exe`) with suspicious `-EncodedCommand`, `IEX`, `DownloadFile`, or `Invoke-Expression` patterns.
    
- Also search for `*.lnk` whose `Target` contains `powershell -c` or web download commands.
    

### Example analyst steps for Phishing case files (the ones you have)

- Open `Phishing-Sysmon.evtx` in Event Viewer. Filter:
    
    - Sysmon EventID=11 for `TargetFilename` containing `Downloads` and `.zip`, `.lnk`, `.exe`, `.scr`, `.com`.
        
    - For each file created, check subsequent Sysmon Event ID 1 events for executables run from that path.
        
    - For LNKs, inspect the LNK file properties manually (if possible) to read Target field; look for network downloads in PowerShell commands.
        

---

# Removable Media (USB) — detection notes

USB delivery often looks like phishing in the execution trace (user double-clicks via explorer), but there are telltales:

### What to hunt

- **Process creation** (Sysmon Event ID 1) with `Image` path starting with a removable drive letter (e.g., `E:\malware.exe`) or mounted device GUID paths.
    
- File creation events for new files appearing on non-system volumes (E:, F:, or `\\?\Volume{...}`).
    
- Kernel/Setup/System logs contain device arrival messages when a USB is plugged (look for PnP or DeviceInstall events — useful if you forward System logs).
    
- Look for `explorer.exe` opening files from removable letter, followed by suspicious process creation.
    

![[Pasted image 20251111061528.png]]

### Analyst checklist

1. In Sysmon: search `EventID=1` where `Image` startswith `E:\` or any non-C: drive.
    
2. In System log: search for device arrival / USB plug events correlated to time of process creation.
    
3. If found, capture MD5/SHA of binaries for IOC, isolate host, and collect disk image of USB if available.
    

---

# MITRE mapping (compact)

- **T1133** — External Remote Services (RDP/VNC).
    
- **T1190** — Exploit Public-Facing Application (web/MS SQL etc).
    
- **T1566** — Phishing (attachments, links).
    
- **T1091** — Removable Media (USB).
    
- Also watch for post-access techniques (credential dumping, lateral movement, persistence) — correlate with other logs.
    

---

# Concrete SIEM / Query templates (generic)

Use these as starting points; adapt field names to your SIEM.

**Failed RDP brute force (Windows Security)**

```
index=wineventlog_security EventID=4625
| where LogonType in (3,10)
| stats count by Account_Name, Source_Network_Address, bin(_time, 1m)
| where count > 10
```

**Successful RDP login**

```
index=wineventlog_security EventID=4624
| where LogonType==10
| table _time, Account_Name, Source_Network_Address, Logon_ID
```

**Browser → download → execution (Sysmon)**

```
index=sysmon EventID IN (1,11)
| transaction maxspan=5m startswith=(EventID=11 Image="*msedge.exe" OR Image="*chrome.exe")
| search TargetFilename="*\\Downloads\\*"
| table _time, Image, TargetFilename, CommandLine, ParentImage
```

**LNK detection**

```
index=sysmon EventID=11 TargetFilename="*\\Downloads\\*.lnk"
| join type=left EventID=1 [ search index=sysmon EventID=1 ParentImage="*\\explorer.exe" CommandLine="*powershell*"]
```

(Adapt to Splunk/ELK/QRadar query syntax.)

---

# Quick containment & triage checklist (when you detect a confirmed access)

1. **Isolate** infected host from network (but keep forensic logs intact).
    
2. **Capture volatile evidence** (running processes, network connections).
    
3. **Collect EVTX and Sysmon** around compromise window (±2 hours).
    
4. **Dump accounts & Logon IDs** used, map to all related ProcessCreate events.
    
5. **Search for persistence** (scheduled tasks, services, registry run keys).
    
6. **Scan endpoints** for same IOCs (files, source IPs).
    
7. **If ransomware suspected**: preserve VSS snapshots, disconnect backups, notify IR.
    

---

# Quick reference — event IDs & meaning (as used above)

- **Windows Security**
    
    - `4625` — Failed logon (use for brute force detection).
        
    - `4624` — Successful logon (use to identify compromised account & Logon ID).
        
- **Sysmon**
    
    - `1` — Process creation (who launched what, parent/child, command line).
        
    - `11` — File created (downloads, extracted files).
        
    - (Other Sysmon events are useful too — e.g., network connections, image loads — check your Sysmon config.)
        

---

# Things analysts often miss (be smug about catching these)

- **LNK leaves a file creation entry** — many miss this and assume LNKs have no trace. Hunt the download first.
    
- **Logon ID is the glue** — tie `4624` Logon ID to Sysmon process creation to see exact actor activity.
    
- **Double-extension trick** — users see `invoice.pdf` while real file is `invoice.pdf.exe`. Use filetype vs. extension discrepancies in detections.
    
- **Processes launched from non-C:** immediate red flag for USB cases.
    

---

# Short example playbook (RDP case you described)

1. Open `RDP-Security.evtx` → filter `EventID=4625` → identify brute force source IP(s).
    
2. Filter `EventID=4624` for LogonType=10 around the time of highest failed attempts to find successful login and Logon ID.
    
3. Open Sysmon logs → search for processes with that Logon ID → extract binaries executed, network connections, and anything written to disk.
    
4. Hunt for lateral movement and persistence artifacts; contain host and collect artifacts.
    

# Short example playbook (Phishing Case 3)

1. Open `Phishing-Sysmon.evtx`. Search Sysmon `EventID=11` for `Downloads` and for any `.lnk`, `.exe`, `.zip` entries.
    
2. For each created file, follow forward to Sysmon `EventID=1` to see if it was executed.
    
3. Extract PowerShell commandlines if present, base64 decode `-EncodedCommand` payloads, and dump URLs and downloaded filenames.
    
4. Triage: quarantine host, collect the downloaded file for AV/analysis, and hunt other hosts for same download URL / hash.
    

# Short example playbook (USB Case)

1. Open `USB-Sysmon.evtx`. Find `EventID=1` where `Image` begins with `E:\` (or other non-C:).
    
2. Correlate with System log events for device arrival at the same timestamp.
    
3. If malicious file ran from USB, capture the file hash and plug the USB into isolated analysis environment.
    

---

