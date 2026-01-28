# Splunk Linux Forwarder Lab

This repository captures a complete lab setup for installing and troubleshooting:

- **Splunk Enterprise (Indexer)** — runs the search UI, stores events, listens on port 9997 for forwarder connections.  
- **Splunk Universal Forwarder (UF)** — lightweight agent that monitors files and forwards them to the indexer.  

The repo is structured around **learning by doing**, including real problems we hit during setup and how they were fixed.

## Repository Structure
- `README.md` → high-level overview (this file)  
- `TROUBLESHOOTING.md` → detailed list of errors and resolutions  
- `scripts/` → helper shell scripts for setup and verification  
- `configs/` → Splunk config snippets (inputs.conf, outputs.conf)  
- `docs/` → scratch notes and useful commands  

## Quick Start
1. Enable receiving on the indexer:
   ```bash
   sudo /opt/splunk/bin/splunk enable listen 9997
   sudo /opt/splunk/bin/splunk restart


### On Forwarder (Splunk UF)
sudo /opt/splunkforwarder/bin/splunk add forward-server 127.0.0.1:9997
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/syslog
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
sudo /opt/splunkforwarder/bin/splunk restart

### Verify

Splunk Web → http://localhost:8000

CLI:

/opt/splunkforwarder/bin/splunk list forward-server
/opt/splunkforwarder/bin/splunk list inputstatus

### Why This Repo

Linux setup is trickier than Windows (permissions, ports, configs).
This repo keeps all the commands, configs, and troubleshooting notes in one place so the setup can be repeated quickly next time.f
