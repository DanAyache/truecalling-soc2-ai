# API Key Inventory

**Owner:** Engineering Lead
**Last Updated:** 2026-06-01
**Review Cycle:** Quarterly (aligned with access reviews)
**Related Policy:** [Secrets Management Policy](../../policies/secrets-management-policy.md) §4
**Related Risk:** [R-013](../risk-register/risk-register-2026.md) (Twilio credential compromise — extends to all provider credentials in scope)
**SOC 2 Criteria:** CC6.1, CC6.7

---

## Purpose

This log is the authoritative record of all named API keys, tokens, and secrets issued to individuals or systems on behalf of TrueCalling.ai. It is updated at every provisioning and revocation event and reviewed at each quarterly access review.

Keys are never stored here — only metadata (name, owner, dates, purpose, status).

---

## 1. Inventory Completeness Statement

This inventory is currently **incomplete**. No provider has been enumerated yet. The Engineering Lead is responsible for full enumeration of all providers by **2026-06-30**, after which this statement will be replaced with a completion attestation.

This file is committed in its current state to:

- Transparently document the open inventory gap rather than represent the control as executed
- Enumerate every provider in scope so none is overlooked at completion time
- Provide auditable evidence of the planned completion path

Progress is tracked in the per-provider table below and in Risk Register entry [R-013](../risk-register/risk-register-2026.md).

### Per-Provider Status

| Provider | Status | Owner Responsible | Target Completion |
|----------|--------|------------------|-------------------|
| OpenAI | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Anthropic | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Supabase | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Vercel | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| GitHub | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Twilio (covers voice / SMS / WhatsApp) | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| FullEnrich | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |

A provider's status changes to ✅ Complete only after its keys have been verified at the provider's UI and the corresponding rows added to §2 Active Keys.

---

## 2. Active Keys

This table will be populated as each provider's inventory is verified. Until then, no rows are recorded here. Per-provider enumeration sources are listed in §3 below.

| Key Name | Service | Owner | Date Created | Rotation Due | Purpose | Status |
|----------|---------|-------|-------------|-------------|---------|--------|
| *(no rows populated yet — inventory in progress; targeted completion 2026-06-30)* | | | | | | |

---

## 3. Per-Provider Enumeration Plan

The following providers are confirmed to have at least one active credential issued for TrueCalling.ai. None has been inventoried yet. Each will be enumerated by 2026-06-30; the Engineering Lead is responsible for completion of every entry.

### 3.1 OpenAI

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** platform.openai.com/api-keys
- **Scope:** All active API keys, including any service account keys; note project assignment where applicable
- **Expected fields per key:** name, owner, created, last rotated, purpose, status

### 3.2 Anthropic

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** console.anthropic.com → Settings → API Keys
- **Scope:** Active API keys used for Claude model access

### 3.3 Supabase

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Sources for enumeration:** (a) Project → Settings → API (anon key, service_role key, JWT secret); (b) Account → Access Tokens (personal access tokens)
- **Scope:** Production and staging project credentials; any personal access tokens with project access
- **Note:** Architectural keys (anon, service_role, JWT secret) are auto-generated per project and exist by definition; they will be inventoried with `created: unknown` if creation date is not visible in the dashboard

### 3.4 Vercel

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** vercel.com/account/tokens
- **Scope:** Account-level tokens; team-level tokens if any; webhook secrets

### 3.5 GitHub

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Sources for enumeration:** (a) github.com/settings/tokens — Personal Access Tokens (Classic and Fine-grained); (b) Repo → Settings → Secrets and variables → Actions (repository-scoped secrets including any third-party integration tokens, license keys, or deployment credentials)
- **Scope:** Per-person PATs for individual contributors; repository Actions secrets

### 3.6 Twilio

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** console.twilio.com → Account → API keys & tokens (and main Account Auth Token under Account Info)
- **Scope:** Account SID + Auth Token (treated as a single credential pair); any separately-issued API Keys; WhatsApp Business API credentials are managed under this same Twilio account
- **Related risk:** [R-013](../risk-register/risk-register-2026.md) — credential compromise risk explicitly tracked

### 3.7 FullEnrich

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** app.fullenrich.com → Settings → API
- **Scope:** Active API keys for contact data enrichment

---

## 4. Revoked Keys

| Key Name | Service | Owner | Date Created | Date Revoked | Reason |
|----------|---------|-------|-------------|-------------|--------|
| *(none recorded)* | | | | | |

---

## 5. Naming Convention

| Key Type | Format | Example |
|----------|--------|---------|
| Per-person OpenAI key | `<name>-<role>-<YYYY-MM>` | `alice-engineer-2026-05` |
| Per-person Anthropic key | `<name>-<role>-<YYYY-MM>` | `stephane-lead-2026-05` |
| GitHub PAT | `<name>-<purpose>-<YYYY-MM>` | `stephane-deployments-2026-05` |
| Supabase service key | `supabase-service-role-<env>` | `supabase-service-role-prod` |
| Webhook secret | `<service>-webhook-<env>` | `stripe-webhook-prod` |

---

## 6. How to Add a Key

1. Create the key in the provider platform using the correct naming convention above
2. Add a row to **Active Keys** (§2) immediately — before distributing the key
3. Set rotation due 90 days from creation (180 days for webhook secrets)
4. Commit this file with message: `chore: add api key <key-name> to inventory`

## 7. How to Revoke a Key

1. Revoke the key in the provider platform
2. Move the row from Active Keys to **Revoked Keys** (§4)
3. Add `Date Revoked` and `Reason`
4. If triggered by offboarding, link the offboarding issue in the Reason column
5. Commit this file with message: `chore: revoke api key <key-name>`

## 8. Quarterly Review

At each quarterly access review, the Engineering Lead:

1. Pulls the current key list from each provider (OpenAI, Anthropic, Supabase, Vercel, GitHub, Twilio, FullEnrich)
2. Verifies every active key in this log appears in the provider and vice versa
3. Flags keys past their rotation due date — treat as P2 incident
4. Documents the review with a commit: `chore: quarterly api key review <YYYY-MM-DD>`

## 9. Completion Tracking

Until full enumeration is complete (target: 2026-06-30):

1. The **Per-Provider Status** table in §1 is updated as each provider transitions from "Inventory not yet completed" → ✅ Complete
2. As each provider is fully enumerated, its rows are added to §2 Active Keys
3. Once all providers reach ✅ Complete, §1 is replaced with a completion attestation noting the date and the total number of keys inventoried
4. Until then, this file remains an evidence gap acknowledgement, not a completed control

---

*File Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
