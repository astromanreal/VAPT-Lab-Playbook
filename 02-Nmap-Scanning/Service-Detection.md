# Service Detection

## Objective

Identify the software, versions, operating system, and default configuration of services running on the target host. This information is used to prioritize enumeration and identify potential vulnerabilities.

---

## Command

```bash
nmap -sV -sC -O \
-p21,22,23,25,53,80,111,139,445,512,513,514,1099,1524,2049,2121,3306,3632,5432,5900,6000,6667,6697,8009,8180,8787,33009,36408,57682,60194 \
-oA service_scan 192.168.56.104
```

---

## Scan Options

| Option | Description |
|---------|-------------|
| `-sV` | Detect service versions |
| `-sC` | Execute default safe NSE scripts |
| `-O` | Attempt operating system detection |
| `-p` | Scan only the previously identified open ports |
| `-oA` | Save results in Normal, XML, and Grepable formats |

---

## Findings

| Port | Service | Version / Information |
|------|---------|-----------------------|
| 21 | FTP | vsftpd 2.3.4 (Anonymous login enabled) |
| 22 | SSH | OpenSSH 4.7p1 Debian |
| 23 | Telnet | Linux telnetd |
| 25 | SMTP | Postfix SMTP Server |
| 53 | DNS | ISC BIND 9.4.2 |
| 80 | HTTP | Apache HTTP Server 2.2.8 |
| 111 | RPCBind | RPCBind v2 |
| 139 | NetBIOS | Samba 3.x–4.x |
| 445 | SMB | Samba 3.0.20-Debian |
| 512 | rexec | Netkit rexecd |
| 513 | rlogin | Remote Login Service |
| 514 | rsh | Netkit rshd |
| 1099 | Java RMI | GNU Classpath RMI Registry |
| 1524 | Bind Shell | Root Bind Shell |
| 2049 | NFS | Network File System |
| 2121 | FTP | ProFTPD 1.3.1 |
| 3306 | MySQL | MySQL 5.0.51a |
| 3632 | DistCC | DistCC 4.2.4 |
| 5432 | PostgreSQL | PostgreSQL 8.3 |
| 5900 | VNC | VNC Protocol 3.3 |
| 6000 | X11 | X Window System |
| 6667 | IRC | UnrealIRCd |
| 6697 | IRC | UnrealIRCd (TLS) |
| 8009 | AJP13 | Apache JServ Protocol |
| 8180 | HTTP | Apache Tomcat 5.5 |
| 8787 | DRb | Ruby Distributed Object Service |
| 33009 | Nlockmgr | RPC Lock Manager |
| 36408 | Mountd | NFS Mount Daemon |
| 57682 | Status | RPC Status Service |
| 60194 | Java RMI | GNU Classpath RMI Registry |

---

## Key Observations

- Anonymous FTP login is enabled on the vsftpd service.
- Multiple legacy and insecure remote access services (Telnet, rexec, rlogin, and rsh) are exposed.
- Samba is running on ports 139 and 445, providing SMB file-sharing services.
- A root bind shell is exposed on port 1524, indicating an intentionally vulnerable service.
- DistCC, UnrealIRCd, and Apache Tomcat are present and require further investigation due to their historical security issues.
- Multiple RPC services suggest NFS is enabled and should be enumerated.
- Two database servers (MySQL and PostgreSQL) are accessible.
- The HTTP service on port 80 and the Tomcat instance on port 8180 represent primary web attack surfaces.

---

## Operating System Detection

- Operating System: Linux
- Kernel Range: 2.6.9 – 2.6.33
- Network Distance: 1 Hop

---

## Analysis

The service detection scan identified numerous exposed services, including several legacy protocols and applications with publicly documented vulnerabilities. These findings significantly expand the target's attack surface and establish a clear roadmap for the enumeration phase.

Rather than attempting immediate exploitation, each identified service will be individually enumerated to verify configurations, identify weaknesses, collect evidence, and determine whether exploitation is possible.

---

## Next Step

Perform detailed enumeration of each exposed service, beginning with the highest-priority targets identified during the attack surface analysis.