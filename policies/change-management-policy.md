# Change Management Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**SOC 2 Criteria:** CC8.1 (Change Management)

---

## 1. Purpose

Ensure all changes to production systems are reviewed, tested, approved, and traceable — preventing unauthorized or untested changes from introducing security vulnerabilities, data loss, or service disruption.

## 2. Scope

All changes to production systems including:
- Application code deployed to Vercel
- Supabase schema migrations, RLS policies, and configuration changes
- GitHub Actions workflows and repository settings
- Security policies, procedures, and access controls
- Third-party integrations and API configurations

---

## 3. Change Categories

| Category | Definition | Approval Required | Review Required |
|----------|------------|------------------|-----------------|
| **Standard** | Routine code change, bug fix, dependency update | Peer engineer | 1 reviewer |
| **Significant** | New feature, schema migration, new integration, access control change | Engineering Lead | 1 reviewer + Engineering Lead |
| **Security** | Changes to workflows, policies, CODEOWNERS, secrets, or auth logic | Engineering Lead | Engineering Lead only |
| **Emergency** | Production incident requiring immediate fix outside normal process | Engineering Lead (post-hoc within 24 hr) | Post-deployment review required |

---

## 4. Standard Change Process (all non-emergency changes)

All changes to production must follow this process:

### Step 1 — Branch
- Create a feature branch from `main` with a descriptive name: `<type>/<short-description>` (e.g., `fix/supabase-rls-policy`, `feat/onboarding-flow`)
- Never commit directly to `main` or `master`

### Step 2 — Develop and Test
- Write and test the change locally or in a staging environment
- For schema migrations: test against a copy of production schema in staging before opening a PR
- For security changes: run the security workflow locally if possible (`act` or manual trigger)

### Step 3 — Pull Request
- Open a PR against `main` with:
  - **Title:** following conventional commit format (`feat:`, `fix:`, `security:`, `policy:`, etc.)
  - **Description:** what changed, why, how it was tested, and any rollback plan
  - **Linked issue** (if applicable)
- The `SOC2 Security Checks` workflow runs automatically on all PRs — all checks must pass before merge

### Step 4 — Review
- At least **1 approved review** is required before merge (enforced by branch protection)
- The PR author may not approve their own PR
- For **Significant** or **Security** changes: Engineering Lead must be one of the reviewers
- CODEOWNERS enforcement: changes to `policies/`, `procedures/`, `evidence/`, and `.github/` automatically require Engineering Lead review

### Step 5 — Merge
- Merge using "Squash and merge" or "Merge commit" — no force-push merges
- Delete the feature branch after merge
- The merged PR is the permanent change record in the audit trail

---

## 5. Deployment to Production

### Vercel (application deployments)
- Production deployments are triggered automatically on merge to `main`
- Deployments can be rolled back via the Vercel dashboard (last 90 days of deployments retained)
- Environment variable changes require Engineering Lead approval and must be documented in the PR or as a Vercel deployment comment

### Supabase (database changes)
- Schema migrations must be applied via the migration file in the repository (not the Supabase SQL editor), so changes are tracked in git
- Direct SQL editor changes to production are prohibited except during declared incidents, and must be documented in the incident record immediately after
- RLS policy changes are treated as Security-category changes and require Engineering Lead review

---

## 6. Emergency Change Process

When a production incident requires an immediate fix that cannot wait for a normal PR review:

1. **Declare** the change as emergency in the `#security-incidents` Slack channel or incident issue
2. **Implement** the minimal fix required to contain the incident
3. **Notify** Engineering Lead immediately — they must be aware before or during deployment
4. **Document** within 24 hours: what was changed, why, what the risk was, and what the rollback plan was
5. **Open a follow-up PR** for the emergency fix (even if already deployed) — this creates the audit record
6. Engineering Lead reviews and approves the follow-up PR within 2 business days

Emergency changes are logged in `evidence/change-management/emergency-changes-<YYYY>.md`.

---

## 7. Change Rollback

Every significant change should have a rollback plan documented in the PR description. Rollback mechanisms by system:

| System | Rollback Mechanism |
|--------|-------------------|
| Vercel | Instant rollback to previous deployment via Vercel dashboard (< 2 min) |
| Supabase schema | Reverse migration file applied via SQL or migration runner |
| Supabase config | Revert via dashboard or re-apply prior RLS policy from git history |
| GitHub Actions | Revert commit to previous workflow version |
| Policies/procedures | Revert PR via git; old version in git history |

---

## 8. Change Records and Evidence

The git commit history and merged PR list in GitHub are the primary change management audit trail. For SOC 2 purposes:

- All PRs must have a meaningful description — one-word descriptions are not acceptable
- The PR list is exportable from GitHub and constitutes evidence for CC8.1
- For **Significant** and **Security** changes: Engineering Lead stores a note in `evidence/change-management/` if additional context beyond the PR is needed (e.g., justification for a major architectural decision)
- Emergency changes are always logged in `evidence/change-management/emergency-changes-<YYYY>.md`

---

## 9. Prohibited Changes

- Direct commits to `main` without a PR (enforced by branch protection)
- Disabling or bypassing the `SOC2 Security Checks` workflow
- Modifying CODEOWNERS to remove required reviewers without Engineering Lead approval
- Applying schema changes to production outside of the migration file process
- Granting new production access during a change as a workaround (access changes follow the Access Control Policy)

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
