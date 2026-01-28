# Log Monitoring Guide

## Step 0 — Zoom Out
Break into key-value pairs:
- When: timestamp
- Where: src_ip, dst_ip
- Channel: proto, ports
- Action: type, status
- Extras: payload, user, file

## Step 1 — Identify OSI/TCP-IP Layer
- TCP/UDP → Transport
- HTTP GET, MAIL FROM → Application
- MAC, VLAN → Data Link

## Step 2 — Who Talked to Who
- Source = initiator
- Destination = receiver

## Step 3 — Guess Protocol
- Port 53 → DNS
- Port 80/443 → HTTP/HTTPS
- Etc.

## Step 4–8 — Action, Object, Time, Baseline, Triage
- Normal → move on
- Weird → investigate
- Severe + confirmed → escalate
