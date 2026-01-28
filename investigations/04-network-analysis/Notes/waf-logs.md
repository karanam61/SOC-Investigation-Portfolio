# WAF Log Analysis

WAFs protect web servers, often with SSL offloading for HTTPS inspection.

## Key Log Fields
- date/time
- type, main_type, sub_type
- severity_level
- proto, service
- action, policy
- src/dst IP & ports
- http_method, http_url, http_host
- msg, signature_subclass
- attack_type

## Process
- Check src/dst
- Validate WAF action (block/alert)
- Review HTTP status codes
- Determine if attack reached server

## Common Detections
- SQLi
- XSS
- Code injection
- Suspicious HTTP methods (PUT, DELETE)

## Example Log
date=2025-08-14 time=14:45:30 type=attack main_type=sql_injection sub_type=select_statement severity_level=high proto=https service=web action=alert policy="Alert_Policy" src=19.6.150.138 src_port=51234 dst=172.16.10.10 dst_port=443 http_method=GET http_url="/index.php?id=1' OR '1'='1" attack_type="SQL Injection"


### Field-by-Field Description
| Field          | Example Value       | Meaning |
|----------------|---------------------|---------|
| date           | 2025-08-14          | Event date |
| time           | 14:45:30            | Event time |
| type           | attack               | Log type |
| main_type      | sql_injection        | Main detection type |
| sub_type       | select_statement     | Specific attack signature |
| severity_level | high                 | Severity |
| proto          | https                | Protocol |
| service        | web                  | Service targeted |
| action         | alert                 | Action taken |
| policy         | Alert_Policy         | WAF policy name |
| src            | 19.6.150.138         | Attacker IP |
| src_port       | 51234                 | Attacker port |
| dst            | 172.16.10.10         | Target IP |
| dst_port       | 443                   | Target port |
| http_method    | GET                   | HTTP method used |
| http_url       | /index.php?id=1...    | Requested URL |
| attack_type    | SQL Injection         | Type of attack detected |

**Interpretation:**  
Detected high-severity SQL injection attempt from `19.6.150.138` to `172.16.10.10` via HTTPS GET request.

