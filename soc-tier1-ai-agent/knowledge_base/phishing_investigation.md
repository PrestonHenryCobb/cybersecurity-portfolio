# Phishing Investigation Procedure

## Step 1: Gather the artifact
- Full email headers (From, Reply-To, Return-Path, Received chain)
- Sender IP and domain
- Any URLs (do not click — extract and check via sandbox/URL scanner)
- Attachments (hash only, do not open)

## Step 2: Check authentication
- SPF / DKIM / DMARC results (pass/fail?)
- Does the sending domain match the claimed sender?

## Step 3: Analyze links and attachments
- Check URL reputation (VirusTotal, urlscan.io, etc.)
- Check attachment hash reputation
- Look for typosquatted domains (e.g., "micros0ft.com")

## Step 4: Check for user interaction
- Did the user click the link or open the attachment?
- Check proxy/EDR logs for outbound connections matching the phishing infrastructure
- Check for new processes spawned around the time of the click

## Step 5: Determine scope
- Was this a single targeted email or part of a mass campaign?
- Search mail logs for the same sender/subject/URL sent to other users

## Step 6: Recommend action
- Block sender domain/IP and URL at the email gateway/proxy
- Force password reset if credentials may have been entered
- Notify affected users
- Escalate to Tier 2 if user interaction occurred or credentials were compromised
