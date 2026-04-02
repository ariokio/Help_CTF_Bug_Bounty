# 🟢 RED OPS — Guide d'utilisation
### *Pentest · CTF · Bug Bounty · Red Team*

> **Version :** 1.0 — Avril 2026  
> **Format :** Guide de référence opérationnel — utilisable offline

---

## Table des matières

1. [Vue d'ensemble de l'interface](#1-vue-densemble-de-linterface)
2. [Navigation — comment s'orienter](#2-navigation--comment-sorierter)
3. [Workflow CTF — Résoudre un challenge step by step](#3-workflow-ctf--résoudre-un-challenge-step-by-step)
4. [Workflow Pentest / Red Team](#4-workflow-pentest--red-team)
5. [Workflow Bug Bounty](#5-workflow-bug-bounty)
6. [La section CVE Qualification](#6-la-section-cve-qualification)
7. [Le Rapport HTB/CTF intégré](#7-le-rapport-htbctf-intégré)
8. [Astuces & raccourcis](#8-astuces--raccourcis)
9. [Cas pratiques — exemples concrets](#9-cas-pratiques--exemples-concrets)

---

## 1. Vue d'ensemble de l'interface

```
┌─────────────────────────────────────────────────────────────────┐
│  [ RED_OPS ]          TOPBAR — statique, toujours visible        │
│  // PENTEST & RED TEAM REFERENCE GUIDE //                        │
│  ⚠ AVERTISSEMENT LÉGAL                                           │
├──────────────┬──────────────────────────────────────────────────┤
│              │                                                   │
│   SIDEBAR    │              ZONE DE CONTENU                      │
│   (gauche)   │         (s'affiche au clic menu)                  │
│              │                                                   │
│  // frameworks                                                   │
│  // phases pentest                                               │
│  // domaines spéciaux                                            │
│  // objectifs                                                    │
│  // techniques avancées                                          │
│              │                                                   │
└──────────────┴──────────────────────────────────────────────────┘
```

La page est divisée en **3 zones fixes** :

| Zone | Rôle | Comportement |
|------|------|-------------|
| **Topbar** (haut) | Logo, titre, badges frameworks, disclaimer légal | Fixe — ne scroll jamais |
| **Sidebar** (gauche) | Menu de navigation thématique | Scroll indépendant |
| **Main** (centre) | Contenu de la section active | Scroll indépendant, change au clic |

---

## 2. Navigation — comment s'orienter

### 2.1 Structure du menu gauche

Le menu est organisé en **5 groupes** :

```
// FRAMEWORKS
  ⬡ Vue d'ensemble          → Comparatif PTES, NIST, Kill Chain, MITRE

// PHASES PENTEST            → Le cœur méthodologique
  🔭 Reconnaissance         01
  📡 Scanning & Enum        02
  🔍 Vulnérabilités         03
  💥 Exploitation           04
  🏴 Post-Exploitation      05
  ⬆  Privilege Escalation   06
  ↔  Lateral Movement       07
  🔗 Persistance            08
  📤 Exfiltration           09

// DOMAINES SPÉCIAUX         → Par type de cible
  🌐 Web Attacks
  🏢 Active Directory
  📶 Wireless
  🔐 Crypto & Stego

// OBJECTIFS                 → Par type de mission
  🏁 CTF Methodology
  💰 Bug Bounty
  🧰 Arsenal Complet
  📚 Ressources
  📋 Rapport HTB/CTF        ← Nouveau : rapport automatique
  🛡  CVE Qualification      ← Nouveau : scoring CVSS/SSVC

// TECHNIQUES AVANCÉES       → Expert level
  ☁️  Cloud AWS/Azure/GCP
  🐋 Container Escape
  🪟 Windows Advanced
  🌐 Web Avancé             ← JWT, OAuth, GraphQL, XSS avancé
  💣 Pwn / BOF
  🥷 AV Bypass
  🔬 Forensics & Stéga
  ⚙️  Reverse Engineering
  📡 C2 Frameworks
```

### 2.2 Principe d'affichage

- **Un clic = une section affichée** — les autres sont masquées
- **Par défaut** : la Vue d'ensemble s'affiche au chargement
- **La barre de recherche** en haut du contenu permet de trouver une technique ou commande et d'aller directement à la bonne section
- Chaque **technique-card** se déplie/referme au clic sur son titre

---

## 3. Workflow CTF — Résoudre un challenge step by step

> Applicable à **HackTheBox**, **TryHackMe**, **RootMe**, **PicoCTF**, etc.

### Étape 0 — Setup

```
Menu : Vue d'ensemble → bloc "Setup environnement"
```

```bash
# Connexion VPN HTB
sudo openvpn ~/Downloads/lab_username.ovpn

# Vérifier son IP de tunnel
ip addr show tun0
```

### Étape 1 — Reconnaissance initiale

```
Menu : 🔭 Reconnaissance (Phase 01)
```

**Actions :**
1. Ouvrir la carte **"Reconnaissance Active"**
2. Lancer le scan Nmap de base

```bash
nmap -sV -sC -O -T4 -p- 10.10.10.X -oA scan_initial
```

3. Si la machine répond lentement → utiliser **Masscan** d'abord :
```bash
masscan -p1-65535 --rate 10000 10.10.10.X -oG masscan.txt
nmap -sC -sV -p $(cat masscan.txt | grep open | awk -F'/' '{print $1}' | tr '\n' ',') 10.10.10.X
```

> 💡 **Astuce :** Copier le bloc de commandes directement depuis la page — chaque bloc a un bouton `COPY` en haut à droite.

### Étape 2 — Énumération des services

```
Menu : 📡 Scanning & Enum (Phase 02)
```

Selon les ports ouverts, consulter la carte correspondante :

| Port trouvé | Carte à ouvrir |
|-------------|----------------|
| 80/443/8080 | Énumération Web |
| 445 | Énumération SMB |
| 21 / 22 / 161 / 389 | Autres Services |

### Étape 3 — Identification des vulnérabilités

```
Menu : 🔍 Vulnérabilités (Phase 03)
```

Chercher la correspondance avec la version de service trouvée. Utiliser la **barre de recherche** avec le nom du service ou CVE.

### Étape 4 — Exploitation

```
Menu : 💥 Exploitation (Phase 04)
```

Chaque carte présente :
- Le contexte d'attaque
- Le payload / exploit prêt à copier
- Les variantes (MSF / manuel)

### Étape 5 — Post-Exploitation & Privesc

```
Menu : 🏴 Post-Exploitation (Phase 05)
       ⬆  Privilege Escalation (Phase 06)
```

**Checklist Linux Privesc :**
```bash
id && whoami
sudo -l                          # sudo sans mot de passe ?
find / -perm -4000 2>/dev/null   # SUID
cat /etc/crontab                 # cron jobs
```

**Checklist Windows Privesc :**
```powershell
whoami /all
net user
net localgroup administrators
systeminfo | findstr /B /C:"OS"
```

### Étape 6 — Rédiger le rapport

```
Menu : 📋 Rapport HTB/CTF
```

Le formulaire intégré génère automatiquement un rapport complet. Voir [section 7](#7-le-rapport-htbctf-intégré).

---

## 4. Workflow Pentest / Red Team

### Phase de planification

Avant toute action, se référer à la **Vue d'ensemble** pour choisir le framework adapté au scope :

| Contexte | Framework recommandé |
|----------|---------------------|
| Pentest boîte noire, client externe | **PTES** (7 phases) |
| Mission gouvernementale / compliance | **NIST SP 800-115** |
| Simulation attaque réelle APT | **Cyber Kill Chain** |
| Red Team avec purple team | **MITRE ATT&CK** |

### Kill Chain — séquence opérationnelle

```
RECON → WEAPONIZE → DELIVER → EXPLOIT → INSTALL → C2 → ACTIONS
  01        02          03        04         05       06      07
```

Chaque étape de la Kill Chain est **cliquable** dans la vue d'ensemble et pointe vers la phase correspondante.

### Techniques avancées Red Team

Pour des engagements complexes, utiliser le groupe **// TECHNIQUES AVANCÉES** :

| Objectif | Section |
|----------|---------|
| Contournement EDR/AV | 🥷 AV Bypass |
| Infrastructure C2 | 📡 C2 Frameworks (Sliver, Havoc, Covenant) |
| Mouvement latéral AD | 🏢 Active Directory |
| Cloud pivot | ☁️ Cloud AWS/Azure/GCP |
| Conteneurs compromis | 🐋 Container Escape |

---

## 5. Workflow Bug Bounty

```
Menu : 💰 Bug Bounty
```

### Méthodologie recommandée

```
1. Scope & Rules → Lire le programme (HackerOne, Bugcrowd, Intigriti)
2. Recon passive → sous-domaines, GitHub leaks, Shodan
3. Surface d'attaque → endpoints, paramètres, APIs
4. Test méthodique → OWASP Top 10 en priorité
5. Preuve → PoC reproductible, capture, vidéo
6. Report → titre clair, impact, CVSS, remediation
```

### Outils clés Bug Bounty dans RED OPS

| Outil | Section | Usage |
|-------|---------|-------|
| **subfinder / amass** | 🔭 Recon | Énumération sous-domaines |
| **dalfox / XSStrike** | 🌐 Web Avancé | XSS automatisé |
| **sqlmap** | 🌐 Web Avancé | SQLi automatisé |
| **ffuf** | 📡 Scanning | Fuzzing endpoints |
| **jwt_tool** | 🌐 Web Avancé | Attaques JWT |
| **nuclei** | 🔍 Vulnérabilités | Scan templates |

### Qualifier un CVE trouvé

```
Menu : 🛡 CVE Qualification
```

Saisir le CVE-ID → l'outil récupère automatiquement les données NVD et calcule le score SSVC + CVSS. Voir [section 6](#6-la-section-cve-qualification).

---

## 6. La section CVE Qualification

```
Menu : 🛡 CVE Qualification  (badge CVSS dans la sidebar)
```

Cette section ouvre un **panneau modal** avec 4 onglets :

### Onglet 1 — Qualification CVE

| Champ | Description |
|-------|-------------|
| CVE-ID | Ex : `CVE-2021-44228` (Log4Shell) |
| Score CVSS | Récupéré automatiquement depuis NVD |
| Score EPSS | Probabilité d'exploitation dans les 30j |
| KEV | In-the-wild exploitation confirmée (CISA) |
| Décision SSVC | **Act / Attend / Monitor / Track** |

**Workflow de qualification :**
```
1. Saisir le CVE-ID
2. Cliquer "Analyser"
3. Lire la décision SSVC automatique
4. Ajuster avec le contexte interne (onglet 3)
5. Copier le résumé pour le rapport
```

### Onglet 2 — Convertisseur CVSS

Permet de **recalculer** le score CVSS avec les métriques environnementales propres à l'organisation.

### Onglet 3 — Contexte interne

Deux vues disponibles :
- **🧭 Questionnaire guidé** — 7 questions pour recommandation automatique
- **📋 Grille de référence** — tableau complet Exposition × Impact MEF

> Cette grille est particulièrement utile en pentest pour prioriser les findings selon l'impact métier réel.

### Onglet 4 — Documentation

Guide complet d'interprétation des scores, glossaire CVSS/SSVC/EPSS, et liens vers les ressources officielles.

---

## 7. Le Rapport HTB/CTF intégré

```
Menu : 📋 Rapport HTB/CTF
```

### Fonctionnalités

Le module de rapport permet de **documenter une session** en temps réel et d'exporter en plusieurs formats.

### Structure du rapport

```
┌─ INFORMATIONS GÉNÉRALES ──────────────────────┐
│  Titre · Auteur · Date · Type · Cible · OS    │
│  Difficulté · Objectifs (flags obtenus)        │
└───────────────────────────────────────────────┘
┌─ RÉSUMÉ EXÉCUTIF ─────────────────────────────┐
│  Description narrative de la session           │
└───────────────────────────────────────────────┘
┌─ PHASE 1 — RECONNAISSANCE ────────────────────┐
│  Output Nmap · Services · Sous-domaines        │
└───────────────────────────────────────────────┘
┌─ PHASE 2 — ACCÈS INITIAL ─────────────────────┐
│  Vecteur d'attaque · CVE · Exploit utilisé     │
└───────────────────────────────────────────────┘
┌─ PHASE 3 — ÉLÉVATION DE PRIVILÈGES ───────────┐
│  Méthode · Commandes · Preuve                  │
└───────────────────────────────────────────────┘
┌─ FINDINGS ────────────────────────────────────┐
│  [+ Ajouter] par finding :                    │
│   • Titre · Sévérité (Critical→Info)           │
│   • CVSS · CVE · Description · PoC · Reméd.   │
└───────────────────────────────────────────────┘
┌─ TIMELINE ────────────────────────────────────┐
│  Notes horodatées de progression               │
└───────────────────────────────────────────────┘
┌─ RECOMMANDATIONS & CONCLUSION ────────────────┐
└───────────────────────────────────────────────┘
```

### Templates disponibles

Cliquer sur **"Charger un template"** pour pré-remplir le rapport :

| Template | Usage |
|----------|-------|
| `HTB Machine Linux` | Machine HackTheBox type Linux |
| `HTB Machine Windows` | Machine HackTheBox type Windows |
| `Bug Bounty Report` | Rapport de soumission bug bounty |
| `Pentest Réseau Interne` | Pentest réseau d'entreprise |

### Export

| Bouton | Format | Utilisation |
|--------|--------|-------------|
| `📄 Exporter HTML` | `.html` standalone | Partage client, archivage |
| `📝 Markdown` | `.md` | GitHub, Obsidian, Notion |
| `🖨 Imprimer` | PDF via navigateur | Rapport formel |

> 💡 **Conseil :** Rédiger les notes de timeline **en temps réel** pendant la session — c'est plus précis et plus utile pour le rapport final.

---

## 8. Astuces & raccourcis

### Navigation rapide

| Action | Comment faire |
|--------|--------------|
| Aller à une technique | Barre de recherche en haut du contenu |
| Copier une commande | Bouton `COPY` en haut à droite de chaque bloc |
| Dérouler tous les détails | Cliquer sur l'en-tête de chaque carte |
| Changer de section | Clic sur un item du menu gauche |

### Barre de recherche

La recherche est **transversale** — elle parcourt toutes les sections et ouvre automatiquement la première qui contient le terme, en dépliant les cartes correspondantes.

Exemples de recherches utiles :

```
"log4j"       → Exploitation Log4Shell
"john"        → Hash cracking avec John The Ripper
"bloodhound"  → Active Directory enumeration
"linpeas"     → Linux privilege escalation
"chisel"      → Tunneling / pivoting
"jwt"         → Attaques JWT
```

### Bouton COPY

Chaque bloc de code dispose d'un bouton `COPY` qui copie le contenu **brut** (sans la coloration syntaxique) directement dans le presse-papiers. Pratique pour coller dans un terminal.

---

## 9. Cas pratiques — exemples concrets

### Cas 1 — Machine HTB "Linux Web App"

```
Situation : scan nmap → port 80 ouvert, Apache 2.4.49

1. Menu 📡 Scanning → carte "Énumération Web"
   → gobuster pour trouver /cgi-bin/

2. Menu 🔍 Vulnérabilités → recherche "CVE-2021-41773"
   → Path Traversal / RCE Apache

3. Menu 💥 Exploitation → curl exploit

4. Menu ⬆ Privilege Escalation → SUID find binary
   → find / -perm -4000 → /usr/bin/find → GTFOBins

5. Menu 📋 Rapport → Remplir, exporter HTML
```

### Cas 2 — CTF Crypto

```
Situation : fichier .jpg + texte chiffré

1. Menu 🔐 Crypto & Stego → carte "Stéganographie"
   → binwalk, steghide, strings

2. Menu 🔐 Crypto & Stego → carte "Chiffrement classique"
   → CyberChef, ROT13, Base64, Vigenère

3. Menu 🔬 Forensics & Stéga → analyse métadonnées
   → exiftool, foremost
```

### Cas 3 — Bug Bounty XSS

```
Situation : formulaire de recherche d'un site web

1. Menu 💰 Bug Bounty → méthodologie de test

2. Menu 🌐 Web Avancé → carte "XSS Avancé"
   → dalfox scan automatique
   → XSStrike pour bypass WAF
   → Payloads DOM XSS

3. Menu 🛡 CVE Qualification → documenter l'impact

4. Menu 📋 Rapport → template Bug Bounty
   → Sévérité, PoC, recommandations
```

### Cas 4 — Active Directory Pentest

```
Situation : accès initial obtenu, credentials domaine faibles

1. Menu 🏢 Active Directory → carte "BloodHound"
   → SharpHound collection, analyse des chemins

2. Menu 🏢 Active Directory → carte "Kerberoasting"
   → GetUserSPNs.py → hashcat -m 13100

3. Menu ↔ Lateral Movement → Pass-the-Hash / Pass-the-Ticket

4. Menu ⬆ Privilege Escalation → DCSync si DA accessible
```

---

## Annexes

### Frameworks couverts

| Framework | Description | Phase couverte |
|-----------|-------------|----------------|
| **PTES** | Penetration Testing Execution Standard | Toutes les phases 01-09 |
| **NIST SP 800-115** | Standard gouvernemental US | Vue d'ensemble |
| **Cyber Kill Chain** | Lockheed Martin — 7 étapes | Vue d'ensemble + kill chain interactive |
| **MITRE ATT&CK** | Matrice tactiques/techniques | CVE Qualification + référence |
| **OWASP** | Web security best practices | Web Attacks + Web Avancé |
| **CVSS v3.1** | Scoring de vulnérabilités | CVE Qualification |
| **SSVC** | Stakeholder-Specific Vuln. Categorization | CVE Qualification |

### Plateformes supportées

| Plateforme | Tags dans la page |
|------------|------------------|
| HackTheBox | `HTB` (orange) |
| TryHackMe | `THM` (vert) |
| RootMe | `RM` (rouge) |
| Bug Bounty | `BB` (bleu) |

### Ressources recommandées

- **HackTricks** — https://book.hacktricks.xyz
- **GTFOBins** — https://gtfobins.github.io
- **LOLBAS** — https://lolbas-project.github.io
- **PortSwigger Web Academy** — https://portswigger.net/web-security
- **PayloadsAllTheThings** — https://github.com/swisskyrepo/PayloadsAllTheThings
- **MITRE ATT&CK** — https://attack.mitre.org
- **NVD/NIST** — https://nvd.nist.gov
- **CyberChef** — https://gchq.github.io/CyberChef

---

*Guide rédigé pour RED OPS v1.0 — Avril 2026*  
*Usage exclusivement légal : CTF, labs autorisés, Bug Bounty, pentest avec autorisation écrite*

