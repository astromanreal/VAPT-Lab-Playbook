# Reconnaissance

## Overview

Reconnaissance is the first phase of a penetration test. The objective is to collect as much information as possible about the target without attempting exploitation.

The information gathered during this phase helps identify potential attack surfaces and guides the scanning and enumeration process.

---

## Objectives

- Identify the target host
- Gather IP address information
- Discover open services
- Collect DNS information
- Identify technologies in use
- Perform passive and active information gathering
- Prepare for port scanning and service enumeration

---

## Activities Performed

- Target Identification
- Host Discovery
- DNS Enumeration
- Banner Grabbing
- Technology Identification
- Basic Network Information Gathering

---

## Tools Used

- Ping
- Nmap
- Netcat (nc)
- Dig
- Host
- Nslookup
- WhatWeb
- Curl

---

## Directory Structure

```
Reconnaissance/
├── README.md
├── host-discovery.md
├── dns-recon.md
├── banner-grabbing.md
├── technology-identification.md
├── screenshots/
└── reports/
```

---

## Outcome

- Identified the target host
- Collected basic network information
- Gathered DNS records
- Identified exposed technologies
- Prepared the target for Nmap scanning
