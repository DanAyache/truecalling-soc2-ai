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
It is contingent on completion of F-19 (2026-06-04), F-14 (2026-06-30), F-20 (2026-06-30),
and F-16 (2026-07-15). If any gate slips, the as-of date moves to the date the final in-scope
control is evidenced; it will never be set earlier than that date.

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

TrueCalling.ai is migrating its production hosting from **Vercel to Microsoft Azure**.
Azure is the **primary production platform**, and production secrets reside in **Azure Key Vault**.
Until cut-over is complete, both the Azure and legacy (Vercel / Supabase / GitHub) environments
are treated as **in scope (dual-running period)**.

Migration determinations (confirmed by the Engineering Lead — gap-analysis §2 A–F):

| # | Question | Determination |
|---|----------|---------------|
| A | Production live on Azure, still Vercel, or split? | [answer] |
| B | Azure compute hosting the app (App Service / Container Apps / AKS / Functions)? | [answer] |
| C | Database: Supabase retained, or moving to Azure PostgreSQL? | [answer] |
| D | Identity = Entra ID, with Conditional Access available? | [answer] |
| E | Azure region(s) (EU residency)? | [answer] |
| F | Source/CI staying on GitHub or moving to Azure DevOps? | [answer] |

The audit is scoped to **the production system as it actually runs as of the as-of date** (§2).
Azure access control, secret management, logging, change management, and backup/recovery are
in scope and tracked under F-16; the legacy platform controls remain in scope for any component
not yet migrated at the as-of date.

## 6. System boundary

**In scope**
- **Platforms:** Azure (Entra ID, Key Vault, Azure compute per §5-B, Azure Monitor/Defender) as
  primary; legacy Vercel, Supabase, and GitHub for any component not yet migrated.
- **Data:** customer call recordings, transcripts, and voice-biometric data; AI model assets;
  API credentials and secrets.
- **People:** all individuals with access to the above (current team enumerated in the
  2026-05-27 RBAC review and the API Key Inventory population).
- **Supporting services / sub-processors:** OpenAI, Anthropic, Supabase, Vercel, GitHub, Twilio,
  FullEnrich, Microsoft Azure, Google Workspace (per the reconciled vendor list — F-06).

**Out of scope**
- Processing Integrity and Privacy TSCs (§3).
- GDPR, EU AI Act, and ISO 27001 programs (deferred — F-25/F-26/F-27).
- The vendored `claude-code-plugins-plus-skills/` tree (third-party submodule, not a TrueCalling
  control — F-21).
