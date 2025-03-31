# Network Port Scanning with Nmap

## Overview
This project demonstrates **network reconnaissance** using **Nmap** (Network Mapper) to identify open ports, services, and operating systems on target systems across different networks. The goal was to discover potential **attack surfaces** (open ports/services) that could be exploited by malicious actors and recommend mitigation strategies.

---

## Key Concepts
- **Port Scanning**: Identifying open ports on networked systems.
- **Attack Surface**: The collection of exposed vulnerabilities (e.g., unnecessary open ports).
- **Threat Vector**: A pathway (e.g., an open port) that could be used to compromise a system.
- **Credentialed vs. Non-Credentialed Scans**: Nmap can scan with or without authentication for deeper insights.

---

## Tools Used
- **Nmap**: Primary tool for port scanning and service detection.
- **Kali Linux**: Hosting the Nmap scanner.
- **Target Systems**:
  - `230.0.113.1` (External network)
  - `10.1.16.2` (Internal VLAN)
- **Grep**: Filtering scan results for critical information.

---

## Steps Performed
1. **Initial Scan (External Network)**:
   - Scanned `230.0.113.1` using:
     ```bash
     nmap 230.0.113.1 -F -sS -sV -O -Pn -oN border-scan.nmap
     ```
   - **Flags Used**:
     - `-F`: Fast mode - Scans only the 100 most common ports.
     - `-sS`: Stealth SYN scan (avoids full TCP handshake).
     - `-sV`: Service version detection.
     - `-O`: OS detection.
     - `-Pn`: Skip host discovery (assume host is up).
     - `-oN`: Output to file.
   - **Findings**: Discovered **Port 25 (SMTP)** open on a FreeBSD system.

2. **Internal Network Scan (VLAN Clients)**:
   - Switched to `eth2` (VLAN) and scanned `10.1.16.2`:
     ```bash
     nmap 10.1.16.2 -F -sS -sV -O -Pn -oN server-scan.nmap
     ```
   - **Findings**:
     - **Port 80 (HTTP)**: Unencrypted web service.
     - **Port 445 (Microsoft DS)**: SMB service (potential EternalBlue vulnerability).
     - **OS**: Microsoft Windows Server.

3. **Analysis**:
   - Used `grep` to filter results:
     ```bash
     grep open server-scan.nmap
     grep OS server-scan.nmap
     ```
   - Prioritized vulnerabilities based on service criticality (e.g., SMB vs. HTTP).

---

## Key Findings
| Target          | Open Ports | Services       | OS          | Risk Level |
|-----------------|------------|----------------|-------------|------------|
| `230.0.113.1`    | 25         | SMTP           | FreeBSD     | Medium     |
| `10.1.16.2`   | 80, 445    | HTTP, SMB      | Windows Server | High       |

---

## Mitigation Strategies
1. **Close Unnecessary Ports**:
   - Disable SMB (Port 445) if not needed to prevent exploits like EternalBlue, or enable SMB encryption and apply patches.
2. **Encrypt Services**:
   - Use HTTPS (Port 443) instead of HTTP (Port 80).
   - For SMTP (Port 25), Implement SPF/DKIM/DMARC to prevent email spoofing.
   - Enable SMB encryption for secure file sharing.
3. **Patch Management**:
   - Regularly update services (e.g., SMTP, SMB) to fix known vulnerabilities.
4. **Network Segmentation**:
   - Restrict access to critical ports using firewalls/VLANs.

---

## Screenshots


---

## How to Replicate
1. Install Nmap on Kali Linux:
   ```bash
   sudo apt install nmap
2. Run a basic scan:
   nmap <TARGET_IP> -F -sS -sV -O -Pn -oN scan-results.nmap
3. Analyze results:
   grep open scan-results.nmap


## Lessons Learned:

Fast Scans (-F): Efficient for initial reconnaissance but may miss less common ports.

Stealth Scans (-sS): Useful for avoiding detection but may miss some services.

Service Identification (-sV): Critical for understanding what’s running on open ports.

Attack Surface Reduction: Closing/encrypting ports is more effective than relying on obscurity.