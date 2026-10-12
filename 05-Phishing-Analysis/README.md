
# 🎣 Module 05 — Phishing Analysis & Email Threat Investigation

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Focus](https://img.shields.io/badge/Focus-Blue%20Team-blue)
![Status](https://img.shields.io/badge/Status-Documenting-yellow)

## 📌 Overview

This module documents my hands-on learning and investigations from the **Phishing Analysis** section of the TryHackMe SOC Level 1 path.

The focus was on developing the practical skills required to identify, analyze, and investigate phishing attempts from a Security Operations Center (SOC) perspective.

Through guided labs and practical challenges, I examined suspicious emails, analyzed message headers, investigated malicious attachments, enriched threat indicators, and studied phishing prevention mechanisms.

Rather than relying on a single detection result, I focused on combining multiple sources of evidence to understand suspicious activity and support informed investigation decisions.

## 🎯 Learning Objectives

- Analyze raw email headers and MIME structures.
- Investigate sender spoofing and authentication results.
- Identify phishing URLs, domains, and suspicious attachments.
- Perform file hash and reputation analysis.
- Interpret dynamic malware sandbox reports.
- Analyze SMTP traffic using Wireshark.
- Extract and document Indicators of Compromise (IOCs).
- Understand phishing prevention through SPF, DKIM, and DMARC.
- Develop a structured approach to phishing incident triage.

## 🛠️ Tools & Technologies

| Category | Tools |
|---|---|
| Email Investigation | Mozilla Thunderbird, Raw Email Headers |
| Threat Intelligence | VirusTotal, IPinfo, Cisco Talos |
| Phishing Analysis | PhishTool, urlscan.io |
| Malware Analysis | ANY.RUN |
| Network Analysis | Wireshark |
| Data Processing | CyberChef |
| Command Line | Linux, sha256sum |
| Security Frameworks | MITRE ATT&CK |

Some tools were used in practical exercises, while others were studied as part of the investigation workflow.

## 📚 Completed Rooms

| TryHackMe Room | Main Focus | Status |
|---|---|---|
| Phishing Analysis Fundamentals | Email architecture, headers, MIME | ✅ Completed |
| Phishing Emails in Action | Phishing techniques and suspicious indicators | ✅ Completed |
| Phishing Analysis Tools | Threat intelligence and malware analysis | ✅ Completed |
| Phishing Prevention | Email security controls and SMTP traffic analysis | ✅ Completed |
| The Greenholt Phish | Suspicious email and attachment investigation | ✅ Completed |
| Snapped Phish-ing Line | Phishing kit and infrastructure investigation | ✅ Completed |

## 🔬 Technical Documentation

The detailed walkthroughs are organized into separate files to make the investigations easier to follow.

| Document | Description |
|---|---|
| [Email Fundamentals & Prevention](01-Email-Fundamentals.md) | Email structure, header analysis, MIME, SPF, DKIM, DMARC, and SMTP |
| [Phishing Investigation Tools](02-Phishing-Investigation-Tools.md) | VirusTotal, IPinfo, ANY.RUN, and Wireshark analysis with screenshots |
| [The Greenholt Phish](03-Greenholt-Phish.md) | Malicious attachment investigation and threat intelligence findings |
| [Snapped Phish-ing Line](04-Snapped-Phishing-Line.md) | Phishing kit investigation, hashing, and infrastructure analysis |

## 🔎 Practical Investigation Highlights

### 1. Email Header and Attachment Analysis

Inspected raw email source to identify sender information, authentication indicators, MIME fields, and Base64-encoded attachments.

### 2. Threat Intelligence Enrichment

Investigated suspicious files and IP addresses using VirusTotal and IPinfo, including limitations of malware detection scores and reputation data.

### 3. Dynamic Malware Analysis

Reviewed ANY.RUN sandbox results to understand suspicious process execution, network connections, and exploit-related indicators.

### 4. SMTP Network Traffic Investigation

Used Wireshark to examine SMTP traffic, interpret server responses, and reconstruct email communications through TCP streams.

### 5. Practical Phishing Challenges

Investigated a suspicious email attachment in **The Greenholt Phish** and phishing infrastructure and archive artifacts in **Snapped Phish-ing Line**.

Detailed evidence, screenshots, technical explanations, and findings are included in the linked investigation documents.

## 🧠 Key Takeaways

- An email's visible sender identity is not sufficient to establish authenticity.
- SPF, DKIM, and DMARC are important security controls but do not eliminate phishing.
- A clean antivirus detection score does not guarantee that a file is harmless.
- File hashes and threat intelligence help connect suspicious artifacts to known threats.
- Dynamic analysis can reveal behavior not visible from static inspection alone.
- Network traffic analysis provides useful context for email security investigations.
- Reliable SOC investigations require evidence correlation, documentation, and appropriate escalation.

## 🚀 Future Work

I plan to document Microsoft Sentinel alert investigations and Splunk-based security investigations as separate hands-on projects.

These projects will focus on practical SIEM workflows, alert triage, log analysis, and incident investigation.

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
**Focus:** SOC Analysis | Blue Team | Phishing Investigation

