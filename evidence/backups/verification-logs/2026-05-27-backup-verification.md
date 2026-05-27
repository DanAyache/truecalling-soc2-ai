# Backup Verification Log

**Date:** 2026-05-27  
**Performed By:** Engineering Lead  
**Verification Type:** Initial restore test (quarterly)  
**SOC 2 Criteria:** A1.2  
**Reference:** Backup Policy §5, Evidence Collection Procedure §4.5

---

## Pre-Test Checks

| Check | Status | Notes |
|-------|--------|-------|
| Supabase daily backup confirmed running | | Screenshot: `supabase-backup-list-2026-05-27.png` |
| PITR enabled | | Screenshot showing PITR window ≥ 7 days |
| PITR retention window | | *(enter actual window shown in dashboard)* |
| Backup retention count | | *(enter number of daily backups shown)* |
| Most recent backup timestamp | | |

---

## Restore Test

| Field | Value |
|-------|-------|
| Backup snapshot used | *(date/time of the backup restored — must be ≥ 48h ago)* |
| Restore start time | |
| Restore end time | |
| Restore target | Staging Supabase project — ID: *(enter project ID)* |
| Restore method | Supabase dashboard → Database → Backups → Restore |

---

## Data Integrity Verification

**Method:** Row count query against key tables on restored database

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

**Screenshot:** `supabase-restore-rowcount-2026-05-27.png`

---

## Result

- [ ] **PASS** — Restore completed successfully; data integrity verified; RPO and RTO targets confirmed achievable
- [ ] **FAIL** — *(describe what failed)*

**RPO target:** 24 hours — confirmed achievable? *(yes/no)*  
**RTO target:** 4 hours — restore completed within target? *(yes/no, actual time: )*

---

## Notes

*(Any observations, warnings, or follow-up actions)*

---

## Remediation (if FAIL)

A failed backup verification is treated as a **P2 incident** per the Backup Policy §5. Open an incident issue in `truecalling-incidents` and link it here.

Incident issue link: *(if applicable)*

---

**Sign-off:** *(Engineering Lead name, date)*  
**Next restore test due:** 2026-08-27 (quarterly)  
**Next full DR drill due:** 2027-05-27 (annual)
