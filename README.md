# NETWORKWALKS-B083-WK4-PENETRATION-TESTING-PROJECT
Black-box penetration testing against Mediroza General Hospital web application.
# Penetration Testing Report – Mediroza General Hospital

This repository contains the findings, methodology, and remediation recommendations from an authorized black-box penetration testing assessment performed on the **Mediroza General Hospital** web application.

---

## 📌 Executive Summary

An authorized black-box penetration testing assessment was conducted against the Mediroza General Hospital web application in a controlled environment for educational and security-testing purposes. The assessment covered multiple stages including reconnaissance, network/application scanning, initial access testing, data extraction, password recovery, and sensitive information discovery.

Key security findings include:

* **SQL Injection** vulnerability enabling authentication bypass in the patient portal.


* **Weak PDF Password** allowing unauthorized access to protected documents.


* **Exposed Legacy Database Resource** (`sql-db-2019`) found in an accessible directory (`/old/`).


* **Information Disclosure** via the `robots.txt` file revealing internal application paths.



---

## 📋 Scope & Methodology

### Project Overview

* **Client / Target:** Mediroza General Hospital


* **Target URL:** `[https://medirozahospital.com](https://medirozahospital.com)`

* **Assessment Type:** Black-Box Penetration Testing


* **Duration:** 5 Days


* **Program:** Cybersecurity Internship - Networkwalks (Batch B083 – Week 4)


* **Mentor:** Waqas Karim, CCIE[cite: 3]
* **Performed By:** [Rohith KR](https://www.google.com/search?q=https://linkedin.com/in/rohith-k-r-55236a30b)

* **Status:** Final – All Milestones Completed



### Testing Milestones

| Milestone | Activity Description | Details |
| --- | --- | --- |
| **M1 – Initial Access** | Website Testing | Reconnaissance & security testing on target website; identified SQL Injection in login functionality[cite: 3]. |
| **M2 – Data Extraction** | File Password Recovery | Extracted password hashes from protected PDF files and performed offline cracking[cite: 3]. |
| **M3 – Attack / Cracking** | Sensitive Data Discovery | Investigated exposed resources to identify legacy database-related files and sensitive information[cite: 3]. |

---

## 🛠️ Tools Used

| Tool | Purpose |
| --- | --- |
| **Kali Linux** | Primary OS used for reconnaissance and penetration testing[cite: 3]. |
| **WHOIS** | Collected domain registration and nameserver information[cite: 3]. |
| **WhatWeb** | Identified web technologies and server-related details[cite: 3]. |
| **nslookup** | Resolved domain name to target IP address[cite: 3]. |
| **curl -I** | Inspected HTTP response headers returned by the server[cite: 3]. |
| **WAFW00F** | Checked for Web Application Firewall (WAF) presence[cite: 4]. |
| **Zenmap** | Network scanner used to identify open ports and exposed services[cite: 4]. |
| **SQL Injection** | Tested login functionality for input handling flaws and authentication bypass[cite: 4]. |
| **pdf2john** | Converted PDF password hash structures for processing with John the Ripper[cite: 4]. |
| **John the Ripper** | Executed offline password recovery testing against extracted PDF hashes[cite: 4]. |

---

## 🚨 Findings & Proof of Exploitation

### 1. M1 – Initial Access (SQL Injection)

* **Observation:** Port scanning via Zenmap identified open ports **80** (HTTP) and **443** (HTTPS)[cite: 4]. Analysis of the patient portal login revealed improper handling of user input in SQL queries[cite: 4].
* **Exploitation:** A crafted SQL payload bypassed password verification, allowing unauthorized access to the application without a valid credential[cite: 4].
* **Impact:** Allows attackers to bypass authentication and access protected patient data and administrative functions[cite: 5].

### 2. M2 – Data Extraction (Weak PDF Password)

* **Observation:** Password-protected medical PDF reports were discovered in the portal[cite: 6].
* **Exploitation:** Hashes were extracted using `pdf2john` and cracked offline using `John the Ripper`[cite: 6]. The password was recovered as:
```
password

```


* **Impact:** Extremely weak document passwords allow trivial unauthorized access to confidential patient laboratory reports (e.g., Pathology reports)[cite: 7, 8].

### 3. M3 – Sensitive Data Discovery (Legacy Database Exposure)

* **Observation:** Inspecting `robots.txt` revealed several unindexed paths:
* `/patient/`[cite: 11]
* `/staff/`[cite: 11]
* `/old/`[cite: 11]


* **Exploitation:** Navigating to the `/old/` directory revealed an exposed legacy database backup file named `sql-db-2019`[cite: 11].
* **Impact:** Public exposure of legacy databases can reveal historical patient records, system structural details, and credential dumps[cite: 11].

---

## 📊 Risk Rating

| Finding | Evidence / Observation | Potential Impact | Risk Level |
| --- | --- | --- | --- |
| **SQL Injection – Authentication Bypass** | Login input fields executed raw SQL input to bypass password authentication[cite: 14]. | Unauthorized administrative or user access to application backend[cite: 14]. | **High**[cite: 14] |
| **Exposed Legacy Database Resource** | `/old/` directory exposed database file `sql-db-2019` via `robots.txt` path[cite: 14]. | Disclosure of sensitive data and infrastructure layout[cite: 14]. | **Medium**[cite: 14] |
| **Weak PDF Password** | Password hash cracked using John the Ripper (`password`)[cite: 7, 14]. | Unauthorized exposure of confidential medical records[cite: 14]. | **Medium**[cite: 14] |
| **Information Disclosure via `robots.txt**` | Disclosed restricted directory paths (`/patient/`, `/staff/`, `/old/`)[cite: 11, 14]. | Assists external mapping of sensitive locations[cite: 14]. | **Low**[cite: 14] |

---

## 🛡️ Remediation & Recommendations

### SQL Injection Mitigation

* Use **prepared statements** and **parameterized queries** for all database interactions[cite: 15].
* Implement strict server-side input validation and sanitization[cite: 15].
* Disable verbose SQL error messages in production[cite: 15].

### Legacy Resource Cleanup

* Immediately remove deprecated or legacy directories (e.g., `/old/`) from the web root[cite: 15].
* Store database backups outside of the public web folder[cite: 15].

### Password Policy Enhancement

* Enforce complex password rules for encrypted documents (mix of uppercase, lowercase, numbers, and special characters)[cite: 16].
* Re-encrypt historical confidential documents with stronger key derivation algorithms[cite: 16].

### Information Disclosure Prevention

* Do not rely on `robots.txt` to enforce access control[cite: 16].
* Enforce strict role-based access control (RBAC) on `/patient/`, `/staff/`, and sensitive subdirectories[cite: 16].

---

## ⚖️ Liability Disclaimer

All testing activities described in this report were performed strictly against systems where authorization was granted within a controlled environment[cite: 1, 17]. The contents are strictly intended for educational, research, and authorized security testing purposes[cite: 1, 17].
