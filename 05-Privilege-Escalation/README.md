# Privilege Escalation

## Overview

This section documents the privilege escalation phase performed after obtaining an initial shell on the target machine.

The objective was to identify local privilege escalation vectors, misconfigurations, weak permissions, and vulnerable binaries that could lead to elevated privileges.

## Objectives

- Identify the current user
- Enumerate system information
- Check sudo permissions
- Search for SUID/SGID binaries
- Enumerate writable directories
- Review scheduled cron jobs
- Inspect running processes
- Search for credentials
- Analyze services running as root
- Document privilege escalation opportunities

## Enumeration Checklist

- User Information
- Operating System
- Kernel Version
- Environment Variables
- Groups & Permissions
- Sudo Privileges
- SUID Binaries
- SGID Binaries
- Writable Files & Directories
- Cron Jobs
- Running Services
- Network Configuration
- Installed Software
- Password Files
- SSH Keys
- Mounted Filesystems

## Tools Used

- Bash
- find
- sudo
- ps
- id
- whoami
- uname
- groups
- env
- cat
- ls
- grep

## Directory Structure

```
Privilege-Escalation/
├── README.md
├── distcc.md
├── samba.md
├── unrealircd.md
├── java-rmi.md
├── screenshots/
└── reports/
```

## Status

| Exploit | Initial User | Root Access |
|---------|--------------|-------------|
| DistCC | daemon | ❌ (Enumeration Performed) |
| UnrealIRCd | root | ✅ |
| Samba | root | ✅ |
| Java RMI | root | ✅ |

## Summary

Privilege escalation was necessary only for exploits that resulted in a low-privileged shell (e.g., DistCC). Exploits such as UnrealIRCd, Samba, and Java RMI directly provided root access, eliminating the need for additional privilege escalation.