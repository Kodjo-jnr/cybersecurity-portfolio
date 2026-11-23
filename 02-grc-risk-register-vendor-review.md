# GRC Work Product — Risk Register and Vendor Review

**Analyst:** Isaac Osei Mensah
**Scenario:** Fictional 40-staff community clinic ("the Clinic") handling electronic protected health information (ePHI). Built as a training work product from my HIPAA operations experience and GRC training. No real organisation, no real patient data.
**Frameworks:** HIPAA Security Rule (45 CFR Part 164, Subpart C) mapped to NIST Cybersecurity Framework 2.0.

---

## Part 1 — Risk register (top 5)

Scoring: Likelihood × Impact on a 1–5 scale. Inherent = before controls. Residual = with current controls in place.

| # | Risk | L×I = Inherent | Key controls in place | Residual | Treatment | Owner | Review |
|---|---|---|---|---|---|---|---|
| 1 | Phishing email leads to credential theft and unauthorised ePHI access | 4×5 = **20** | MFA on email/EHR; annual training; spam filtering | 3×4 = **12** | **Mitigate**: phishing-resistant MFA for EHR, quarterly phishing simulations, one-click report button | Clinic IT Lead | Quarterly |
| 2 | Lost or stolen laptop containing cached ePHI | 3×5 = **15** | Asset inventory; sign-out log | 2×2 = **4** if encrypted | **Mitigate**: full-disk encryption on every endpoint (this also changes the breach analysis — encrypted data is "secured" under HITECH, so a lost encrypted laptop is usually not a reportable breach); remote wipe | Clinic IT Lead | Monthly asset check |
| 3 | Former employee's access not removed after departure | 3×4 = **12** | HR notifies IT by email | 2×3 = **6** | **Mitigate**: same-day deprovisioning checklist tied to HR termination workflow; quarterly access reviews against the HR roster | Practice Manager + IT Lead | Quarterly |
| 4 | Ransomware encrypts the EHR and shared drives | 3×5 = **15** | Antivirus; weekly backups | 2×4 = **8** | **Mitigate + Transfer**: EDR, segmented backups with one offline/immutable copy, tested restore twice a year; cyber insurance for residual financial impact | Clinic Director | Twice yearly (incl. restore test) |
| 5 | Vendor with ePHI access suffers a breach | 2×5 = **10** | BAA signed with main vendors | 2×4 = **8** | **Mitigate**: vendor review before signing (see Part 2), annual reassessment, breach-notification clause (≤72 hours) in every BAA | Compliance Officer | Annually per vendor |

**Reading the register:** Risk 1 stays the top residual risk even with MFA, because the clinic's own incident history pattern (in this scenario) is staff clicking realistic scheduling-themed lures. That is why the treatment spends on simulation and reporting culture, not another tool. Risk 2 shows why one control (encryption) can cut both likelihood of harm and legal exposure at once.

**HIPAA mapping examples:** Risk 3 → §164.312(a)(1) unique user identification and §164.308(a)(3) workforce clearance procedure; Risk 2 → §164.312(a)(2)(iv) encryption (addressable — which means assess, implement if reasonable, and if not, document why and use an equivalent alternative; it does not mean optional); Risk 5 → §164.308(b) business associate contracts. NIST CSF 2.0 functions used: Govern, Identify, Protect, Detect, Respond, Recover.

---

## Part 2 — Vendor review: telehealth scheduling platform ("Vendor S")

**Request:** Clinic wants to adopt a cloud scheduling/telehealth platform in two weeks. The platform stores patient names, appointment details, and visit notes — ePHI. Vendor supplied a SOC 2 Type II report.

**What the SOC 2 report does and does not tell me**

- **Type II** covers *operating effectiveness over a period* (here: 12 months ending 30 September) — stronger than Type I, which is design at a single point in time.
- It covers the Trust Services Criteria for Security and Confidentiality. It does **not** certify HIPAA compliance, and it does not cover the clinic's own side of the connection.
- **Exceptions noted in the report:** two terminated vendor employees retained system access for 6 and 11 days after departure in the audit period. That maps directly to my Risk 3 — and it is the vendor's weakness, not a footnote.
- **Complementary user entity controls (CUECs):** the report assumes the clinic enforces MFA, reviews user access quarterly, and configures session timeouts. If the clinic does not do these, the vendor's controls do not deliver their stated result.
- **Subprocessors:** the report lists a cloud host and an analytics provider; the analytics provider must not receive ePHI — configuration question for implementation, and it must be answered in writing.

**Decision: approve with conditions**

1. BAA signed before any ePHI flows, including a ≤72-hour breach-notification clause and the access-termination finding raised as a contract discussion point; vendor to confirm its corrective action (it states automated deprovisioning was implemented in August — inside the audit period's end, so the next report should show it working).
2. Clinic-side CUECs implemented and evidenced before go-live: MFA enforced, named admin accounts, quarterly access review scheduled.
3. Analytics subprocessor contractually excluded from ePHI; data-flow confirmed in writing.
4. Reassess in 12 months against the next SOC 2 report, checking specifically that the access-termination exception does not recur.

**Business translation:** the report is good enough to proceed, but the one real finding is about exactly the failure that causes healthcare breaches — accounts that outlive employment. We sign with our eyes open, we hold up our side of the controls, and we check again next year.
