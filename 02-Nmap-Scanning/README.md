# Nmap Scanning

## Overview

This phase focuses on discovering live hosts, identifying open ports, detecting running services, and gathering version information using Nmap.

The results from this phase provided the foundation for the Service Enumeration and Exploitation phases.

---

## Objectives

- Discover live hosts
- Identify open TCP ports
- Detect running services
- Identify service versions
- Perform OS detection
- Execute relevant NSE scripts
- Collect information for further enumeration

---

## Scans Performed

- Host Discovery
- TCP Port Scan
- Service Version Detection
- Default NSE Scripts
- OS Detection
- Targeted Port Scans
- Vulnerability Detection Scripts

---

## Tools Used

- Nmap
- Nmap Scripting Engine (NSE)

---

## Directory Structure

```
Nmap-Scanning/
├── README.md
├── host-discovery.md
├── tcp-scan.md
├── service-version.md
├── os-detection.md
├── nse-scripts.md
├── screenshots/
└── reports/
```

---

## Outcome

- Identified live target host
- Discovered exposed TCP services
- Detected service versions
- Gathered operating system information
- Established the attack surface for further enumeration