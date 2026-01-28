# VPN Log Analysis

VPNs are essential for secure remote access, making their logs critical for SOC analysts.  
Since VPN services are publicly exposed, they represent a prime attack surface.

---

## Key Log Fields
- **date** – Date of the event
- **time** – Time of the event
- **devname** – Hostname of the device
- **devid** – Device ID
- **eventtime** – Event timestamp (epoch format)
- **tz** – Time zone offset
- **logid** – Log ID
- **type** – Log type (traffic, event, etc.)
- **subtype** – VPN, webfilter, virus, etc.
- **level** – Severity
- **logdesc** – Description
- **action** – System action taken
- **tunneltype** – SSL, IPSec, etc.
- **remip** – Remote IP initiating VPN
- **user** – Authenticated username
- **reason** – Access result
- **msg** – Status message

---

## Critical Analysis Points
- Source IP of connection
- Authenticated username
- Success/failure status

---

## Common Detections
- Successful/unsuccessful VPN logins
- Brute-force attempts
- Access from non-approved geos/times

---

**Example:**  
date=2025-08-14 time=14:21:45 devname="Firewall-1" devid="FGT60E" eventtime=1692028905000000 tz="+0000" logid="0720018432" type="event" subtype="vpn" level="notice" logdesc="SSL VPN login" action="login" tunneltype="ssl" remip="13.29.5.4" user="letsdefend-user" reason="login success" msg="login successfully"

### Field-by-Field Description
| Field         | Example Value         | Meaning |
|---------------|-----------------------|---------|
| date          | 2025-08-14            | Event date |
| time          | 14:21:45              | Event time |
| devname       | Firewall-1            | Device hostname |
| devid         | FGT60E                 | Device ID/model |
| eventtime     | 1692028905000000       | Epoch timestamp (microseconds) |
| tz            | +0000                  | Timezone offset |
| logid         | 0720018432             | Log identifier |
| type          | event                  | Main log type |
| subtype       | vpn                    | Sub-category (VPN) |
| level         | notice                  | Severity level |
| logdesc       | SSL VPN login          | Description of event |
| action        | login                   | Action taken |
| tunneltype    | ssl                     | VPN tunnel type |
| remip         | 13.29.5.4               | Remote IP initiating VPN |
| user          | letsdefend-user         | Username used |
| reason        | login success           | Login result |
| msg           | login successfully      | Message from device |

**Interpretation:**  
User `letsdefend-user` from IP `13.29.5.4` successfully logged into the SSL VPN at 14:21:45 UTC
