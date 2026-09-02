# Playbook: Phishing Email

1. Collect the reported email, full headers, URLs, and attachment hashes.
2. Check SPF/DKIM/DMARC results and sender domain legitimacy.
3. Check URL/attachment reputation via threat intel tools.
4. Determine if the user interacted (clicked link / opened attachment / entered credentials).
5. If interaction occurred: check EDR/proxy logs for follow-on activity (new processes, outbound connections).
6. Search mail logs for other recipients of the same campaign.
7. Recommend: block sender/URL at gateway, force password reset if credentials entered, notify affected users.
8. Escalate to Tier 2 if there's evidence of successful compromise or a wide-reaching campaign.
