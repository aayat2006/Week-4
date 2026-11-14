# Week 4 - SIEM in Practice: Splunk, Wazuh & Log Ingestion

## Overview
This repository contains the complete theoretical notes, TryHackMe lab writeups, hands-on SIEM evidence, custom detection rules, alert triage documentation, and summary reports for **Week 4 (SIEM in Practice)** under the **DG Interns Hub Cybersecurity Track**.

> **Environment & Hardware Note:** Following the assignment hardware guidelines, **Splunk Enterprise** was deployed on an Ubuntu Linux VM (`index=lab`) for live log ingestion, attack simulation, SPL alerting, and dashboard visualization. **Wazuh** was covered through architectural analysis and custom XML rule engineering (`local_rules.xml`).

---

## Folder Structure
```text
Week 4/
├── Notes/                         # 8 separate files answering all theoretical questions
├── Writeups/                      # TryHackMe writeups (Intro to SIEM & Splunk: Exploring SPL)
├── Findings/                      # Hands-on screenshots, attack evidence & 3 Triage Notes
├── Detections/                    # 5 SPL queries (with MITRE mappings) & Wazuh local_rules.xml
├── Cheat Sheet/                   # SPL Command table (15 entries) & SIEM Alert playbook (10 entries)
├── PDF_Report_Week4.pdf           # Consolidated weekly technical report
├── Presentation_Report_Week4.pptx # Executive presentation deck
└── README.md                      # Structured overview of the week's work
```

---

## Key Practical Milestones Completed
1. **Splunk Enterprise Deployment & Ingestion:**
   - Installed Splunk Enterprise on Ubuntu Linux and created a dedicated `lab` index.
   - Configured `/opt/splunk/etc/apps/search/local/inputs.conf` to ingest `/var/log/auth.log` under `sourcetype=syslog`.
2. **Attack Simulation (`Attack to Alert`):**
   - Executed an SSH dictionary brute-force attack using **Hydra** (`hydra -l ubuntu -P pass.txt ssh://127.0.0.1 -t 4`).
   - Executed a TCP SYN stealth scan using **Nmap** (`sudo nmap -sS -p 22,80,443,8000 127.0.0.1`).
   - Extracted raw events and parsed fields (`src_ip`, `process`, `sourcetype`) inside Splunk.
3. **Detection Engineering & Visualization:**
   - Authored **5 SPL detections** mapped to MITRE ATT&CK (`T1110`, `T1059.001`, `T1136.001`, `T1055`).
   - Configured **2 scheduled alerts** with threshold triggers (`count > 5`).
   - Built a **4-panel SOC Monitoring Dashboard** tracking SSH failures by IP, attack timeline, targeted usernames, and auth processes.
   - Wrote custom Wazuh rule `100001` in `local_rules.xml` for SSH brute-force detection.
4. **Tier-1 SOC Alert Triage:**
   - Documented full 5W investigations (Who, What, When, Where, Why), verdicts (True/False Positive), and remediation steps for 3 alerts.

---

## Submission Details
- **Program:** DG Interns Hub — Cybersecurity Internship
- **Submission Window:** November 13 – November 15, 2026 (11:59 PM IST)

