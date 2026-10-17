# Case Track — Phishing Binary with Misleading Extension

## Scenario and evidence

The supplied training screenshot shows a file named `www.skype.com` presented as an **MS-DOS Application**, with a command window displaying the message `Just kidding, I am just a malware...` and the flag `THM{misleading_extension}`. The notes identify this as the Phishing Case 1 artifact.

## Assessment

The filename resembles a domain/website name, but the file type and execution behavior indicate a misleading-extension training sample. Do not treat the name as proof that the file is related to the legitimate Skype service.

## Analyst workflow

1. Preserve the sample without executing it again.
2. Record SHA-256, file type, signature and source path in an isolated lab.
3. Review available Sysmon process creation and file events around execution.
4. Check the parent process, user, command line and any network/DNS activity.
5. Record the observed training flag as a lab result, not as evidence of real-world compromise.

## MITRE ATT&CK mapping (working hypotheses)

- **T1566 — Phishing:** applicable only if delivery through a phishing message is established by the wider case evidence.
- **T1204.002 — User Execution: Malicious File:** relevant to a user launching a malicious executable.
- **T1036.007 — Double File Extension:** not directly proven by the exact filename `www.skype.com`; the case is more generally a misleading filename/extension scenario. Avoid claiming the double-extension sub-technique unless the artifact supports it.

## Limitation

The screenshot alone does not establish the initial delivery vector, persistence, network activity, or any impact beyond the training program output.
