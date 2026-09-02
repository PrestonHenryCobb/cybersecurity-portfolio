# Wireshark Analysis Notes

Use this file to record findings from packet capture analysis.

## Useful display filters
- `tcp.flags.syn==1 && tcp.flags.ack==0` — isolate SYN packets (connection attempts)
- `http.request` — isolate HTTP requests
- `dns` — isolate DNS traffic
- `tcp.analysis.retransmission` — find retransmissions (possible network issues or scanning)
- `ip.addr==<IP>` — filter to traffic involving a specific host

## Example investigation notes
- Document capture date/time, source/destination hosts, and what you were looking for
- Note anything anomalous: unexpected ports, cleartext credentials, beaconing patterns, DNS to suspicious domains
