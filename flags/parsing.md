# 🔍 Reading & Parsing Nmap Output

Nmap gives you a lot of information. Knowing how to read it fast — and pull out exactly what you need — is a skill that separates beginners from people who actually move efficiently during a scan.

Think of it like getting a massive grocery receipt. You don't read every line — you scan for the things that matter.

---

## 📄 Understanding Raw Nmap Output

Here's what a standard nmap result looks like and what each part means:
```bash
Nmap scan report for 10.10.10.1
Host is up (0.032s latency).

PORT     STATE    SERVICE    VERSION
22/tcp   open     ssh        OpenSSH 7.9 (protocol 2.0)
80/tcp   open     http       Apache httpd 2.4.38
443/tcp  closed   https
8080/tcp filtered http-proxy
```

| Term | What it means |
|---|---|
| `open` | Something is actively listening on this port — investigate it |
| `closed` | Port is reachable but nothing is listening — move on |
| `filtered` | Firewall is blocking nmap — can't tell if it's open or closed |
| `open\|filtered` | Can't determine — common in UDP scans |

> 💡 **Only `open` ports matter.** Closed and filtered are dead ends unless you have a specific reason to investigate.

---

## ⚡ Filtering Output with --open

The fastest way to cut noise — only show open ports:
```bash
nmap -sV -sC --open -oN scan.txt <target>
```

---

## 🔎 Grep Tricks — Pull What You Need Fast
Note: ⚠️ **Windows users:** `grep` and `awk` are not available natively on Windows.
> Use Git Bash, WSL (Windows Subsystem for Linux), or PowerShell equivalents.
> The Python examples work identically on all three platforms.

Once you have a saved output file, use `grep` to find exactly what you're looking for.
```bash
# Show only open ports
grep "open" scan.txt

# Find a specific service
grep "ssh" scan.txt

# Find version info
grep "VERSION" scan.txt

# Show only lines with port numbers
grep "^[0-9]" scan.txt

# Find all HTTP services
grep -i "http" scan.txt
```

---

## 🧮 Grep on Grepable Output (-oG)

The grepable format is specifically designed for filtering. Always save with `-oA` to get all formats including grepable.
```bash
# Extract just open ports from grepable output
grep "open" scan.gnmap | awk '{print $2, $5}'

# Pull all open ports as a comma-separated list
grep "open" scan.gnmap | grep -oP '\d+/open' | cut -d'/' -f1 | tr '\n' ','

# Find hosts with port 80 open
grep "80/open" scan.gnmap
```

---

## 🛠️ Extracting Open Ports for Follow-up Scans

This is a power move — extract open ports from a fast scan and feed them into a deeper scan automatically.
```bash
# Step 1 — fast scan, save grepable output
nmap -p- --min-rate 5000 -oG ports.gnmap <target>

# Step 2 — extract open ports into a variable
ports=$(grep "open" ports.gnmap | grep -oP '\d+/open' | cut -d'/' -f1 | tr '\n' ',' | sed 's/,$//')

# Step 3 — deep scan only the open ports
nmap -sV -sC -p $ports -oN deep.txt <target>
```

This two-step approach is faster than running a full `-p-` scan with all scripts — find the ports fast, then interrogate only what's open.

---

## 📊 Reading Version Detection Output

When you run `-sV`, nmap tries to identify exactly what software is running. Here's how to use that information:
```bash
22/tcp open  ssh     OpenSSH 7.9p1 Ubuntu 10 (Ubuntu Linux; protocol 2.0)
```

From this one line you know:
- **Port 22** is open
- **SSH** is running
- **OpenSSH 7.9p1** — searchable on exploit-db.com and CVE databases
- **Ubuntu** — tells you the OS family
- **Protocol 2.0** — SSHv2, expected

> 💡 Every version number nmap gives you is a potential CVE search. Copy it directly into https://exploit-db.com or https://nvd.nist.gov and see what comes up.

---

## 🖥️ XML Output — For Tool Integration

The XML output format (`-oX`) is designed for importing into other tools.
```bash
# Import nmap XML into Metasploit
msf> db_import scan.xml

# Parse XML with Python (basic example)
python3 -c "
import xml.etree.ElementTree as ET
tree = ET.parse('scan.xml')
root = tree.getroot()
for host in root.findall('host'):
    for port in host.findall('.//port'):
        if port.find('state').get('state') == 'open':
            print(port.get('portid'), port.find('service').get('name'))
"
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*