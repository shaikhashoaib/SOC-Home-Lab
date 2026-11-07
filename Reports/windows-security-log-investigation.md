# Windows Security Log Collection and First Investigation

**Project:** SOC Home Lab  
**Platform:** Splunk Enterprise 10.6.1.1 on Windows  
**Data source:** Windows Local Event Logs  
**Status:** Initial collection and event review completed

## 1. Objective

Configure Splunk to collect local Windows Event Logs and practise searching and triaging Security events, focusing on successful logons (Event ID 4624) and failed logons (Event ID 4625).

## 2. Lab configuration

- Splunk Enterprise was installed on the Windows host and its web dashboard login was verified.
- Splunk TCP receiving port 9997 was enabled for the later Ubuntu-forwarder stage.
- A Splunk **Local Event Logs** input was created.
- Available Windows Event Log channels were selected, including Security, System, and Application.
- The input destination index was configured as `default`.
- Events were observed with source and sourcetype `WinEventLog:Security`.
- The actual event host shown in Splunk was `DESKTOP-60CUGND` (as displayed in the captured event results). The custom host value entered during the input wizard did not appear as the event host in the observed records.

## 3. Verification searches

### Search all Security events

```spl
index=* source="WinEventLog:Security"
```

A captured search returned **9,298 events** over the selected time range. This is a point-in-time result from the lab, not a permanent event count.

### Successful logons — Event ID 4624

```spl
index=* source="WinEventLog:Security" EventCode=4624
```

Captured result: **408 events**.

### Failed logons — Event ID 4625

```spl
index=* source="WinEventLog:Security" EventCode=4625
```

The first captured search returned one event. A later search returned **four events**, including three records with timestamps around 22:12 on 2026-11-05 after a controlled sign-in test was attempted.

## 4. Failed-logon event review

The detailed event expanded in Splunk showed these fields:

| Field | Observed value |
|---|---|
| EventCode | `4625` |
| Account_Name | `DESKTOP-60CUGND$` |
| Account_Domain | `WORKGROUP` |
| Logon_Type | `2` |
| Source_Network_Address | `127.0.0.1` |
| Source_Port | `0` |
| Caller_Process_Name | `C:\Windows\System32\svchost.exe` |
| Caller_Process_ID | `0x678` |
| Logon_Process | `User32` |
| Authentication_Package | `Negotiate` |
| Status | `0xC000000D` |
| Sub_Status | `0xC0000380` |
| Failure_Reason | `An Error occurred during Logon.` |

The event text describes a local logon failure and identifies the subject as a local system context. The account name ends in `$`, which is consistent with a machine-account-style name. The source address is loopback (`127.0.0.1`), and the caller process shown is `svchost.exe`.

### Initial assessment

This single event does **not** establish an external attack. Its local source address, machine-style account name, logon type 2, and Windows service-host process suggest a local system/service-related authentication failure may be involved. The exact root cause has not been established.

Three additional 4625 records appeared at approximately 22:12 on 2026-11-05 after a controlled incorrect-password attempt was made from the Windows sign-in screen. However, the visible summary rows continued to show the machine-style account and loopback address. The available evidence does not conclusively prove that those records represent the intended interactive user's failed password attempt. Further event correlation is required.

## 5. Useful triage SPL

### Tabulate failed-logon context

```spl
index=* source="WinEventLog:Security" EventCode=4625
| sort -_time
| table _time Account_Name Account_Domain Failure_Reason Logon_Type Source_Network_Address
```

### Review detailed event fields

```spl
index=* source="WinEventLog:Security" EventCode=4625
```

Expand an event and review fields such as `TargetUserName`, `Status`, `Sub_Status`, `ProcessName`, `LogonType`, and source network information where available.

## 6. Conclusion

- Splunk successfully ingested local Windows Security events.
- Event IDs 4624 and 4625 were searchable.
- A first failed-logon event was expanded and reviewed.
- The event was not labelled malicious because the evidence was insufficient.
- The Windows event review remains an initial triage exercise. The separately documented Ubuntu SSH exercise demonstrates the completed threshold-based failed-login detection and scheduled alert workflow.

## 7. Limitations and next steps

- Event counts are snapshots and will change as new events arrive.
- The current Windows event collection is local to the Splunk host.
- Ubuntu authentication-log forwarding was completed and is documented separately in [Ubuntu SSH Failed-Login Detection](ubuntu-ssh-failed-login-detection.md).
- The repository has a `Screenshots/` folder containing two image files. Their exact contents and redaction status should be checked before relying on them as evidence for this Windows-specific report.
- Do not publish raw event exports or screenshots containing usernames, machine identifiers, IPs, or other personal information without reviewing and redacting them.

The Windows investigation is complete as an initial triage exercise. Its inconclusive 4625 attribution is intentionally preserved rather than presented as a confirmed user-password failure. The related Linux log-forwarding and SSH threshold-detection exercise is documented in the companion report.
