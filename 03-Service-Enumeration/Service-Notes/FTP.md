# FTP Enumeration

## Objective

Enumerate the FTP service to identify authentication methods, accessible resources, server configuration, and potential security weaknesses before attempting exploitation.

---

## Service Information

| Item | Value |
|------|-------|
| Port | 21/TCP |
| Service | FTP |
| Software | vsftpd |
| Version | 2.3.4 |
| Authentication | Anonymous Login Enabled |

---

## Enumeration Methodology

The FTP service was manually enumerated to verify the results obtained during the Nmap service detection phase.

The objectives of this enumeration were to:

- Verify anonymous authentication.
- Inspect accessible directories.
- Identify downloadable files.
- Determine whether file upload is permitted.
- Identify sensitive information exposed through the FTP service.

---

## Commands Executed

### Connect to FTP

```bash
ftp 192.168.56.104
```

### Login

**Username**

```text
anonymous
```

**Password**

```text
anonymous
```

---

### Verify Current Directory

```bash
pwd
```

---

### List Directory Contents

```bash
ls
```

```bash
dir
```

---

### Display Available Commands

```bash
help
```

---

### Switch Transfer Modes

```bash
binary
```

```bash
ascii
```

---

### Attempt to Download Sensitive File

```bash
get /etc/passwd
```

---

## Results

### Anonymous Authentication

Anonymous login was successful.

**Evidence**

```
230 Login successful.
```

---

### Working Directory

```
Remote directory: /
```

---

### Directory Listing

Both `ls` and `dir` executed successfully.

No files or directories were displayed.

---

### FTP Commands

The server supports the standard FTP command set including:

- GET
- PUT
- DELETE
- RENAME
- MKDIR
- RMDIR

The presence of these commands does not confirm that the current user has permission to perform these actions.

---

### File Download Attempt

An attempt was made to download `/etc/passwd`.

```
ftp: Can't access '/etc/passwd': Permission denied
```

The anonymous FTP user does not have permission to access arbitrary files on the operating system.

---

## Findings

### Positive Findings

- Anonymous FTP authentication is enabled.
- Manual testing confirms the Nmap NSE results.
- The service is running vsftpd 2.3.4.

### Negative Findings

- No files were exposed through the anonymous FTP directory.
- No evidence of sensitive information disclosure was observed.
- Access to protected system files is restricted.

---

## Risk Assessment

| Finding | Risk |
|---------|------|
| Anonymous FTP Login | Medium |
| Public FTP Service | Medium |
| Direct Access to System Files | Not Observed |

---

## Conclusion

The FTP service permits anonymous authentication, which should be considered a security concern because unauthenticated users can access the service.

During this assessment, no publicly accessible files or sensitive information were identified, and attempts to access protected system files were correctly denied.

Additional assessment is required to determine whether the detected vsftpd version is susceptible to known vulnerabilities and whether exploitation is applicable in this environment.

---

## Evidence

- `screenshots/ftp/ftp-login-success.png`

---

## Next Step

Research the detected software version and identify publicly documented vulnerabilities before proceeding to exploitation.