# 🗺️ What To Do With Your Results

You ran the scan. Nmap gave you a list of open ports and services. Now what?

This is where a lot of beginners freeze. The scan worked — but the output is just sitting there. Here's how to turn nmap results into actual next steps.

---

## 🧠 The Mindset Shift

Nmap is not the hack. Nmap is the map.

Think of it like being a chef prepping for service. The scan is your mise en place — everything laid out, organized, ready to work with. The actual cooking hasn't started yet. Now you take what you found and you do something with it.

---

## 🔄 The Basic Flow
```
Open Port Found
      ↓
Identify the Service & Version
      ↓
Research the Service
      ↓
Look for Known Vulnerabilities
      ↓
Attempt Exploitation or Deeper Enumeration
      ↓
Document Everything
```

---

## 🎯 Port by Port — What To Do Next

### Port 22 — SSH
- Check the version nmap found against https://exploit-db.com
- If you have credentials — try them
- If you have a private key — try it: `ssh -i id_rsa user@<target>`
- Look for username enumeration vulnerabilities on older versions

### Port 80 / 443 — HTTP / HTTPS
- Open it in your browser first — look at it with your eyes
- Run gobuster or feroxbuster for directory enumeration
- Run Nikto for quick vulnerability check
- Check the SSL certificate on 443 — it often leaks hostnames and domains
- Look at page source — comments, hidden fields, linked files
- Check for robots.txt and sitemap.xml
```bash
gobuster dir -u http://<target> -w /usr/share/wordlists/dirb/common.txt
nikto -h http://<target>
curl http://<target>/robots.txt
```

### Port 445 — SMB
- Run enum4linux for full enumeration
- Check for EternalBlue with nmap script
- List shares and try to access them
```bash
enum4linux -a <target>
smbclient -L //<target>/ -N
nmap --script smb-vuln-ms17-010 -p 445 <target>
```

### Port 21 — FTP
- Always try anonymous login first
- Look for files you can download
- Check if you can upload files
```bash
ftp <target>
# username: anonymous
# password: anything
```

### Port 3306 — MySQL
- Try root with no password
- Try common default credentials
```bash
mysql -u root -h <target>
mysql -u root -p -h <target>
```

### Unknown Port / Unusual Service
- Connect with netcat and grab the banner
- Google the service name and version
- Search exploit-db.com for the exact version
```bash
nc <target> <port>
```

---

## 🔍 Research Workflow

For every service version nmap identifies, run it through these resources:

| Resource | What to look for | URL |
|---|---|---|
| Exploit-DB | Public exploits for the exact version | https://exploit-db.com |
| NVD | CVE details and severity scores | https://nvd.nist.gov |
| GTFOBins | Unix binary abuse for privesc | https://gtfobins.github.io |
| HackTricks | Methodology and technique reference | https://book.hacktricks.xyz |

---

## 📋 Documentation Habit

Whether you're in a CTF or a real engagement — document as you go. You will forget what you found and when.
```bash
# Minimum documentation per scan
nmap -sV -sC -oA <machinename>-initial <target>

# Keep a notes file
echo "Port 80 open - Apache 2.4.38 - check for CVEs" >> notes.txt
echo "Port 22 - OpenSSH 7.9 - try default creds" >> notes.txt
```

For real engagements, every finding needs:
- What you found
- How you found it
- What the risk is
- How to fix it

---

## 🌸 Want to Go Deeper?

This reference covers nmap. But nmap is just the beginning.

The full picture — from pre-engagement through reporting, privilege escalation, lateral movement, and beyond — is covered in the **SudoCode Pentesting Methodology Guide**.

Follow along for updates and new tool references as they drop:

- 📸 Instagram: [@sudochef](https://www.instagram.com/sudochef)
- 🎵 TikTok: [@sudochef](https://www.tiktok.com/@sudochef)

More documentation covering additional tools, techniques, and methodology is coming to this GitHub. Stay tuned. 🔪

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*