# Quarterly Access Review — [YYYY-MM-DD]

> **Instructions:** Copy this file to `evidence/access-control/quarterly-reviews/rbac-<YYYY-MM-DD>/rbac-review-<YYYY-MM-DD>.md`
> Replace all `<placeholders>` before committing.

---

**Review Date:** `<YYYY-MM-DD>`  
**Reviewer:** Engineering Lead  
**Review Type:** Quarterly / Annual / Ad-hoc  
**Previous Review Date:** `<YYYY-MM-DD>`  
**Next Review Due:** `<YYYY-MM-DD>` *(+90 days)*  
**SOC 2 Criteria:** CC6.1, CC6.2  
**Reference:** Access Control Policy §8

---

## Instructions

1. Pull current member lists from GitHub, Supabase, and Vercel
2. Fill in each table below with the actual members and their roles
3. Compare each role against the approved RBAC ceiling in the Access Control Policy
4. Flag any over-provisioned access in the Issues section
5. Complete all justification sections for non-Engineering-Lead members
6. Sign off at the bottom and commit

Screenshots for each system must be saved alongside this file.

---

## GitHub — Organization Members

**Source:** github.com → truecalling-ai → Settings → Members  
**Screenshot:** `github-members-<YYYY-MM-DD>.png`

| Username | Display Name | Org Role | MFA Enabled | RBAC Ceiling (from Policy) | Match? |
|----------|-------------|---------|------------|--------------------------|--------|
| | | | | | |
| | | | | | |

**Access Justifications** *(required for any non-Engineering-Lead member)*

**User:**  
**Role:**  
**Business Justification:**  
**Reviewed By:**  
**Decision:**

---

**Team memberships reviewed:** *(list teams, or "No teams configured")*

---

## Supabase — Project Team

**Source:** supabase.com → Organization Settings → Team  
**Screenshot:** `supabase-members-<YYYY-MM-DD>.png`

| Email | Name | Supabase Role | MFA Enabled | RBAC Ceiling (from Policy) | Match? |
|-------|------|-------------|------------|--------------------------|--------|
| | | | | | |
| | | | | | |

> **MFA requirement:** Any member with MFA Disabled must be recorded in the Issues section below with a remediation due date of no more than 5 business days. Access should not be expanded for any member until MFA is confirmed enabled.

**Access Justifications** *(required for any non-Engineering-Lead member)*

**User:**  
**Role:**  
**Business Justification:**  
**Reviewed By:**  
**Decision:**

---

## truecalling-incidents Repository

**Source:** github.com → truecalling-ai → truecalling-incidents → Settings  
**Screenshot:** `incidents-repo-settings-<YYYY-MM-DD>.png`

| Check | Status | Notes |
|-------|--------|-------|
| Repository visibility confirmed Private | ✅ / ❌ | |
| No unexpected collaborators added | ✅ / ❌ | |
| Issue templates present and unmodified | ✅ / ❌ | |

**Collaborators reviewed:**

| Username | Role | Business Justification | Still Required? |
|----------|------|----------------------|----------------|
| | | | |

*(If no external collaborators: "No collaborators — org members only.")*

---

## Vercel — Team Members

**Source:** vercel.com → Team Settings → Members  
**Screenshot:** `vercel-members-<YYYY-MM-DD>.png`

| Email | Name | Vercel Role | RBAC Ceiling (from Policy) | Match? |
|-------|------|-----------|--------------------------|--------|
| | | | | |

---

## API Key Review

**Source:** `evidence/api-keys/openai-keys.md`

- [ ] GitHub personal access tokens reviewed — no expired or unauthorized tokens
- [ ] Supabase service role keys reviewed — rotation dates confirmed
- [ ] Vercel deployment tokens reviewed
- [ ] OpenAI API keys reviewed — all named, rotation dates confirmed
- [ ] Anthropic / Claude Code keys reviewed
- [ ] No unused or unauthorized API keys identified

**Keys past rotation due date:** *(list any, or "None")*

---

## Issues Identified

| # | System | Issue | Action Required | Due Date | Resolved? |
|---|--------|-------|----------------|----------|-----------|
| | | | | | |

*(If no issues: "No issues identified — all access matches approved RBAC table.")*

---

## Changes Since Last Review

*(List any access changes — provisioning, revocations, role changes — since the previous review)*

| Change Type | Person | System | Old Role | New Role | Date | Evidence |
|-------------|--------|--------|---------|---------|------|---------|
| | | | | | | |

*(If no changes: "No access changes since last review.")*

---

## Reviewer Sign-Off

- [ ] All member tables completed with current data
- [ ] All access matches approved RBAC ceilings in the Access Control Policy
- [ ] All over-provisioned access has a remediation due date assigned
- [ ] All API keys reviewed and rotation dates confirmed
- [ ] Screenshots saved alongside this file

**Reviewed By:** `<Engineering Lead name>`  
**GitHub Handle:** `<@handle>`  
**Review Date:** `<YYYY-MM-DD>`  
**Approval Status:** Approved / Approved with findings

**Notes:**

---

*Next quarterly review due: `<YYYY-MM-DD>`*  
*Evidence retained for: 3 years*
