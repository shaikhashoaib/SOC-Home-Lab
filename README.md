<!--
  SOC Home Lab | Portfolio README
  Visual theme: midnight blue + cyan
-->

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:081A2F,50:123B5D,100:00B8D9&height=190&section=header&text=SOC%20HOME%20LAB&fontSize=42&fontColor=FFFFFF&fontAlignY=38&desc=Security%20Monitoring%20%7C%20SIEM%20%7C%20Incident%20Investigation&descAlignY=60&descSize=14" width="100%" alt="SOC Home Lab banner" />

# 🛡️ SOC Home Lab

**A hands-on cybersecurity portfolio focused on SOC Analyst Level 1 skills**

<p>
  <img src="https://img.shields.io/badge/Project-Completed-16A34A?style=for-the-badge" alt="Project completed" />
  <img src="https://img.shields.io/badge/Focus-SOC%20Analyst%20L1-2563EB?style=for-the-badge" alt="SOC Analyst L1" />
  <img src="https://img.shields.io/badge/Environment-Home%20Lab-0F766E?style=for-the-badge" alt="Home lab" />
</p>

<p>
  <a href="#-project-overview">Overview</a> •
  <a href="#-lab-environment">Environment</a> •
  <a href="#-technology-stack">Technology</a> •
  <a href="#-project-roadmap">Roadmap</a> •
  <a href="#-safety">Safety</a>
</p>

</div>

---

## 🎯 Project overview

A practical Security Operations Center (SOC) home lab built around Splunk Enterprise on Windows and Ubuntu Server telemetry. It documents Windows and Linux log collection, initial event triage, a controlled SSH failed-login test, a threshold-based SPL detection, and a scheduled Splunk alert whose Log Event output was verified.

<badge label="Core lab project completed" color="success" />

### Objectives

- Build a small virtual lab using Oracle VirtualBox.
- Deploy a resource-appropriate SIEM.
- Collect and analyse Windows endpoint telemetry.
- Generate controlled security events and investigate them.
- Practise detection engineering, triage, and incident documentation.
- Forward Linux authentication telemetry and investigate controlled failed SSH logins.\n- Document threat-intelligence enrichment as a possible future extension.

## 🖥️ Lab environment

### Host system

| Component | Specification |
|---|---|
| Computer | HP ProDesk 400 G1 SFF |
| Host OS | Windows 10 Pro 64-bit |
| CPU | Intel Core i3-4160 @ 3.60 GHz |
| CPU topology | 2 physical cores / 4 logical processors |
| RAM | 8 GB |
| Hypervisor | Oracle VirtualBox |

### Ubuntu virtual machine — SOC-Ubuntu

| Setting | Configuration |
|---|---|
| Operating system | Ubuntu Server 24.04.5 LTS, AMD64 |
| VM memory | 2 GB |
| Virtual CPU | 1 |
| Virtual disk | 40.48 GB |
| Adapter 1 | NAT — internet access |
| Adapter 2 | Host-only — host/guest lab communication |
| NAT SSH forwarding | Windows localhost TCP 2222 → Ubuntu TCP 22 |

Ubuntu is installed, updated, and accessible from Windows through SSH. The Host-only network was configured for direct communication between Windows and Ubuntu while NAT remains available for outbound connectivity.

### SIEM host

Splunk Enterprise is installed on the Windows host. The web dashboard is accessible locally, the TCP receiving port **9997** is enabled, and Windows Event Log input has been created.

| Endpoint | Lab address |
|---|---|
| Windows Host-only adapter | `192.168.56.1` |
| Ubuntu Host-only interface | `192.168.56.10` |
| Splunk receiving port | TCP `9997` |

> **Hardware note:** The host has 8 GB RAM and a 2-core CPU. Local Wazuh all-in-one deployment is deferred. The lab prioritises Splunk on Windows and one lightweight Ubuntu VM.

## 🧰 Technology stack

| Area | Technology | Status |
|---|---|---|
| Virtualisation | Oracle VirtualBox | Configured |
| Linux server | Ubuntu Server 24.04.5 LTS | Installed, updated, SSH-accessible |
| Primary SIEM | Splunk Enterprise | Installed and operational on Windows |
| Windows telemetry | Windows Event Logs | Security and other local event channels configured |
| Event investigation | Splunk Search Processing Language (SPL) | Initial 4624/4625 searches completed |
| Linux telemetry | Splunk Universal Forwarder 10.4.3 | Installed; `/var/log/auth.log` forwarding verified |
| Future SIEM option | Wazuh | Deferred due to host resource limits |
| Network analysis | Wireshark | Planned |
| Network detection | Suricata | Optional; hardware permitting |
| Threat intelligence | VirusTotal, AbuseIPDB | Planned |
| File analysis | YARA | Planned |
| Threat intelligence platform | MISP | Optional; hardware permitting |

## 🔎 Completed monitoring exercise

### Windows Security Event Log collection

- Created a Splunk **Local Event Logs** input on the Windows host.
- Selected the available Windows Event Log channels, including **Security**, **System**, and **Application**.
- Confirmed events are indexed with source `WinEventLog:Security` and sourcetype `WinEventLog:Security`.
- Observed **9,298 Security events** in one search result at the time of verification (a point-in-time count, not a fixed total).
- Queried successful logons (Event ID `4624`): **408 events** returned in the captured search.
- Queried failed logons (Event ID `4625`): **4 events** returned in the later investigation search.

Detailed notes and SPL queries: [Windows Security Log Investigation](Reports/windows-security-log-investigation.md).

## 🚨 Scheduled detection alert

A scheduled alert named **SSH - Multiple Failed Login Attempts** has been created in Splunk Enterprise.

| Configuration | Verified value |
|---|---|
| Status | Enabled (saved in Splunk) |
| Alert type | Scheduled |
| Schedule | Every 5 minutes (`*/5 * * * *`) |
| Trigger condition | Number of results greater than 0 |
| Trigger action | Log Event |
| Sharing | Private |
| Confirmed fired event | Nov 7, 2026, 3:00:02 PM IST |
| Log Event output | Confirmed in `main` index |

**Validation result:** The detection search returned three failed SSH attempts for `wronguser` from `10.0.2.2` on `soc-ubuntu`. The scheduled alert was changed to run every five minutes with a five-minute search time range. A fired Log Event was then confirmed in the `main` index at **Nov 7, 2026, 3:00:02 PM IST**, with the message: `SSH failed login threshold reached: wronguser from 10.0.2.2 with 3 failures.` This validates the lab alert path. The Splunk UI had warned that scheduled searches stop after the Enterprise Trial License expires.

## 🗺️ Project roadmap

- [x] Create the GitHub repository and project README.
- [x] Create the SOC-Ubuntu VirtualBox VM.
- [x] Install and update Ubuntu Server 24.04.5 LTS.
- [x] Configure SSH access from Windows through NAT port forwarding.
- [x] Add a Host-only network for Windows/Ubuntu communication.
- [x] Install Splunk Enterprise on Windows and verify the dashboard.
- [x] Enable Splunk TCP receiving on port 9997.
- [x] Create a Windows Local Event Logs input in Splunk.
- [x] Verify Windows Security events are searchable.
- [x] Run initial SPL searches for Event IDs 4624 and 4625.
- [x] Document the first failed-logon event review.
- [x] Install Splunk Universal Forwarder on Ubuntu and forward Linux authentication logs.
- [x] Verify Ubuntu `auth.log` events arrive in Windows Splunk.
- [x] Run a controlled failed SSH login test and build a threshold-based SPL search.
- [x] Create and enable a scheduled Splunk alert for the SSH failed-login threshold.
- [x] Verify the alert fires on its five-minute schedule; confirmed event at 3:00:02 PM IST on Nov 7, 2026.
- [x] Review alert execution and confirm the Log Event action writes to the `main` index.
- [x] Complete final evidence review and portfolio documentation.



## 📁 Repository structure

```text
SOC-Home-Lab/
├── README.md
├── Setup/
│   └── virtualbox-setup.md
└── Reports/
    └── windows-security-log-investigation.md
```

The repository contains the VirtualBox setup guide, Windows and Ubuntu investigation reports, and two image files under `Screenshots/`.

## 📝 Evidence and documentation standard

Each lab exercise should record:

- Objective and authorised scope
- Lab topology and prerequisites
- Tools and versions actually used
- Configuration steps and SPL queries
- Test procedure and observed results
- Analysis, conclusion, and limitations
- Redacted screenshots or sample logs where available

Only verified work is marked completed. Two screenshot image files are present in `Screenshots/`; review their contents, filenames, captions, and redaction before treating them as formal evidence.

## 🔐 Safety

- Testing is restricted to systems and networks owned by the author or explicitly authorised for testing.
- No public targets or third-party systems are to be scanned or attacked.
- Never commit passwords, access tokens, API keys, private keys, or other secrets.
- Redact personal information, public IP addresses, host identifiers, and sensitive log data before publishing evidence.
- Do not upload VM disk images, installer ISOs, or large raw log exports.

## 👨‍💻 Author

**Shaikh A Shoaib**

This repository is a learning portfolio documenting practical cybersecurity lab work. It is not a production SOC implementation.

<div align="center">

![GitHub last commit](https://img.shields.io/github/last-commit/shaikhashoaib/SOC-Home-Lab?style=flat-square&color=0EA5E9)
![GitHub repo size](https://img.shields.io/github/repo-size/shaikhashoaib/SOC-Home-Lab?style=flat-square&color=0F766E)

<sub>Built with curiosity, documented with evidence. 🚀</sub>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00B8D9,50:123B5D,100:081A2F&height=90&section=footer" width="100%" alt="" />

</div>
