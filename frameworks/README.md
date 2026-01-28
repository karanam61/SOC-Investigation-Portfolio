# Mitre-attack-frameworks-
General overview about Mitre-attack frameworks 

# MITRE ATT&CK Framework

This repository covers my study notes and practical understanding of the **MITRE ATT&CK Framework**.  
Instead of dumping raw theory, I’ve rewritten the content in my own words and tied it back to SOC workflows.

## What’s Inside
- Overview of MITRE ATT&CK matrices (Enterprise, Mobile, ICS)  
- Explanation of tactics, techniques, sub-techniques, and procedures  
- Notes on mitigations, APT groups, and software  
- Practical mapping of alerts/incidents to MITRE ATT&CK techniques  
- Plain-English explainers for non-technical audiences  

## Why It Matters
MITRE ATT&CK isn’t just a chart. It’s the standard reference for mapping adversary behavior.  
I use it to:
- Classify incidents during triage and investigation  
- Connect alerts to attacker objectives (e.g., phishing → Initial Access → C2)  
- Align security work with frameworks like NIST CSF and ISO 27001  

## Example Use
- Suspicious PowerShell alert → mapped to **Execution (T1059.001)**  
- Phishing email → mapped to **Initial Access (T1566)**  

## References
- [MITRE ATT&CK Enterprise Matrix](https://attack.mitre.org/matrices/enterprise/)  
- [MITRE ATT&CK Mobile Matrix](https://attack.mitre.org/matrices/mobile/)  
- [MITRE ATT&CK ICS Matrix](https://attack.mitre.org/matrices/ics/)  
