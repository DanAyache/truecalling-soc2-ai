# Backup Verification Log — [YYYY-MM-DD]

> **Instructions:** Copy this file to `evidence/backups/verification-logs/<YYYY-MM-DD>-backup-verification.md`
> Replace all `<placeholders>` before committing.

---

**Date:** `<YYYY-MM-DD>`  
**Performed By:** Engineering Lead  
**Verification Type:** Quarterly restore test / Annual DR drill / Ad-hoc  
**SOC 2 Criteria:** A1.2  
**Reference:** Backup Policy §5, Evidence Collection Procedure §4.5  
**Next test due:** `<YYYY-MM-DD>` *(+90 days for quarterly)*

---

## Pre-Test Checks

| Check | Status | Notes |
|-------|--------|-------|
| Supabase daily backup confirmed running | ✅ / ❌ | Screenshot: `supabase-backup-list-<YYYY-MM-DD>.png` |
| PITR (Point-in-Time Recovery) enabled | ✅ / ❌ | Screenshot showing PITR enabled |
| PITR retention window | | *(enter actual window shown in dashboard, e.g. "7 days")* |
| Backup retention count | | *(enter number of daily backups shown)* |
| Most recent backup timestamp | | *(e.g. "2026-08-26 03:00 UTC")* |
| Staging project available for restore | ✅ / ❌ | Supabase project ID: `<staging-project-id>` |

> **If PITR is not enabled:** Mark the verification as FAIL and open a remediation item.

---

## Backup Selection

| Field | Value |
|-------|-------|
| Backup snapshot selected | *(date/time of the backup to restore — must be ≥ 48 hours old)* |
| Reason for selecting this snapshot | *(e.g. "48-hour-old daily backup — standard quarterly test")* |

---

## Restore Procedure

| Step | Timestamp (UTC) | Status | Notes |
|------|-----------------|--------|-------|
| Restore initiated | | ✅ / ❌ | Supabase dashboard → Database → Backups → Restore |
| Restore completed | | ✅ / ❌ | |
| Restore duration | | | *(end time minus start time)* |
| Restore target | | | Staging Supabase project ID: `<staging-project-id>` |

---

## Data Integrity Verification

**Method:** Row count query against key tables on the restored database

```sql
SELECT schemaname, tablename, n_live_tup AS row_count
FROM pg_stat_user_tables
ORDER BY n_live_tup DESC;
```

**Query results:**

| Table | Row Count | Expected (approx.) | Match? |
|-------|-----------|-------------------|--------|
| | | | |
| | | | |
| | | | |

**Screenshot:** `supabase-restore-rowcount-<YYYY-MM-DD>.png`

**Additional spot checks performed:** *(optional — list any specific queries run to verify data integrity)*

---

## RPO / RTO Assessment

| Target | Value | Achieved? |
|--------|-------|-----------|
| RPO (Recovery Point Objective) | 24 hours | *(yes — backup was from `<timestamp>`) / no* |
| RTO (Recovery Time Objective) | 4 hours | *(yes — restore completed in `<duration>`) / no* |

---

## Result

- [ ] **PASS** — Restore completed successfully; data integrity verified; RPO and RTO targets met
- [ ] **FAIL** — *(describe what failed and open a P2 incident issue)*

**Overall result:** PASS / FAIL

---

## Findings & Notes

*(Any observations, warnings, or follow-up actions from this test)*

**Impact Assessment:**

- Customer impact: None / Low / Medium / High
- Follow-up required: Yes / No

---

## Remediation (if FAIL)

A failed backup verification is treated as a **P2 incident** per Backup Policy §5.

- [ ] P2 incident opened in `truecalling-incidents`: `<issue link>`
- [ ] Root cause documented in the incident issue
- [ ] Remediation plan confirmed by Engineering Lead

---

## Evidence Checklist

- [ ] Pre-test screenshot: `supabase-backup-list-<YYYY-MM-DD>.png`
- [ ] PITR confirmation screenshot (if newly verified)
- [ ] Row count query screenshot: `supabase-restore-rowcount-<YYYY-MM-DD>.png`
- [ ] This completed log committed to `evidence/backups/verification-logs/`

---

**Sign-off:** `<Engineering Lead name>`, `<YYYY-MM-DD>`  
**Next restore test due:** `<YYYY-MM-DD>`  
**Next full DR drill due:** `<YYYY+1-MM-DD>` *(annual)*

---

*Evidence retained for: 3 years per Backup Policy §6*
