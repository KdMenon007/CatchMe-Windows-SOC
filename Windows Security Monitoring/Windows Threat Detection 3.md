
---

## What is C2 (Command & Control)

- **C2 = the channel malware uses to receive attacker commands and send data back.**
    
- Sometimes unnecessary (e.g., live RDP session); most attackers prefer a persistent C2 to keep access after the initial breach.
    

---

## Attacks without C2

- Example: attacker uses RDP interactively and types commands directly.
    
- Downside: access ends when the RDP session closes or credentials change.
    
![[Pasted image 20251111064034.png]]
---

## Simplest C2 patterns

- **Direct**: phishing attachment itself opens an outbound connection to attacker C2.
    
- **Staged**: attachment downloads a second-stage C2 binary, hides it (e.g., `C:\Temp`), and runs it — more resilient if the original file is removed.
    

![[Pasted image 20251111064055.png]]

---

## Why persistence matters

- Short-lived theft (data stealer) may run minutes then exit.
    
- Most attacks need long-term access (days → months) for lateral movement, spying, botnets, or ransomware preparation.
    
![[Pasted image 20251111064104.png]]

---

## Common persistence techniques (quick table)

|Method|Example command / action|When it runs|Useful logs to detect|
|---|--:|---|---|
|Create Windows Service|`sc create "BadService" binpath= "C:\malware.exe" start= auto`|On boot|Sysmon process create (event 1), Security 4697, System 7045|
|Scheduled Task|`schtasks /create /tn "BadTask" /tr "C:\malware.exe" /sc onstart /ru System`|On boot / schedule|Sysmon event 1 (schtasks launch), Security 4698|
|Startup folder|Copy to `%AppData%\...\Startup\malware.exe`|On user login|Sysmon file create (event 11); process parent often `explorer.exe`|
|Run registry keys|`reg add "HKCU\...\Run" /v BadKey /d "C:\malware.exe"`|On user login|Sysmon registry change (event 13) affecting Run keys|
|Backdoored user|`net user "bad" "pass" /add` + add to Admins|Any time (enables RDP/login)|Security event 4720 (create), 4732 (group add), 4724 (password reset)|

![[Pasted image 20251111064129.png]]

![[Pasted image 20251111064140.png]]

---

## Persistence via RDP / user management

- Attackers may create new users or reset old users’ passwords to regain RDP access.
    
- **Detect:** Security events 4720 (user created), 4732 (added to privileged group), 4724 (password reset).
    
- Don’t hunt for suspicious names only — investigate _who_ created the account, source IP, and session context.
    

---

## How to detect malicious services/tasks/startup

- Watch for **creation actions** (sc.exe, schtasks.exe, file writes to Startup, registry Run changes).
    
- Use Sysmon for process/file/registry visibility and Windows Security logs for service/task creation events (4697, 4698).
    
- Look at process parent chains: suspicious services/tasks often have odd parents (e.g., `svchost` spawning unexpected child).

![[Pasted image 20251111064220.png]]

![[Pasted image 20251111064239.png]]

---

## Practical detection tips (playbook-style)

1. Alert on `sc create` and `schtasks /create` process launches (Sysmon EID 1).
    
2. Alert on Security events: **4697**, **4698**, **4720**, **4732**, **4724**.
    
3. Monitor file creation in Startup folders (Sysmon EID 11) and registry Run key writes (Sysmon EID 13).
    
4. For new/changed accounts: capture creator user, creator source IP, and correlated suspicious events in same session.
    
5. Investigate processes with unusual persistence parentage (e.g., `services.exe`/`svchost` → unknown exe).
    


---

# **Why Persistence Matters (Key Points)**

Attackers _don’t_ always want a quick “in-and-out” data grab.  
Often, they need **long-term**, **quiet**, **reliable** access. Why?

|Purpose|Explanation|Real-World Example|
|---|---|---|
|**Botnets / Further Attacks**|Compromised machines are used as _weapons_ — DDoS, crypto mining, spreading malware, etc.|**Kraken Botnet** ran miners + data-stealer + C2 on victims forever.|
|**Espionage / Intelligence Gathering**|Quiet surveillance, data collection, network monitoring.|**Volt Typhoon** hid in U.S. power grid for almost **a year**.|
|**Staging for Bigger Breach / Ransomware**|A single foothold → privilege escalation → lateral movement → domain compromise → mass encryption.|Typical enterprise ransomware breaches ~**20–60 days** before impact.|

**Bottom line:**  
Persistence lets attackers _take their time_ to escalate from _one machine → whole network._

---

# **Active Directory + Ransomware = The Endgame**

When a network uses **Active Directory**, everything is centralized:

- Accounts
    
- Permissions
    
- Computers
    
- File Shares
    

So if an attacker gains **Domain Admin**, the game is over:

- They can **deploy ransomware to every machine at once.**
    
- They can **exfiltrate data silently.**
    
- They can **disable security tools.**
    

This is why ransomware **paralyzes entire organizations**, like:

- **McLaren Hospitals** → 743,000 patients impacted
    
- Ransom notes printed across every office printer like confetti from hell
    

---

# **Threat Detection Recap (Your Core Skillset Now)**

You learned how to catch attacks in the **early stages**, _before_ ransomware detonates.

|MITRE Stage|Example Actions|What You Learned to Detect|
|---|---|---|
|**Initial Access**|Phishing, RDP brute-force|Event logs, source IP anomalies|
|**Execution**|Macro, PowerShell, DLL load|Sysmon Event ID 1, AMSI logs|
|**Privilege Escalation**|Token theft, UAC bypass|Process anomalies, access logs|
|**Credential Access**|LSASS dump, Mimikatz|Sysmon event chains, memory access patterns|
|**Lateral Movement**|SMB, WMI, RDP|Network logs + session correlation|
|**Persistence**|Services, scheduled tasks, registry Run keys|4697, 4698, 11, 13, parent-child process tracing|
|**Command & Control**|Malware beaconing back to C2|DNS anomalies, beacon timing patterns|

**Your job:**  
Catch attackers **before** the “Impact” stage — where ransomware detonates.

---

# **If you Remember _Only One Line_:**

> **Detect persistence early. If persistence survives, ransomware follows.**

---

