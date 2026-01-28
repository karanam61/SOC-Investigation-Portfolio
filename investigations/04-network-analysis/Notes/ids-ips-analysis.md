# IDS/IPS Log Analysis

IDS/IPS logs show intrusion attempts and prevention actions.  
Correlated with SIEM to link related events (e.g., port scan → exploit attempt).

## Key Checks
- Attack direction (inbound/outbound)
- Severity level (low → critical)
- Multiple signature triggers between same src/dst
- Service running on target port?
- Was attack blocked or just detected?

## Common Detections
- Port scans
- Vuln scans
- Code injection
- Brute force
- DoS/DDoS
- Trojan/botnet activity

## Example Log

<134>Aug 14 15:22:30 IDS-1 snort[2456]: [1:2010935:2] ET POLICY Possible Outbound SSL Connection to Bad Reputation IP [Classification: Misc Attack] [Priority: 2] {TCP} 192.168.1.50:52344 -> 185.199.110.153:443

### Field-by-Field Description
| Field         | Example Value                         | Meaning |
|---------------|---------------------------------------|---------|
| Syslog Header | <134>                                 | Syslog priority/severity |
| Timestamp     | Aug 14 15:22:30                        | Event time |
| Sensor Name   | IDS-1                                 | IDS hostname |
| Process       | snort[2456]                           | IDS process name and PID |
| Signature     | [1:2010935:2]                         | Rule identifiers |
| Alert Msg     | ET POLICY Possible Outbound SSL...    | Description of detection |
| Classification| Misc Attack                           | Alert classification |
| Priority      | 2                                     | Severity rating |
| Protocol      | TCP                                   | Transport protocol |
| Source IP:Port| 192.168.1.50:52344                     | Origin of traffic |
| Dest IP:Port  | 185.199.110.153:443                    | Target of traffic |

**Interpretation:**  
An IDS alert flagged outbound SSL traffic from `192.168.1.50` to a bad-reputation IP `185.199.110.153` on port 443.

