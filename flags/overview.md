# 🚩 Flag Reference — Overview

Nmap flags are options you add to your command to control *how* the scan runs — how fast, how loud, what to look for, and what to save.

Think of it like ordering at a restaurant. `nmap <target>` is just "I'll have something." The flags are how you say "medium rare, no onions, extra sauce."

---

## 📂 Flag Categories

| Category | File | What it covers |
|---|---|---|
| 🎯 Target Specification | `targets.md` | How to tell nmap what to scan |
| ⚡ Scan Techniques | `techniques.md` | SYN, TCP, UDP, ping sweeps |
| 🕐 Timing & Speed | `timing.md` | T0 through T5 — stealth vs speed |
| 🔒 Port Specification | `ports.md` | Single ports, ranges, all ports |
| 🔎 Detection | `detection.md` | Version detection, OS fingerprinting |
| 📜 Scripts | `scripts.md` | NSE — nmap's built-in script engine |
| 💾 Output | `output.md` | Saving your results |
| 🥷 Evasion | `evasion.md` | Staying under the radar |

---

## 🔑 The Most Important Flags to Know First

If you are just getting started, these are the ones you will use on almost every scan:

| Flag | What it does |
|---|---|
| `-sV` | Detects what software and version is running on open ports |
| `-sC` | Runs default scripts — quick automated checks |
| `-p-` | Scans all 65535 ports instead of just the top 1000 |
| `-oN` | Saves output to a file you can read later |
| `-T4` | Speeds things up — good for CTF and lab environments |

Combined, these give you the workhorse scan:
```bash
nmap -sV -sC -oN scan.txt <target>
```

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*