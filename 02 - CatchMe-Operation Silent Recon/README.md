# CatchMe Project 02 — Operation Silent Recon

**Focus:** Windows discovery, collection, ingress tool transfer, and command-and-control investigation  
**Portfolio track:** CatchMe SOC Threat Hunting Series  
**Evidence basis:** User-supplied screenshots and notes from a controlled TryHackMe training scenario.

## Executive summary

This project documents a simulated Windows intrusion chain in which a phishing-style payload performs host and user discovery, searches for sensitive files and browser data, stages collected files, and makes DNS/network requests associated with a training exfiltration scenario. A separate set of exercises demonstrates ingress tool transfer using a browser, `curl.exe`, `certutil.exe`, and PowerShell `Invoke-WebRequest`.

The purpose is to show how a SOC analyst can build a timeline from Sysmon process-creation and DNS-query events, correlate parent/child processes, identify suspicious discovery and collection behavior, and translate evidence into detection opportunities.

> **Lab-safety and publication note:** This repository documents a training simulation. No malware binaries, stolen archives, private-key contents, or raw credential values are included. A browser credential screenshot has been redacted for publication. Verify every artifact and domain against the original lab before presenting the project as independently reproduced.

## Scope

- Discovery: user/privilege, host, application, process, network, and security-tool discovery.
- Collection: targeted file searches, staging-directory creation, browser data and clipboard access indicators.
- Ingress tool transfer: browser and command-line download methods demonstrated by the training exercise.
- Command and control / exfiltration: DNS-query evidence and the lab-provided destination indicators.
- Detection engineering: Sysmon Event ID 1 and Event ID 22, process lineage, command-line patterns, and correlated sequences.

## Repository map

- `docs/` — investigation notes, lab architecture, timeline, ATT&CK mapping.
- `cases/` — case-specific findings for each stage.
- `detections/` — detection hypotheses, example Splunk SPL, triage guidance.
- `reports/` — executive summary and incident-response report.
- `evidence/screenshots/` — supplied screenshots with descriptive names.
- `Evidence-Register.csv` — screenshot index and provenance notes.

## Key observations from supplied notes

- The first command executed by the simulated discovery payload was reported as `whoami`.
- A command-line check for Microsoft Defender-related process `MsSense.exe` was reported.
- The discovery-stage destination supplied in the notes was `exfil.beecz.cafe`.
- The collection-stage activity created a directory named `staging_58f1`, searched for `.docx`, `.pdf`, and `.xlsx` files, and used PowerShell `Get-Clipboard`.
- The collection-stage DNS destination supplied in the notes was `collecteddata-storage-2025.s3.amazonaws.com`.
- Ingress tool transfer training flags were recorded for browser, `curl.exe`, `certutil.exe`, and PowerShell IWR in the case notes. Flags are retained as lab outcomes, not as production indicators.

## How to use this project

1. Read the case notes and evidence register.
2. Review the screenshots and identify the exact fields visible in each event.
3. Validate the proposed detections against your own lab telemetry before claiming they alert successfully.
4. Record host, timestamps, process GUID/PID, parent process, user, command line, DNS query, and query result where available.
5. Keep any newly collected raw logs, secrets, malware samples, and unredacted screenshots out of a public repository.

## Limitations

The package contains screenshots and user-provided notes, not the original EVTX/PCAP data. Some conclusions therefore remain reported lab observations rather than independently validated findings. Domain names, timestamps, process lineage, and event fields should be rechecked against source logs when available.
