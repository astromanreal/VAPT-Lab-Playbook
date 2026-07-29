# Apache Tomcat Enumeration (8180)

## Objective

Identify the Apache Tomcat service, determine its version, enumerate accessible resources, identify administrative interfaces, and assess potential attack vectors.

---

# Service Identification

## Version Detection

**Command**

```bash
nmap -sV -p8180 192.168.56.104
```

**Result**

```
8180/tcp open  http  Apache Tomcat/Coyote JSP engine 1.1
```

**Finding**

- Service: Apache Tomcat
- Port: 8180
- Web Server: Apache-Coyote
- Tomcat Version: 5.5

**Screenshot**

```
01_tomcat_version.png
```

---

# Web Technology Fingerprinting

## WhatWeb Enumeration

**Command**

```bash
whatweb http://192.168.56.104:8180
```

**Observation**

The scan identified:

- Apache Tomcat 5.5
- Apache-Coyote/1.1
- Default Tomcat installation
- HTTP Server running on port 8180

**Screenshot**

```
02_whatweb.png
```

---

# HTTP Header Enumeration

## HTTP Response Headers

**Command**

```bash
curl -I http://192.168.56.104:8180
```

**Result**

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Type: text/html
```

**Finding**

The HTTP headers disclose:

- Web server software
- Server version
- Content type

This information can assist attackers in identifying version-specific vulnerabilities.

**Screenshot**

```
03_http_headers.png
```

---

## Nmap HTTP Enumeration

**Command**

```bash
nmap --script http-title,http-headers -p8180 192.168.56.104
```

**Observation**

The NSE scripts confirmed:

- HTTP Title: Apache Tomcat/5.5
- Server Header: Apache-Coyote/1.1
- HTTP response headers

**Screenshot**

```
04_http_nse.png
```

---

# Directory Enumeration

## Gobuster Scan

**Command**

```bash
gobuster dir \
-u http://192.168.56.104:8180 \
-w /usr/share/wordlists/dirb/common.txt
```

**Directories Discovered**

```
/admin
/host-manager
/manager
/jsp-examples
/servlets-examples
/tomcat-docs
/WEB-INF
/webdav
/favicon.ico
```

**Finding**

Several default Tomcat directories were exposed, including:

- Manager Application
- Host Manager
- Documentation
- JSP Examples
- Servlet Examples

These directories may expose administrative functionality or sensitive information if improperly configured.

**Screenshot**

```
05_gobuster.png
```

---

# Public Exploit Research

## SearchSploit

**Command**

```bash
searchsploit tomcat
```

**Observation**

SearchSploit returned numerous public vulnerabilities affecting Apache Tomcat, including:

- Directory Traversal
- File Disclosure
- Remote Code Execution
- Denial of Service
- Cross-Site Scripting (XSS)
- Ghostcat (AJP File Read / Inclusion)

Most exploits target newer or different Tomcat versions than the detected installation.

**Screenshot**

```
06_searchsploit.png
```

---

# Security Assessment

## Identified Risks

- Outdated Apache Tomcat installation
- Default Tomcat resources exposed
- Administrative interfaces discovered
- Server version disclosure
- Default documentation publicly accessible
- Example applications exposed

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Detection | ✅ Successful |
| Technology Fingerprinting | ✅ Successful |
| HTTP Header Enumeration | ✅ Successful |
| Directory Enumeration | ✅ Successful |
| SearchSploit Review | ✅ Completed |
| Administrative Pages Identified | ✅ Yes |
| Remote Code Execution | ⏳ Not Attempted |
| Authentication Bypass | ⏳ Not Attempted |

---

# Commands Executed

```bash
nmap -sV -p8180 192.168.56.104

whatweb http://192.168.56.104:8180

curl -I http://192.168.56.104:8180

nmap --script http-title,http-headers -p8180 192.168.56.104

gobuster dir \
-u http://192.168.56.104:8180 \
-w /usr/share/wordlists/dirb/common.txt

searchsploit tomcat
```

---

# Conclusion

The Apache Tomcat service running on **port 8180** was successfully identified and enumerated. Fingerprinting confirmed **Apache Tomcat 5.5** using the **Apache-Coyote/1.1** web server. Directory enumeration revealed several default Tomcat resources, including the **Manager**, **Host Manager**, **JSP Examples**, **Servlet Examples**, and **Tomcat Documentation**, indicating that the installation retains default components. Public exploit research identified multiple known vulnerabilities affecting various Tomcat versions; however, no exploitation was performed during this phase. Any authentication testing, deployment of malicious WAR files, or privilege escalation activities will be documented separately in the **Exploitation** section of this assessment.