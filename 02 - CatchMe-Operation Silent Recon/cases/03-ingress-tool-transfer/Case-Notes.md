# Case 03 — Ingress Tool Transfer

## Objective
Document the lab's demonstration of multiple ways a file can be downloaded to a Windows endpoint.

## Methods demonstrated in the supplied notes
- Web browser navigation to a training URL.
- `curl.exe`.
- `certutil.exe -urlcache -f` in the lab exercise.
- PowerShell `Invoke-WebRequest` (IWR).

## Recorded training outcomes
- Browser: `THM{just_use_web_browser}`
- `curl.exe`: `THM{curl_is_cool}`
- `certutil.exe`: `THM{abusing_certutil}`
- PowerShell IWR: `THM{power_of_powershell}`

These are training-room flags, not credentials or production indicators. The package intentionally does not include downloaded executable payloads.

## Detection approach
Correlate process creation, command line, file creation, and outbound network activity. Review parent process and user context; the utilities are legitimate administrative tools and should not be blocked solely because they appear in a command line.
