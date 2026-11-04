<!--
  SOC Home Lab | Portfolio README
  Visual theme: midnight blue + cyan
-->

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:081A2F,50:123B5D,100:00B8D9&height=190&section=header&text=SOC%20HOME%20LAB&fontSize=42&fontColor=FFFFFF&fontAlignY=38&desc=Security%20Monitoring%20%7C%20SIEM%20%7C%20Incident%20Investigation&descAlignY=60&descSize=14" width="100%" alt="SOC Home Lab banner" />

# 🛡️ SOC Home Lab

**A hands-on cybersecurity portfolio focused on SOC Analyst Level 1 skills**

<p>
  <img src="https://img.shields.io/badge/Project-In%20Progress-0EA5E9?style=for-the-badge" alt="Project in progress" />
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

A practical Security Operations Center (SOC) home lab being built to practise security monitoring and investigation in an isolated, controlled environment. This repository records the lab design, configuration, test cases, evidence, findings, and lessons learned as each milestone is completed.

### Objectives

- Build a small virtual lab using Oracle VirtualBox.
- Deploy and configure a SIEM suitable for the available hardware.
- Collect and analyse Windows endpoint telemetry.
- Generate controlled security events and investigate the resulting alerts.
- Practise basic network monitoring and threat-intelligence enrichment where resources permit.
- Produce clear, reproducible investigation notes and incident reports.

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

### Virtual machine — SOC-Ubuntu

| Setting | Configuration |
|---|---|
| Operating system media | Ubuntu Server 24.04.5 LTS, AMD64 |
| VM memory | 2 GB |
| Virtual CPU | 1 |
| Virtual disk | 40.48 GB |
| Network mode | NAT |
| Installation | Manual installation |

**Current milestone:** Ubuntu Server 24.04.5 LTS is installed, updated, and accessible from Windows through SSH over VirtualBox NAT port forwarding. Outbound IP connectivity and DNS resolution have been tested.

> ⚙️ **Hardware note:** The host has limited memory and CPU capacity. SIEM deployment choices will be validated against actual resource usage. This project will not claim a full production-scale SOC deployment.

## 🧰 Technology stack

The table distinguishes the selected local SIEM from tools that remain planned or deferred. Statuses reflect only work verified so far.

| Area | Technology | Status |
|---|---|---|
| Virtualisation | ![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=flat&logo=virtualbox&logoColor=white) | VM created |
| Linux server | ![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white) | Installed, updated, and SSH-accessible |
| Primary SIEM | ![Splunk](https://img.shields.io/badge/Splunk-000000?style=flat&logo=splunk&logoColor=white) | Existing Windows installation; dashboard and Windows Event ID 4625 search previously verified |\n| Future SIEM option | ![Wazuh](https://img.shields.io/badge/Wazuh-3B82F6?style=flat&logo=wazuh&logoColor=white) | Deferred until resources or remote deployment are available |
| Windows telemetry | ![Windows](https://img.shields.io/badge/Windows-0078D4?style=flat&logo=windows&logoColor=white) Sysmon | Planned |
| Test workstation | ![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat&logo=kalilinux&logoColor=white) | Available as a separate lab laptop |
| Network analysis | ![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat&logo=wireshark&logoColor=white) | Planned |
| Network detection | Suricata | Optional; hardware permitting |
| Threat intelligence | VirusTotal, AbuseIPDB | Planned |
| File analysis | YARA | Planned |
| Threat intelligence platform | MISP | Optional; hardware permitting |

## 🗺️ Project roadmap

- [x] Create the GitHub repository.
- [x] Create the SOC-Ubuntu VirtualBox VM.
- [x] Install Ubuntu Server 24.04.5 LTS.
- [x] Verify first boot and console login.
- [x] Verify network connectivity (outbound IP connectivity and DNS resolution).
- [x] Configure SSH remote access from Windows through VirtualBox NAT port forwarding.
- [x] Reboot Ubuntu and verify SSH reconnection from Windows.
- [x] Apply available Ubuntu package updates.
- [x] Select Splunk on the Windows host as the primary local SIEM; defer local Wazuh deployment due to hardware limits.
- [ ] Deploy SIEM and connect endpoint telemetry.
- [ ] Complete and document detection investigations.
- [ ] Publish final architecture and project report.

## 🔬 Project workflow

1. **Build the lab:** verify the Ubuntu VM, network connectivity, and endpoint arrangement.
2. **Deploy monitoring:** select a resource-appropriate Wazuh deployment and connect a Windows endpoint if feasible.
3. **Collect telemetry:** configure endpoint logging and confirm events arrive at the monitoring platform.
4. **Run controlled tests:** generate safe, authorised test events in the lab.
5. **Investigate:** record alert context, event evidence, analysis, and conclusions.
6. **Document:** add reproducible procedures, redacted screenshots, and incident reports.

## 📁 Repository structure

```text
SOC-Home-Lab/
├── README.md
├── Setup/
│   └── virtualbox-setup.md
├── Architecture/              # Planned
├── Windows-Monitoring/         # Planned
├── Detection-Labs/             # Planned
├── Threat-Intelligence/        # Planned
├── Screenshots/                # Planned
└── Reports/                    # Planned
```

Folders will be added as the corresponding lab work is completed and documented.

## 📝 Evidence and documentation

Each completed lab exercise should include:

- Objective and scope
- Lab topology and prerequisites
- Tools and versions actually used
- Configuration steps and commands
- Test procedure and expected result
- Observed evidence (redacted screenshots or sample logs)
- Analysis, conclusion, and limitations
- Cleanup or rollback steps where relevant

Only verified work will be marked as completed. Planned work will remain clearly labelled as planned or in progress.

## 🔐 Safety

- Testing is restricted to systems and networks owned by the author or explicitly authorised for testing.
- No public targets or third-party systems are to be scanned or attacked.
- Never commit passwords, access tokens, API keys, private keys, or other secrets.
- Redact public IP addresses, personal information, host identifiers, and sensitive log data before publishing evidence.
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
