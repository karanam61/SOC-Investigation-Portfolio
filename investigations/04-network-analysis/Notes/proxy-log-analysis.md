# Proxy Log Analysis

Proxies control access to HTTP, HTTPS, FTP, etc., based on policy.  
They query a category DB for the requested domain and apply **block** or **pass** actions.

## Example Analysis
- **act**: blocked
- **app**: https
- **src**: Internal server IP
- **dst**: Target IP
- **policy**: Block_Risk_Category_Policy
- **category**: 194 → Suspicious Content

## Analyst Actions
- Identify process/user making request.
- Cross-check with EDR/XDR logs for process origin.
- Determine if compromised system is attempting covert comms.

**Risks detected via proxy logs:**
- Malware beaconing to C2
- Policy evasion attempts
- Access to risky/malicious content categories

## Example Log
Jun 17 10:47:00 10.10.18.11 CEF:0|Forcepoint|Security|8.5.4|194|Transaction blocked|7| act=blocked app=https dvc=10.10.18.11 dst=104.26.11.18 dhost=sentry-proxy.cargox.cc dpt=443 src=10.80.18.50 spt=61603 suser=Test_User requestMethod=POST cs1Label=Policy cs1=Block_Risk_Category_Policy(Servers)

### Field-by-Field Description
| Field        | Example Value                  | Meaning |
|--------------|--------------------------------|---------|
| Timestamp    | Jun 17 10:47:00                 | Date & time of request |
| Device IP    | 10.10.18.11                     | IP of proxy device |
| Vendor Info  | CEF:0\|Forcepoint\|Security... | Common Event Format header |
| act          | blocked                         | Action taken |
| app          | https                           | Application/protocol |
| dvc          | 10.10.18.11                     | Device IP again |
| dst          | 104.26.11.18                    | Destination IP |
| dhost        | sentry-proxy.cargox.cc          | Destination host |
| dpt          | 443                             | Destination port |
| src          | 10.80.18.50                     | Source IP making request |
| spt          | 61603                           | Source port |
| suser        | Test_User                       | Username |
| requestMethod| POST                            | HTTP request method |
| cs1Label     | Policy                          | Custom field label |
| cs1          | Block_Risk_Category_Policy(Servers) | Policy name triggered |

**Interpretation:**  
At 10:47, `Test_User` attempted a POST request to `sentry-proxy.cargox.cc` (IP 104.26.11.18) over HTTPS, blocked due to a risk-category policy.
