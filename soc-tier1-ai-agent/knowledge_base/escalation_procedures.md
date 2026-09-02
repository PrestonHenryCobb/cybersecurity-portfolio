# SOC Escalation Procedures

## When to escalate to Tier 2
- Confirmed or highly likely malicious activity (not just a suspicious alert)
- Any evidence of successful compromise (credential theft, malware execution, lateral movement)
- Ambiguous alerts where Tier 1 cannot determine severity confidently
- Any alert involving a critical asset (domain controller, exec accounts, production systems)

## Escalation package should include
- Alert type and source
- All extracted IOCs
- MITRE ATT&CK mapping
- Severity rating and justification
- Investigation steps already taken and their results
- Clear recommendation (not just raw data)

## Severity-based response times (example SLAs)
| Severity | Initial Response | Escalation |
|---|---|---|
| Critical | Immediate | Immediate — page on-call Tier 2/IR |
| High | < 30 min | Escalate within 1 hour |
| Medium | < 2 hours | Escalate if unresolved in 4 hours |
| Low | Same business day | Escalate if pattern emerges |
| Informational | Log only | No escalation needed |
