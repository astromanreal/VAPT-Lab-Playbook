# Vulnerability Assessment Report

## Target

- Target Machine: Metasploitable 2
- IP Address: 192.168.56.104
- Assessment Type: Black Box Internal Network Assessment
- Tester: Manish Pandey
- Operating System: Linux 2.6.24-16-server

---

# Executive Summary

A vulnerability assessment and penetration test were conducted against the Metasploitable 2 virtual machine. The objective was to identify exposed services, enumerate them, exploit vulnerable services where applicable, and document the security impact.

The assessment identified several critical vulnerabilities leading to remote code execution and root-level compromise.

---

# Vulnerability Summary

| Service | Port | Vulnerability | Severity | CVE | Status |
|----------|------|---------------|----------|------|--------|
| DistCC | 3632 | Remote Command Execution | Critical | CVE-2004-2687 | Exploited |
| UnrealIRCd | 6667 | Backdoored IRC Server | Critical | CVE-2010-2075 | Exploited |
| Samba | 139/445 | Username Map Script RCE | Critical | CVE-2007-2447 | Exploited |
| Java RMI | 1099 | Insecure Remote Class Loading | Critical | CVE-2011-3556 | Exploited |
| FTP | 21 | Anonymous Login | High | N/A | Verified |
| NFS | 2049 | Export Enumeration | Medium | N/A | Verified |
| RPCBind | 111 | Information Disclosure | Medium | N/A | Verified |
| SSH | 22 | Legacy OpenSSH Version | Medium | N/A | Enumerated |

---

# Successful Exploitations

## DistCC

- Initial access obtained
- Shell user: daemon

Impact:

- Remote command execution
- Low-privileged shell

---

## UnrealIRCd

- Remote code execution achieved

Impact:

- Root shell obtained
- Full system compromise

---

## Samba

- Username Map Script vulnerability exploited

Impact:

- Root shell obtained
- Full administrative access

---

## Java RMI

- Remote class loading vulnerability exploited

Impact:

- Root shell obtained
- Complete system compromise

---

# Risk Assessment

| Severity | Count |
|----------|------:|
| Critical | 4 |
| High | 1 |
| Medium | 3 |
| Low | 0 |

---

# Recommendations

- Upgrade vulnerable software to supported versions.
- Disable unnecessary services.
- Restrict network exposure using firewall rules.
- Disable anonymous FTP access.
- Remove vulnerable Samba configuration options.
- Upgrade OpenSSH.
- Disable insecure Java RMI remote class loading.
- Regularly perform vulnerability assessments and penetration testing.

---

# Conclusion

The assessment demonstrated that multiple exposed services were vulnerable to publicly known exploits. Several vulnerabilities resulted in remote code execution, with three services providing direct root-level access. The findings highlight the importance of timely patch management, secure service configuration, and minimizing the attack surface.