# 🟢 RED OPS — Usage Guide
### *Pentest · CTF · Bug Bounty · Red Team*

> **Version:** 1.0 — April 2026  
> **Format:** Operational Reference Guide — usable offline

---

## Table of Contents

1. [Interface Overview](#1-interface-overview)
2. [Navigation — How to Find Your Way](#2-navigation--how-to-find-your-way)
3. [CTF Workflow — Solve a Challenge Step by Step](#3-ctf-workflow--solve-a-challenge-step-by-step)
4. [Pentest / Red Team Workflow](#4-pentest--red-team-workflow)
5. [Bug Bounty Workflow](#5-bug-bounty-workflow)
6. [The CVE Qualification Section](#6-the-cve-qualification-section)
7. [The Integrated HTB/CTF Report](#7-the-integrated-htbctf-report)
8. [Tips & Shortcuts](#8-tips--shortcuts)
9. [Practical Cases — Real Examples](#9-practical-cases--real-examples)

---

## 1. Interface Overview

```
┌────────────────────────────────────────────────────────────────[...]
│  [ RED_OPS ]          TOPBAR — static, always visible          │
│  // PENTEST & RED TEAM REFERENCE GUIDE //                      │
│  ⚠ LEGAL DISCLAIMER                                            │
├──────────────┬──────────────────────────────────────────────────[...]
│              │                                                  │
│   SIDEBAR    │              CONTENT ZONE                        │
│   (left)     │         (displayed on menu click)                │
│              │                                                  │
│  // frameworks                                                  │
│  // pentest phases                                              │
│  // special domains                                             │
│  // objectives                                                  │
│  // advanced techniques                                         │
│              │                                                  │
└──────────────┴──────────────────────────────────────────────────[...]
```

The page is divided into **3 fixed zones**:

| Zone | Role | Behavior |
|------|------|----------|
| **Topbar** (top) | Logo, title, framework badges, legal disclaimer | Fixed — never scrolls |
| **Sidebar** (left) | Thematic navigation menu | Independent scroll |
| **Main** (center) | Content of active section | Independent scroll, changes on click |

---

## 2. Navigation — How to Find Your Way

### 2.1 Left Menu Structure

The menu is organized into **5 groups**:

```bash
// FRAMEWORKS
  ⬡ Overview          → Comparison of PTES, NIST, Kill Chain, MITRE

// PENTEST PHASES            → The methodological core
  🔭 Reconnaissance         01
  📡 Scanning & Enum        02
  🔍 Vulnerabilities        03
  💥 Exploitation           04
  🏴 Post-Exploitation      05
  ⬆  Privilege Escalation   06
  ↔  Lateral Movement       07
  🔗 Persistence            08
  📤 Exfiltration           09

// SPECIAL DOMAINS         → By target type
  🌐 Web Attacks
  🏢 Active Directory
  📶 Wireless
  🔐 Crypto & Stego

// OBJECTIVES                 → By mission type
  🏁 CTF Methodology
  💰 Bug Bounty
  🧰 Complete Arsenal
  📚 Resources
  📋 HTB/CTF Report        ← New: automatic report
  🛡  CVE Qualification      ← New: CVSS/SSVC scoring

// ADVANCED TECHNIQUES       → Expert level
  ☁️  Cloud AWS/Azure/GCP
  🐋 Container Escape
  🪟 Windows Advanced
  🌐 Advanced Web             ← JWT, OAuth, GraphQL, Advanced XSS
  💣 Pwn / BOF
  🥷 AV Bypass
  🔬 Forensics & Stega
  ⚙️  Reverse Engineering
  📡 C2 Frameworks
```

### 2.2 Display Principle

- **One click = one section displayed** — others are hidden
- **By default**: the Overview is displayed on page load
- **The search bar** at the top of content lets you find a technique or command and jump directly to the right section
- Each **technique-card** expands/collapses on click of its title

---

## 3. CTF Workflow — Solve a Challenge Step by Step

> Applicable to **HackTheBox**, **TryHackMe**, **RootMe**, **PicoCTF**, etc.

### Step 0 — Setup

```
Menu: Overview → "Setup environment" section
```

```bash
# HTB VPN Connection
sudo openvpn ~/Downloads/lab_username.ovpn

# Check your tunnel IP
ip addr show tun0
```

### Step 1 — Initial Reconnaissance

```
Menu: 🔭 Reconnaissance (Phase 01)
```

**Actions:**
1. Open the **"Active Reconnaissance"** card
2. Launch the basic Nmap scan

```bash
nmap -sV -sC -O -T4 -p- 10.10.10.X -oA scan_initial
```

3. If the machine responds slowly → use **Masscan** first:
```bash
masscan -p1-65535 --rate 10000 10.10.10.X -oG masscan.txt
nmap -sC -sV -p $(cat masscan.txt | grep open | awk -F'/' '{print $1}' | tr '\n' ',') 10.10.10.X
```

> 💡 **Tip:** Copy command blocks directly from the page — each block has a `COPY` button in the top right.

### Step 2 — Service Enumeration

```
Menu: 📡 Scanning & Enum (Phase 02)
```

Based on open ports, consult the corresponding card:

| Port Found | Card to Open |
|------------|--------------|
| 80/443/8080 | Web Enumeration |
| 445 | SMB Enumeration |
| 21 / 22 / 161 / 389 | Other Services |

### Step 3 — Vulnerability Identification

```
Menu: 🔍 Vulnerabilities (Phase 03)
```

Find the match with the service version found. Use the **search bar** with the service name or CVE.

### Step 4 — Exploitation

```
Menu: 💥 Exploitation (Phase 04)
```

Each card presents:
- The attack context
- The ready-to-copy payload / exploit
- Variants (MSF / manual)

### Step 5 — Post-Exploitation & Privesc

```
Menu: 🏴 Post-Exploitation (Phase 05)
       ⬆  Privilege Escalation (Phase 06)
```

**Linux Privesc Checklist:**
```bash
id && whoami
sudo -l                          # sudo without password?
find / -perm -4000 2>/dev/null   # SUID
cat /etc/crontab                 # cron jobs
```

**Windows Privesc Checklist:**
```powershell
whoami /all
net user
net localgroup administrators
systeminfo | findstr /B /C:"OS"
```

### Step 6 — Write the Report

```
Menu: 📋 HTB/CTF Report
```

The integrated form automatically generates a complete report. See [section 7](#7-the-integrated-htbctf-report).

---

## 4. Pentest / Red Team Workflow

### Planning Phase

Before any action, refer to the **Overview** to choose the framework suited to the scope:

| Context | Recommended Framework |
|---------|----------------------|
| Black box pentest, external client | **PTES** (7 phases) |
| Government mission / compliance | **NIST SP 800-115** |
| Real APT attack simulation | **Cyber Kill Chain** |
| Red Team with purple team | **MITRE ATT&CK** |

### Kill Chain — Operational Sequence

```
RECON → WEAPONIZE → DELIVER → EXPLOIT → INSTALL → C2 → ACTIONS
  01        02          03        04         05       06      07
```

Each step of the Kill Chain is **clickable** in the overview and points to the corresponding phase.

### Advanced Red Team Techniques

For complex engagements, use the **// ADVANCED TECHNIQUES** group:

| Objective | Section |
|-----------|---------|
| EDR/AV Bypass | 🥷 AV Bypass |
| C2 Infrastructure | 📡 C2 Frameworks (Sliver, Havoc, Covenant) |
| AD Lateral Movement | 🏢 Active Directory |
| Cloud Pivot | ☁️ Cloud AWS/Azure/GCP |
| Compromised Containers | 🐋 Container Escape |

---

## 5. Bug Bounty Workflow

```
Menu: 💰 Bug Bounty
```

### Recommended Methodology

```
1. Scope & Rules → Read the program (HackerOne, Bugcrowd, Intigriti)
2. Passive Recon → Subdomains, GitHub leaks, Shodan
3. Attack Surface → Endpoints, parameters, APIs
4. Methodical Testing → OWASP Top 10 priority
5. Proof → Reproducible PoC, screenshot, video
6. Report → Clear title, impact, CVSS, remediation
```

### Key Bug Bounty Tools in RED OPS

| Tool | Section | Usage |
|------|---------|-------|
| **subfinder / amass** | 🔭 Recon | Subdomain enumeration |
| **dalfox / XSStrike** | 🌐 Advanced Web | Automated XSS |
| **sqlmap** | 🌐 Advanced Web | Automated SQLi |
| **ffuf** | 📡 Scanning | Endpoint fuzzing |
| **jwt_tool** | 🌐 Advanced Web | JWT attacks |
| **nuclei** | 🔍 Vulnerabilities | Template scanning |

### Qualify a Found CVE

```
Menu: 🛡 CVE Qualification
```

Enter the CVE-ID → the tool automatically fetches NVD data and calculates SSVC + CVSS score. See [section 6](#6-the-cve-qualification-section).

---

## 6. The CVE Qualification Section

```
Menu: 🛡 CVE Qualification  (CVSS badge in sidebar)
```

This section opens a **modal panel** with 4 tabs:

### Tab 1 — CVE Qualification

| Field | Description |
|-------|-------------|
| CVE-ID | Example: `CVE-2021-44228` (Log4Shell) |
| CVSS Score | Automatically fetched from NVD |
| EPSS Score | Probability of exploitation in 30 days |
| KEV | In-the-wild exploitation confirmed (CISA) |
| SSVC Decision | **Act / Attend / Monitor / Track** |

**Qualification Workflow:**
```
1. Enter the CVE-ID
2. Click "Analyze"
3. Read the automatic SSVC decision
4. Adjust with internal context (tab 3)
5. Copy the summary for the report
```

### Tab 2 — CVSS Converter

Allows you to **recalculate** the CVSS score with environmental metrics specific to your organization.

### Tab 3 — Internal Context

Two available views:
- **🧭 Guided Questionnaire** — 7 questions for automatic recommendation
- **📋 Reference Grid** — complete Exposure × Impact MEF table

> This grid is especially useful in pentests for prioritizing findings according to actual business impact.

### Tab 4 — Documentation

Complete guide to interpreting scores, CVSS/SSVC/EPSS glossary, and links to official resources.

---

## 7. The Integrated HTB/CTF Report

```
Menu: 📋 HTB/CTF Report
```

### Features

The report module allows you to **document a session** in real-time and export in multiple formats.

### Report Structure

```
┌─ GENERAL INFORMATION ──────────────────────────┐
│  Title · Author · Date · Type · Target · OS    │
│  Difficulty · Objectives (flags obtained)       │
└────────────────────────────────────────────────┘
┌─ EXECUTIVE SUMMARY ───────────────────────────┐
│  Narrative description of the session          │
└────────────────────────────────────────────────┘
┌─ PHASE 1 — RECONNAISSANCE ────────────────────┐
│  Nmap Output · Services · Subdomains           │
└────────────────────────────────────────────────┘
┌─ PHASE 2 — INITIAL ACCESS ───────────────────┐
│  Attack Vector · CVE · Exploit Used            │
└────────────────────────────────────────────────┘
┌─ PHASE 3 — PRIVILEGE ESCALATION ─────────────┐
│  Method · Commands · Proof                     │
└────────────────────────────────────────────────┘
┌─ FINDINGS ────────────────────────────────────┐
│  [+ Add] per finding:                          │
│   • Title · Severity (Critical→Info)           │
│   • CVSS · CVE · Description · PoC · Remediation│
└────────────────────────────────────────────────┘
┌─ TIMELINE ────────────────────────────────────┐
│  Time-stamped progress notes                   │
└────────────────────────────────────────────────┘
┌─ RECOMMENDATIONS & CONCLUSION ────────────────┐
└────────────────────────────────────────────────┘
```

### Available Templates

Click on **"Load a template"** to pre-fill the report:

| Template | Usage |
|----------|-------|
| `HTB Machine Linux` | Typical HackTheBox Linux machine |
| `HTB Machine Windows` | Typical HackTheBox Windows machine |
| `Bug Bounty Report` | Bug bounty submission report |
| `Internal Network Pentest` | Corporate network pentest |

### Export

| Button | Format | Usage |
|--------|--------|-------|
| `📄 Export HTML` | `.html` standalone | Client sharing, archiving |
| `📝 Markdown` | `.md` | GitHub, Obsidian, Notion |
| `🖨 Print` | PDF via browser | Formal report |

> 💡 **Tip:** Write timeline notes **in real-time** during the session — it's more accurate and more useful for the final report.

---

## 8. Tips & Shortcuts

### Quick Navigation

| Action | How to Do It |
|--------|-------------|
| Go to a technique | Search bar at the top of content |
| Copy a command | `COPY` button in the top right of each block |
| Expand all details | Click on the header of each card |
| Switch sections | Click on a left menu item |

### Search Bar

The search is **cross-section** — it scans all sections and automatically opens the first one containing the term, expanding the corresponding cards.

Useful search examples:

```
"log4j"       → Log4Shell Exploitation
"john"        → Hash cracking with John The Ripper
"bloodhound"  → Active Directory enumeration
"linpeas"     → Linux privilege escalation
"chisel"      → Tunneling / pivoting
"jwt"         → JWT Attacks
```

### COPY Button

Each code block has a `COPY` button that copies the **raw** content (without syntax highlighting) directly to your clipboard. Great for pasting into a terminal.

---

## 9. Practical Cases — Real Examples

### Case 1 — HTB Machine "Linux Web App"

```
Situation: nmap scan → port 80 open, Apache 2.4.49

1. Menu 📡 Scanning → "Web Enumeration" card
   → gobuster to find /cgi-bin/

2. Menu 🔍 Vulnerabilities → search "CVE-2021-41773"
   → Path Traversal / RCE Apache

3. Menu 💥 Exploitation → curl exploit

4. Menu ⬆ Privilege Escalation → SUID find binary
   → find / -perm -4000 → /usr/bin/find → GTFOBins

5. Menu 📋 Report → Fill in, export HTML
```

### Case 2 — CTF Crypto

```
Situation: .jpg file + encrypted text

1. Menu 🔐 Crypto & Stego → "Steganography" card
   → binwalk, steghide, strings

2. Menu 🔐 Crypto & Stego → "Classical Encryption" card
   → CyberChef, ROT13, Base64, Vigenère

3. Menu 🔬 Forensics & Stega → Metadata analysis
   → exiftool, foremost
```

### Case 3 — Bug Bounty XSS

```
Situation: Search form on a website

1. Menu 💰 Bug Bounty → Testing methodology

2. Menu 🌐 Advanced Web → "Advanced XSS" card
   → dalfox automated scan
   → XSStrike for WAF bypass
   → DOM XSS Payloads

3. Menu 🛡 CVE Qualification → Document impact

4. Menu 📋 Report → Bug Bounty template
   → Severity, PoC, recommendations
```

### Case 4 — Active Directory Pentest

```
Situation: Initial access obtained, weak domain credentials

1. Menu 🏢 Active Directory → "BloodHound" card
   → SharpHound collection, path analysis

2. Menu 🏢 Active Directory → "Kerberoasting" card
   → GetUserSPNs.py → hashcat -m 13100

3. Menu ↔ Lateral Movement → Pass-the-Hash / Pass-the-Ticket

4. Menu ⬆ Privilege Escalation → DCSync if DA accessible
```

---

## Appendices

### Covered Frameworks

| Framework | Description | Phase Covered |
|-----------|-------------|----------------|
| **PTES** | Penetration Testing Execution Standard | All phases 01-09 |
| **NIST SP 800-115** | US government standard | Overview |
| **Cyber Kill Chain** | Lockheed Martin — 7 steps | Overview + interactive kill chain |
| **MITRE ATT&CK** | Tactics/techniques matrix | CVE Qualification + reference |
| **OWASP** | Web security best practices | Web Attacks + Advanced Web |
| **CVSS v3.1** | Vulnerability scoring | CVE Qualification |
| **SSVC** | Stakeholder-Specific Vuln. Categorization | CVE Qualification |

### Supported Platforms

| Platform | Tags on Page |
|----------|--------------|
| HackTheBox | `HTB` (orange) |
| TryHackMe | `THM` (green) |
| RootMe | `RM` (red) |
| Bug Bounty | `BB` (blue) |

### Recommended Resources

- **HackTricks** — https://book.hacktricks.xyz
- **GTFOBins** — https://gtfobins.github.io
- **LOLBAS** — https://lolbas-project.github.io
- **PortSwigger Web Academy** — https://portswigger.net/web-security
- **PayloadsAllTheThings** — https://github.com/swisskyrepo/PayloadsAllTheThings
- **MITRE ATT&CK** — https://attack.mitre.org
- **NVD/NIST** — https://nvd.nist.gov
- **CyberChef** — https://gchq.github.io/CyberChef

---

*Guide written for RED OPS v1.0 — April 2026*  
*Exclusively legal usage: CTF, authorized labs, Bug Bounty, authorized pentest*