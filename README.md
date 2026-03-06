# Port Scan Detection using Splunk

## Project Overview
This project demonstrates how a Security Operations Center (SOC) analyst can detect a port scanning attack using Splunk. Port scanning is a common reconnaissance technique used by attackers to discover open services on a target system.

In this lab, a port scan is generated using Nmap and the traffic logs are analyzed in Splunk to identify suspicious activity.

---

## Scenario
An attacker attempts to scan multiple ports on a target system to identify open services. The objective is to detect this activity using Splunk log analysis.

---

## Tools Used
- Ubuntu Linux
- Splunk Enterprise
- Nmap
- Terminal

---

## Project Structure


port-scan-detection/
│
├── README.md
├── Port_Scan_Investigation.md
│
├── pcap_files/
│
└── screenshots/
├── 1_logs.png
├── 2_scan_traffic.png
├── 3_port_extraction.png
├── 4_scan_detection.png
└── 5_attacker_ip.png


---

## Attack Simulation

A port scan was simulated using Nmap to generate network traffic logs.

Example command used:


nmap -sS -p 1-1000 127.0.0.1


This command performs a SYN scan across the first 1000 ports.

---

## Splunk Investigation

The logs were imported into Splunk and analyzed using SPL queries.

Example detection query:


index=main source=portscan.log
| rex "→ (?<dest_ip>\d+.\d+.\d+.\d+).* (?<src_port>\d+) → (?<dest_port>\d+)"
| stats dc(dest_port) as ports_scanned by dest_ip
| where ports_scanned > 50


This query identifies hosts scanning a large number of ports.

---

## Evidence

### Logs in Splunk
![Logs](screenshots/1.png)

### Network Traffic
![Traffic](screenshots/2.png)

### Extracted Ports
![Ports](screenshots/3.png)

### Scan Detection
![Detection](screenshots/4.png)

### Attacker IP Identification
![Attacker](screenshots/5.png)

---

## Mitigation Recommendations

To prevent port scanning attacks:

- Implement firewall rules to limit scanning activity
- Use Intrusion Detection Systems (IDS)
- Monitor network traffic continuously
- Block suspicious IP addresses

---

## Learning Outcome

After completing this project, the following skills were demonstrated:

- Attack simulation using Nmap
- Log ingestion in Splunk
- SPL query writing
- Security investigation workflow
- Detection of reconnaissance activity
