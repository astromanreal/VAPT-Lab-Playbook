# Telnet (23)

## Enumeration

### 1. Version Detection
```bash
nmap -sV -p23 192.168.56.104
```

**Result**
```
23/tcp open  telnet  Linux telnetd
```

---

### 2. Banner Grabbing

```bash
telnet 192.168.56.104
```

**Result**

```
220 Login Banner
Linux telnetd
```

Metasploitable2 login banner is displayed.

---

### 3. Default Credentials

```text
Username: msfadmin
Password: msfadmin
```

**Login Successful**

```bash
whoami
hostname
id
pwd
uname -a
```

Output

```
whoami
msfadmin

hostname
metasploitable

id
uid=1000(msfadmin)

pwd
/home/msfadmin

uname -a
Linux metasploitable 2.6.24-16-server
```

---

### 4. Searchsploit

```bash
searchsploit telnet
```

Result

- No useful exploit for Linux telnetd on Metasploitable2.
- Mostly exploits for embedded devices, Windows, Solaris, and BSD.

---

### 5. Metasploit Modules

```bash
msfconsole -q
search telnet
```

Useful modules

```text
auxiliary/scanner/telnet/telnet_login
auxiliary/scanner/telnet/telnet_version
```

No direct RCE module exists for this Linux telnet service.

---

## Findings

- Telnet service is enabled.
- Plain-text authentication (credentials sent unencrypted).
- Default credentials (`msfadmin:msfadmin`) allow remote login.
- Shell access obtained successfully.

---

## Impact

An attacker can authenticate using default credentials and obtain an interactive shell, leading to complete system compromise depending on user privileges.

---

## Next Steps

- Perform Linux post-exploitation enumeration.
- Check `sudo -l`.
- Enumerate SUID/SGID binaries.
- Search for sensitive files and credentials.
- Attempt privilege escalation.