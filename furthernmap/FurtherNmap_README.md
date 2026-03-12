# 🌐 Further Nmap — Advanced Network Scanning

> A group presentation created as part of a cybersecurity course, based on the [TryHackMe Further Nmap room](https://tryhackme.com/room/furthernmap).

---

## 📋 About This Project

This presentation covers **Nmap** — the industry-standard network scanning and enumeration tool used by penetration testers, security researchers, and system administrators worldwide. The room takes an in-depth look at scan types, the Nmap Scripting Engine (NSE), output options, and firewall evasion techniques.

**Difficulty:** Easy  
**Estimated Time:** 60 minutes  
**Platform:** [TryHackMe](https://tryhackme.com)  
**Skills Practised:** Port Scanning · Network Enumeration · Service Detection · Firewall Evasion · NSE Scripting

---

## 📂 Files

| File | Description |
|------|-------------|
| `FurtherNmap_Presentation.pptx` | Editable PowerPoint presentation (10 slides) |
| `FurtherNmap_Presentation.pdf` | PDF version for easy viewing |
| `README.md` | This file |

---

## 🗂️ Presentation Outline

| Slide | Topic |
|-------|-------|
| 1 | Title & Overview |
| 2 | What is Nmap? — Ports & Basics |
| 3 | TCP Scan Types (-sT, -sS, -sU) |
| 4 | Special Scan Types (NULL, FIN, Xmas, Ping Sweep) |
| 5 | Key Nmap Switches Reference |
| 6 | NSE — Nmap Scripting Engine |
| 7 | Firewall Evasion Techniques |
| 8 | Practical Scanning Workflow |
| 9 | Common Ports Cheat Sheet |
| 10 | Key Takeaways |

---

## 🧠 Key Concepts Covered

### Ports & Scanning Basics
Every computer has **65,535 ports** — virtual doors that let services communicate. The first 1,024 are "well-known" standard ports.

**Port states:**
- **Open** — A service is actively listening. A potential entry point.
- **Closed** — No service listening, but the port is reachable.
- **Filtered** — A firewall is blocking Nmap from determining the state.

```bash
# Basic Nmap syntax
nmap [scan type] [options] <target IP>
```

---

### TCP Scan Types

| Switch | Name | Description |
|--------|------|-------------|
| `-sT` | TCP Connect | Full 3-way handshake. Loud but works without sudo. |
| `-sS` | SYN / Stealth | Half-open scan — sends RST instead of ACK. Default scan. Requires sudo. |
| `-sU` | UDP Scan | Checks UDP services. Slow — use `--top-ports 20` to speed up. |

**TCP 3-Way Handshake:**
```
SYN  →  SYN/ACK  →  ACK    (full connection — TCP Connect)
SYN  →  SYN/ACK  →  RST    (half-open, stealthy — SYN scan)
```

---

### Special Scan Types

| Switch | Name | How it works |
|--------|------|-------------|
| `-sN` | NULL Scan | No flags set. Open = no response. Closed = RST. |
| `-sF` | FIN Scan | FIN flag only. Open = ignored. Closed = RST. |
| `-sX` | Xmas Scan | FIN + PSH + URG flags. Same rules as NULL/FIN. |
| `-sn` | Ping Sweep | No port scan — just checks which hosts are alive. |

> ⚠️ NULL, FIN, and Xmas scans are **unreliable on Windows** — Microsoft returns RST for all ports.

```bash
# Ping sweep example
nmap -sn 172.16.0.0/16
```

---

### Key Nmap Switches

**Detection:**
```bash
-sV          # Detect service versions
-O           # Detect operating system
-A           # Aggressive: OS + version + scripts + traceroute
```

**Output:**
```bash
-v           # Increase verbosity
-vv          # Verbosity level 2 (recommended)
-oA output   # Save in all 3 formats (.nmap, .xml, .gnmap)
-oN output   # Save in normal format
-oG output   # Save in grepable format
```

**Port Selection:**
```bash
-p 80        # Scan port 80 only
-p 1-1000    # Scan ports 1 to 1000
-p-          # Scan all 65,535 ports
--top-ports 20  # Scan the 20 most common ports
```

**Timing (T0–T5):**
```bash
-T0  # Paranoid — IDS evasion, very slow
-T3  # Normal — default
-T4  # Aggressive — recommended for CTFs
-T5  # Insane — very noisy
```

---

### NSE — Nmap Scripting Engine

NSE scripts are written in **Lua** and stored in `/usr/share/nmap/scripts/`. They extend Nmap into a full recon and exploitation framework.

**Script categories:**

| Category | Purpose |
|----------|---------|
| `safe` | Safe to run — no impact on target |
| `intrusive` | May crash services or trigger alerts |
| `vuln` | Check for known vulnerabilities |
| `exploit` | Attempt to exploit vulnerabilities |
| `auth` | Test for authentication bypass |
| `brute` | Brute-force service logins |
| `discovery` | Query target for more info |

```bash
# Run a specific script
nmap --script=http-title <target>

# Run multiple scripts
nmap --script=smb-enum-shares,smb-enum-users <target>

# Run a whole category
nmap --script=vuln <target>

# Pass arguments to a script
nmap --script=http-put --script-args http-put.url='/path',http-put.file='file.txt' <target>

# Find scripts
ls /usr/share/nmap/scripts/ | grep smb
grep -r "categories" /usr/share/nmap/scripts/*.nse | grep vuln
```

---

### Firewall Evasion Techniques

| Switch | Technique | Use case |
|--------|-----------|---------|
| `-Pn` | Skip host discovery | Windows hosts block ICMP ping — use this to treat them as alive |
| `-f` | Fragment packets | Splits packets so IDS can't match signatures |
| `--mtu <size>` | Set MTU manually | Control fragment size (must be multiple of 8) |
| `--scan-delay <time>` | Add timing delay | Slow scan below IDS rate-detection thresholds |
| `--data-length <n>` | Append random data | Makes each packet a different size — harder to fingerprint |

```bash
# Common evasion combination
nmap -Pn -f --data-length 200 -T2 <target>
```

---

### Practical Scanning Workflow

```bash
# 1. Host discovery — find live hosts
nmap -sn 192.168.1.0/24

# 2. Quick scan — top 1000 ports
nmap -sS --top-ports 1000 -T4 <target>

# 3. Full port scan — all 65,535 ports
nmap -sS -p- -T4 <target>

# 4. Service and OS detection on open ports
nmap -sV -O -sC -p 22,80,443 <target>

# 5. Vulnerability scripts
nmap --script=vuln <target>

# 6. Save everything
nmap -sS -sV -O -oA full_scan <target>
```

---

## 📋 Common Ports Quick Reference

| Port | Service | Protocol | Notes |
|------|---------|----------|-------|
| 21 | FTP | TCP | Check for anonymous login |
| 22 | SSH | TCP | Brute force or key misuse |
| 23 | Telnet | TCP | Unencrypted — intercept credentials |
| 25 | SMTP | TCP | Email sending — user enumeration |
| 53 | DNS | TCP/UDP | Zone transfer attacks |
| 80 | HTTP | TCP | Web app testing |
| 139 | NetBIOS | TCP | Windows file sharing |
| 161 | SNMP | UDP | Community string exploitation |
| 443 | HTTPS | TCP | SSL/TLS configuration checks |
| 445 | SMB | TCP | EternalBlue / MS17-010 |
| 3306 | MySQL | TCP | Remote database access |
| 3389 | RDP | TCP | Windows Remote Desktop brute force |

---

## 💡 Key Takeaways

- **Scan before you attack** — Nmap is the first step in every pentest
- **SYN scan (`-sS`) is the default** — faster, stealthier, requires root
- **UDP is slow** — run on top ports only unless you have a reason to go deeper
- **NSE unlocks massive power** — go beyond scanning into exploitation
- **Always save results** — `nmap -oA` saves time and creates a paper trail for reports
- **Firewall evasion is built in** — `-Pn`, `-f`, `--data-length` are your first tools

---

## ⚖️ Ethical & Legal Use

Nmap is a powerful tool that **must only be used on networks and systems you have explicit permission to scan**. Unauthorised port scanning may be illegal in your country.

- Always obtain written permission before scanning
- Use timing flags (`-T1`, `-T2`) to minimise impact on production systems
- Never run `--script=exploit` or `intrusive` scripts without clear authorisation

---

## 🔗 Resources

- 🔒 [TryHackMe Further Nmap Room](https://tryhackme.com/room/furthernmap)
- 🌐 [Official Nmap Website](https://nmap.org)
- 📖 [Nmap Reference Guide](https://nmap.org/book/man.html)
- 📜 [NSE Script Database](https://nmap.org/nsedoc/)
- 🛠️ [SecLists — Wordlists for brute force](https://github.com/danielmiessler/SecLists)

---

## 📌 Credits

- **Room Author:** TryHackMe  
- **Presentation:** Created for a cybersecurity group course project

---

*This project is for educational purposes only.*
