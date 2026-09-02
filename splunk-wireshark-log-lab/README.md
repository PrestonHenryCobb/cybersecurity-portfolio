# Splunk & Wireshark Log Capture Lab

A home lab for practicing network traffic capture, log analysis, and SOC-style investigation using Wireshark (packet capture/analysis) and Splunk (log ingestion/search).

## Goals
- Capture and analyze network traffic with Wireshark
- Ingest logs into Splunk and write SPL queries to detect suspicious activity
- Practice end-to-end investigation: capture → search → findings → write-up

## Structure
```
splunk-wireshark-log-lab/
├── lab-setup/
│   └── setup_notes.md         # Environment/lab setup notes
├── splunk/
│   ├── queries/
│   │   └── sample_spl_queries.spl
│   └── notes.md                # Splunk-specific observations
├── wireshark/
│   ├── captures/               # .pcap files go here (gitignored)
│   └── analysis_notes.md       # Packet analysis notes
└── findings/
    └── investigation_log_template.md
```

## Notes
Actual `.pcap` capture files and raw log exports are excluded from version control (see `.gitignore`) since they can contain sensitive/large data — only the write-ups, queries, and templates are tracked.
