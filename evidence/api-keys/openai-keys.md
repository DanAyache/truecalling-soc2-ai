# API Key Inventory

**Owner:** Engineering Lead
**Last Updated:** 2026-06-02
**Review Cycle:** Quarterly (aligned with access reviews)
**Finding ID:** F-14 (API Key Inventory completion) — see [Findings & Remediation Register](../../findings/findings-remediation-register.md). *Note: previously referenced as "F-04" in Risk Register R-013; consolidated under F-14.*
**Related Policy:** [Secrets Management Policy](../../policies/secrets-management-policy.md) §4
**Related Risk:** [R-013](../risk-register/risk-register-2026.md) (Twilio credential compromise — extends to all provider credentials in scope)
**Related Work-Paper:** [api-key-inventory-request.xlsx](api-key-inventory-request.xlsx) (client enumeration request) · [api-key-inventory-worksheet.md](api-key-inventory-worksheet.md) (collection worksheet)
**SOC 2 Criteria:** CC6.1, CC6.7

> **Scope correction (2026-06-02):** Production is migrating to **Microsoft Azure** (primary hosting platform). This inventory's scope is therefore **expanded** to include **Azure Key Vault secrets**, **Azure RBAC / Entra ID identities** (service principals, managed identities, user-assigned identities), and any **Azure DevOps** PATs/service connections, in addition to the original seven providers. See the [Azure Control Coverage Gap Analysis](../azure/azure-control-coverage-gap-analysis.md). An inventory that omitted Azure would be incomplete by definition, since the project's secrets now reside in Azure Key Vault.

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

| Provider / Component | Status | Owner Responsible | Target Completion |
|----------|--------|------------------|-------------------|
| OpenAI | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Anthropic | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Supabase | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Vercel | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| GitHub | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| Twilio (covers voice / SMS / WhatsApp) | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| FullEnrich | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| **Azure Key Vault** (secrets/keys/certs) | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| **Azure RBAC / Entra ID identities** (service principals, managed identities) | ⏳ Inventory not yet completed | Engineering Lead | 2026-06-30 |
| **Azure DevOps** (PATs, service connections) — *if used for source/CI* | ⏳ Scope to confirm | Engineering Lead | 2026-06-30 |

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

### 3.8 Azure Key Vault

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Source for enumeration:** Azure Portal → Key Vaults → *(each vault)* → **Objects → Secrets / Keys / Certificates** (also `az keyvault secret list --vault-name <vault>`)
- **Scope:** Every secret, key, and certificate object across **all** vaults in **all** in-scope subscriptions/resource groups (production and staging). Record object name, vault, enabled state, created/updated, expiry, and the rotation policy if set. **Values are never recorded — metadata only.**
- **Notes:** Confirm **soft-delete** and **purge protection** are enabled on each vault (captured in the [Azure Gap Analysis](../azure/azure-control-coverage-gap-analysis.md), CC6.7). Each object's **expiration date** doubles as its rotation-due date for §8 review.

### 3.9 Azure RBAC / Entra ID Identities

- **Status:** Inventory not yet completed
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Sources for enumeration:** (a) Entra ID → **App registrations** (service principals + their client secrets/certificates and expiry); (b) Entra ID → **Managed Identities** (system- and user-assigned); (c) Subscription/Resource Group → **Access control (IAM) → Role assignments** (who/what holds which role)
- **Scope:** All non-human credentials that can reach Azure resources — service principal secrets/certs (with expiry), federated credentials (OIDC), and the role assignments granting them access. These are credentials in the SOC 2 CC6.1/CC6.7 sense even though they are not "API keys" in the SaaS sense.
- **Notes:** Service principal client secrets have expiry dates — capture them so expiring credentials surface in the §8 quarterly review. Prefer **federated (OIDC) credentials or managed identities over long-lived client secrets** (recommendation tracked in the Azure Gap Analysis).

### 3.10 Azure DevOps *(confirm applicability)*

- **Status:** Scope to confirm — the request workbook lists "GitHub / Azure DevOps." Confirm whether Azure DevOps is used for source/CI; if not, mark **N/A** with a one-line note and close this row.
- **Owner responsible:** Engineering Lead
- **Target completion:** 2026-06-30
- **Sources for enumeration:** Azure DevOps → **User settings → Personal access tokens**; Project → **Service connections**; Pipelines → **Library → secret variables/variable groups**
- **Scope:** Active PATs (with scope + expiry), service connections, and pipeline secret variables.

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

1. Pulls the current key list from each provider (OpenAI, Anthropic, Supabase, Vercel, GitHub, Twilio, FullEnrich, **Azure Key Vault, Azure RBAC/Entra ID identities**, and **Azure DevOps** if in use)
2. Verifies every active key in this log appears in the provider and vice versa
3. Flags keys past their rotation due date — treat as P2 incident
4. Documents the review with a commit: `chore: quarterly api key review <YYYY-MM-DD>`

## 9. Completion Tracking

Until full enumeration is complete (target: 2026-06-30):

1. The **Per-Provider Status** table in §1 is updated as each provider transitions from "Inventory not yet completed" → ✅ Complete
2. As each provider is fully enumerated, its rows are added to §2 Active Keys
3. Once all providers reach ✅ Complete, §1 is replaced with a completion attestation noting the date and the total number of keys inventoried
4. Until then, this file remains an evidence gap acknowledgement, not a completed control

### 9.1 Remaining Steps to Close F-14 (owner action — requires provider/portal access)

This document is now **fully scoped and structured**; the remaining work is data collection that only the Engineering Lead can perform (it requires authenticated access to each provider's UI). No key data has been fabricated.

| # | Step | Where it lands | Done? |
|---|------|----------------|-------|
| 1 | Enumerate each provider/component in §3 (incl. Azure Key Vault, Entra ID identities) using the documented sources | §2 Active Keys + worksheet | ☐ |
| 2 | For each key/secret record: name, owner, created, rotation-due (or expiry), purpose, status — **never the value** | §2 | ☐ |
| 3 | Capture the screenshots listed in the [request workbook](api-key-inventory-request.xlsx) (values hidden) | `evidence/api-keys/screenshots/` | ☐ |
| 4 | Confirm or mark **N/A** the Azure DevOps row (§3.10) | §1 table | ☐ |
| 5 | Reconcile the 2026-05-27 RBAC review's API-key attestation against the populated inventory | [RBAC review correction](../access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md) | ☐ |
| 6 | Flip each §1 row to ✅ Complete; replace the §1 "incomplete" statement with a dated completion attestation + total key count | §1 | ☐ |
| 7 | Update Risk Register R-013 to reference **F-14** (not F-04) and set status accordingly | [risk-register-2026.md](../risk-register/risk-register-2026.md) | ☐ |
| 8 | Commit: `chore: complete F-14 api key inventory <YYYY-MM-DD>` | git history | ☐ |

**Status as of 2026-06-02:** F-14 is **In Progress — scoped & structured, awaiting enumeration.** Target completion **2026-06-30**.

---

*File Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
