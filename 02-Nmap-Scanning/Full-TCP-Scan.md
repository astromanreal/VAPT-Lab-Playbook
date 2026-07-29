# Full TCP Port Scan

## Objective

Identify all TCP ports exposed by the target system to determine the available attack surface for further enumeration.

---

## Command

```bash
nmap -Pn -p- -T4 -oA full_tcp_scan 192.168.56.104
```

---

## Scan Options

| Option | Description |
|---------|-------------|
| -Pn | Skip host discovery and assume the target is online. |
| -p- | Scan all 65,535 TCP ports. |
| -T4 | Use aggressive timing suitable for a lab environment. |
| -oA | Save output in Normal, XML, and Grepable formats. |

---

## Results

| Port | Service | Initial Assessment |
|------|---------|--------------------|
| 21 | FTP | File Transfer Protocol exposed |
| 22 | SSH | Secure remote administration |
| 23 | Telnet | Insecure remote login service |
| 25 | SMTP | Mail service |
| 53 | DNS | Domain Name Service |
| 80 | HTTP | Web application available |
| 111 | RPCBind | RPC service |
| 139 | NetBIOS | SMB related service |
| 445 | SMB | Windows file sharing |
| 512 | rexec | Legacy remote execution |
| 513 | rlogin | Legacy remote login |
| 514 | rsh | Remote shell service |
| 1099 | Java RMI | Java Remote Method Invocation |
| 1524 | ingreslock | Unusual service requiring investigation |
| 2049 | NFS | Network File System |
| 2121 | FTP | Additional FTP service |
| 3306 | MySQL | Database server |
| 3632 | DistCC | Distributed compiler service |
| 5432 | PostgreSQL | Database server |
| 5900 | VNC | Remote desktop |
| 6000 | X11 | X Window System |
| 6667 | IRC | Internet Relay Chat |
| 6697 | IRC Secure | IRC over TLS |
| 8009 | AJP | Apache JServ Protocol |
| 8180 | HTTP | Web application / Tomcat candidate |
| 8787 | Unknown | Requires enumeration |
| 33009 | Unknown | Requires enumeration |
| 36408 | Unknown | Requires enumeration |
| 57682 | Unknown | Requires enumeration |
| 60194 | Unknown | Requires enumeration |

---

## Analysis

The target exposes a large number of network services, significantly increasing its attack surface. Several services such as Telnet, SMB, DistCC, Java RMI, and legacy remote execution protocols are commonly associated with known vulnerabilities or insecure configurations.

These findings indicate that further service enumeration is required to determine software versions, authentication mechanisms, exposed resources, and potential vulnerabilities.

---

## Next Step

Perform service and version detection on all identified ports using Nmap.