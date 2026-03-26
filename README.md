# 🗺️ nmap-reference

> A clean, practical nmap reference guide for CTF players and security professionals.
> Bookmark it, star it, share it. Take a look around for noobs, students, CTF players, and pros.  
> Part of the SudoCode Pentesting Methodology Guide — by SudoChef.

![Documentation](https://img.shields.io/badge/Documentation-Complete-C9667A?style=flat-square)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-Ready-9B8EC4?style=flat-square)
![CTF Ready](https://img.shields.io/badge/CTF-Ready-7ABFB0?style=flat-square)
![No Code](https://img.shields.io/badge/Docs_Only-No_Code-2B2D3A?style=flat-square)

---

## 🧭 Navigation

- [What is nmap?](#-what-is-nmap)
- [Scan Menu](#-scan-menu)
- [Flag Reference](#-flag-reference)
- [CTF vs Professional Use](#-ctf-vs-professional-use)
- [Common Ports](#-common-ports)
- [Complementary Tools](#-complementary-tools)
- [Cheat Sheet](#-cheat-sheet)

---

## 🔍 What is nmap?

**nmap** (Network Mapper) is a free, open-source tool used to discover hosts and services on a network. Think of it like knocking on every door in a building to see who answers — and then noting what kind of door each one has.

It is one of the first tools you will run on any target during a penetration test (authorized security assessment) or CTF (Capture the Flag) challenge.

Official documentation: https://nmap.org/docs.html

---

## 🍽️ Scan Menu

These are the most useful named scan combinations. Pick the one that fits your situation.

| Scan Name | Command | Use Case |
|---|---|---|
| 🔍 The Standard | `nmap -sV -sC -oN scan.txt <target>` | Default starting scan for most targets |
| 👻 The Ghost | `nmap -sS -T2 -f --data-length 200 <target>` | Slow, fragmented — evade basic detection |
| 🔬 The Deep Dive | `nmap -sV -sC -p- -oN full.txt <target>` | All 65535 ports, full version + scripts |
| 🌐 The Full Sweep | `nmap -sn 192.168.1.0/24` | Host discovery across a subnet |
| 🕸️ The Web Check | `nmap -sV -p 80,443,8080,8443 --script http-enum <target>` | Web service fingerprinting |
| 📡 The UDP Pass | `nmap -sU --top-ports 20 <target>` | Top 20 UDP ports |
| 🛡️ The Vuln Sweep | `nmap --script vuln <target>` | Check for known vulnerabilities |

---

## 🚩 Flag Reference

<details>
<summary><strong>🎯 Target Specification</strong></summary>

| Flag | What it does |
|---|---|
| `<target>` | Single IP or hostname |
| `192.168.1.0/24` | Entire subnet |
| `-iL targets.txt` | Read targets from a file |
| `--exclude <IP>` | Skip a specific host |

</details>

<details>
<summary><strong>⚡ Scan Techniques</strong></summary>

| Flag | What it does | Noise Level |
|---|---|---|
| `-sS` | SYN scan — half-open, fast, stealthy | 🟡 Medium |
| `-sT` | TCP connect scan — full connection | 🔴 Noisy |
| `-sU` | UDP scan | 🟡 Medium |
| `-sn` | Ping sweep — no port scan, just host discovery | 🟢 Quiet |
| `-sV` | Version detection — what software is running | 🟡 Medium |
| `-sC` | Default scripts — runs common nmap scripts | 🟡 Medium |
| `-A` | Aggressive — OS detection, version, scripts, traceroute | 🔴 Noisy |

</details>

<details>
<summary><strong>🕐 Timing & Speed</strong></summary>

Nmap has 6 timing templates. T0 is the slowest and stealthiest. T5 is the fastest and loudest.

| Flag | Name | When to use |
|---|---|---|
| `-T0` | Paranoid | IDS evasion — extremely slow |
| `-T1` | Sneaky | Slow evasion |
| `-T2` | Polite | Reduces bandwidth usage |
| `-T3` | Normal | Default |
| `-T4` | Aggressive | CTF and lab environments |
| `-T5` | Insane | Fast — unreliable on slow networks |

</details>

<details>
<summary><strong>🔒 Port Specification</strong></summary>

| Flag | What it does |
|---|---|
| `-p 80` | Scan a single port |
| `-p 80,443,8080` | Scan specific ports |
| `-p 1-1000` | Scan a port range |
| `-p-` | Scan all 65535 ports |
| `--top-ports 20` | Scan the 20 most common ports |

</details>

<details>
<summary><strong>💾 Output Formats</strong></summary>

Always save your output. You will need it later.

| Flag | What it does |
|---|---|
| `-oN scan.txt` | Normal output — human readable |
| `-oX scan.xml` | XML output — for tools like Metasploit |
| `-oG scan.gnmap` | Grepable output |
| `-oA scan` | All three formats at once |

</details>

---

## ⚔️ CTF vs Professional Use

| Situation | CTF | Professional Engagement |
|---|---|---|
| Speed | Fast is fine — T4 | Slow and deliberate — T2 or T3 |
| Noise | Doesn't matter | Matters — stay under IDS radar |
| Full port scan | Always | Only if scoped and approved |
| Aggressive scan (-A) | Go for it | Avoid — too loud |
| Save output | Good habit | Required — goes in your report |
| Scripts (--script) | Use freely | Check scope first |

---

## 🔌 Common Ports

<details>
<summary><strong>View common ports and what to do when you find them</strong></summary>

| Port | Service | What to do |
|---|---|---|
| 21 | FTP | Try anonymous login: `ftp <target>` |
| 22 | SSH | Check version, try creds if you have them |
| 23 | Telnet | Unencrypted — try connecting |
| 25 | SMTP | Email server — check for user enumeration |
| 53 | DNS | Try zone transfer: `dig axfr @<target>` |
| 80 | HTTP | Open in browser, run Nikto, gobuster |
| 110 | POP3 | Email — try default creds |
| 139/445 | SMB | Run `enum4linux`, check for EternalBlue |
| 443 | HTTPS | Same as 80 — check cert for info |
| 3306 | MySQL | Try `mysql -u root -h <target>` |
| 3389 | RDP | Remote Desktop — check for BlueKeep |
| 5432 | PostgreSQL | Try default creds |
| 6379 | Redis | Often unauthenticated — high value |
| 8080 | HTTP-alt | Another web server — check it |
| 27017 | MongoDB | Often unauthenticated |

</details>

---

## 🛠️ Complementary Tools

These tools work alongside nmap to give you a fuller picture.

| Tool | What it does | Best for |
|---|---|---|
| **RustScan** | Fast port scanner — finds open ports then hands off to nmap | CTF speed runs |
| **Masscan** | Fastest port scanner available — scans the entire internet | Large networks |
| **Nikto** | Web server scanner — finds misconfigs and vulnerabilities | Web targets |
| **Netcat** | Network swiss army knife — connect, listen, transfer | Everything |

---

## 📋 Beginners Cheat Sheet (Intermediate and Advanced see below ⬇️
```bash
# Quick start — standard scan
nmap -sV -sC -oN scan.txt <target>

# Full port scan
nmap -sV -sC -p- -oN full.txt <target>

# Host discovery only
nmap -sn 192.168.1.0/24

# Stealth scan
nmap -sS -T2 <target>

# Vulnerability scripts
nmap --script vuln <target>

# UDP top ports
nmap -sU --top-ports 20 <target>

# Web enumeration
nmap -sV -p 80,443,8080 --script http-enum <target>
```

---

## 📁 Advanced Reference Index

### 🚩 Flags
| File | What's inside |
|---|---|
| [Flag Overview & Complete Reference](flags/overview.md) | Important flags, `-Pn`, `--min-rate`, smart CTF port strategy, top 20 ports |
| [Reading & Parsing Output](flags/parsing.md) | How to read nmap results, grep tricks, extracting ports, XML integration |
| [NSE Scripts — Nmap Scripting Engine](flags/nse-scripts.md) | Script categories, 600+ scripts, best scripts by scenario |

### 🍽️ Scan Combinations
| File | What's inside |
|---|---|
| [Named Scans](scan-combos/named-scans.md) | The Standard, The Ghost, The Deep Dive, The Full Sweep, and more |
| [Field Scans](scan-combos/field-scans.md) | Real scans used in CTF, school labs, research, and professional engagements |
| [Advanced Techniques](scan-combos/advanced.md) | Pivot scanning, OS fingerprinting, decoys, firewall evasion, internal recon |
| [What To Do With Your Results](scan-combos/what-next.md) | Post-scan workflow, service research, documentation habits |

### 🔌 Ports
| File | What's inside |
|---|---|
| [Common Ports Reference](ports/common-ports.md) | 25 common ports, what they are, and exactly what to do when you find them open |

### ⚔️ CTF vs Professional
| File | What's inside |
|---|---|
| [CTF vs Professional Engagement](ctf-vs-real/differences.md) | Mindset, methodology, rules, and the golden rule that keeps you out of trouble |

### 🛠️ Other Tools
| File | What's inside |
|---|---|
| [Complementary Tools](other-scanners/tools.md) | RustScan, Masscan, Nikto, Netcat — with Mac, Linux, and Windows install instructions |

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*
