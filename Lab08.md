### Lab 04: Host Hardening & Automated Defense

## Overview
In this lab, I transitioned from network-level detection to **Host Hardening**. For physical servers, the operating system itself must be resilient against brute-force attacks and unauthorized access.

## Implementation

### 1. Firewall Configuration (UFW)
I implemented a "Default Deny" policy to minimize the attack surface, allowing only essential management ports.
* **Command:** `sudo ufw default deny incoming`
* **Allowed Ports:** 22/TCP (SSH) and 8080/TCP (Web Analysis).

### 2. Automated Intrusion Prevention (Fail2Ban)
I configured Fail2Ban to monitor `/var/log/auth.log` for brute-force SSH attacks. 

**Key Discovery:** During testing, Fail2Ban correctly identified a self-attack on `127.0.0.1` but declined to ban the IP due to the `ignoreself` protection rule. This is a vital safety mechanism for physical servers to prevent internal service disruption.

<img width="1366" height="402" alt="Screenshot_20260103_003027" src="https://github.com/user-attachments/assets/2443e00a-170c-4487-8a9a-08f8d2376a91" />

*(Figure 1: Fail2Ban logs showing the 'Ignore 127.0.0.1' rule in action, preventing a self-lockout during testing.)*

### 3. Log Summarization (Logwatch)
To maintain daily oversight of server health, I installed Logwatch to generate high-level security summaries, capturing failed login attempts and system configuration changes in a single report.

## Conclusion
Host hardening ensures that even if an attacker bypasses network filters, the individual server remains a difficult target. Understanding local safety rules like `ignoreself` is essential for maintaining high availability in bare-metal environments.
