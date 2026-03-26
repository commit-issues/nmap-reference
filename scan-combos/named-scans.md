# 🍽️ Named Scan Combinations

These are ready-to-use scan combinations for common situations. Each one has a name, a command, and an explanation of when and why to use it.

Think of these like recipes — you don't need to understand every ingredient to follow the recipe. But the more you use them, the more you'll start to understand why each piece is there.

---

## 🔍 The Standard
```bash
nmap -sV -sC -oN scan.txt <target>
```

**What it does:** Version detection + default scripts + saves output.  
**When to use it:** Your default starting scan on any target.  
**Noise level:** 🟡 Medium  

---

## 👻 The Ghost
```bash
nmap -sS -T2 -f --data-length 200 <target>
```

**What it does:** SYN scan, slow timing, fragmented packets, padded data length — harder to detect.  
**When to use it:** When you need to stay under the radar on a real engagement.  
**Noise level:** 🟢 Low  

---

## 🔬 The Deep Dive
```bash
nmap -sV -sC -p- -oN full.txt <target>
```

**What it does:** Full port scan — all 65535 ports — with version detection and default scripts.  
**When to use it:** After your standard scan, when you want to make sure nothing is hiding on an unusual port.  
**Noise level:** 🔴 High — takes time and generates a lot of traffic  

---

## 🌐 The Full Sweep
```bash
nmap -sn 192.168.1.0/24
```

**What it does:** Ping sweep — finds which hosts are alive on a network without scanning ports.  
**When to use it:** When you have a subnet and need to know what's online before you start scanning.  
**Noise level:** 🟢 Low  

---

## 🕸️ The Web Check
```bash
nmap -sV -p 80,443,8080,8443 --script http-enum <target>
```

**What it does:** Targets web ports specifically and runs the http-enum script to find directories and services.  
**When to use it:** When you know you're dealing with a web target and want a fast web-focused scan.  
**Noise level:** 🟡 Medium  

---

## 📡 The UDP Pass
```bash
nmap -sU --top-ports 20 <target>
```

**What it does:** Scans the top 20 most common UDP ports.  
**When to use it:** UDP is often overlooked — services like DNS (53), SNMP (161), and TFTP (69) run on UDP.  
**Noise level:** 🟡 Medium — UDP scans are slow  

---

## 🛡️ The Vuln Sweep
```bash
nmap --script vuln <target>
```

**What it does:** Runs nmap's built-in vulnerability detection scripts against the target.  
**When to use it:** Quick check for known vulnerabilities on open ports. Not a replacement for a full vuln scanner but a solid first pass.  
**Noise level:** 🔴 High — only use with permission  

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*