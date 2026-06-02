# Findings & Remediation Register

**Organization:** TrueCalling.ai
**Version:** 1.0
**Created:** 2026-06-02
**Owner:** Engineering Lead
**SOC 2 Program Lead:** Stéphane (stephane@truecalling.ai)
**Review Cycle:** Monthly, and on any new finding
**SOC 2 Criteria:** CC3.x (risk assessment), CC4.x (monitoring), CC5.x (control activities); supports ISO 27001 Cl. 10.1 (nonconformity & corrective action)

---

## 1. Purpose

This register is the **single authoritative source of truth** for every audit finding, its severity, owner, due date, status, and remediation evidence. It was created to close audit finding **C-6 / F-17**: findings F-04…F-15 were previously referenced across policies, the risk register, and compensating-control documents **with no consolidated catalogue**, and with **inconsistent numbering** (the API Key Inventory was called "F-04" in Risk Register R-013 but "F-14" in the project state).

All finding IDs are now canonical here. When any document references a finding, it must match this register.

---

## 2. ID reconciliation notes

| Issue | Resolution |
|-------|------------|
| API Key Inventory referenced as both **F-04** (R-013) and **F-14** (project state / active remediation) | **Canonical = F-14.** R-013's "(F-04)" is a stale alias for the same finding. Action: update R-013 to reference F-14. |
| **F-15** referenced in R-004/R-006 as "F-15 §4.A / §4.B" | F-15 = the **RBAC self-review compensating control** document. Recorded below as a closed/operating control, not an open finding. |
| Findings F-01, F-02, F-03, F-08, F-10, F-11, F-12 | Numbers referenced historically but **no source artifact exists** in the repo. Recorded as "Unverified — reserved" pending the program lead confirming their meaning. Do not reuse these numbers. |

---

## 3. Status legend

- **Open** — identified, remediation not started
- **In Progress** — remediation underway
- **Evidence Pending** — control designed/implemented; awaiting screenshot/log/attestation
- **Closed** — remediated and evidenced
- **Deferred** — out of current SOC 2 Type I sprint scope (tracked, not worked)

Severity: **Critical / High / Medium / Low** (per the 2026-06-02 audit).

---

## 4. Active findings — current SOC 2 Type I sprint

| ID | Finding | Sev | SOC 2 | Risk | Owner | Target | Status | Remediation evidence / artifact |
|----|---------|-----|-------|------|-------|--------|--------|---------------------------------|
| **F-14** | API Key Inventory incomplete — 0 of 8 providers/components enumerated | Critical | CC6.1, CC6.7 | R-013 | Eng Lead | 2026-06-30 | **In Progress** (scoped & structured 2026-06-02; awaiting enumeration) | [openai-keys.md](../evidence/api-keys/openai-keys.md) §9.1; [worksheet](../evidence/api-keys/api-key-inventory-worksheet.md); [request](../evidence/api-keys/api-key-inventory-request.xlsx) |
| **F-16** | Azure control coverage gap — control set documents Vercel/Supabase, not the Azure production target | Critical | CC6.1/6.6/6.7, CC7.x, CC8.1, A1.2 | R-016 | Eng Lead | 2026-07-15 | **In Progress** (gap analysis done 2026-06-02; artifact edits queued) | [azure-control-coverage-gap-analysis.md](../evidence/azure/azure-control-coverage-gap-analysis.md) |
| **F-17** | No consolidated findings/remediation register; inconsistent finding IDs | Critical | CC3.x, CC4.x | — | Eng Lead | 2026-06-02 | **Closed** (this document) | this register |
| **F-18** | Evidence contradiction — RBAC review attests an API-key review the inventory says never occurred (audit C-3) | Critical | CC6.1 (evidence integrity) | R-013 | Eng Lead | 2026-06-02 | **Closed** (correction appended) | [rbac-review-2026-05-27.md](../evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md) §Correction |
| **R-002 / F-19** | Supabase MFA disabled for both members (Owner + Administrator) | High | CC6.1 | R-002 | Eng Lead | 2026-06-04 | **Evidence Pending** (record + collection steps ready; awaiting members to enable MFA + screenshot) | [supabase-mfa-remediation-2026-06.md](../evidence/access-control/mfa-enforcement/supabase-mfa-remediation-2026-06.md) |

---

## 5. Active findings — SOC 2 Type I readiness (queued, in scope, not yet started this sprint)

| ID | Finding | Sev | SOC 2 | Risk | Owner | Target | Status |
|----|---------|-----|-------|------|-------|--------|--------|
| **F-01** | Framework scope undecided/contradictory — Type I objective vs Type II framing in README/Bootstrap (audit C-1) | Critical | program scope | — | Program Lead | 2026-06-15 | Open |
| **F-07** | Access-lifecycle evidence empty — no executed onboarding/offboarding packages (audit H-6) | High | CC6.2 | R-015 | Eng Lead | 2026-07-31 | Open |
| **F-06** | Vendor due diligence not collected — no sub-processor SOC 2 / DPA (audit H-5); add Microsoft Azure | High | CC9.2 | R-012 | Eng Lead | 2026-07-31 | Open |
| **F-09** | Single GitHub Owner / key-person dependency — no second Owner (audit H-3) | High | CC1.x, CC6.1 | R-001 | Eng Lead | 2026-07-01 | Open |
| **R-007 / F-20** | Backup restore capability unverified (audit H-1) | High | A1.2 | R-007 | Eng Lead | 2026-06-30 | Open |
| **F-13** | Repository under personal GitHub account; not org-owned (audit H-4) | High | CC6.1, CC8.1 | R-004 | Eng Lead | 2026-09-01 | Open |
| **F-05** | Supabase accounts on personal emails, not company domain (audit M-3) | Medium | CC6.1, CC6.2 | R-003 | Eng Lead | 2026-09-01 | Open |
| **F-21** | 14,857-file `claude-code-plugins-plus-skills` tree vendored into compliance repo (audit M-1) | Medium | CC8.1 | — | Eng Lead | 2026-06-30 | Open |
| **F-22** | Developer-local `.claude/settings.local.json` committed (audit M-2) | Low | CC8.1 | — | Eng Lead | 2026-06-30 | Open |
| **F-23** | No data classification / retention-erasure schedule (audit M-4) | Medium | CC3.2, C-series | R-008 | Eng Lead | 2026-08-31 | Open |
| **F-24** | README overstates maturity vs collected evidence (audit M-6) | Low | CC2.x | — | Eng Lead | 2026-06-30 | Open |

---

## 6. Deferred findings — NOT in current sprint (tracked only)

Per current direction, GDPR / ISO 27001 / EU AI Act work is deferred. Listed so nothing is lost.

| ID | Finding | Framework | Status |
|----|---------|-----------|--------|
| F-25 | GDPR program absent (RoPA, DPIA for voice/biometric, lawful basis, retention/erasure, privacy notice, DPA register) — audit C-4 | GDPR | Deferred |
| F-26 | EU AI Act program absent (system classification, Annex IV technical file, transparency, human oversight, logging) — audit C-5 | EU AI Act | Deferred |
| F-27 | ISO 27001 ISMS artifacts absent (scope, SoA, risk treatment plan, internal audit, management review) — audit §9 | ISO 27001 | Deferred |

---

## 7. Closed / operating controls (reference)

| ID | Item | Status | Evidence |
|----|------|--------|----------|
| F-15 | RBAC self-review compensating control (separation-of-duties for single reviewer) | Operating | [rbac-self-review-compensating-control.md](../evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md) |
| R-005 | Secrets-in-source-history risk | Mitigated | Gitleaks + TruffleHog CI |
| R-014 | Dependency/supply-chain risk | Mitigated | npm audit + Dependabot |

---

## 8. Maintenance

1. Every new finding gets the **next free F-number** here first, then is referenced elsewhere by that ID.
2. Update **Status** and **evidence** link as remediation progresses; never delete a finding — move it to Closed with its evidence.
3. Reconcile against the [Risk Register](../evidence/risk-register/risk-register-2026.md) at each monthly review (every R-xxx with an open mitigation should map to an F-id here).
4. Commit changes: `chore: update findings register <YYYY-MM-DD>`.

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Next review: 2026-07-01*
*Evidence retention: 3 years*
