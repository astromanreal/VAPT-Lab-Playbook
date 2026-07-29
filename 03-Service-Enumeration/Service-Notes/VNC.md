# VNC Enumeration

## Objective

Identify the VNC service version, supported authentication methods, and determine whether remote desktop access is exposed.

---

## Service Information

| Property | Value |
|----------|-------|
| Port | 5900 |
| Service | VNC |
| Protocol Version | 3.3 |
| Authentication | VNC Password Authentication |

---

## Commands Executed

### Version Detection

```bash
nmap -sV -p5900 192.168.56.104
```

---

### NSE Enumeration

```bash
nmap --script vnc-info -p5900 192.168.56.104
```

Result:

```
Protocol Version: 3.3

Security Types:
VNC Authentication (2)
```

---

### Search for Public Exploits

```bash
searchsploit vnc
```

Several VNC-related exploits were identified, although none specifically matched the detected service.

---

### Manual Connection

```bash
vncviewer 192.168.56.104:0
```

Connection Result:

```
Connected using protocol version 3.3

Password authentication required
```

---

## Findings

### Service Version

```
VNC Protocol 3.3
```

---

### Authentication

The VNC server requires password authentication before allowing desktop access.

---

### Attack Surface

- Password authentication
- Weak or default VNC passwords
- Credential reuse
- Known vulnerabilities affecting older VNC implementations

---

## Analysis

The target exposes a VNC remote desktop service over TCP port 5900. Enumeration confirmed protocol version 3.3 and password-based authentication. No anonymous access was available. Manual connection successfully reached the authentication prompt, confirming the service is operational.

No exploitation was attempted during the enumeration phase.

---

## Screenshots

- screenshots/vnc/nmap_version.png
- screenshots/vnc/vnc_info.png
- screenshots/vnc/vncviewer_password.png
- screenshots/vnc/searchsploit.png

---

## Next Step

Continue enumeration of the next exposed service.