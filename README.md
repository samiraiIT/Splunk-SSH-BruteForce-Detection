# 🛡️ SIEM Lab: SSH Brute-Force Detection & Investigation with Splunk

## 📌 Project Overview
This project demonstrates a practical Security Operations Center (SOC) Tier 1 workflow by deploying **Splunk Enterprise** on an **Ubuntu Linux** environment, ingesting system authentication logs, simulating an **SSH Brute-Force attack**, and investigating the security incident using Splunk Search Processing Language (SPL).

## 🛠️ Environment & Tools
* **OS:** Ubuntu Linux (VirtualBox)
* **SIEM Solution:** Splunk Enterprise
* **Target Service:** OpenSSH (`sshd`)
* **Log Source:** `/var/log/auth.log`

---

## ⚙️ Lab Implementation Steps

### 1. Log Ingestion & Configuration
Configured Splunk Enterprise to monitor and index local Linux authentication logs (`/var/log/auth.log`) under a custom source type (`linux_auth`) to capture real-time login attempts, privilege escalations, and SSH session events.

### 2. Attack Simulation (MITRE ATT&CK: T1110 - Brute Force)
Simulated an unauthorized access attempt against the local SSH service using invalid credentials and non-existent usernames (`hacker`) to generate authentication failure events:
```bash
ssh hacker@localhost
```
### 3. Incident Detection & SPL Investigation
Queried the ingested logs in Splunk to isolate failed authentication attempts and identify the source of the attack:

Detection Query (SPL):
``` Splunk SPL
source="/var/log/auth.log" ("Failed password" OR "Invalid user")
```
Statistical Aggregation Query (SPL):
Splunk SPL
```
source="/var/log/auth.log" ("Failed password" OR "Invalid user") | stats count by host, source 
```
### 4. Key Investigation Findings
Attack Vector: SSH Password Guessing / Brute-Force

Target Host: ubuntu

Source IP: 127.0.0.1

Targeted Accounts: Invalid/non-existent user (hacker)

Total Failed Events: 10+ consecutive failed authentication attempts within a short timeframe.



