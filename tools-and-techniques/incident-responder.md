# Incident Response & SOC Operations

This guide is for SOC analysts and incident responders.  
It provides structured IR lifecycle, SOC workflows, persistence hunting, and tools.

---

## Incident Response Plan (6 Stages)

1. **Preparation**
   - Central logging, NTP sync
   - Asset/patch visibility
   - User account hygiene
   - Alternate comms
   - Legal escalation defined

2. **Identification**
   - Review alerts
   - Assign first responder
   - Use checklists for consistency

3. **Scope**
   - Classify event type
   - Contain (disable accounts, block IPs)
   - Collect volatile data
   - Isolate hosts

4. **Eradication**
   - Remove root cause
   - Wipe/rebuild if rootkits
   - Patch, harden
   - Run vuln scans

5. **Recovery**
   - Verify systems & logs
   - Restore from clean backups
   - Monitor recurrence

6. **Lessons Learned**
   - Write report
   - Capture gaps
   - Update playbooks

---

## SOC Incident Workflow

1. **Alert** → SIEM triggers from EDR/IDS/IPS/WAF  
2. **Analyze** → Tier 1 filters FPs  
3. **Investigate** → source + attacker progress  
4. **Assess Impact** → damage scope  
5. **Contain** → isolate hosts  
6. **Respond** → remove persistence, restore, document

---

## Active Threat Analysis
Ask:
1. **Is malware active?** (`excel.exe → powershell.exe`)  
2. **Suspicious comms?** (bad IPs, big transfers, odd ports)  
3. **Persistence?** (tasks, registry, services, backdoors)

---

## Memory Analysis
Check:
- **Process Tree**: odd hierarchies, injected processes  
- **Network Connections**: IPs, ports, traffic anomalies  
- **Signature Status**: unsigned = suspicious, signed ≠ safe (SolarWinds)  

➡ Tools: Process Hacker, Volatility.

---

## Persistence Techniques

### 1. Users
- Create low-profile users (support, sysadmin, admin).
- **Event IDs**: 4720 (created), 4726 (deleted), 4732 (added to group).
- Tools: `net user`, `lusrmgr.msc`, Event Viewer.

### 2. Scheduled Tasks
- Automates persistence.
- Tools: Autoruns, Task Scheduler, `schtasks`.
- **Event IDs**: 4698 (created), 4702 (updated), 4699 (deleted).
- Logs: `Microsoft-Windows-TaskScheduler/Operational.evtx`.

**Example:**
- Task `Update-Daily` runs `important.bat`.
- Contents:
  ```bat
  net user User123 P@ssword123 /add
  net localgroup "Remote Desktop Users" User123 /add
