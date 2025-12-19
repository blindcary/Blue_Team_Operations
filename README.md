# Blue Team Operations & SOC Analysis Training

## Overview
This repository serves as a professional lab journal for my journey into **Blue Team Cybersecurity**. Under the mentorship of a senior SOC analyst, I am learning to move beyond theory and into hands-on defensive operations, focusing on visibility, detection engineering, and incident response.

**Goal:** Achieve competency in SOC Analyst workflows and technical toolsets by **January 6, 2025**.

---

## Learning Roadmap

### Week 1: Foundations & System Visibility (Current)
* [x] **Linux Logging Fundamentals:** Understanding `systemd-journald` and `journalctl`.
* [x] **Endpoint Auditing:** Implementing file integrity monitoring with `auditd`.
* [ ] **Log Analysis at Scale:** Mastering the "Analyst Pipeline" (`grep`, `awk`, `sed`).
* [ ] **Network Traffic Analysis:** Basics of Wireshark and PCAP analysis.

### Week 2: Detection & Strategy
* [ ] **MITRE ATT&CK Mapping:** Learning how to categorize attacker behavior.
* [ ] **Intrusion Detection:** Introduction to Zeek and Suricata.
* [ ] **Detection Engineering:** Writing basic Sigma or YARA rules.

### Week 3: Incident Response & SIEM logic
* [ ] **SIEM Fundamentals:** Understanding how logs aggregate into alerts.
* [ ] **The IR Lifecycle:** Preparation, Identification, Containment, and Eradication.
* [ ] **Final Capstone:** Handling a simulated multi-stage breach.

---

## Toolset Mastery
| Category | Tools |
| :--- | :--- |
| **Log Analysis** | `journalctl`, `auditd`, `grep`, `awk` |
| **Network** | Wireshark (Upcoming) |
| **EDR/Endpoint** | Sysmon for Linux (Upcoming) |
| **Frameworks** | MITRE ATT&CK, NIST 800-61 |

---

## Completed Labs
* [Lab 00: Linux System Visibility & "Ground Truth" Logging](./Lab00_Linux_Logging.md) - *Simulated persistence via user creation.*
* [Lab 01: Linux System Auditing & File Monitoring](./Lab01_Linux_Auditing.md) - *Monitoring /etc/shadow for unauthorized access.*
* [Lab 02: Log Analysis at Scale](./Lab02_Log_Analysis.md) - *Transforming raw audit data into actionable reports.*

---

## 📝 Key Takeaways
- **AUID Matters:** Even if an attacker uses `sudo`, the Audit User ID (`auid`) preserves their original identity for non-repudiation.
- **Visibility is King:** If an event isn't logged, it didn't happen. A SOC analyst's first job is ensuring visibility is active.
