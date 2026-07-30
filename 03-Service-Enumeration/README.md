# Service Enumeration

## Overview

This phase focuses on identifying and analyzing the services running on the target machine after the initial port scan.

Each exposed service was individually enumerated to determine:
- Service version
- Configuration
- Authentication methods
- Shared resources
- Potential vulnerabilities
- Exploitation opportunities

The information collected during this phase was used to plan the exploitation stage.

---

## Enumerated Services

| Service | Port | Status |
|---------|------|--------|
| FTP | 21 | Completed |
| SSH | 22 | Completed |
| Telnet | 23 | Completed |
| SMTP | 25 | Completed |
| DNS | 53 | Completed |
| HTTP | 80 | Completed |
| RPCBind | 111 | Completed |
| SMB (Samba) | 139,445 | Completed |
| NFS | 2049 | Completed |
| DistCC | 3632 | Completed |
| Java RMI | 1099 | Completed |
| UnrealIRCd | 6667 | Completed |

---

## Tools Used

- Nmap
- NSE Scripts
- rpcinfo
- showmount
- enum4linux
- smbclient
- dig
- host
- netcat
- searchsploit
- Metasploit (for vulnerability verification only)

---

## Directory Structure

```
Service-Enumeration/
├── README.md
├── ftp.md
├── ssh.md
├── telnet.md
├── smtp.md
├── dns.md
├── http.md
├── rpcbind.md
├── smb.md
├── nfs.md
├── distcc.md
├── java-rmi.md
├── unrealircd.md
├── screenshots/
└── reports/
```

---

## Outcome

- Identified service versions
- Enumerated exposed resources
- Collected authentication information
- Verified vulnerable software versions
- Prepared targets for the Exploitation phase