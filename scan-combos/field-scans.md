# 🔭 Field Scans — Real World, School Labs & CTF

These are the scans that actually get used. Not theoretical — these are the commands people run in the field, in school labs, and during CTF competitions. Bookmarked, memorized, and ready to go.

---

## 🏫 School & Lab Environments

Low stakes, controlled networks. The goal is learning and documentation.
```bash
# Standard lab scan — version detection, default scripts, save output
nmap -sV -sC -oA lab-scan <target>

# Full network discovery — who's online
nmap -sn 192.168.1.0/24

# Full port scan on a single lab host
nmap -sV -sC -p- --min-rate 5000 -oA lab-full <target>

# Quick web check on a lab web server
nmap -sV -p 80,443,8080,8443 --script http-enum <target>
```

---

## 🚩 CTF — HackTheBox, TryHackMe, PicoCTF

Speed matters. You want open ports fast, then go deep on what's there.
```bash
# Step 1 — Fast top ports first (don't waste time on a full scan yet)
nmap -sV -sC -T4 --top-ports 20 -oN quick.txt <target>

# Step 2 — Host not responding? Always try -Pn on HTB
nmap -sV -sC -T4 -Pn --top-ports 20 -oN quick.txt <target>

# Step 3 — Expand if needed
nmap -sV -sC -T4 -p 1-10000 -oN medium.txt <target>

# Step 4 — Full sweep only if you're stuck
nmap -sV -sC -p- --min-rate 5000 -T4 -oN full.txt <target>

# Step 5 — Don't forget UDP — DNS, SNMP, and TFTP hide here
nmap -sU --top-ports 20 -oN udp.txt <target>

# Web box? Add this
nmap -sV -p 80,443,8080,8443 --script http-enum -oN web.txt <target>

# Windows box? Check SMB
nmap --script smb-vuln-ms17-010 -p 445 -oN smb.txt <target>
```

---

## 🔬 Security Research

Thorough documentation, full coverage, saving everything.
```bash
# Comprehensive single host scan
nmap -sV -sC -O -p- --min-rate 5000 -oA research-full <target>

# Service version maximum intensity
nmap -sV --version-intensity 9 -oN versions.txt <target>

# Full vulnerability sweep
nmap --script vuln -oN vulns.txt <target>

# SSL/TLS audit
nmap --script ssl-enum-ciphers,ssl-cert,ssl-heartbleed -p 443 -oN ssl.txt <target>

# Full SMB audit
nmap --script "smb-vuln*,smb-enum*" -p 445 -oN smb-full.txt <target>

# DNS audit
nmap --script dns-zone-transfer,dns-brute -p 53 -oN dns.txt <target>
```

---

## 💼 Professional Engagement

Slow, deliberate, documented. Noise level matters. Scope matters. Everything gets saved.
```bash
# Initial discovery — quiet ping sweep
nmap -sn --randomize-hosts -T2 192.168.1.0/24 -oA discovery

# Targeted service scan — only approved hosts
nmap -sS -sV -T2 -p 22,80,443,445,3389 -oA targeted <target>

# Stealth scan — evade basic IDS
nmap -sS -T2 -f --data-length 200 --source-port 53 -oA stealth <target>

# Full port scan — only if explicitly in scope
nmap -sS -p- --min-rate 1000 -T2 -oA full-engagement <target>

# Vulnerability check — only run with written permission
nmap --script "vuln and safe" -oA safe-vulns <target>
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*