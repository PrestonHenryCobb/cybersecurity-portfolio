# Splunk Observations & Notes

Use this file to log takeaways from working in Splunk — quirks, useful SPL patterns discovered, dashboards built, etc.

## Example entries
- `transaction` is useful for correlating failed-to-successful logon events within a time window, but can be resource-intensive on large datasets; `stats`/`eventstats` is often a better-performing alternative.
- `sourcetype=WinEventLog:Security` requires the Splunk Add-on for Windows to parse Event IDs into searchable fields.
