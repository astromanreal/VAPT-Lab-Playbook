# MySQL Enumeration (3306)

## Objective

Identify the MySQL service, determine its version, test for anonymous or weak authentication, enumerate available information, and assess whether the service can be exploited for unauthorized access or remote code execution.

---

# Service Identification

## Version Detection

Command:

```bash
nmap -sV -p3306 192.168.56.104
```

Result:

```
3306/tcp open  mysql  MySQL 5.0.51a-3ubuntu5
```

**Finding**

- Service: MySQL
- Version: 5.0.51a-3ubuntu5
- Status: Running

Screenshot:

```
01_mysql_service_version.png
```

---

# Service Enumeration

## MySQL Information

Command:

```bash
nmap --script mysql-info -p3306 192.168.56.104
```

Result:

- Protocol Version: 10
- MySQL Version: 5.0.51a-3ubuntu5
- Status: Autocommit
- Authentication required

Screenshot:

```
02_mysql_info.png
```

---

# Testing for Empty Password

Command:

```bash
nmap --script mysql-empty-password -p3306 192.168.56.104
```

Result:

No empty password detected.

Screenshot:

```
03_mysql_empty_password.png
```

---

# Manual Authentication

Command:

```bash
mysql -h 192.168.56.104 -u root --skip-ssl
```

Alternative:

```bash
mysql --ssl-mode=DISABLED -h 192.168.56.104 -u root
```

Observation:

- Initial SSL negotiation failed because the server is an old MySQL version.
- Disabling SSL allowed the connection attempt.
- Authentication still required a valid password.

Screenshot:

```
04_mysql_login_attempt.png
```

---

# Default Credential Testing

Tested common credentials:

```
root
root:root
mysql:mysql
admin:admin
msfadmin:msfadmin
(blank password)
```

Result:

No valid credentials discovered.

Screenshot:

```
05_mysql_default_credentials.png
```

---

# Public Exploit Research

Command:

```bash
searchsploit mysql 5.0
```

Relevant Results:

- Authentication Bypass
- Zero-Length Password Authentication Bypass
- User Defined Function (UDF) Command Execution

Observation:

The available exploits require one of the following:

- Valid database credentials
- Administrative privileges
- Local access

No unauthenticated remote code execution exploit was identified.

Screenshot:

```
06_mysql_searchsploit.png
```

---

# Brute Force Test (Optional)

Command:

```bash
nmap --script mysql-brute -p3306 192.168.56.104
```

Result:

No valid credentials recovered.

Screenshot:

```
07_mysql_bruteforce.png
```

---

# Security Assessment

## Identified Risks

- Outdated MySQL version
- Authentication required
- Possible exposure to weak-password attacks
- Potential privilege escalation if valid credentials are obtained

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Enumeration | ✅ Successful |
| Service Detection | ✅ Successful |
| Authentication Required | ✅ Yes |
| Anonymous Login | ❌ Failed |
| Empty Password | ❌ Not Found |
| Default Credentials | ❌ Failed |
| SearchSploit Review | ✅ Completed |
| Remote Code Execution | ❌ Not Possible Without Credentials |

---

# Commands Executed

```bash
nmap -sV -p3306 192.168.56.104

nmap --script mysql-info -p3306 192.168.56.104

nmap --script mysql-empty-password -p3306 192.168.56.104

mysql -h 192.168.56.104 -u root --skip-ssl

searchsploit mysql 5.0

nmap --script mysql-brute -p3306 192.168.56.104
```

---

# Conclusion

The MySQL service was successfully identified as **MySQL 5.0.51a-3ubuntu5**. Enumeration confirmed the service version and protocol information. Authentication was enforced, and attempts using anonymous access, empty passwords, and common default credentials were unsuccessful. Public exploit research revealed techniques that require valid credentials or elevated privileges; therefore, no unauthenticated exploitation path was identified during this assessment.

The service should be documented as an exposed database requiring authentication. If credentials are obtained through another attack vector (e.g., SMB, SSH, web application, or password reuse), further database enumeration and potential privilege escalation techniques such as UDF-based command execution should be assessed.