# 🛡️ Module 03: Core SOC Solutions & Incident Triage
> **TryHackMe — SOC Level 1 Path**

---

## 📌 Overview
Hands-on laboratory investigations covering SIEM operations, query design, and endpoint threat response within modern SOC environments.

---

## 1️⃣ Splunk: Log Ingestion & SPL Querying

### 🔹 Architecture
* **Forwarder:** Telemetry collection from endpoints.
* **Indexer:** Normalizes data into searchable events.
* **Search Head:** SPL query engine and visualization interface.

### 🔹 Ingestion & Field Extraction
* Ingested newline-delimited JSON logs into index `VPN_Logs`.
* Executed field extraction using `| spath` to normalize nested JSON objects into searchable key-value pairs.


```spl
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count

```

## 2️⃣ Elastic Stack (ELK): Log Investigation

### 🔹 Pipeline Workflow
* **Ingestion Pipeline:** Beats ➔ Logstash ➔ Elasticsearch ➔ Kibana.
* **Scoping:** Applied strict absolute time filters to isolate anomalies without query noise.

### 🔹 Triage & Visualizations
* Filtered logs by specific attributes (`UserName: Emanda`).
* Correlated `Source_ip` with connection spikes and monitored brute-force patterns via `action: failed`.

---

## 3️⃣ Endpoint Detection & Response (EDR)

### 🔹 Attack Scenarios
* **Initial Access:** Malicious Word document (`invoice.docm`) spawning `cmd.exe` and calling `curl` to stage payloads.
* **Credential Access:** Flagged `syncsvc.exe` attempting memory dumps from `LSASS`.
* **Persistence & Exfiltration:** Persistence established via Run registry keys; traffic flagged outbound to `files-wetransfer.com`.

### 🔹 Response Actions
* Host isolation, artifact quarantine (`C:\Users\Public\install.exe`), and domain blocking.

---

## 💡 Practical SOC Skills Acquired
1. **JSON Log Parsing:** Handling semi-structured events with field extraction.
2. **Search Discipline:** Scoping precise event windows using SPL and KQL.
3. **Artifact Analysis:** Moving from alert triage to root-cause IoC extraction.
