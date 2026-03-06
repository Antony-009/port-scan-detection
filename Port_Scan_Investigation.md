# Port Scan Investigation Report

## 1. Objective

The objective of this investigation is to detect and analyze a port scanning attack using Splunk logs.

Port scanning is a reconnaissance technique used by attackers to identify open ports and services on a target system.

---

# 2. Environment Setup

System Used

Ubuntu Linux Virtual Machine

Tools Installed

Splunk Enterprise  
Nmap

---

# 3. Installing Required Tools

## Install Nmap


sudo apt update
sudo apt install nmap


Verify installation


nmap --version


---

## Install Splunk

Download Splunk from the official website.

Extract the file.


tar -xvzf splunk-9.x.x-linux.tgz


Move Splunk to the opt directory.


sudo mv splunk /opt/


Start Splunk


cd /opt/splunk/bin
sudo ./splunk start --run-as-root


Open Splunk in browser


http://localhost:8000


---

# 4. Generating Port Scan Traffic

To simulate an attacker, a port scan was executed using Nmap.

Command used:


nmap -sS -p 1-1000 127.0.0.1


Explanation

-sS = SYN scan  
-p = port range  
127.0.0.1 = target system

This command scans the first 1000 ports.

---

# 5. Log Collection

Network logs were saved into a file called


portscan.log


This log file was then uploaded into Splunk for analysis.

---

# 6. Log Analysis in Splunk

Search Query


index=main source=portscan.log


This query retrieves all network logs related to the port scan.

Screenshot:

![Logs](screenshots/1.png)

---

# 7. Extracting Port Information

The destination port was extracted using a regular expression.


| rex "→ (?<dest_ip>\d+.\d+.\d+.\d+).* (?<src_port>\d+) → (?<dest_port>\d+)"


Screenshot:

![Extraction](screenshots/3.png)

---

# 8. Detecting Port Scanning Activity

To identify scanning behavior, the number of unique ports accessed was counted.

Detection Query


index=main source=portscan.log
| rex "→ (?<dest_ip>\d+.\d+.\d+.\d+).* (?<src_port>\d+) → (?<dest_port>\d+)"
| stats dc(dest_port) as ports_scanned by dest_ip
| where ports_scanned > 50


Screenshot:

![Detection](screenshots/4.png)

---

# 9. Identifying Attacker IP

The source IP responsible for scanning multiple ports was identified.

Example:


127.0.0.1


Screenshot:

![Attacker](screenshots/5.png)

---

# 10. Timeline Analysis

Splunk timeline visualization shows a spike in network traffic during the scan.

Screenshot:

![Timeline](screenshots/2.png)

---

# 11. Conclusion

The investigation successfully identified port scanning activity using Splunk.

Key findings

- Multiple ports were scanned within a short time
- The scanning activity was detected using SPL queries
- The attacker IP address was identified

---

# 12. Mitigation

Recommended defensive actions

- Implement firewall rules
- Enable intrusion detection systems
- Monitor abnormal network activity
- Block malicious IP addresses
