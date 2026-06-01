# Risk Register — TrueCalling.ai

**Organization:** TrueCalling.ai
**Version:** 1.0
**Effective Date:** 2026-06-01
**Owner:** Engineering Lead
**Review Cycle:** Quarterly (every 90 days), and ad-hoc on any P1/P2 incident
**SOC 2 Criteria:** CC3.1, CC3.2, CC3.3, CC3.4 (Risk Assessment)
**Related Policy:** [Information Security Policy](../../policies/information-security-policy.md) §4.3 (Guiding Principles)

---

## 1. Purpose

This Risk Register is the authoritative record of security, operational, and compliance risks identified by TrueCalling.ai. It documents the likelihood, impact, existing controls, mitigation plan, owner, and current status of each risk, and is reviewed quarterly by the Engineering Lead per SOC 2 CC3 requirements.

## 2. Scope

All risks within the scope of the Information Security Policy: customer data, AI model assets, production systems (Supabase, Vercel, GitHub, Google Workspace), and personnel with access to any of the above.

---

## 3. Scoring Methodology

### 3.1 Likelihood (probability of occurrence in the next 12 months)

| Rating | Definition |
|--------|-----------|
| 1 — Rare | <5% chance; would require an unusual sequence of events |
| 2 — Unlikely | 5–25%; could occur but not expected |
| 3 — Possible | 25–50%; reasonable chance; has occurred in similar orgs |
| 4 — Likely | 50–75%; expected to occur absent mitigation |
| 5 — Almost Certain | >75%; expected to occur, possibly multiple times |

### 3.2 Impact (worst plausible consequence if the risk materializes)

| Rating | Definition |
|--------|-----------|
| 1 — Negligible | No customer impact; internal-only inconvenience |
| 2 — Minor | Limited customer impact; recoverable within 1 business day |
| 3 — Moderate | Customer impact lasting <1 week; SLA breach; remediable |
| 4 — Major | Multi-customer impact; data exposure; regulatory notification likely |
| 5 — Severe | Existential business impact; widespread breach; legal liability |

### 3.3 Risk Rating = Likelihood × Impact

| Score | Rating | Required Action | Review Cadence |
|-------|--------|-----------------|----------------|
| 1–4 | **Low** | Accept or monitor | Annual |
| 5–9 | **Medium** | Mitigate with planned controls | Quarterly |
| 10–16 | **High** | Active mitigation required | Monthly |
| 17–25 | **Critical** | Immediate mitigation; escalate to Engineering Lead within 24h | Weekly until reduced |

### 3.4 Status Values

- **Open** — Identified; mitigation not yet started
- **In Progress** — Mitigation underway
- **Mitigated** — Controls in place; residual risk acceptable
- **Accepted** — Risk acknowledged; no further action (justification required in Notes)
- **Closed** — Risk no longer applicable

---

## 4. Active Risks

| ID | Description | L | I | R | Existing Controls | Mitigation Plan | Owner | Status | Review Date |
|----|-------------|---|---|---|------------------|-----------------|-------|--------|-------------|
| R-001 | Key person dependency on Engineering Lead — sole accountable owner of security program, single GitHub org owner | 3 | 4 | **12 High** | Documented policies and procedures committed to git; evidence trail in `evidence/` | Add second GitHub org owner (F-09); cross-train Authorized Personnel on backup verification and access reviews | Engineering Lead | Open | 2026-07-01 |
| R-002 | Supabase MFA not enforced for Yarone Cohen and Patrick Simon Bouaziz; dashboard access without second factor | 3 | 4 | **12 High** | Access Control Policy mandates MFA; Engineering Lead has MFA enabled | Both members must enable MFA by 2026-06-04; enforce at org level once Supabase MFA enforcement available | Engineering Lead | In Progress | 2026-06-04 |
| R-003 | Personal email addresses used for Supabase accounts; offboarding may leave residual access if personal email is the auth factor | 2 | 3 | **6 Medium** | Quarterly RBAC review (CC6.1) | Migrate Supabase accounts to company emails under truecalling.ai domain (F-05) | Engineering Lead | Open | 2026-09-01 |
| R-004 | Source repository hosted under personal GitHub account (`DanAyache`) rather than a GitHub organization; governance and ownership tied to an individual account | 2 | 4 | **8 Medium** | MFA enabled on personal account; branch protection on `main`; CODEOWNERS enforced; required PR reviews | Evaluate migration to organization ownership or implement equivalent governance controls | Engineering Lead | Open | 2026-09-01 |
| R-005 | Secrets inadvertently committed to source code or git history | 2 | 4 | **8 Medium** | Gitleaks + TruffleHog scans on every PR via GitHub Actions; Secrets Management Policy; npm audit | Maintain CI gate; quarterly review of scanner exceptions; add pre-commit hook on developer machines (future) | Engineering Lead | Mitigated | 2026-09-01 |
| R-006 | Tampering with GitHub Actions workflows to bypass security checks or inject malicious build steps | 2 | 4 | **8 Medium** | CODEOWNERS protects `.github/workflows/`; branch protection requires PR review on `main`; required status checks | Pin third-party Actions to commit SHAs (not version tags) at next workflow update; audit `.github/` directory each quarterly review | Engineering Lead | In Progress | 2026-09-01 |
| R-007 | Backup restore process has not been tested during current observation period; restore capability is unverified | 3 | 4 | **12 High** | Daily automated Supabase backups; Point-in-Time Recovery enabled; Backup Policy defines RPO 24h / RTO 4h | Complete first quarterly restore test against staging (currently blocked — access pending); document outcome in `evidence/backups/verification-logs/` | Engineering Lead | Open | 2026-06-30 |
| R-008 | Exposure of customer call recordings, transcripts, or voice biometric data through dashboard access or query | 2 | 5 | **10 High** | Supabase Row Level Security; encrypted at rest; access logged; RBAC ceiling enforces least privilege on production DB | Document data classification scheme (Confidential vs Restricted); implement automatic retention/deletion policy for transcripts; revisit RLS coverage quarterly | Engineering Lead | Open | 2026-07-01 |
| R-009 | Prompt injection against AI agent via crafted caller input, document upload, or webhook payload | 4 | 3 | **12 High** | Input sanitization at boundary; system prompt isolation from user-supplied content; output moderation; logging of agent interactions | Build automated test suite for known injection patterns; quarterly red team exercise focused on prompt injection; document degraded-mode behavior | Engineering Lead | Open | 2026-09-01 |
| R-010 | LLM provider (Anthropic, OpenAI) outage, rate-limit, or breaking API change disrupts service | 4 | 3 | **12 High** | Multi-provider integration in place (Anthropic + OpenAI); error monitoring | Implement provider fallback routing in agent runtime; document degraded-mode user experience; subscribe to provider status pages | Engineering Lead | In Progress | 2026-09-01 |
| R-011 | Customer data inadvertently ingested into LLM provider's training corpus | 2 | 4 | **8 Medium** | Default provider API terms exclude API data from model training (per published OpenAI API data usage policy and Anthropic Commercial Terms) | Complete vendor SOC 2 and DPA collection (F-06); evaluate enrollment in zero-data-retention programs (OpenAI ZDR / Anthropic equivalent) for production traffic; document training opt-out attestations once obtained | Engineering Lead | In Progress | 2026-09-01 |
| R-012 | Vendor SOC 2 Type II reports and DPAs not collected for critical sub-processors (Supabase, Vercel, OpenAI, Anthropic, Google, GitHub, 1Password) | 3 | 3 | **9 Medium** | Vendor Management Policy in place; vendors selected from SOC 2–attested set | Complete F-06: download SOC 2 reports + DPAs and store under `evidence/vendor/third-party-reviews/` by 2026-07-31 | Engineering Lead | Open | 2026-07-31 |
| R-013 | Twilio API credential compromise leading to toll fraud, SMS/WhatsApp abuse, or unauthorized calls | 2 | 4 | **8 Medium** | Twilio account MFA; API keys scoped per environment; fraud detection alerts | Add Twilio credentials to API key inventory (F-04); enable geographic permissions restrictions; set monthly spend alerts | Engineering Lead | Open | 2026-07-31 |
| R-014 | Dependency or supply chain vulnerability in npm packages | 3 | 3 | **9 Medium** | `npm audit` runs in CI; Dependabot weekly PRs; Critical/High SLA per SECURITY.md (7d / 30d) | Configure auto-merge for low-risk Dependabot updates after CI passes; quarterly review of unresolved alerts | Engineering Lead | Mitigated | 2026-09-01 |
| R-015 | Personnel turnover during SOC 2 observation period causes access lifecycle (provisioning/deprovisioning) evidence gaps | 3 | 3 | **9 Medium** | Onboarding and offboarding issue templates in `truecalling-incidents`; 4-hour deprovisioning SLA per Access Control Policy | Complete missing onboarding packages for current team (F-07); rehearse a synthetic offboarding to validate the 4h SLA | Engineering Lead | Open | 2026-07-31 |

---

## 5. Risk Acceptance Decisions

*(Empty — populate when any risk is moved to status **Accepted**. Each entry must include: risk ID, justification, compensating control, expiration date, approver.)*

| Risk ID | Justification | Compensating Control | Expiration | Approver |
|---------|---------------|---------------------|------------|----------|
| | | | | |

---

## 6. Closed / Retired Risks

*(Empty — populate as risks are mitigated to closure or become inapplicable.)*

| Risk ID | Description | Date Closed | Closure Reason |
|---------|-------------|-------------|----------------|
| | | | |

---

## 7. Review Procedure

At each quarterly review the Engineering Lead must:

1. Re-score every Open and In Progress risk based on current environment
2. Move any risks where mitigation is complete to Mitigated → Closed once residual risk is acceptable
3. Add any new risks identified since the last review (e.g., new vendor, new product capability, post-incident findings)
4. Confirm every High or Critical risk has an active mitigation plan with a named owner and due date
5. Commit the updated register with message: `risk: quarterly risk register review <YYYY-MM-DD>`
6. Reference the review in the [Compliance Calendar](../../templates/compliance-calendar.md)

The Engineering Lead may delegate execution to Authorized Personnel but retains accountability for the register's accuracy.

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Next quarterly review: 2026-09-01*
*Evidence retention: 3 years*
