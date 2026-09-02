# Incident Response Procedures — Tier 1 Reference

Based on the general IR lifecycle (NIST SP 800-61).

## 1. Preparation
- Ensure logging/monitoring is functioning (SIEM, EDR, firewall logs)
- Know your escalation contacts and playbooks

## 2. Detection & Analysis
- Validate the alert is not a false positive
- Extract all indicators of compromise (IOCs)
- Determine scope: single host? Single user? Multiple systems?
- Correlate with other alerts/logs around the same timeframe

## 3. Containment
- **Short-term:** isolate the affected host/account without destroying evidence (requires analyst/Tier 2+ approval)
- **Long-term:** patch, rotate credentials, apply firewall rules

## 4. Eradication
- Remove malware, disable compromised accounts, close the exploited vulnerability

## 5. Recovery
- Restore systems from clean backups, monitor closely for recurrence

## 6. Post-Incident Activity
- Document timeline, root cause, and lessons learned
- Update detection rules/playbooks based on findings

## Tier 1 analyst scope
Tier 1's job is Detection & Analysis — triage, enrich, and escalate with a clear recommendation. Containment/Eradication/Recovery actions require Tier 2+ or analyst approval.
