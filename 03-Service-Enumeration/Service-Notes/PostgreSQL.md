# PostgreSQL Enumeration

## Objective

Enumerate the PostgreSQL database service to identify its version, authentication status, accessible databases, and potential attack vectors.

---

## Service Information

| Property | Value |
|----------|-------|
| Port | 5432 |
| Service | PostgreSQL |
| Version | PostgreSQL 8.3.1 (Nmap: 8.3.0–8.3.7) |
| Authentication | Password Authentication Enabled |

---

## Commands Executed

### Version Detection

```bash
nmap -sV -p5432 192.168.56.104
```

---

### PostgreSQL Brute Force Script

```bash
nmap --script pgsql-brute -p5432 192.168.56.104
```

Result:

- Script timed out
- No credentials discovered

---

### Manual Connection

```bash
psql -h 192.168.56.104 -U postgres
```

Successful connection:

```
postgres=#
```

---

### Search for Known Vulnerabilities

```bash
searchsploit postgresql 8.3
```

Results:

- PostgreSQL 8.2/8.3/8.4 - UDF Command Execution
- PostgreSQL 8.3.6 - Information Disclosure
- PostgreSQL 8.3.6 - DoS

---

## Findings

### PostgreSQL Version

```
PostgreSQL 8.3.1
```

---

### Authentication

Manual login was successful using the PostgreSQL client.

---

### Potential Attack Surface

- Database enumeration
- User enumeration
- Weak credentials
- User Defined Function (UDF) abuse
- Local privilege escalation if filesystem write is allowed

---

## Analysis

The PostgreSQL service is accessible over the network and accepts authenticated connections. Enumeration confirmed an outdated version that has publicly documented vulnerabilities. Further database enumeration can be performed after obtaining valid credentials or during post-exploitation.

---

## Next Step

Enumerate the VNC service running on port 5900.