# VAPT Lab Playbook – Metasploitable 2

A complete Vulnerability Assessment and Penetration Testing (VAPT) project performed against **Metasploitable 2** in a controlled laboratory environment using an industry-standard penetration testing methodology.

> **Educational Purpose Only:** This project was conducted in an isolated lab environment against an intentionally vulnerable machine. Do not use these techniques against systems without explicit authorization.

---

# Objectives

This project demonstrates the complete lifecycle of a penetration test, from information gathering to exploitation and reporting.

The primary objectives include:

- Reconnaissance
- Network Scanning
- Service Enumeration
- Vulnerability Assessment
- Exploitation
- Privilege Escalation
- Documentation & Reporting

---

# Lab Environment

| Component | Details |
|-----------|---------|
| Attacker Machine | Kali Linux |
| Target Machine | Metasploitable 2 |
| Virtualization | Oracle VirtualBox |
| Network | Host-Only Network |
| Target IP | 192.168.56.104 |

---

# Tools Used

### Reconnaissance & Enumeration

- Nmap
- Nmap NSE Scripts
- Netcat
- rpcinfo
- showmount
- smbclient
- enum4linux
- dig
- host
- nslookup
- curl

### Web Testing

- Burp Suite Community Edition
- Gobuster
- FFUF

### Exploitation

- Metasploit Framework
- Searchsploit

### Post Exploitation

- Linux Built-in Commands
- Bash

---

# Methodology

```
Reconnaissance
        │
        ▼
Nmap Scanning
        │
        ▼
Service Enumeration
        │
        ▼
Vulnerability Assessment
        │
        ▼
Exploitation
        │
        ▼
Privilege Escalation
        │
        ▼
Reporting
```

---

# Project Structure

```text
VAPT-Lab-Playbook/
│
├── Reconnaissance/
├── Nmap-Scanning/
├── Service-Enumeration/
├── Exploitation/
├── Privilege-Escalation/
├── Reports/
├── Assets/
└── README.md
```

---

# Services Assessed

| Port | Service | Status |
|------|---------|--------|
| 21 | FTP | ✅ Completed |
| 22 | SSH | ✅ Completed |
| 23 | Telnet | ✅ Completed |
| 25 | SMTP | ✅ Completed |
| 53 | DNS | ✅ Completed |
| 80 | HTTP | ✅ Completed |
| 111 | RPCBind | ✅ Completed |
| 139/445 | Samba | ✅ Completed |
| 2049 | NFS | ✅ Completed |
| 3632 | DistCC | ✅ Exploited |
| 1099 | Java RMI | ✅ Exploited |
| 6667 | UnrealIRCd | ✅ Exploited |

---

# Exploitation Summary

| Vulnerability | Result |
|--------------|--------|
| DistCC Remote Command Execution | ✅ Shell (daemon) |
| Samba Username Map Script (CVE-2007-2447) | ✅ Root Shell |
| UnrealIRCd Backdoor | ✅ Root Shell |
| Java RMI Insecure Configuration | ✅ Root Shell |

---

# Skills Demonstrated

- Information Gathering
- Network Enumeration
- Service Enumeration
- Vulnerability Validation
- Manual Verification
- Remote Code Execution
- Linux Enumeration
- Privilege Escalation
- Documentation
- Technical Reporting

---

# Repository Contents

Each phase contains:

- Detailed Markdown documentation
- Commands executed
- Observations
- Findings
- Exploitation evidence
- Reports
- Screenshots

---

# Disclaimer

This repository is intended solely for educational purposes and cybersecurity training. All activities were performed in a controlled laboratory environment against intentionally vulnerable systems. The techniques demonstrated should only be used on systems for which you have explicit authorization.