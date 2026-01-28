# IT Security Basics

Covers the fundamental building blocks of infrastructure and IT security.  
These practices reduce attack surface, enforce resilience, and prepare for incidents.

---

## Inventory
Maintain records of:
- Hardware, Software (with versions)
- Security controls (AV/EDR, configs, encryption)
- Access rights
- Last audit date

➡ Visibility enables patching, compliance, and vulnerability management.

---

## End-of-Life Equipment
- Unsupported = no patches = risk.
- Remove or isolate.
- If unavoidable: document **risk acceptance** with CISO approval.

---

## Secure Boot
- Enable on all compatible devices.
- Prevents bootkits/rootkits by enforcing signed boot code.

---

## Software Management
- **Allowed software**: pre-approved via GPO/Intune.
- **Forbidden software**: block with AppLocker/Intune.
- Report attempts to use forbidden apps.

---

## Security Hardening
- Use standards (CIS Benchmarks).
- Perform handover checklists, secure config audits, config drift alerts.
- Automate with Ansible, GPO, Intune.

---

## Antivirus / EDR
- Deploy across all endpoints.
- Monitor alerts, keep updated.
- Prioritize critical systems first.

---

## Backup – 3-2-1 Rule
- 3 copies
- 2 different media
- 1 offsite copy
- Test restore process regularly.

---

## DNS Filtering
- Block malicious/unwanted domains.
- Don’t forget **URL shorteners** (phishing-heavy).
- Review blocked attempts.

---

## Centralized Browser Management
- Enforce updates.
- Restrict extensions/plugins.
- Disable auto-execution (Flash, ActiveX).
- Apply via Intune/GPO.

---

## Patch Management (SLA by CVE/CVSS)
- Critical (9–10): patch in 24–48h
- High (7–8.9): 7 days
- Medium (4–6.9): 30 days
- Low (<4): 90 days
- Internet-facing/0-days → patch immediately
- Track via WSUS, Intune, Ansible.

---

## Access Control & Zero Trust
- Strong password policies + MFA (start with admins).
- Disable unused, no shared accounts.
- Apply least privilege.
- Domain Admin accounts <15%.

---

## Risk Management & Business Continuity
- BIA → RTOs/RPOs.
- Assess connected network risks (partners/vendors).
- SLAs for patching/config standards.
- Verify partner configs.

---

## Network Security
- **PCAP**: capture traffic for anomaly detection, forensics.
- **Segmentation**: VLAN/PVLAN → reduce blast radius.
- **Blocked flows review**: blocked requests may reveal shadow IT/compromised hosts.
