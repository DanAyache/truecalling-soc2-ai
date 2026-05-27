# Secrets Management Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policies:** Access Control Policy, Change Management Policy, Incident Response Policy  
**SOC 2 Criteria:** CC6.1, CC6.7

---

## 1. Purpose

Define how TrueCalling.ai manages secrets — API keys, tokens, passwords, and certificates — to prevent unauthorized access, accidental exposure, and credential compromise across the Supabase, Vercel, GitHub, and AI tool stack.

## 2. Scope

All secrets used by TrueCalling.ai systems and personnel, including:
- Supabase service role keys and anon keys
- OpenAI and Anthropic API keys
- Vercel environment variables
- GitHub Actions secrets and Personal Access Tokens (PATs)
- Database connection strings
- Webhook signing secrets
- Any credential granting access to a production system

---

## 3. Approved Secret Storage Locations

| Location | Approved For | Not Approved For |
|----------|-------------|-----------------|
| **Vercel Environment Variables** | Production and preview runtime secrets (Supabase keys, OpenAI keys, webhook secrets) | Local development secrets |
| **Company Password Manager** (1Password / Bitwarden) | Shared team credentials, recovery credentials, secondary backup of env var values | Individual API keys that should be per-person |
| **GitHub Actions Secrets** | CI/CD pipeline secrets (deployment tokens, test API keys) | Production service role keys |
| **Local `.env` file** (never committed) | Local development only — must be in `.gitignore` | Anything that touches production data |

**Prohibited storage locations — zero tolerance:**
- Source code files (any language, any repository — public or private)
- Commit messages or PR descriptions
- Slack messages, email, or any chat tool
- Google Docs, Notion, or any unencrypted document
- Personal cloud storage

---

## 4. Secret Naming and Inventory

All secrets must be named descriptively and logged:

### Naming Convention

| Secret Type | Format | Example |
|-------------|--------|---------|
| OpenAI API key | `<name>-<role>-<YYYY-MM>` | `alice-engineer-2026-05` |
| Supabase service key | `supabase-service-role-prod` | — |
| GitHub PAT | `<name>-<purpose>-<YYYY-MM>` | `stephane-deployments-2026-05` |
| Webhook secret | `<service>-webhook-<env>` | `stripe-webhook-prod` |

### Inventory

Named API keys are logged in `evidence/api-keys/openai-keys.md` with:
- Key name
- Owner (person or system)
- Date created
- Date revoked (when applicable)
- Purpose

The Engineering Lead is responsible for keeping this inventory current. It is reviewed at each quarterly access review.

---

## 5. Rotation Schedule

Secrets must be rotated proactively on schedule, and immediately on any trigger event.

### Scheduled Rotation

| Secret Type | Rotation Frequency |
|-------------|-------------------|
| Supabase service role key | Every 90 days |
| OpenAI / Anthropic API keys (per-person) | Every 90 days or on role change |
| GitHub PATs | Every 90 days (enforce expiry at creation) |
| Webhook signing secrets | Every 180 days |
| Vercel deployment tokens | Every 90 days |

GitHub PATs must have an expiry date set at creation — non-expiring PATs are prohibited.

### Mandatory Immediate Rotation Triggers

A secret must be rotated immediately when any of the following occur:

- An employee or contractor with access to the secret departs (within 24 hours per the Offboarding Procedure)
- A secret is suspected or confirmed to have been exposed (Gitleaks, TruffleHog, or manual discovery)
- A device with access to the secret is lost or stolen
- A system that stored the secret is compromised
- A secret has been shared in violation of this policy (even if unintentionally)

Treat any confirmed secret exposure as a minimum P2 incident under the Incident Response Policy.

---

## 6. Secret Lifecycle

### Creation
- Secrets are created with the minimum scope required (e.g., read-only API key for read-only operations)
- Per-person keys are created with the person's name in the key name so they can be revoked individually without disrupting others
- Expiry dates are set at creation where the platform supports it

### Distribution
- Secrets are distributed via the approved storage location only (Vercel env vars, password manager)
- Secrets are never sent via Slack DM, email, or any other channel
- When a secret must be shared with a new engineer, update Vercel or the password manager — do not copy-paste the value

### Use
- Application code must read secrets from environment variables — never hardcoded
- Local development uses `.env` files that are listed in `.gitignore` and never committed
- Secrets must not be logged by application code (mask secrets in logging)

### Revocation
- Revoked immediately on any trigger event above
- Revocation is confirmed and logged in `evidence/api-keys/openai-keys.md`
- After revocation, dependent services are updated to use the new secret before the old one is deleted

---

## 7. Detecting Exposed Secrets

### Automated Detection
- **Gitleaks** and **TruffleHog** scan every push and PR via the `SOC2 Security Checks` workflow — verified secrets cause immediate build failure
- **GitHub native secret scanning** is enabled at the organization level to catch secrets pushed to any repository

### Manual Discovery
- Any employee who discovers a secret in an unauthorized location (commit, Slack, doc) must:
  1. Do not attempt to quietly delete it — report immediately to Engineering Lead
  2. Engineering Lead opens a P1 or P2 incident per the Incident Response Policy
  3. Secret is rotated before the exposed version is removed from the unauthorized location
  4. Git history is cleaned (via `git filter-repo` or GitHub support) after rotation

---

## 8. `.gitignore` Requirements

Every repository must include a `.gitignore` that excludes at minimum:

```
.env
.env.local
.env.*.local
*.pem
*.key
*.p12
*.pfx
```

The presence of these entries is verified during PR review for any new repository setup.

---

## 9. Third-Party Secret Handling

- Secrets provided by third-party vendors (e.g., Stripe webhook keys, SendGrid API keys) are treated identically to internally generated secrets
- Vendor-provided secrets follow the same storage, rotation, and revocation rules
- When a vendor relationship ends, their secrets are revoked within 24 hours per the offboarding process for vendor access in the Access Control Policy

---

## 10. Violations

Committing a secret to a repository — even privately, even if quickly deleted — constitutes a policy violation and triggers immediate incident response. The Engineering Lead determines whether customer notification is required based on what data the secret could have accessed and whether any unauthorized access occurred.

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
