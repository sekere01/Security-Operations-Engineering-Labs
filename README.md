# 🛡️ ALT-SOE-025 — Security Operations Engineering Labs

![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Program](https://img.shields.io/badge/AltSchool%20Africa-SOE%20Track-orange)
![Tools](https://img.shields.io/badge/Tools-Nmap%20%7C%20Amass%20%7C%20Shodan%20%7C%20theHarvester-blue)
![Environment](https://img.shields.io/badge/Environment-Kali%20Linux-red)

**Student:** Oluwafisayomi Adekoya  
**ID:** ALT/SOE/025/0358  
**Program:** AltSchool Africa — School of Engineering (Cybersecurity Track)

---

## 👋 About This Repository

This repository documents my hands-on cybersecurity lab work completed as part of the AltSchool Africa Security Operations Engineering program. Each week covers a core offensive/defensive security concept, executed in a controlled, authorized environment.

The labs demonstrate practical skills in reconnaissance, scanning, exploitation, and reporting — the same workflow used in real-world penetration testing engagements.

---

## 📁 Labs Index

| Week | Topic | Key Tools | Status |
|------|-------|-----------|--------|
| [Week 1](./Week1-Recon/) | Reconnaissance & Footprinting | Amass, Nmap, Shodan, theHarvester, dig | ✅ Completed |

> More weeks will be added as the program progresses.

---

## 🔍 Week 1 Highlights — Recon & Footprinting

> **Target:** altschoolafrica.com (authorized lab domain)  
> **Date:** 12 December 2025

### What I did
- Performed **passive OSINT** to enumerate subdomains, DNS records, email infrastructure, and hosting providers without touching the target directly
- Ran **active Nmap scans** to identify open ports and services
- Identified **staging environments** publicly exposed on the internet
- Built a **Data Flow Diagram** mapping the full attack surface
- Prioritized the **top 5 targets** by risk level

### Key Findings
- 10+ subdomains discovered including exposed staging servers (`students-staging`, `connect-stage`)
- Domain protected by **Cloudflare** — origin IP successfully hidden
- **Google Workspace** confirmed as email provider via MX records
- Ports **80, 443, 8080, 8443** open — all routed through Cloudflare reverse proxy
- No direct OS fingerprinting possible due to Cloudflare obfuscation

📄 **[Read the full report →](./Week1-Recon/reports/Week1_Recon_Report.md)**

---

## 🧰 Tools & Skills Demonstrated

| Category | Tools / Concepts |
|----------|-----------------|
| Passive Recon | Amass, theHarvester, WHOIS, Shodan |
| DNS Analysis | dig (ANY, MX, TXT), dnsenum |
| Active Scanning | Nmap (`-sV`, `-sC`, `-A`, `-sn`) |
| Infrastructure Analysis | Cloudflare detection, CNAME mapping |
| Reporting | Attack surface mapping, DFD construction, risk prioritization |

---

## ⚠️ Disclaimer

All assessments in this repository were conducted on **authorized lab domains** provided by AltSchool Africa. All activities followed a defined **Rules of Engagement (ROE)**. No unauthorized systems were targeted.
