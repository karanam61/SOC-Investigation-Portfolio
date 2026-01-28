# NetFlow Log Analysis

NetFlow provides high-level network flow summaries, not individual packet details.

Think of it like a CCTV camera: it records:
- Who talked to whom
- When
- How much data was transferred  
Strictly based on IPs.

**Key Points:**
- Records conversations (flows) between IP addresses.
- Includes timestamps, duration, byte/packet counts.
- Useful for identifying top talkers, suspicious data exfiltration, and unusual communication patterns.

## Example Log

2025-08-14T10:35:12Z 192.168.1.10 10.0.0.5 TCP 443 52345 10240

2025-08-14T10:35:12Z 192.168.1.10 10.0.0.5 TCP 443 52345 10240


### Field-by-Field Description
| Field | Example Value         | Meaning |
|-------|-----------------------|---------|
| Timestamp | 2025-08-14T10:35:12Z | When the flow was recorded (UTC) |
| Source IP | 192.168.1.10         | Device initiating the connection |
| Destination IP | 10.0.0.5       | Target device |
| Protocol | TCP                  | Transport protocol used |
| Destination Port | 443          | Service port (HTTPS) |
| Packet Count | 52345            | Number of packets sent in this flow |
| Byte Count | 10240              | Total data size in bytes |

**Interpretation:**  
At 10:35:12 UTC, `192.168.1.10` connected to `10.0.0.5` over TCP port 443, sending 52,345 packets

