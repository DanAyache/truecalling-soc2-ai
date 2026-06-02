# API Key Inventory — Collection Worksheet (F-14)

**Purpose:** Working sheet the Engineering Lead fills in while enumerating each provider, before transcribing finalized rows into [§2 Active Keys](openai-keys.md#2-active-keys) of the inventory.
**Owner:** Engineering Lead
**Created:** 2026-06-02
**Target completion:** 2026-06-30
**Finding:** F-14 · **SOC 2:** CC6.1, CC6.7
**Source request:** [api-key-inventory-request.xlsx](api-key-inventory-request.xlsx)

> **Rule:** Record **metadata only** — name, owner, dates, scope, status. **Never** paste a secret value. Take screenshots with values hidden/redacted and save under `evidence/api-keys/screenshots/`.

---

## Mapping: request workbook → where each item lands

| Workbook item | Lands in | Screenshot target |
|---|---|---|
| OpenAI — API keys list | §2 + §3.1 | `screenshots/openai-keys-YYYY-MM-DD.png` |
| Azure Key Vault — secrets list | §2 + §3.8 | `screenshots/azure-keyvault-secrets-YYYY-MM-DD.png` |
| Azure RBAC — users and roles | §2 + §3.9 + RBAC review | `screenshots/azure-rbac-YYYY-MM-DD.png` |
| Azure Resources — resource group / hosting | Azure Gap Analysis | `screenshots/azure-resources-YYYY-MM-DD.png` |
| Supabase API (Settings → API) | §2 + §3.3 | `screenshots/supabase-api-YYYY-MM-DD.png` |
| Supabase Auth settings | Access Control evidence | `screenshots/supabase-auth-YYYY-MM-DD.png` |
| Supabase Team members/roles | RBAC review | already collected 2026-05-27 |
| Supabase Secrets config | §2 + §3.3 | `screenshots/supabase-secrets-YYYY-MM-DD.png` |
| Vercel Project overview | Azure Gap Analysis (migration source) | `screenshots/vercel-project-YYYY-MM-DD.png` |
| Vercel Env Vars | §2 + §3.4 | `screenshots/vercel-envvars-YYYY-MM-DD.png` |
| Vercel Team members/roles | RBAC review | already collected 2026-05-27 |
| GitHub / Azure DevOps — admins/devs/roles | §2 + §3.5 / §3.10 + RBAC review | `screenshots/github-access-YYYY-MM-DD.png` |
| MFA evidence | `evidence/access-control/mfa-enforcement/` | per platform |
| Owners (OpenAI/Azure/Supabase/Vercel) | "Owner" column below | n/a (no screenshot) |
| Secret rotation process/frequency | Secrets Mgmt Policy §5 (verify matches reality) | n/a |

---

## Working table (fill, then copy finalized rows to §2 of the inventory)

| Key / Secret Name | Service / Component | Owner | Created | Rotation Due / Expiry | Purpose | Status | Source verified at |
|---|---|---|---|---|---|---|---|
| *(fill during enumeration — metadata only)* | | | | | | | |

---

## Per-component checklist

- [ ] OpenAI — `platform.openai.com/api-keys`
- [ ] Anthropic — `console.anthropic.com → Settings → API Keys`
- [ ] Supabase — Project → Settings → API (anon, service_role, JWT secret) + Account → Access Tokens
- [ ] Vercel — `vercel.com/account/tokens` + project env vars
- [ ] GitHub — `github.com/settings/tokens` (PATs) + repo Actions secrets
- [ ] Twilio — `console.twilio.com → Account → API keys & tokens` (+ Account SID/Auth Token)
- [ ] FullEnrich — `app.fullenrich.com → Settings → API`
- [ ] **Azure Key Vault** — Portal → Key Vaults → each vault → Objects (Secrets/Keys/Certificates); confirm soft-delete + purge protection
- [ ] **Azure RBAC / Entra ID** — App registrations (SP secrets + expiry), Managed Identities, IAM role assignments
- [ ] **Azure DevOps** — PATs / service connections / variable groups *(or mark N/A)*

When every box is checked and §2 is populated, replace the §1 "incomplete" statement in the inventory with a dated completion attestation and total key count, then close **F-14** in the [Findings & Remediation Register](../../findings/findings-remediation-register.md).
