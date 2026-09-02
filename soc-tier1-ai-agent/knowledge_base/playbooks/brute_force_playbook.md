# Playbook: Brute Force / Password Spray

1. Identify the target account(s) and source IP(s) from repeated 4625 (failed logon) events.
2. Determine the pattern: single account/many passwords (brute force) vs. many accounts/few passwords (spray).
3. Check whether any attempt succeeded (4624 following the failed attempts from the same source).
4. Check source IP reputation and geolocation for anomalies.
5. If no success: recommend blocking the source IP, monitoring for continued attempts.
6. If success occurred: treat as confirmed compromise — recommend forced password reset, session termination, and escalate to Tier 2 immediately.
7. Note whether MFA is enabled on the targeted account(s); if not, recommend enabling it in the remediation notes.
