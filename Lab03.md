# Lab 03: Network Traffic Analysis (tshark)

## Objective
Analyze live network traffic via the command line to identify source identities and browser fingerprints.

## Tools Used
- `tshark` (Terminal-based Wireshark)

## Methodology
1. **Pivot:** Due to a GUI dissector bug in Wireshark, transitioned to `tshark` for data collection.
2. **Command Execution:**
   `sudo tshark -i eth1 -Y "http.request" -T fields -e frame.time -e ip.src -e http.user_agent`
   - `-i eth1`: Targeted the active network interface.
   - `-Y "http.request"`: Filtered for only outgoing web requests.
   - `-T fields`: Formatted the output for readability.

## Findings
- **Source IP Identified:** `172.20.10.5`
- **User-Agent:** `Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0`
- **Observation:** The traffic was unencrypted (HTTP), allowing for full visibility into the headers.
<img width="1366" height="371" alt="Screenshot_20251220_194249" src="https://github.com/user-attachments/assets/50d9e134-8b52-4d0b-a36e-62708d28d3a6" />

## Conclusion
Command-line packet analysis is a critical skill for environments where a GUI is unavailable or broken. By extracting the User-Agent, I can differentiate between human browser activity and automated attack scripts.
