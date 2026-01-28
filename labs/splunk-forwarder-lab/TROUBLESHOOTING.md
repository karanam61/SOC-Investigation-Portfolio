```markdown
# Splunk Linux Forwarder Lab — Troubleshooting

This document logs the issues faced while setting up Splunk Enterprise and Universal Forwarder, with causes and fixes.

---

## 1. Forwarder shows "configured but inactive"
- **Symptom**: `splunk list forward-server` showed indexer but inactive.  
- **Cause**: Indexer not listening on port 9997 or wrong IP used.  
- **Fix**: Enabled receiver on indexer (`splunk enable listen 9997`) and restarted UF.

---

## 2. Permission error: "Please run 'splunk ftr' as boot-start user"
- **Symptom**: UF complained about `/opt/splunkforwarder` permissions.  
- **Cause**: Service user had no ownership of Splunk dir or `/var/log/*`.  
- **Fix**: Either run UF as root, or `chown` directories to `splunkfwd` and add it to `adm` group.

---

## 3. Indexer shows 9997 enabled but no logs
- **Symptom**: Splunk Web showed receiver enabled, but searches returned no data.  
- **Cause**: No monitored inputs were added.  
- **Fix**: Added monitors for `/var/log/syslog` and `/var/log/auth.log`.

---

## 4. "Forwarder Management" shows 0 clients
- **Cause**: That page only displays forwarders managed by a deployment server.  
- **Fix**: Expected in this lab — ignore.

---

## 5. Management port mismatch (8085 vs 8089)
- **Symptom**: Indexer commands failed until correct port was given.  
- **Fix**: Run with `-p 8085` (e.g., `/opt/splunk/bin/splunk -p 8085 enable listen 9997`).

---

## 6. Socket errors in UF logs
- **Symptom**: `TcpOutputProc connect failed` in UF logs.  
- **Cause**: UF couldn’t reach indexer listener (port/firewall/IP mismatch).  
- **Fix**: Verified listening socket with `ss -ltnp | grep 9997`, fixed firewall rules with `ufw allow 9997/tcp`.
