# 🚩 Flag Reference — Complete Guide

Nmap flags are options you add to your command to control *how* the scan runs — how fast, how loud, what to look for, and what to save.

Think of it like ordering at a restaurant. `nmap <target>` is just "I'll have something." The flags are how you say "medium rare, no onions, extra sauce — and I needed it five minutes ago."

---

## 🎯 Target Specification

| Flag | What it does | Example |
|---|---|---|
| `<target>` | Single IP or hostname | `nmap 10.10.10.1` |
| `192.168.1.0/24` | Entire subnet | `nmap 192.168.1.0/24` |
| `-iL targets.txt` | Read targets from a file | `nmap -iL targets.txt` |
| `--exclude <IP>` | Skip a specific host | `nmap 10.10.10.0/24 --exclude 10.10.10.1` |
| `-6` | IPv6 scanning | `nmap -6 <target>` |

---

## 🔊 Understanding Noise Levels

Every scan in this guide has a noise rating. Here's what each level means:

| Level | Meaning |
|---|---|
| 🟢 Quiet | Minimal packets — unlikely to appear in logs or trigger alerts |
| 🟡 Medium | Moderate traffic — may appear in logs, unlikely to trigger IDS |
| 🟠 Loud | Significant traffic — likely to trigger IDS alerts on monitored networks |
| 🔴 Noisy | Heavy traffic — will trigger alerts, may cause instability or false negatives |

> 💡 **False negatives** happen when a scan misses something that's actually there. Going too fast causes packets to drop — open ports show as closed. Slower isn't just stealthier, it's more accurate.

---

## ⚡ Scan Techniques

| Flag | What it does | Noise Level | Notes |
|---|---|---|---|
| `-sS` | SYN scan — half-open, never completes the handshake | 🟡 Medium | Default when run as root |
| `-sT` | TCP connect — full connection, more detectable | 🔴 Noisy | Default without root |
| `-sU` | UDP scan — slow but finds things TCP misses | 🟡 Medium | Always worth running |
| `-sn` | Ping sweep — host discovery only, no port scan | 🟢 Quiet | Great for subnet recon |
| `-sV` | Version detection — what software is running | 🟠 Loud | Essential on every scan |
| `-sC` | Default scripts — automated common checks | 🟠 Loud | Pairs with `-sV` always |
| `-A` | Aggressive — OS, version, scripts, traceroute | 🔴 Noisy | CTF only |
| `-sN` | Null scan — no flags set | 🟢 Quiet | Evades some firewalls |
| `-sF` | FIN scan — only FIN flag set | 🟢 Quiet | Evades some firewalls |
| `-sX` | Xmas scan — FIN, PSH, URG flags set | 🟢 Quiet | Evades some firewalls |

---

## 🔒 Port Specification

This is where most beginners get it wrong. Scanning all 65535 ports on every CTF box is not the move — at least not first.

### 🧠 Smart CTF Port Strategy

**Step 1 — Fast targeted scan first:**
```bash
nmap -sV -sC --top-ports 20 -oN quick.txt <target>
```

**Step 2 — If nothing interesting, expand:**
```bash
nmap -sV -sC -p 1-10000 -oN medium.txt <target>
```

**Step 3 — Still nothing? Full port sweep:**
```bash
nmap -sV -sC -p- --min-rate 5000 -oN full.txt <target>
```

| Flag | What it does |
|---|---|
| `-p 80` | Single port |
| `-p 80,443,8080` | Specific ports |
| `-p 1-1000` | Port range |
| `-p-` | All 65535 ports |
| `--top-ports 20` | 20 most common ports |
| `--top-ports 100` | 100 most common ports |
| `--min-rate 5000` | Send at least 5000 packets per second — speeds up full scans massively |

---

## 🏓 Host Discovery

| Flag | What it does | When to use |
|---|---|---|
| `-Pn` | Skip ping — treat host as online | **Use this on HTB when nmap says host is down but you know it's up** |
| `-PE` | ICMP echo ping | Default ping method |
| `-PS` | TCP SYN ping | Good when ICMP is blocked |
| `-PA` | TCP ACK ping | Bypasses some firewalls |
| `-n` | No DNS resolution | Speeds up scans |
| `-R` | Always resolve DNS | Force reverse DNS lookup |

> 💡 **HTB Pro Tip:** If nmap comes back with "Host seems down" — your first move is always `-Pn`. The box is up, the ping is just blocked.

---

## 🕐 Timing & Speed

| Flag | Name | When to use |
|---|---|---|
| `-T0` | Paranoid | IDS evasion — extremely slow, real engagements |
| `-T1` | Sneaky | Slow evasion, real engagements |
| `-T2` | Polite | Reduces bandwidth, professional engagements |
| `-T3` | Normal | Default |
| `-T4` | Aggressive | CTF and lab environments — use this as your default |
| `-T5` | Insane | Fastest — unreliable on slow or unstable networks |

> 💡 Pair `-T4` with `--min-rate 5000` on full port scans to cut scan time dramatically.

---

## 🔎 Detection & Fingerprinting

| Flag | What it does |
|---|---|
| `-sV` | Detect service versions on open ports |
| `-sV --version-intensity 9` | Maximum version detection effort |
| `-O` | OS detection — guess the operating system |
| `--osscan-guess` | Aggressive OS guessing when nmap isn't confident |
| `-A` | All of the above plus traceroute |

---

## 🥷 Evasion Flags

These help you stay under the radar on real engagements or evade basic IDS (Intrusion Detection Systems).

| Flag | What it does |
|---|---|
| `-f` | Fragment packets — splits packets into 8-byte chunks |
| `--mtu 16` | Set custom packet size — must be multiple of 8 |
| `-D RND:10` | Decoy scan — masks your real IP with 10 random decoys |
| `--source-port 53` | Spoof source port — firewalls often allow DNS traffic |
| `--data-length 200` | Pad packets with random data to avoid signature detection |
| `--randomize-hosts` | Scan targets in random order |
| `-sN` / `-sF` / `-sX` | Null, FIN, Xmas scans — bypass some stateless firewalls |

---

## 💾 Output Formats

Always save your output. You will refer back to it constantly.

| Flag | What it does |
|---|---|
| `-oN scan.txt` | Normal — human readable |
| `-oX scan.xml` | XML — for importing into Metasploit and other tools |
| `-oG scan.gnmap` | Grepable — easy to filter with grep and awk |
| `-oA scan` | All three formats at once — use this as your default |
| `-v` | Verbose — shows results as they come in |
| `-vv` | Very verbose — even more detail |
| `-vvv` | Maximum verbosity — everything nmap is doing in real time. Most references skip this one |
| `--open` | Only show open ports — cuts noise from filtered/closed |
| `--reason` | Shows exactly why nmap marked each port as open, closed, or filtered — essential for understanding scan results and debugging unexpected output. More useful in real-world engagements than CTF but worth knowing either way |

---

## 🏆 Top 20 Ports — Know These Cold

These are the ports nmap scans when you use `--top-ports 20`. If you see any of these open, you have something to work with.

| Port | Service | Why it matters |
|---|---|---|
| 21 | FTP | File transfer — try anonymous login |
| 22 | SSH | Remote access — check version for CVEs |
| 23 | Telnet | Unencrypted remote access |
| 25 | SMTP | Email — check for user enumeration |
| 53 | DNS | Zone transfers, subdomain brute forcing |
| 80 | HTTP | Web app — enumerate everything |
| 110 | POP3 | Email retrieval |
| 111 | RPCBind | RPC services — run rpcinfo |
| 135 | MSRPC | Windows RPC |
| 139 | NetBIOS | Windows file sharing |
| 143 | IMAP | Email |
| 443 | HTTPS | Web app over SSL |
| 445 | SMB | Windows shares — high value target |
| 993 | IMAPS | Encrypted IMAP |
| 995 | POP3S | Encrypted POP3 |
| 1723 | PPTP | VPN |
| 3306 | MySQL | Database — try default creds |
| 3389 | RDP | Windows Remote Desktop |
| 5900 | VNC | Remote desktop |
| 8080 | HTTP-alt | Secondary web server |

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*