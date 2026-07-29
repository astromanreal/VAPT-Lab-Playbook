# Attack Surface Analysis

## Objective

Prioritize exposed services for enumeration based on potential security risk, known vulnerabilities, and likelihood of successful exploitation.

---

## Identified Services

| Port | Service | Version | Priority | Reason |
|------|----------|----------|----------|--------|
|21|FTP|vsftpd 2.3.4|High|Anonymous login enabled and known vulnerable version|
|22|SSH|OpenSSH 4.7p1|Medium|Remote administration service|
|23|Telnet|Linux telnetd|High|Credentials transmitted in plaintext|
|25|SMTP|Postfix|Medium|User enumeration may be possible|
|53|DNS|BIND 9.4.2|Medium|Version disclosure|
|80|Apache|2.2.8|High|Primary web application|
|139|SMB|Samba|High|Known vulnerable Samba version|
|445|SMB|Samba 3.0.20|Critical|Remote file sharing service|
|2049|NFS|NFS|High|Potential filesystem exposure|
|2121|ProFTPD|1.3.1|High|Known historical vulnerabilities|
|3306|MySQL|5.0.51|Medium|Database service|
|3632|DistCC|4.2.4|Critical|Historically vulnerable to remote command execution|
|5432|PostgreSQL|8.3|Medium|Database service|
|6667|UnrealIRCd|Unknown|Critical|Known backdoor version on Metasploitable|
|8009|AJP|Apache JServ|Medium|Tomcat connector|
|8180|Tomcat|5.5|High|Administrative interface may be exposed|

---

## Enumeration Priority

1. FTP
2. HTTP
3. SMB
4. DistCC
5. UnrealIRCd
6. Tomcat
7. NFS
8. MySQL
9. PostgreSQL
10. SSH