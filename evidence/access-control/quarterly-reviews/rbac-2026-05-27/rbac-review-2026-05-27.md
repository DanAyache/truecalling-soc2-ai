# RBAC Quarterly Access Review

**Review Date:** 2026-05-27  
**Reviewer:** Engineering Lead  
**Review Type:** Initial bootstrap (first quarterly review)  
**Next Review Due:** 2026-08-27  
**SOC 2 Criteria:** CC6.1, CC6.2  
**Reference:** Access Control Policy §8

---

## Instructions

1. Pull current member lists from GitHub, Supabase, and Vercel
2. Fill in each table below with the actual members and their roles
3. Compare each role against the approved RBAC ceiling in the Access Control Policy
4. Flag any over-provisioned access in the Issues section
5. Sign off at the bottom and commit

Screenshots for each system are saved alongside this file.

---

## GitHub — Organization Members

**Source:** github.com → Org Settings → Members  
**Screenshot:** `github-members-2026-05-27.png`

| Username | Display Name | Org Role | MFA Enabled | RBAC Ceiling (from policy) | Match? |
|----------|-------------|----------|-------------|---------------------------|--------|
| | | | | | |
| | | | | | |
| | | | | | |

**Team memberships reviewed:** *(list teams and confirm members match role)*

---

## Supabase — Project Team

**Source:** supabase.com → Project Settings → Team  
**Screenshot:** `supabase-members-2026-05-27.png`

| Email | Name | Supabase Role | RBAC Ceiling (from policy) | Match? |
|-------|------|--------------|---------------------------|--------|
| | | | | |
| | | | | |

---

## Vercel — Team Members

**Source:** vercel.com → Team Settings → Members  
**Screenshot:** `vercel-members-2026-05-27.png`

| Email | Name | Vercel Role | RBAC Ceiling (from policy) | Match? |
|-------|------|------------|---------------------------|--------|
| | | | | |
| | | | | |

---

## API Key Review

**Source:** `evidence/api-keys/openai-keys.md`

- [ ] All active keys in the inventory are confirmed present in the provider platform
- [ ] No keys exist in the provider platform that are absent from the inventory
- [ ] All active keys are within their 90-day rotation window
- [ ] No keys are owned by departed employees

---

## Issues Found

*(List any over-provisioned access, missing MFA, unrecognized accounts, or keys past rotation due date)*

| # | System | Issue | Action Required | Due Date | Resolved? |
|---|--------|-------|----------------|---------|-----------|
| | | | | | |

Over-provisioned access must be remediated within **5 business days** per the Access Control Policy §8.

---

## Sign-off

- [ ] All tables completed
- [ ] All screenshots saved alongside this file
- [ ] All issues logged above and GitHub issues opened where required
- [ ] No unresolved critical access gaps

**Reviewed by:** *(Engineering Lead name)*  
**Date completed:** *(date)*  
**Signature / GitHub handle:** *(handle)*

---

*Next quarterly review due: 2026-08-27*  
*Evidence retained for: 3 years*
