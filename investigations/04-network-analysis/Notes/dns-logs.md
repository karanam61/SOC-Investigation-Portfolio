## Purpose
DNS translates domains to IPs; logs are useful for detecting suspicious lookups.

## Key Questions
- Access to non-approved/risky domains?
- Attempts to use DOT/DOH?
- Matches to Threat Intel IOCs?

## Log Sources
- DNS server events (audit logs)
- DNS query logs (must be enabled)

## Suspicious Patterns
- Random subdomains in short time → tunneling
- Servers making cloud service queries unexpectedly
- NXDOMAIN spikes


## Example Log
Feb 5 09:12:11 ns1 named[80090]: client 192.168.10.3#3261: query: login.microsoftonline.com IN A


### Field-by-Field Description
| Field          | Example Value             | Meaning |
|----------------|---------------------------|---------|
| Timestamp      | Feb 5 09:12:11            | Time of DNS request |
| Server Host    | ns1                       | DNS server hostname |
| Process        | named[80090]              | Process handling the query |
| Client IP:Port | 192.168.10.3#3261          | IP and source port making the query |
| Query          | login.microsoftonline.com | Domain requested |
| Record Type    | IN A                      | Internet Address (IPv4) record |

**Interpretation:**  
`192.168.10.3` queried for the IPv4 address of `login.microsoftonline.com`. Unexpected if `192.168.10.3` is a DB server — possible exfiltration vector.
