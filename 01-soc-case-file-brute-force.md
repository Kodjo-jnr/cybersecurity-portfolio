# SOC Case File — Brute-Force Login Investigation

**Analyst:** Isaac Osei Mensah
**Environment:** Simulated lab dataset (Windows Server + workstation logs). All names, hosts, and addresses are fictional lab values.
**Case type:** Authentication attack — brute force with successful logon
**Verdict:** True positive — escalated
**MITRE ATT&CK:** T1110.001 (Brute Force: Password Guessing), T1078 (Valid Accounts)

---

## 1. Alert

A lab SIEM rule fired: **"Multiple failed logons from a single source in 10 minutes"** — 214 failed logon events against one workstation (WS-LAB-014) inside 9 minutes, all from one external IP, followed by activity I needed to explain before closing anything.

## 2. What I checked (Observe → Context → Correlate)

**Observe — what the logs actually show**

- Windows Security event **4625** (failed logon), Logon Type **3** (network), repeated against three usernames: `admin`, `jsmith`, and `svc_backup`.
- Source IP: `203.0.113.47` (TEST-NET-3 documentation range, standing in for an external address in this lab).
- One Windows Security event **4624** (successful logon), Logon Type 3, for `svc_backup`, 41 seconds after the last 4625 burst.
- After the success: event **4672** (special logon) for the same account, then a short session and logoff (4634).

What this does **not** prove on its own: a 4624 after failures can be a legitimate user who finally typed the right password. The pattern around it is what decides.

**Context — baseline**

- `svc_backup` is a service account. Service accounts should authenticate on a schedule from known hosts, not interactively from an external IP at 02:13.
- WS-LAB-014 has no business reason to accept inbound network logons from outside the lab LAN.
- The failures cycled through usernames (`admin`, `jsmith`, `svc_backup`) — that is password guessing across accounts, not one user mistyping.

**Correlate — one more source**

- Firewall logs for the same window show repeated inbound connections from `203.0.113.47` to WS-LAB-014 on port 445 (SMB) only — no other hosts targeted in the dataset, no other ports.
- No matching VPN or remote-access logon for that IP or account, so there is no legitimate remote-work explanation.
- Threat-intel check on the source IP (lab-provided feed): flagged for credential-guessing activity.

## 3. Timeline (lab time, UTC)

| Time | Event |
|---|---|
| 02:04:11 | First 4625 from 203.0.113.47 against `admin` |
| 02:04–02:09 | 96 failures across `admin` and `jsmith` |
| 02:09–02:13 | 118 failures against `svc_backup` |
| 02:13:52 | **4624 successful logon, `svc_backup`, Logon Type 3** |
| 02:13:53 | 4672 special logon (elevated privileges assigned) |
| 02:19:07 | 4634 logoff; no further events from the source IP |

## 4. Indicators of compromise

- Source IP: `203.0.113.47`
- Target host: `WS-LAB-014`
- Compromised account: `svc_backup`
- Pattern: 214 × 4625 → 1 × 4624, Logon Type 3, SMB (445)

## 5. Decision

**Escalate as a true positive, severity high.** Reasoning: a service account authenticated successfully from an external IP immediately after a multi-account guessing burst, outside any scheduled window, with elevated privileges. That is a compromised credential until proven otherwise. I would not close this as a false positive, and I would not wait for more evidence before escalating — containment (disable/force-reset `svc_backup`, block the source IP at the perimeter, isolate WS-LAB-014 pending review) is a Tier 2/IR decision and the clock matters.

## 6. Ticket note (as written)

> 02:13 UTC — 214 failed logons (4625, Type 3) from 203.0.113.47 against admin/jsmith/svc_backup on WS-LAB-014, followed by successful logon (4624) + special logon (4672) for svc_backup. Service account, external source, off-hours, no VPN record. Firewall confirms SMB-only inbound from same IP. Verdict: TP, brute force with credential compromise (T1110.001/T1078). Escalating to Tier 2 for containment: disable/reset svc_backup, block source IP, isolate host. Evidence: Security + firewall logs for 02:00–02:30 UTC attached.

## 7. What I would do next if this were my queue

- Confirm containment happened; check the same source IP against every other host in the environment (one dataset showed one target — a real environment needs the sweep).
- Hunt for the same pattern at lower volume (slow guessing under the alert threshold) across the last 30 days.
- Recommend: disable SMB exposure to untrusted networks, MFA or lockout policy on service accounts, alert tuning so a success-after-failures pattern pages immediately (detection rule in `03-sigma-detection-rule.md`).
