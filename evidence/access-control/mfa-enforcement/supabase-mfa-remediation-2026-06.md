# Supabase MFA Remediation & Evidence Record

**Date opened:** 2026-05-28 (identified in [RBAC review 2026-05-27](../quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md) Issue #1)
**Owner:** Engineering Lead
**Finding ID:** F-19 · **Risk:** [R-002](../../risk-register/risk-register-2026.md)
**SOC 2 Criteria:** CC6.1 (Authentication) · **Policy:** [Access Control Policy](../../../policies/access-control-policy.md) §5
**Remediation due:** 2026-06-04 (5 business days per Access Control Policy §8)

---

## 1. Finding

The 2026-05-27 quarterly RBAC review found **MFA disabled for both Supabase members** holding privileged access to the production project:

| Member | Email | Supabase Role | MFA at review (2026-05-27) | MFA required by |
|--------|-------|---------------|----------------------------|-----------------|
| Yarone Cohen | cohen.yarone@icloud.com | Owner | ❌ No | Access Control Policy §5 (mandatory, TOTP) |
| Patrick Simon Bouaziz | patrick@sarona-partners.com | Administrator | ❌ No | Access Control Policy §5 (mandatory, TOTP) |

**Why it matters:** Both accounts can reach production data (customer call recordings/transcripts — see R-008). A compromised password without a second factor is sufficient for full dashboard access.

**Constraint:** The Supabase **Free plan does not enforce org-level MFA**, so enforcement is per-account until the org upgrades to a plan with MFA enforcement. Both members were notified to enable MFA on their personal Supabase accounts.

---

## 2. Remediation steps (owner action)

For **each** member:

1. Sign in to Supabase → **Account → Account Settings → Security / Multi-Factor Authentication**.
2. Add a **TOTP authenticator** (Google Authenticator / Authy). *SMS is not an approved method per Access Control Policy §5.*
3. Complete enrolment and confirm MFA shows **enabled**.
4. Capture evidence per §3 below.

Then, at the organization level:

5. Re-check **Project → Settings → Team** to confirm both members now show MFA enabled.
6. Record the date MFA enforcement at org level becomes possible (on plan upgrade) and open a follow-up to enforce it.

---

## 3. Evidence to collect

Save screenshots in this folder (`evidence/access-control/mfa-enforcement/`). Redact nothing sensitive is needed — these show MFA *state*, not secrets.

| # | Screenshot | Save as | Collected? |
|---|------------|---------|------------|
| 1 | Yarone Cohen — account security page showing MFA/TOTP **enabled** | `supabase-mfa-yarone-2026-06.png` | ☐ |
| 2 | Patrick Simon Bouaziz — account security page showing MFA/TOTP **enabled** | `supabase-mfa-patrick-2026-06.png` | ☐ |
| 3 | Project → Settings → Team showing both members with MFA enabled | `supabase-team-mfa-2026-06.png` | ☐ |

> **Do not fabricate.** This record is committed in advance to track the remediation; the screenshots above are the actual evidence and must be captured from the live Supabase dashboard once each member completes enrolment.

---

## 4. Verification & sign-off

| Field | Value |
|-------|-------|
| Both members MFA enabled (TOTP) | ☐ Yes — date: __________ |
| Screenshots 1–3 committed | ☐ Yes |
| Org-level MFA enforcement available? | ☐ Yes / ☐ No (plan limitation) — follow-up: __________ |
| R-002 status updated in Risk Register | ☐ Yes |
| Issue #1 in RBAC review marked Resolved | ☐ Yes |
| **Verified by** | __________ (name) |
| **Date** | __________ |

On completion: update [R-002](../../risk-register/risk-register-2026.md) → **Mitigated**, mark Issue #1 **Resolved** in the [RBAC review](../quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md), and set **F-19 → Closed** in the [Findings & Remediation Register](../../../findings/findings-remediation-register.md). Commit: `security: supabase MFA enabled for all members 2026-06`.

---

*Status as of 2026-06-02:* **Evidence Pending** — remediation record and collection steps ready; awaiting members to enable MFA and capture screenshots (due 2026-06-04).
*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
