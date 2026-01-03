## Lab 05: Incident Response Simulation

### 1. Identification
* **Detected:** Malicious-Bot-Scanner User-Agent via Suricata/ELK.
* **Source IP:** 127.0.0.1 (Internal Simulation).

### 2. Analysis
* **Wireshark Findings:** Captured an attempt to access `captured_hashes.b64`. 
* **Impact:** The attacker successfully received a 200 OK response, indicating potential data exposure.

### 3. Containment
* **Firewall:** Applied UFW rules to restrict incoming traffic.
* **Intrusion Prevention:** Fail2Ban successfully identified and isolated the brute-force attempt.

### 4. Eradication & Recovery
* **Action:** Rotated all passwords and updated the Suricata ruleset to include the new scanner signature.
* **Status:** System Hardened; Monitoring ongoing.
