# Publication Checklist — Operation Shadow Persistence

This is an **updated review build**, not yet approved for public publication.

- [x] Preserve originals locally; do not include raw EVTX, ZIP, or LNK artifacts in this package.
- [x] Record SHA-256 hashes for six EVTX files, the ZIP archive, and the LNK.
- [x] Correct the evidence register to distinguish the two different after-reboot EVTX filenames.
- [x] Update the investigation timeline and incident-response report with parsed event IDs, record IDs, timestamps, and caveats.
- [x] Keep DNS query attempts separate from claims of successful command-and-control.
- [ ] Verify timestamps and all key fields against the original EVTX XML.
- [ ] Confirm provenance/host context for the distinct after-reboot EVTX exports.
- [ ] Verify ATT&CK technique identifiers and names against current official documentation.
- [ ] Review/redact every screenshot before adding it to the public repository. Screenshots are withheld from this review build.
- [ ] Run a secret scan on the final public file set.
- [ ] Review every file and the upload list immediately before committing through the GitHub website.
- [ ] Regenerate and verify the package checksum manifest after all approved changes.

Do not upload raw logs, suspicious archives, executable samples, parsed raw event dumps, backups, credentials, or private lab artifacts.
