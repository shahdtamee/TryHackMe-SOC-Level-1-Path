# 🛡️ Cyber Defence Frameworks & Threat Intelligence Mapping

Practical documentation from the **TryHackMe SOC Level 1 Path**, focusing on how Blue Teams and SOC analysts use cybersecurity frameworks and threat intelligence to understand attacks, map adversary behaviour, and develop detection strategies.

---

## 📌 Module Overview

This module covers:

- **Pyramid of Pain** — Understanding IOC types and measuring the impact of detection on adversaries.
- **Cyber Kill Chain** — Analysing traditional stages of a cyber attack.
- **Unified Kill Chain (UKC)** — Mapping the complete attack lifecycle across 18 phases.
- **MITRE ATT&CK & D3FEND** — Mapping adversary behaviour to defensive techniques.
- **Hands-on Labs** — Applying threat intelligence, malware analysis, Sigma rules, and defensive controls.

![Module Completion](screenshots/module-completion.png)

---

## 1. 🔗 Unified Kill Chain — 18 Attack Phases

The **Unified Kill Chain (UKC)** provides a broader view of the attack lifecycle than traditional perimeter-focused models.

The 18 phases are organised into three main stages:

### 🔵 In — Initial Foothold
1. Reconnaissance
2. Weaponization
3. Delivery
4. Social Engineering
5. Exploitation
6. Persistence
7. Defense Evasion
8. Command & Control (C2)
9. Pivoting

### 🟠 Through — Network Propagation
10. Discovery
11. Privilege Escalation
12. Execution
13. Credential Access
14. Lateral Movement

### 🔴 Out — Action on Objectives
15. Collection
16. Exfiltration
17. Impact
18. Objectives

This framework was used to understand how an attacker can progress from initial reconnaissance and access to lateral movement, data theft, and final objectives.

![Unified Kill Chain](screenshots/unified-kill-chain.png)

---

## 2. 🎯 Threat Actor Profiling & MITRE ATT&CK

Threat intelligence can be mapped to **MITRE ATT&CK** to better understand adversary behaviour and identify potential defensive coverage gaps.

### Key Activities:
- Analysed threat actor group profiles such as:
  - `APT33`
  - `APT28`
  - `Mustang Panda [G0129]`
- Navigated the **MITRE ATT&CK Enterprise Matrix** and identified tactics including:
  - Reconnaissance — `TA0043`
  - Execution — `TA0002`
  - Persistence — `TA0003`
  - Defense Evasion — `TA0005`
  - Command & Control — `TA0011`
  - Exfiltration — `TA0010`
- Practiced distinguishing between:
  - **Parent Techniques** such as `T1598`
  - **Sub-techniques** such as `T1598.003`
- Analysed techniques that can appear across different stages of an attack, such as phishing-related activity.

![Mustang Panda Reconnaissance & Navigator](screenshots/navigator-analysis.png)

![APT28 Full TTP Mapping](screenshots/apt28-navigator-ttps.png)

### 🛡️ MITRE D3FEND & Mitigations

Explored the relationship between offensive techniques and defensive countermeasures using **MITRE D3FEND** and ATT&CK Mitigations.

* **User Geolocation Logon Pattern Analysis — `D3-UGLPA`**: Used to identify anomalous authentication attempts by inspecting network traffic.
* **User Account Management — `M1018`**: Mitigating exposure to valid cloud accounts (`T1078.004`) by regularly auditing and removing inactive or unnecessary accounts.

![Cloud Accounts Mitigation](screenshots/mitre-mitigation-cloud.png)

---

## 3. 🔍 Practical Labs & Detection Engineering

### A. Malware Triage & Firewall Mitigation

Performed practical malware analysis activities using a sandboxed environment:

- Performed basic static and dynamic malware analysis on suspicious binaries.
- Extracted cryptographic hashes:
  - `MD5`
  - `SHA256`
- Identified network-related Indicators of Compromise (IOCs) and process behaviors (e.g., attempts to disable security tooling).
- Applied outbound firewall controls using **Egress / Deny rules** to block active communication channels toward external C2 infrastructure over `443/TCP`.

![Malware Sandbox Analysis](screenshots/sample4-sandbox-analysis.png)

![Sphinx Detection & Flag Retrieval](screenshots/sphinx-flag-success.png)

---

### B. Sysmon & Sigma Detection Engineering

Practiced creating behavioural detection rules using **Sysmon events** and **Sigma**, mapped directly to MITRE ATT&CK tactics:

#### 1. Defense Evasion — `TA0005`
* **Detection objective:** Detect attempts to tamper with Windows Defender to disable real-time protection.
* **Mechanism:** Sysmon Registry Event monitoring.
* **Target Key/Value:** `DisableRealtimeMonitoring = 1`.

#### 2. Command & Control — `TA0011`
* **Detection objective:** Identify periodic automated outbound connections indicating C2 beaconing.
* **Observed characteristics:**
  - Regular connection interval: `1800 seconds` (30 minutes).
  - Uniform packet size: `97 bytes`.
  - Destination abstraction: `Remote IP: Any` (resilient against dynamic adversary infrastructure).

#### 3. Data Staging & Exfiltration — `TA0010`
* **Detection objective:** Detect local aggregation and staging of system discovery commands prior to network exfiltration.
* **Sysmon Event:** Event ID `11` — File Creation.
* **Observed indicator:** Tracking automated command output redirection into temporary storage:
  ```text
  %temp%\exfiltr8.log
