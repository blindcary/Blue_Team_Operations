# Security Controls Compliance Mapping
**Environment:** Debian 12 Server
**Last Updated:** [Date]
**Purpose:** Map technical controls to regulatory requirements

## Control Matrix

| Security Control | Technical Implementation | ISO 27001 | NIST CSF | Bank of Uganda | PCI-DSS | Evidence Location |
|-----------------|-------------------------|-----------|----------|----------------|---------|-------------------|
| **Access Control** |
| SSH Key Authentication | /etc/ssh/sshd_config | A.9.4.2 | PR.AC-1 | Ch.5 Sec.3.1 | 8.2.1 | /var/log/auth.log |
| Fail2ban IPS | /etc/fail2ban/jail.local | A.9.4.4 | PR.AC-7 | Ch.5 Sec.3.4 | 8.1.6 | /var/log/fail2ban.log |
| Firewall (nftables) | /etc/nftables.conf | A.13.1.1 | PR.AC-5 | Ch.8 Sec.2.1 | 1.2.1 | `nft list ruleset` |
| Account Lockout | fail2ban maxretry=3 | A.9.4.2 | PR.AC-7 | Ch.5 Sec.3.4 | 8.1.7 | /var/log/fail2ban.log |
| **Logging & Monitoring** |
| Audit Logging (auditd) | /etc/audit/rules.d/ | A.12.4.1 | DE.AE-3 | Ch.9 Sec.2 | 10.2 | /var/log/audit/audit.log |
| Log Retention (1 year) | logrotate config | A.12.4.2 | DE.AE-5 | Ch.9 Sec.3 | 10.7 | /etc/logrotate.d/ |
| Centralized Logging | rsyslog/Wazuh | A.12.4.1 | DE.CM-1 | Ch.9 Sec.2.1 | 10.5.3 | Wazuh dashboard |
| **Cryptography** |
| SSH Encryption | Strong ciphers only | A.10.1.1 | PR.DS-2 | Ch.8 Sec.4 | 4.1 | /etc/ssh/sshd_config |
| TLS 1.3 (nginx) | nginx SSL config | A.13.1.1 | PR.DS-2 | Ch.8 Sec.4.1 | 4.1 | /etc/nginx/nginx.conf |
| Certificate Management | Let's Encrypt/certbot | A.14.1.2 | PR.DS-5 | Ch.8 Sec.4.2 | 4.1.1 | /etc/letsencrypt/ |
| **Network Security** |
| VPN (WireGuard) | /etc/wireguard/wg0.conf | A.13.1.1 | PR.AC-5 | Ch.8 Sec.2.3 | 1.3 | `wg show` |
| Port Restriction | nftables rules | A.13.1.3 | PR.PT-4 | Ch.8 Sec.2.1 | 1.2.1 | `nft list ruleset` |
| **System Hardening** |
| Minimal Services | systemctl list-units | A.12.6.2 | PR.IP-1 | Ch.6 Sec.2 | 2.2.2 | `ss -tlnp` |
| Security Updates | unattended-upgrades | A.12.6.1 | PR.IP-12 | Ch.6 Sec.3 | 6.2 | /var/log/unattended-upgrades/ |
| File Integrity | AIDE/Tripwire | A.12.4.1 | DE.CM-7 | Ch.9 Sec.4 | 11.5 | /var/lib/aide/ |
| **Incident Response** |
| Security Monitoring | Wazuh SIEM | A.16.1.2 | DE.CM-1 | Ch.10 Sec.2 | 10.6 | Wazuh alerts |
| Automated Response | fail2ban actions | A.16.1.5 | RS.MI-3 | Ch.10 Sec.3 | 10.8 | /var/log/fail2ban.log |

## ISO 27001 Annex A Control Details

### A.9.4.2 - Secure Log-on Procedures
**Requirement:** Access to systems shall be controlled by a secure log-on procedure.

**Our Implementation:**
- SSH key-based authentication (stronger than passwords)
- No root login permitted
- fail2ban blocks brute force attempts
- Session timeouts configured
- All login attempts logged

**Evidence Files:**
- `/etc/ssh/sshd_config`: PasswordAuthentication no, PubkeyAuthentication yes
- `/var/log/auth.log`: All authentication attempts
- `/etc/fail2ban/jail.local`: Brute force protection

**Gap Analysis:** ✓ Compliant

---

### A.12.4.1 - Event Logging
**Requirement:** Event logs recording user activities, exceptions, faults and information security events shall be produced, kept and regularly reviewed.

**Our Implementation:**
- auditd captures all security-relevant events
- rsyslog centralizes logs
- Wazuh provides automated analysis
- Logs retained for 1 year (exceeds 6-month minimum)

**Evidence Files:**
- `/etc/audit/rules.d/fintech.rules`: Audit configuration
- `/var/log/audit/audit.log`: Audit trail
- Wazuh dashboard: Automated monitoring

**Gap Analysis:** ✓ Compliant

---

### A.13.1.1 - Network Controls
**Requirement:** Networks shall be managed and controlled to protect information in systems and applications.

**Our Implementation:**
- nftables firewall with default-deny policy
- VPN required for remote access
- Only essential ports exposed
- Network traffic logged

**Evidence Files:**
- `/etc/nftables.conf`: Firewall rules
- `/etc/wireguard/wg0.conf`: VPN configuration
- Network flow logs

**Gap Analysis:** ✓ Compliant

---

## NIST Cybersecurity Framework Mapping

### Function: PROTECT (PR)

**PR.AC-1:** Identities and credentials are issued, managed, verified, revoked, and audited for authorized devices, users and processes.
- **Controls:** SSH key management, auditd monitoring
- **Evidence:** `/home/*/.ssh/authorized_keys`, audit logs

**PR.AC-5:** Network integrity is protected (e.g., network segregation, network segmentation).
- **Controls:** nftables firewall, VPN segmentation
- **Evidence:** Network diagrams, firewall rules

**PR.AC-7:** Users, devices, and other assets are authenticated.
- **Controls:** SSH keys, fail2ban
- **Evidence:** auth.log, fail2ban.log

### Function: DETECT (DE)

**DE.AE-3:** Event data are collected and correlated from multiple sources and sensors.
- **Controls:** auditd, rsyslog, Wazuh SIEM
- **Evidence:** Centralized log repository

**DE.CM-1:** The network is monitored to detect potential cybersecurity events.
- **Controls:** Wazuh, firewall logs, IDS (future)
- **Evidence:** SIEM dashboard, alerts

---

## Bank of Uganda IT Risk Management Guidelines Mapping

### Chapter 5: Access Control

**Section 5.3.1 - User Access Management**
**Requirement:** Formal procedures to control user access rights to information systems.

**Our Implementation:**
- SSH key-based authentication
- Principle of least privilege
- Regular access reviews (monthly)

**Evidence:** User access matrix, audit logs

**Section 5.3.4 - Password Management**
**Requirement:** Password management systems shall enforce strong passwords.

**Our Implementation:**
- SSH keys (stronger than passwords)
- When passwords used: minimum 12 characters, complexity requirements
- Account lockout after 3 failed attempts (fail2ban)

**Evidence:** `/etc/pam.d/common-password`, fail2ban config

### Chapter 8: Network Security

**Section 8.2.1 - Network Security Controls**
**Requirement:** Networks shall be segregated and controlled through firewalls.

**Our Implementation:**
- nftables firewall with default-deny
- VPN for remote access
- DMZ for public-facing services (if applicable)

**Evidence:** Network diagram, firewall rules

**Section 8.4 - Cryptographic Controls**
**Requirement:** Use of encryption for data in transit and at rest.

**Our Implementation:**
- TLS 1.3 for web services (nginx)
- SSH v2 with strong ciphers
- WireGuard VPN encryption

**Evidence:** `testssl.sh` scan results, SSH config

### Chapter 9: Logging and Monitoring

**Section 9.2 - Event Logging**
**Requirement:** Security events shall be logged and retained for minimum 1 year.

**Our Implementation:**
- auditd for system events
- Application logs (nginx, etc.)
- SIEM correlation (Wazuh)
- Automated backups to separate system

**Evidence:** Log retention policy, backup records

---

## PCI-DSS Mapping (If Handling Card Data)

### Requirement 1: Install and maintain a firewall
**1.2.1:** Restrict inbound and outbound traffic to that which is necessary.
- **Control:** nftables default-deny policy
- **Evidence:** `nft list ruleset`

### Requirement 4: Encrypt transmission of cardholder data
**4.1:** Use strong cryptography and security protocols.
- **Control:** TLS 1.3 on nginx, SSH strong ciphers
- **Evidence:** SSL Labs scan results

### Requirement 8: Identify and authenticate access
**8.2.1:** Use strong cryptography to render authentication credentials unreadable.
- **Control:** SSH keys (public key cryptography)
- **Evidence:** `/etc/ssh/sshd_config`

### Requirement 10: Track and monitor all access
**10.2:** Implement automated audit trails.
- **Control:** auditd, Wazuh SIEM
- **Evidence:** Audit logs, SIEM dashboard
