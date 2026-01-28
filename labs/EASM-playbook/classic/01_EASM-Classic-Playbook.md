# Classic EASM Playbook (Step-by-Step)

External Attack Surface Management (EASM) is about discovering and monitoring all **internet-facing assets** of an organization.

### 1. Domains
- Start with the **primary domain** (e.g., `abanca.com`).
- Expand with:
  - Host.io → co-hosted, backlinks, redirects  
  - Reverse WHOIS → viewdns.info, whoxy.com  
  - DNS records → identify shared nameservers  

### 2. Subdomains
- Tools:
  - `SecurityTrails`
  - `Sublist3r`
  - `Aquatone`
  - `Assetfinder`
- Collect from multiple sources → merge into one list.

### 3. Websites
- Use `httpx` or `httprobe` to check if subdomains/domains are live.

### 4. Login Pages
- Detect manually or use Python (`requests` + `BeautifulSoup`).
- Indicators: forms, “Login/Password” fields, keywords.

### 5. Technologies
- Browser plugins: **Wappalyzer**, **WhatRuns**, **BuiltWith**.
- Online: whatcms.org
- Manual: inspect headers + page source.

### 6. IP Addresses & Blocks
- Resolve domains/subdomains → map A records.
- Group into IP ranges.
- Verify ownership with **whois**, **Shodan**, **bgp.he.net**.

### 7. DNS Records
- Use `dig`, `dnslytics.com`, or Google’s dig tool.
- Monitor for suspicious changes.

### 8. C-Level Emails
- Tools: **SalesQL**, **RocketReach**, **Apollo**, **ContactOut**.
- Warning: always use safe/fake accounts for collection.

### 9. Network Applications & OS
- Passive: Shodan, BinaryEdge, Zoomeye  
- Active: port scans (Nmap, Masscan)  
- Detect service banners + OS.

### 10. BIN & SWIFT Codes (for banks)
- BIN: bincheck.io, freebinchecker.com, bintable.com  
- SWIFT: wise.com, bank.codes, theswiftcodes.com  

### 11. SSL Certificates
- Sources: `crt.sh`, **Censys.io**  
- Important for cert expiry + misuse tracking.

---
✅ **Output**: An asset inventory that forms the baseline for monitoring & defense.
