# Case 02 — Collection and Staging

## Objective
Identify commands that search for valuable files, access clipboard data, and prepare a staging directory.

## Reported findings
- Staging directory: `staging_58f1`.
- File extensions searched: `.docx`, `.pdf`, and `.xlsx`.
- PowerShell cmdlet shown in the notes: `Get-Clipboard`.
- The scenario identifies browser data, SSH key filenames, and a PDF as possible collection targets.
- Collection-related DNS destination reported in the notes: `collecteddata-storage-2025.s3.amazonaws.com`.

## Important caveats
- A command that searches for a file does not prove the file was copied or exfiltrated.
- A DNS query does not prove that an upload completed.
- The screenshot showing a browser credential-manager page is redacted in this package. No credential value is reproduced in the Markdown files.
- A screenshot lists SSH key filenames, but no private-key contents are included.

## Next validation steps
1. Correlate process creation with file creation/copy events.
2. Check whether the staging directory was populated.
3. Review network telemetry around the DNS query.
4. Preserve timestamps and hashes for any authorized lab artifacts.
