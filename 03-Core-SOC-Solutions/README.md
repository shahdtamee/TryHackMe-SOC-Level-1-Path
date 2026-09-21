# Module 03: Core SOC Solutions (SIEM & EDR Operations)
**TryHackMe - SOC Level 1 Path**

## 📌 Overview
This repository documents practical exercises and foundational investigations in modern Security Operations Center (SOC) environments. It focuses on hands-on log ingestion, search syntax, data visualization in SIEM tools (Elastic Stack and Splunk), and triage workflows within Endpoint Detection and Response (EDR) platforms.

---

## 1. Splunk: Log Ingestion & Search Processing Language (SPL)

### Key Takeaways:
* **Architecture:** Hands-on understanding of core components:
  * **Forwarder:** Shipped raw telemetry from endpoints.
  * **Indexer:** Parsed and indexed raw logs into searchable events.
  * **Search Head:** Interfaced for running SPL queries and generating reports.
* **Data Ingestion:** Successfully uploaded newline-delimited JSON logs into dedicated indexes (`index=VPN_Logs`).
* **Querying & Field Extraction:** 
  * Handled structured JSON formatting using the `| spath` command to extract dynamic field-value pairs.
  * Filtered targeted accounts and performed statistical aggregation to isolate anomalous activities.

```spl
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
2. Elastic Stack (ELK): Log Investigation & Data Views
Key Takeaways:
Core Components: Handled data streams utilizing the ELK pipeline (Beats -> Logstash -> Elasticsearch -> Kibana).

Discover Tab Exploration:

Configured absolute time boundaries to eliminate query noise and focus on critical incident windows.

Filtered security records based on parsed field values (e.g., UserName: Emanda).

Investigated client connections and isolated high-frequency Source IPs (Source_ip) and traffic anomalies.

Visualizations & Tables: Formatted raw JSON logs into structured tabular views and generated correlation tables for failed authentication attempts.

3. Endpoint Detection & Response (EDR) Investigation
Key Takeaways:
Alert Triage: Monitored real-time detections categorized by MITRE ATT&CK tactics and severity levels (e.g., Initial Access via Malicious Office Document, Credential Dumping via LSASS).

Indicator of Compromise (IoC) Extraction:

Traced process lineage: Document macros launching WINWORD.EXE -> cmd.exe -> curl staging payloads.

Extracted critical host and network artifacts:

Dropped Payloads: C:\Users\Public\install.exe

File Hash: SHA256 validation for malicious executables

Network Indicators: C2 domain resolution (ayebd.thm) and external IP connections

Response Actions: Initiated endpoint containment, artifact quarantine, and firewall domain blocks.

🛡️ Summary of Practical SOC Skills Acquired
Log Normalization: Parsing unstructured and semi-structured formats (JSON) to enable field-level investigation.

Search Discipline: Scoping precise query boundaries using SPL and KQL to distinguish legitimate traffic from brute-force or staging activity.

Artifact-Driven Response: Moving from high-level EDR detections to actionable IoC extraction and host remediation.
