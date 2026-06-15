# OWNER_DECISION_REVIEW.md — Review of Human Owner Decisions (2026-06-08)

**Author:** Claude (review role per [AGENTS.md](AGENTS.md))
**Date:** 2026-06-08
**Status:** REVIEW ONLY — **no files modified, no evidence updated, no commits, no remediation.**
**Inputs reviewed:** [PLAN.md](PLAN.md), [REMEDIATION_ROADMAP.md](REMEDIATION_ROADMAP.md),
[APPROVAL_PACKAGE.md](APPROVAL_PACKAGE.md),
[Findings Register](truecalling-soc2-ai/findings/findings-remediation-register.md),
[Risk Register](truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md),
[SOC2-ENGAGEMENT-SCOPE.md](truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md).

> Bottom line: the six decisions are coherent and **unblock A-5, A-6, and the closure of F-01** —
> but closure and "approved" status are **conditional on documentation edits that have not yet
> been made.** Three decisions also create downstream reconciliation work the original
> APPROVAL_PACKAGE did not fully anticipate (Azure already written *into* scope; "Deferred" not a
> Risk-Register status; Google Workspace newly named in-scope). Details below.

---

## 1. Validation of each decision

| # | Decision | Verdict | Notes |
|---|----------|:------:|-------|
| 1 | As-of date remains **2026-07-31** | ✅ Consistent | Matches `SOC2-ENGAGEMENT-SCOPE.md` §2 and the README. No change needed. |
| 2 | Type I scope = **Vercel, Supabase, GitHub, Google Workspace** (current production) | 🟠 Consistent *but requires edits* | The **Risk Register §2** already names exactly these four. The **scope doc §6** does **not** — it calls Vercel/Supabase/GitHub "legacy" and Azure "primary," and lists Google Workspace only as a *sub-processor*, not a platform. Scope doc must be rewritten (see §4). **Google Workspace being elevated to an in-scope production platform creates a new evidence expectation** (§3, gap G-1). |
| 3 | **Azure** future-state only, **outside** the Type I boundary | 🟠 Correct decision, *contradicts current docs* | This is the right call and matches PLAN.md / project-state guidance. But it **directly contradicts** `SOC2-ENGAGEMENT-SCOPE.md` §5 ("Azure is the **primary** production platform, and production secrets **reside in** Azure Key Vault") and the Azure gap-analysis §1 (same present-tense claims). Those statements are now **factually wrong** and must be reframed as future-state. **F-16 must also be reclassified** from a Critical Type-I blocker to a deferred/future-milestone item. |
| 4 | Signatory = **Dan Ayache, Human Owner** | 🟠 Authorized, *title needs mapping* | AGENTS.md vests final authority in the Human Owner, so Dan Ayache signing is correct. **Concern:** "Human Owner" is an internal AI-governance term — an external auditor will not recognize it as a management title, and every existing doc names the approving role as **"Engineering Lead."** Recommend the approval records read **"Dan Ayache, Engineering Lead"** (or "…, Owner/Founder") with "Human Owner" as an internal note, and that you confirm Dan Ayache **is** the "Engineering Lead" referenced throughout (vs. Stéphane, the SOC 2 Program Lead). See §2 concern AC-2. |
| 5 | Approval date **2026-06-08**, **no backdating** | ✅ Correct | Matches APPROVAL_PACKAGE D-2 Option A and avoids the F-18/F-24/F-28 integrity defect. **Note:** 8 policies show Effective Date 2026-05-27 (ISP 2026-06-01); approving 2026-06-08 leaves an "effective-before-approved" gap that should carry a one-line adoption note rather than be silently left (AC-3). |
| 6 | Create **R-016 "Azure Migration Risk", Status: Deferred** | 🟠 Intent clear, *status value undefined* | Adding R-016 resolves the dangling reference in F-16 and the gap analysis (closes AUD-OBS-1). **Problem:** **"Deferred" is not a status in the Risk Register's own legend** (§3.4 allows Open / In Progress / Mitigated / Accepted / Closed). Using it as-is creates an internal inconsistency an auditor will catch. Reconcile (see §4-C). Human Owner also did **not** specify L×I scoring — needs a value. |

**Overall:** all six are accepted as sound management decisions. None is rejected. Four (2, 3, 4, 6)
require documentation reconciliation before they are internally consistent.

---

## 2. Inconsistencies & audit concerns

**INC — Inconsistencies (current docs vs. the decisions):**

- **INC-1 (High):** `SOC2-ENGAGEMENT-SCOPE.md` §5/§6 place **Azure in scope as the primary
  platform** and the four production systems as "legacy." Decision 3 reverses this. Until edited,
  the authoritative scope document contradicts the Human Owner's boundary.
- **INC-2 (High):** Scope doc §5 and gap-analysis §1 state **production secrets "reside in Azure
  Key Vault"** (present tense). Decision 3 says Azure is future-state with no production secrets.
  These are now false statements in committed evidence.
- **INC-3 (Medium):** **F-16** is logged in the Findings Register §4 as **Critical / In Progress /
  Type-I blocker (target 2026-07-15).** Decision 3 takes Azure out of the Type I boundary, so F-16
  is no longer a current-sprint blocker — it must move to deferred/future-milestone, or the
  register shows an open Critical that is simultaneously "out of scope."
- **INC-4 (Medium):** **"Deferred" status for R-016** is undefined in the Risk Register legend
  (§3.4). The Findings Register defines "Deferred"; the Risk Register does not.
- **INC-5 (Low):** Scope doc §6 lists **Google Workspace** only under sub-processors; Decision 2
  makes it an in-scope **platform**. Minor, but the boundary list must be updated.

**AC — Audit concerns:**

- **AC-1 (Medium):** **Do not close F-01 or mark A-5 "done" before the edits land.** The scope doc
  still contains placeholders and Azure-in-scope text. Closing the finding against an unsigned,
  self-contradicting document would recreate exactly the evidence-contradiction class the program
  already closed (F-18/F-24/F-28).
- **AC-2 (Medium):** **Signatory title & identity.** "Human Owner" is not an auditor-recognizable
  management title; the repo uses "Engineering Lead." Confirm Dan Ayache = Engineering Lead and use
  a business title in the approval records. Also note **key-person concentration** (F-09/F-13):
  Dan Ayache owns the personal GitHub account hosting the repo, authors the commits, **and** signs
  the approvals — a separation-of-duties optic for CC1.x. Pre-existing; the F-15 compensating
  control partially addresses it, but naming the same individual as sole approver reinforces it.
- **AC-3 (Low):** **Effective-before-approved timeline** (Effective 2026-05-27 vs. Approval
  2026-06-08). Honest and acceptable, but add a one-line note ("originally adopted 2026-05-27;
  formal approval record dated 2026-06-08") so the gap reads as documented, not accidental.
- **AC-4 (Low→Medium):** **Azure-out-of-scope must be definitively true.** The boundary holds only
  if **no** production data/secret actually sits in Azure today. Project-state asserts this is
  user-confirmed; reaffirm it, because the gap analysis and `openai-keys.md` currently claim the
  opposite, and a single real Azure-resident production secret would invalidate the out-of-scope
  claim.

---

## 3. Evidence gaps

- **G-1 (NEW — Medium):** **Google Workspace access-control evidence is absent.** By naming
  Workspace an in-scope production system (Decision 2), the audit now expects Workspace admin-console
  **MFA enforcement, admin RBAC, and a user list** — none collected. The 2026-05-27 RBAC review
  covers GitHub/Supabase/Vercel **only**. Recommend opening a new finding (suggested **F-29 —
  Google Workspace access evidence not collected**, CC6.1/CC6.2) and adding Workspace to the next
  RBAC review and the API Key Inventory scope.
- **G-2 (Existing):** F-19 (Supabase MFA) and F-14 (API Key Inventory) remain the live blockers;
  unaffected by these decisions but still gating overall readiness (A-1, A-3).
- **G-3 (Existing):** R-016's mitigation evidence is intentionally **deferred** — acceptable, but
  the register must show the deferral trigger ("mitigation begins at Azure cut-over"), not a blank.

---

## 4. Documentation updates required

*(Recommendations only — not applied. Grouped by file.)*

**A. [SOC2-ENGAGEMENT-SCOPE.md](truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md)**
1. Header (lines 5–7): `Decision date: 2026-06-08`; `Approved by: Dan Ayache, Engineering Lead (Human Owner)`; `Approval date: 2026-06-08`.
2. §5: rewrite so **Azure is future-state, outside the Type I boundary, deferred to the Type II milestone**; remove "primary production platform" and "secrets reside in Azure Key Vault."
3. §5 A–F table: fill answers — A *Still Vercel*; B *None yet*; C *Supabase retained*; D *No (Entra ID future-state)*; E *N/A*; F *GitHub*.
4. §6: in-scope **Platforms = Vercel, Supabase, GitHub, Google Workspace**; move Azure to **Out of scope (future-state)**. Add Google Workspace as a platform, not only a sub-processor.

**B. [azure-control-coverage-gap-analysis.md](truecalling-soc2-ai/evidence/azure/azure-control-coverage-gap-analysis.md)**
5. §1: reframe present-tense Azure claims as future-state.
6. §2 A–F table: mirror the scope-doc answers ("Azure not started").
7. Add a banner: **F-16 deferred — Azure outside the current Type I boundary; baseline required at cut-over.**

**C. [Risk Register](truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md)**
8. Add **R-016 — "Azure Migration Risk"**, Status **Deferred**, with a deferral trigger ("active mitigation begins when Azure implementation/cut-over starts; reviewed at that point").
9. **Reconcile the status legend (§3.4):** add **"Deferred — tracked; mitigation begins on a defined trigger"** (mirrors the Findings Register), so R-016's status is legal. *(Preferred over forcing R-016 into "Accepted," which would require a compensating control + expiration it doesn't have.)*
10. Confirm **scoring**: recommend **L 2 × I 4 = 8 (Medium)** given Azure holds no in-scope data today and work is deferred — or retain the analytical 12 (High) from APPROVAL_PACKAGE with status Deferred. **Human Owner to pick.**

**D. [Findings Register](truecalling-soc2-ai/findings/findings-remediation-register.md)**
11. **F-16:** move from §4 (Critical Type-I blocker) to a **deferred/future-milestone** classification consistent with Decision 3; note "Azure outside Type I boundary; reactivates at cut-over." Cross-reference R-016.
12. **F-01 → Closed** — *but only in the final commit, after A is signed* (see AC-1): "Closed 2026-06-08 — scope signed by Dan Ayache; as-of 2026-07-31 against legacy production (Vercel/Supabase/GitHub/Workspace); Azure deferred (R-016); A–F answered."
13. Mark **AUD-OBS-1** resolved (R-016 now exists).
14. *(If accepted)* add **F-29** for Google Workspace evidence (G-1).

**E. Policies — 8 files (A-6)**
15. Append the approval block (template in APPROVAL_PACKAGE) to the 8 policies lacking one
    (access-control, acceptable-use, backup, change-management, incident-response,
    secrets-management, security-awareness, vendor-management): **Approved By: Dan Ayache,
    Engineering Lead (Human Owner); Approval Date: 2026-06-08; Effective Date: <unchanged>;
    Next Review Due: 2027-05-27.** ISP already compliant — re-confirm signatory only.
16. secrets-management-policy: note v1.1 (2026-06-02) approved 2026-06-08; reference the existing
    version table (don't duplicate).
17. Optionally add the AC-3 adoption note where Effective predates Approval.

**F. Optional:** scan README for any residual "Azure primary" language; the scope doc is
authoritative, but consistency helps.

---

## 5. Determinations

| Question | Determination |
|----------|---------------|
| **Can F-01 be closed?** | **Not yet — but decision-complete.** All inputs now exist (as-of date, scope, Azure-out, signatory, date, R-016). Closure is valid **only after** updates A (signed scope, Azure rescoped, A–F filled) and C (R-016 added) are committed. Closing before the edits land = AC-1 violation. **Close it in the last commit of the A-5 chain.** |
| **Is A-5 approved?** | **Yes — approved to proceed**, with an **expanded edit scope.** The Human Owner has supplied every wording input. But A-5 implementation must now also rewrite scope-doc §5/§6 and the gap analysis to remove Azure from the boundary and add Google Workspace — broader than APPROVAL_PACKAGE originally framed. Proceed once the §4 updates are authorized. |
| **Is A-6 approved?** | **Yes — approved to proceed**, with **one caveat to resolve first:** confirm the signatory **title/identity** (Dan Ayache = Engineering Lead; use a business title, not "Human Owner," in the audit-facing block — AC-2). With that settled, the 8 approval blocks can be applied. |

---

## 6. Remaining blockers

**For A-5 / F-01 closure:**
1. Authorize the scope-doc + gap-analysis Azure rescoping edits (§4-A, §4-B).
2. Pick R-016 **scoring** (§4-C item 10) and approve adding **"Deferred"** to the Risk Register legend (§4-C item 9).
3. Reaffirm **no production data/secret resides in Azure** today (AC-4).

**For A-6:**
4. Confirm **Dan Ayache = Engineering Lead** and the **title** to print in approval blocks (AC-2).

**New, not blocking A-5/A-6 but newly opened by Decision 2:**
5. Decide whether to open **F-29 (Google Workspace access evidence)** and add Workspace to RBAC + key-inventory scope (G-1).

*(F-19 and F-14 remain the separate live remediation blockers under A-1/A-3 — outside this review.)*

---

## 7. Recommended commit order

Sequenced so no document ever references something that doesn't exist yet, and F-01 never closes
against an unsigned scope doc. **A-6 is independent and can run in parallel.**

**A-5 chain (strict order):**
1. `risk: add R-016 Azure migration risk (Deferred) + reconcile status legend` — Risk Register (§4-C). *Creates R-016 first so later references resolve.*
2. `docs: rescope SOC2 engagement to legacy production; Azure future-state; sign-off` — scope doc §5/§6 + A–F + header signature (§4-A). *The actual A-5 sign-off.*
3. `docs: reframe Azure gap analysis as future-state; answer A–F` — gap analysis (§4-B).
4. `chore: reclassify F-16 as deferred; close F-01; resolve AUD-OBS-1` — Findings Register (§4-D). **Last**, so closure follows the signed scope.

**A-6 (parallel, independent):**
5. `policy: add management approval records to 8 subordinate policies (Dan Ayache, 2026-06-08)` (§4-E).

**Optional follow-ups:**
6. `chore: open F-29 Google Workspace access evidence` (if accepted) + README Azure-language sweep.

Each commit is a single logical change with a conventional-commit prefix (satisfies the CI
`pr-validation` change-management gate). Per AGENTS.md, all of this waits for Human Owner
authorization and goes through Codex → Claude review → Owner approval → merge.

---

## 8. Risk assessment

| Risk | Likelihood | Impact | If unmanaged | Mitigation |
|------|:---------:|:------:|--------------|------------|
| Close F-01 before scope doc is signed/rescoped | Med | **High** | Recreates F-18-class evidence contradiction; undermines integrity narrative | Close F-01 in the **final** commit only (AC-1; commit order §7) |
| R-016 logged with undefined "Deferred" status | High | Med | Internal inconsistency; auditor flags risk-register hygiene | Add "Deferred" to §3.4 legend (§4-C item 9) |
| "Human Owner" printed as the approval title | Med | Med | Auditor doesn't recognize the title; weakens CC1.x management-commitment evidence | Use "Dan Ayache, Engineering Lead"; confirm identity (AC-2) |
| Azure-out-of-scope while gap analysis/openai-keys still claim Azure holds secrets | Med | **High** | If any secret truly is in Azure, the boundary is false → material scoping error | Reaffirm AC-4; fix the present-tense claims (§4-B, and A-3/F-14 for openai-keys) |
| F-16 left as open Critical "in scope" | Med | Med | Contradicts the boundary; open Critical at audit time | Reclassify F-16 to deferred (§4-D item 11) |
| Google Workspace in scope, no evidence | Med | Med | New audit gap surfaces late | Open F-29; add to RBAC + key inventory (G-1) |
| Key-person concentration (owner = committer = approver) | Existing | Med | Separation-of-duties optic for CC1.x | Tracked under F-09/F-13; F-15 compensating control; consider a second approver/Owner |
| Effective-before-approved gap unexplained | Low | Low | Minor auditor question | One-line adoption note (AC-3) |

**Net:** the decisions move F-01 from blocked to closeable and authorize A-5/A-6, **lowering**
program risk overall. The residual risk is almost entirely **sequencing and consistency** — handled
by the §7 commit order and the §4 reconciliations — plus one substantive new item (Google
Workspace evidence) that Decision 2 newly created.

---

## Approval

- [ ] Human Owner resolves the four A-5/A-6 blockers in §6 (items 1–4)
- [ ] Human Owner decides on F-29 / Google Workspace scope (§6 item 5)
- [ ] Human Owner authorizes Codex to apply §4 updates in the §7 commit order
- [ ] Claude reviews each applied commit before F-01 / AUD-OBS-1 are marked resolved

**This document changed nothing. No files, evidence, policies, configuration, or commits were
modified.** Reviewer: _______________  Date: __________
