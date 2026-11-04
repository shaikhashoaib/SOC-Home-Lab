# SOC Home Lab

> **Status:** In progress  
> **Focus:** Practical SOC Analyst L1 skills — endpoint monitoring, SIEM alert triage, investigation, and incident documentation.

A hands-on Security Operations Center (SOC) home lab being built to practise security monitoring and investigation in an isolated, controlled environment. The project documents the lab design, configuration, test cases, evidence, findings, and lessons learned as each milestone is completed.

## Project objectives

- Build a small virtual lab using Oracle VirtualBox.
- Deploy and configure a SIEM suitable for the available hardware.
- Collect and analyse Windows endpoint telemetry.
- Generate controlled security events and investigate the resulting alerts.
- Practise basic network monitoring and threat-intelligence enrichment where resources permit.
- Produce clear, reproducible investigation notes and incident reports.

## Current environment

### Host system

| Component | Specification |
|---|---|
| Computer | HP ProDesk 400 G1 SFF |
| Host OS | Windows 10 Pro 64-bit |
| CPU | Intel Core i3-4160 @ 3.60 GHz |
| CPU topology | 2 physical cores / 4 logical processors |
| RAM | 8 GB |
| Hypervisor | Oracle VirtualBox |

### Virtual machine: SOC-Ubuntu

| Setting | Configuration |
|---|---|
| Operating system media | Ubuntu Server 24.04.5 LTS, AMD64 |
| VM memory | 2 GB |
| Virtual CPU | 1 |
| Virtual disk | 40.48 GB |
| Network mode | NAT |
| Installation | Manual installation |

**Current milestone:** Ubuntu Server installation completed. First-boot login and network verification are pending.

> Hardware note: The host has limited memory and CPU capacity. SIEM deployment choices will be validated against actual resource usage. The project will not claim a full production-scale SOC deployment.

## Planned technology

Tools below are planned or under evaluation; they should not be interpreted as already installed or operational.

| Area | Planned technology | Status |
|---|---|---|
| Virtualisation | Oracle VirtualBox | VM created |
| SOC server | Ubuntu Server | Installed; first login pending |
| SIEM | Wazuh | Planned |
| Windows telemetry | Sysmon, Windows Event Logs | Planned |
| Test workstation | Kali Linux | Available as a separate lab laptop |
| Network analysis | Wireshark | Planned |
| Network detection | Suricata | Optional; hardware permitting |
| Threat intelligence | VirusTotal, AbuseIPDB | Planned |
| File analysis | YARA | Planned |
| Threat intelligence platform | MISP | Optional; hardware permitting |

## Repository structure

```text
SOC-Home-Lab/
├── README.md
├── Setup/
│   └── virtualbox-setup.md
├── Architecture/              # Planned
├── Windows-Monitoring/        # Planned
├── Detection-Labs/            # Planned
├── Threat-Intelligence/       # Planned
├── Screenshots/               # Planned
└── Reports/                   # Planned
```

Folders will be added as the corresponding lab work is completed and documented.

## Project workflow

1. **Build the lab:** verify the Ubuntu VM, network connectivity, and endpoint arrangement.
2. **Deploy monitoring:** select a resource-appropriate Wazuh deployment and connect a Windows endpoint if feasible.
3. **Collect telemetry:** configure endpoint logging and confirm events arrive at the monitoring platform.
4. **Run controlled tests:** generate safe, authorised test events in the lab.
5. **Investigate:** record alert context, event evidence, analysis, and conclusions.
6. **Document:** add reproducible procedures, redacted screenshots, and incident reports.

## Evidence and documentation standards

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

## Safety and responsible use

- Testing is restricted to systems and networks owned by the author or explicitly authorised for testing.
- No public targets or third-party systems are to be scanned or attacked.
- Never commit passwords, access tokens, API keys, private keys, or other secrets.
- Redact public IP addresses, personal information, host identifiers, and sensitive log data before publishing evidence.
- Do not upload VM disk images, installer ISOs, or large raw log exports.

## Progress

- [x] Create the GitHub repository.
- [x] Create the SOC-Ubuntu VirtualBox VM.
- [x] Install Ubuntu Server 24.04.5 LTS.
- [ ] Verify first boot and console login.
- [ ] Verify network connectivity and record the lab IP.
- [ ] Finalise the resource-appropriate SIEM deployment plan.
- [ ] Deploy SIEM and connect endpoint telemetry.
- [ ] Complete and document detection investigations.
- [ ] Publish final architecture and project report.

## Author

**Shaikh A Shoaib**

This repository is a learning portfolio documenting practical cybersecurity lab work. It is not a production SOC implementation.
