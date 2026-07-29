# HTTP Enumeration

## Objective

Enumerate the HTTP service to identify exposed web applications, technologies, hidden resources, and potential attack vectors.

---

## Service Information

| Item | Value |
|------|-------|
| Port | 80/TCP |
| Service | HTTP |
| Web Server | Apache HTTP Server 2.2.8 |
| Operating System | Ubuntu |
| PHP Version | 5.2.4 |
| WebDAV | Enabled |

---

## Enumeration Performed

### Homepage Review

The default Metasploitable2 web page was successfully accessed.

**Status:** Accessible (HTTP 200)

---

### HTTP Header Inspection

**Command**

```bash
curl -I http://192.168.56.104
```

**Response**

```
HTTP/1.1 200 OK
Server: Apache/2.2.8 (Ubuntu) DAV/2
X-Powered-By: PHP/5.2.4-2ubuntu5.10
Content-Type: text/html
```

**Observations**

- Apache version disclosed.
- PHP version disclosed.
- WebDAV is enabled.
- Server banner is exposed.

---

### robots.txt

**Command**

```bash
curl http://192.168.56.104/robots.txt
```

**Result**

```
404 Not Found
```

No robots.txt file was present.

---

### Technology Fingerprinting

**Command**

```bash
whatweb http://192.168.56.104
```

**Technologies Identified**

- Apache 2.2.8
- Ubuntu
- PHP 5.2.4
- WebDAV
- X-Powered-By header
- Homepage Title: Metasploitable2 - Linux

---

### Directory Enumeration

**Command**

```bash
gobuster dir \
-u http://192.168.56.104 \
-w /usr/share/wordlists/dirb/common.txt
```

**Discovered Resources**

| Path | Status | Notes |
|------|--------|------|
| /dav/ | 301 | WebDAV directory |
| /phpMyAdmin/ | 301 | Database administration interface |
| /phpinfo.php | 200 | PHP configuration page |
| /test/ | 301 | Test directory |
| /twiki/ | 301 | TWiki application |
| /cgi-bin/ | 403 | Exists but access denied |
| /.htaccess | 403 | Protected |
| /.htpasswd | 403 | Protected |
| /.hta | 403 | Protected |
| /server-status | 403 | Exists but restricted |

---

## Findings

### Information Disclosure

The HTTP response headers disclose:

- Apache version
- PHP version
- Operating system
- WebDAV support

---

### Exposed Applications

Multiple web applications were identified including:

- phpMyAdmin
- TWiki
- PHPInfo
- WebDAV

These applications require individual assessment during later phases.

---

### Administrative Interfaces

phpMyAdmin was discovered and may expose database administration functionality if improperly configured.

---

### PHP Information Page

The phpinfo page is publicly accessible and may disclose sensitive server configuration details.

---

## Risk Assessment

| Finding | Severity |
|---------|----------|
| Version Disclosure | Low |
| phpinfo Exposed | Medium |
| WebDAV Enabled | Medium |
| phpMyAdmin Accessible | Medium |
| Multiple Web Applications | Informational |

---

## Conclusion

The HTTP service exposes several applications and administrative interfaces that significantly increase the attack surface. While no exploitation was attempted during this phase, the discovered resources will be individually enumerated and assessed in subsequent testing.

---

## Next Steps

- Enumerate WebDAV.
- Assess phpMyAdmin.
- Review phpinfo for sensitive information.
- Enumerate TWiki.
- Assess the test application.