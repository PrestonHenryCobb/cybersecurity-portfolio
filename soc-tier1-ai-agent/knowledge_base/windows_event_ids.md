# Windows Event ID Quick Reference

| Event ID | Description | Why it matters |
|---|---|---|
| 4624 | Successful logon | Baseline; correlate with 4625 for brute force |
| 4625 | Failed logon | Repeated instances = brute force / password spray |
| 4634 | Logoff | Session end |
| 4648 | Logon using explicit credentials | Can indicate lateral movement (runas, psexec) |
| 4672 | Special privileges assigned to new logon | Admin-level logon; watch for unexpected accounts |
| 4688 | New process created | Key for detecting suspicious command execution |
| 4697 | Service installed | Possible persistence mechanism |
| 4720 | User account created | Watch for unauthorized account creation |
| 4732 | Member added to security-enabled local group | Privilege escalation indicator |
| 4768 / 4769 | Kerberos TGT/service ticket requested | Kerberoasting / Golden Ticket indicators |
| 1102 | Audit log cleared | Strong indicator of anti-forensics / cover-up |

## Notes
- Always correlate event IDs with source IP, account name, and timestamp.
- A single event rarely confirms an incident — look for patterns (e.g., 4625 x20 followed by 4624 from the same source).
