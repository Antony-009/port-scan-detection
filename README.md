# Port Scan Detection using Splunk

## Scenario
An attacker is scanning multiple ports to discover open services.

## Tools Used
- Splunk
- Nmap
- Ubuntu

## Investigation Steps

1. Generate port scan using Nmap
2. Import logs into Splunk
3. Detect abnormal traffic
4. Identify attacker IP
5. Count scanned ports

## Detection Query
index=main source=portscan.log
| rex "→ (?<dest_ip>\d+.\d+.\d+.\d+).* (?<src_port>\d+) → (?<dest_port>\d+)"
| stats dc(dest_port) as ports_scanned by dest_ip


```markdown
## Evidence

### Splunk Logs
![Logs](screenshots/1.png)

### Port Extraction
![Ports](screenshots/2.png)

### Scan Detection
![Scan](screenshots/3.png)

### Timeline
![Timeline](screenshots/4.png)

### Attacker IP
![Attacker](screenshots/5.png)
