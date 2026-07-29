# NFS Enumeration

## Objective

Enumerate Network File System (NFS) exports to identify shared directories, insecure configurations, and accessible files that could assist in privilege escalation or credential discovery.

---

## Service Information

| Item | Value |
|------|-------|
| Service | NFS |
| Port | 2049/TCP |
| Target | 192.168.56.104 |

---

# Why Enumerate NFS?

NFS allows remote systems to mount shared directories over the network. Misconfigured NFS exports can expose sensitive files, user home directories, SSH keys, configuration files, and other information that may lead to privilege escalation or unauthorized access.

---

# Step 1 – Identify Exported Shares

### Command

```bash
showmount -e 192.168.56.104
```

### Result

```text
Export list for 192.168.56.104:
/ *
```

### Observation

The target exports its root filesystem (`/`) to all hosts (`*`), indicating an insecure NFS configuration.

---

# Step 2 – Enumerate NFS Using Nmap

### Command

```bash
nmap --script nfs-showmount,nfs-ls,nfs-statfs -p2049 192.168.56.104
```

### Result

The scan confirmed that the NFS service is running on TCP port 2049.

No additional information was returned by the NSE scripts.

---

# Step 3 – Mount the Exported Share

### Create Mount Directory

```bash
mkdir ~/nfs_mount
```

### Mount the Share

```bash
sudo mount -t nfs 192.168.56.104:/home ~/nfs_mount
```

---

# Step 4 – Enumerate Accessible Files

### Command

```bash
find ~/nfs_mount -maxdepth 2
```

### Findings

Accessible user directories included:

- ftp
- msfadmin
- service
- user

Interesting files discovered:

- `.ssh/`
- `.bash_history`
- `.mysql_history`
- `.rhosts`
- `.distcc/`
- `.profile`
- `.bashrc`
- `.bash_logout`
- `vulnerable/`

---

# Security Analysis

The NFS server allows remote mounting of exported directories without authentication.

Access to user home directories exposes sensitive files that may reveal:

- SSH configuration and keys
- Command history
- Database history
- Trust relationship files (.rhosts)
- User-specific configuration files

Such information can significantly assist attackers during post-exploitation or privilege escalation.

---

# Risk Assessment

| Risk | Level |
|------|-------|
| Information Disclosure | High |
| Unauthorized File Access | High |
| Privilege Escalation Potential | High |

---

# Evidence Collected

- Root filesystem exported to all hosts.
- `/home` successfully mounted.
- Multiple user home directories accessible.
- Sensitive configuration and history files exposed.

---

# Screenshots

Store screenshots in:

```
screenshots/nfs/
```

Suggested screenshots:

- `showmount.png`
- `nmap_nfs_scripts.png`
- `mounted_share.png`
- `find_output.png`

---

# Conclusion

The NFS service is insecurely configured and permits remote mounting of exported directories. Sensitive user files are accessible without authentication, increasing the risk of credential exposure and privilege escalation. Although no direct code execution was achieved during this phase, the exposed data provides valuable information for later stages of the penetration test.

---

# Next Step

Proceed with enumeration of the DistCC service running on TCP port 3632 to assess potential remote code execution vulnerabilities.