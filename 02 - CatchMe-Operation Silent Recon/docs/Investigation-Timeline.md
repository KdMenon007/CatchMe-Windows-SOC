# Investigation Timeline

The timeline below is a behavioral sequence assembled from the supplied notes. Exact event times should be extracted from original logs if they become available.

1. **Initial access context** — training narrative describes a phishing-style executable being launched.
2. **User discovery** — first command reported by the lab: `whoami`.
3. **Privilege and host discovery** — command activity includes user/account inspection and host/network/security-tool checks.
4. **Security-tool discovery** — a command checks for `MsSense.exe` and prints a message if it is not found. This alone does not prove that Defender or EDR is absent.
5. **DNS activity** — supplied notes associate discovery-stage activity with `exfil.beecz.cafe`.
6. **Collection preparation** — a directory named `staging_58f1` is reported as created.
7. **File discovery and collection** — commands search for `.docx`, `.pdf`, and `.xlsx`; a PowerShell process uses `Get-Clipboard`.
8. **Potential sensitive-data targeting** — the scenario identifies browser data, SSH key filenames, and a PDF as collection targets. The package does not include private-key contents.
9. **DNS activity associated with collection/exfiltration** — supplied notes identify `collecteddata-storage-2025.s3.amazonaws.com`.
10. **Ingress tool transfer exercise** — separate lab steps demonstrate downloads using a browser, `curl.exe`, `certutil.exe`, and PowerShell IWR.

## Confidence notes

- High confidence: what is directly visible in the supplied screenshots.
- Medium confidence: behavior summarized in the user's lab notes but not independently cross-checked against raw event XML.
- Not established from this package: successful credential theft, actual upload completion, persistence, lateral movement, or impact on a real system.
