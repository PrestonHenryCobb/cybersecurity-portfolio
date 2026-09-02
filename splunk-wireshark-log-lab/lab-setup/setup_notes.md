# Lab Setup Notes

## Environment
- Virtualized lab environment (VirtualBox/VMware) with an isolated network
- Splunk (Free / Splunk Enterprise trial) running as a log collection/search endpoint
- Wireshark installed on a monitoring host or configured with a SPAN/mirror port

## Typical setup steps
1. Stand up target VM(s) (e.g., Windows/Linux endpoints) on an isolated virtual network
2. Configure a monitoring point (host-based capture or virtual switch port mirroring) for Wireshark
3. Forward relevant logs (Windows Event Logs, Sysmon, etc.) to Splunk via a forwarder
4. Generate traffic/events to analyze (e.g., simulated login attempts, benign and suspicious traffic)
5. Capture with Wireshark and cross-reference with what appears in Splunk

## Tools used
- Wireshark
- Splunk (Free / Enterprise Trial)
- Sysmon (optional, for richer Windows telemetry)
