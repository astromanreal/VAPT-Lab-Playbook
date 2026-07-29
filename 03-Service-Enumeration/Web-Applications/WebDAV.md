# WebDAV Enumeration

## Objective

Assess the WebDAV service to determine supported methods, upload capabilities, and the potential for remote code execution.

---

## URL

```
http://192.168.56.104/dav/
```

---

## Enumeration Performed

### Supported HTTP Methods

**Command**

```bash
curl -X OPTIONS -i http://192.168.56.104/dav/
```

### Result

WebDAV support confirmed.

```
DAV: 1,2
MS-Author-Via: DAV
```

Allowed methods included:

- GET
- POST
- DELETE
- TRACE
- PROPFIND
- PROPPATCH
- COPY
- MOVE
- LOCK
- UNLOCK

---

### WebDAV Validation

**Command**

```bash
davtest -url http://192.168.56.104/dav/
```

### Results

Successful operations:

- Directory creation (MKCOL)
- File upload (PUT)
- PHP file upload
- HTML upload
- Text file upload

Execution testing:

| File Type | Result |
|-----------|--------|
| PHP | Executed |
| HTML | Accessible |
| TXT | Accessible |
| JSP | Failed |
| CGI | Failed |
| ASP | Failed |

---

## Findings

### Anonymous File Upload

The WebDAV service permits unauthenticated file uploads.

### PHP Execution

Uploaded PHP files are executed by the web server, indicating a potential Remote Code Execution attack path.

### WebDAV Enabled

WebDAV is fully enabled with multiple authoring methods available.

---

## Risk Assessment

| Finding | Severity |
|---------|----------|
| WebDAV Enabled | Medium |
| Anonymous File Upload | High |
| Executable PHP Upload | Critical |

---

## Conclusion

The WebDAV service allows directory creation and arbitrary file uploads. Uploaded PHP files are executed by the web server, creating a high-risk path to remote code execution if an attacker uploads a malicious PHP payload.

---

## Evidence

- webdav_options.png
- davtest_results.png