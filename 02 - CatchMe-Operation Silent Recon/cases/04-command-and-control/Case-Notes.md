# Case 04 — DNS and Potential Command-and-Control / Exfiltration

## Reported DNS indicators
- `exfil.beecz.cafe`
- `collecteddata-storage-2025.s3.amazonaws.com`

## Interpretation
The training notes associate these domains with discovery-stage communication and collection/exfiltration. Treat them as lab-provided indicators that require validation against the actual Sysmon Event ID 22 records, DNS response, process ID, timestamps, and any available proxy/firewall/packet-capture telemetry.

A DNS query by itself does not prove command-and-control or successful data exfiltration. Check whether the process made a subsequent connection, whether data transfer occurred, and whether the destination is expected in the lab environment.

## Recommended triage
1. Identify the process and user associated with each DNS query.
2. Correlate the query time with process creation and staging activity.
3. Resolve and document the answer returned in the lab at the time of the event.
4. Check network telemetry for follow-on connections and transfer volume.
5. Keep these indicators scoped to the training environment unless independently validated.
