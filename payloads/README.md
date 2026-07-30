# Payloads Used

This directory documents the Metasploit payloads used during exploitation.

| Service | Exploit Module | Payload Used |
|----------|----------------|--------------|
| DistCC | exploit/unix/misc/distcc_exec | cmd/unix/reverse_perl |
| UnrealIRCd | exploit/unix/irc/unreal_ircd_3281_backdoor | cmd/linux/http/x86/meterpreter/reverse_tcp |
| Samba | exploit/multi/samba/usermap_script | cmd/unix/reverse_netcat |
| Java RMI | exploit/multi/misc/java_rmi_server | java/meterpreter/reverse_tcp |

---

## DistCC

**Exploit**

```
exploit/unix/misc/distcc_exec
```

**Payload**

```
cmd/unix/reverse_perl
```

Result:

- Reverse shell
- User: daemon

---

## UnrealIRCd

**Exploit**

```
exploit/unix/irc/unreal_ircd_3281_backdoor
```

**Payload**

```
cmd/linux/http/x86/meterpreter/reverse_tcp
```

Result:

- Meterpreter session
- Root shell after `shell`

---

## Samba

**Exploit**

```
exploit/multi/samba/usermap_script
```

**Payload**

```
cmd/unix/reverse_netcat
```

Result:

- Root shell

---

## Java RMI

**Exploit**

```
exploit/multi/misc/java_rmi_server
```

**Payload**

```
java/meterpreter/reverse_tcp
```

Result:

- Meterpreter session
- Root shell after `shell`