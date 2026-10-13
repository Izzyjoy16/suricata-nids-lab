# Suricata NIDS Detection Finding

## Objective
Validate Suricata's ability to identify a DNS query using a custom detection signature against a saved packet capture.

## Environment
- OS: Kali Linux
- Suricata: 8.0.7
- Analysis mode: Offline PCAP analysis
- Input: `pcap/dns.cap`
- Custom rule: `local.rules`
- Rule SID: 1000001

## Detection Rule
```text
alert dns any any -> any any (msg:"LAB DNS query for www.example.com"; dns.query; content:"www.example.com"; nocase; sid:1000001; rev:1;)
```

## Observed Result
Suricata processed one PCAP containing 38 packets and generated an alert matching the custom signature.

- Source IP: `192.168.170.8`
- Destination IP: `192.168.170.20`
- Signature: `LAB DNS query for www.example.com`
- Severity: 3
- Action: allowed

## Analysis
The alert demonstrates that Suricata successfully matched a DNS query against a custom signature. The `allowed` action indicates detection without blocking.

The alert alone does not indicate malicious activity. This was a controlled test using a historical sample capture.

## Conclusion
The custom DNS detection rule was successfully validated. The PCAP analysis and generated alert provide evidence of Suricata's basic intrusion detection capabilities.
