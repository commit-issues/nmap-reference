# 🔌 Common Ports Reference

A port is like a numbered door on a building. Every service that runs on a computer listens on a specific port number — web servers on 80, SSH on 22, and so on. When nmap finds an open port, your job is to figure out what's behind that door.

---

## 📋 Common Ports & What To Do

| Port | Service | Protocol | What to do when you find it |
|---|---|---|---|
| 21 | FTP | TCP | Try anonymous or admin login: `ftp <target>` — user: `anonymous` or `admin` pass: anything |
| 22 | SSH | TCP | Check version for CVEs, try creds if you have them |
| 23 | Telnet | TCP | Unencrypted remote access — try connecting: `telnet <target>` |
| 25 | SMTP | TCP | Email server — check for open relay, user enumeration |
| 53 | DNS | TCP/UDP | Try zone transfer: `dig axfr @<target> <domain>` |
| 80 | HTTP | TCP | Open in browser, run Nikto, run gobuster |
| 110 | POP3 | TCP | Email retrieval — try default credentials |
| 111 | RPCBind | TCP/UDP | Run `rpcinfo -p <target>` to enumerate RPC services |
| 135 | MSRPC | TCP | Windows RPC — run `enum4linux` |
| 139 | NetBIOS | TCP | Windows file sharing — run `enum4linux` |
| 143 | IMAP | TCP | Email — try default credentials |
| 443 | HTTPS | TCP | Same as 80 — also check the SSL certificate for hostnames |
| 445 | SMB | TCP | Windows file sharing — run `enum4linux`, check for EternalBlue (MS17-010) |
| 1433 | MSSQL | TCP | Microsoft SQL Server — try `sa` with blank password |
| 1521 | Oracle DB | TCP | Oracle database — try default credentials |
| 2049 | NFS | TCP | Network file share — check for mountable shares: `showmount -e <target>` |
| 3306 | MySQL | TCP | Try: `mysql -u root -h <target>` with no password |
| 3389 | RDP | TCP | Windows Remote Desktop — check for BlueKeep (CVE-2019-0708) |
| 5432 | PostgreSQL | TCP | Try default credentials: `psql -U postgres -h <target>` |
| 5900 | VNC | TCP | Remote desktop — try no password or default password |
| 6379 | Redis | TCP | Often unauthenticated — high value target |
| 8080 | HTTP-alt | TCP | Secondary web server — treat like port 80 |
| 8443 | HTTPS-alt | TCP | Secondary HTTPS — treat like port 443 |
| 27017 | MongoDB | TCP | Often unauthenticated — check for open access |

---

## 💡 Quick Tips

- **Always check HTTP and HTTPS separately** — the content is often different
- **SMB (445) on Windows boxes is almost always worth investigating** — it has a long history of critical vulnerabilities
- **Redis and MongoDB on default ports with no auth** — instant win in CTF and a critical finding in real engagements
- **DNS zone transfers (port 53)** — often misconfigured and reveals the entire internal domain structure

---

*by SudoChef · Part of the SudoCode Pentesting Methodology Guide*