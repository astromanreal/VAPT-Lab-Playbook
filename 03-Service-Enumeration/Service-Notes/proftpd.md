# ProFTPD (Port 2121)

## Objective

Enumerate the ProFTPD service, identify its version, test for anonymous authentication, research publicly available exploits, and assess whether the detected version is vulnerable.

---

## Service Detection

Identify the FTP service and version.

```bash
nmap -sV -p2121 192.168.56.104
```

Output

```
PORT     STATE SERVICE VERSION
2121/tcp open  ftp     ProFTPD 1.3.1
Service Info: OS: Unix
```

**Finding**

- Port **2121** is open.
- FTP service is running **ProFTPD 1.3.1** on a Unix-based operating system.

**Screenshot**

**Image:** `proftpd-service-scan.png`

---

## Banner Grabbing

Connect using Netcat to retrieve the FTP banner.

```bash
nc 192.168.56.104 2121
```

Output

```
220 ProFTPD 1.3.1 Server (Debian)
```

**Finding**

The FTP banner confirms the server is **ProFTPD 1.3.1** running on Debian.

**Screenshot**

**Image:** `proftpd-banner.png`

---

## FTP Enumeration

Run Nmap FTP enumeration scripts.

```bash
nmap --script ftp-anon,ftp-syst -p2121 192.168.56.104
```

Output

```
2121/tcp open
```

No additional information such as anonymous access or system information was disclosed.

**Finding**

The FTP service did not expose anonymous access or additional system details through the NSE scripts.

**Screenshot**

**Image:** `proftpd-nmap-scripts.png`

---

## Anonymous Login Test

Attempt to authenticate using the anonymous account.

```bash
ftp 192.168.56.104 2121
```

Credentials

```
Username: anonymous
Password: anonymous
```

Output

```
530 Login incorrect.
```

**Finding**

Anonymous FTP login is **disabled**.

**Screenshot**

**Image:** `proftpd-anonymous-login.png`

---

## Exploit Research

Search Exploit-DB for vulnerabilities affecting ProFTPD.

```bash
searchsploit proftpd
```

Relevant results

```
ProFTPD 1.2 - 1.3.0 sreplace Buffer Overflow
ProFTPD 1.3.2rc3 - 1.3.3b Telnet IAC Buffer Overflow
ProFTPD 1.3.3c Backdoor Command Execution
ProFTPD 1.3.5 Mod_Copy Command Execution
```

**Finding**

Several public exploits exist for other ProFTPD versions, but none specifically target **ProFTPD 1.3.1**.

**Screenshot**

**Image:** `proftpd-searchsploit.png`

---

## Metasploit Research

Start Metasploit.

```bash
msfconsole -q
```

Search for available ProFTPD modules.

```bash
search proftpd
```

Available modules

```
exploit/linux/ftp/proftp_sreplace
exploit/linux/ftp/proftp_telnet_iac
exploit/unix/ftp/proftpd_modcopy_exec
exploit/unix/ftp/proftpd_133c_backdoor
```

**Version Comparison**

| Module | Vulnerable Version | Applicable |
|---------|--------------------|------------|
| proftp_sreplace | 1.2 – 1.3.0 | ❌ No |
| proftp_telnet_iac | 1.3.2 – 1.3.3b | ❌ No |
| proftpd_modcopy_exec | 1.3.5 | ❌ No |
| proftpd_133c_backdoor | 1.3.3c | ❌ No |

**Finding**

Although Metasploit contains several ProFTPD exploits, none match the detected version (**1.3.1**). Therefore, no suitable exploit was available during testing.

**Screenshot**

**Image:** `proftpd-metasploit-search.png`

---

# Summary

| Item | Result |
|------|--------|
| Port | 2121 |
| Service | ProFTPD |
| Version | 1.3.1 |
| Banner Retrieved | Yes |
| Anonymous Login | Disabled |
| Public Exploit Available | No |
| Metasploit Module Applicable | No |
| Successful Exploitation | No |

---

## Conclusion

The ProFTPD service running on **port 2121** was successfully identified as **ProFTPD 1.3.1**. Anonymous authentication was tested and found to be disabled. Exploit research using SearchSploit and Metasploit identified multiple vulnerabilities affecting other ProFTPD versions; however, none applied to the detected version. Based on the assessment, no practical exploitation path was identified for this service.