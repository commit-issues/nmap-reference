# 🛠️ Complementary Tools

Nmap is the foundation — but it works best as part of a toolkit. These tools each do something nmap doesn't, or do something nmap does but faster or deeper.

---

## ⚡ RustScan

RustScan is a port scanner built for speed. It finds open ports in seconds and then automatically hands the results off to nmap for deeper scanning. Think of it as nmap's faster little sibling — it does the grunt work so nmap can focus on the details.

**Install:**
- Kali/Linux: `sudo apt install rustscan`
- macOS: `brew install rustscan`
- Windows: Download from https://github.com/RustScan/RustScan/releases
```bash
# Basic scan — finds open ports then passes to nmap
rustscan -a <target> -- -sV -sC

# Scan with custom port range
rustscan -a <target> -p 1-10000 -- -sV
```

**Best for:** CTF speed runs where you need open ports fast.

---

## 🚀 Masscan

Masscan is the fastest port scanner available — capable of scanning the entire internet in under 6 minutes. It sacrifices depth for raw speed. Use it to get a quick picture of what's open across a large range of IPs.

**Install:**
- Kali/Linux: `sudo apt install masscan`
- macOS: `brew install masscan`
- Windows: Download from https://github.com/robertdavidgraham/masscan/releases
```bash
# Scan a subnet for port 80
masscan 192.168.1.0/24 -p 80

# Scan top ports across a range
masscan 192.168.1.0/24 -p 22,80,443 --rate 10000
```

**Best for:** Large networks where nmap would take too long.  
⚠️ Never use on networks you don't own or have explicit permission to scan — it generates massive traffic.

---

## 🌐 Nikto

Nikto is a web server scanner. Point it at a web target and it checks for misconfigurations, outdated software, dangerous files, and known vulnerabilities. It's loud and not stealthy — but it's fast and thorough.

**Install:**
- Kali/Linux: Pre-installed on Kali — or `sudo apt install nikto`
- macOS: `brew install nikto`
- Windows: Download from https://github.com/sullo/nikto/releases
```bash
# Basic web scan
nikto -h http://<target>

# Scan specific port
nikto -h <target> -p 8080

# Save output
nikto -h <target> -o nikto.txt
```

**Best for:** Web targets — run it alongside gobuster for full web enumeration coverage.

---

## 🔧 Netcat

Netcat is the swiss army knife of networking. It can connect to open ports, listen for incoming connections, transfer files, and create reverse shells. It doesn't scan — it communicates. Once nmap tells you what's open, netcat helps you interact with it.

**Install:**
- Kali/Linux: Pre-installed — or `sudo apt install netcat`
- macOS: Pre-installed — or `brew install netcat`
- Windows: Download ncat from https://nmap.org/ncat
(Windows: Use `ncat` instead of `nc` in all commands below)
```bash
# Connect to an open port and banner grab
nc <target> <port>

# Listen for incoming connection
nc -lvnp 4444

# Simple file transfer — receiver
nc -lvnp 4444 > received_file

# Simple file transfer — sender
nc <target> 4444 < file_to_send
```

**Best for:** Banner grabbing, testing open ports manually, catching reverse shells.

---

### ⚡ NetExec

The actively maintained successor to CrackMapExec (CME). Where CME left off, NetExec picks up — faster, more reliable, and actively developed. Used for SMB, LDAP, WinRM, SSH, and RDP enumeration and credential testing across networks. If you are doing any Windows or Active Directory work, this is in your toolkit.

**Install:**
```bash
# Kali Linux
sudo apt install netexec

# pip
pip3 install netexec
```

**Basic usage:**
```bash
# SMB host discovery across subnet
netexec smb 192.168.1.0/24

# Check for null sessions
netexec smb <target> -u '' -p ''

# Enumerate shares
netexec smb <target> -u user -p password --shares

# Check for SMB signing (important for relay attacks)
netexec smb 192.168.1.0/24 --gen-relay-list relay.txt

# WinRM enumeration
netexec winrm <target> -u user -p password

# LDAP enumeration
netexec ldap <target> -u user -p password --users
```

**Resources:**
- Docs: https://www.netexec.wiki/
- Kali: https://www.kali.org/tools/netexec/

---

## 🗺️ More Documentation Coming

This is just the beginning. More tool references, methodology guides, and deep dives are on the way — all part of the SudoCode Pentesting Methodology Guide series.

Follow along and reach out directly:

- 📸 Instagram: [@sudochef](https://www.instagram.com/sudochef)
- 🎵 TikTok: [@sudochef](https://www.tiktok.com/@sudochef)

Have questions, found something useful, or want to see a specific tool covered next? Slide into the DMs — always happy to connect with people learning security. 

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*