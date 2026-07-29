# phpMyAdmin Enumeration

## Objective

Enumerate the phpMyAdmin application to identify version information, authentication mechanisms, exposed resources, and potential security weaknesses.

---

## URL

```
http://192.168.56.104/phpMyAdmin/
```

---

## Service Information

| Item | Value |
|------|-------|
| Application | phpMyAdmin |
| Web Server | Apache 2.2.8 |
| PHP Version | 5.2.4 |
| Authentication | Login Form |
| Session Cookies | Enabled |

---

## HTTP Header Inspection

### Command

```bash
curl -I http://192.168.56.104/phpMyAdmin/
```

### Observations

- HTTP 200 OK
- Apache 2.2.8
- PHP 5.2.4
- Session cookies generated
- HttpOnly cookies present
- Last-Modified header exposed

---

## Technology Fingerprinting

### Command

```bash
whatweb http://192.168.56.104/phpMyAdmin/
```

### Technologies Identified

- Apache 2.2.8
- PHP 5.2.4
- phpMyAdmin
- WebDAV enabled
- HttpOnly cookies
- Login form detected
- JavaScript enabled

---

## Directory Enumeration

### Command

```bash
gobuster dir \
-u http://192.168.56.104/phpMyAdmin/ \
-w /usr/share/wordlists/dirb/common.txt
```

### Interesting Resources

| Resource | Description |
|----------|-------------|
| /setup/ | phpMyAdmin setup interface |
| /docs/ | Documentation |
| /README | Installation information |
| /ChangeLog | Version history |
| /LICENSE | License file |
| /robots.txt | Robots file |
| /phpinfo.php | PHP information page |
| /test/ | Test directory |
| /scripts/ | Client-side scripts |
| /libraries/ | Application libraries |

---

## Findings

### Version Disclosure

Apache and PHP versions are disclosed through HTTP headers.

### Administrative Interface

phpMyAdmin is publicly accessible through the web interface.

### Documentation Files

Documentation and changelog files are publicly accessible.

### Setup Directory

The setup interface is present and should be reviewed to determine whether it is accessible.

---

## Risk Assessment

| Finding | Severity |
|---------|----------|
| Version Disclosure | Low |
| Documentation Exposure | Low |
| Setup Directory Present | Medium |
| Administrative Interface Exposed | Medium |

---

## Conclusion

The phpMyAdmin administrative interface is publicly accessible and several auxiliary resources, including documentation and the setup directory, are exposed. Further assessment should determine whether default credentials, weak authentication, or known phpMyAdmin vulnerabilities are present.

---

## Evidence

- screenshots/http/phpmyadmin_login.png
- screenshots/http/phpmyadmin_whatweb.png
- screenshots/http/phpmyadmin_gobuster.png