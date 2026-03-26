# 🔭 Advanced Nmap Techniques

This is where nmap stops being a beginner tool and starts being a serious weapon. These techniques are used post-exploitation, during internal network recon, and in scenarios where you need to scan through tunnels, proxies, or from inside a compromised host.

This is the stuff that separates people who *use* nmap from people who *know* nmap.

---

## 🖥️ OS Fingerprinting

OS fingerprinting means using nmap to guess what operating system is running on a target. This feeds directly into your exploitation and privilege escalation research — knowing the OS and kernel version tells you exactly what CVEs to look for.
```bash
# Basic OS detection
nmap -O <target>

# Aggressive OS guessing — use when nmap isn't confident
nmap -O --osscan-guess <target>

# OS + version + scripts + traceroute
nmap -A <target>

# Maximum OS detection effort
nmap -O --osscan-limit --osscan-guess -sV --version-intensity 9 <target>
```

> 💡 OS detection requires at least one open and one closed port to work accurately. If you only have open ports, results may be unreliable.

---

## 🔀 Pivot Scanning — Scanning Internal Networks from a Compromised Host

Once you have a foothold on a machine, you can use that machine to scan the internal network — targets that are completely invisible from the outside. This is called pivot scanning.

Nmap itself doesn't pivot — you need a tunnel or proxy first. Here's how it works:

### Using Proxychains + Nmap
```bash
# Step 1 — set up a SOCKS proxy through your SSH session
ssh -D 1080 -f -N user@<compromised-host>

# Step 2 — configure proxychains to use your SOCKS proxy
# Edit /etc/proxychains.conf and add:
# socks5 127.0.0.1 1080

# Step 3 — run nmap through proxychains
proxychains nmap -sT -Pn -n <internal-target>
```

> ⚠️ When scanning through proxychains use `-sT` (TCP connect) not `-sS` (SYN scan) — SYN scans don't work through proxies. Also use `-Pn` since ICMP won't pass through the tunnel.

### Using Chisel
```bash
# On your attack machine — start chisel server
chisel server -p 8080 --reverse

# On the compromised host — connect back and create tunnel
chisel client <your-ip>:8080 R:1080:socks

# Now scan through the tunnel
proxychains nmap -sT -Pn -n <internal-target>
```

---

## 🌐 Internal Subnet Discovery

You're inside a network. You need to know what else is there. Start quiet, then go deeper.
```bash
# Step 1 — find your own IP and subnet
ip addr show
# or on Windows:
ipconfig

# Step 2 — ping sweep the subnet (quiet, fast)
nmap -sn 172.16.0.0/24

# Step 3 — quick port check on discovered hosts
nmap -sT -Pn -p 22,80,443,445,3389 172.16.0.0/24

# Step 4 — full scan on interesting hosts
nmap -sT -Pn -sV -p- --min-rate 5000 <internal-target>
```

---

## 🎭 Decoy Scanning

Make it look like the scan is coming from multiple sources at once — your real IP is hidden among decoys.
```bash
# Scan with 10 random decoys
nmap -D RND:10 <target>

# Scan with specific decoys
nmap -D 10.0.0.1,10.0.0.2,10.0.0.3 <target>

# Full stealth combo — decoys + fragmentation + spoofed source port
nmap -sS -D RND:10 -f --source-port 53 --data-length 200 -T2 <target>
```

> ⚠️ Decoy scanning doesn't make you invisible — it makes forensic attribution harder. A good IDS will still flag the scan. Use on authorized engagements only.

---

## 🔌 Firewall & IDS Evasion Techniques

When standard scans get blocked, these techniques help you get through.
```bash
# Fragment packets — breaks scan into tiny pieces
nmap -f <target>

# Custom MTU size — must be multiple of 8
nmap --mtu 16 <target>

# Spoof source port — firewalls often allow DNS (53) and HTTP (80) traffic
nmap --source-port 53 <target>
nmap --source-port 80 <target>

# Pad packets with random data
nmap --data-length 200 <target>

# Slow scan — stay under threshold-based detection
nmap -T1 --scan-delay 5s <target>

# Randomize host order on subnet scans
nmap --randomize-hosts 192.168.1.0/24

# Full evasion combo
nmap -sS -T2 -f --mtu 16 -D RND:5 --source-port 53 --data-length 200 --randomize-hosts -oA evasion-scan <target>
```

---

## 🕳️ Scanning Through Firewalls with ACK Scans

An ACK scan doesn't tell you if ports are open — it tells you if ports are **filtered** by a firewall. Use it to map firewall rules.
```bash
# ACK scan — maps firewall rules
nmap -sA <target>

# Window scan — similar to ACK, works on some systems
nmap -sW <target>
```

| Result | What it means |
|---|---|
| `unfiltered` | Firewall is NOT blocking this port |
| `filtered` | Firewall IS blocking this port |

---

## ⚡ Speed Optimization for Large Scans
```bash
# Fastest full port scan
nmap -p- --min-rate 10000 -T5 <target>

# Balanced speed + accuracy
nmap -p- --min-rate 5000 -T4 <target>

# Scan multiple hosts simultaneously
nmap -sV --min-parallelism 100 192.168.1.0/24

# Disable DNS resolution — speeds up scans significantly
nmap -n -p- --min-rate 5000 <target>
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*