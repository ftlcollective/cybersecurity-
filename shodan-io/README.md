# 🔍 Shodan.io — Cybersecurity Group Presentation

> A group presentation created as part of a cybersecurity course, based on the [TryHackMe Shodan.io room](https://tryhackme.com/room/shodan).

---

## 📋 About This Project

This presentation covers **Shodan.io** — the search engine for internet-connected devices. It was built to explain Shodan in simple terms for a group study session, covering all major concepts from the TryHackMe room.

**Difficulty:** Easy  
**Estimated Time:** 45 minutes  
**Platform:** [TryHackMe](https://tryhackme.com)

---

## 📂 Files

| File | Description |
|------|-------------|
| `Shodan_Presentation.pptx` | Editable PowerPoint presentation (10 slides) |
| `Shodan_Presentation.pdf` | PDF version for easy viewing |
| `README.md` | This file |

---

## 🗂️ Presentation Outline

| Slide | Topic |
|-------|-------|
| 1 | Title & Overview |
| 2 | What is Shodan? |
| 3 | How Shodan Works — Banners |
| 4 | Searching with Filters |
| 5 | Autonomous System Numbers (ASN) |
| 6 | Shodan Dorking |
| 7 | Shodan Monitor |
| 8 | Browser Extension & API |
| 9 | Ethics & Legal Use |
| 10 | Key Takeaways |

---

## 🧠 Key Concepts Covered

### What is Shodan?
Shodan is a search engine that scans and indexes internet-connected devices — not websites. It can find CCTV cameras, web servers, databases, industrial control systems (ICS/SCADA), and more.

### Banners
When Shodan connects to a device's open port, the device responds with a **banner** — metadata about what service is running. Shodan stores and indexes these banners, making them searchable.

### Filters
Shodan's power comes from search filters:

```
asn:AS14061          # Search by Autonomous System Number
product:MySQL        # Find devices running a specific service
port:22              # Find devices with a specific port open
country:ZA           # Filter by country
os:"Windows 10"      # Filter by operating system
vuln:ms17-010        # Find vulnerable IPs (premium only)
```

Filters can be combined: `asn:AS14061 product:MySQL`

### ASN Lookup
Large companies own blocks of IP addresses identified by an ASN. You can:
1. `ping` a domain to get its IP
2. Use [asnlookup.com](https://asnlookup.com) to find its ASN
3. Search Shodan with `asn:AS#####` to find all their exposed services

### Shodan Dorking
Pre-built search queries for finding specific targets:

| Dork | Purpose |
|------|---------|
| `has_screenshot:true encrypted attention` | Find ransomware-infected machines |
| `screenshot.label:ics` | Find industrial control systems |
| `vuln:CVE-2014-0160` | Find Heartbleed-vulnerable machines (premium) |
| `http.favicon.hash:-1776962843` | Track SolarWinds via favicon hash |

### Shodan Monitor
A dashboard at [monitor.shodan.io](https://monitor.shodan.io/dashboard) that continuously scans your own IP ranges and alerts you to:
- Top open ports
- Known vulnerabilities
- Unusual/notable ports
- Notable IPs

### Extension & API
- **Chrome Extension** — Instantly see Shodan data for any website you visit
- **API** — Programmatically query Shodan; write scripts to monitor your organisation's exposure

---

## ⚖️ Ethical & Legal Use

| ✅ Do | ❌ Don't |
|-------|---------|
| Audit your own network | Attack or exploit devices you find |
| Pen test with written permission | Try to break into password-protected systems |
| View publicly accessible services | Access private data without authorisation |
| Report vulnerabilities responsibly | Scan networks you don't own |

> **Always research the computer crime laws in your country before using Shodan.**

---

## 🔗 Resources

- 🔒 [TryHackMe Shodan Room](https://tryhackme.com/room/shodan) — Original room by [BeeSec](https://twitter.com/bee_sec_san)
- 🌐 [Shodan.io](https://shodan.io) — The tool itself
- 📡 [Shodan Monitor](https://monitor.shodan.io/dashboard)
- 🧩 [Shodan Chrome Extension](https://chrome.google.com/webstore/detail/shodan/jjalcfnidlmpjhdfepjhjbhnhkbgleap)
- 📖 [Shodan API Docs](https://developer.shodan.io)
- 📝 [BeeSec Blog Post](https://skerritt.blog/shodan)

---

## 📌 Credits

- **Room Author:** [BeeSec](https://twitter.com/bee_sec_san) & contributors on TryHackMe
- **Presentation:** Created for a cybersecurity group course project

---

*This project is for educational purposes only.*
