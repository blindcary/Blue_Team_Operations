# Lab 00: Linux System Visibility & "Ground Truth" Logging

## Objective
Establish a baseline for system visibility on a Kali Linux endpoint and identify the digital footprints left by administrative changes (Persistence simulation).

## Tools Used
- `systemd-journald` / `journalctl`
- `useradd` (Simulation tool)

## Methodology
1. **Attack Simulation:** Created a new user account to simulate a common persistence mechanism used by attackers.
   - Command: `sudo useradd -m backup_admin`
2. **Log Discovery:** Attempted to locate the event in legacy flat-file logs (`/var/log/auth.log`).
3. **Pivot to Modern Logging:** Confirmed the absence of legacy log files and pivoted to the **Systemd Journal** to extract binary log data.
   - Command: `journalctl --since "5 minutes ago" | grep "new user"`

## Findings & Evidence
- **Event Type:** User Creation (Persistence)
- **Timestamp:** Dec 18 22:52:41
- **Process ID (PID):** `3415`
- **Target User:** `backup_admin`
- **UID/GID:** `1001/1001`
  <img width="1366" height="768" alt="Screenshot_20251218_230538" src="https://github.com/user-attachments/assets/85f44a0d-f7e4-4750-9ddb-1e1b9833e998" />


## Analyst Notes
Modern Kali Linux prioritizes the binary `journalctl` over traditional text logs. The PID (3415) is the primary anchor for investigating what other actions this process took.
