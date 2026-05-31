# Evidence Bootstrap Checklist

**Purpose:** First-round evidence collection to start the SOC 2 Type II observation period.  
**Date Started:** 2026-05-27  
**Owner:** Engineering Lead (stephane@truecalling.ai)  
**Target Completion:** Within 7 days of this date

Complete every item below. Each checkbox includes the exact navigation path and the file name to save the artifact under. Commit the evidence folder after completing each section.

---

## Section 1 — GitHub: MFA & Branch Protection (CC6.1, CC8.1)

> **Navigation:** github.com → Your organization → Settings

### 1.1 MFA Enforcement Screenshot
- [ ] Go to: **Org Settings → Authentication security**
- [ ] Confirm "Require two-factor authentication" is **checked**
- [ ] Take a full-page screenshot showing the org name and the checkbox state
- [ ] Save as: `evidence/access-control/mfa-enforcement/github-mfa-enforcement-2026-05-27.png`

### 1.2 Branch Protection Screenshot
- [ ] Go to: **Repo → Settings → Branches**
- [ ] Click on the protection rule for `main`
- [ ] Screenshot showing:
  - "Require a pull request before merging" — checked
  - "Require approvals" — checked (minimum 1)
  - "Require status checks to pass" — checked (`SOC2 Security Checks`)
  - "Do not allow bypassing the above settings" — checked
- [ ] Save as: `evidence/access-control/mfa-enforcement/github-branch-protection-main-2026-05-27.png`

> **If branch protection is not yet configured:** set it up now at Repo → Settings → Branches → Add rule, then take the screenshot. This is a prerequisite for Type II.

### 1.3 GitHub Org Member List
- [ ] Go to: **Org Settings → Members**
- [ ] Screenshot showing all members, their roles (Owner / Member), and that each has MFA enabled (green shield icon)
- [ ] Save as: `evidence/access-control/quarterly-reviews/rbac-2026-05-27/github-members-2026-05-27.png`
- [ ] Fill in the GitHub section of the RBAC review record: `evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md`

---

## Section 2 — Supabase: Team Members (CC6.1)

> **Navigation:** supabase.com → Project → Settings → Team

### 2.1 Supabase Team Screenshot
- [ ] Go to: **Project → Settings → Team**
- [ ] Screenshot showing all members and their roles (Owner / Developer / Viewer)
- [ ] Save as: `evidence/access-control/quarterly-reviews/rbac-2026-05-27/supabase-members-2026-05-27.png`
- [ ] Fill in the Supabase section of the RBAC review record

---

## Section 3 — Vercel: Team Members (CC6.1)

> **Navigation:** vercel.com → Team → Settings → Members

### 3.1 Vercel Team Screenshot
- [ ] Go to: **Team Settings → Members**
- [ ] Screenshot showing all members and their roles (Owner / Member / Viewer)
- [ ] Save as: `evidence/access-control/quarterly-reviews/rbac-2026-05-27/vercel-members-2026-05-27.png`
- [ ] Fill in the Vercel section of the RBAC review record

---

## Section 4 — RBAC Review Sign-off (CC6.1)

- [ ] Open `evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md`
- [ ] Complete all tables with the member data from screenshots above
- [ ] Compare each member against the RBAC table in `policies/access-control-policy.md`
- [ ] Note any over-provisioned access and open a remediation issue
- [ ] Sign off at the bottom of the review record
- [ ] Commit: `chore: initial rbac review 2026-05-27`

---

## Section 5 — Backup Restore Test (A1.2)

> **Estimated time: 30–60 minutes.** Run against staging, not production.

- [ ] Go to: **Supabase → Project (Production) → Database → Backups**
- [ ] Screenshot showing recent daily backups and PITR enabled
- [ ] Save as: `evidence/backups/verification-logs/supabase-backup-list-2026-05-27.png`
- [ ] Select a backup from **at least 48 hours ago** (not the most recent)
- [ ] Click **Restore** → choose your **staging** Supabase project as the restore target
- [ ] Wait for restore to complete
- [ ] Connect to the restored staging database and run:
  ```sql
  -- Row count verification on key tables
  SELECT schemaname, tablename, n_live_tup AS row_count
  FROM pg_stat_user_tables
  ORDER BY n_live_tup DESC;
  ```
- [ ] Screenshot the query result
- [ ] Save as: `evidence/backups/verification-logs/supabase-restore-rowcount-2026-05-27.png`
- [ ] Fill in the result in: `evidence/backups/verification-logs/2026-05-27-backup-verification.md`
- [ ] Commit: `chore: first backup restore test 2026-05-27`

> **If PITR is not enabled:** go to Supabase → Project Settings → Add-ons → enable Point in Time Recovery. Take a screenshot confirming it is on before doing the restore test.

---

## Section 6 — GitHub Actions Security Scan Export (CC6.7)

- [x] Go to: **Repo → Actions → SOC2 Security Checks workflow**
- [x] Click the most recent successful run
- [x] Download artifact: `soc2-security-summary-<run-id>`
- [x] Save the extracted `security-summary.txt` to: `evidence/security-scans/gitleaks/2026-05/security-summary-2026-05-31.txt`
- [x] No High/Critical findings — all 3 scans passed (Gitleaks, TruffleHog, npm audit)
- [x] SARIF artifact saved: `evidence/security-scans/gitleaks/2026-05/gitleaks-results.sarif.zip`
- [x] Code scanning screenshot saved: `evidence/security-scans/gitleaks/2026-05/security-scan-summary-2026-05-31.png`

---

## Section 7 — truecalling-incidents Repo Setup (CC6.2, CC7.2)

- [x] Check if `truecalling-incidents` private repo exists in your GitHub org
- [x] If not: create it as a **private** repo — this is where incident records and onboarding/offboarding issues are tracked
- [x] Copy the issue templates from `templates/` in this repo into `.github/ISSUE_TEMPLATE/` of `truecalling-incidents`
- [x] Confirm the repo is private (not public)
- [ ] Screenshot of repo settings showing visibility = Private
- [ ] Save as: `evidence/incidents/truecalling-incidents-repo-private-2026-05-27.png`

---

## Section 8 — Commit All Evidence

After completing all sections:
- [ ] `git add evidence/`
- [ ] `git commit -m "chore: evidence bootstrap — initial rbac review, backup restore, scan export 2026-05-27"`
- [ ] Verify commit appears in `git log`

---

## Completion Sign-off

| Section | Status | Date Completed | Notes |
|---------|--------|---------------|-------|
| 1 — GitHub MFA & Branch Protection | | | |
| 2 — Supabase Members | | | |
| 3 — Vercel Members | | | |
| 4 — RBAC Review Sign-off | | | |
| 5 — Backup Restore Test | | | |
| 6 — Security Scan Export | | | |
| 7 — truecalling-incidents Repo | | | |

**Engineering Lead sign-off:** *(name and date)*  
**Type II observation period start date:** *(date all sections complete)*
