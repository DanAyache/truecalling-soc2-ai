# REMEDIATION_ROADMAP.md — Approved Actions A-1 … A-6

**Author:** Claude (Architecture / Security / SOC 2 review role per [AGENTS.md](AGENTS.md))
**Date:** 2026-06-08
**Status:** DRAFT ROADMAP — planning only. No controls, evidence, or configuration modified.
**Source of authority:** [DECISIONS.md](DECISIONS.md) (2026-06-08) approved A-1…A-6; A-7, A-11–A-15 deferred.
**Plan reference:** [PLAN.md](PLAN.md) §6.

> This document describes *how* each approved action will be executed. It does **not** execute
> any of them. A-1, A-2, A-3, A-6 touch Protected Areas (MFA, API Keys, Secrets, SOC 2 evidence
> repository) per AGENTS.md and require Human Owner sign-off at the point of implementation.
> Implementation is Codex's role for documentation edits; the credential/portal/MFA actions can
> only be performed by the named Human actors. Claude reviews the result.

---

## 0. Execution Overview — Immediate vs. Blocked

The single biggest constraint: **most of this work is gated on a human enabling MFA or granting
authenticated portal access, not on authoring hours.** Classification:

| Action | Can start immediately? | Gated on |
|--------|------------------------|----------|
| **A-1** Supabase MFA closure | ⛔ **Blocked** | Yarone **and** Patrick must enrol TOTP in the live Supabase dashboard; team-view must then show both Enabled |
| **A-2** Exposed anon-key screenshot remediation | ✅ **Immediate** (repo side) | Doc/repo action now; key **rotation** (if value already in git history) needs Supabase access |
| **A-3** API Key Inventory (F-14) | 🟡 **Partial** | Structure/worksheet ready now; row data needs authenticated access to 5–7 provider portals |
| **A-4** Backup restore test (F-20) | ⛔ **Blocked** | Supabase access + a staging project to restore into |
| **A-5** Scope decision & sign-off (F-01) | ✅ **Immediate** | Human Owner decision on Azure A–F and the as-of date; no third-party access needed |
| **A-6** Policy approval records | ✅ **Immediate** | Human Owner (approver) signs; no third-party access needed |

**Recommended ordering:** A-5 and A-6 first (pure decisions, unblock the as-of date and remove
the approval gap), A-2 in parallel (repo hygiene), then A-1 → A-3 → A-4 as access becomes
available. A-1 must precede the deferred F-07 lifecycle evidence; A-5's as-of-date decision
governs whether A-4's restore test and the deferred F-16 are even in the Type I boundary.

---

## A-1 — Close F-19 (Supabase MFA Enforcement)

**Exact objective:** Bring both privileged Supabase accounts (Yarone Cohen — Owner; Patrick
Simon Bouaziz — Administrator) under enforced TOTP MFA, evidenced by live-dashboard screenshots
in which the **team view shows both members Enabled**, then close F-19 and move R-002 to
Mitigated. The bar is *actual enforcement*, not a documented intent.

**Exact steps:**
1. Each member signs in to Supabase → Account → Account Settings → Security / Multi-Factor
   Authentication and adds a **TOTP authenticator** (Google Authenticator / Authy). *SMS is not
   an approved method per Access Control Policy §5.*
2. Each confirms MFA shows **enabled** on their own account security page.
3. Re-check Project → Settings → Team to confirm **both** members show MFA enabled (resolves the
   prior team-view "Disabled" contradiction noted in the latest project state).
4. Capture the three screenshots in §"Evidence required" with member emails visible.
5. Complete the §4 sign-off table in
   [supabase-mfa-remediation-2026-06.md](truecalling-soc2-ai/evidence/access-control/mfa-enforcement/supabase-mfa-remediation-2026-06.md)
   (both-enabled date, org-enforcement follow-up, verified-by, date).
6. Record the org-level enforcement follow-up: Supabase Free plan cannot enforce org-wide MFA;
   note the date enforcement becomes available on plan upgrade and open the follow-up item.
7. Update [Risk Register](truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md)
   R-002 → **Mitigated**; mark Issue #1 **Resolved** in the
   [RBAC review](truecalling-soc2-ai/evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md);
   set **F-19 → Closed** in the [Findings Register](truecalling-soc2-ai/findings/findings-remediation-register.md).
8. Commit: `security: supabase MFA enabled for all members 2026-06`.

**Evidence required:**
- `supabase-mfa-yarone-2026-06.png` — Yarone account security page, TOTP **enabled**, email `cohen.yarone@icloud.com` **visible** (re-capture: existing shot lacks the email).
- `supabase-mfa-patrick-2026-06.png` — Patrick account security page, TOTP **enabled** (does not yet exist — hard blocker).
- `supabase-team-mfa-2026-06.png` — Project → Settings → Team showing **both** Enabled (current shot shows both Disabled — must be re-captured).
- Completed §4 sign-off block; R-002 / Issue #1 / F-19 status updates.

**Human actions required:** Yarone Cohen and Patrick Simon Bouaziz each enrol TOTP (cannot be
delegated). Engineering Lead captures screenshots and signs off.

**Estimated completion time:** **S** — ~1 day of work once both members act; **calendar time
depends entirely on member responsiveness** (already past the 2026-06-04 due date).

**Dependencies:** Patrick's TOTP enrolment (currently no evidence); a re-captured team view that
actually shows both Enabled. Blocks the deferred **F-07** access-lifecycle evidence.

**SOC 2 controls affected:** CC6.1 (Authentication / logical access). Supports R-002, R-008
(protects production customer call/voice-biometric data behind a second factor).

**Success criteria:** Three screenshots committed showing both accounts + team view Enabled
(emails visible); §4 sign-off complete; account pages and team view internally consistent;
R-002 → Mitigated, F-19 → Closed, Issue #1 → Resolved. *If the team view still shows Disabled,
MFA is not active — stop and remediate; do not document around it.*

---

## A-2 — Remediate the Exposed Supabase Anon-Key Screenshot

**Exact objective:** Ensure no committed evidence exposes a credential value; specifically the
Supabase anon JWT visible in `supabase-api-settings-2026-06.png`, which violates the inventory's
metadata-only / no-values rule.

**Exact steps:**
1. Confirm the current state in git: is `evidence/api-keys/screenshots/supabase-api-settings-2026-06.png`
   **committed** or only **untracked** in the working tree? (Latest checkpoint lists it as
   untracked — verify before acting.)
2. Do **not** commit the value-exposing capture. Re-capture the Supabase API settings page with
   the anon / service_role / JWT values **masked**, saved as `supabase-api-2026-06-03.png` (or
   current date), values hidden.
3. Remove the misfiled duplicate `evidence/api-keys/screenshots/supabase-team-mfa-2026-06.png`
   (byte-identical to the MFA team shot — not an API-key artifact; belongs only under
   `mfa-enforcement/`).
4. **If the value-exposing image is already in git history:** treat the anon key as disclosed —
   purge it from history (e.g. `git filter-repo`) **and rotate the affected Supabase key**
   (Protected Area — requires Human Owner approval and Supabase access).
5. Commit the redacted replacement under the F-14 work; reference this remediation in the F-14
   record.

**Evidence required:** Redacted `supabase-api-2026-06-03.png` (values masked); confirmation the
exposing image is absent from the working tree and (if it ever landed) from history; rotation
record if a key was rotated.

**Human actions required:** Engineering Lead re-captures the redacted screenshot. **If history
purge or key rotation is needed:** Human Owner approval (Protected Area) + Supabase access.

**Estimated completion time:** **S** — <1 day if the image is only untracked (delete/replace).
Add time if a history rewrite + key rotation is required.

**Dependencies:** Git-history status of the file. Rotation path depends on Supabase access and
Human Owner approval. Naturally pairs with A-3 (same screenshot set).

**SOC 2 controls affected:** CC6.7 (secret/credential protection), and evidence-integrity under
CC6.1. Relates to R-005 (secrets in source history).

**Success criteria:** No committed artifact exposes a key value; redacted replacement in place;
misfiled duplicate removed; if the value ever reached history, it is purged and the key rotated.

---

## A-3 — Complete the API Key Inventory (F-14)

**Exact objective:** Replace the empty §2 Active Keys table in
[openai-keys.md](truecalling-soc2-ai/evidence/api-keys/openai-keys.md) with a verified,
metadata-only enumeration of every in-scope provider credential, then issue a dated completion
attestation + total key count and close F-14.

**Exact steps:**
1. Confirm the **scope set** with the Human Owner: 5 mandatory providers — **Supabase, GitHub,
   OpenAI, Anthropic, Vercel** — plus **Twilio** and **FullEnrich** (active-or-N/A). Mark the
   three **Azure** rows (Key Vault, Entra/RBAC, DevOps) **N/A — not yet operational** (Azure is
   future-state per A-5); this aligns the inventory with the deferred F-16/A-15.
2. For each in-scope provider, authenticate to the portal listed in §3 of the inventory and
   enumerate active keys/tokens/secrets. Record **metadata only** — name, owner, created,
   rotation-due/expiry, purpose, status. **Never paste a value.**
3. Fill the [worksheet](truecalling-soc2-ai/evidence/api-keys/api-key-inventory-worksheet.md)
   first, then transcribe finalized rows into §2 Active Keys.
4. Capture redacted screenshots (values hidden) into `evidence/api-keys/screenshots/` (coordinate
   with A-2 for the Supabase shot).
5. Reconcile the 2026-05-27 RBAC review's API-key attestation against the now-populated inventory
   (closes the F-18 lineage).
6. Flip each §1 per-provider row to ✅ Complete; replace the §1 "incomplete" statement with a
   dated completion attestation + total key count.
7. Fix the documentation-drift items bundled into F-14: reword the §"Scope correction" line that
   asserts secrets "now reside in Azure Key Vault" (factually wrong — future-state); update Risk
   Register **R-013** to reference **F-14** (not the stale "F-04" alias) — *(AUD-OBS-3 / AUD-OBS-5;
   these specific drift fixes are in-scope here even though the broader A-13 is deferred, because
   the inventory close-out edits these exact lines)*.
8. Commit: `chore: complete F-14 api key inventory <YYYY-MM-DD>`.

**Evidence required:** Populated §2 Active Keys (metadata only); redacted per-provider
screenshots; dated §1 completion attestation + key count; RBAC-review reconciliation note;
R-013 alias/count correction.

**Human actions required:** Engineering Lead performs all portal enumeration (requires
authenticated access to each provider — cannot be delegated to an agent). Human Owner confirms
the Twilio/FullEnrich active-or-N/A scope and the Azure-N/A treatment (Protected Area: API Keys).

**Estimated completion time:** **M** — ~2–4 days of focused work, gated on having credentials/
access to all 5–7 portals in hand.

**Dependencies:** Authenticated access to each provider portal; A-5 (confirms Azure-N/A scope);
A-2 (shared Supabase screenshot). Target date 2026-06-30.

**SOC 2 controls affected:** CC6.1 (logical access — credential ownership), CC6.7 (credential
protection / rotation). Mitigates R-013.

**Success criteria:** Every in-scope provider enumerated and ✅ Complete; §2 populated with
metadata only (zero values); dated attestation + count present; R-013 references F-14; F-14 →
Closed in the Findings Register.

---

## A-4 — Backup Restore Test (F-20)

**Exact objective:** Prove restore capability by performing one real Supabase backup restore into
staging, verifying data integrity and measuring against RPO 24h / RTO 4h, then move R-007 to
Mitigated and close F-20.

**Exact steps:**
1. Copy the [backup-verification-log.md](truecalling-soc2-ai/templates/backup-verification-log.md)
   template to `evidence/backups/verification-logs/<YYYY-MM-DD>-backup-verification.md`.
2. **Pre-test checks:** confirm Supabase daily backup is running and PITR is enabled; record PITR
   retention window, backup retention count, most-recent backup timestamp; confirm a staging
   project is available (record its project ID). *If PITR is not enabled → mark FAIL and open a
   remediation item.*
3. **Select a backup snapshot ≥ 48 hours old**; record the reason (standard quarterly test).
4. **Restore** into the staging project (Database → Backups → Restore); record initiated/
   completed timestamps and duration.
5. **Integrity check:** run the row-count query (`pg_stat_user_tables`) against the restored DB;
   compare key-table counts to expected; capture `supabase-restore-rowcount-<YYYY-MM-DD>.png`.
6. **RPO/RTO assessment:** RPO met if backup ≤ 24h window; RTO met if restore ≤ 4h.
7. Mark **PASS/FAIL**; on FAIL open a P2 incident in `truecalling-incidents` and document root
   cause.
8. Sign off the log; update R-007 → **Mitigated**; set **F-20 → Closed** in the Findings
   Register. Commit per Backup Policy convention.

**Evidence required:** Completed verification log; `supabase-backup-list-<date>.png`; PITR
confirmation screenshot; `supabase-restore-rowcount-<date>.png`; PASS/FAIL result with RPO/RTO
verdict; sign-off and next-test-due date.

**Human actions required:** Engineering Lead (or delegate with access) performs the restore in
the live Supabase dashboard against staging. Requires a staging project to restore into.

**Estimated completion time:** **M** — ~1–3 days, gated on Supabase access and staging
availability (the latest project state notes restore access was pending/blocked).

**Dependencies:** Supabase dashboard access; an available staging project; PITR actually enabled.
In-scope for Type I only if A-5 confirms the as-of date targets the legacy (Supabase) system.

**SOC 2 controls affected:** A1.2 (backup & recovery — Availability). Mitigates R-007.

**Success criteria:** A completed, signed restore log showing PASS — restore succeeded, row
counts verified, RPO and RTO targets met; R-007 → Mitigated; F-20 → Closed.

---

## A-5 — Engagement Scope Decision & Sign-off (F-01)

**Exact objective:** Finalize the SOC 2 engagement scope so the as-of date and the Azure
boundary are unambiguous: sign
[SOC2-ENGAGEMENT-SCOPE.md](truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md), answer the Azure A–F
determinations, add the missing R-016 risk entry, and close F-01.

**Exact steps:**
1. Human Owner records the **decision date**, **approver name + role**, and **approval date**
   (replace the `[YYYY-MM-DD]` / `[name, role]` placeholders in the scope doc header).
2. Answer Azure determinations **A–F** in §5 (the project state already establishes the expected
   answers: production live on **Vercel**, not Azure; Azure has no production resources/secrets
   yet → "Azure not started; legacy is production"). Mirror the same answers in the Azure Gap
   Analysis §2 table.
3. **Decide and record the as-of-date basis:** confirm whether **2026-07-31 targets the legacy
   production system** (Azure deferred → F-16 out of the Type I boundary) **or waits for Azure
   cut-over.** This is the pivotal decision that governs A-4's scope and whether the deferred
   F-16/A-15 is a blocker.
4. Add **R-016** (Azure migration control coverage) to the
   [Risk Register](truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md) with L×I
   score, owner, and mitigation (closes AUD-OBS-1).
5. Set **F-01 → Closed** in the Findings Register; commit.

**Evidence required:** Signed scope document (approver + dates filled, no placeholders); Azure
A–F answered in scope doc + gap analysis; explicit as-of-date basis statement; R-016 row in the
Risk Register; F-01 status update.

**Human actions required:** **Human Owner decision** on Azure A–F and the as-of-date basis, plus
signature. No third-party or portal access required.

**Estimated completion time:** **S** — <1 day once the Human Owner decides.

**Dependencies:** None external — purely a Human Owner decision. **Unblocks** the scope of A-4
and clarifies the deferred F-16/A-15.

**SOC 2 controls affected:** Program scope / governance (CC1.x). Establishes the audit boundary
all other controls are assessed against.

**Success criteria:** Scope doc carries a real approver + dates (no `[ ]` placeholders); Azure
A–F answered consistently in both documents; the as-of-date basis is stated in one sentence;
R-016 is in the Risk Register; F-01 → Closed.

---

## A-6 — Policy Approval Records (AUD-OBS-6)

**Exact objective:** Ensure every in-scope policy and the engagement scope document carries a
signed, dated management approval record, so an auditor can confirm policies were formally
adopted (not merely drafted).

**Exact steps:**
1. Inventory the approval state of all 9 policies in
   [policies/](truecalling-soc2-ai/policies/) plus the scope doc. **Note the gap is not uniform:**
   the [Information Security Policy](truecalling-soc2-ai/policies/information-security-policy.md)
   already has a complete approval block (Approved By / Approval Date / Next Review); the
   [Access Control Policy](truecalling-soc2-ai/policies/access-control-policy.md) has **no**
   formal approval block — only an owner footer. Identify which documents lack the block.
2. For each policy missing it, append a standardized approval block matching the ISP format:
   **Approved By | Approval Date | Next Review Due | Version History**.
3. Human Owner (or the designated approving authority) records the **real approver name/role and
   approval date** for each — consistent with each policy's stated Effective Date.
4. Reconcile dates: several policies state Effective 2026-05-27 and others 2026-06-01 — ensure
   approval dates are coherent with effective dates.
5. Commit: `policy: add management approval records to subordinate policies <YYYY-MM-DD>`.

**Evidence required:** An approval block on all 9 policies + the scope doc, each with a named
approver and a date; reconciled effective/approval dates. (Git commit history provides the
version-history trail.)

**Human actions required:** The approving authority (Human Owner / Engineering Lead per AGENTS.md
governance) signs each record. No third-party access required.

**Estimated completion time:** **S** — <1 day of editing once the approver and dates are
provided.

**Dependencies:** None external. Overlaps with A-5 (scope-doc signature is shared between both
actions — do once).

**SOC 2 controls affected:** CC1.x (control environment — management commitment), CC2.x
(communication of policies). Supports the §4 readiness gap in PLAN.md.

**Success criteria:** Every in-scope policy and the scope doc shows a signed, dated approval
record with coherent effective/approval dates; AUD-OBS-6 resolved.

---

## Approval

Per AGENTS.md Workflow — implementation does not begin until checked:

- [ ] **Human Owner** approves this roadmap and authorizes execution of A-1 … A-6
- [ ] Protected-Area actions (A-1 MFA, A-2 key rotation, A-3 API keys, A-6 approvals) explicitly authorized
- [ ] Named human actors (Yarone, Patrick, Engineering Lead) briefed on their required actions
- [ ] Claude reviews each completed action's evidence before its finding is marked Closed

**No modifications to controls, evidence, or configuration will be made until the box above is
checked.** Reviewer: _______________  Date: __________
