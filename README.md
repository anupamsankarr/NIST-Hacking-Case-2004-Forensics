# 🔍 NIST CFReDS Hacking Case – Digital Forensics Investigation

> Forensic analysis of a 2004 wireless interception case using Autopsy 4.23.0  
> Course: MACS804 | M.Sc. Applied Cybersecurity

---

## 📌 Overview

This project is a full digital forensic investigation of the **NIST CFReDS Hacking Case (2004)** — a real-world-style dataset involving a suspected wireless network intruder known by the alias **"Mr. Evil"**.

A Dell CPi laptop was found abandoned near public Wi-Fi hotspots. The suspect allegedly used specialized wireless equipment to intercept internet traffic and steal credentials from unsuspecting users.

This investigation reconstructs the activity on the device using forensic tools, recovering deleted files, analyzing artifacts, and building a timeline of events.

---

## 🧰 Tools Used

| Tool | Purpose |
|------|---------|
| Autopsy 4.23.0 | Primary forensic analysis platform |
| FTK Imager | Disk image verification and integrity checking |
| Surfshark AV | Scanning carved executables for malware |
| PhotoRec Carver (via Autopsy) | Recovering files from unallocated space |

---

## 🗂️ Disk Image Specification

| Property | Value |
|----------|-------|
| Image Format | EnCase (.E01) |
| File System | NTFS |
| Operating System | Windows XP |
| Primary User | Greg Schardt ("Mr. Evil") |
| MD5 Hash | `aee4fcd9301c03b3b054623ca261959a` |

> 📥 Dataset available from NIST CFReDS:  
> https://cfreds.nist.gov/

---

## 🔬 Investigation Steps

### 1. Evidence Ingestion
- Loaded the `.E01` disk image into Autopsy
- Disabled Plaso module to speed up initial triage (run separately in production)
- Verified image integrity via MD5 hash

### 2. File System Analysis
- Explored full directory tree from root
- Found a **hidden folder** in the user profile not linked to any standard Windows application
- Identified multiple files in `Temp/` created/modified within a 90-minute window
- Autopsy flagged **7,145 deleted files** recoverable from unallocated space

### 3. Artifact Examination
- Reviewed **Recent Documents** and **Run Programs** artifacts
- Found shortcut pointing to `D:\Drivers\Anonymizer` — anonymizing software
- Found artifacts linked to **GhostWare** (accessed ~20 August 2004)
- Hex pane confirmed paths and metadata in shortcut files

### 4. Keyword Search
- Searched: `john`, `password`, `metasploit`
- `john` returned **357 hits** — includes source files `cracker.c`, `best.c` (John the Ripper) inside `My Documents`
- `password` returned ~**1,900 hits** concentrated in the user profile

### 5. File Export & Malware Scan
- Exported suspicious executables for external analysis
- PhotoRec Carver recovered deleted `.exe` files from unallocated space
- **4 files flagged** by antivirus — one confirmed **TR/W32.Trojan**

### 6. Timeline Analysis
- OS installed: **19 August 2004**
- Last shutdown: **27 August 2004**
- ~8-day active window with concentrated tool installation and use
- Timeline showed heavy activity in `My Documents` folder
- Tools found: **Pandora, Cain & Abel, fping, NetStumbler, Ethereal**

---

## 🚨 Key Findings

| Finding | Detail |
|---------|--------|
| Hacking Tools | NetStumbler, Ethereal, Cain & Abel, 123 Write All Stored Passwords |
| Password Cracking | John the Ripper source files in My Documents |
| Malware | 4 trojans recovered from unallocated space (TR/W32.Trojan) |
| Anonymizing Software | Anonymizer found in Recent Documents |
| Network Capture | Interception file with captured HTTP traffic present |
| Web History | WinPcap download confirmed in browser history |
| Deleted Files | 7,145 deleted files on volume |
| Activity Window | All suspicious activity: 19–27 August 2004, attributed to "Mr. Evil" account |

---

## 📊 Autopsy – Strengths & Limitations

**Strengths**
- Beginner-friendly GUI — no deep CLI knowledge needed
- Combines file carving, keyword search, timeline, and web history in one tool
- Auto-flags deleted files and unallocated space
- Timeline view clearly shows when suspicious activity occurred

**Limitations**
- Slow on large disk images
- Carved files lose original filenames — manual identification needed
- AV integration requires third-party tools
- Some artifacts need manual cross-referencing

---

## ✅ Conclusion

The forensic examination confirmed that the Dell laptop was a **purpose-built wireless interception platform**. The combination of network scanning tools, packet capture software, password cracking code, trojan executables, and browser history pointing to hacking resources clearly establishes the machine's intended use — all within an 8-day window after OS installation.

---

## 📁 Repository Structure

```
hacking-case-forensics/
├── README.md               ← This file
├── report/
│   └── MACS804_Assessment2_ForensicReport.pdf
├── notes/
│   ├── methodology.md      ← Step-by-step investigation notes
│   ├── tools-reference.md  ← Quick reference for tools used
│   └── findings-summary.md ← All key findings in one place
└── screenshots/            ← Add your Autopsy screenshots here
```

---

## ⚠️ Disclaimer

This project is for **educational purposes only** as part of an academic cybersecurity program. The dataset is publicly available via NIST CFReDS. No actual systems were compromised.
