# SMTP (Port 25) - Postfix Enumeration

## Objective
Enumerate the SMTP service running on the target and identify potential misconfigurations such as supported commands and user enumeration.

---

## Target

- **IP:** 192.168.56.104
- **Port:** 25
- **Service:** SMTP
- **Server:** Postfix (Ubuntu)

---

# 1. Service Enumeration

## Command

```bash
nmap -sV -p25 192.168.56.104
```

### Result

- Port **25/tcp** is open.
- Service detected as **Postfix SMTP**.

### Screenshot

**smtp-01-nmap-version.png**

---

# 2. Banner Grabbing

## Command

```bash
nc 192.168.56.104 25
```

### SMTP Commands

```
EHLO kali
QUIT
```

### Result

Banner revealed:

```
220 metasploitable.localdomain ESMTP Postfix (Ubuntu)
```

Server supports:

- PIPELINING
- SIZE
- VRFY
- ETRN
- STARTTLS
- ENHANCEDSTATUSCODES
- 8BITMIME
- DSN

### Screenshot

**smtp-02-banner.png**

---

# 3. Enumerate SMTP Commands

## Command

```bash
nmap --script smtp-commands -p25 192.168.56.104
```

### Result

Supported commands discovered:

- PIPELINING
- SIZE
- VRFY
- ETRN
- STARTTLS
- ENHANCEDSTATUSCODES
- 8BITMIME
- DSN

### Screenshot

**smtp-03-smtp-commands.png**

---

# 4. User Enumeration using VRFY

## Command

```bash
telnet 192.168.56.104 25
```

Inside telnet:

```
VRFY root
VRFY daemon
VRFY msfadmin
VRFY postgres
VRFY mysql
VRFY user
VRFY ftp
QUIT
```

### Result

Server responded with:

```
252 2.0.0 root
252 2.0.0 daemon
252 2.0.0 msfadmin
252 2.0.0 postgres
252 2.0.0 mysql
252 2.0.0 user
252 2.0.0 ftp
```

This indicates the SMTP server accepts **VRFY** requests, allowing user enumeration.

### Screenshot

**smtp-04-vrfy-users.png**

---

# 5. NSE User Enumeration

## Command

```bash
nmap --script smtp-enum-users -p25 192.168.56.104
```

### Result

```
Method RCPT returned an unhandled status code.
```

The NSE script could not enumerate users automatically, but manual VRFY enumeration was successful.

### Screenshot

**smtp-05-smtp-enum-users.png**

---

# 6. Search for Public Exploits

## SearchSploit

```bash
searchsploit postfix
```

```bash
searchsploit smtp
```

### Result

Only outdated or unrelated exploits were available.

No applicable remote code execution exploit matched the detected Postfix version.

### Screenshot

**smtp-06-searchsploit.png**

---

# 7. Metasploit Search

## Command

```bash
msfconsole -q
```

```text
search smtp
```

### Result

Numerous SMTP-related modules were listed, but none directly targeted the detected Postfix service.

No exploitation was performed.

### Screenshot

**smtp-07-metasploit-search.png**

---

# Findings

- SMTP service identified as **Postfix (Ubuntu)**.
- Server banner successfully obtained.
- SMTP capabilities enumerated.
- **VRFY command enabled**, allowing valid username enumeration.
- Valid usernames identified:
  - root
  - daemon
  - msfadmin
  - postgres
  - mysql
  - user
  - ftp
- No applicable remote exploit identified.
- Service remained accessible without authentication.

---

# Conclusion

The SMTP service was successfully enumerated. A security misconfiguration was identified where the **VRFY** command is enabled, allowing an attacker to enumerate valid system users. Although no direct remote code execution vulnerability was found, the discovered usernames can be leveraged in password attacks against services such as SSH, FTP, or Telnet during later phases of a penetration test.