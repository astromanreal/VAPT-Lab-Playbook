# SSH Enumeration (22)

## Objective

Identify the SSH service, determine its version, enumerate supported authentication methods and cryptographic algorithms, collect host key information, and assess the service for known vulnerabilities or weak configurations.

---

# Service Identification

## Version Detection

Command:

```bash
nmap -sV -p22 192.168.56.104
```

Result:

```
22/tcp open  ssh  OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
```

**Finding**

- Service: SSH
- Software: OpenSSH
- Version: 4.7p1 Debian 8ubuntu1
- Protocol: SSH-2.0

Screenshot:

```
01_ssh_version.png
```

---

# Service Enumeration

## SSH Host Key Enumeration

Command:

```bash
nmap --script ssh-hostkey -p22 192.168.56.104
```

Result:

```
DSA Host Key
RSA Host Key
```

**Finding**

The SSH server exposes both RSA and DSA host keys for client authentication.

Screenshot:

```
02_ssh_hostkey.png
```

---

## Supported Authentication Methods

Command:

```bash
nmap --script ssh-auth-methods -p22 192.168.56.104
```

Result:

```
Supported authentication methods:
- publickey
- password
```

**Finding**

The SSH server allows both password-based and public key authentication.

Screenshot:

```
03_ssh_auth_methods.png
```

---

## Supported Cryptographic Algorithms

Command:

```bash
nmap --script ssh2-enum-algos -p22 192.168.56.104
```

Result:

The server supports multiple legacy algorithms including:

- Diffie-Hellman Group1 SHA1
- SSH-RSA
- SSH-DSS
- CBC encryption ciphers
- HMAC-MD5
- HMAC-SHA1

**Finding**

Several legacy cryptographic algorithms are enabled, which would not be recommended on modern production systems.

Screenshot:

```
04_ssh_algorithms.png
```

---

## Banner Grabbing

Command:

```bash
nc 192.168.56.104 22
```

Result:

```
SSH-2.0-OpenSSH_4.7p1 Debian-8ubuntu1
```

**Finding**

Banner grabbing confirmed the OpenSSH version without authentication.

Screenshot:

```
05_ssh_banner.png
```

---

## Manual Authentication Test

Command:

```bash
ssh msfadmin@192.168.56.104
```

Result:

```
Unable to negotiate with 192.168.56.104 port 22:
no matching host key type found.
Their offer: ssh-rsa, ssh-dss
```

**Observation**

The SSH server only offers legacy host key algorithms (RSA and DSA). Modern OpenSSH clients disable these algorithms by default, preventing authentication without explicitly enabling legacy compatibility options.

Screenshot:

```
06_ssh_login_attempt.png
```

---

## Public Exploit Research

Command:

```bash
searchsploit openssh
```

Result:

Public exploits were identified for multiple OpenSSH versions, including:

- Username Enumeration
- SCP File Overwrite
- Authenticated Privilege Escalation
- Denial of Service

No unauthenticated remote code execution exploit applicable to **OpenSSH 4.7p1** was identified during this assessment.

Screenshot:

```
07_ssh_searchsploit.png
```

---

# Security Assessment

## Identified Risks

- Outdated OpenSSH version
- Password authentication enabled
- Legacy SSH-RSA and SSH-DSS host keys
- Legacy key exchange algorithms
- Legacy CBC encryption ciphers
- Legacy SHA1 and MD5 MAC algorithms

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Enumeration | ✅ Successful |
| Host Key Enumeration | ✅ Successful |
| Authentication Methods | ✅ Enumerated |
| Cryptographic Algorithms | ✅ Enumerated |
| Banner Grabbing | ✅ Successful |
| Manual Login | ⚠️ Blocked by client compatibility |
| SearchSploit Review | ✅ Completed |
| Unauthenticated RCE | ❌ Not Identified |

---

# Commands Executed

```bash
nmap -sV -p22 192.168.56.104

nmap --script ssh-hostkey -p22 192.168.56.104

nmap --script ssh-auth-methods -p22 192.168.56.104

nmap --script ssh2-enum-algos -p22 192.168.56.104

nc 192.168.56.104 22

ssh -v 192.168.56.104

ssh msfadmin@192.168.56.104

searchsploit openssh
```

---

# Conclusion

The SSH service was successfully identified as **OpenSSH 4.7p1 Debian 8ubuntu1**. Enumeration revealed support for both password and public key authentication, along with legacy host key, key exchange, encryption, and message authentication algorithms. Banner grabbing confirmed the server version without authentication. Manual login from a modern OpenSSH client failed because the server only supports deprecated host key algorithms that are disabled by default in current clients. Public exploit research identified several historical vulnerabilities affecting older OpenSSH releases; however, no unauthenticated remote code execution vulnerability applicable to this configuration was identified during the assessment. The service should be documented as an outdated SSH implementation with legacy cryptographic support that may warrant further testing if valid credentials are obtained.