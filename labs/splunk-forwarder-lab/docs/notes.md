# Lab Notes

## Ports
- Splunk Web (Indexer) → 8000  
- Management API → 8085 (custom in this lab)  
- Receiving for UFs → 9997  

## Handy Commands
- `ss -ltnp | grep 9997` → check indexer listening  
- `/opt/splunkforwarder/bin/splunk list forward-server` → UF connection state  
- `/opt/splunkforwarder/bin/splunk list inputstatus` → which files UF is monitoring  
- `tail -f /opt/splunkforwarder/var/log/splunk/splunkd.log` → live UF logs

## Notes
- Windows install was smoother; Linux required fixing permissions and custom ports.  
- Deployment Server is not used here → "Forwarder Management" page will stay empty.  
