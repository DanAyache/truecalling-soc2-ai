# PROJECT STATE — 2026-06-03 (END OF SESSION)

**Project:** TrueCalling.ai SOC 2 compliance program
**Compliance repo:** `truecalling-soc2-ai` (branch `main`)
**Supersedes:** `PROJECT-STATE-2026-06-03.md` (checkpoint at commit `a8ff065`)
**This checkpoint:** commit `d6fb016`
**Location:** workspace root (outside the compliance repo — no evidence touched)

---

# Current Git State

- **Branch:** `main`
- **Current commit hash:** `d6fb0165b777d95d9692428cfa77534f72a8be1f` (`d6fb016`)
- **Push status:** in sync — `main...origin/main`, 0 ahead / 0 behind; `origin/main` = `d6fb016`. Nothing unpushed.
- **Commits made since last checkpoint (`a8ff065`):**
  - `d6fb016` — *docs: correct AUD-OBS-7 security-awareness contradiction and F-24 README overstatement; close F-24/F-28* (3 files, +7/−4). **Pushed.**
- **Modified files (uncommitted):**
  - `claude-code-plugins-plus-skills` (submodule pointer — modified content; pre-existing, F-21)
- **Untracked files (uncommitted):**
  - `evidence/access-control/mfa-enforcement/supabase-mfa-yarone-2026-06.png` (F-19 — Yarone account MFA)
  - `evidence/access-control/mfa-enforcement/supabase-team-mfa-2026-06.png` (F-19 — team view, shows Disabled)
  - `evidence/api-keys/screenshots/supabase-api-settings-2026-06.png` (F-14 — ⚠ exposes anon key value)
  - `evidence/api-keys/screenshots/supabase-team-mfa-2026-06.png` (misfiled duplicate of the team MFA shot)
  - `evidence/incidents/truecalling-incidents-repo-private-2026-05-31.png` (pre-existing)

---

# Work Completed Since Last Checkpoint

- **F-24 — README overstates maturity → CLOSED** (commit `d6fb016`). Three minimal README corrections: CC6.2 onboarding/offboarding pointer marked *(planned — not yet collected, F-07)*; CC7.1 pointer corrected to dependabot + npm-audit *(outputs not yet committed)*; "collected audit evidence" softened to "audit evidence (collection in progress)". Register row → Closed.
- **F-28 (AUD-OBS-7) — security-awareness evidence contradiction → CLOSED** (commit `d6fb016`). Dated erratum appended to `evidence/security-awareness/annual-2026.md`: the Day-1 onboarding/AUP records are *asserted, not evidenced* (folders contain only `.gitkeep`); underlying collection tracked under F-07; no records backdated. New finding **F-28** created in register (§4, integrity, CC9.2, Closed) — AUD-OBS-7 is now ticketed and resolved.
- **Readiness change:** **none — 57/100 unchanged.** Both closures were documentation-integrity fixes (CC2.x / CC9.2 honesty); they corrected overstatements rather than moving any control from Documented→Implemented→Evidenced.
- **Scope clarifications (read-only analysis; no files changed):**
  - **Supabase Team MFA column** determined to represent **individual member MFA enrollment status**, NOT org-level enforcement (enforcement requires Pro/Team/Enterprise; org is Free). Source: Supabase MFA docs. ⇒ the team view showing "Disabled" for both is a genuine contradiction of closure, not a plan artifact.
  - **Azure confirmed FUTURE-STATE ONLY** (user-confirmed: no production secrets in Azure, no production resources in Azure). ⇒ F-14 Azure rows (Key Vault / Entra-RBAC / DevOps) drop to **N/A** for this Type I; F-14 minimal set reduces to 5 mandatory providers.
- **Evidence collected this session:** none *committed*. Four untracked screenshots appeared (see Git State); none yet validated/committed.

---

# Current Open Findings

## MUST (Type I blockers)
| ID | Status | % | Remaining work | Blocking dependency |
|----|--------|---|----------------|---------------------|
| **F-01** Framework scope | In Progress | ~80% | Approver+date on scope doc; fill Azure A–F; register → Closed | Eng Lead decision on Azure A–F |
| **F-14** API key inventory | In Progress | ~25% | Enumerate 5 mandatory providers; populate §2; redacted screenshots; Azure rows → N/A; attestation + count; fix R-013 alias/count | Provider portal access |
| **F-16** Azure control coverage | In Progress | ~15% | Stand up + export Azure baseline; policy edits 2–6; add R-016 | Azure cut-over (future-state); F-01 A–F |
| **F-19** Supabase MFA | Evidence Pending | ~50% | Patrick MFA; consistent team view (both Enabled); §4 sign-off; commit; R-002→Mitigated; close | **Patrick enrollment**; team-view contradiction |
| **F-20** Backup restore | Open | ~10% | Run restore to staging; integrity + RPO/RTO; signed log; R-007→Mitigated | Supabase access |
| **R-016** Missing risk entry (AUD-OBS-1) | Open | 0% | Add R-016 (Azure coverage) with L×I, owner, mitigation | — |

## SHOULD
| ID | Status | % | Remaining work | Blocking dependency |
|----|--------|---|----------------|---------------------|
| **F-07** Lifecycle evidence | Open | ~20% | Incumbent attestations + 1 onboarding + 1 offboarding | **Blocked by F-19** (MFA gate) |
| **F-06** Vendor due diligence | Open | ~5% | Collect SOC 2/DPAs; reconcile 4 vendor lists | Vendor portals |
| **F-09** Single GitHub Owner | Open | 0% | Add 2nd Owner + break-glass | — |
| **F-13** Repo on personal account | Open | 0% | Migrate to org or document compensating controls | Org plan (target 2026-09-01) |
| **F-23** Data classification/retention | Open | 0% | Design classification + retention scheme | — |
| **F-05** Supabase personal emails | Open | ~30% | Document exception (mitigated by MFA+RBAC) | — |
| **AUD-OBS-6** Policy approvals | Open | ~10% | Sign 8 subordinate policies + scope doc | — |

## LOW
| ID | Status | % | Remaining work | Blocking dependency |
|----|--------|---|----------------|---------------------|
| **F-21** Vendored 14.8k-file tree | Open | 0% | Remove/submodule `claude-code-plugins-plus-skills` | — |
| **F-22** Local settings committed | Open | 0% | Remove `.claude/settings.local.json` | — |
| **F-24** README overstatement | **Closed** ✅ | 100% | — | — |
| **AUD-OBS-3** R-013 "F-04" alias | Open | 0% | Fix during F-14 | — |
| **AUD-OBS-5** F-14 count "8" vs 10 | Open | 0% | Fix during F-14 | — |

## DEFERRED (out of SOC 2 Type I scope)
| ID | Status | Note |
|----|--------|------|
| **F-25** GDPR | Deferred | Separate program |
| **F-26** EU AI Act | Deferred | Separate program |
| **F-27** ISO 27001 | Deferred | Separate program |

## CLOSED this session
- **F-24** (README overstatement) · **F-28 / AUD-OBS-7** (security-awareness contradiction).
- (Prior baseline closed: F-17, F-18; F-15 Operating; R-005, R-014 Mitigated.)

---

# F-19 Status — outstanding closure criteria

Evidence Pending. Authoritative record: `evidence/access-control/mfa-enforcement/supabase-mfa-remediation-2026-06.md`.

**Still outstanding:**
1. **Patrick TOTP enrollment** — no individual screenshot exists. (`supabase-mfa-patrick-2026-06.png` missing.)
2. **Team view contradiction** — `supabase-team-mfa-2026-06.png` currently shows **both members MFA = Disabled**. Closure requires a re-captured team view showing **both Enabled**. (Column = individual enrollment, confirmed via Supabase docs — so "Disabled" must be resolved, not explained away.)
3. **Yarone attribution** — `supabase-mfa-yarone-2026-06.png` shows "1 app configured" but **no email on-page**; re-capture with `cohen.yarone@icloud.com` visible.
4. **Internal consistency** — account pages (Enabled) must agree with the team view (Enabled for both); they currently conflict.
5. **§4 sign-off** — blank (verified-by, date, both-enabled, org-enforcement follow-up).
6. **Closeout** — commit screenshots; R-002 → Mitigated; RBAC review Issue #1 → Resolved; register F-19 → Closed.

**Verdict: F-19 cannot be closed.** Two hard blockers: Patrick has no MFA evidence; team view disproves closure.

---

# F-14 Status — screenshot inventory

**Revised scope (Azure future-state):** 5 mandatory providers + 2 conditional. Inventory body (`evidence/api-keys/openai-keys.md` §2) is **empty (0 rows)**.

**Screenshots collected (1):**
- `evidence/api-keys/screenshots/supabase-api-settings-2026-06.png` — Supabase API Keys page (anon/service_role).

**Screenshots still required (redacted, → `evidence/api-keys/screenshots/`):**
- `supabase-api-2026-06-03.png` (redacted re-capture of the above) + `supabase-secrets/-auth` as needed
- `github-access-2026-06-03.png`
- `openai-keys-2026-06-03.png`
- `anthropic-keys-2026-06-03.png`
- `vercel-envvars-2026-06-03.png`
- *Conditional (only if active):* `twilio-keys-2026-06-03.png`, `fullenrich-keys-2026-06-03.png`

**Must NOT be committed:**
- `evidence/api-keys/screenshots/supabase-api-settings-2026-06.png` — **exposes the anon public-key JWT value** (violates F-14 metadata-only / no-values rule). Re-capture with values masked before any commit.
- `evidence/api-keys/screenshots/supabase-team-mfa-2026-06.png` — **misfiled MFA duplicate** (byte-identical to the team-MFA shot); not an API-key artifact. Remove/relocate; do not commit under api-keys.

**Azure rows (Key Vault / Entra-RBAC / DevOps):** record as **N/A — not yet operational; pending Azure cut-over (F-16)**. No Azure screenshots required for Type I.

---

# Azure Scope Assessment

- **Current production:** ❌ No
- **Partial production:** ❌ No
- **Future-state only:** ✅ **Yes** (user-confirmed: no production secrets in Azure Key Vault; no production resources running in Azure; migration not started in production).

**Impact on F-14:** the three Azure components are **N/A for this Type I** — they hold no in-scope credentials today. F-14's minimal closeable set is **5 mandatory providers** (Supabase, GitHub, OpenAI, Anthropic, Vercel) + Twilio/FullEnrich if active. ⚠ The `openai-keys.md` §scope-correction line asserting secrets "now reside in Azure Key Vault" is **factually wrong** (present-tense vs future-state) and should be reworded when F-14 is worked — same overstatement pattern as the closed F-24.

**Impact on F-16:** F-16 (Azure control coverage) is **not a current-as-of Type I blocker for the legacy production system**; it becomes mandatory at/after Azure cut-over. Per `SOC2-ENGAGEMENT-SCOPE.md` §5, the audit is scoped to "the production system as it actually runs as of the as-of date," and legacy controls cover any unmigrated component. F-16 + R-016 remain tracked for the future Azure milestone; the 16-item Azure baseline is **not** required to evidence the current legacy system.

> Note: this lightens the previously-stated as-of date dependency — if Type I is assessed against the legacy production system, Azure (F-16) need not be evidenced for that opinion. Confirm with the program lead whether the 2026-07-31 as-of targets the legacy system (Azure deferred) or waits for Azure cut-over.

---

# Current Readiness Score

Deterministic Type I model (ΣW = 47; the model file's "46" is a known doc defect):
- **Design score:** **93%** (Σ(w·D) = 43.5 / 47)
- **Implementation score:** **34%** (Σ(w·I) = 16.0 / 47)
- **Overall Type I readiness:** **57 / 100** = 0.40 × 93 + 0.60 × 34 = 37.2 + 20.4 = 57.6 → **57%**
- **Operating Effectiveness:** N/A — Type I engagement
- **Delta since previous checkpoint (`a8ff065`, 57/100):** **0.** F-24 and F-28 closures corrected documentation-integrity overstatements; no control tier moved, so the score is unchanged. (Band: 40–59% — design largely in place, implementation/evidence thin.)

---

# Next Session Plan (exact execution order)

1. **F-19 finish:** confirm Patrick enrolls TOTP → capture `supabase-mfa-patrick-2026-06.png` (email visible); re-capture Yarone (email visible); **re-capture team view — must show both Enabled**; if still Disabled, MFA isn't active (stop). Then §4 sign-off, commit, R-002→Mitigated, RBAC Issue #1→Resolved, register F-19→Closed.
2. **F-14:** capture the 5 mandatory providers (redacted); decide Twilio/FullEnrich active-or-N/A; populate §2; set Azure rows N/A; dated attestation + key count; fix R-013 alias (AUD-OBS-3) + count (AUD-OBS-5); reword the Azure "now reside" overstatement; commit. (Re-capture the exposed Supabase API shot; drop the misfiled duplicate.)
3. **F-01 close:** approver+date on scope doc; fill Azure A–F (or record "Azure not started, legacy is production"); **add R-016** to risk register; register F-01→Closed.
4. **F-20:** run + log restore test (integrity, RPO/RTO); replace gap file; R-007→Mitigated.
5. **F-07** (after F-19): incumbent attestations + 1 onboarding + 1 offboarding.
6. **F-06 / F-09 / F-13:** vendor SOC 2/DPAs + reconcile lists; 2nd Owner + break-glass; org-ownership plan.
7. **Cleanup:** F-05, F-21, F-22, F-23; sign subordinate policies (AUD-OBS-6); commit npm-audit/TruffleHog outputs.
8. **F-16 (future milestone):** Azure baseline — only when cut-over begins.

---

# Top 5 Risks Preventing SOC 2 Type I Readiness

1. **F-19 live MFA exposure unresolved** — both Supabase production-data accounts show MFA Disabled at team level; Patrick has no enrollment evidence. Critical live exposure + blocks F-07.
2. **F-14 inventory empty + evidence-hygiene breach** — §2 has 0 rows; the one captured screenshot exposes a key value. A core (weight-3) control with no implementation evidence.
3. **F-20 recoverability unproven** — restore test never executed; Availability (A1.2) is in scope; C18 (weight-3) unevidenced.
4. **As-of date / Azure scope ambiguity** — F-01 still Open; whether Type I targets the legacy system (Azure deferred) or waits for Azure cut-over is unconfirmed. This drives whether F-16 is a blocker at all.
5. **Governance concentration** — single GitHub Owner (F-09) + repo on a personal account (F-13); key-person / separation-of-duties risk an auditor treats as a significant deficiency.

---

*Checkpoint only. No compliance documents, controls, or evidence were created or modified. No files staged, committed, or pushed. Readiness reflects the deterministic Type I model as of `d6fb016`.*
