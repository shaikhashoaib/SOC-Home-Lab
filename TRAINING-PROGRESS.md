# SOC Analyst L1 — Training & Progress Log

This file tracks completed training and practical exercises. Only completed activities are marked as completed. The TryHackMe SOC Level 1 path is currently in progress.

## Credentials and courses

| Platform | Course / learning path | Status | Credential |
|---|---|---|---|
| LetsDefend | SOC Analyst Learning Path | Completed — November 2026 | Completion certificate earned |
| Udemy | SOC Analyst Level 1 & 2 Masterclass — Splunk, Wazuh and investigations | Course completed | No separate professional certification claimed |
| TryHackMe | SOC Level 1 | In progress — Premium active November 2026 | Path certificate not yet earned |

## TryHackMe rooms completed

- [x] Junior Security Analyst Intro
- [x] SOC Role in Blue Team
- [x] Humans as Attack Vectors
- [x] Systems as Attack Vectors
- [x] SOC L1 Alert Triage
- [x] SOC L1 Alert Reporting
- [x] SOC Workbooks and Lookups
- [x] SOC Metrics and Objectives
- [x] Phishing Emails in Action
- [x] ParrotPost: Phishing Analysis

The completed rooms provide guided practice in SOC and blue-team roles, human-focused and systems-focused attack vectors, SOC L1 alert triage, alert reporting, escalation procedures and SOC communication, use of asset-inventory and network-map lookups, SOC workbooks, SOC performance metrics including MTTD, MTTA and MTTR, false-positive and threat-detection rates, phishing-email investigation, and phishing analysis. The full SOC Level 1 path contains 14 sections. Completion of individual rooms does not mean the full path is complete.

## Practical experience — personal home lab

### Splunk and Windows security monitoring

- Installed and accessed Splunk Enterprise on a Windows 10 host.
- Configured Windows Event Log collection.
- Queried and reviewed Windows authentication events:
  - Event ID 4624 — successful logon
  - Event ID 4625 — failed logon

### Linux authentication monitoring

- Set up an Ubuntu Server virtual machine using Oracle VirtualBox.
- Installed and configured Splunk Universal Forwarder.
- Forwarded Linux `/var/log/auth.log` events to Splunk.
- Reviewed SSH authentication events.

### Detection engineering fundamentals

- Generated controlled failed SSH login attempts in the personal lab.
- Wrote an SPL search to extract usernames and source IP addresses and count repeated failures.
- Configured a threshold-based detection for three or more failures within a five-minute search window.
- Scheduled and validated a Splunk alert.

All activity above is simulated home-lab practice, not production SOC employment or investigation of a real external incident.

## Topics practised

- SOC and blue-team roles
- Human-focused attack vectors
- Introductory security operations
- Phishing email analysis
- Splunk search fundamentals
- Windows authentication logs
- Linux authentication logs
- SSH failed-login detection
- Basic alert configuration and validation
- Alert triage and true-positive classification in a guided SOC simulation
- Writing alert comments and case reports for L2 handoff
- Understanding escalation procedures and cross-team communication
- Using existing asset-inventory and network-map lookups to add context during alert investigations
- Understanding how SOC workbooks standardise and streamline analyst procedures
- Understanding SOC internal metrics: false-positive rate and threat-detection rate
- Understanding SOC performance metrics: Mean Time to Detect (MTTD), Mean Time to Acknowledge (MTTA), and Mean Time to Respond (MTTR), including their relevance to SLAs
- Learning how SOC metrics can be tracked and improved
- Incident evidence documentation

## SOC Level 1 learning path — planned coverage

The following topics are included in the path and will be marked as completed only after the corresponding training and exercises are finished:

1. Blue Team Introduction
2. SOC Team Internals
3. Core SOC Solutions: EDR, SIEM, Splunk, Elastic and SOAR
4. Cyber Defence Frameworks
5. Phishing Analysis
6. Network Traffic Analysis
7. Network Security Monitoring
8. Web Security Monitoring
9. Windows Security Monitoring
10. Linux Security Monitoring
11. Malware Concepts for SOC
12. Threat Analysis Tools and Cyber Threat Intelligence
13. SIEM Triage for SOC
14. SOC Level 1 Capstone Challenges

## Next milestones

- [ ] Complete remaining SOC Level 1 rooms.
- [ ] Practise alert triage and incident reporting.
- [ ] Complete network, Windows and Linux monitoring exercises.
- [ ] Complete SIEM triage exercises using Splunk and Elastic.
- [ ] Complete Tempest and Boogeyman capstone challenges.
- [ ] Earn the TryHackMe SOC Level 1 learning-path completion certificate.
- [ ] Add evidence-backed investigation reports and redacted screenshots to this repository.

---

**Integrity note:** This portfolio distinguishes training, simulated lab practice, and earned course-completion certificates from professional employment and vendor-issued professional certifications.
