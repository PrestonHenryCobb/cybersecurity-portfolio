# Common Attack Indicators (IOC Cheat Sheet)

## Network
- Beaconing (regular, repeated connections to the same external IP/domain at fixed intervals)
- Traffic to known-bad IPs/domains (check threat intel feeds)
- Unusual outbound data volume
- Connections over non-standard ports for the protocol used
- DNS requests to newly registered or algorithmically generated domains (DGA)

## Host
- Unexpected new processes, especially spawned from Office apps or browsers (e.g., winword.exe → powershell.exe)
- Obfuscated/encoded PowerShell commands
- New scheduled tasks or services
- Unusual parent-child process relationships
- Files written to unusual locations (Temp, AppData, startup folders)

## Identity
- Logons at unusual times or from unusual locations/geographies
- Impossible travel (same account, two distant locations, short time window)
- Repeated failed logons followed by success
- New MFA device registered unexpectedly

## Email
- Spoofed or lookalike sender domains
- Urgency/pressure language, requests for credentials or payment
- Mismatched display name vs. actual sending address
- Links pointing to a different domain than displayed text
