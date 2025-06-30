# 🔐 Cybersecurity Lab Report  
**Subject**: Metasploitable2 – Service Enumeration and Preliminary Analysis  
**Author**: Abdulrahman Said  
**Date**: June 30, 2025  
**Tools Used**: Kali Linux, OpenSSH, cURL, Netstat

---

## 1. Executive Summary  
This report documents the initial reconnaissance and service enumeration of the Metasploitable2 virtual machine using Kali Linux. The primary objective was to identify active services and assess their potential for exploitation. The enumeration process revealed multiple high-risk services running on default configurations, several of which are known to contain vulnerabilities. This environment will serve as a foundation for hands-on offensive security practice.

---

## 2. Objective  
To enumerate exposed services on Metasploitable2, validate their functionality, and identify potential targets for exploitation in subsequent stages of testing.

---

## 3. Environment Setup  
- **Attacker Machine**: Kali Linux (192.168.21.X)  
- **Target Machine**: Metasploitable2 (192.168.21.128)  
- **Connection**: Internal NAT/Host-Only Virtual Network  
- **Authentication Credentials**:  
  - Username: `msfadmin`  
  - Password: `msfadmin`

---

## 4. Procedures

### 4.1 SSH Access  
**Command**:
```bash
ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa msfadmin@192.168.21.128
```
**Result**: Successfully logged in using default credentials.  
**Privilege Escalation**: Gained root access using:
```bash
sudo -i
```

---

### 4.2 Service Enumeration  
**Command**:
```bash
netstat -antp
```
**Purpose**: Identify open ports and associated services.

| Port | Protocol | Service            | Notes                                 |
|------|----------|--------------------|----------------------------------------|
| 21   | TCP      | FTP (xinetd)       | Likely allows anonymous login          |
| 22   | TCP6     | SSH                | Open and authenticated                 |
| 23   | TCP      | Telnet             | Insecure, should be tested             |
| 25   | TCP      | SMTP               | Possible mail injection vector         |
| 80   | TCP      | Apache2            | Hosts DVWA and other test apps         |
| 3306 | TCP      | MySQL              | Default creds possible                 |
| 5432 | TCP      | PostgreSQL         | May be exploitable                     |
| 8180 | TCP      | Apache Tomcat 5.5  | Admin interfaces exposed               |
| 8787 | TCP      | Ruby DRb           | Responded with malformed packet error  |

---

### 4.3 Web Application Service Validation  
**Tool Used**: `curl`  
**Results**:

| URL                  | Response Summary                                   |
|----------------------|----------------------------------------------------|
| `http://localhost:80`    | Apache default page, DVWA and others linked      |
| `http://localhost:8180`  | Apache Tomcat 5.5 homepage                      |
| `http://localhost:8787`  | DRbConnError – “too large packet”               |

**Note about DRb (Port 8787)**:  
The service is live and responsive, but returned a malformed Ruby object stream error. This suggests potential for future interaction or exploitation using Ruby-based tools or Metasploit modules.

---

## 5. Key Findings

- ✅ **Full system compromise via SSH** using default credentials.  
- ✅ **Multiple exposed services**, many on default ports and known to be vulnerable.  
- ✅ **DRb Service**:  
  - Active and responsive.  
  - Error message hints at improper input structure — exploitable with crafted payloads.

---

## 6. Lessons Learned

- Default credentials are a critical security flaw.  
- Service enumeration is crucial to find potential attack vectors.  
- Even error messages (like DRb) can expose valuable information.

---

## 7. Next Steps

1. Attempt login to `/phpMyAdmin` with common usernames: `admin`, `root`, `msfadmin`, `toor`.  
2. Test `/dav/` WebDAV for arbitrary file upload (e.g., reverse shell).  
3. Explore `/mutillidae/`, `/dvwa/`, `/twiki/` for SQLi, XSS, LFI, and RCE vulnerabilities.  
4. Investigate Apache Tomcat Manager/Admin interfaces on port 8180.  
5. Craft payloads to interact with DRb (port 8787) using Metasploit or custom Ruby scripts.