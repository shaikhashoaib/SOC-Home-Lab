# SOC Analyst L1 — Security Operations Home Lab

A practical cybersecurity portfolio documenting hands-on work with SIEM monitoring, Windows and Linux log collection, authentication-event analysis, and alert creation.

**Author:** Shaikh A Shoaib  
**Focus:** SOC Analyst L1 | SIEM | Security Monitoring | Incident Investigation

---

## Project overview

This home lab uses **Splunk Enterprise on Windows** and **Ubuntu Server** in Oracle VirtualBox to practise foundational Security Operations Center (SOC) workflows.

The project demonstrates:

- Collecting and searching Windows Security Event Logs.
- Forwarding Linux authentication logs to Splunk.
- Reviewing successful and failed authentication events.
- Creating a threshold-based detection for repeated SSH login failures.
- Configuring and validating a scheduled Splunk alert.
- Recording configuration, investigation steps, and observed results.

This is a personal learning environment, not a production SOC deployment.

## Skills and learning

### LetsDefend — SOC Analyst Learning Path

**Status:** Completed  
**Completion:** November 2026  
**Credential:** Course completion certificate earned.

Skills covered in the learning path:

- Security Operations Center (SOC)
- Security Information and Event Management (SIEM)
- Incident Investigation
- Threat Detection
- Network Security

### Practical home-lab skills

- Splunk Enterprise and foundational SPL searches
- Windows Event Log collection and review
- Linux authentication log monitoring
- Splunk Universal Forwarder configuration
- SSH authentication-event analysis
- Threshold-based detection logic
- Scheduled alert configuration and validation
- VirtualBox networking and Linux command-line fundamentals

## Lab architecture

| Component | Configuration |
|---|---|
| Host operating system | Windows 10 Pro 64-bit |
| Host hardware | HP ProDesk 400 G1 SFF, Intel Core i3-4160, 8 GB RAM |
| Virtualisation | Oracle VirtualBox |
| Linux virtual machine | Ubuntu Server 24.04.5 LTS |
| SIEM | Splunk Enterprise 10.6.1.1 on Windows |
| Linux log forwarder | Splunk Universal Forwarder 10.4.3 |
| Splunk receiving port | TCP 9997 |
| Linux log source | `/var/log/auth.log` |
| Windows telemetry | Windows Event Logs, including Security |

**Resource note:** The host has 8 GB RAM and a dual-core processor. The lab is intentionally kept lightweight; a local Wazuh all-in-one deployment has been deferred.

## Completed practical exercises

### 1. Windows Security Event Log monitoring

- Configured Splunk Local Event Logs input on the Windows host.
- Collected Security, System, and Application event channels.
- Verified that Windows Security events were searchable in Splunk.
- Practised searches for:
  - Event ID `4624` — successful logon
  - Event ID `4625` — failed logon

Detailed documentation: [Windows Security Log Investigation](Reports/windows-security-log-investigation.md).

### 2. Ubuntu authentication log forwarding

- Installed Splunk Universal Forwarder on Ubuntu Server.
- Configured monitoring of `/var/log/auth.log`.
- Verified that Linux authentication events were forwarded to Splunk on the Windows host.
- Reviewed SSH authentication events in the SIEM.

### 3. SSH failed-login detection and alert validation

- Generated controlled failed SSH login attempts in the authorised home lab.
- Created an SPL search to extract username and source IP and count repeated failures.
- Configured a threshold of three or more failed attempts within a five-minute search window.
- Created a scheduled alert that runs every five minutes.
- Verified that the alert fired and its Log Event action wrote an event to the Splunk `main` index.

The test events were deliberately generated for this lab and should not be represented as a real external attack. The Splunk trial environment displayed a warning that scheduled searches would stop after the trial licence expires.

Detailed documentation: [Ubuntu SSH Failed-Login Detection](Reports/ubuntu-ssh-failed-login-detection.md).

## Repository structure

```text
SOC-Home-Lab/
├── README.md
├── Setup/
│   └── virtualbox-setup.md
├── Reports/
│   ├── windows-security-log-investigation.md
│   └── ubuntu-ssh-failed-login-detection.md
└── Screenshots/
```

## Tools and technologies

| Category | Tools / technologies |
|---|---|
| SIEM | Splunk Enterprise |
| Log collection | Splunk Universal Forwarder |
| Operating systems | Windows 10, Ubuntu Server |
| Virtualisation | Oracle VirtualBox |
| Query language | Splunk Processing Language (SPL) |
| Telemetry | Windows Event Logs, Linux `auth.log` |

Tools such as Wazuh, Wireshark, Suricata, YARA, and threat-intelligence platforms are potential future learning areas; they are not claimed as completed hands-on work in this project.

## Learning roadmap

- [x] Build the VirtualBox lab environment.
- [x] Install and configure Ubuntu Server.
- [x] Configure SSH access between the Windows host and Ubuntu VM.
- [x] Install Splunk Enterprise on Windows.
- [x] Configure Windows Event Log collection.
- [x] Search and review Windows authentication events.
- [x] Install Splunk Universal Forwarder on Ubuntu.
- [x] Forward and search Linux authentication logs.
- [x] Create an SSH failed-login threshold detection.
- [x] Configure and validate a scheduled Splunk alert.
- [x] Complete the LetsDefend SOC Analyst Learning Path and earn its completion certificate.

## Safety and scope

All testing documented here was performed in a personal lab or on systems under the author's control. No third-party systems were targeted. Sensitive credentials, tokens, and private keys must never be committed to this repository. Screenshots and logs should be reviewed and redacted before publication.

---

*This repository is a learning portfolio demonstrating foundational SOC Analyst L1 practice through documented home-lab exercises and completed training.*
