# 🔐 Cybersecurity Lab Report: Scanning & Exploiting Metasploitable2 Using Kali Linux

**Prepared by:** Abdul Rahman Said  
**Date:** June 25, 2025

---

## 🧰 Lab Overview

This lab demonstrates foundational reconnaissance and exploitation techniques using **Kali Linux** against a vulnerable machine (**Metasploitable2**) within a **host-only network** using VMware Workstation.

---

## 🖥️ Environment Configuration

| Component         | Description                          |
|------------------|--------------------------------------|
| Host OS          | Windows                              |
| Virtual Machines | Kali Linux, Metasploitable2          |
| Network Type     | Host-only (VMnet1 - 192.168.21.0/24) |
| Tools Used       | Nmap, Telnet, FTP                    |

---

## ✅ Step-by-Step Execution

### 🔹 Step 1: Configure Network on VMware
- Used **Virtual Network Editor**.
- Set network to **Host-only (VMnet1)** with subnet: `192.168.21.0/24`.
- Enabled **DHCP**.

### 🔹 Step 2: Verify IP Addresses
- **Kali Linux:** `192.168.21.129`
- **Metasploitable2:** `192.168.21.128`

Command:
```bash
ip a
```

---

### 🔹 Step 3: Scan Metasploitable2 with Nmap

Command:
```bash
nmap 192.168.21.128
```

Result: Detected 23 open ports, including:

- 21/tcp (FTP)  
- 22/tcp (SSH)  
- 23/tcp (Telnet)  
- 25/tcp (SMTP)  
- 53/tcp (DNS)  
- 80/tcp (HTTP)  
- 139, 445/tcp (NetBIOS/SMB)  
- 3306/tcp (MySQL)  
- 5900/tcp (VNC)  
- And others...

---

### 🔹 Step 4: Test FTP Access (Anonymous Login)

Command:
```bash
ftp 192.168.21.128
```

- Entered username: `anonymous`
- ✅ Login successful
- Navigated directories, attempted file downloads (some failed due to permission issues)

---

### 🔹 Step 5: Telnet Login Attempt

Command:
```bash
telnet 192.168.21.128
```

- Banner displayed: `metasploitable2`
- Used default credentials:
  - Username: `msfadmin`
  - Password: `msfadmin`
- ✅ Login successful

---

### 🔹 Step 6: Post-Login Enumeration

Commands used:
```bash
whoami
hostname
ls -la
```

- User: `msfadmin`
- Discovered interesting files:
  - `.ssh`, `.profile`, `.rhosts`
  - `sudo_as_admin_successful`

Indicates possible privilege escalation vectors.

---

### 🔹 Step 7: Exit Remote Session

Command:
```bash
exit
```

Returned to Kali Linux prompt.

---

## 🧾 Conclusion

✅ Successfully:

- Configured a safe, isolated penetration testing lab  
- Scanned a vulnerable target (Metasploitable2) using Nmap  
- Accessed FTP service via anonymous login  
- Logged into Telnet using default credentials  
- Enumerated user and file system for potential privilege escalation

---

## 📌 Disclaimer

This lab was conducted in a **fully isolated and ethical environment** for educational purposes only.  
**Never attempt these actions on unauthorized systems.**
