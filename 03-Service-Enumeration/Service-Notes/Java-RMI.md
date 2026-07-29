# Java RMI (Port 1099)

## Objective

Enumerate the exposed Java RMI Registry service, identify potential misconfigurations, and check for known Remote Code Execution (RCE) vulnerabilities.

---

## Service Detection

Scan the service version.

```bash
nmap -sV -p1099 192.168.56.104
```

Output

```
1099/tcp open  java-rmi GNU Classpath grmiregistry
```

**Finding**

- Port **1099** is open.
- Service: **GNU Classpath grmiregistry**
- Java RMI Registry is accessible over the network.

**Screenshot**

**Image:** `java-rmi-service-scan.png`

---

## RMI Registry Enumeration

Enumerate objects registered inside the RMI Registry.

```bash
nmap --script rmi-dumpregistry -p1099 192.168.56.104
```

Output

```
1099/tcp open rmiregistry
```

No registered remote objects were returned.

**Finding**

The registry is accessible but does not expose any registered remote objects for enumeration.

**Screenshot**

**Image:** `java-rmi-registry-enum.png`

---

## Vulnerability Detection

Run the RMI vulnerability detection scripts.

```bash
nmap --script=rmi-dumpregistry,rmi-vuln-classloader -p1099 192.168.56.104
```

Output

```
VULNERABLE:
RMI registry default configuration remote code execution vulnerability

State: VULNERABLE

Default configuration of RMI registry allows loading classes from remote URLs which can lead to remote code execution.
```

**Finding**

The NSE script reports that the Java RMI Registry is configured with the default insecure class loader configuration, which may allow loading Java classes from remote locations and potentially lead to **Remote Code Execution (RCE)**.

**References**

- Metasploit Java RMI Server module
- CVE/Default RMI Class Loader Misconfiguration

**Screenshot**

**Image:** `java-rmi-vulnerability.png`

---

## Exploit Research

Search Exploit-DB for known Java RMI exploits.

```bash
searchsploit java rmi
```

Relevant results

```
Java RMI - Server Insecure Default Configuration
Java - RMIConnectionImpl Deserialization
```

Search for registry-specific exploits.

```bash
searchsploit java registry
```

Output

```
Oracle WebLogic RMI Registry
```

**Finding**

Exploit-DB contains multiple Java RMI related exploits, including insecure default configuration and deserialization vulnerabilities.

**Screenshot**

**Image:** `java-rmi-searchsploit.png`

---

## Metasploit Enumeration

Start Metasploit.

```bash
msfconsole -q
```

Search for Java RMI modules.

```bash
search java_rmi
```

Use the registry enumeration module.

```bash
use auxiliary/gather/java_rmi_registry
```

Configure the target.

```bash
set RHOSTS 192.168.56.104
```

Execute the module.

```bash
run
```

Output

```
Sending RMI Header...
Listing names in the Registry...
Names not found in the Registry
```

**Finding**

The registry accepted the connection successfully, but no registered objects were available for enumeration. Therefore, no direct exploitation path was identified through this module.

**Screenshot**

**Image:** `java-rmi-metasploit.png`

---

# Summary

| Item | Result |
|------|--------|
| Port | 1099 |
| Service | GNU Classpath grmiregistry |
| Registry Accessible | Yes |
| Registered Objects | None Found |
| Vulnerability Detected | Default RMI Class Loader Configuration |
| Potential Impact | Remote Code Execution (RCE) |
| Successful Exploitation | No |

---

## Conclusion

The Java RMI Registry on port **1099** was successfully identified and enumerated. Although the registry did not expose any registered remote objects, the `rmi-vuln-classloader` NSE script reported a default insecure class loader configuration that may allow remote class loading and potentially lead to Remote Code Execution under certain conditions. No exploitable RMI objects were present during testing, so exploitation was not achieved.