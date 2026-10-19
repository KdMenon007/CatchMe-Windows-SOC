
---

## 1. Application-Level Gateway (ALG)

### Overview

- Also known as a **proxy firewall**
    
- Operates at the **Application Layer (Layer 7)**
    
- Acts as an **intermediary between client and server**
    

### Working Principle

- Terminates the incoming client connection
    
- Creates a **new connection** to the destination on behalf of the client
    
- Fully inspects application-layer communication
    

### Capabilities

- Deep understanding of **protocol semantics** (e.g., HTTP, FTP, SMTP)
    
- Can:
    
    - Validate **command syntax**
        
    - Enforce **protocol compliance**
        
    - **Sanitise payloads**
        
    - Strip malicious content
        
- Prevents:
    
    - Injection attacks (basic level)
        
    - Protocol misuse
        

### Advantages

- High visibility into application traffic
    
- Strong control over allowed operations
    
- Effective against malformed or non-compliant protocol usage
    

### Limitations

- **Performance overhead** (due to full proxying)
    
- **Limited scalability**
    
- Requires protocol-specific implementations
    

### Significance

- Early demonstration that:
    
    > “Effective application security requires understanding application logic.”
    

---

## 2. Deep Packet Inspection (DPI)

### Overview

- A **capability**, not a standalone firewall type
    
- Works across **Layer 3 to Layer 7**
    
- Inspects both:
    
    - Packet headers
        
    - Packet payload (actual data)
        

### Working Principle

- Analyzes packet contents for:
    
    - Known signatures
        
    - Byte patterns
        
    - Traffic anomalies
        

### Detection Capabilities

- Malware signatures
    
- Peer-to-peer (P2P) traffic
    
- Command-and-control (C2) communications
    
- Encrypted tunnels (e.g., Tor over HTTPS)
    

### Use Cases

- Intrusion detection/prevention systems
    
- Content filtering
    
- Network monitoring
    

### Strengths

- Can identify **hidden or disguised threats**
    
- Detects suspicious patterns inside “legitimate-looking” traffic
    

### Limitations

- **Lacks contextual awareness**
    
    - Cannot differentiate:
        
        - Legitimate vs malicious intent
            
- Example:
    
    - Detects `UNION SELECT`
        
    - But cannot determine if it’s:
        
        - A DBA query ✔
            
        - SQL injection attack ❌ (without context)
            

### Key Insight

- DPI = **pattern recognition without understanding intent**
    

---

## 3. Next-Generation Firewall (NGFW)

### Overview

- Advanced evolution of traditional stateful firewalls
    
- Operates across **Layer 3–7 + Identity Layer**
    

### Core Philosophy

- Moves from:
    
    - “Where is traffic going?”
        
- To:
    
    - “Who is sending it, what are they doing, and should they be allowed?”
        

### Core Features

#### 1. Stateful Inspection

- Tracks active sessions
    
- Validates packets against expected connection states
    

#### 2. Deep Packet Inspection (DPI)

- Inspects payload for malicious patterns
    

#### 3. Intrusion Prevention System (IPS)

- Detects and blocks:
    
    - Exploits
        
    - Vulnerabilities
        
    - Known attack signatures
        

#### 4. SSL/TLS Decryption

- Decrypts encrypted traffic for inspection
    
- Re-encrypts before forwarding
    

#### 5. Identity Awareness

- Integrates with:
    
    - Active Directory
        
    - Certificates
        
    - SAML
        
- Enables **user-based policies**
    

#### 6. Application Awareness

- Identifies applications regardless of:
    
    - Port
        
    - Protocol
        
- Example:
    
    - Detects WhatsApp traffic even on non-standard ports
        

---

### Advantages

- Granular access control
    
- Context-aware decision making
    
- Unified threat management
    

### Limitations

- Still treats HTTP/HTTPS as **generic protocols**
    
- Lacks deep understanding of:
    
    - Application logic
        
    - Business rules
        

### Critical Gap

- Weak against:
    
    - SQL Injection
        
    - Parameter tampering
        
    - Logic-based attacks
        

➡️ This gap leads to the need for **Web Application Firewalls (WAFs)**

---

# 🔥 Firewall Types — Comprehensive Comparison

---

## 1. Stateless Packet Filter

### OSI Layer

- Layer 3–4
    

### Characteristics

- Filters packets based on:
    
    - Source IP
        
    - Destination IP
        
    - Port
        
    - Protocol
        

### Behavior

- Each packet is treated **independently**
    
- No memory of previous packets
    

### Pros

- Fast
    
- Low resource usage
    

### Cons

- No session awareness
    
- Vulnerable to spoofing and fragmented attacks
    

---

## 2. Stateful Inspection Firewall

### OSI Layer

- Layer 3–4
    

### Characteristics

- Maintains a **state table** of active connections
    

### Tracks:

- TCP flags
    
- Sequence numbers
    
- Session states (ESTABLISHED, SYN, etc.)
    

### Pros

- More secure than stateless
    
- Prevents invalid or unexpected packets
    

### Cons

- Limited to network/transport layers
    
- No payload inspection
    

---

## 3. Application-Level Gateway (ALG)

### OSI Layer

- Layer 7
    

### Characteristics

- Full proxy behavior
    
- Protocol-aware inspection
    

### Example Protocols

- HTTP
    
- FTP
    
- SMTP
    

### Pros

- Deep inspection
    
- Strong application control
    

### Cons

- Performance overhead
    
- Protocol-specific limitations
    

---

## 4. Deep Packet Inspection (DPI)

### OSI Layer

- Layer 3–7
    

### Characteristics

- Inspects full packet content
    

### Pros

- Detects hidden threats
    
- Useful for advanced threat detection
    

### Cons

- No contextual understanding
    
- Signature-dependent
    

---

## 5. Next-Generation Firewall (NGFW)

### OSI Layer

- Layer 3–7 + Identity
    

### Characteristics

- Combines:
    
    - Stateful inspection
        
    - DPI
        
    - IPS
        
    - SSL/TLS inspection
        
    - Identity awareness
        

### Pros

- Context-aware security
    
- Fine-grained policies
    

### Cons

- Not specialized for web app logic attacks
    
- Complex configuration
    

---

# 🧠 Conceptual Evolution (Very Important)

Think of firewall evolution like intelligence levels:

1. **Stateless** → Sees packets
    
2. **Stateful** → Understands sessions
    
3. **ALG** → Understands protocols
    
4. **DPI** → Reads data inside packets
    
5. **NGFW** → Understands users + apps + behavior
    

---

## 1. What is a WAF?

### Definition

- A **Web Application Firewall (WAF)** is a security control that:
    
    - Inspects **HTTP/HTTPS traffic**
        
    - Monitors both:
        
        - **Inbound (client → server)**
            
        - **Outbound (server → client)** traffic
            

### Core Function

- Detects and blocks **malicious application-layer activity**
    

### Key Difference from Other Firewalls

- Traditional firewalls → focus on **ports & IPs**
    
- NGFW → focuses on **applications & users**
    
- WAF → understands **web application logic & structure**
    

---

## 2. What Does a WAF Inspect?

A WAF deeply parses HTTP requests, including:

- HTTP methods (GET, POST, PUT, etc.)
    
- Headers
    
- Cookies
    
- Query strings
    
- POST body data
    

👉 Basically, it reads web traffic like a backend server would.

---

## 3. Attacks a WAF Protects Against

WAFs are designed around **web-specific attack patterns**, especially:

- **SQL Injection (SQLi)**
    
- **Cross-Site Scripting (XSS)**
    
- **File Inclusion (LFI/RFI)**
    
- **Command Injection**
    
- **Parameter Tampering**
    

### OWASP Coverage

- Aims to protect against:  
    👉 **OWASP Top 10 vulnerabilities**
    

---

## 4. Virtual Patching

### Concept

- WAFs do NOT fix code vulnerabilities
    
- Instead, they apply **runtime protection**
    

### Definition

- **Virtual patching** = blocking exploit attempts **without modifying source code**
    

### Important Limitation

- If the WAF rule:
    
    - ❌ Does not recognize a payload → attack passes through
        

👉 Translation:

> WAF ≠ cure, it’s a **band-aid with intelligence**

---

## 5. Why WAF is Powerful

### Unique Capability

- Deep understanding of **web attack patterns**
    

### Examples

- Detects SQLi:
    
    ```
    ?id=1' OR '1'='1
    ```
    
- Detects XSS:
    
    ```
    <img src=x onerror=alert(1)>
    ```
    

### Insight

- WAF rules are:
    
    - Either **signature-based**
        
    - Or **trained to recognize attack behavior**
        

---

# 🏗️ WAF Deployment Models

---

## 1. Cloud-Based (Reverse Proxy)

![Image](https://developers.cloudflare.com/_astro/security-ref-arch-6.DGieuMIT_Z7OIzr.svg)

![Image](https://www.researchgate.net/publication/330055462/figure/fig3/AS%3A731192269746178%401551341230183/Architecture-of-regular-WAFs-running-in-reverse-proxy-mode-or-layer-two-bridge-mode-see.ppm)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/04/02/ARCHBLOG-1141-arch-diagr-1024x669.png)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/03/05/OLX-Edge-Security-Architecture-1260x618.png)

### Description

- Traffic routed through **vendor infrastructure**
    
- Acts as a **reverse proxy**
    
- Terminates TLS (HTTPS)
    

### Examples

- Cloudflare
    
- AWS WAF
    
- Akamai
    
- Imperva Cloud
    

### Pros

- Easy deployment
    
- Scalable
    
- DDoS protection included
    

### Cons

- Dependency on third-party provider
    
- Latency concerns
    

---

## 2. Network Appliance

![Image](https://cdn.studio.f5.com/images/k6fem79d/production/52a3999fe4a134e3e86a795ac627401133863375-766x275.gif)

![Image](https://community.f5.com/t5/s/zihoc95639/images/bS0zMDczNDktRkQ5RERL?image-dimensions=600x381&revision=87)

![Image](https://marvel-b1-cdn.bc0a.com/f00000000310757/www.fortinet.com/content/dam/fortinet/images/cyberglossary/waf-vs-network-firewall.png)

![Image](https://fortinetweb.s3.amazonaws.com/docs.fortinet.com/v2/resources/0c84fec2-3030-11ef-bfe5-fa163e15d75b/images/4fb5ebb4f21b2a72ef6c86ec98efcddc_network_topology_inline_1arm.png)

### Description

- Physical or virtual device
    
- Deployed **inline with network traffic**
    
- Often paired with load balancers
    

### Examples

- F5 Advanced WAF
    
- Fortinet WAF
    
- Barracuda WAF
    

### Pros

- Full control
    
- High performance
    

### Cons

- Expensive
    
- Complex setup
    

---

## 3. Host-Based (Embedded)

![Image](https://static.wixstatic.com/media/bf4d1b_ff0ac8f2798e4887a65e3b67f24c6daa~mv2.png/v1/fill/w_975%2Ch_645%2Cal_c%2Cq_90%2Cenc_avif%2Cquality_auto/bf4d1b_ff0ac8f2798e4887a65e3b67f24c6daa~mv2.png)

![Image](https://www.zenarmor.com/docs/what-is-waf.png)

![Image](https://media.licdn.com/dms/image/v2/D4D12AQGJTxOOYf4kiA/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1684332997565?e=2147483647&t=j6k9GmcpPNjlug20n_plCQqBpogtCAdq_bba57Q9vlU&v=beta)

![Image](https://coreruleset.org/docs/2-how-crs-works/2-1-anomaly_scoring/as_inbound_no_fonts.svg?height=36em)

### Description

- Runs inside the web server
    
- Implemented as a module
    

### Examples

- ModSecurity
    
- OWASP Core Rule Set (CRS)
    

### Pros

- Fine-grained control
    
- No external dependency
    

### Cons

- Consumes server resources
    
- Harder to scale
    

---

# 🕵️‍♂️ WAF Fingerprinting (Recon Phase)

---

## Why It Matters

> “You don’t always need to break the wall — sometimes just knowing it exists changes the game.”

### Benefits

- Avoid wasting payloads
    
- Tailor bypass techniques
    
- Prevent IP bans / rate limits
    

---

# 🔍 1. Passive Header Inspection

### Method

- Use:
    
    ```bash
    curl -I target.com
    ```
    

### Look for WAF Signatures

|Header|WAF|
|---|---|
|`server: cloudflare`|Cloudflare|
|`X-Sucuri-ID`|Sucuri|
|`X-CDN: Imperva`|Imperva|
|`Akamai-Origin-Hop`|Akamai|
|`X-F5-Application: ASM`|F5 WAF|

### Insight

- Fast, non-intrusive
    
- But:
    
    - Can be hidden/spoofed
        

---

# ⚡ 2. Behavioural Analysis

![Image](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2021/01/20/Picture1-5-e1611172377720.png)

![Image](https://www.appknox.com/hs-fs/hubfs/What%20is%20an%20SQL%20injection_.png?height=1928&name=What+is+an+SQL+injection_.png&width=2324)

![Image](https://www.yeswehack.com/_next/image?q=85&url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2Fd51e1jt0%2Fproduction%2F3bd27a87d85e788d49c0c6dff2a68715803d92d9-1511x1145.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840)

![Image](https://www.paloaltonetworks.com/content/dam/pan/en_US/images/cyberpedia/xss-cross-site-scripting/cross-site-scripting-xss.jpg?imwidth=480)

### Concept

- Send **attack-like payloads**
    
- Observe response changes
    

### Test Payloads

- SQLi:
    
    ```
    ' OR 1=1--
    ```
    
- XSS:
    
    ```
    <script>alert(1)</script>
    ```
    
- LFI:
    
    ```
    ../../../../etc/passwd
    ```
    

---

### Response Indicators

|Response|Meaning|
|---|---|
|403 Forbidden|WAF blocking|
|406 Not Acceptable|ModSecurity|
|Custom "Access Denied"|Vendor-specific|
|Delay (>500ms)|Inspection overhead|
|No change|Possibly no WAF|

---

# 🤖 3. Automated Fingerprinting — WAFW00F

### Tool Overview

- Detects **500+ WAFs**
    

### Workflow

1. Sends normal request
    
2. Sends malicious payloads
    
3. Compares:
    
    - Status codes
        
    - Headers
        
    - Response patterns
        

### Key Insight

- Detects:
    
    - Presence of WAF ✔
        
    - Even if vendor unknown ✔
        

---

# 🌐 4. Subnet-Level Detection — Nmap Script

### Script

```bash
http-waf-fingerprint
```

### Usage

```bash
nmap -p 80,443 --script http-waf-fingerprint TARGET_IP
```

### Features

- Detects:
    
    - WAF presence
        
    - Type/version (sometimes)
        

### Limitation

- Silent if:
    
    - WAF not clearly identifiable
        

---

# ⚔️ Final Security Insight (This is Gold)

- WAF = **last line of defense for web apps**
    
- But:
    
    - It is **rule-based**
        
    - Attackers exploit:
        
        - Encoding tricks
            
        - Obfuscation
            
        - Logic flaws
            

👉 Real-world truth:

> If your app is vulnerable, a WAF just buys you time — it doesn’t save you forever.

---

# 🧠 Mental Model (Lock This In)

- **NGFW** → understands network + user
    
- **WAF** → understands web attacks
    
- **App Code** → actual security boundary
    
---
# 🧠 WAF Detection Mechanisms
---

## 🔪 Core Analogy (Lock this in)

- **Signature-based** → “I know what a knife looks like”
    
- **Behavioural-based** → “I know how a normal person uses a knife”
    

👉 Reality check:

- Signature can be fooled by **disguise**
    
- Behavioural can be fooled by **mimicry**
    

---

# 1️⃣ Signature-Based Detection

---

## 📌 Overview

- Detects attacks using **predefined patterns (signatures)**
    
- Compares incoming traffic against a **rule database**
    

### Working Principle

- If request matches a known malicious pattern → **block**
    
- Otherwise → **allow**
    

---

## 🔍 Example

### Malicious Request

```
GET /search?q=' OR 1=1-- HTTP/1.1
```

### WAF Action

- Matches SQL injection signature
    
- Responds with:
    

```
403 Forbidden
```

---

## ⚔️ Evasion Techniques (Attacker Perspective)

This is where things get fun (and dangerous).

### 1. Encoding

- Transform payload to avoid pattern match:
    

```
' OR 1=1-- 
→ %27%20OR%201%3D1--
```

---

### 2. Case Variation

```
UNION SELECT
→ unIOn sElEcT
```

---

### 3. Comment Insertion

```
UNION/**/SELECT
```

---

### 4. Alternative Syntax

```
1' AND SLEEP(10)-- 
→ 1' AND IF(1=1, SLEEP(10), 1) --
```

---

## ✅ Advantages

- Low **false positives** (when tuned well)
    
- Easy to:
    
    - Write
        
    - Understand
        
- High performance:
    
    - Fast regex matching
        

---

## ❌ Disadvantages

- Blind to:
    
    - Encoding
        
    - Obfuscation
        
- Cannot detect:
    
    - **Zero-day attacks**
        
- Requires:
    
    - Constant rule updates
        

---

## 🧠 Key Insight

> Signature-based systems only know what they’ve seen before.

---

# 2️⃣ Behavioural Detection (Anomaly-Based)

---

## 📌 Overview

- Learns what **normal traffic looks like**
    
- Flags **deviations from baseline**
    

---

## ⚙️ How It Works

### Step 1: Training Phase

- Observe normal traffic patterns:
    
    - Input length
        
    - Parameter count
        
    - Data types
        
    - Request frequency
        
    - User behavior
        

---

### Step 2: Detection Phase

- Compare incoming request against baseline
    

---

## 🔍 Example

### Normal Request

```
username=alice&password=pass1234
```

### Malicious Request

```
username=' OR '1'='1&password=somepass
```

### Why It Gets Flagged

- Unexpected:
    
    - Special characters
        
    - Whitespace
        
    - Pattern deviation
        

---

## ✅ Advantages

- Detects:
    
    - **Zero-day attacks**
        
- Harder to bypass with:
    
    - Encoding
        
    - Obfuscation
        
- Adapts over time:
    
    - Learns application behavior
        

---

## ❌ Disadvantages

- High **false positives**
    
    - Example:
        
        - Legit long input → blocked
            
- Requires:
    
    - Training period (learning mode)
        
- Computationally expensive
    

---

## 🧠 Key Insight

> Behavioural detection doesn’t care what the attack looks like — only that it behaves weirdly.

---

# 3️⃣ Hybrid Detection (Modern WAF Approach)

---

## 📌 Overview

- Combines:
    
    - Signature-based detection
        
    - Behavioural/anomaly detection
        

---

## ⚙️ Detection Pipeline

### Step 1: Signature Check (Fast)

- Quick filtering using known patterns
    

---

### Step 2: Behavioural Analysis (Deep)

- Evaluate anomaly score
    

---

### Step 3: Risk Scoring

- Assign weighted score based on:
    
    - Signature match
        
    - Behaviour deviation
        

---

### Step 4: Decision

- Allow / Block / Challenge (CAPTCHA, etc.)
    

---

## 🧠 Why Hybrid Wins

- Signature → catches **known attacks fast**
    
- Behavioural → catches **unknown attacks**
    
- Combined → reduces weaknesses of both
    

---

# ⚔️ Real-World Security Insight

Let’s be blunt:

- Signature-based WAF → script kiddies killer 🪦
    
- Behavioural WAF → catches sloppy attackers
    
- Hybrid WAF → slows down skilled attackers
    

👉 But:

> A determined attacker with time + creativity can still bypass all three.

---

# 🧬 Mental Model (This is gold)

- Signature = **memory**
    
- Behavioural = **intuition**
    
- Hybrid = **judgment**
    

---

# 🚨 Final Take (Practical Truth)

- WAF is:
    
    - A **filter**, not a guarantee
        
- Security depends on:
    
    - Secure coding
        
    - Input validation
        
    - Proper architecture
        

👉 Because:

> If your app trusts user input blindly, no WAF can save you forever.

---
# 🧩 ModSecurity Rule 

---

## 🔹 Full Context

- Rule belongs to:
    
    - **ModSecurity WAF**
        
    - **OWASP Core Rule Set (CRS) v3.3.7**
        
- Purpose:
    
    - Detect **SQL Injection (SQLi)** using **libinjection**
        

---

# 1️⃣ Rule Trigger (Detection Logic)

```bash
SecRule REQUEST_BASENAME "@detectSQLi"
```

### Components

### 🔸 `SecRule`

- Standard directive to define a rule in ModSecurity
    

---

### 🔸 `REQUEST_BASENAME`

- Variable representing:
    
    - The **last part of the URL path**
        

#### Example:

```
/api/user/123  → basename = 123
```

👉 This rule inspects **endpoint values**, not headers or body here

---

### 🔸 `"@detectSQLi"`

- Calls **libinjection engine**
    

### ⚙️ Important Insight

- Not simple regex matching ❌
    
- Uses **tokenization like a SQL parser** ✔
    

👉 This makes it smarter than basic signature rules

---

# 2️⃣ Rule Metadata (Identity & Execution Context)

```bash
id:942101,
phase:2,
block,
capture,
```

---

### 🔸 `id:942101`

- Unique rule identifier
    
- Part of CRS numbering scheme
    

---

### 🔸 `phase:2`

- Executes during:
    
    - **Request body processing phase**
        

### ModSecurity Phases (quick context):

- Phase 1 → headers
    
- Phase 2 → body (this rule runs here)
    
- Phase 3–5 → response processing
    

---

### 🔸 `block`

- Action:
    
    - Immediately **deny request**
        

---

### 🔸 `capture`

- Stores matched payload into:
    
    - `%{TX.0}`
        

👉 Used later for logging & analysis

---

# 3️⃣ Input Normalisation (Critical Layer)

```bash
t:none,t:utf8toUnicode,t:urlDecodeUni,t:removeNulls
```

This is where WAFs get serious — **defeating obfuscation before detection**.

---

### 🔸 `t:none`

- Start with raw input
    

---

### 🔸 `t:utf8toUnicode`

- Converts encoded UTF-8 → Unicode
    

---

### 🔸 `t:urlDecodeUni`

- Recursive decoding
    

#### Example:

```
%2527 → %27 → '
```

---

### 🔸 `t:removeNulls`

- Removes:
    
    - `%00` (null bytes)
        

---

## 🧠 Why This Matters

> If you don’t normalize input, attackers win with encoding tricks.

---

# 4️⃣ Logging & Forensics

```bash
msg:'SQL Injection Attack Detected via libinjection',
logdata:'Matched Data: %{TX.0} found within %{MATCHED_VAR_NAME}: %{MATCHED_VAR}',
```

---

### 🔸 `msg`

- Human-readable alert
    
- Appears in logs / SIEM
    

---

### 🔸 `logdata`

- Detailed forensic info:
    

|Variable|Meaning|
|---|---|
|`%{TX.0}`|Exact matched payload|
|`%{MATCHED_VAR_NAME}`|Where it was found|
|`%{MATCHED_VAR}`|Full value|

---

## 🔍 Example Output

```
Matched Data: ' OR '1'='1 found within REQUEST_BASENAME: 123' OR '1'='1
```

---

# 5️⃣ Tags (Threat Intelligence & Compliance Mapping)

```bash
tag:'attack-sqli'
tag:'OWASP_CRS'
tag:'capec/1000/152/248/66'
tag:'PCI/6.5.2'
tag:'paranoia-level/3'
```

---

## 🔸 Purpose of Tags

- Not just labels — they enable:
    
    - Threat classification
        
    - Compliance mapping
        
    - Rule grouping
        

---

### 🔸 Key Tags Explained

#### ✅ `attack-sqli`

- Identifies attack type
    

---

#### ✅ `OWASP_CRS`

- Belongs to OWASP rule set
    

---

#### ✅ `capec/...`

- Maps to:
    
    - **CAPEC attack patterns**
        
- Used in:
    
    - Threat modeling
        

---

#### ✅ `PCI/6.5.2`

- Compliance reference:
    
    - PCI DSS requirement for injection protection
        

---

#### ✅ `paranoia-level/3`

### CRS Paranoia Levels:

|Level|Behavior|
|---|---|
|PL1|Basic protection|
|PL2|Moderate|
|PL3|Strict (this rule)|
|PL4|Very aggressive|

👉 Higher level:

- ✔ More detection
    
- ❌ More false positives
    

---

# 6️⃣ Scoring System (Anomaly Engine Integration)

```bash
setvar:'tx.sql_injection_score=+%{tx.critical_anomaly_score}'
setvar:'tx.anomaly_score_pl3=+%{tx.critical_anomaly_score}'
```

---

## 🔸 What’s Happening Here?

### 1. `tx.sql_injection_score`

- Tracks cumulative SQLi risk
    

---

### 2. `tx.anomaly_score_pl3`

- Adds to total anomaly score (PL3)
    

---

## 🧠 Key Concept: Scoring-Based WAF

Instead of:

- Block immediately ❌
    

Modern WAF:

- Assigns **risk score**
    
- Blocks when threshold exceeded ✔
    

---

## Example Flow

|Rule Trigger|Score|
|---|---|
|SQLi detected|+5|
|XSS detected|+5|
|Threshold = 10|BLOCK|

---

# 🔥 Big Picture Insight

This rule is NOT just:

> “If SQLi → block”

It is:

- ✔ Normalizing input
    
- ✔ Using intelligent detection (libinjection)
    
- ✔ Logging context-rich data
    
- ✔ Mapping to standards
    
- ✔ Feeding a scoring engine
    

---

# 🧠 Final Mental Model

This rule acts as:

- 🔍 Detector → finds SQLi
    
- 🧹 Cleaner → normalizes input
    
- 🧾 Logger → records evidence
    
- 🏷️ Classifier → tags attack
    
- 📊 Scorer → feeds risk system
    

---

# ⚔️ Real-World Take (No sugarcoating)

- This is **high-quality defense engineering**
    
- But:
    
    - Still bypassable with:
        
        - Advanced obfuscation
            
        - Logic flaws
            
        - App-specific tricks
            

👉 Because:

> WAF protects patterns — not broken business logic.

---
# ⚠️ Web Application Firewall (WAF) — Limitations 
---

# 1️⃣ WAFs Are Only as Good as Their Rules

### 📌 Core Idea

- A WAF is **rule-driven**
    
- If rules are weak → protection is weak
    

---

## 🔍 Problems

### ❌ Signature Dependence

- Misses:
    
    - New payloads
        
    - Zero-day attacks
        
- Easily bypassed via:
    
    - Encoding
        
    - Comments
        
    - Whitespace tricks
        

---

### ❌ Log-Only Mode (Big Real-World Issue)

- Many orgs run WAF in:
    
    - **Detection mode only (no blocking)**
        

👉 Why?

- High **false positives**
    

👉 Result:

> WAF becomes a _glorified logging system_, not protection

---

## 🧠 Insight

> A WAF without good rules is like a guard who only recognizes last week’s criminals.

---

# 2️⃣ WAFs Don’t Understand Application Logic

---

## 📌 Core Limitation

- WAF understands:
    
    - Syntax ✔
        
- WAF does NOT understand:
    
    - **Business logic ❌**
        

---

## 🔥 Real Attack Examples

### 🔸 IDOR (Insecure Direct Object Reference)

```http
GET /api/user/123
→ attacker changes → /api/user/124
```

- No payload
    
- No special characters
    
- WAF sees:
    
    - “Looks normal to me 🤷”
        

---

### 🔸 Authorization Bypass

```http
GET /admin
```

- If access control is broken:
    
    - WAF cannot detect it
        

---

### 🔸 Parameter Tampering

```http
?price=1.99 → ?price=0.01
```

- Business logic issue
    
- Not a syntax attack
    

---

### 🔸 Race Conditions / Replay Attacks

- No malicious patterns
    
- Pure logic abuse
    

---

## 🧠 Golden Line

> WAF filters **syntax**, not **intent**.

---

# 3️⃣ Encrypted Traffic = Blind Spot

---

## 📌 Problem

- HTTPS traffic is **encrypted**
    

👉 Without decryption:

- WAF sees:
    
    - ❌ Encrypted blobs
        
    - Not actual HTTP requests
        

---

## 🔐 Solution

- WAF sits at **TLS termination point**
    
- Decrypt → inspect → re-encrypt
    

---

## ⚠️ Trade-offs

### ❌ Complexity

- Certificate management
    
- Key handling risks
    

---

### ❌ Privacy / Compliance Issues

- Some orgs avoid decryption due to:
    
    - Legal concerns
        
    - Data sensitivity
        

---

## 🧠 Insight

> If you can’t see the traffic, you can’t secure it.

---

# 4️⃣ Client-Side Attacks Bypass WAF

---

## 📌 Key Idea

- WAF only inspects:
    
    - **Server-bound traffic**
        

---

## 🔥 Examples

### 🔸 DOM-Based XSS

- Payload executes:
    
    - Entirely in browser
        
- Never hits server → WAF sees nothing
    

---

### 🔸 Malicious JavaScript (Supply Chain)

- Compromised scripts:
    
    - Analytics
        
    - CDN libraries
        

👉 No suspicious request → no detection

---

## 🧠 Insight

> If the attack lives in the browser, the WAF is blind.

---

# 5️⃣ WAFs Introduce New Attack Surfaces

---

## 📌 Irony

- Adding security can:
    
    - Create new vulnerabilities
        

---

## 🔥 Risks

### 🔸 Rule Injection

- Poorly written rules → exploitable
    

---

### 🔸 Denial of Service (DoS)

- Complex regex or large payloads:
    
    - Exhaust CPU/memory
        

---

### 🔸 Exposed Admin Interface

- If public:
    
    - Brute-force login
        
    - Credential attacks
        

---

### 🔸 Known Vulnerabilities

#### Example:

- **CVE-2020-5902**
    
    - A critical vulnerability in F5 BIG-IP WAF
        
    - Allowed:
        
        - Remote code execution
            

---

## 🧠 Insight

> A badly configured WAF can become the weakest link.

---

# 6️⃣ Performance vs Security Trade-Off

---

## 📌 Reality

- Deep inspection ≠ free
    

---

## ⚙️ Performance Costs

### 🔸 Signature-Based

- Regex on large payloads → CPU heavy
    

---

### 🔸 Behavioural Detection

- ML / anomaly engines:
    
    - Resource intensive
        

---

### 🔸 Encoding Handling

- Multi-layer decoding adds latency
    

---

## ⚠️ Impact

- Increased:
    
    - Response time
        
    - Latency
        
- Risk of:
    
    - Legit requests timing out
        

---

## 🧠 Insight

> The more you inspect, the slower things get. No shortcuts.

---

# 7️⃣ False Sense of Security (Most Dangerous One)

---

## 📌 Problem

- Organizations think:
    
    > “We have a WAF, we’re secure.”
    

---

## ❌ Reality

- WAF is:
    
    - Just **one layer**
        

---

## ✅ Real Security Requires

- Secure coding practices
    
- Input validation
    
- Proper authentication & authorization
    
- Logging & monitoring
    
- Multiple layers:
    
    - WAF + NGFW + EDR + SIEM
        

---

## 🧠 Final Truth

> A WAF buys you time — it doesn’t buy you invincibility.

---

# 🧬 Final Mental Model (Lock this in)

|What WAF Does|What It Cannot Do|
|---|---|
|Filters malicious patterns|Understand business logic|
|Blocks known attacks|Stop zero-days reliably|
|Inspects HTTP traffic|See encrypted traffic (without TLS termination)|
|Protects server-side|Stop client-side attacks|

---

# ⚔️ Brutally Honest Take

- WAF stops:
    
    - Noisy attackers
        
    - Script kiddies
        
- Slows down:
    
    - Intermediate attackers
        
- Does NOT stop:
    
    - Skilled, patient attackers
        

---
