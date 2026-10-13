# Suricata Network Intrusion Detection Lab

## Overview
A hands-on cybersecurity project using Suricata 8.0.7 to analyze a saved packet capture and validate a custom DNS detection rule.

## Objectives
- Install and validate Suricata on Kali Linux.
- Configure the Emerging Threats Open ruleset.
- Analyze a PCAP offline.
- Create a custom DNS detection signature.
- Review EVE JSON alerts using `jq`.
- Document findings for a cybersecurity portfolio.

## Lab Environment
- Operating system: Kali Linux
- IDS engine: Suricata 8.0.7
- Analysis mode: Offline PCAP
- Sample capture: `pcap/dns.cap`
- Custom rule: `local.rules`

## Custom Detection Rule
```text
alert dns any any -> any any (msg:"LAB DNS query for www.example.com"; dns.query; content:"www.example.com"; nocase; sid:1000001; rev:1;)
```

## Results
Suricata processed 38 packets (3,706 bytes) and generated an alert for a DNS query matching the custom signature.

- Rule SID: `1000001`
- Source IP: `192.168.170.8`
- Destination IP: `192.168.170.20`
- Alert action: `allowed`
- Evidence: `evidence/suricata-dns-alert.png`

The alert confirms signature matching. It does not, by itself, indicate malicious activity. The `allowed` action means the traffic was not blocked.

## Project Structure
- `local.rules` — custom DNS detection signature
- `pcap/dns.cap` — sample packet capture
- `logs/detection/eve.json` — detection event output
- `evidence/suricata-dns-alert.png` — alert screenshot
- `report/detection-finding.md` — finding report

## Reproduce the Test
Run from the project root:

```bash
mkdir -p logs/detection
sudo suricata -r pcap/dns.cap \
  -c /etc/suricata/suricata.yaml \
  -S local.rules \
  -l logs/detection/
```

Inspect the alert:

```bash
sudo jq 'select(.event_type == "alert") |
  {timestamp, src_ip, dest_ip, alert}' logs/detection/eve.json
```

## Skills Demonstrated
Suricata configuration, PCAP analysis, custom IDS signatures, DNS traffic inspection, JSON log analysis, and security finding documentation.

## Limitations
This project uses a historical sample capture and offline analysis. It does not demonstrate live network monitoring, traffic blocking, or proof of a real attack.
