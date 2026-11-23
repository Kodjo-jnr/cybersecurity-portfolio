# Detection Rule — Successful Logon After Repeated Failures

**Author:** Isaac Osei Mensah (training portfolio; written and tested against lab log samples)
**Purpose:** Catch the moment a brute-force or password-spray attack *succeeds* — the pattern investigated in `01-soc-case-file-brute-force.md`. Failed logons alone are noisy; a success after a failure burst is the high-signal event.
**MITRE ATT&CK:** T1110.001 (Password Guessing), T1110.003 (Password Spraying), T1078 (Valid Accounts)

---

## Sigma rule

```yaml
title: Successful Logon After Multiple Failed Logons From Same Source
id: 7c1f2a44-3b6d-4e9a-9c2f-1a5d8e0b6f31
status: test
description: Detects a successful Windows logon (4624) from a source that produced many failed logons (4625) shortly before, indicating brute force or password spraying with a compromised credential.
author: Isaac Osei Mensah
date: 2026-11-17
references:
    - https://attack.mitre.org/techniques/T1110/001/
logsource:
    product: windows
    service: security
detection:
    selection_failure:
        EventID: 4625
    selection_success:
        EventID: 4624
    condition: selection_success
correlation:
    type: temporal_ordered
    rules:
        - failed-logons-single-source   # companion count rule: >= 10 x 4625 grouped by IpAddress + TargetUserName
        - this-rule
    group-by:
        - IpAddress
        - ComputerName
    timespan: 10m
    ordered: true
level: high
tags:
    - attack.initial_access
    - attack.credential_access
    - attack.t1110.001
    - attack.t1110.003
    - attack.t1078
falsepositives:
    - A user who mistypes their password repeatedly and then succeeds from their normal workstation (mitigate by alerting on network Logon Type 3/10 and external or unusual sources first, and by excluding known admin jump hosts)
    - Service accounts with an expired rotated password retrying until the new credential propagates (tune per-account after change windows)
fields:
    - IpAddress
    - TargetUserName
    - LogonType
    - ComputerName
```

Companion count rule (summary): `EventID: 4625 | count() by IpAddress, TargetUserName > 10 in 10m` — the correlation fires only when the failure threshold is crossed *before* the success, in that order, from the same source against the same host.

## Equivalent queries

**Splunk SPL**

```spl
index=wineventlog EventCode=4625 OR EventCode=4624 src_ip=*
| bin _time span=10m
| stats count(eval(EventCode=4625)) AS fails count(eval(EventCode=4624)) AS success BY src_ip, dest, user, _time
| where fails>=10 AND success>=1
```

**Microsoft Sentinel KQL**

```kusto
SecurityEvent
| where EventID in (4625, 4624)
| summarize Fails = countif(EventID == 4625), Success = countif(EventID == 4624)
    by IpAddress, Computer, Account, bin(TimeGenerated, 10m)
| where Fails >= 10 and Success >= 1
```

## Test notes (lab samples)

- **Case-file dataset** (`01-soc-case-file-brute-force.md`): 214 × 4625 then 1 × 4624 from the same source in 9 minutes → **fires**, as intended.
- **Normal-user sample** (3 mistyped passwords, then success, Logon Type 2 at the keyboard): below threshold → no alert, as intended.
- **Known limitation:** a slow attack (fewer than 10 failures per 10 minutes per account) stays under this rule. That gap is why the companion hunt looks at 24-hour totals per source across *all* accounts (password spraying spreads failures thin by design). I would pair this rule with a low-and-slow spray correlation rather than lowering this threshold and drowning in false positives.

## Response guidance attached to the alert

Treat a fire as a compromised credential until cleared: identify the account and logon type, check whether the source is expected for that account, look for post-logon activity (4672 special logon, new service installs, unusual process or network activity), and escalate with the timeline rather than closing on the first plausible explanation.
