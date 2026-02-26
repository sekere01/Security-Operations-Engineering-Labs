# Week 1 — Reconnaissance & Footprinting

![Date](https://img.shields.io/badge/Date-12%20Dec%202025-blue)
![Domain](https://img.shields.io/badge/Lab%20Domain-altschoolafrica.com-lightgrey)
![Findings](https://img.shields.io/badge/OSINT%20Findings-10-brightgreen)
![Scope](https://img.shields.io/badge/Scope-Authorized-success)

**Date:** 12 December 2025  
**Lab Domain:** altschoolafrica.com  
**Environment:** Kali Linux VM  

---

## 🎯 Objectives

- Perform passive reconnaissance using OSINT tools without touching the target
- Conduct active scanning using Nmap to identify open ports and services
- Map the full attack surface and construct a Data Flow Diagram (DFD)
- Prioritize the top 5 targets by risk level with justification

---

## 🛠️ Tools Used

| Tool | Purpose | Phase |
|------|---------|-------|
| `amass` | Subdomain enumeration | Passive |
| `theHarvester` | Email harvesting | Passive |
| `dnsenum` / `dig` | DNS record enumeration | Passive |
| `whois` | Domain ownership & registrar info | Passive |
| `shodan` | Internet-exposed asset discovery | Passive |
| `nmap -sV -sC -A` | Port, service & OS detection | Active |

---

## 📊 Summary of Findings

| # | Finding | Sensitivity | Source |
|---|---------|-------------|--------|
| 1 | 8+ subdomains enumerated | Public | Amass |
| 2 | Staging servers publicly exposed | Internal | Amass |
| 3 | Cloudways CNAME references found | Public | Amass |
| 4 | Google Workspace MX records confirmed | Public | dig |
| 5 | Cloudflare IPs identified (origin hidden) | Public | Amass |
| 6 | IPv6 Cloudflare records discovered | Public | Amass |
| 7 | DIG ANY failed on Kali VM | N/A | dig |
| 8 | Zero emails found via theHarvester | Public | theHarvester |
| 9 | Business-unit microservice subdomains | Possibly Internal | Amass |
| 10 | LMS-specific subdomains identified | Public | Amass |

---

## 🔴 Top 5 Priority Targets

| Priority | Target | Risk Level |
|----------|--------|------------|
| 1 | Web Server (Port 80/443) | Critical |
| 2 | DNS Server (Port 53) | High |
| 3 | Legacy Admin Interface (Port 8080) | High |
| 4 | Public API Endpoint | High |
| 5 | SSH Service (Port 22) | Medium |

---

## 📄 Files

- [`reports/Week1_Recon_Report.md`](./reports/Week1_Recon_Report.md) — Full detailed reconnaissance report with all commands, findings, DFD, and reflection
