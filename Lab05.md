# Lab 01: Week 2 - Automating Detection with Suricata

## Overview
This lab demonstrates the transition from manual log analysis to **Automated Detection Engineering**. By configuring the Suricata IDS (Intrusion Detection System), I established real-time monitoring to alert on unauthorized reconnaissance, spoofed headers, and command-and-control (C2) activity.

---

## Environment Configuration
* **IDS Engine:** Suricata v8.0.2
* **Monitoring Interface:** Loopback (`lo`) & Ethernet (`eth1`)
* **Network Range:** `172.20.10.0/28`
* **Rule Path:** `/var/lib/suricata/rules/local.rules`

---

## Custom Signature Engineering
I developed and implemented the following custom rules to detect specific phases of an attack:

| SID | Alert Message | Protocol | Logic / Trigger |
| :--- | :--- | :--- | :--- |
| **1000001** | ICMP Ping Discovery Detected | ICMP | Detects basic network sweep/reconnaissance. |
| **1000002** | Malicious User-Agent Detected | HTTP | Identifies spoofed "Trusted-Update-Service" headers. |
| **1000003** | ALARM: Possible Reverse Shell | TCP | Flags outbound connections on common shell port 4444. |
| **1000004** | Unauthorized Name Discovery | TCP | Uses Deep Packet Inspection (DPI) to find "kevin" in raw payloads. |

---

## Live Fire Evidence (fast.log)
The following alerts were successfully triggered and captured in the Suricata `fast.log`:
<img width="809" height="529" alt="Screenshot_20251223_125705" src="https://github.com/user-attachments/assets/d688cba0-89f6-4347-8be3-926ec6f97ed6" />
```text
12/23/2025-00:12:23.807808 [**] [1:1000001:1] ICMP Ping Discovery Detected [**] [Priority: 3] {ICMP} 172.20.10.5 -> 172.20.10.1
12/23/2025-00:46:42.811723 [**] [1:1000003:2] ALARM: Possible Reverse Shell Activity on Port 4444 [**] [Priority: 3] {TCP} 127.0.0.1:48142 -> 127.0.0.1:4444
12/23/2025-01:04:23.622424 [**] [1:1000004:1] SECURITY ALERT: Unauthorized Name Discovery [**] [Priority: 3] {TCP} 127.0.0.1:51520 -> 127.0.0.1:4444
