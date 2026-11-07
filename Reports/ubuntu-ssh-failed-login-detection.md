# Ubuntu SSH Failed-Login Detection

**Project:** SOC Home Lab  
**Status:** Detection search validated; five-minute scheduled alert fired; Log Event output verified  
**Last documented:** 2026-11-07

## 1. Objective

Forward Ubuntu authentication logs to Splunk Enterprise on Windows, generate a controlled failed SSH login, extract relevant fields, build a threshold-based detection search, and configure a scheduled alert.

## 2. Lab architecture and data flow

- **Endpoint:** Ubuntu Server 24.04.5 LTS in VirtualBox (hostname: `soc-ubuntu`).
- **Log source:** `/var/log/auth.log`.
- **Forwarder:** Splunk Universal Forwarder 10.4.3 on Ubuntu.
- **SIEM:** Splunk Enterprise 10.6.1.1 on the Windows host.
- **Receiver:** Windows Host-only address `192.168.56.1:9997`.
- **Index:** `main`.
- **Observed event metadata:** host `soc-ubuntu`, source `/var/log/auth.log`, sourcetype `auth`.

The Windows receiver was confirmed listening on TCP port 9997, and Ubuntu-to-Windows connectivity to `192.168.56.1:9997` was tested successfully. The Universal Forwarder later showed `192.168.56.1:9997` under **Active forwards**.

The `splunkfwd` service account was added to the `adm` group to grant access to the authentication log, and `/var/log/auth.log` was successfully added as a monitored file.

## 3. Confirm Linux log ingestion

Search:

```spl
index=main source="/var/log/auth.log"
```

A captured search returned **202 events** at the time of verification. This is a point-in-time count, not a fixed total. Event previews showed SSH session activity and local `socadmin` activity from Ubuntu.

## 4. Controlled SSH failed-login test

A controlled test was performed against the user's own lab by attempting an SSH login from Windows PowerShell using the deliberately invalid username `wronguser`. Splunk displayed three failed-password events.

Search:

```spl
index=main source="/var/log/auth.log" "Failed password"
```

Observed evidence:

| Field | Observed value |
|---|---|
| Event count | 3 |
| Host | `soc-ubuntu` |
| Username | `wronguser` |
| Source IP | `10.0.2.2` |
| Service | SSH (`sshd`) |
| Message | `Failed password for invalid user` |

These events were intentionally generated for the lab. They are not evidence of an actual external attack.

## 5. SPL field extraction and threshold detection

The final detection search counts matching failures by source IP, username, and host. The scheduled alert is configured with a **Last 5 minutes** search time range and runs every five minutes:

```spl
index=main source="/var/log/auth.log" "Failed password"
| rex "Failed password for (invalid user )?(?<username>\\S+) from (?<src_ip>\\d+\\.\\d+\\.\\d+\\.\\d+)"
| stats count as failed_attempts by src_ip username host
| where failed_attempts >= 3
```

The captured Statistics result showed source IP `10.0.2.2`, username `wronguser`, host `soc-ubantu` (as displayed in that search result), and **3** failed attempts. The alert action's separately indexed event used `soc-ubuntu` as its host metadata.

## 6. Scheduled Splunk alert configuration

A scheduled alert was created in Splunk Enterprise with the following settings, as shown in the Splunk UI:

| Setting | Configured value |
|---|---|
| Alert title | SSH - Multiple Failed Login Attempts |
| Description | Detects 3 or more failed SSH login attempts from the same source IP within 5 minutes. |
| Permissions | Private |
| Alert type | Scheduled |
| Schedule | Cron: `*/5 * * * *` (every 5 minutes) |
| Trigger condition | Number of results greater than 0 |
| Trigger mode | Once |
| Trigger action | Log Event |
| Status | Enabled |
| Search time range | Last 5 minutes |

The Log Event action was configured to write an event to the `main` index, using source `SSH_Failed_Login_Alert`, sourcetype `ssh_failed_login_alert`, and host `soc-ubuntu`. This is the host value set on the alert action; the raw authentication search displayed `soc-ubantu` in its result.

### Alert validation result

The alert fired successfully on **Nov 7, 2026 at 3:00:02 PM IST**. The Log Event action wrote the following event to the `main` index:

```text
SSH failed login threshold reached: wronguser from 10.0.2.2 with 3 failures.
```

The returned event showed:

| Field | Verified value |
|---|---|
| Host | `soc-ubuntu` |
| Source | `SSH_Failed_Login_Alert` |
| Sourcetype | `ssh_failed_login_alert` |
| Index | `main` |
| Failed attempts | `3` |
| Username | `wronguser` |
| Source IP | `10.0.2.2` |

This confirms the scheduled alert executed and the Log Event action successfully indexed the alert record. The Splunk UI warned that the scheduled search will stop running after the Splunk Enterprise Trial License expires. This is an environment/licensing limitation, not a failed detection test.

## 7. Initial SOC assessment

The controlled test produced three failed SSH authentication events for the same invalid username and source IP. Repeated SSH failures can warrant triage in a real environment, but a threshold alone does not prove malicious activity.

For this lab, the activity was analyst-generated and should be classified as an expected test / informational result. In a real investigation, validate the source, time window, target account, surrounding authentication activity, asset owner, and whether the attempts were authorised before escalating.

## 8. Completion status

- [x] Verify the scheduled alert fires on its five-minute schedule.
- [x] Confirm the Log Event action writes an event to the `main` index.
- [x] Document the final SPL, alert configuration, and observed output.
- [ ] Optional improvement: add descriptive captions to screenshots after checking their contents.

## 9. Evidence and limitations

This report records the lab configuration and observations reviewed in Splunk on 2026-11-07. Event counts and timestamps are snapshots and will change as new events arrive. The source IP is recorded exactly as Splunk displayed it; its broader network role has not been independently established in this report.

No passwords, tokens, or private keys are included.
