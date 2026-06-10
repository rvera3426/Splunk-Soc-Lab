# Home SOC Lab — Splunk SIEM & Incident Response

A personal enterprise-grade security operations lab built to simulate real-world SOC workflows — including threat detection, alert triage, incident investigation, and documentation.

---

## Overview

This lab deploys Splunk Enterprise SIEM to continuously monitor Windows and Linux environments, ingesting security event logs from a Windows Domain Controller and Linux authentication logs. The environment is designed to mirror defense contractor SOC operations, with detection rules, escalation criteria, and shift-change documentation.

**Continuously analyzing 5,000+ security events across both platforms.**

---

## Environment

| Component | Details |
|---|---|
| SIEM | Splunk Enterprise |
| Windows Host | Windows Server 2022 — Domain Controller |
| Linux Host | Ubuntu Linux |
| Log Sources | Windows Security Event Logs, Linux auth logs (`/var/log/auth.log`) |
| Active Directory | Configured with security hardening (password policies, lockout thresholds, least-privilege groups) |

---

## Dashboards

![Splunk Dashboards](Screenshot%202026-06-10%20120637.png)

Six custom dashboards built including **Security Overview** and **Successful Logins Over Time** for continuous monitoring visibility.

---

## Detection Rules

Four custom detection rules built, tested, and validated with scheduled alerts, severity classifications, and documented escalation criteria.

| Rule | Log Source | Severity | Trigger |
|---|---|---|---|
| SSH Brute Force | Linux auth log | High | 5+ failed SSH attempts within 5 min from same source IP |
| Windows Failed Logins | Windows Security Event Log | Medium | 5+ Event ID 4625 within 5 min |
| Account Lockout | Windows Security Event Log | High | Event ID 4740 detected |
| Unauthorized Account Creation | Windows Security Event Log | Critical | Event ID 4720 outside approved hours |

---

## Detection Rule Screenshots

### Brute Force SSH Detection
SPL query identifying repeated failed SSH attempts by source IP across `/var/log/auth.log`.

![Brute Force SSH Detection](Screenshot%202026-06-10%20120242.png)

### Brute Force Attack — High Volume Visualization
Column chart visualization showing failure vs. success counts per source IP — 6,791 failures detected from a single host.

![Brute Force High Volume](Screenshot%202026-06-10%20120524.png)

### Windows Failed Login Detection (Event ID 4625)
Tracks failed Windows logins by account name — flagging accounts with more than 3 failures.

![Windows Failed Login](Screenshot%202026-06-10%20120613.png)

### Account Lockout Detected (Event ID 4740)
Captures lockout events with account name and caller computer name — enabling rapid pivot to source workstation.

![Account Lockout](Screenshot%202026-06-10%20120601.png)

### New User Account Created (Event ID 4720)
Detects unauthorized account creation events with timestamp, account name, and creating subject.

![New User Account Created](Screenshot%202026-06-10%20120541.png)

---

## Incident Response Workflow

Each triggered alert follows a documented response procedure:

1. **Triage** — Review alert in Splunk, confirm true/false positive
2. **Investigation** — Pivot on source IP, user account, and timestamps; correlate across log sources
3. **Root Cause Analysis** — Identify origin (e.g. failed auth attempts from specific workstation, lockout triggered by service account)
4. **Remediation** — Document corrective action taken
5. **Shift-Change Documentation** — Produce handoff notes summarizing open items, findings, and escalations

---

## Sample Investigations

### SSH Brute Force — Linux Host
- **Alert triggered:** 12 failed SSH attempts from `192.168.100.128` within 3 minutes
- **Investigation:** Pivoted to auth.log — all attempts targeting root account, consistent timing suggesting automated tool
- **Action:** Documented source IP, flagged for blocking, escalated per procedure
- **Outcome:** Confirmed brute force attempt; no successful authentication

### Account Lockout — Windows Domain Controller
- **Alert triggered:** Event ID 4740 for user `jsmith`
- **Investigation:** Traced lockout source to workstation `WKS01`; correlated with repeated Event ID 4625 preceding lockout
- **Action:** Investigated whether credential stuffing or misconfigured service; documented findings
- **Outcome:** Determined stale cached credentials on WKS01; resolved and documented

---

## Network Reconnaissance

Conducted regular Nmap scans across all lab hosts to identify open ports, exposed services, and potential attack surfaces — establishing and maintaining security baselines.

```bash
# Example: Full port scan with service version detection
nmap -sV -p- 192.168.100.0/24
```

---

## Active Directory Security Hardening

Configured enterprise AD environment aligned with STIG/NIST 800-53 controls:

- Minimum password length: 12 characters
- Account lockout threshold: 5 failed attempts
- Lockout duration: 30 minutes
- Screen lock enforcement via Group Policy
- Least-privilege access — department-based security groups

---

## Skills Demonstrated

- Splunk SPL (Search Processing Language) — search, detection, alerting
- Windows Security Event Log analysis (Event IDs 4625, 4720, 4740, etc.)
- Linux authentication log analysis
- Incident triage and investigative documentation
- Active Directory security hardening
- Network reconnaissance and baseline establishment (Nmap)
- Python scripting for log parsing and alert automation
- Shift-change and incident handoff documentation

---

## Certifications & Background

- CompTIA Security+ CE — DoD 8570 IAT Level II Compliant
- AWS Cloud Practitioner — In Progress (Jun 2026)
- B.S. Cybersecurity and Information Assurance — WGU (Expected 2028)
- Network & Systems Administrator — GDIT, Fort Bliss (Defense Industrial Base)
