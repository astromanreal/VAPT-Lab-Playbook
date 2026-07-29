# DNS Enumeration (53)

## Objective

Identify the DNS service, determine its version, test for common DNS misconfigurations such as zone transfer and recursion, enumerate available DNS records, and assess potential vulnerabilities.

---

# Service Identification

## Version Detection

Command:

```bash
nmap -sV -p53 192.168.56.104
```

Result:

```
53/tcp open  domain  ISC BIND 9.4.2
```

**Finding**

- Service: DNS
- Software: ISC BIND
- Version: 9.4.2
- Status: Running

Screenshot:

```
01_dns_version.png
```

---

# Service Enumeration

## DNS Service Discovery

Command:

```bash
nmap --script dns-service-discovery -p53 192.168.56.104
```

Result:

The NSE script completed successfully but did not identify any additional DNS services or configuration details.

Screenshot:

```
02_dns_service_discovery.png
```

---

## Zone Transfer Test (AXFR)

Command:

```bash
dig axfr @192.168.56.104 metasploitable.localdomain
```

Alternative:

```bash
host -l metasploitable.localdomain 192.168.56.104
```

Result:

```
Transfer failed.
```

```
Host metasploitable.localdomain not found: NOTAUTH
Transfer failed.
```

**Finding**

Zone transfer is not permitted. The DNS server correctly denies unauthorized AXFR requests.

Screenshot:

```
03_dns_zone_transfer.png
```

---

## Reverse DNS Lookup

Command:

```bash
dig -x 192.168.56.104 @192.168.56.104
```

Result:

```
status: SERVFAIL
```

**Finding**

The DNS server did not return a PTR record for the target IP address.

Screenshot:

```
04_dns_reverse_lookup.png
```

---

## DNS Record Enumeration

Command:

```bash
dig @192.168.56.104 metasploitable.localdomain ANY
```

Alternative:

```bash
nslookup metasploitable.localdomain 192.168.56.104
```

Result:

```
status: SERVFAIL
```

```
server can't find metasploitable.localdomain: SERVFAIL
```

**Finding**

No DNS records were returned for the queried domain.

Screenshot:

```
05_dns_records.png
```

---

## Recursion Test

Command:

```bash
dig google.com @192.168.56.104
```

Result:

```
status: SERVFAIL
```

**Finding**

The DNS server did not resolve external domains during testing. Open recursion was not observed.

Screenshot:

```
06_dns_recursion.png
```

---

## Public Exploit Research

Command:

```bash
searchsploit bind 9.4
```

Result:

Several historical vulnerabilities exist for older ISC BIND 9.4.x releases, including denial-of-service and cache poisoning issues. No unauthenticated remote code execution exploit applicable to this configuration was identified during the assessment.

Screenshot:

```
07_dns_searchsploit.png
```

---

# Security Assessment

## Identified Risks

- Outdated ISC BIND 9.4.2 version
- Historical vulnerabilities exist for this software branch
- Zone transfer is properly restricted
- No evidence of open recursive DNS resolution
- DNS record enumeration was unsuccessful

---

# Exploitation Status

| Test | Result |
|-------|--------|
| Version Enumeration | ✅ Successful |
| Service Detection | ✅ Successful |
| Zone Transfer | ❌ Denied |
| DNS Record Enumeration | ❌ Failed (SERVFAIL) |
| Reverse Lookup | ❌ Failed |
| Open Recursion | ❌ Not Observed |
| SearchSploit Review | ✅ Completed |
| Remote Code Execution | ❌ Not Identified |

---

# Commands Executed

```bash
nmap -sV -p53 192.168.56.104

nmap --script dns-service-discovery -p53 192.168.56.104

dig axfr @192.168.56.104 metasploitable.localdomain

host -l metasploitable.localdomain 192.168.56.104

dig -x 192.168.56.104 @192.168.56.104

dig @192.168.56.104 metasploitable.localdomain ANY

nslookup metasploitable.localdomain 192.168.56.104

dig google.com @192.168.56.104

searchsploit bind 9.4
```

---

# Conclusion

The DNS service was identified as **ISC BIND 9.4.2** running on TCP port 53. Enumeration confirmed that the service was reachable and disclosed its version. Security testing showed that unauthorized zone transfers were correctly denied, reverse DNS lookups and DNS record enumeration returned **SERVFAIL**, and recursive resolution of external domains was not observed. Although the server runs an outdated BIND version with publicly documented historical vulnerabilities, no practical unauthenticated exploitation path was identified during this assessment. The service should be documented as an outdated DNS server with restricted enumeration capabilities.