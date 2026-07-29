# Host Discovery

## Objective

Verify that the target is reachable before beginning active scanning.

---

## Lab Information

| Item | Value |
|------|-------|
| Attacker Machine | Kali Linux |
| Target Machine | Metasploitable 2 |
| Attacker IP | 192.168.56.103 |
| Target IP | 192.168.56.104 |

---

## Step 1 – Identify Attacker IP

### Command

```bash
ip a
```

### Result

Attacker IP:

```text
192.168.56.103
```

---

## Step 2 – Verify Target Availability

### Command

```bash
ping -c 4 192.168.56.104
```

### Result

The target responded successfully to ICMP echo requests, confirming that it is reachable.

---

## Observations

- Target is online.
- Communication between attacker and target is successful.
- Network connectivity is verified.

---

## Analysis

The successful ICMP responses indicate that the target host is reachable from the attack machine. Network connectivity has been verified, allowing active reconnaissance and port scanning to proceed.

At this stage, no assumptions are made regarding the services running on the target. The next phase is to identify exposed ports and enumerate the services listening on those ports.

## Next Step

Perform full TCP port scanning using Nmap.