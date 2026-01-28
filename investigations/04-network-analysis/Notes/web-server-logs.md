# Web Server Logs

## Common Status Codes
- 200 OK — Success
- 301 — Redirect
- 403 — Forbidden
- 404 — Not Found
- 503 — Service Unavailable

## Use in SOC Analysis
- Detect anomalies in request rates, methods, or source IPs.
- Spot brute-force or fuzzing attempts.


## Example Log
192.168.1.20 - - [14/Aug/2025:15:05:22 +0000] "GET /login.php HTTP/1.1" 200 532 "-" "Mozilla/5.0"


### Field-by-Field Description
| Field           | Example Value              | Meaning |
|-----------------|----------------------------|---------|
| Client IP       | 192.168.1.20               | IP making the request |
| Identd/User     | - -                        | Not provided |
| Timestamp       | [14/Aug/2025:15:05:22 +0000]| Request time and timezone |
| Method & Path   | GET /login.php HTTP/1.1    | HTTP method, resource, and version |
| Status Code     | 200                        | Response status |
| Size            | 532                        | Response size in bytes |
| Referrer        | "-"                        | Referring page |
| User-Agent      | Mozilla/5.0                | Browser or client details |

**Interpretation:**  
Browser `Mozilla/5.0` from IP `192.168.1.20` accessed `/login.php` and received a 200 OK response.
