# 🕵️ OhSINT — Open Source Intelligence Challenge

> A group presentation created as part of a cybersecurity course, based on the [TryHackMe OhSINT room](https://tryhackme.com/room/ohsint).

---

## 📋 About This Project

This presentation covers the **OhSINT** challenge — a beginner-friendly OSINT room on TryHackMe where you are given a single image file and tasked with uncovering as much information as possible about the person behind it using only open-source intelligence techniques.

**Difficulty:** Easy  
**Estimated Time:** 60 minutes  
**Platform:** [TryHackMe](https://tryhackme.com)  
**Skills Practised:** OSINT · Metadata Analysis · Reconnaissance · WiFi Geolocation · Web Enumeration

---

## 📂 Files

| File | Description |
|------|-------------|
| `OhSINT_Presentation.pptx` | Editable PowerPoint presentation (10 slides) |
| `OhSINT_Presentation.pdf` | PDF version for easy viewing |
| `README.md` | This file |

---

## 🗂️ Presentation Outline

| Slide | Topic |
|-------|-------|
| 1 | Title & Overview |
| 2 | What is OSINT? |
| 3 | The Challenge — One Image File |
| 4 | Step 1 — EXIF Metadata with ExifTool |
| 5 | Step 2 — Tracing the Digital Footprint |
| 6 | Step 3 — WiFi Geolocation with Wigle.net |
| 7 | Step 4 — Finding Hidden Data in Page Source |
| 8 | OSINT Tools Reference |
| 9 | Key OSINT Concepts & Takeaways |
| 10 | The Investigation — Solved |

---

## 🧠 Key Concepts Covered

### What is OSINT?
Open Source Intelligence (OSINT) is the process of gathering and analysing **publicly available information** to build intelligence on a target. It is used in:
- Security assessments
- Penetration testing reconnaissance
- Digital forensics
- Threat intelligence

### The Challenge
You are given a single file: `WindowsXP.jpg`

At first glance it looks completely ordinary — but in OSINT, the details are often hidden beneath the surface. The goal is to answer 7 questions using only this image and publicly available resources.

**The 7 Questions:**
1. What is this user's avatar of?
2. What city is this person in?
3. What is the SSID of the WAP they connected to?
4. What is their personal email address?
5. What site did you find the email address on?
6. Where have they gone on holiday?
7. What is the person's password?

### Investigation Flow
```
Image → Metadata → Username
Username → Twitter → Avatar + BSSID
Username → GitHub → City + Email
BSSID → Wigle.net → WiFi SSID
Username → Blog → Holiday + Hidden Password
```

---

## 🛠️ Tools Used

| Tool | Purpose | Link |
|------|---------|------|
| **ExifTool** | Extract EXIF metadata from image files | [exiftool.org](https://exiftool.org) |
| **Google** | Search username across platforms | [google.com](https://google.com) |
| **Twitter / X** | Find social profile, avatar, posted BSSID | [twitter.com](https://twitter.com) |
| **GitHub** | Find city location and email address | [github.com](https://github.com) |
| **Wigle.net** | Geolocate WiFi network using BSSID | [wigle.net](https://wigle.net) |
| **Browser Dev Tools** | Inspect HTML source for hidden content | F12 / Right-click → View Page Source |

---

## 🔬 Step-by-Step Methodology

### Step 1 — EXIF Metadata
Run ExifTool on the provided image:

```bash
exiftool WindowsXP.jpg
```

Key findings:
```
GPS Latitude   : 54 deg 17' 41.27" N
GPS Longitude  : 2 deg 15' 1.33" W
Copyright      : OWoodflint
```

The copyright field reveals a username: **OWoodflint**

### Step 2 — Digital Footprint
Searching `OWoodflint` on Google reveals three linked profiles:

- **Twitter/X** → Profile avatar is a cat 🐱; user publicly posted their WiFi BSSID
- **GitHub** → Location listed as London; personal email visible in profile  
- **WordPress Blog** → Holiday destination mentioned; password hidden in page HTML

### Step 3 — WiFi Geolocation
The Twitter post contained: `BSSID: B4:5D:50:AA:86:41`

Using [Wigle.net](https://wigle.net) Advanced Search with this BSSID reveals:
- **SSID: UnileverWiFi** — the name of the WiFi network they connected to

### Step 4 — Hidden Page Data
On the WordPress blog, the password was hidden using white text on a white background — invisible to the eye but visible in HTML source:

```html
<p style="color:#ffffff;" class="has-text-color">pennYDr0pper.!</p>
```

Right-click → View Page Source reveals the hidden content.

---

## 💡 Key Takeaways

| Concept | Lesson |
|---------|--------|
| **Metadata tells stories** | EXIF data in images can contain GPS, author name, timestamps — always check |
| **Usernames create trails** | The same username across platforms links multiple data sources together |
| **Oversharing is dangerous** | Posting a WiFi BSSID publicly lets strangers pinpoint your location |
| **Look beyond the surface** | Passwords can be hidden in HTML with invisible styling — inspect source code |
| **Information compounds** | One clue leads to the next: username → profiles → location → email → password |

---

## ⚖️ Ethical & Legal Use

OSINT on publicly available data is legal — but using it to stalk, harass, or target individuals crosses a legal and ethical line. Always:

- Obtain written permission before performing OSINT on a third party
- Use OSINT skills defensively — to protect yourself and your organisation
- Research the computer crime laws in your country before practising

---

## 🔗 Resources

- 🔒 [TryHackMe OhSINT Room](https://tryhackme.com/room/ohsint)
- 🔬 [ExifTool by Phil Harvey](https://exiftool.org)
- 📡 [Wigle.net WiFi Database](https://wigle.net)
- 🧰 [OSINT Framework](https://osintframework.com)

---

## 📌 Credits

- **Room Author:** TryHackMe & SecurityNomad  
- **Presentation:** Created for a cybersecurity group course project

---

*This project is for educational purposes only.*
