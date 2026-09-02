# MITRE ATT&CK — Quick Reference Summary

High-level summary of ATT&CK tactics and commonly seen techniques for Tier 1 triage. Not exhaustive — treat as a fast lookup, and consult the full ATT&CK framework (attack.mitre.org) for complete technique details.

## Tactics (Enterprise Matrix)
- **Reconnaissance (TA0043)** — Gathering info to plan future operations
- **Resource Development (TA0042)** — Establishing resources to support operations
- **Initial Access (TA0001)** — Getting into the network
- **Execution (TA0002)** — Running malicious code
- **Persistence (TA0003)** — Maintaining foothold
- **Privilege Escalation (TA0004)** — Gaining higher-level permissions
- **Defense Evasion (TA0005)** — Avoiding detection
- **Credential Access (TA0006)** — Stealing credentials
- **Discovery (TA0007)** — Learning about the environment
- **Lateral Movement (TA0008)** — Moving through the environment
- **Collection (TA0009)** — Gathering data of interest
- **Command and Control (TA0011)** — Communicating with compromised systems
- **Exfiltration (TA0010)** — Stealing data
- **Impact (TA0040)** — Disrupting availability or integrity

## Commonly seen techniques for Tier 1 triage
| Technique | ID | Tactic | Notes |
|---|---|---|---|
| Phishing | T1566 | Initial Access | Most common initial access vector; check email headers, links, attachments |
| Valid Accounts | T1078 | Defense Evasion / Persistence / Privilege Escalation / Initial Access | Look for anomalous logon times/locations |
| Brute Force | T1110 | Credential Access | Repeated failed logons (Event ID 4625) followed by a success (4624) |
| Command and Scripting Interpreter | T1059 | Execution | PowerShell, cmd, bash — check for obfuscated commands |
| Exfiltration Over C2 Channel | T1041 | Exfiltration | Unusual outbound volume/destination |
| Remote Services | T1021 | Lateral Movement | RDP, SMB, WinRM used to move laterally |
| Ingress Tool Transfer | T1105 | Command and Control | Malware/tools downloaded post-compromise |

## How to use this
When triaging an alert, ask: "What is the attacker likely trying to accomplish, and which tactic does that map to?" Then narrow to the specific technique based on artifacts (process name, network behavior, log source).
