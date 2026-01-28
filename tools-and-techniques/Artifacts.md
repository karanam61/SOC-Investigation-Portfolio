```markdown
# IR & SOC Quick Reference Cheat Sheet

One-page lookup for Event IDs, commands, and tools.

---

## Event IDs
| Event ID | Meaning                             | Where to Check |
|----------|-------------------------------------|----------------|
| 4720     | User created                        | Security Log   |
| 4726     | User deleted                        | Security Log   |
| 4732     | User added to privileged group      | Security Log   |
| 4698     | Scheduled task created              | Security/TaskScheduler Log |
| 4702     | Scheduled task updated              | Security/TaskScheduler Log |
| 4699     | Scheduled task deleted              | TaskScheduler Log |
| 4657     | Registry value modified             | Security Log   |

---

## Commands
- **Users**
  - `net user` → list users
  - `net user USERNAME` → inspect details
  - `lusrmgr.msc` → local user GUI

- **Scheduled Tasks**
  - `schtasks /query /fo LIST /v` → list all
  - `schtasks /query /fo LIST /v | findstr /i ".bat"` → filter batch files

- **Registry**
  - `regedit` → inspect Run/RunOnce/RunServices keys

- **Services**
  - `services.msc` → review services

---

## Tools
- Process Hacker → processes, net conns, signatures  
- Autoruns → startup, registry, services, tasks  
- FullEventLogView → filter logs  
- LastActivityView → timeline view  
- BrowsingHistoryView → web history  
- Volatility → memory forensics  

---

## Memory Analysis
Focus checks:
1. **Process Tree** → abnormal parent-child (`excel.exe → powershell.exe`)  
2. **Network Connections** → suspicious IPs, ports, large transfers  
3. **Signature Status** → unsigned/mismatched executables  
