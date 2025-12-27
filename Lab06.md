# Lab 02: Week 2 - SIEM & Log Visualization

## Overview
While Lab 01 focused on **Detection**, Lab 02 focuses on **Operational Visibility**. I implemented an ELK Stack (Elasticsearch, Logstash, Kibana) pipeline to ingest Suricata telemetry and visualize network threats on a centralized SOC dashboard.

## Components
* **Log Shipper:** Filebeat (configured with the Suricata module).
* **Search Engine:** Elasticsearch v8.x.
* **Visualization:** Kibana.

## Dashboard Insights
By integrating Suricata with Kibana, I can now visualize:
* **Top Alerted Signatures:** Instantly identifying the most common threats (e.g., SID 1000002).
* **Source/Destination Heatmaps:** Mapping where internal/external traffic is originating.
* **Flow Trends:** Monitoring spikes in TCP/UDP traffic that may indicate a DDoS attack or data exfiltration.

## Conclusion
Transitioning from raw text logs to a SIEM is the difference between "hunting" and "monitoring." This setup allows for faster incident response times and provides a high-level overview of the network's security posture, essential for professional SOC environments.
