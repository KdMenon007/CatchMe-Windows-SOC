# Detection Engineering — Splunk SPL Examples

These are **starting-point searches**, not validated production detections. Adjust index/sourcetype and field extractions to match your Sysmon ingestion. Test against benign baselines and document false positives before enabling alerts.

## 1. Discovery command-line hunting

```spl
index=windows (sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" OR sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational") EventCode=1
| eval cmd=lower(coalesce(CommandLine, process_command_line, Process_Command_Line))
| where match(cmd, "(whoami|ipconfig|net user|net localgroup|tasklist|systeminfo|wmic|powershell.*get-service|netstat)")
| table _time host user Image ParentImage ProcessId ParentProcessId CommandLine
| sort 0 _time
```

## 2. Suspicious discovery sequence by host and short time window

```spl
index=windows EventCode=1
| eval cmd=lower(coalesce(CommandLine, process_command_line))
| where match(cmd, "(whoami|ipconfig|net user|net localgroup|tasklist|systeminfo|wmic|netstat)")
| bin _time span=5m
| stats count values(cmd) as commands values(ParentImage) as parent_images values(user) as users by host _time
| where count >= 4
| sort 0 _time
```

## 3. File-search and staging indicators

```spl
index=windows EventCode=1
| eval cmd=lower(coalesce(CommandLine, process_command_line))
| where match(cmd, "(get-childitem|dir /s|\.docx|\.pdf|\.xlsx|compress-archive|staging_[a-z0-9]+|get-clipboard)")
| table _time host user Image ParentImage CommandLine ProcessId ParentProcessId
| sort 0 _time
```

## 4. Ingress tool transfer utility usage

```spl
index=windows EventCode=1
| eval img=lower(coalesce(Image, process_path)), cmd=lower(coalesce(CommandLine, process_command_line))
| where (match(img, "\\(curl|certutil|powershell|pwsh|bitsadmin|wget)\.exe$") AND match(cmd, "(http://|https://|invoke-webrequest| iwr |urlcache|downloadfile|start-bitstransfer)"))
| table _time host user Image ParentImage CommandLine
| sort 0 _time
```

## 5. DNS query to lab indicators

```spl
index=windows EventCode=22
| eval query=lower(coalesce(QueryName, query, dns_query))
| where query="exfil.beecz.cafe" OR query="collecteddata-storage-2025.s3.amazonaws.com"
| table _time host ProcessId Image QueryName QueryResults
| sort 0 _time
```

## 6. Correlate process and DNS activity

```spl
index=windows (EventCode=1 OR EventCode=22)
| eval proc_id=coalesce(ProcessId, ProcessID, process_id)
| eval cmd=coalesce(CommandLine, Process_Command_Line)
| eval dns=coalesce(QueryName, query)
| stats min(_time) as first_seen max(_time) as last_seen values(EventCode) as event_ids values(Image) as images values(ParentImage) as parents values(cmd) as commands values(dns) as dns_queries by host proc_id
| convert ctime(first_seen) ctime(last_seen)
```

## Validation checklist
- Confirm Sysmon Event IDs are mapped to `EventCode` or change the filter to the field used in your environment.
- Confirm process and DNS fields are extracted correctly.
- Use process GUIDs when available to avoid PID reuse issues.
- Test legitimate IT activity and software-installation workflows.
- Add allowlists only when supported by known-good baselines.
- Do not mark a rule as validated until it has been tested against real lab telemetry.
