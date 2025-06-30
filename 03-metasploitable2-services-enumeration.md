title: "Metasploitable2 Service Enumeration Report"

author: "Abdulrahman Said"

date: "June 30, 2025"

tools: [Kali Linux, OpenSSH, cURL, Netstat]
---

# 🔐 Metasploitable2 – Service Enumeration and Preliminary Analysis

## 1. Executive Summary
This report documents the initial reconnaissance and service enumeration of the Metasploitable2 virtual machine using Kali Linux. The primary goal was to identify exposed network services and assess their security posture. Multiple high-risk services were found running with default configurations—many of which are known to be vulnerable—making this VM ideal for safe offensive security practice.

---

## 2. Objective
- Enumerate open ports and services running on Metasploitable2.
- Validate key services and their responses.
- Identify vulnerabilities or misconfigurations for future exploitation.

---

## 3. Environment Setup

| Component        | Description                  |
|------------------|------------------------------|
| Attacker Machine | Kali Linux (192.168.21.X)    |
| Target Machine   | Metasploitable2 (192.168.21.128) |
| Network Type     | NAT / Host-only Virtual Network |
| Credentials      | `msfadmin / msfadmin`        |

---

## 4. Methodology & Findings

### 4.1 SSH Access
- **Command Used:**
  ```bash
  ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa msfadmin@192.168.21.128
  ```
- **Result:** Successful login with default credentials.
- **Privilege Escalation:** Gained root access using:
  ```bash
  sudo -i
  ```

---

### 4.2 Service Enumeration
- **Command Used:**
  ```bash
  netstat -antp
  ```
- **Purpose:** Identify active TCP/UDP ports and services.

| Port | Protocol | Service         | Description                       |
|------|----------|------------------|-----------------------------------|
| 21   | TCP      | FTP (xinetd)     | Anonymous login may be possible   |
| 22   | TCP6     | SSH              | Accessible via default creds      |
| 23   | TCP      | Telnet           | Insecure, enabled                 |
| 25   | TCP      | SMTP             | Possible mail relay vector        |
| 80   | TCP      | Apache2          | Hosts DVWA and others             |
| 3306 | TCP      | MySQL            | Check for weak credentials        |
| 5432 | TCP      | PostgreSQL       | PostgreSQL service active         |
| 8180 | TCP      | Apache Tomcat 5.5| Admin interface exposed           |
| 8787 | TCP      | Ruby DRb         | Service running, returns error    |

---

### 4.3 Web Application Validation

#### ✅ Apache Web (Port 80)
- **Tool Used:** `curl http://localhost:80`
- **Result:** Apache welcome page showing links to:
  - DVWA
  - phpMyAdmin
  - Mutillidae
  - WebDAV
  - TWiki

#### ✅ Tomcat (Port 8180)
- **Tool Used:** `curl http://localhost:8180`
- **Result:** Apache Tomcat 5.5 interface available, including manager and admin links.

#### ⚠️ Ruby DRb (Port 8787)
- **Tool Used:** `curl http://localhost:8787`
- **Result:** DRbConnError: “too large packet”
- **Insight:** Service is active but expects serialized Ruby objects. This behavior hints at future Metasploit/Ruby exploitation potential.

---

## 5. Key Findings

- ✅ **Full system access** obtained via SSH with default credentials (`msfadmin/msfadmin`).
- ⚠️ **Multiple exploitable services** exposed (e.g., Tomcat, phpMyAdmin, FTP, WebDAV).
- 💡 **DRb service is active** and responds to invalid input—this may serve as an advanced exploitation target.
- 🔓 **Web apps (DVWA, Mutillidae)** available for SQLi, XSS, and RCE testing.

---

## 6. Lessons Learned

- **Default credentials remain a severe risk.**
- **Enumeration is the foundation** for identifying attack surfaces.
- **Error responses** (like the DRb packet issue) often reveal valuable clues for exploitation.

---

## 7. Next Steps

| Action | Description |
|--------|-------------|
| 🔐 phpMyAdmin | Attempt login with common credentials |
| 🧪 WebDAV | Try uploading reverse shell (PHP) |
| 🧬 DVWA / Mutillidae | Test for SQLi, XSS, LFI, and command injection |
| 🧠 Tomcat Manager | Check for remote WAR deployment via 8180 |
| 🧰 DRb | Research and test Metasploit DRb modules or Ruby scripts |

---

_This document is part of a cybersecurity home lab series by Abdulrahman Said._

