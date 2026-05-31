# Compliance Calendar — TrueCalling.ai

**Owner:** Engineering Lead  
**SOC 2 Observation Period Start:** 2026-05-27  
**Calendar Version:** 1.0  
**Last Updated:** 2026-05-31

This calendar is the authoritative schedule of all recurring SOC 2 control obligations. At the start of each quarter, the Engineering Lead reviews this calendar, updates completion dates, and confirms the next due dates are calendared.

---

## How to Use This Calendar

1. At the start of each quarter, copy the **Quarterly block** below and update the dates
2. When a control is completed, mark it ✅ and add the completion date and evidence link
3. If a control is missed, open a GitHub issue in `truecalling-incidents` and document the gap
4. Commit this file after each update: `chore: compliance calendar update <YYYY-MM-DD>`

---

## Quarterly Controls

### Q2 2026 (May – Jul) — First quarter of observation period

| Control | Due Date | Owner | Status | Evidence Location |
|---------|----------|-------|--------|------------------|
| RBAC quarterly access review | 2026-05-27 | Engineering Lead | ✅ 2026-05-27 | `evidence/access-control/quarterly-reviews/rbac-2026-05-27/` |
| Backup restore test (staging) | 2026-05-27 | Engineering Lead | ⏳ Pending | `evidence/backups/verification-logs/2026-05-27-backup-verification.md` |
| Security scan evidence export | 2026-05-31 | Engineering Lead | ✅ 2026-05-31 | `evidence/security-scans/gitleaks/2026-05/` |
| API key rotation review | 2026-08-27 | Engineering Lead | — | `evidence/api-keys/openai-keys.md` |
| Dependabot alert review | 2026-06-30 | Engineering Lead | — | GitHub Security tab screenshot |

### Q3 2026 (Aug – Oct)

| Control | Due Date | Owner | Status | Evidence Location |
|---------|----------|-------|--------|------------------|
| RBAC quarterly access review | 2026-08-27 | Engineering Lead | — | `evidence/access-control/quarterly-reviews/rbac-2026-08-27/` |
| Backup restore test (staging) | 2026-08-27 | Engineering Lead | — | `evidence/backups/verification-logs/2026-08-27-backup-verification.md` |
| Security scan evidence export | 2026-09-30 | Engineering Lead | — | `evidence/security-scans/gitleaks/2026-09/` |
| API key rotation review | 2026-08-27 | Engineering Lead | — | `evidence/api-keys/openai-keys.md` |
| Dependabot alert review | 2026-09-30 | Engineering Lead | — | GitHub Security tab screenshot |

### Q4 2026 (Nov – Dec)

| Control | Due Date | Owner | Status | Evidence Location |
|---------|----------|-------|--------|------------------|
| RBAC quarterly access review | 2026-11-27 | Engineering Lead | — | `evidence/access-control/quarterly-reviews/rbac-2026-11-27/` |
| Backup restore test (staging) | 2026-11-27 | Engineering Lead | — | `evidence/backups/verification-logs/2026-11-27-backup-verification.md` |
| Security scan evidence export | 2026-12-31 | Engineering Lead | — | `evidence/security-scans/gitleaks/2026-12/` |
| API key rotation review | 2026-11-27 | Engineering Lead | — | `evidence/api-keys/openai-keys.md` |
| Dependabot alert review | 2026-12-31 | Engineering Lead | — | GitHub Security tab screenshot |
| Annual tabletop exercise | 2026-12-31 | Engineering Lead | — | `evidence/incidents/tabletop-2026.md` |

---

## Annual Controls

| Control | Frequency | Next Due | Owner | Evidence Location |
|---------|-----------|----------|-------|------------------|
| Security awareness training — all staff | Annual (Q1) | 2027-05-27 | Engineering Lead | `evidence/security-awareness/annual-2027.md` |
| Security awareness training — engineering role-specific | Annual (Q1) | 2027-05-27 | Engineering Lead | `evidence/security-awareness/annual-2027.md` |
| Annual tabletop exercise | Annual (Q4) | 2026-12-31 | Engineering Lead | `evidence/incidents/tabletop-2026.md` |
| Policy annual review — all 8 policies | Annual | 2027-05-27 | Engineering Lead | Policy version history (git log) |
| Vendor management annual review | Annual | 2027-05-27 | Engineering Lead | `evidence/vendor/third-party-reviews/` |
| SOC 2 audit readiness review | Annual | Target Q1 2027 | Engineering Lead | Full audit engagement |

---

## Event-Triggered Controls

These do not run on a schedule but must be completed within the defined SLA when triggered.

| Event | Control Required | SLA | Evidence Location |
|-------|-----------------|-----|------------------|
| New employee / contractor starts | Onboarding issue + access provisioning | Day 1 | `evidence/onboarding/<name>-<date>/` |
| Employee / contractor departs | Offboarding issue + access revocation | 4 hours | `evidence/offboarding/<name>-<date>/` |
| P1 / P2 incident | Incident issue + post-mortem | Post-mortem within 5 business days | `evidence/incidents/` + `truecalling-incidents` GitHub issue |
| API key created | Add to key inventory | Immediate | `evidence/api-keys/openai-keys.md` |
| API key revoked | Update key inventory | Immediate | `evidence/api-keys/openai-keys.md` |
| High/Critical CVE identified | Remediation or documented exception | 7 days (Critical) / 30 days (High) | `evidence/security-scans/exceptions/` |
| New vendor added | Vendor security review | Before production use | `evidence/vendor/third-party-reviews/` |
| Supabase MFA disabled for any member | Open remediation issue | 5 business days | `truecalling-incidents` GitHub issue |

---

## Open Items Tracker

| # | Item | Due Date | Owner | Status |
|---|------|----------|-------|--------|
| 1 | Enable Supabase MFA — Yarone Cohen | 2026-06-04 | Yarone Cohen | ⏳ Open |
| 2 | Enable Supabase MFA — Patrick Simon Bouaziz | 2026-06-04 | Patrick Simon Bouaziz | ⏳ Open |
| 3 | Add second GitHub org owner | — | Engineering Lead | ⏳ Open |
| 4 | Complete backup restore test | — | Engineering Lead | ⏳ Blocked — access pending |
| 5 | Add GITLEAKS_LICENSE to GitHub Secrets | 2026-05-31 | Engineering Lead | ✅ Closed — not required; repo hosted under personal account (DanAyache), not a GitHub org. Workflow correctly configured; Gitleaks scan passed 2026-05-31 with no license secret needed. |
| 6 | Populate API key inventory | — | Engineering Lead | ⏳ Open |

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next calendar review: 2026-08-27 (Q3 start)*
