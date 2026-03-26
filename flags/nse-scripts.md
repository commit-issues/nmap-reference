# 📜 NSE — Nmap Scripting Engine

Most people know `--script vuln` and stop there. That's like buying a chef's knife and only using it to open packages. LITERALLY NEVER DO THIS! 🔪🔪🔪

NSE (Nmap Scripting Engine) is a built-in scripting framework that lets nmap do way more than just find open ports — it can enumerate users, brute force credentials, check for specific CVEs, crawl web directories, and more. There are over 600 scripts included with nmap by default.

---

## 📂 Script Categories

Scripts are organized into categories. You can run an entire category at once or call individual scripts by name.

| Category | What it does | Noise Level |
|---|---|---|
| `auth` | Tests authentication — default creds, bypass attempts | 🟡 Medium |
| `broadcast` | Sends broadcast packets to discover hosts and services | 🟡 Medium |
| `brute` | Brute force credentials on services | 🔴 Noisy |
| `default` | Safe, useful scripts — what `-sC` runs | 🟡 Medium |
| `discovery` | Enumerate information about targets | 🟡 Medium |
| `dos` | Denial of service testing | 🔴 Never in production |
| `exploit` | Attempts to exploit vulnerabilities | 🔴 Noisy — use with permission only |
| `external` | Queries external databases and services | 🟡 Medium |
| `fuzzer` | Sends unexpected input to find crashes | 🔴 Noisy |
| `intrusive` | Aggressive scripts likely to cause issues | 🔴 Noisy |
| `malware` | Checks for malware and backdoors | 🟡 Medium |
| `safe` | Won't crash services or generate alerts | 🟢 Quiet |
| `version` | Helps with version detection | 🟡 Medium |
| `vuln` | Checks for known vulnerabilities | 🔴 Noisy |

---

## 🚀 Running Scripts
```bash
# Run default scripts (same as -sC)
nmap --script default <target>

# Run a specific script
nmap --script http-enum <target>

# Run multiple scripts
nmap --script http-enum,http-headers <target>

# Run an entire category
nmap --script vuln <target>

# Run multiple categories
nmap --script "default,safe" <target>

# Run all scripts matching a pattern
nmap --script "http-*" <target>

# Pass arguments to a script
nmap --script http-brute --script-args userdb=users.txt,passdb=pass.txt <target>
```

---

## 🎯 Most Useful Scripts by Scenario

### 🌐 Web Targets
```bash
# Enumerate web directories and files
nmap --script http-enum -p 80,443,8080 <target>

# Grab HTTP headers — reveals server info
nmap --script http-headers -p 80,443 <target>

# Check for common web vulnerabilities
nmap --script http-vuln* -p 80,443 <target>

# Find virtual hosts
nmap --script http-vhosts -p 80,443 <target>

# Check for shellshock
nmap --script http-shellshock -p 80,443 <target>

# SQL injection detection
nmap --script http-sql-injection -p 80,443 <target>
```

### 🖥️ SMB / Windows Targets
```bash
# Full SMB enumeration
nmap --script smb-enum-shares,smb-enum-users -p 445 <target>

# Check for EternalBlue (MS17-010) — WannaCry vulnerability
nmap --script smb-vuln-ms17-010 -p 445 <target>

# Check for all SMB vulnerabilities
nmap --script "smb-vuln*" -p 445 <target>

# Get OS info via SMB
nmap --script smb-os-discovery -p 445 <target>

# Enumerate SMB security level
nmap --script smb-security-mode -p 445 <target>
```

### 🔑 SSH Targets
```bash
# Check SSH host key and algorithms
nmap --script ssh-hostkey -p 22 <target>

# Enumerate supported auth methods
nmap --script ssh-auth-methods -p 22 <target>

# Brute force SSH (use carefully, will lock accounts)
nmap --script ssh-brute -p 22 <target>
```

### 🗄️ Database Targets
```bash
# MySQL enumeration
nmap --script mysql-info,mysql-databases,mysql-users -p 3306 <target>

# MySQL empty root password check
nmap --script mysql-empty-password -p 3306 <target>

# MSSQL enumeration
nmap --script ms-sql-info,ms-sql-config -p 1433 <target>

# MongoDB check
nmap --script mongodb-info -p 27017 <target>
```

### 📡 DNS Targets
```bash
# Zone transfer attempt
nmap --script dns-zone-transfer -p 53 <target>

# Brute force subdomains
nmap --script dns-brute <target>

# DNS service discovery
nmap --script dns-service-discovery <target>
```

### 🔒 SSL/TLS Targets
```bash
# Full SSL/TLS enumeration — cipher suites, cert info
nmap --script ssl-enum-ciphers -p 443 <target>

# Check for Heartbleed
nmap --script ssl-heartbleed -p 443 <target>

# Check for POODLE
nmap --script ssl-poodle -p 443 <target>

# Get certificate details
nmap --script ssl-cert -p 443 <target>
```

### 🔍 General Vulnerability Scanning
```bash
# Run all vuln scripts — the kitchen sink
nmap --script vuln <target>

# Safe vuln check — less noisy
nmap --script "vuln and safe" <target>
```

---

## 📍 Where Scripts Live
```bash
# Linux / macOS
/usr/share/nmap/scripts/

# List all available scripts
ls /usr/share/nmap/scripts/

# Search for scripts by keyword
ls /usr/share/nmap/scripts/ | grep http
```

On Windows with nmap installed, scripts are in:
```
C:\Program Files (x86)\Nmap\scripts\
```

---

## 📖 Script Help

Every script has built-in documentation.
```bash
# View script documentation
nmap --script-help http-enum

# View all script args
nmap --script-help http-brute
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*