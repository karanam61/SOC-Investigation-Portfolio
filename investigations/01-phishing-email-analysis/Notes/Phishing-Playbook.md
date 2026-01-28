# Phishing Email Investigation Playbook

This document is my reference for investigating phishing alerts in a SOC environment.  
It’s based on how I actually pivot across SIEM components (Monitoring, Log Management, Email Security, Endpoint Security) instead of just theory.

---

## 1. Monitoring Tab (Alerts)
- Alerts usually come in from SIEM rules or mail security filters.  
- First step: check if the alert is user-reported or system-generated.  
- Metadata I look at: sender, subject, time, triggering rule.  
- Prioritize based on scope (one user vs. many) and whether attachments/links are involved.  

---

## 2. Log Management
- Pull SMTP logs to confirm the sending IP and domain.  
- Compare “Received” chain against expected MX servers.  
- Check for repeated hits from the same sender/IP → indicates a campaign.  
- Review network logs for outbound traffic if any links were clicked.  
- Tools: `whois`, MXToolbox, Cisco Talos, AbuseIPDB for IP/domain reputation.  

---

## 3. Email Security
### Authentication & Spoofing
- Validate SPF/DKIM/DMARC.  
- Cross-check Return-Path vs. From field.  
- If DMARC fails, verify sending IP against legitimate MX records.  

### Header Analysis
- Message-ID → consistency check.  
- Received headers → trace origin (bottom = source).  
- X-Spam-Status → spam filter baseline.  

### Static Analysis
- Hover links → check if displayed vs. actual URL match.  
- VirusTotal for domains/files (verify scan dates).  
- Check domain age/registration to flag newly created infra.  

---

## 4. Endpoint Security
If a user clicked or opened an attachment:  
- Check endpoint logs for execution events (Word, Excel, PowerShell).  
- Look for abnormal process trees (e.g., Excel → PowerShell).  
- Detect registry/service persistence attempts.  
- If confirmed: isolate the host, reset credentials, reimage if required.  

*Example:* Excel attachment launched PowerShell → MITRE T1059.001 (Execution).  

---

## 5. Dynamic Analysis (Safe Environment)
When static checks aren’t enough:  
- Sandbox detonation (AnyRun, VMRay, JoeSandbox).  
- Browserling for safe URL preview.  
- Remove tracking tokens from URLs before detonating.  
- Watch for delayed callbacks (common in modern phishing malware).  

---

## 6. Abuse of Legit Services
Attackers hide behind trusted domains:  
- Cloud storage (Google Drive, OneDrive) hosting malware.  
- Free hosting (Wix, WordPress).  
- Form services (Google Forms) for credential harvesting.  

⚠️ Always validate the specific subpage/file, not just the parent domain.  

---

## 7. Investigation Workflow (End-to-End)
1. Alert shows up in Monitoring tab.  
2. Pull headers in Email Security and validate SPF/DKIM/DMARC.  
3. Use Log Management to confirm SMTP IP/domain behavior.  
4. Static checks on links and attachments.  
5. Sandbox for dynamic behavior.  
6. Endpoint Security to check if the user executed anything.  
7. Escalate or block as needed.  

---

## 8. Soft Skills (Explaining to Non-IT)
> “This email looked like it came from Microsoft, but it was actually sent from an attacker’s server. One person opened the attachment, which launched a hidden script. We blocked the sender, reset the account, and cleaned the machine before any damage was done.”  

---

## 9. Framework Mapping
- **MITRE ATT&CK** → T1566 (Phishing), T1059.001 (PowerShell Execution), T1071 (C2).  
- **NIST CSF** → Detect → Respond → Recover.  
- **ISO 27001** → A.12.4 Logging & monitoring.  
- **OWASP Top 10** → A5 – Security Misconfigurations (attackers abusing legit services).  

---
