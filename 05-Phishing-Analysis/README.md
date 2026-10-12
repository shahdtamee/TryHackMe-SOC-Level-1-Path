
# 🎣 Module 05 — Phishing Analysis & Email Threat Investigation

> **TryHackMe — SOC Level 1 Path**  
> Email Security | Threat Intelligence | Malware Analysis | Network Investigation

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team-blue)

---

## 📌 Module Overview

This module documents my hands-on experience investigating phishing emails and malicious artifacts through the **TryHackMe SOC Level 1 Path**.

I practiced inspecting raw email headers, identifying suspicious domains and attachments, analyzing malware reputation and sandbox reports, and investigating SMTP traffic.

The main objective was to build a structured phishing investigation workflow based on technical evidence rather than relying on a single security tool.

### Completed Rooms

| Room | Focus |
|---|---|
| Phishing Analysis Fundamentals | Email headers, MIME, attachments |
| Phishing Emails in Action | Spoofing, phishing indicators, suspicious links |
| Phishing Analysis Tools | Threat intelligence and malware analysis |
| Phishing Prevention | Email authentication and SMTP investigation |
| The Greenholt Phish | Malicious attachment investigation |
| Snapped Phish-ing Line | Phishing kit and infrastructure investigation |

---

## 📧 1. Email Fundamentals & Header Investigation

### Email Protocols

- **SMTP:** Used to send and relay email messages.
- **IMAP:** Allows users to access and synchronize emails across devices.
- **POP3:** Retrieves emails from a mail server.

Understanding email delivery helps analysts identify where suspicious messages originated and how they traveled through mail infrastructure.

### Important Email Headers

| Header | Investigation Purpose |
|---|---|
| From | Visible sender identity |
| Return-Path | Envelope sender address |
| Reply-To | Destination of replies |
| Received | Mail delivery hops |
| Authentication-Results | SPF, DKIM, and DMARC results |
| Message-ID | Message identifier |

### Practical Investigation — Suspicious Sender

Using Mozilla Thunderbird, I inspected the raw source of a suspicious email.

The headers revealed:

```text
Return-Path: <postmaster@etekno.xyz>
X-Originating-IP: [209.85.167.226]
Received-SPF: none
```

The domain `etekno[.]xyz` was identified as an indicator requiring further investigation.

**Analysis:** A mismatch between the visible sender identity and underlying email infrastructure may indicate impersonation. However, SPF `none` alone does not prove that an email is malicious.

![Email Header Investigation](email-header-spf-investigation.png)

*Figure 1: Investigating the Return-Path, originating IP, and SPF results in Thunderbird.*

### MIME and Base64 Attachments

MIME enables email messages to carry different content types and attachments.

In a training email, I inspected the source file `email2.txt` and identified:

```text
Content-Type: application/pdf; name="zmqpalgh.pdf"
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="zmqpalgh.pdf"
```

The message contained a Base64-encoded PDF attachment.

**Key learning:** Base64 is an encoding mechanism, not encryption. Analysts can reconstruct encoded attachments in an isolated investigation environment.

![MIME Attachment Investigation](email-mime-attachment.png)

*Figure 2: Identifying a Base64-encoded PDF attachment through MIME headers.*

---

## 🛡️ 2. Email Authentication & Phishing Prevention

### SPF — Sender Policy Framework

SPF specifies which sending servers are authorized by a domain.

Example:

```text
v=spf1 ip4:192.0.2.10 -all
```

### DKIM — DomainKeys Identified Mail

DKIM uses cryptographic signatures to verify selected email content and headers.

### DMARC — Domain-Based Message Authentication

DMARC checks domain alignment using SPF and/or DKIM and specifies handling policies.

| Policy | Function |
|---|---|
| `p=none` | Monitoring |
| `p=quarantine` | Request suspicious-message treatment |
| `p=reject` | Request rejection of failing messages |

**SOC takeaway:** SPF, DKIM, and DMARC provide important authentication evidence, but successful authentication does not guarantee an email is safe.

Other preventive controls include secure email gateways, URL filtering, sandboxing, user awareness, and MFA.

---

## 🔎 3. Threat Intelligence & Malware Analysis

### VirusTotal — File Reputation

I used VirusTotal to examine file reputation and security vendor detection results.

**Example A — Password-Protected PDF**

| Finding | Result |
|---|---|
| File type | PDF |
| Size | 3.30 MB |
| Detection | 0/66 |
| Limitation | Password-protected content |

![VirusTotal Encrypted PDF](virustotal-encrypted-pdf.png)

*Figure 3: VirusTotal displaying zero detections while warning that encrypted content may not have been fully inspected.*

**Analyst finding:** A file with zero antivirus detections is not automatically safe, especially when security engines cannot inspect its contents.

### IPinfo — Infrastructure Enrichment

I investigated IP address `35.89.44.33` using IPinfo.

| Property | Finding |
|---|---|
| Location | Boardman, Oregon, US |
| ASN | AS16509 — Amazon |
| Hostname | omta34.uswest2.a.cloudfilter.net |
| Company | Proofpoint, Inc. |

The page also displayed an abuse-related warning.

![IPinfo IP Enrichment](ipinfo-ip-enrichment.png)

*Figure 4: IP enrichment showing hosting infrastructure and associated provider information.*

**Analyst finding:** An abuse warning is not proof of malicious activity. The role of an IP address, particularly within email security infrastructure, must be considered before reaching a conclusion.

### ANY.RUN — Dynamic Malware Analysis

ANY.RUN provides visibility into file behavior in an isolated sandbox.

I examined sandbox reports containing:

- Process execution information
- HTTP and network requests
- Suspicious activity indicators
- Malware classifications

![ANY.RUN PDF Analysis](anyrun-pdf-dynamic-analysis.png)

*Figure 5: Inspecting sandbox behavior and network activity associated with a suspicious PDF.*

In another investigation, the sandbox report identified:

| Indicator | Finding |
|---|---|
| File format | Microsoft Excel |
| Classification | Trojan |
| Associated vulnerability | CVE-2017-11882 |
| Suspicious IP | 204.11.56.48 |
| Suspicious domain | findresults.site |

![ANY.RUN Malware Investigation](anyrun-malware-iocs-cve.png)

*Figure 6: Reviewing malware classification, exploit tagging, and indicators from sandbox analysis.*

**Technical learning:** CVE-2017-11882 relates to remote code execution in Microsoft's Equation Editor. Malicious Office documents can exploit vulnerable client software.

The observed sandbox indicators support further investigation and must be correlated with endpoint and network evidence.

---

## 🌐 4. Wireshark — SMTP Traffic Investigation

I analyzed `traffic.pcap` using Wireshark to understand email communication at the network level.

### SMTP Analysis

The investigation involved:

1. Identifying SMTP traffic.
2. Examining server response codes.
3. Applying Wireshark display filters.
4. Inspecting relevant packets.
5. Reconstructing conversations with Follow TCP Stream.

Common SMTP responses include:

| Code | Meaning |
|---|---|
| 220 | Service ready |
| 250 | Requested action completed |
| 354 | Start mail input |
| 550 | Requested action rejected |

![Wireshark SMTP Traffic](wireshark-smtp-traffic.png)

*Figure 7: Inspecting the packet capture during SMTP response analysis.*

### Follow TCP Stream

Using **Follow TCP Stream**, I examined an SMTP conversation containing:

```text
EHLO mozilla.org
MAIL FROM:<postmaster@mozilla.org>
RCPT TO:<talkback@mozilla.org>
DATA
Subject: delivery failed
```

The exercise included analysis of 512 SMTP packets.

![Wireshark Follow TCP Stream](wireshark-tcp-stream.png)

*Figure 8: Reconstructing an SMTP conversation and examining email headers and delivery commands.*

**SOC takeaway:** SMTP traffic analysis helps investigators identify communicating systems, delivery status, sender and recipient information, and message content when the traffic is readable.

---

## 🚨 5. Practical Challenge — The Greenholt Phish

**Objective:** Investigate a suspicious financial-themed email and its attachment.

### Investigation Workflow

1. Opened the message using Thunderbird.
2. Inspected the raw email source.
3. Identified the attachment metadata.
4. Investigated the attachment's file hash and reputation.
5. Reviewed security vendor detections.

### Attachment Analysis

The source identified an attachment beginning with:

```text
SWT_#09674321
```

The attachment had a `.CAB` extension and was represented using Base64.

![Greenholt Attachment](greenholt-email-attachment.png)

*Figure 9: Examining the email source and identifying the suspicious attachment.*

### VirusTotal Findings

| Property | Result |
|---|---|
| Size | Approximately 400.26 KB |
| Detection | 49/64 |
| Popular threat label | trojan.msil/loki |
| Additional labels | msil, loki, agensla |

![VirusTotal Malicious Attachment](virustotal-malicious-cab.png)

*Figure 10: VirusTotal showing multiple malicious detections associated with the attachment.*

### Investigation Verdict

**Assessment: Malicious attachment — high confidence based on available reputation evidence.**

The detection results and suspicious attachment context supported a malicious-file classification.

**Recommended SOC response:**

- Quarantine the email.
- Identify other recipients of the same attachment.
- Search for related attachment hashes in security logs.
- Investigate whether the file was opened or executed.
- Escalate potential endpoint compromise.

These are recommended actions, not claims of remediation performed during the lab.

---

## 🎯 6. Practical Challenge — Snapped Phish-ing Line

**Objective:** Investigate phishing infrastructure and a phishing kit archive.

A phishing kit can contain files used to create fraudulent websites, including login page templates, scripts, and credential collection components.

### Investigation Workflow

1. Identified suspicious phishing-related infrastructure.
2. Accessed the authorized training environment.
3. Downloaded the relevant archive.
4. Prepared to calculate its SHA-256 hash.
5. Investigated the role of file reputation and archive contents.

### Phishing Kit Artifact

The investigated archive was named:

```text
Update365.zip
```

Associated domain observed during the exercise:

```text
kennaroaads[.]buzz
```

### Hashing Investigation

Command:

```bash
cd ~/Downloads
sha256sum Update365.zip
```

SHA-256 provides a repeatable file fingerprint that can be used for reputation lookups and incident correlation.

![Snapped Phishing Kit](snapped-phishing-kit-hash.png)

*Figure 11: Preparing the SHA-256 calculation for the phishing kit archive.*

**Evidence note:** The saved screenshot shows the command before execution; the resulting hash is not visible.

### Key Learning

A phishing investigation may involve tracing relationships between emails, domains, websites, downloadable artifacts, and potential credential-harvesting infrastructure.

This provides more context than investigating the suspicious email alone.

---

## 🧠 7. SOC Investigation Methodology

The module helped me develop a repeatable phishing triage workflow:

| Stage | Analyst Action |
|---|---|
| 1. Initial Triage | Examine the reported email and preserve evidence |
| 2. Header Analysis | Review sender identity and authentication |
| 3. IOC Extraction | Identify URLs, domains, IPs, hashes, and attachments |
| 4. Enrichment | Investigate reputation and infrastructure |
| 5. Behavioral Analysis | Review sandbox activity when necessary |
| 6. Assessment | Correlate evidence and determine a verdict |
| 7. Reporting | Document findings and recommend appropriate action |

### Skills Developed

- Email header and MIME analysis
- Phishing indicator identification
- File hash and reputation investigation
- IP address enrichment
- Sandbox report interpretation
- SMTP traffic and TCP stream analysis
- IOC extraction and documentation
- Evidence-based security analysis

---

## 🚀 Future Work

I plan to document more advanced hands-on SOC investigations separately, including:

- **Microsoft Sentinel:** Alert-by-alert triage, KQL queries, log analysis, and investigation reporting.
- **Splunk:** Security log searching, event correlation, and alert investigation.

These future projects will focus on practical SIEM investigations rather than repeating the phishing fundamentals covered here.

---

## 📚 References

- [TryHackMe](https://tryhackme.com/)
- [VirusTotal](https://www.virustotal.com/)
- [ANY.RUN](https://any.run/)
- [IPinfo](https://ipinfo.io/)
- [Wireshark](https://www.wireshark.org/)
- [MITRE ATT&CK](https://attack.mitre.org/)

---

**Author:** [Shahd Tamee](https://github.com/shahdtamee)  
**Learning Path:** TryHackMe SOC Level 1  
**Focus:** Blue Team | SOC Analysis | Phishing Investigation
