# Example: Sample Alert Analysis

**Raw alert (example SIEM output):**
```
Alert: Multiple failed logon attempts followed by success
User: jsmith
Source IP: 185.220.101.7
Destination: DC01 (Domain Controller)
Events: 4625 x 47 (02:14-02:19 UTC), 4624 (02:19 UTC)
```

**Agent output:**

- **Alert Type:** Brute force login attempt against a domain controller
- **Indicators Extracted:** User `jsmith`, source IP `185.220.101.7`, destination host `DC01`, timestamps 02:14-02:19 UTC
- **MITRE ATT&CK Mapping:** Credential Access — Brute Force (T1110), targeting Valid Accounts (T1078)
- **Severity:** High — a successful logon followed 47 failed attempts against a domain controller, a critical asset
- **Recommended Investigation Steps:** Confirm whether the 02:19 UTC logon for `jsmith` originated from the same source IP; check for any post-logon activity (new processes, lateral movement) on DC01; check IP reputation for `185.220.101.7` (known Tor exit node range)
- **Recommended Containment/Remediation (pending human approval):** Force password reset for `jsmith`; block source IP at the perimeter firewall; enable/verify MFA on the account
- **Confidence / Uncertainty Notes:** High confidence this represents a successful brute-force compromise given the failed-to-success pattern against a critical asset; recommend immediate Tier 2 escalation
