# Severity Classification Guidelines

## Critical
- Active compromise of a critical system (domain controller, core infrastructure)
- Confirmed data exfiltration
- Ransomware execution/detonation
- Widespread, multi-host compromise

## High
- Confirmed malware execution on a single host
- Confirmed credential compromise
- Successful lateral movement
- Active, ongoing attack

## Medium
- Suspicious activity with partial evidence of success (e.g., user clicked phishing link, but no evidence of follow-on execution)
- Isolated brute-force attempts with no successful logon
- Policy violations with potential security impact

## Low
- Failed attack attempts with no evidence of success
- Isolated anomalies that don't yet form a pattern
- Known-benign activity that triggered a rule (needs tuning, not response)

## Informational
- Expected/authorized activity that happened to trigger a rule
- Noise / false positives — flag for detection rule tuning

## Justification requirement
Every severity rating must include a one- or two-sentence justification referencing the specific evidence (indicators, event IDs, ATT&CK mapping) that supports it.
