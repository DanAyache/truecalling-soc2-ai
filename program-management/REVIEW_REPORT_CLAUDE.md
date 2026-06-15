# REVIEW_REPORT.md — SOC 2 Repository Review

**Author:** Claude (review role per [AGENTS.md](AGENTS.md))
**Date:** 2026-06-08
**Status:** REVIEW ONLY — **no files modified, no evidence updated, no policies edited, no commits.**
**Scope reviewed:** `truecalling-soc2-ai/` (policies, procedures, evidence, findings, templates,
CI), plus the workspace governance docs.

> This report focuses on what the existing finding/risk registers do **not** already capture.
> Items already ticketed are cross-referenced by their F-/R- IDs; **newly identified** issues are
> tagged **NEW-n** and are not yet in any register. The two highest-value new findings are a
> **GitHub topology contradiction** (NEW-1) and an **"Engineering Lead" identity ambiguity**
> (NEW-2) — both are evidence-integrity issues of the same class as the already-closed F-18.

---

## 1. Audit Risks

| ID | Risk | Severity | Evidence / basis |
|----|------|:--------:|------------------|
| **NEW-1** | **GitHub topology is self-contradictory across evidence.** The [RBAC review](truecalling-soc2-ai/evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md) and [BOOTSTRAP-CHECKLIST](truecalling-soc2-ai/evidence/BOOTSTRAP-CHECKLIST.md) describe a **GitHub organization `truecalling-ai`** with org-level members and **org-level MFA enforcement**; but the [compensating-control doc](truecalling-soc2-ai/evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md) §4.B (P-4, D-2), the [compliance calendar](truecalling-soc2-ai/templates/compliance-calendar.md) item 5, and finding **F-13** state the repo is on a **personal account `DanAyache` with no org**. Both cannot be true. An auditor cross-checking org screenshots against "no org exists" will treat the org-level MFA/branch-protection evidence as unsupported. | **High** | RBAC review §GitHub uses `github.com/organizations/truecalling-ai`; calendar item 5 "repo hosted under personal account (DanAyache), not a GitHub org" |
| **NEW-2** | **"Engineering Lead" is not a single identified person.** Every policy is "Owner: Engineering Lead." The RBAC review names **Yarone Cohen** as "CEO / Engineering Lead" and the GitHub/Supabase/Vercel **Owner**; CODEOWNERS routes all approvals to **@CohenYarone01**. But the git author and personal-account holder is **Dan Ayache**, whom the 2026-06-08 Human Owner decision names as the **policy signatory** — and Dan Ayache does **not appear in the RBAC member list at all**. So A-6 would sign policies "Dan Ayache, Engineering Lead" while the evidence says the Engineering Lead is Yarone Cohen. | **High** | RBAC review §Vercel ("Yarone Cohen … CEO / Engineering Lead"); CODEOWNERS `@CohenYarone01`; DECISIONS.md (signatory Dan Ayache); RBAC members = CohenYarone01, rbzil only |
| **A-1 / F-19** | Both privileged Supabase accounts show **MFA disabled** at the team level; Patrick has no enrolment evidence. A live CC6.1 deficiency the as-of date cannot paper over. | **Critical** | [supabase-mfa-remediation](truecalling-soc2-ai/evidence/access-control/mfa-enforcement/supabase-mfa-remediation-2026-06.md); RBAC review Issue #1 |
| **NEW-3** | **Self-review of own access.** Yarone Cohen signs the RBAC review that approves **his own** Owner access on all three platforms (reviewer = reviewee). A compensating control exists ([rbac-self-review](truecalling-soc2-ai/evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md)) and Stéphane provides evidence-layer review, but the **platform-layer self-review weakness remains** and is explicitly acknowledged. | Medium | Compensating-control doc §2.2, §3.3 |
| **NEW-4** | **CI scans the wrong codebase for vulnerability management.** `security.yml` npm-audit + Dependabot run against **this compliance repo**, which has **no `package.json`** at root → the audit no-ops. The actual production application (Vercel/Supabase app) is **not** in this repo and is therefore **not** covered by the CC7.1 vulnerability-identification control here. R-014's "mitigated by npm audit + Dependabot" rests on scanning a repo with no application dependencies. | Medium | [security.yml](truecalling-soc2-ai/.github/workflows/security.yml) job 2; [dependabot.yml](truecalling-soc2-ai/.github/dependabot.yml) `directory: "/"` |
| **NEW-5** | **Single approver / unsigned-commit evidence integrity.** CODEOWNERS names one approver (@CohenYarone01); the entire evidence trail sits on one personal account; commits are **not GPG-signed** (tamper-evidence is "structural, not cryptographic" per the compensating-control doc D-1). A single account compromise could rewrite evidence. | Medium | CODEOWNERS; compensating-control D-1 caveat |
| **F-01 / F-16** | Azure scope ambiguity now resolved by Owner decision (out of boundary) but **not yet reflected** in the scope doc/gap analysis/findings register — see [OWNER_DECISION_REVIEW.md](OWNER_DECISION_REVIEW.md). Until edited, the authoritative scope doc still calls Azure "primary." | Medium | scope doc §5/§6; covered in OWNER_DECISION_REVIEW |

---

## 2. Security Risks

| ID | Risk | Severity | Notes |
|----|------|:--------:|-------|
| F-19 / R-002 | Supabase MFA disabled for Yarone + Patrick — single-factor access to customer call/voice-biometric data. | **Critical** | Live exposure; past due (2026-06-04). |
| A-2 | Screenshot `supabase-api-settings-2026-06.png` **exposes the Supabase anon JWT value** (metadata-only rule breach). | **High** | Re-capture redacted; purge + rotate if it reached history. |
| F-13 / NEW-1 | **All SOC 2 evidence hosted on a personal GitHub account.** Compromise or loss of that single personal account = loss/tamper of the entire evidence trail; no org-level audit log or enforcement. | **High** | Compensating-control D-2 notes only a *user* security log is available. |
| F-14 / R-013 | API keys across 5–7 providers **not inventoried** — no way to detect a stale, leaked, or over-scoped credential; Twilio toll-fraud exposure unmonitored. | **High** | Inventory §2 empty. |
| F-05 / R-003 | Privileged accounts on **personal emails** (`cohen.yarone@icloud.com`, `patrick@sarona-partners.com`) — offboarding can't fully revoke if personal email is the auth factor. | Medium | RBAC review Supabase table. |
| R-009 / R-010 | Prompt injection and LLM-provider outage against the AI agent — registered, unmitigated (product-side, not in this repo). | Medium | Risk register. |
| NEW-6 | **No commit signing / branch-protection evidence is unverified.** Branch protection on `main` is "inferred" (P-2), not evidenced; required-review enforcement on a personal-account repo is weaker than org branch protection. | Medium | Compensating-control §4.B P-2; bootstrap §1.2 unchecked. |

---

## 3. Missing Evidence

Folders that exist but contain only `.gitkeep`, or controls referenced with no artifact:

| Evidence | Control | Tracking | State |
|----------|---------|----------|-------|
| Backup restore test log | A1.2 | F-20 / R-007 | **Never executed** — calendar ⏳ Pending; bootstrap §5 unchecked. |
| Onboarding / offboarding packages | CC6.2 | F-07 | Empty `.gitkeep`; security-awareness Day-1 records asserted but unevidenced (F-28 corrected). |
| Vendor SOC 2 reports / DPAs | CC9.2 | F-06 | `evidence/vendor/third-party-reviews/` empty. |
| API Key Inventory rows | CC6.1/6.7 | F-14 | §2 has 0 rows. |
| Patrick MFA screenshot | CC6.1 | F-19 | Does not exist. |
| npm-audit / TruffleHog output artifacts | CC7.1/6.7 | — | `.gitkeep` only; CI produces them but they're not committed. |
| **Google Workspace access evidence** | CC6.1/6.2 | **NEW-7** | Workspace is an in-scope production system (Owner decision 2 + Vendor Policy "Critical") but **no MFA/RBAC/user-list evidence** exists; the RBAC review covers only GitHub/Supabase/Vercel. Recommend opening a finding (e.g. **F-29**). |
| Annual tabletop exercise | CC7.x | — | `evidence/incidents/tabletop-2026.md` referenced, not present (due 2026-12-31). |
| Quarterly review compensating-control statement | CC6.1/CC1.4 | C-3 (Planned) | Not yet integrated into the review template. |
| GitHub MFA **enforcement** (vs account-level) | CC6.1 | NEW-1 | Committed file is `github-mfa-enabled-…png` (account MFA), not org **enforcement**; org enforcement is impossible if no org exists. |

---

## 4. Documentation Inconsistencies

| ID | Inconsistency | Severity | Where |
|----|---------------|:--------:|-------|
| NEW-1 | GitHub **org `truecalling-ai`** vs **personal account `DanAyache`, no org** | **High** | RBAC review / bootstrap vs compensating-control + calendar item 5 + F-13 |
| NEW-2 | **Engineering Lead = Yarone Cohen** (RBAC, CODEOWNERS) vs **signatory Dan Ayache** (Owner decision); Stéphane is Program Lead — three names, unmapped roles | **High** | RBAC review; CODEOWNERS; DECISIONS.md; ISP §3 |
| NEW-8 | **"8 policies" everywhere vs 9 policy files.** README table, compliance calendar, and AGENTS all say 8; `policies/` contains **9** — [acceptable-use-policy.md](truecalling-soc2-ai/policies/acceptable-use-policy.md) is omitted from the README policy table and the count. | Medium | README §Policies; calendar "all 8 policies"; vs `policies/` directory |
| NEW-9 | **"Observation period start 2026-05-27"** still asserted in the [calendar](truecalling-soc2-ai/templates/compliance-calendar.md) header and bootstrap, but `SOC2-ENGAGEMENT-SCOPE.md` §4 explicitly **retires** that note ("the 2026-05-27 'observation period initiated' note is retired"). | Medium | calendar header / bootstrap vs scope doc §4 |
| NEW-10 | **Vendor lists diverge (F-06 made concrete):** **Twilio and FullEnrich** are in the scope doc + API inventory but **absent** from the Vendor Management Policy's "authoritative" Critical-vendor table and R-012; **1Password/Bitwarden** is in the vendor policy + R-012 but absent from the inventory/scope. Twilio processes customer voice/SMS yet is missing from the authoritative vendor inventory. | Medium | [vendor-management-policy](truecalling-soc2-ai/policies/vendor-management-policy.md) §4 vs scope §6 vs R-012 vs API inventory |
| NEW-11 | **Azure present-tense claims** ("primary production platform"; "secrets reside in Azure Key Vault") contradict Azure-future-state (Owner decision). | Medium | scope §5; gap-analysis §1; openai-keys §scope-correction |
| NEW-12 | **GitHub MFA asserted "Yes"** for both members in the RBAC table, but the compensating control P-1 says GitHub MFA "to be verified." | Low | RBAC review §GitHub vs compensating-control §4.B P-1 |
| NEW-13 | **Bootstrap completion table blank** while inline sections 6–7 are checked and artifacts for §§1–4 exist — the checklist's own status is internally unfinished/contradictory. | Low | BOOTSTRAP-CHECKLIST completion table |
| NEW-14 | **Calendar Open Item 3 "Add second GitHub *org* owner"** assumes an org that (per NEW-1) may not exist. | Low | calendar Open Items |
| NEW-15 | **Evidence file-name drift** vs the names the bootstrap/templates specify (`github-mfa-enabled` vs `…-enforcement`; `github-branch-protection` vs `…-main`). | Low | bootstrap §1 vs committed filenames |
| F-16 | F-16 logged **Critical Type-I blocker** while Azure is out of boundary (Owner decision). | Medium | findings §4 (covered in OWNER_DECISION_REVIEW) |

---

## 5. Technical Debt

| ID | Item | Impact |
|----|------|--------|
| TD-1 / F-13 | Nested repos are **gitlinks with no `.gitmodules`**; a clone won't fetch `truecalling-soc2-ai` / `truecalling-incidents`. | High (reproducibility) |
| TD-2 / F-21 | **14,857-file vendored `claude-code-plugins-plus-skills/`** tree inside the compliance repo. | Medium |
| TD-3 | **CODEOWNERS coverage gaps:** `/findings/` (the single-source-of-truth register) and root governance docs (`SOC2-ENGAGEMENT-SCOPE.md`, `README.md`, `SECURITY.md`) are **not** under CODEOWNERS → can change without mandatory review. | Medium |
| TD-4 / NEW-4 | **Dependabot npm target (`/`) has no manifest** → no-op; CI vulnerability control scans nothing meaningful. | Medium |
| TD-5 | **Duplicated artifacts** — RBAC screenshots/CSV at workspace root duplicate `evidence/access-control/`; `truecalling-incidents` vs `truecalling-incidents-setup` near-duplicates. | Medium |
| TD-6 / F-22 | `.claude/settings.local.json` committed (local paths + permissive allowlist). | Low |
| TD-7 | **No top-level `.gitignore`**; `.vscode/`, `.xlsx`, loose screenshots accumulate at root. | Low |
| TD-8 | **Doc-count drift** ("8" vs 9 policies; ΣW "46" vs 47 in the readiness model). | Low |
| TD-9 | **Residual Type II framing** in calendar/bootstrap despite the Type I decision. | Low |

---

## 6. Summary & Recommended Next Steps (non-binding)

**Most urgent (evidence-integrity, because they taint trust in everything else):**
1. **Resolve NEW-1 / NEW-2 first.** Clarify in one authoritative place: (a) is there a `truecalling-ai` GitHub **org** or is everything on the **DanAyache** personal account? (b) Who is the **Engineering Lead** (Yarone Cohen?), the **Human Owner/signatory** (Dan Ayache?), and the **Program Lead** (Stéphane?) — and ensure the A-6 approval block names the role consistently with the rest of the repo. These are the same defect class as the closed F-18 and should be ticketed (suggest **F-30 topology**, **F-31 role mapping**).
2. **A-1 / F-19** — close the live Supabase MFA gap (Patrick enrolment + honest team-view).
3. **A-2** — remove/redact the exposed anon-key screenshot; rotate if it reached history.

**Then:** the already-approved A-3 (inventory), A-4 (restore test), A-5/A-6 (scope + approvals per
[OWNER_DECISION_REVIEW.md](OWNER_DECISION_REVIEW.md)), plus open **F-29** (Google Workspace
evidence) and reconcile the vendor lists (NEW-10 / F-06).

**Newly recommended tickets not yet in any register:** NEW-1 (topology), NEW-2 (role identity),
NEW-4 (CI scans wrong repo / CC7.1 coverage), NEW-7 (Google Workspace evidence), NEW-8 (policy
count), NEW-9 (observation-period framing), NEW-10 (vendor-list reconciliation specifics),
plus CODEOWNERS coverage of `/findings/` and root docs (TD-3).

---

**This document changed nothing.** No files, evidence, policies, configuration, or commits were
modified. All remediation awaits Human Owner approval per [AGENTS.md](AGENTS.md).

*Reviewer: _______________  Date: __________*
