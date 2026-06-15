# APPROVAL_PACKAGE.md — A-5 & A-6 Sign-off Worksheet

**Author:** Claude (review role per [AGENTS.md](AGENTS.md))
**Date:** 2026-06-08
**Status:** DRAFT — decision worksheet. **No files modified.** Recommended wording is a proposal
for the Human Owner to accept, edit, or reject before Codex applies it.
**Scope:** Only **A-5** (engagement scope sign-off, F-01) and **A-6** (policy approval records,
AUD-OBS-6), per [REMEDIATION_ROADMAP.md](REMEDIATION_ROADMAP.md).

> Both actions are pure **management decisions** — no third-party or portal access required.
> Nothing here is applied until the Human Owner fills the blanks and authorizes the edit.
> Where a real **person's name** is needed, it is left as `‹…›` — I will not invent a signatory.

---

## Cross-cutting decision (affects both A-5 and A-6): who signs, and with what date

Two choices recur in every blank below. Decide them once.

### D-1 — Named signatory vs. role title

Every existing document uses the **role** "Engineering Lead," never a person's name. The repo's
git author is `Dan ayache`; the SOC 2 Program Lead is `Stéphane (stephane@truecalling.ai)`.

| Option | Wording | Risk |
|--------|---------|------|
| **A (Recommended)** Named person + role | `Stéphane ‹Last Name›, SOC 2 Program Lead` (and/or `‹Name›, Engineering Lead`) | Best for audit — an auditor wants a **named accountable individual**, not an anonymous title. Requires confirming the real legal name. |
| B Role title only | `Engineering Lead` | Matches the existing Information Security Policy block, but an auditor may flag "who is that?" — role-only approvals weaken the CC1.x management-commitment evidence. |

**Recommendation: Option A.** AGENTS.md vests final authority in the **Human Owner**; the approval
record should name that human. Confirm whether the approving authority is Stéphane (Program Lead),
the Engineering Lead, or both co-signing.

### D-2 — Approval date: actual signing date vs. backdate to "Effective Date"

8 policies state **Effective Date 2026-05-27** (ISP: 2026-06-01); approval is happening **2026-06-08**.

| Option | Wording | Risk |
|--------|---------|------|
| **A (Recommended)** Approval Date = **actual** signing date (2026-06-08), keep Effective Date as-is | `Approval Date: 2026-06-08` | Truthful. May surface the question "the policy was 'effective' 2026-05-27 but only approved 2026-06-08?" — answerable: effective = intended adoption; approval-record formalization followed. Honest gap beats a fabricated one. |
| B Backdate Approval Date to 2026-05-27 to match Effective Date | `Approval Date: 2026-05-27` | **Do not do this.** Backdating an approval to a date it did not occur is exactly the evidence-integrity defect the program already caught and closed three times (F-18, F-24, F-28). An auditor cross-checking git history (the block is added 2026-06-08) against a 2026-05-27 approval date will treat it as falsified evidence — far more damaging than a benign timing gap. |

**Recommendation: Option A — never backdate.** If the policies truly were verbally approved on
their effective dates, record that as a separate, dated note ("originally adopted 2026-05-27;
approval record formalized 2026-06-08"), not by overwriting the approval date.

---

## A-5 — Engagement Scope Decision & Sign-off (F-01)

### Files requiring approval updates

| # | File | What changes |
|---|------|--------------|
| 1 | [truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md](truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md) | Header signature block (3 placeholders) + Azure A–F table (§5) |
| 2 | [truecalling-soc2-ai/evidence/azure/azure-control-coverage-gap-analysis.md](truecalling-soc2-ai/evidence/azure/azure-control-coverage-gap-analysis.md) | §2 migration-status table A–F (mirror the same answers) |
| 3 | [truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md](truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md) | Add **R-016** row |
| 4 | [truecalling-soc2-ai/findings/findings-remediation-register.md](truecalling-soc2-ai/findings/findings-remediation-register.md) | F-01 status → Closed |

### Exact text that needs completion — file 1, header (lines 5–7)

Current:
```
**Decision date:** [YYYY-MM-DD]
**Approved by:** [name, role]  ·  **Approval date:** [YYYY-MM-DD]
```

**Recommended wording** (apply D-1 Option A, D-2 Option A):
```
**Decision date:** 2026-06-08
**Approved by:** ‹Name›, ‹Engineering Lead / SOC 2 Program Lead›  ·  **Approval date:** 2026-06-08
```

### Exact text that needs completion — file 1, §5 Azure determinations (table A–F, currently `[answer]`)

The single most important decision in this package is **B-1 below** (the as-of-date basis). The
A–F answers follow from it. The latest project state already establishes the factual answers
(Azure not started; Vercel is production).

| # | Question | **Recommended answer** |
|---|----------|------------------------|
| A | Production live on Azure, still Vercel, or split? | **Still Vercel.** Azure has no production resources; migration not started in production. |
| B | Azure compute hosting the app? | **None yet** — no Azure compute in production. |
| C | Database: Supabase retained or moving to Azure PostgreSQL? | **Supabase retained** (production DB). |
| D | Identity = Entra ID with Conditional Access? | **No** — identity is GitHub/Supabase/Vercel logins today; Entra ID is future-state. |
| E | Azure region(s) (EU residency)? | **N/A** — no Azure resources provisioned. |
| F | Source/CI on GitHub or Azure DevOps? | **GitHub** (staying). |

> Mirror these six answers verbatim into the gap-analysis §2 table (file 2) so the two documents
> agree — an inconsistency between them is itself an audit finding.

### Pivotal decision — B-1: the as-of-date basis

This governs whether A-4 (backup restore) and the deferred F-16/A-15 are even inside the Type I
boundary. State the chosen basis in one sentence in the scope doc §2.

| Option | One-sentence wording to add | Risk |
|--------|------------------------------|------|
| **A (Recommended)** Type I assessed against the **legacy** production system; Azure deferred | "The 2026-07-31 as-of date is assessed against the legacy production system (Vercel + Supabase + GitHub); the Azure migration (F-16) is **out of the Type I boundary** and deferred to the Type II milestone." | Fastest path to a Type I opinion — controls already exist for the legacy stack. Risk: if Azure cut-over happens **before** 2026-07-31, the report's boundary no longer matches reality and must be re-scoped. Mitigate by freezing Azure production changes until after the as-of date, or moving the as-of date earlier. |
| B Type I **waits** for Azure cut-over | "The as-of date moves to the date Azure cut-over completes and all Azure controls (F-16) are implemented and evidenced." | Single report covers the real future platform. Risk: makes the **entire 16-item Azure baseline (F-16) a hard blocker**, pushing the as-of date out by months and keeping F-01 open. Contradicts the current 57/100 readiness path. |

**Recommendation: Option A** — it matches `SOC2-ENGAGEMENT-SCOPE.md` §5 ("scoped to the production
system as it actually runs as of the as-of date") and the project-state guidance, and keeps the
2026-07-31 target achievable.

### Exact text that needs completion — file 3, R-016 (new Risk Register row)

The Risk Register §4 table has columns: `ID | Description | L | I | R | Existing Controls |
Mitigation Plan | Owner | Status | Review Date`. **Recommended row:**

```
| R-016 | Azure migration control coverage — documented control set was authored for Vercel/Supabase/GitHub; Azure access, secret, logging, change-management, and backup controls are not yet designed/implemented for the future Azure production target | 3 | 4 | **12 High** | Azure Control Coverage Gap Analysis (F-16) documents the required baseline; legacy controls cover current production; Azure not yet in production | Build + evidence the Azure security baseline (Conditional Access MFA, Key Vault soft-delete/purge/RBAC/diagnostics, Defender for Cloud, Log Analytics) before cut-over; do not cut over production until F-16 controls are implemented and evidenced | Engineering Lead | Open | 2026-09-01 |
```

- **Risk of the L=3 / I=4 = 12 (High) scoring:** It signals Azure as a material open risk without
  overstating it (not Critical, because Azure holds no production data today). If the Human Owner
  judges cut-over imminent, raise L to 4 (→ 16, still High). Under-scoring (e.g. Low) would
  understate a known migration gap and read as risk-register window-dressing to an auditor.

### Exact text — file 4, Findings Register F-01

Current F-01 row status (§5): `Open`. **Recommended:** change to
`**Closed** (2026-06-08 — scope doc signed; Azure A–F answered "legacy is production, Azure deferred"; R-016 added)`
and move the row to §7 reference table if desired. **Risk of closing F-01:** only close it once
files 1–3 are actually signed/added — closing the finding while the scope doc still shows
placeholders would re-create an evidence-contradiction (the F-18 pattern). Close **last**.

---

## A-6 — Policy Approval Records (AUD-OBS-6)

### Files requiring approval updates

**1 of 9 policies already has a complete approval block** — use it as the template:

| Status | File | Effective Date | Action |
|--------|------|----------------|--------|
| ✅ Has block | [information-security-policy.md](truecalling-soc2-ai/policies/information-security-policy.md) | 2026-06-01 | None (model to copy) — optionally re-confirm signatory per D-1 |
| ❌ No block | [access-control-policy.md](truecalling-soc2-ai/policies/access-control-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [acceptable-use-policy.md](truecalling-soc2-ai/policies/acceptable-use-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [backup-policy.md](truecalling-soc2-ai/policies/backup-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [change-management-policy.md](truecalling-soc2-ai/policies/change-management-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [incident-response-policy.md](truecalling-soc2-ai/policies/incident-response-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [secrets-management-policy.md](truecalling-soc2-ai/policies/secrets-management-policy.md) | 2026-05-27 (v1.0) / 2026-06-02 (v1.1) | Add block; note v1.1 approval |
| ❌ No block | [security-awareness-policy.md](truecalling-soc2-ai/policies/security-awareness-policy.md) | 2026-05-27 | Add block |
| ❌ No block | [vendor-management-policy.md](truecalling-soc2-ai/policies/vendor-management-policy.md) | 2026-05-27 | Add block |

→ **8 policies need a block added.** (The scope doc's signature is handled under A-5, so it is
not double-counted here.)

### Exact insertion point (identical pattern in all 8)

Each of the 8 ends with this two-line footer (line numbers vary per file):
```
*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
```

The approval block is inserted **immediately above that footer**, separated by a `---` rule —
matching how the Information Security Policy is structured.

### Recommended wording — approval block to insert (per policy)

```
---

## Approval

This policy is approved by management on behalf of TrueCalling.ai and is reviewed annually.

| Field | Value |
|-------|-------|
| **Approved By** | ‹Name›, ‹Engineering Lead / SOC 2 Program Lead› |
| **Approval Date** | 2026-06-08 |
| **Effective Date** | 2026-05-27 |
| **Next Review Due** | 2027-05-27 |
| **Version History** | Tracked via `git log policies/‹this-file›.md` |

---
```

Per-file adjustments:
- **secrets-management-policy.md** — it already has a Version History table (v1.0 2026-05-27,
  v1.1 2026-06-02). Set **Effective Date** to "2026-05-27 (v1.0); 2026-06-02 (v1.1)" and add a
  line: "v1.1 (Azure Key Vault addition) approved 2026-06-08." Do **not** duplicate the existing
  version table — reference it.
- **information-security-policy.md** — already compliant; only re-confirm the signatory if D-1
  Option A changes "Engineering Lead" to a named person.

### Risks of each option (A-6)

| Decision | Option | Risk |
|----------|--------|------|
| Signatory | Named person (D-1 A) | Strongest CC1.x evidence; requires the real name. **Recommended.** |
| Signatory | Role only (D-1 B) | Matches existing ISP but auditor may challenge accountability. |
| Approval date | Actual (2026-06-08) (D-2 A) | Truthful; minor "effective-before-approved" question, easily explained. **Recommended.** |
| Approval date | Backdated to 2026-05-27 (D-2 B) | **Falsified evidence** vs. git history; repeats the F-18/F-24/F-28 defect class. **Reject.** |
| Effective vs. Approval mismatch | Keep effective 2026-05-27, approval 2026-06-08, add adoption note | Transparent. **Recommended.** |
| Effective vs. Approval mismatch | Silently leave a 12-day unexplained gap | Auditor asks why; looks like a process gap rather than a documented timeline. |
| Consistency | Apply the **same** block format to all 8 | Uniform, easy to audit. **Recommended.** |
| Consistency | Hand-vary each block | Inconsistent approval records read as ad-hoc governance. |

---

## What the Human Owner must provide to unblock A-5 + A-6

A short checklist — once these are answered, Codex can apply both actions in well under a day:

1. **Approving authority's real name + role** (`‹Name›` above) — and whether one person or co-signers. *(D-1)*
2. **Confirm approval date = 2026-06-08** (or the actual signing date). *(D-2)*
3. **B-1 as-of-date basis** — legacy-system Type I (recommended) **or** wait-for-Azure. *(A-5)*
4. **Confirm the Azure A–F answers** above are correct (Azure not in production). *(A-5)*
5. **Approve the R-016 scoring** (L3×I4 = 12 High) or adjust. *(A-5)*
6. **Authorize** applying these edits (Protected Area: SOC 2 evidence repository).

---

## Approval

- [ ] Human Owner provides items 1–6 above
- [ ] Human Owner authorizes Codex to apply the agreed wording to A-5 and A-6 files
- [ ] Claude reviews the applied edits before F-01 / AUD-OBS-6 are marked resolved

**No files will be modified until authorized.** Reviewer: _______________  Date: __________
