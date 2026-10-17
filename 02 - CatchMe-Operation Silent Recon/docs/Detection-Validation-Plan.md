# Detection Validation Plan

1. Confirm Sysmon is configured to log process creation (Event ID 1) and DNS queries (Event ID 22).
2. Verify ingestion and timestamp normalization in Splunk.
3. Check field extraction for `Image`, `CommandLine`, `ParentImage`, `ProcessId`, `ParentProcessId`, `ProcessGuid`, `QueryName`, and `QueryResults`.
4. Run each search against the supplied lab time window.
5. Compare hits to the screenshots and original event XML where available.
6. Test known-good administrative behavior to estimate false positives.
7. Tune thresholds and allowlists based on evidence.
8. Document the final test date, expected result, actual result, and analyst.
9. Do not claim successful detection validation until the query has been run against telemetry.
