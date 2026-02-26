# Reconnaissance & Footprinting Report
## Security Assessment for AltSchoolPay

**Student:** Oluwafisayomi Adekoya  
**Student ID:** ALT/SOE/025/0358  
**Date:** 12 December 2025  
**Lab Domain:** altschoolafrica.com  
**Environment:** Kali Linux VM  

---

## Executive Summary

This report documents reconnaissance and footprinting activities conducted against `altschoolafrica.com` using approved OSINT tools and Nmap scanning. The assessment identified critical security observations including exposed staging subdomains, publicly accessible microservice endpoints, and Cloudflare-protected infrastructure.

All activities were conducted within authorized scope following the **Rules of Engagement (ROE)**.

---

## Part A — Passive Reconnaissance (OSINT) Findings

### Commands Used

```bash
# Subdomain Enumeration
amass enum -passive -d altschoolafrica.com -o amass_results.txt

# Email Harvesting
theHarvester -d altschoolafrica.com
emailharvester -d altschoolafrica.com

# DNS Enumeration
dnsenum altschoolafrica.com
dig altschoolafrica.com ANY
dig altschoolafrica.com MX
dig altschoolafrica.com TXT

# WHOIS Lookup
whois altschoolafrica.com

# Shodan Search
shodan search 172.67.210.81
```

---

### OSINT Findings

#### Finding 1 — Multiple Subdomains Discovered (Amass)

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `amass_results.txt` |

Subdomains found:
- `learn.altschoolafrica.com`
- `apply.altschoolafrica.com`
- `core.altschoolafrica.com`
- `business.altschoolafrica.com`
- `engineering.altschoolafrica.com`
- `assessment.altschoolafrica.com`
- `portal.altschoolafrica.com`
- `store.altschoolafrica.com`

---

#### Finding 2 — Staging & Pre-Production Subdomains Found

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Internal (typically not meant for public exposure) |
| **Source** | `amass_results.txt` |

Subdomains found:
- `students-staging.altschoolafrica.com`
- `connect-stage.altschoolafrica.com`
- `appl-staging.altschoolafrica.com`

---

#### Finding 3 — Cloudways Hosting CNAME References

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `amass_results.txt` |

Subdomains mapped to `secure.cloudwayssites.com`:
- `3mtt.altschoolafrica.com`
- `launchpad.altschoolafrica.com`
- `youthrive.altschoolafrica.com`

> **Note:** Cloudways environments sometimes reveal staging/debug endpoints.

---

#### Finding 4 — Google Workspace MX Records Detected

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `dig.txt` |

```
altschoolafrica.com. MX 1  aspmx.l.google.com.
altschoolafrica.com. MX 5  alt1.aspmx.l.google.com.
altschoolafrica.com. MX 5  alt2.aspmx.l.google.com.
altschoolafrica.com. MX 10 alt3.aspmx.l.google.com.
altschoolafrica.com. MX 10 alt4.aspmx.l.google.com.
```

> **Note:** Confirms domain uses Google Workspace for email.

---

#### Finding 5 — Cloudflare DNS Infrastructure Identified

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `amass_results.txt` |

IP addresses identified:
- `104.21.37.155`
- `172.67.210.81`

> **Note:** Cloudflare hides the real backend/origin server.

---

#### Finding 6 — Cloudflare IPv6 Records Identified

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `amass_results.txt` |

IPv6 addresses:
- `2606:4700:3033::6815:259b`
- `2606:4700:3033::ac43:d251`

> **Note:** Confirms IPv6 support through Cloudflare.

---

#### Finding 7 — DIG ANY Query Failed From Local VM

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `dig.txt` |

```
Output: no servers could be reached
```

> **Note:** Indicates DNS resolution failure within the Kali VM environment, not an issue with the target domain.

---

#### Finding 8 — No Emails Discovered via theHarvester

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `emailHarvester.txt` |

theHarvester attempted multiple search engines (LinkedIn, Bing, Google, Baidu, YouTube) but produced zero valid email addresses.

> **Note:** Likely due to Cloudflare protection, LinkedIn restrictions, and absence of employee directory leakage.

---

#### Finding 9 — High Number of Business-Unit Subdomains

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public / Possibly Internal |
| **Source** | `amass_results.txt` |

Examples:
- `product.altschoolafrica.com`
- `data.altschoolafrica.com`
- `atom.altschoolafrica.com`

> **Note:** These may correspond to microservices or internal systems exposed publicly.

---

#### Finding 10 — Education Platform–Specific Subdomains

| Field | Detail |
|-------|--------|
| **Confidence** | High |
| **Sensitivity** | Public |
| **Source** | `amass_results.txt` |

Examples:
- `courses.altschoolafrica.com`
- `engineering.altschoolafrica.com`
- `learn.altschoolafrica.com`

> **Note:** Typical LMS platform components that increase the overall attack surface.

---

## Part B — Active Scanning (Nmap) Results

### Scan Commands Executed

```bash
# Host Discovery
sudo nmap -sn 172.67.210.81 -oN host_discovery.txt

# Comprehensive Service Scan
sudo nmap -sV -sC 172.67.210.81
sudo nmap -A 172.67.210.81 -oN nmap3
```

---

### Discovered Hosts and Services

**Host:** `www.altschoolafrica.com` / `172.67.210.81`

| IP Address | Open Port | Service | Version / Banner | Notes |
|------------|-----------|---------|-----------------|-------|
| 172.67.210.81 | 80/tcp | HTTP Proxy | Cloudflare | Behind Cloudflare → Origin server hidden |
| 172.67.210.81 | 443/tcp | HTTPS Proxy | Cloudflare | "400 plain HTTP sent to HTTPS port" |
| 172.67.210.81 | 8080/tcp | HTTP Proxy | Cloudflare | No server title (text/plain) |
| 172.67.210.81 | 8443/tcp | HTTPS-Alt | Cloudflare | HTTPS proxy, origin unknown |
| 172.67.210.81 | — | Ping Discovery | Host is up (0-hop) | Cloudflare edge infrastructure |

### Scan Observations

- Cloudflare **obfuscates** real server OS, software, and versions.
- No exploitable services exposed directly.
- All ports lead to Cloudflare reverse proxy nodes.
- OS detection **not possible** due to Cloudflare blocking fingerprinting.
- Additional recon requires DNS, origin IP discovery, and deeper subdomain enumeration.

---

## Part C — Attack Surface DFD & Prioritization

### Data Flow Diagram

```
┌─────────────────┐         ┌──────────────────────────────────────┐
│  External Actors│         │           Entry Points               │
│                 │         │                                      │
│  ┌───────────┐  │         │  ┌────────────┐   ┌──────────────┐  │
│  │   USERS   │──┼────────►│  │ Web Server │──►│   Web App    │  │
│  └───────────┘  │         │  │(port 80/   │   └──────────────┘  │
│                 │         │  │   443)     │                      │
│                 │    ┌───►│  │            │   ┌──────────────┐  │
│  ┌───────────┐  │    │    │  │   Auth     │──►│     SSH      │  │
│  │  3rd-Party│──┼────┘    │  │   Orders   │   └──────────────┘  │
│  │ Merchants │  │         │  │  Database  │                      │
│  └───────────┘  │         │  └─────┬──────┘   ┌──────────────┐  │
└─────────────────┘         │        │      ──►│ Mail Server  │  │
         ▲                  │        │          │    (MX)      │  │
         │   API Gateway    │        ▼          └──────────────┘  │
         └──────────────────┤   ┌──────────┐                      │
                            │   │ Database │                      │
                            │   └──────────┘                      │
                            └──────────────────────────────────────┘
```

### System Components

**External Actors:**
- **Users** — interact with the web application and API.
- **3rd-Party Merchants** — interact via API for payments, orders, or data exchange.

**Entry Points:**
- **Web App (HTTP/HTTPS)** — serves UI, receives user input.
- **API Gateway** — receives requests from users or merchants.
- **SSH** — for administration/management (internal/remote access).
- **Mail Server (MX)** — handles notifications, password resets, and alerts.

**Microservices / Datastores:**
- Web Server (port 80/443) — hosts front-end & back-end logic.
- Application Microservices — authentication, order management, analytics.
- Database / Datastore — stores user info, merchant info, transaction data.
- Legacy / Admin Interface (port 8080) — older management interface.
- Mail Server — Google-hosted (MX: `aspmx.l.google.com`).

**Data Flows:**
1. Users → Web App: Login, view content, place orders.
2. Web App → API Gateway: Requests for data, transactions.
3. API Gateway → Microservices: Processes requests (auth, inventory, orders).
4. Microservices → Database: Read/write user and merchant data.
5. Web App / Microservices → Mail Server: Notifications, alerts.
6. 3rd-Party Merchants → API Gateway: Data exchange, order processing.
7. Admin → SSH / Legacy Interface: System management, monitoring.

---

### Top 5 Priority Targets

| Priority | Target | Risk |
|----------|--------|------|
| 1 | **Web Server — Port 80/443** | Publicly accessible; prime target for SQLi, XSS, outdated software exploitation. |
| 2 | **DNS Server — Port 53** | Susceptible to cache poisoning or amplification attacks; misconfigs can leak internal info. |
| 3 | **Legacy Admin Interface — Port 8080** | Non-standard port; may have unpatched vulnerabilities, default credentials, or debug interfaces. |
| 4 | **Public API Endpoint** | May expose sensitive business logic; improper auth or input validation can lead to data exfiltration. |
| 5 | **SSH Service — Port 22** | If internet-exposed with weak credentials or outdated software, brute-force or exploitation risk. |

---

## Reflection

During this assignment, one of the main challenges was reconciling information from OSINT sources and Nmap scans to create a coherent view of the system. Some services had ambiguous results or required cross-verifying with multiple tools, which was time-consuming.

Translating technical scan data into a Data Flow Diagram also required careful abstraction — deciding which components to include without overcomplicating the diagram.

Through this process, key lessons learned include:

- The importance of prioritizing actionable information.
- Identifying key entry points and understanding how external actors interact with internal services.
- Practical experience visualizing complex systems to clearly communicate data flows and potential security considerations.

Overall, this exercise strengthened the ability to combine reconnaissance data with structured system representation — a critical skill in cybersecurity assessment and reporting.

---

*Report prepared by Oluwafisayomi Adekoya — ALT/SOE/025/0358 | AltSchool Africa*
