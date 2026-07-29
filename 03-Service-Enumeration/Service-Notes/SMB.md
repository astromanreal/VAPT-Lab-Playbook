# SMB Enumeration

## Objective

Enumerate the SMB service to identify accessible shares, anonymous access, user accounts, and potential security weaknesses.

---

## Target Information

| Item | Value |
|------|-------|
| Service | SMB |
| Ports | 139, 445 |
| Software | Samba 3.0.20-Debian |
| Workgroup | WORKGROUP |
| Hostname | METASPLOITABLE |

---

## Anonymous Share Enumeration

### Command

```bash
smbclient -L //192.168.56.104 -N
```

### Result

Anonymous authentication succeeded.

Available shares:

| Share | Access |
|--------|--------|
| print$ | Denied |
| tmp | Accessible |
| opt | Denied |
| IPC$ | IPC |
| ADMIN$ | Denied |

---

## SMB Enumeration

### Command

```bash
enum4linux -a 192.168.56.104
```

### Findings

Successfully enumerated:

- Workgroup
- Hostname
- Samba Version
- Users
- Shares
- Null Session
- Domain SID

Anonymous sessions are permitted.

---

## Users Discovered

Interesting accounts:

- root
- msfadmin
- user
- mysql
- postgres
- ftp
- tomcat55
- www-data
- distccd

Numerous system service accounts were also identified.

---

## Share Enumeration

### Command

```bash
nmap --script smb-enum-shares,smb-enum-users,smb-os-discovery -p445 192.168.56.104
```

### Shares

| Share | Anonymous Access |
|--------|------------------|
| tmp | READ / WRITE |
| IPC$ | READ / WRITE |
| print$ | None |
| opt | None |
| ADMIN$ | None |

---

## Operating System Information

- Samba 3.0.20-Debian
- Unix
- Hostname: metasploitable
- Domain: localdomain

---

## Findings

### Anonymous Login Enabled

The SMB service accepts anonymous authentication without credentials.

### User Enumeration

User enumeration is possible through anonymous SMB access.

### Writable Share

The **tmp** share allows anonymous read/write access.

### Information Disclosure

SMB discloses:

- Operating system
- Workgroup
- Hostname
- User accounts
- Shares

---

## Risk Assessment

| Finding | Severity |
|----------|----------|
| Anonymous SMB Login | High |
| User Enumeration | Medium |
| Writable Anonymous Share | High |
| Information Disclosure | Medium |

---

## Conclusion

The SMB service exposes multiple security weaknesses including anonymous authentication, user enumeration, and a writable network share. These findings could assist an attacker in credential attacks, lateral movement, or malware staging.

---

## Evidence

- screenshots/smb/smbclient.png
- screenshots/smb/enum4linux.png
- screenshots/smb/nmap_smb.png