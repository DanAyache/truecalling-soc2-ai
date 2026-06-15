# PROJECT STATE — 2026-06-03

**Project:** TrueCalling.ai SOC 2 compliance program
**Compliance repo:** `truecalling-soc2-ai` (branch `main`)
**Checkpoint author:** session checkpoint (read-only of compliance repo; this file lives at the workspace root, not inside the compliance repo)
**Git commit at checkpoint:** `a8ff0650281ca29c888d264cb3a338ab1db177de` (`a8ff065`)

---

# Current Status

- **Readiness score:** **57 / 100** (deterministic: Design 93% × 0.40 + Implementation 34% × 0.60; ΣW=47, Σ(W·D)=43.5, Σ(W·I)=16.0)
- **SOC 2 phase:** **Type I Readiness** (point-in-time design + implementation). Type II deferred until after Azure cut-over (per `SOC2-ENGAGEMENT-SCOPE.md` §4).
- **Status:** NOT READY — strong design, thin implementation evidence; Azure production target unevidenced.

### Open findings
- **Critical:** F-01 (substantially remediated, not closed), F-14, F-16
- **High:** F-19, F-20, F-06, F-07, F-09, F-13
- **Medium:** F-05, F-21, F-23
- **Low:** F-22, F-24 (partially remediated)
- **Auditor observations (open):** AUD-OBS-1 (R-016 missing), AUD-OBS-2, AUD-OBS-3, AUD-OBS-5, AUD-OBS-6, AUD-OBS-7 (new — security-awareness evidence contradiction)
- **Deferred (out of SOC 2 scope):** F-25 (GDPR), F-26 (EU AI Act), F-27 (ISO 27001)

### Closed findings
- **F-17** — consolidated findings register (Closed)
- **F-18** — RBAC API-key attestation correction (Closed)
- **F-15** — RBAC self-review compensating control (Operating)
- **R-005** (secrets-in-history) and **R-014** (dependency/supply-chain) — Mitigated

---

# Completed Today

1. **F-01 remediation — commit `8a4cba4` (pushed):**
   - Created `SOC2-ENGAGEMENT-SCOPE.md` (engagement type = Type I, target as-of 2026-07-31, TSC scope, Azure statement, system boundary).
   - Corrected `README.md` (SOC 2 Type II → Type I framing).
   - Added scope-note banner to `evidence/BOOTSTRAP-CHECKLIST.md`.
   - Result: F-01 **substantially remediated** — register row still **Open** (approver/date, Azure A–F, and register update deliberately not done).
2. **Dependabot evidence — commit `a8ff065` (pushed):**
   - Added `evidence/security-scans/dependabot/2026-06/dependabot-alerts-2026-06-03.png` (verified: Dependabot **alerts** view, 0 Open / 0 Closed) and `dependabot-summary-2026-06-03.md`.
   - Renamed the screenshot from `...prs...` to `...alerts...` after confirming the content was the alerts view; correctly **not** linked to F-14.
   - Result: C12 (vulnerability management) moved to **Evidenced**; supports R-014 / CC7.1; readiness 56 → 57.
3. **Audit & advisory deliverables (reports only — no file changes):**
   - Multiple full SOC 2 Type I readiness assessments (deterministic, reproducible).
   - F-19 execution checklist (Supabase MFA enrolment steps + screenshots + closure).
   - F-07 closure review (cannot close via backdated founder onboarding; blocked by F-19).
   - MUST / SHOULD / CAN-DEFER classification of all findings.
   - **Discovered AUD-OBS-7**: `security-awareness/annual-2026.md` asserts onboarding packages + signed AUPs at `evidence/onboarding/<name>/` paths that are empty.
4. **Observed at checkpoint (uncommitted, not actioned):** partial F-19 screenshots dropped into `evidence/access-control/mfa-enforcement/` (see Risks).

---

# Open MUST Items

### F-01 — Framework scope (Critical)
- **Status:** In Progress — substantially remediated; register still Open.
- **Remaining work:** add approver name + date to scope doc; fill Azure A–F determinations (still `[answer]`); update register row F-01 → Closed.
- **Evidence location:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`

### F-14 — API Key Inventory (Critical)
- **Status:** In Progress — §2 Active Keys **empty** (0 of 10 providers/components).
- **Remaining work:** enumerate all providers incl. Azure Key Vault + Entra; populate §2 (metadata only, no values); commit per-provider screenshots; fix R-013 stale "F-04" alias → F-14; replace §1 incomplete statement with dated completion attestation.
- **Evidence location:** `evidence/api-keys/openai-keys.md` (+ `api-key-inventory-worksheet.md`, `api-key-inventory-request.xlsx`, `screenshots/`)

### F-16 — Azure control coverage (Critical)
- **Status:** In Progress — gap map only; **no Azure implementation evidence**.
- **Remaining work:** stand up + export the 16-item Azure baseline (Conditional Access MFA, Key Vault soft-delete/purge/RBAC/diagnostics, Defender, Log Analytics, network, break-glass, etc.); complete policy/procedure edits items 2–6; add R-016.
- **Evidence location:** `evidence/azure/azure-control-coverage-gap-analysis.md`

### F-19 — Supabase MFA disabled (High; rubric-Critical live exposure)
- **Status:** Evidence Pending — partial screenshots present **but uncommitted and incomplete** (see Risks).
- **Remaining work:** confirm TOTP enabled for **both** members; provide correctly-named PNGs (`supabase-mfa-yarone-2026-06.png`, `supabase-mfa-patrick-2026-06.png`, `supabase-team-mfa-2026-06.png`); complete §4 sign-off; commit; update R-002 → Mitigated; mark RBAC Issue #1 Resolved; F-19 → Closed.
- **Evidence location:** `evidence/access-control/mfa-enforcement/` (target folder) + `supabase-mfa-remediation-2026-06.md`

### F-20 — Backup restore unverified (High; A1.2, Availability in scope)
- **Status:** Open — "Test Not Performed" gap-acknowledgement.
- **Remaining work:** run restore to staging; capture backup-config screenshot + integrity-query result; document RPO/RTO; replace gap file with signed log; R-007 → Mitigated.
- **Evidence location:** `evidence/backups/verification-logs/2026-05-27-backup-verification.md`

### AUD-OBS-7 — Security-awareness evidence contradiction (new; integrity)
- **Status:** Open — not yet ticketed in register.
- **Remaining work:** append a dated erratum to `annual-2026.md` (per F-18 precedent) OR produce the cited onboarding/AUP artifacts; assign next free F-number. Do **not** backfill/backdate.
- **Evidence location:** `evidence/security-awareness/annual-2026.md` (claims) vs `evidence/onboarding/` (`.gitkeep` only)

### R-016 — Missing risk register entry (AUD-OBS-1)
- **Status:** Open — referenced by F-16 but absent from the register.
- **Remaining work:** add R-016 (Azure migration control coverage) with L×I score, owner, mitigation, review date.
- **Evidence location:** `evidence/risk-register/risk-register-2026.md` (currently R-001…R-015)

---

# Open SHOULD Items

| Item | Status | Note |
|------|--------|------|
| **F-07** Onboarding/offboarding evidence | Open | Design only; folders `.gitkeep`. Needs incumbent attestation + 1 onboarding + 1 offboarding. **Blocked by F-19** (MFA gate). |
| **F-06** Vendor due diligence | Open | `evidence/vendor/**` empty; collect SOC 2/DPAs (esp. Supabase, Azure, LLM providers); reconcile 4 vendor lists. |
| **F-09** Single GitHub Owner / break-glass | Open | Add 2nd Owner + break-glass; partially compensated by F-15. |
| **F-23** Data classification & retention | Open | Confidentiality in scope → design classification + retention scheme. |
| **F-13** Repo on personal GitHub account | Open | Target 2026-09-01 (after as-of); document compensating controls (branch protection, CODEOWNERS) if not migrated. |
| **F-05** Supabase personal emails | Open | Mitigated by MFA + RBAC review; acceptable as documented exception at as-of. |
| **F-24** README overstates maturity | Partially remediated | Type II→I fixed; finish residual accuracy pass. |
| **AUD-OBS-3** R-013 stale "F-04" alias | Open | Cheap; fix during F-14. |
| **AUD-OBS-5** F-14 register count "8" vs 10 | Open | Cheap; fix during F-14. |
| **AUD-OBS-6** Subordinate-policy approval records | Open | Only ISP signed; sign the other 8 policies + scope doc. |

---

# Evidence Inventory

**Populated / present:**
- `access-control/compensating-controls/` — `rbac-self-review-compensating-control.md`
- `access-control/mfa-enforcement/` — `github-mfa-enabled-2026-05-27.png`, `github-branch-protection-2026-05-27.png`, `supabase-mfa-remediation-2026-06.md` *(+ uncommitted partial F-19 screenshots — see Risks)*
- `access-control/quarterly-reviews/rbac-2026-05-27/` — `rbac-review-2026-05-27.md`, GitHub/Supabase/Vercel member PNGs
- `api-keys/` — `openai-keys.md` (§2 empty), `api-key-inventory-worksheet.md`, `api-key-inventory-request.xlsx`, `screenshots/README.md`
- `azure/` — `azure-control-coverage-gap-analysis.md` (gap map only)
- `backups/verification-logs/` — `2026-05-27-backup-verification.md` (gap-acknowledgement)
- `risk-register/` — `risk-register-2026.md` (R-001…R-015; **R-016 missing**)
- `security-awareness/` — `annual-2026.md` (**contradiction — AUD-OBS-7**)
- `security-scans/gitleaks/2026-05/` — SARIF zip, summary txt, scan PNG
- `security-scans/dependabot/2026-06/` — `dependabot-alerts-2026-06-03.png`, `dependabot-summary-2026-06-03.md` *(added today)*
- `BOOTSTRAP-CHECKLIST.md`

**Empty (`.gitkeep` only — no evidence collected):**
- `onboarding/`, `offboarding/` (F-07)
- `vendor/third-party-reviews/`, `vendor/exceptions/` (F-06)
- `incidents/` (no drill — C14)
- `change-management/`, `deployments/`, `access-reviews/`, `screenshots/`
- `security-scans/npm-audit/`, `security-scans/trufflehog/` (CI runs, outputs not committed; only Gitleaks + Dependabot committed)

---

# Next Session Plan (exact order)

1. **F-19 (finish):** correctly name/complete the 3 Supabase MFA screenshots (Yarone + Patrick + team), confirm both enrolled, commit, §4 sign-off, R-002 → Mitigated, F-19 → Closed.
2. **AUD-OBS-7:** append dated erratum to `security-awareness/annual-2026.md` (claims vs empty onboarding folders).
3. **F-01 (close):** approver+date on scope doc, fill Azure A–F, **add R-016** to risk register, update register F-01 → Closed.
4. **F-14:** enumerate 10 providers/components, populate §2, commit screenshots, fix R-013 alias + count.
5. **F-20:** perform + log restore test (integrity + RPO/RTO), replace gap file, R-007 → Mitigated.
6. **F-16:** enable + export Azure baseline (16 controls), edit policies items 2–6, re-rate matrix.
7. **F-07:** incumbent access attestations + 1 onboarding + 1 offboarding (after F-19).
8. **F-06 / F-09 / F-13:** vendor SOC2-DPAs + reconcile lists; 2nd Owner + break-glass; org-ownership plan.
9. **Cleanup:** F-05, F-21, F-22, F-23, F-24; sign subordinate policies; commit npm-audit/TruffleHog outputs.

---

# Risks (unresolved blockers)

1. **F-19 live exposure NOT yet resolved.** Supabase MFA still effectively open: screenshots dropped in are **uncommitted, partial, and mislabeled** — `supabase-mfa-yaron-2026-06.jpeg` (misspelled "yaron", wrong `.jpeg` extension) and `supabase-team-mfa-2026-06.png` present; **Patrick's screenshot missing**; no sign-off; not committed. Until both members are confirmed enrolled and evidence is committed with correct names, F-19 stays open and C05 fails.
2. **Azure baseline (F-16) is the long pole** — 2–4 weeks of real Azure config + exports; gated on F-01's Azure A–F determinations.
3. **F-07 blocked by F-19** (onboarding MFA verification gate); cannot close lifecycle evidence until MFA confirmed.
4. **Evidence-integrity contradiction (AUD-OBS-7)** undermines trust in the evidence package until corrected.
5. **Repo governance on a personal GitHub account (F-13)** + single Owner (F-09) — key-person/SoD concentration.
6. **Scope dependencies:** F-20 is MUST only because Availability is in scope; F-23 is SHOULD/MUST only because Confidentiality is in scope — both confirmed by the current scope doc.

---

# Git Information (`truecalling-soc2-ai`)

- **Current branch:** `main`
- **Current commit hash:** `a8ff0650281ca29c888d264cb3a338ab1db177de` (`a8ff065`)
- **Tracking:** `main...origin/main` — in sync (0 ahead / 0 behind)
- **Uncommitted files:**
  - `claude-code-plugins-plus-skills` (submodule pointer — modified content; pre-existing, F-21)
  - `evidence/access-control/mfa-enforcement/supabase-mfa-yaron-2026-06.jpeg` (untracked — F-19, misnamed)
  - `evidence/access-control/mfa-enforcement/supabase-team-mfa-2026-06.png` (untracked — F-19)
  - `evidence/incidents/truecalling-incidents-repo-private-2026-05-31.png` (untracked — pre-existing)
- **Files not yet pushed:** none — `a8ff065` and `8a4cba4` are both on `origin/main`.

---

*Checkpoint only. No compliance documents, controls, or evidence were created or modified. Readiness and findings reflect the deterministic Type I scoring model as of `a8ff065`.*
