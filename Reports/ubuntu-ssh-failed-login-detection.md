# Ubuntu SSH Failed-Login Detection

**Project:** SOC Home Lab  
**Status:** Detection search validated; scheduled alert created; alert execution pending  
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

The following search extracts the username and source IP, groups matching events into five-minute buckets, and returns groups with at least three failed attempts:

```spl
index=main source="/var/log/auth.log" "Failed password"
| rex "Failed password for (invalid user )?(?<username>\S+) from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| bin _time span=5m
| stats count as failed_attempts by _time src_ip username host
| where failed_attempts >= 3
| sort - failed_attempts
```

The captured result showed:

| Time bucket | Source IP | Username | Failed attempts |
|---|---|---|---:|
| 2026-11-07 14:10 | `10.0.2.2` | `wronguser` | 3 |

The username and source IP extraction were visually verified in Splunk's Statistics view.

## 6. Scheduled Splunk alert configuration

A scheduled alert was created in Splunk Enterprise with the following settings, as shown in the Splunk UI:

| Setting | Configured value |
|---|---|
| Alert title | SSH - Multiple Failed Login Attempts |
| Description | Detects 3 or more failed SSH login attempts from the same source IP within 5 minutes. |
| Permissions | Private |
| Alert type | Scheduled |
| Schedule | Hourly, at 0 minutes past the hour |
| Trigger condition | Number of results greater than 0 |
| Trigger mode | Once |
| Trigger action | Log Event |
| Status | Enabled |
| Next scheduled run shown | Nov 7, 2026, 3:00 PM |

The Log Event action was configured to write an event to the `main` index, using source `SSH_Failed_Login_Alert`, sourcetype `ssh_failed_login_alert`, and host `soc-ubuntu`.

### Alert validation status

At the latest check on Nov 7, 2026, the alert details page showed **no fired events**. The alert is saved and enabled, but its scheduled execution and Log Event output have **not yet been verified**. The next scheduled run displayed by Splunk was 3:00 PM on Nov 7, 2026.

**Schedule/search-window caveat:** The SPL groups events into five-minute buckets, but the saved scheduled alert runs hourly. This can delay detection and may not behave as intended for a rolling five-minute detection. After confirming the current scheduled run, review the search time range and schedule together and align them before calling the alert operationally reliable. The Splunk UI also warned that the scheduled search will not run after the Splunk Enterprise Trial License expires.

## 7. Initial SOC assessment

The controlled test produced three failed SSH authentication events for the same invalid username and source IP. Repeated SSH failures can warrant triage in a real environment, but a threshold alone does not prove malicious activity.

For this lab, the activity was analyst-generated and should be classified as an expected test / informational result. In a real investigation, validate the source, time window, target account, surrounding authentication activity, asset owner, and whether the attempts were authorised before escalating.

## 8. Remaining validation and improvement

- Check the alert after its next scheduled execution.
- Confirm whether a fired event appears in the alert history.
- Verify that the Log Event action writes the expected record to `main`.
- Align the scheduled search interval and five-minute detection window.
- Add reviewed and redacted screenshots to the repository.
- Complete the final project summary and interview walkthrough.

## 9. Evidence and limitations

This report records the lab configuration and observations shown in Splunk screenshots on 2026-11-07. Event counts and timestamps are snapshots and will change as new events arrive. The source IP is recorded exactly as Splunk displayed it; its broader network role has not been independently established in this report.

No passwords, tokens, or private keys are included.
