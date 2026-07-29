# Samba (SMB) Enumeration

## Objective

Identify the SMB service, enumerate available shares, discover users, determine the Samba version, and identify potential attack vectors.

---

# Service Identification

## Version Detection

**Command**

```bash
nmap -sV -p139,445 192.168.56.104
```

**Result**

```
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
Samba smbd 3.0.20-Debian
```

**Finding**

- Service: SMB (Samba)
- Version: 3.0.20-Debian
- Ports: 139, 445

**Screenshot**

```
01_samba_version.png
```

---

# SMB Enumeration

## SMB NSE Enumeration

**Command**

```bash
nmap --script smb-os-discovery,smb-enum-shares,smb-enum-users,smb-protocols -p139,445 192.168.56.104
```

**Observation**

The NSE scripts identified:

- Hostname
- Operating System
- Workgroup
- Shared folders
- SMB protocol information

**Screenshot**

```
02_samba_nse_enum.png
```

---

## Enumerating Shares

**Command**

```bash
smbclient -L //192.168.56.104 -N
```

**Observation**

The following shares were discovered:

- print$
- tmp
- opt
- IPC$

Anonymous access was permitted for some shares.

**Screenshot**

```
03_smbclient_shares.png
```

---

## Accessing Anonymous Share

**Command**

```bash
smbclient //192.168.56.104/tmp -N
```

**Observation**

Anonymous access to the **tmp** share was successful.

Basic commands executed:

```text
ls
pwd
get <file>
put <file>
```

**Screenshot**

```
04_smbclient_tmp_share.png
```

---

## User Enumeration

**Command**

```bash
enum4linux -a 192.168.56.104
```

**Observation**

The enumeration revealed:

- Host information
- Workgroup
- Local users
- Shared resources
- Password policy
- Operating system details

**Screenshot**

```
05_enum4linux.png
```

---

# Vulnerability Research

## SearchSploit

**Command**

```bash
searchsploit samba 3.0.20
```

**Observation**

Multiple public exploits were identified for Samba 3.0.20, including the well-known **username map script** remote command execution vulnerability.

**Screenshot**

```
06_searchsploit_samba.png
```

---

## Metasploit Module Search

**Command**

```text
msfconsole
search samba
```

**Observation**

Metasploit identified modules compatible with the detected Samba version, including remote code execution modules.

**Screenshot**

```
07_metasploit_samba.png
```

---

# Security Assessment

## Identified Risks

- Outdated Samba version
- Anonymous share access
- Information disclosure
- Publicly known remote code execution vulnerabilities
- Weak share permissions

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Detection | ✅ Successful |
| Share Enumeration | ✅ Successful |
| Anonymous Share Access | ✅ Successful |
| User Enumeration | ✅ Successful |
| SearchSploit Review | ✅ Completed |
| Metasploit Module Review | ✅ Completed |
| Remote Exploitation | ⏳ Documented Separately |

---

# Commands Executed

```bash
nmap -sV -p139,445 192.168.56.104

nmap --script smb-os-discovery,smb-enum-shares,smb-enum-users,smb-protocols -p139,445 192.168.56.104

smbclient -L //192.168.56.104 -N

smbclient //192.168.56.104/tmp -N

enum4linux -a 192.168.56.104

searchsploit samba 3.0.20

msfconsole
search samba
```

---

# Conclusion

The Samba service was successfully enumerated and identified as **Samba 3.0.20-Debian**. Enumeration revealed accessible SMB shares, including anonymous access to the **tmp** share, along with detailed host and user information. Public exploit research confirmed that this outdated version is affected by multiple known vulnerabilities, including remote code execution issues. No exploitation was performed during this enumeration phase; exploitation will be documented separately in the **Exploitation** section of this assessment.