# Privilege Escalation – DistCC

## Objective

Perform post-exploitation enumeration after obtaining an initial shell through the DistCC Remote Code Execution vulnerability. The objective is to identify potential privilege escalation vectors from the low-privileged `daemon` account.

---

# Initial Shell

The DistCC exploit successfully provided a reverse shell running as the `daemon` user.

Commands:

```bash
whoami
id
hostname
uname -a
pwd
```

Result:

```
User      : daemon
UID/GID   : uid=1(daemon) gid=1(daemon)
Hostname  : metasploitable
Directory : /tmp
Kernel    : Linux 2.6.24-16-server
```

Screenshot:

```
01_initial_shell.png
```

---

# User Groups

Command:

```bash
groups
```

Result:

```
daemon
```

Observation:

- The compromised account belongs only to the `daemon` group.
- No administrative or privileged group memberships were identified.

Screenshot:

```
02_groups.png
```

---

# Sudo Privileges

Command:

```bash
sudo -l
```

Result:

```
[sudo] password for daemon:
Sorry, try again.
```

Observation:

- The daemon account cannot authenticate to sudo.
- No privilege escalation path through sudo was identified.

Screenshot:

```
03_sudo_check.png
```

---

# SUID Enumeration

Command:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Result:

Important SUID binaries discovered:

```
/bin/su
/bin/mount
/bin/umount
/bin/ping
/bin/ping6
/bin/fusermount
/sbin/mount.nfs
```

Observation:

- Multiple SUID binaries exist.
- No immediately exploitable custom SUID binaries were identified during this assessment.

Screenshot:

```
04_suid_binaries.png
```

---

# SGID Enumeration

Command:

```bash
find / -perm -2000 -type f 2>/dev/null
```

Result:

Interesting SGID binaries include:

```
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/crontab
/usr/bin/nmap
/usr/bin/screen
/usr/lib/apache2/suexec
```

Observation:

- Several SGID binaries are present.
- Additional manual analysis would be required to determine exploitability.

Screenshot:

```
05_sgid_binaries.png
```

---

# Scheduled Tasks

Command:

```bash
cat /etc/crontab
```

Observation:

- System cron jobs execute as the root user.
- No writable cron entries or obvious privilege escalation opportunities were identified.

Screenshot:

```
06_crontab.png
```

---

# Running Processes

Command:

```bash
ps aux
```

Observation:

The process list confirmed several high-privilege services running as root, including:

- SSH
- Apache
- Samba
- PostgreSQL
- MySQL
- UnrealIRCd
- Java RMI Registry
- Tomcat
- VNC Server
- NFS
- Cron

The DistCC service was confirmed running as:

```
distccd --daemon --user daemon
```

Screenshot:

```
07_running_processes.png
```

---

# Writable Directories

Command:

```bash
find / -writable -type d 2>/dev/null
```

Observation:

Writable directories were enumerated for potential abuse during privilege escalation. No immediately exploitable writable system directories were identified during this assessment.

Screenshot:

```
08_writable_directories.png
```

---

# Security Assessment

## Findings

| Check | Result |
|--------|--------|
| Initial Shell | ✅ daemon |
| Sudo Access | ❌ Not Available |
| Group Membership | daemon only |
| SUID Enumeration | ✅ Completed |
| SGID Enumeration | ✅ Completed |
| Cron Review | ✅ Completed |
| Process Enumeration | ✅ Completed |
| Writable Directory Enumeration | ✅ Completed |

---

# Commands Executed

```bash
whoami

id

hostname

uname -a

pwd

groups

sudo -l

find / -perm -4000 -type f 2>/dev/null

find / -perm -2000 -type f 2>/dev/null

cat /etc/crontab

ps aux

find / -writable -type d 2>/dev/null
```

---

# Conclusion

Following successful exploitation of the DistCC service, a reverse shell was obtained with the privileges of the `daemon` user. Post-exploitation enumeration was conducted to identify privilege escalation opportunities by reviewing sudo permissions, group memberships, SUID/SGID binaries, scheduled tasks, running processes, and writable directories.

No direct privilege escalation vector was identified from the `daemon` account during this assessment. However, the enumeration successfully established the security posture of the compromised system and identified several privileged services that may warrant further investigation during advanced post-exploitation.