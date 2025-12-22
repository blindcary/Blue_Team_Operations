# Lab 04: Week 1 Capstone - Independent Analysis

## Overview
This capstone concludes the "System Visibility" module. I demonstrated the ability to monitor file integrity, automate log reporting, detect spoofed network headers, and identify automated "beaconing" behavior via DNS timing analysis.

---

## Exercise 1: Group Reconnaissance (System Auditing)
**Objective:** Identify the kernel-level activity when sensitive files are accessed.

* **Audit Rule:** `sudo auditctl -w /etc/group -p wa -k group_recon`
* **Finding:** * **Executable:** `/usr/bin/cat` (Note: My initial log showed `auditctl` with syscall 44, which was the rule creation itself).
    * **Syscall Number:** `257` (The `openat` system call used to access file data).
* **Analyst Note:** Tracking syscalls ensures that even if an attacker renames a binary (e.g., renaming `cat` to `test`), the kernel still logs the underlying "open" action.

---

## Exercise 2: Unique Executable Report (Log Pipelining)
**Objective:** Use the "Analyst Pipeline" to summarize security events.

* **Command:** `sudo ausearch -k shadow_changes --raw | grep "SYSCALL" | awk -F' ' '{print $28}' | sort | uniq`
* **Finding:** * **Total Unique Executables:** 1 (`exe="/usr/bin/passwd"`)
* **Analyst Note:** In a real production environment, this command allows a SOC analyst to quickly see if unauthorized scripts or tools (like `python` or `perl`) are interacting with password files.

---

## Exercise 3: User-Agent Spoofing (Network Analysis)
**Objective:** Detect tools attempting to bypass security filters by pretending to be trusted services.

* **Command:** `sudo tshark -i eth1 -Y "http.request" -T fields -e http.user_agent`
* **Trigger:** `curl -A "Trusted-Update-Service" http://neverssl.com`
* **Finding:** Captured the string `Trusted-Update-Service`.
* **Analyst Note:** Standard browsers do not use "Update Service" strings. Identifying this in a network log is a high-fidelity indicator of a scripted attack or unauthorized data exfiltration tool.

---

## Exercise 4: DNS Beaconing (Timing Analysis)
**Objective:** Use packet timing to differentiate between human browsing and automated C2 (Command & Control) heartbeats.

* **Methodology:** Simulated a "beacon" using a bash loop with a 5-second sleep interval.
* **Tshark Evidence:**
    * **Timestamp 1:** 27.54s
    * **Timestamp 2:** 32.59s (~5.05s gap)
    * **Timestamp 3:** 37.64s (~5.05s gap)
    * **Timestamp 4:** 42.68s (~5.04s gap)
* **Conclusion:** The mathematical precision (consistent ~5-second gaps) confirms this is a **C2 Beacon**. Humans exhibit "jitter" or irregular gaps, whereas this traffic is clearly automated.

---

## Final Reflections
Week 1 focused on **Visibility**. I learned that if an event isn't logged or captured on the wire, it didn't happen. By mastering `auditd`, `tshark`, and command-line parsing, I can now generate my own "Ground Truth" evidence for any incident.
