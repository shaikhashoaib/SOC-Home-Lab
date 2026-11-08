# LetsDefend SOC Analyst Practice — Alert #001

**Platform:** LetsDefend  
**Exercise:** Multiple Failed SSH Login Attempts  
**Practice level:** SOC Analyst L1  
**Status:** Initial triage completed; follow-up authentication review pending  
**Recorded:** 2026-11-08

## 1. Alert summary

| Field | Value |
|---|---|
| Alert | Multiple Failed SSH Login Attempts |
| Severity | Medium |
| Host | `soc-ubuntu` |
| Username | `wronguser` |
| Source IP | `10.0.2.2` |
| Failed attempts | 3 |
| Time window | 5 minutes |

## 2. Initial analyst assessment

The first proposed verdict was **False Positive**, based on the assumption that the user was trying to log in to their own system. That conclusion was premature because the initial alert alone did not establish user identity, source ownership, or authorisation.

During follow-up discussion, the exercise context established that the three failed attempts had been deliberately generated from the analyst's own Windows machine as part of an authorised security test.

**Final contextual classification: Benign Positive / expected authorised test activity.**

### Why this is not a False Positive

- The failed SSH attempts genuinely occurred.
- The detection correctly identified the repeated failed logins.
- The activity was intentional and authorised in this lab scenario.
- A False Positive would mean the detection incorrectly identified activity that did not meet its detection criteria.

This classification depends on the confirmed exercise context. In a real SOC case, the analyst must independently validate asset ownership, user authorisation, source IP, timestamps, and change/test records before assigning a benign verdict.

## 3. L1 investigation workflow

For a real alert, collect and correlate:

1. Source IP ownership and device identity.
2. Target host and account validity.
3. Authentication events around the alert time.
4. Whether a successful login followed the failed attempts.
5. User confirmation, approved testing, or change-ticket evidence.
6. Related alerts or suspicious activity from the same source.

Do not conclude that a source is trusted only because it appears to be an internal or familiar address.

## 4. Follow-up Splunk query

The related home-lab telemetry can be reviewed with:

```spl
index=main source="/var/log/auth.log"
("Failed password" OR "Accepted password")
| table _time host message
| sort -_time
```

Review whether an `Accepted password` event appears after the failures. Correlate the timestamp, username, source IP, and host. This query is a starting point; confirm the exact event format and fields in the available logs.

**Follow-up status:** Pending. No successful-login event has been documented for this exercise yet.

## 5. Analyst learning points

- A detection alert is a lead for investigation, not proof of an attack.
- **True Positive:** the detection correctly identifies activity that is genuinely malicious or policy-violating.
- **False Positive:** the detection fires on activity that does not actually meet the suspicious condition it is intended to detect.
- **Benign Positive:** the detected activity occurred, but it is legitimate or authorised.
- **Inconclusive:** evidence is insufficient; continue investigating or escalate.

## 6. Scope and evidence note

This is a training exercise based on the alert details and analyst-provided context recorded during practice. It is not evidence of employment in a production SOC, nor a claim that a real external attack occurred. The associated Ubuntu SSH detection in this repository documents the separately generated home-lab events and Splunk alert validation.

## 7. Completion checklist

- [x] Review alert fields and identify the detection condition.
- [x] Recognise that the initial False Positive verdict was unsupported.
- [x] Reclassify the known authorised test as Benign Positive.
- [ ] Review surrounding authentication logs for a subsequent successful login.
- [ ] Record the final evidence and close/escalate according to the exercise playbook.
