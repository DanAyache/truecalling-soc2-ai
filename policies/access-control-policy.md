# Access Control Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual

---

## 1. Purpose

This policy establishes controls for granting, managing, and revoking access to TrueCalling.ai systems to protect customer data and meet SOC 2 Type II requirements.

## 2. Scope

Applies to all employees, contractors, and third parties accessing: Supabase (database/auth), Vercel (hosting), GitHub (source code), and any SaaS tools used in production operations.

---

## 3. Access Principles

**Least Privilege** — Users receive the minimum permissions required for their role. Access is denied by default and granted explicitly.

**Need to Know** — Access to production data requires documented business justification.

**Separation of Duties** — No single person may both approve and implement a privileged change.

---

## 4. Role-Based Access Control (RBAC)

| Role | GitHub | Vercel | Supabase | Notes |
|------|--------|--------|----------|-------|
| Engineering Lead | Owner | Owner | Owner | Max 2 people |
| Senior Engineer | Maintain | Member | Developer | Write access to production |
| Engineer | Write | Viewer | Viewer | No direct production DB access |
| Contractor | Write (repo-specific) | None | None | Scoped to assigned repos only |
| Support | Read (logs only) | None | None | No code or DB access |

Access levels above are ceilings. Actual grants must match current role responsibilities.

---

## 5. Authentication Requirements

**MFA is mandatory** for all accounts on all platforms (GitHub, Vercel, Supabase, Google Workspace). Enforcement:

- **GitHub:** Organization-level MFA enforcement enabled. Non-compliant accounts are automatically suspended.
- **Vercel:** MFA enforced via SSO (Google Workspace). Direct email/password login is disabled.
- **Supabase:** MFA required on all accounts with dashboard access. Service role keys are never shared with individuals.
- **Approved MFA methods:** TOTP authenticator apps (Google Authenticator, Authy). SMS is not permitted.

Passwords must be managed via a company-approved password manager. Shared passwords are prohibited.

---

## 6. Access Provisioning and Deprovisioning

**Onboarding**
1. Engineering Lead submits access request in the access log (GitHub issue or Notion page) specifying role, systems, and justification.
2. Access is granted within 1 business day of approval.
3. New users receive only default (viewer/read) permissions until onboarding is confirmed complete.

**Offboarding**
1. HR or manager notifies Engineering Lead on or before the last day.
2. All access is revoked within **4 hours** of departure.
3. Shared credentials (API keys, tokens) the user had access to are rotated within 24 hours.

**Transfers** — Role changes trigger an access review. Permissions from the prior role are revoked before new ones are granted.

---

## 7. Privileged Access

- **Production Supabase service role keys** are stored only in Vercel environment variables (encrypted at rest). They are never committed to GitHub or shared in Slack/email.
- **GitHub Actions** use OIDC or short-lived secrets; long-lived PATs are prohibited.
- Direct production database access (Supabase SQL editor) requires Engineering Lead approval and is logged.
- Infrastructure changes (Vercel project settings, Supabase project config) require a peer review via pull request or documented approval before execution.

---

## 8. Access Reviews

| Frequency | Scope |
|-----------|-------|
| Quarterly | Full review of all user permissions across GitHub, Vercel, Supabase |
| On role change | Affected user's access |
| On offboarding | Immediate revocation confirmed by Engineering Lead |

Reviews are documented in the access log. Over-provisioned access is removed within 5 business days of discovery.

---

## 9. Third-Party and Vendor Access

- Contractors receive access scoped to specific repositories or projects, with an expiration date set at provisioning.
- Third-party vendor access to production systems requires written approval from the Engineering Lead and is logged.
- Vendor accounts are reviewed quarterly and removed when the engagement ends.

---

## 10. Policy Violations

Violations (e.g., sharing credentials, bypassing MFA, accessing systems outside assigned scope) are escalated to the Engineering Lead and may result in immediate access suspension pending review under the Incident Response Policy.

---

## 11. Exceptions

Exceptions require written approval from the Engineering Lead, must be time-bounded (maximum 30 days), and are logged with justification.

---

*Policy Owner: Engineering Lead — stephane@truecalling.ai*  
*Next Review: 2027-05-27*
