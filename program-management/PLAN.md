# PLAN.md — Repository Analysis & Recommended Actions

**Author:** Claude (Architecture / Security / SOC 2 review role per [AGENTS.md](AGENTS.md))
**Date:** 2026-06-08
**Status:** DRAFT — awaiting Human Owner approval (AGENTS.md Workflow Step 2)
**Scope of this document:** Read-only analysis. No code, controls, or evidence were modified.

> Per AGENTS.md, this is a Step-1 PLAN. Nothing in §6 is to be executed until the Human
> Owner approves. Protected areas (Authentication, MFA, API Keys, Secrets, SOC 2 evidence
> repository, etc.) require explicit Human Owner sign-off before any change.

---

## 1. Repository Structure

This workspace (`SOC2/`) is a **git repository that contains two nested git repositories**
(committed as gitlinks, mode `160000`) plus a set of working artifacts at the root.

```
SOC2/                                     ← top-level repo (1 commit: "Add GitHub issue templates")
├── AGENTS.md                             ← multi-agent governance policy (authoritative roles)
├── PLAN.md                               ← this document
├── PROJECT-STATE-2026-06-03.md           ← checkpoint (commit a8ff065)
├── PROJECT-STATE-2026-06-03-END-OF-SESSION.md  ← latest checkpoint (commit d6fb016), readiness 57/100
├── Conformité TrueCalling — review-03-06-26.pdf ← external review (1.3 MB)
├── Project Notes/                        ← working notes
├── TrueCalling_F19_MFA_Execution_Package.xlsx
├── TrueCalling_SOC2_API_Key_Inventory_Request.xlsx
├── *.png.jpeg / export-*.csv             ← loose RBAC evidence at root (duplicated inside soc2 repo)
├── .vscode/                              ← editor settings (untracked)
│
├── truecalling-soc2-ai/        [gitlink] ← PRIMARY compliance repo (policies + evidence)
│   ├── README.md, SECURITY.md, SOC2-ENGAGEMENT-SCOPE.md
│   ├── policies/        (9 policy .md files, effective 2026-05-27)
│   ├── procedures/      (onboarding, offboarding, evidence collection, DR/BC)
│   ├── findings/        findings-remediation-register.md  ← single source of truth for findings
│   ├── evidence/        (access-control, api-keys, azure, backups, incidents, risk-register, …)
│   ├── templates/       (compliance calendar, access review, issue templates, backup log)
│   ├── .github/         (workflows/security.yml, CODEOWNERS, dependabot.yml, PR template)
│   ├── skills/ + claude-skills-soc2-policies/   ← Claude skill definitions
│   └── claude-code-plugins-plus-skills/  ← 14,857-file vendored third-party tree (F-21)
│
├── truecalling-incidents/      [gitlink] ← incident/onboarding/offboarding issue templates
└── truecalling-incidents-setup/          ← untracked staging copy of the same templates
```

**Structural notes**
- The two nested repos are committed as **gitlinks but there is no `.gitmodules`** — they are
  not registered submodules. Cloning the top-level repo will not fetch them.
- `truecalling-incidents/` and `truecalling-incidents-setup/` are **near-duplicates** (the
  `-setup` copy is untracked).
- Several root-level files (GitHub/Supabase/Vercel member screenshots, GitHub export CSV) are
  **duplicated** inside `truecalling-soc2-ai/evidence/access-control/`.

---

## 2. Current Architecture

This is a **compliance / governance repository, not an application codebase** — there is no
build, no runtime, no application source. "Architecture" here means the control and evidence
architecture plus the one piece of automation.

**Governance model (AGENTS.md):** three actors — **Claude** (architecture, security, SOC 2,
review; may reject, may not approve/deploy), **Codex** (implementation; may not approve own
work or ignore Claude security findings), **Human Owner** (final authority; sole approver of
merges to main, auth/MFA/billing/DB/secret changes). Workflow is Plan → Owner approval →
implement → review → Owner approval → merge.

**Compliance program (`truecalling-soc2-ai`):**
- **Engagement:** SOC 2 **Type I** (design + implementation, point-in-time). Target as-of date
  **2026-07-31**, contingent on F-19 / F-14 / F-20 / F-16 closure. Type II deferred to after
  the Azure cut-over.
- **TSCs in scope:** Security (CC1–CC9), Availability (A1), Confidentiality (C1). Processing
  Integrity and Privacy excluded. GDPR / EU AI Act / ISO 27001 deferred.
- **Control documentation:** 9 policies + 4 procedures, all mapped to TSC criteria, effective
  2026-05-27.
- **Evidence model:** each TSC maps to a folder under `evidence/`; git commit timestamps are
  the authoritative collection date. The **Findings & Remediation Register** is the single
  source of truth for findings; the **Risk Register** (R-001…R-015) tracks risks quarterly.

**Production system under audit (per SOC2-ENGAGEMENT-SCOPE.md & project state):**
- Live stack: **Vercel (hosting) + Supabase (DB/auth, RLS, daily backups + PITR) + GitHub
  (source/CI) + Google Workspace**. Sub-processors: OpenAI, Anthropic, Twilio, FullEnrich.
- **Azure is future-state only** (user-confirmed: no production resources or secrets in Azure
  yet). The Azure control set (F-16) is documented as a gap but is **not** a blocker for a
  Type I opinion scoped to the legacy production system.

**Automation (`.github/workflows/security.yml`) — the only executable component:**
- 5 jobs on push/PR to main+master and nightly 02:00 UTC: **Gitleaks** (CC6.7), **npm audit**
  (CC7.1, fail on high/critical), **TruffleHog** `--only-verified` (CC6.7), **PR validation**
  (CC8.1 — conventional-commit title, description ≥20 chars, security-label automation),
  **security summary** artifact (365-day retention).
- All third-party actions are **pinned to commit SHAs** — good supply-chain hygiene.
- Supporting controls: `CODEOWNERS` (mandatory review), `dependabot.yml`, branch protection
  (asserted; screenshot evidence target 2026-06-30).

---

## 3. Security Observations

Observations about the **security posture of the program and its automation**. Severity uses
the project's own register vocabulary.

| # | Observation | Severity | Notes |
|---|-------------|----------|-------|
| S-1 | **Supabase MFA disabled** for both production-data accounts (Yarone, Patrick); team view confirms Disabled. Live exposure of customer call/voice-biometric data behind single-factor auth. | **Critical** | F-19 / R-002. Cannot be closed: Patrick has no enrollment evidence; team view disproves closure. |
| S-2 | **API Key Inventory empty** (0 rows in §2) — no enumeration of the 5 mandatory providers' credentials, owners, or rotation state. No basis to detect a stale/over-scoped key. | **Critical** | F-14 / R-013. |
| S-3 | **Evidence-hygiene breach:** a committed/untracked screenshot (`supabase-api-settings-2026-06.png`) **exposes the Supabase anon JWT value**, violating the inventory's metadata-only rule. | **High** | Must be re-captured redacted before any commit; purge from history if it lands. |
| S-4 | **Backup restore never tested** — Availability (A1.2) restore capability unverified; RPO 24h / RTO 4h are claims, not evidence. | **High** | F-20 / R-007. |
| S-5 | **Key-person & governance concentration** — single GitHub Owner (no break-glass) and the repo lives under a **personal GitHub account** (`DanAyache`), not an org. | **High** | F-09 / F-13 / R-001 / R-004. An auditor typically treats this as a significant deficiency. |
| S-6 | **Developer-local `.claude/settings.local.json` committed** — leaks local paths and a permissive allowlist (`git add *`, `git commit *`, broad Bash). Not a secret, but should never be in a compliance repo. | **Low** | F-22. |
| S-7 | **14,857-file vendored third-party tree** (`claude-code-plugins-plus-skills/`) inside the compliance repo dramatically expands the attack/secret-scan surface and dilutes change-management signal. | **Medium** | F-21. Explicitly out of audit scope per scope doc, but still co-resident. |
| S-8 | **Prompt-injection & multi-provider-outage risks** for the AI voice agent are registered but unmitigated (no automated injection test suite, no provider fallback). | **Medium** | R-009 / R-010. Product-side, not in this repo. |
| S-9 | **Personal-email Supabase accounts** — offboarding may leave residual access if personal email is the auth factor. | **Medium** | F-05 / R-003. |

**Positives:** secret scanning is layered (Gitleaks + TruffleHog) and gated in CI; actions are
SHA-pinned; CODEOWNERS + branch protection + conventional-commit enforcement give a credible
change-management trail; the `.gitignore` mandates secret patterns per policy.

---

## 4. SOC 2 Readiness Observations

**Current self-assessed readiness: 57 / 100** (Type I model — Design 93%, Implementation 34%).
The gap is the classic Type-I shape: **design is largely complete; implementation/evidence is
thin.** Policies and procedures exist and map to criteria, but few controls have collected
operating evidence.

**Type I blockers (MUST) — from the Findings Register & latest checkpoint:**

| ID | Finding | Criteria | Status / % |
|----|---------|----------|------------|
| F-01 | Engagement scope doc unsigned; Azure A–F unanswered; R-016 not yet in register | program scope | In Progress ~80% |
| F-14 | API Key Inventory empty (0 of ~5 mandatory providers) | CC6.1, CC6.7 | In Progress ~25% |
| F-19 | Supabase MFA disabled / unevidenced | CC6.1 | Evidence Pending ~50% |
| F-20 | Backup restore unverified | A1.2 | Open ~10% |
| F-16 | Azure control coverage gap (future-state; blocker only if Type I waits for cut-over) | CC6/7/8, A1.2 | In Progress ~15% |

**Readiness strengths**
- Findings Register reconciles previously inconsistent IDs (F-17 closed) and is the single
  source of truth; evidence-integrity contradictions were caught and corrected honestly
  (F-18, F-24, F-28 closed rather than papered over).
- Scope document cleanly states Type I, TSCs, system boundary, and Azure dual-running treatment.

**Readiness gaps an auditor will probe**
- **Empty evidence folders** still referenced (onboarding/offboarding `.gitkeep` only — F-07;
  npm-audit/TruffleHog outputs not committed). README now flags these honestly.
- **Policy approval records** missing — 8 subordinate policies + scope doc lack signed
  approver/date (AUD-OBS-6); the scope doc still has `[YYYY-MM-DD]` / `[name, role]` placeholders.
- **Vendor due diligence** (SOC 2 reports / DPAs) not collected for any sub-processor (F-06).
- **Data classification & retention/erasure schedule** absent (F-23) — material for
  Confidentiality (C1) given voice-biometric data.
- **As-of-date ambiguity:** whether 2026-07-31 targets the legacy system (Azure deferred) or
  waits for cut-over is unconfirmed — this single decision determines whether F-16 is a blocker.

---

## 5. Technical Debt

| # | Item | Impact |
|---|------|--------|
| TD-1 | Nested repos are **gitlinks with no `.gitmodules`** — top-level clone won't fetch `truecalling-soc2-ai` or `truecalling-incidents`; contributors get an incomplete tree. | High — reproducibility |
| TD-2 | **14,857-file vendored tree** (`claude-code-plugins-plus-skills/`) committed into the compliance repo (F-21). Should be a submodule or removed. | Medium |
| TD-3 | **Duplicated evidence/artifacts** — RBAC member screenshots + GitHub CSV exist both at workspace root and inside `truecalling-soc2-ai/evidence/`; `truecalling-incidents` vs `truecalling-incidents-setup` are near-duplicates. Two sources of truth. | Medium |
| TD-4 | **No top-level `.gitignore`** — `.vscode/`, `.xlsx`, loose screenshots accumulate untracked at root; risk of committing local/editor cruft. | Low |
| TD-5 | **`.claude/settings.local.json` committed** (F-22) — local, machine-specific config in a shared repo. | Low |
| TD-6 | **Document drift** — `openai-keys.md` scope line asserts secrets "now reside in Azure Key Vault" (present tense) which is factually wrong (Azure is future-state); R-013 references stale "F-04" alias; F-14 "8 providers" vs ~10 enumerated (AUD-OBS-3 / AUD-OBS-5). | Low |
| TD-7 | **Readiness model doc defect** — ΣW documented as "46" but actual is 47 (noted in checkpoint). | Low |
| TD-8 | **Stray scheduled-tasks artifacts** in the vendored tree (`.claude/scheduled_tasks.lock`) committed — noise. | Low |

---

## 6. Recommended Next Actions

Ordered to match the program's own critical path. **None to be executed until the Human Owner
approves this plan;** items touching MFA, API Keys, Secrets, and the SOC 2 evidence repository
are Protected Areas requiring explicit Owner sign-off. Implementation, once approved, is
Codex's responsibility; Claude reviews.

| # | Action | Addresses | Priority | Est. Effort |
|---|--------|-----------|----------|-------------|
| A-1 | **Close F-19 (Supabase MFA):** confirm Patrick + Yarone TOTP enrollment; re-capture team view showing **both Enabled** (emails visible); §4 sign-off; commit; R-002 → Mitigated. *If team view still shows Disabled, MFA is not active — stop and remediate, do not document around it.* | S-1, F-19 | **High** | S (1–2 days, gated on member action) |
| A-2 | **Remediate the exposed Supabase anon-key screenshot** — do not commit the value-exposing capture; re-capture redacted; if already in history, purge and rotate the key. | S-3 | **High** | S (<1 day) |
| A-3 | **Populate F-14 API Key Inventory** — enumerate the 5 mandatory providers (Supabase, GitHub, OpenAI, Anthropic, Vercel) metadata-only; decide Twilio/FullEnrich active vs N/A; mark Azure rows N/A; redacted screenshots; dated attestation + key count; fix R-013 alias + count. | S-2, F-14, TD-6 | **High** | M (2–4 days, gated on portal access) |
| A-4 | **Run + log the backup restore test** to staging (integrity, RPO/RTO); replace the gap log; R-007 → Mitigated. | S-4, F-20 | **High** | M (1–3 days, gated on Supabase access) |
| A-5 | **Close F-01 scope decision:** fill approver + date on scope doc; answer Azure A–F (or record "Azure not started; legacy is production"); add **R-016** to the Risk Register; confirm whether the 2026-07-31 as-of targets the legacy system. | F-01, readiness as-of-date ambiguity | **High** | S (<1 day, Owner decision) |
| A-6 | **Sign policy approval records** — approver + date on the 8 subordinate policies and the scope doc (AUD-OBS-6). | §4 readiness gap | **High** | S (Owner action) |
| A-7 | **Governance concentration:** add a second GitHub Owner + documented break-glass account (F-09); produce the org-migration-vs-compensating-control decision for the personal-account repo (F-13). | S-5, F-09, F-13 | **High** | M |
| A-8 | **Collect vendor due diligence** (SOC 2 / DPA) for all sub-processors incl. Microsoft Azure; reconcile the 4 divergent vendor lists (F-06). | §4 readiness gap | **Medium** | M |
| A-9 | **Access-lifecycle evidence (F-07):** incumbent attestations + 1 executed onboarding + 1 offboarding package (gated behind A-1). | §4 readiness gap | **Medium** | M |
| A-10 | **Data classification & retention/erasure schedule** (F-23) — material for Confidentiality given voice-biometric data. | S (C1 gap) | **Medium** | M |
| A-11 | **Repo hygiene:** register the nested repos as proper submodules **or** document the intended topology (TD-1); convert the vendored tree to a submodule / remove it (F-21/TD-2); de-duplicate root vs evidence artifacts (TD-3); add a top-level `.gitignore` (TD-4); remove `settings.local.json` (F-22). | TD-1…TD-5, F-21, F-22 | **Medium** | M |
| A-12 | **Commit recurring scan evidence** — export npm-audit / TruffleHog outputs into `evidence/security-scans/` per the monthly calendar; fix the ΣW=46→47 model defect (TD-7). | CC7.1 evidence, TD-7 | **Low** | S |
| A-13 | **Document drift cleanup** — reword the "now reside in Azure Key Vault" overstatement; close AUD-OBS-3 / AUD-OBS-5 during F-14. | TD-6 | **Low** | S |
| A-14 | **Personal-email Supabase exception (F-05)** — document compensating control (MFA + RBAC + 4h deprovision SLA) or migrate to company-domain accounts. | S-9, F-05 | **Low** | S |
| A-15 | **Future Azure milestone (F-16):** stand up + screenshot the Azure security baseline only when cut-over begins; not required for a legacy-system Type I opinion. | S-7 (scope), F-16 | **Low** (now) | L (at cut-over) |

---

## 7. Priority Summary

- **High (Type I blockers / live exposure):** A-1, A-2, A-3, A-4, A-5, A-6, A-7
  → MFA closure, exposed-key remediation, key inventory, restore test, scope sign-off,
  policy approvals, governance concentration.
- **Medium (readiness depth / hygiene):** A-8, A-9, A-10, A-11
  → vendor due diligence, lifecycle evidence, data classification, repo structure.
- **Low (cleanup / deferred):** A-12, A-13, A-14, A-15
  → recurring scan evidence, doc drift, email exception, Azure (future milestone).

## 8. Estimated Effort (legend)

| Size | Meaning |
|------|---------|
| **S** | ≤ ~1 day of focused work (often gated on a person enabling MFA or granting portal access, not on hours) |
| **M** | ~2–4 days |
| **L** | Multi-week / milestone-sized (e.g., Azure baseline build-out) |

**Critical-path estimate to a defensible Type I as-of date (legacy system, Azure deferred):**
roughly **2–3 focused weeks**, the long pole being evidence collection that depends on team
members (MFA enrollment) and portal access (key inventory, restore test) rather than on
authoring effort. This aligns with the program's own 2026-07-31 target **if** the as-of date is
scoped to the legacy production system and Azure (F-16) is confirmed deferred (A-5).

---

## Approval

Per AGENTS.md Workflow:

- [ ] **Human Owner** reviewed and approves this PLAN.md
- [ ] Approved actions assigned to Codex for implementation
- [ ] Claude performs code/evidence review post-implementation
- [ ] Human Owner approves final changes before merge

**No modifications to controls, evidence, or configuration will be made until the box above is
checked.** Reviewer: _______________  Date: __________
