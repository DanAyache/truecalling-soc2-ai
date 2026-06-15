# SOC 2 Engagement Scope & Framework Decision

**Organization:** TrueCalling.ai
**Owner:** Engineering Lead
**SOC 2 Program Lead:** Stéphane (stephane@truecalling.ai)
**Decision date:** [YYYY-MM-DD]
**Approved by:** [name, role]  ·  **Approval date:** [YYYY-MM-DD]
**Addresses finding:** F-01 (framework scope undecided)
**SOC 2 Criteria:** program scope (CC1.x governance)

---

## 1. Engagement type

TrueCalling.ai is pursuing a **SOC 2 Type I** examination as its first milestone:
an independent opinion on the **design and implementation** of controls **as of a single
point in time**. Operating effectiveness over a period (Type II) is **out of scope for this
examination** and is deferred to a second milestone (§4).

This decision supersedes all prior "Type II" framing in `README.md` and
`evidence/BOOTSTRAP-CHECKLIST.md`, which described a future-state goal, not the current engagement.

## 2. As-of date

**Target Type I as-of date: 2026-07-31.**

The as-of date is the point at which every in-scope control is implemented **and** evidenced.
It is contingent on completion of F-19 (2026-06-04), F-14 (2026-06-30), and F-20 (2026-06-30).
If any gate slips, the as-of date moves to the date the final in-scope control is evidenced;
it will never be set earlier than that date.

**As-of basis:** This Type I examination is assessed against the **legacy / current production
system (Vercel + Supabase + GitHub + Google Workspace)**. The Azure migration is **out of the
current Type I boundary** and deferred to the Type II milestone (control gap tracked as F-16;
risk tracked as R-016, status Deferred). Azure / F-16 is therefore **not** an as-of gating item.

## 3. Trust Services Categories in scope

| Category | In scope? | Basis |
|----------|:--------:|-------|
| Security (Common Criteria CC1–CC9) | **Yes (required)** | Baseline for any SOC 2 report |
| Availability (A1) | **Yes** | Customer-facing voice service; backup/restore + DR controls already maintained (Backup Policy, R-007) |
| Confidentiality (C1) | **Yes** | Customer call recordings, transcripts, and voice-biometric data (R-008) |
| Processing Integrity (PI1) | No | Excluded from initial examination; no PI controls designed yet |
| Privacy (P1–P8) | No | Deferred — addressed by the separate GDPR program (F-25, currently Deferred) |

## 4. Type II roadmap (deferred)

A Type II observation period (minimum [6] months) will begin **only after** the Azure
migration cut-over (§5) is complete and the Type I controls are operating stably.
Target Type II audit window: **[Q_ 20__]**. No Type II observation period is considered
to have started before that date; the 2026-05-27 "observation period initiated" note is retired.

## 5. Azure migration & system boundary

TrueCalling.ai plans to migrate its production hosting from **Vercel to Microsoft Azure**. As of
the as-of date this migration has **not started in production**: the **legacy stack
(Vercel + Supabase + GitHub + Google Workspace) is the production platform**, and production
secrets reside in the legacy platforms (Vercel / Supabase) — **not** yet in Azure Key Vault. Azure
is **future-state and out of the current Type I boundary** (deferred — F-16 / R-016) and re-enters
scope at cut-over.

Migration determinations (confirmed by the Engineering Lead — gap-analysis §2 A–F):

| # | Question | Determination |
|---|----------|---------------|
| A | Production live on Azure, still Vercel, or split? | Still Vercel — Azure has no production resources; migration not started in production. |
| B | Azure compute hosting the app (App Service / Container Apps / AKS / Functions)? | None yet — no Azure compute in production. |
| C | Database: Supabase retained, or moving to Azure PostgreSQL? | Supabase retained (production database). |
| D | Identity = Entra ID, with Conditional Access available? | No — identity is GitHub / Supabase / Vercel / Google Workspace logins today; Entra ID is future-state. |
| E | Azure region(s) (EU residency)? | N/A — no Azure resources provisioned. |
| F | Source/CI staying on GitHub or moving to Azure DevOps? | GitHub (staying). |

The audit is scoped to **the production system as it actually runs as of the as-of date** (§2) —
the legacy stack. Azure access control, secret management, logging, change management, and
backup/recovery are **out of the current Type I boundary** (Azure not in production), documented
as a gap under **F-16** and tracked as deferred risk **R-016**; they re-enter scope at cut-over.
The legacy platform controls cover all in-scope components.

## 6. System boundary

**In scope**
- **Platforms:** the legacy production stack — **Vercel** (hosting), **Supabase** (database/auth),
  **GitHub** (source/CI), and **Google Workspace** (identity/email). Azure is **out of scope**
  (future-state — see Out of scope).
- **Data:** customer call recordings, transcripts, and voice-biometric data; AI model assets;
  API credentials and secrets.
- **People:** all individuals with access to the above (current team enumerated in the
  2026-05-27 RBAC review and the API Key Inventory population).
- **Supporting services / sub-processors:** OpenAI, Anthropic, Supabase, Vercel, GitHub, Twilio,
  FullEnrich, Microsoft Azure, Google Workspace (per the reconciled vendor list — F-06).

**Out of scope**
- **Microsoft Azure** (Entra ID, Key Vault, Azure compute / Monitor / Defender) — future-state; no production resources as of the as-of date. Control gap documented under F-16; deferred risk R-016.
- Processing Integrity and Privacy TSCs (§3).
- GDPR, EU AI Act, and ISO 27001 programs (deferred — F-25/F-26/F-27).
- The vendored `claude-code-plugins-plus-skills/` tree (third-party submodule, not a TrueCalling
  control — F-21).
