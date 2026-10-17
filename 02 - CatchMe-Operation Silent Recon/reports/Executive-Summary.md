# Executive Summary

The supplied training evidence describes a simulated Windows attack chain covering discovery, collection/staging, ingress tool transfer, and DNS activity associated by the lab with exfiltration. Sysmon process-creation events provide the primary evidence for command execution and process lineage; Sysmon DNS-query events provide supporting context for domain lookups.

The strongest detection opportunity is a **sequence-based analytic** that correlates suspicious parent process lineage with multiple discovery commands, sensitive-file search activity, staging-directory creation, clipboard access, and subsequent DNS/network events. Individual commands and DNS lookups are not sufficient on their own to prove compromise.

The project package includes descriptive screenshots, case notes, ATT&CK hypotheses, a timeline, a CSV evidence register, and draft Splunk SPL. The searches are examples only and require field validation, baseline testing, and false-positive review before production use.

**Scope limitation:** Findings are based on user-supplied training notes and screenshots. Raw EVTX/PCAP files were not included, and no independent execution or telemetry validation was performed for this package.
