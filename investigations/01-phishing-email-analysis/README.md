# Phishing Email Analysis

This repository is my working portfolio for phishing investigations.  
It contains the notes I’ve written, the alerts I’ve worked through, and the challenges I’ve solved while practicing SOC workflows.  
Everything here is documented in my own words to reflect how I actually use SIEM tools, not just theory.

---

## Repository Structure
- [Notes/](./Notes) → reference material and investigation playbooks  
  - [Phishing Playbook](./Notes/Phishing-Playbook.md)  
- [Alerts/](./Alerts) → individual phishing alerts I investigated  
- [Challenges/](./Challenges) → lab-based exercises and scenarios  

---

## SOC Workflow Components
Every phishing investigation I document follows the same SOC workflow across four SIEM components:
- **Monitoring Tab (Alerts)** → where suspicious activity is first flagged  
- **Log Management** → digging into mail/network logs for IPs, domains, patterns  
- **Email Security** → headers, SPF/DKIM/DMARC, attachments, and URL inspection  
- **Endpoint Security** → checking if a user clicked or executed something and host telemetry  

---

## Framework Mapping
My investigations are tied back to widely used frameworks for context:
- **MITRE ATT&CK** → e.g., T1566 (Phishing), T1059.001 (PowerShell Execution)  
- **NIST CSF** → Detect → Respond → Recover  
- **ISO 27001** → A.12.4 Logging & Monitoring  
- **OWASP Top 10** → when attackers abuse web misconfigurations or trusted services  

---

## Example Highlight
- [Phishing Playbook](./Notes/Phishing-Playbook.md)  
  - Validated sender authenticity using SPF/DKIM/DMARC  
  - Investigated headers and SMTP logs for mismatches and unusual hops  
  - Sandbox testing revealed PowerShell execution from an attachment  
  - Endpoint Security confirmed host compromise → isolated & reimaged  
  - Explained findings in plain English to non-IT staff  

---

## Goal
This repo doubles as:
- A **portfolio** for recruiters and hiring managers who want to see more than resume buzzwords  
- A **personal reference** I can reuse for repeatable SOC workflows  
- A **learning log** to track progress as I move from notes → alerts → full challenge investigations  
