# 🛡️ Module 03: Core SOC Solutions & Incident Triage
> **TryHackMe — SOC Level 1 Path**

---

## 📌 Overview

Hands-on SOC investigations covering SIEM operations, log analysis, query development, and endpoint threat triage.

---

## 1️⃣ Splunk: Log Ingestion & SPL Querying

### 🔹 Architecture

- **Forwarder:** Collects telemetry from endpoints.
- **Indexer:** Processes and stores events for searching.
- **Search Head:** Provides the interface for SPL queries and analysis.

### 🔹 Log Ingestion & Field Extraction

- Worked with newline-delimited JSON logs indexed as `VPN_Logs`.
- Used `spath` to extract fields from nested JSON data.
- Queried events based on specific user attributes.

---

```spl
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count

```

## 2️⃣ Elastic Stack (ELK): Log Investigation

### 🔹 Pipeline Workflow

**Beats ➜ Logstash ➜ Elasticsearch ➜ Kibana**

- Reviewed the role of each component in the log-processing pipeline.
- Applied time-based filtering to narrow investigation scopes.

### 🔹 Triage & Analysis

- Filtered events using specific attributes such as `UserName: Emanda`.
- Investigated `Source_ip` activity and connection patterns.
- Identified failed authentication activity using `action: failed`.

---

## 3️⃣ Endpoint Detection & Response (EDR)

### 🔹 Attack Scenarios

- **Initial Access:** Investigated a malicious Word document (`invoice.docm`) spawning `cmd.exe` and using `curl` to retrieve a payload.
- **Credential Access:** Investigated suspicious access to `lsass.exe` by `syncsvc.exe`, consistent with potential credential-dumping activity.
- **Persistence:** Identified persistence through Windows Run Registry Keys.
- **Network Activity:** Investigated outbound communication involving `files-wetransfer.com`.

### 🔹 Incident Triage

- Reviewed process execution chains and suspicious parent-child relationships.
- Analyzed Indicators of Compromise (IoCs) including file paths, processes, registry keys, and domains.
- Assessed alerts and identified relevant artifacts for further investigation and response.

---

## 💡 Practical SOC Skills Acquired

1. **Log Analysis:** Parsing and investigating semi-structured JSON events.
2. **SIEM Querying:** Using SPL and KQL-style queries to investigate security events.
3. **Alert Triage:** Scoping alerts and identifying relevant indicators.
4. **Endpoint Investigation:** Analyzing processes, command execution, persistence mechanisms, and suspicious network activity.
5. **IoC Extraction:** Identifying artifacts that can support further investigation and response.
