# API Key Inventory

**Owner:** Engineering Lead  
**Last Updated:** 2026-05-27  
**Review Cycle:** Quarterly (aligned with access reviews)  
**Related Policy:** Secrets Management Policy §4  
**SOC 2 Criteria:** CC6.1, CC6.7

---

## Purpose

This log is the authoritative record of all named API keys issued to individuals or systems. It is updated at every provisioning and revocation event and reviewed at each quarterly access review.

Keys are never stored here — only metadata (name, owner, dates, purpose, status).

---

## Active Keys

| Key Name | Service | Owner | Date Created | Rotation Due | Purpose | Status |
|----------|---------|-------|-------------|-------------|---------|--------|
| *(no keys provisioned yet — add a row when first key is created)* | | | | | | |

---

## Revoked Keys

| Key Name | Service | Owner | Date Created | Date Revoked | Reason |
|----------|---------|-------|-------------|-------------|--------|
| *(none)* | | | | | |

---

## Naming Convention

| Key Type | Format | Example |
|----------|--------|---------|
| Per-person OpenAI key | `<name>-<role>-<YYYY-MM>` | `alice-engineer-2026-05` |
| Per-person Anthropic key | `<name>-<role>-<YYYY-MM>` | `stephane-lead-2026-05` |
| GitHub PAT | `<name>-<purpose>-<YYYY-MM>` | `stephane-deployments-2026-05` |
| Supabase service key | `supabase-service-role-<env>` | `supabase-service-role-prod` |
| Webhook secret | `<service>-webhook-<env>` | `stripe-webhook-prod` |

---

## How to Add a Key

1. Create the key in the provider platform using the correct naming convention above
2. Add a row to **Active Keys** immediately — before distributing the key
3. Set `Rotation Due` to 90 days from creation (180 days for webhook secrets)
4. Commit this file with message: `chore: add api key <key-name> to inventory`

## How to Revoke a Key

1. Revoke the key in the provider platform
2. Move the row from Active Keys to **Revoked Keys**
3. Add `Date Revoked` and `Reason`
4. If triggered by offboarding, link the offboarding issue in the Reason column
5. Commit this file with message: `chore: revoke api key <key-name>`

## Quarterly Review

At each quarterly access review, the Engineering Lead:
1. Pulls the current key list from each provider (OpenAI, Anthropic, GitHub)
2. Verifies every active key in this log appears in the provider and vice versa
3. Flags keys past their rotation due date — treat as P2 incident
4. Documents the review with a commit: `chore: quarterly api key review <YYYY-MM-DD>`

---

*File Owner: Engineering Lead — engineering-lead@truecalling.ai*
