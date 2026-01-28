# Threat Intelligence Sources for EASM

One of the most important things in Threat Intelligence is **diversity of sources**.  
The wider the sources → the better the coverage (while managing false positives).

---

## 🔹 Shodan & Alternatives
- Internet-wide search engine for exposed systems.
- Filters: country, org, open ports.
- Alternatives: **BinaryEdge**, **Zoomeye**, **Censys**.

## 🔹 IOC Providers
- Collect **IPs, domains, hashes, C2s**.
- Sources: Alienvault, MalwareBazaar, Abuse.ch, Malshare, Anyrun, Virustotal, Hybrid-Analysis, Phishunt, Spamhaus.
- Use APIs to pull fresh IOCs regularly.

## 🔹 Hacker Forums
- Early chatter before campaigns.
- Used to sell access, data leaks.
- Must monitor with caution (often dark web).

## 🔹 Ransomware Blogs
- Active since 2020.
- Groups: Lockbit, Conti, Revil, Hive, Babuk.
- Need Tor Browser to access `.onion` blogs.

## 🔹 Black Markets
- More structured than forums.
- Sell: credit cards, RDP access, logs.
- No APIs → scraping/parsing required.

## 🔹 Messaging Platforms
- Telegram, ICQ, IRC, Discord → chatter + sales.
- Useful for detecting stolen data offers.

## 🔹 Code Repositories
- GitHub, GitLab, Bitbucket.
- Search for leaked secrets (`dorks`: e.g., `"password" "company.com"`).

## 🔹 File-Sharing Sites
- Anonfiles, Mediafire, WeTransfer, File.io.
- Watch for **data leaks**.
- Indexed files can sometimes be captured via search dorks.

## 🔹 Public Buckets
- AWS S3, Azure Blobs, GCP Buckets.
- Brute-force org names → find misconfigured open buckets.

## 🔹 Honeypots
- Deploy intentionally vulnerable systems.
- Collect attacker IPs, tactics.
- Examples: Kippo, Cowrie, Glastopf, Honeymail.

## 🔹 Security Devices
- SIEM / IDS / IPS / Firewalls → logs are rich intelligence.
- Extract blocked IPs, file hashes, anomalous traffic.

---
✅ **Output**: Curated multi-source intelligence to enrich EASM.
