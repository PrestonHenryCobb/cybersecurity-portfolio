# AI Tier 1 SOC Analyst Agent

An AI assistant agent designed to act as a first-pass (Tier 1) SOC analyst — **not** an autonomous system. It reviews alerts, gathers context, and makes recommendations; all remediation actions require human analyst approval.

## What it does
1. Identifies the alert type
2. Extracts indicators of compromise (IPs, domains, usernames, file hashes, ports, timestamps, processes)
3. Maps observed attacker behavior to MITRE ATT&CK techniques
4. Assigns a severity rating (Informational / Low / Medium / High / Critical) with justification
5. Recommends investigation steps
6. Recommends containment/remediation steps when applicable (for human approval)
7. Flags uncertainty rather than guessing — never confirms an incident without sufficient evidence

## Guardrails
- Assistant tool only — not autonomous
- Must not execute irreversible or destructive actions
- A human analyst must approve any remediation action

## Structure
```
soc-tier1-ai-agent/
├── system_prompt.md          # Full agent system instructions
├── knowledge_base/           # Plaintext reference material the agent draws on
│   ├── mitre_attack_summary.md
│   ├── incident_response_procedures.md
│   ├── phishing_investigation.md
│   ├── windows_event_ids.md
│   ├── attack_indicators.md
│   ├── escalation_procedures.md
│   ├── severity_classification.md
│   └── playbooks/
│       ├── phishing_playbook.md
│       ├── malware_playbook.md
│       └── brute_force_playbook.md
└── examples/
    └── sample_alert_analysis.md
```

## Usage
Load `system_prompt.md` as the system prompt for an LLM (e.g., via the Claude or OpenAI API), and provide the `knowledge_base/` files as reference context or a retrieval corpus. Feed the agent a raw alert (SIEM output, log excerpt, etc.) and it returns a structured triage.
