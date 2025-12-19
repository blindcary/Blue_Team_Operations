# Lab 01: Linux System Visibility & File Integrity Monitoring

## Objective
Detect unauthorized modifications to sensitive system files on a Linux (Kali) endpoint using native auditing tools.

## Tools Used
- `auditd` (Linux Audit Daemon)
- `ausearch` (Audit Log Parser)

## Methodology
1. **Rule Creation:** Established a watch on `/etc/shadow` (password file) with the key `shadow_changes`.
   - Command: `sudo auditctl -w /etc/shadow -p wa -k shadow_changes`
2. **Attack Simulation:** Performed a password change for a test user to trigger the rule.
   - Command: `sudo passwd backup_admin`
3. **Forensic Analysis:** Queried the audit logs to identify the actor and the process.

## Findings & Evidence
| Field | Value | Significance |
| :--- | :--- | :--- |
| **Timestamp** | Thu Dec 18 23:15:10 2025 | Established the exact timeline of the event. |
| **Executable (exe)** | `/usr/bin/passwd` | Confirmed the standard password utility was used. |
| **AUID (Audit ID)** | `1000` | Identified the original user (kevin) despite using `sudo`. |
<img width="1407" height="496" alt="Screenshot_20251218_231849" src="https://github.com/user-attachments/assets/72dc1934-27e3-4832-8b4c-b1704be34904" />


## Conclusion
The audit daemon successfully captured the "AUID," which is critical for non-repudiation. Even if a user gains root privileges, their original login identity remains tied to the action.
