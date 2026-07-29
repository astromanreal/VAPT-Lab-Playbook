# RPCBind Enumeration

## Objective

Enumerate the RPCBind (Portmapper) service to identify registered RPC programs, discover associated services such as NFS and mountd, and determine whether the service exposes any known vulnerabilities or misconfigurations.

---

# Service Identification

## Version Detection

Command:

```bash
nmap -sV -p111 192.168.56.104
```

Result:

```
111/tcp open  rpcbind 2 (RPC #100000)
```

**Finding**

- Service: RPCBind (Portmapper)
- Version: RPCBind v2
- RPC Program Number: 100000

Screenshot:

```
01_rpcbind_service_version.png
```

---

# Service Enumeration

## Enumerate Registered RPC Programs

Command:

```bash
rpcinfo -p 192.168.56.104
```

Result:

Discovered the following registered RPC services:

- Portmapper (RPCBind)
- NFS
- Mountd
- Nlockmgr
- Status (rpc.statd)

Screenshot:

```
02_rpcinfo_programs.png
```

---

## Nmap RPC Enumeration

Command:

```bash
nmap --script rpcinfo -p111 192.168.56.104
```

Result:

Nmap confirmed the registered RPC services and their ports:

| Program | Service | Port |
|----------|---------|------|
| 100000 | rpcbind | 111 |
| 100003 | NFS | 2049 |
| 100005 | mountd | 56736 / 57266 |
| 100021 | nlockmgr | 52132 / 53952 |
| 100024 | status | 46993 / 55537 |

Screenshot:

```
03_rpcinfo_nmap.png
```

---

# Public Exploit Research

Command:

```bash
searchsploit rpcbind
```

Result:

Available public exploits include:

- rpcbind CALLIT Procedure UDP Crash (DoS)
- RPCBind / libtirpc Denial of Service
- Wietse Venema Rpcbind Replacement Denial of Service

Observation:

Only Denial-of-Service vulnerabilities were identified. No authenticated or unauthenticated Remote Code Execution (RCE) exploits were found for the detected RPCBind version.

Screenshot:

```
04_rpcbind_searchsploit.png
```

---

# Security Assessment

## Identified Risks

- RPC services are exposed over the network.
- RPCBind reveals information about registered RPC services.
- NFS and mountd services increase the attack surface if misconfigured.
- Public exploits are limited to Denial-of-Service attacks.

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Enumeration | ✅ Successful |
| RPC Program Enumeration | ✅ Successful |
| NFS Discovery | ✅ Successful |
| mountd Discovery | ✅ Successful |
| Public Exploit Research | ✅ Completed |
| Remote Code Execution | ❌ Not Identified |
| Denial of Service | ⚠️ Public PoCs Available |

---

# Commands Executed

```bash
nmap -sV -p111 192.168.56.104

rpcinfo -p 192.168.56.104

nmap --script rpcinfo -p111 192.168.56.104

searchsploit rpcbind
```

---

# Conclusion

The RPCBind service was successfully identified as **RPCBind v2 (RPC Program #100000)**. Enumeration revealed multiple registered RPC services including **NFS**, **mountd**, **nlockmgr**, and **rpc.statd**, confirming that the host provides network file system functionality. Public exploit research identified only Denial-of-Service vulnerabilities affecting RPCBind, with no evidence of an unauthenticated Remote Code Execution path. The primary security concern is the exposure of RPC-related services, which should be further assessed through dedicated NFS and mountd enumeration.