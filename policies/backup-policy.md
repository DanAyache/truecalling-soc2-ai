# Backup Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Procedures:** Evidence Collection Procedure  
**SOC 2 Criteria:** A1.2 (System Availability — Backup and Recovery)

---

## 1. Purpose

Ensure that TrueCalling.ai customer data and system state can be recovered from any failure scenario with defined recovery objectives, and that backup integrity is verified on a regular schedule.

## 2. Scope

All production data and system configuration managed by TrueCalling.ai, including:
- Supabase PostgreSQL database (primary data store)
- Supabase Storage (file/object storage, if used)
- Vercel environment configuration
- GitHub source code and Actions configuration

---

## 3. Recovery Objectives

| Objective | Target |
|-----------|--------|
| **RPO** (Recovery Point Objective) — maximum acceptable data loss | 24 hours |
| **RTO** (Recovery Time Objective) — maximum acceptable downtime | 4 hours |

If an incident threatens these targets, it is automatically treated as P1 under the Incident Response Policy.

---

## 4. Backup Requirements

### 4.1 Supabase Database

| Requirement | Standard |
|-------------|----------|
| Automated daily backups | Enabled — Supabase performs daily snapshots automatically on Pro plan and above |
| Point-in-Time Recovery (PITR) | Enabled — minimum 7-day PITR window |
| Backup retention | Minimum 30 days of daily snapshots |
| Backup encryption | Enforced by Supabase at rest (AES-256) |
| Geographic redundancy | Supabase-managed; confirm primary region at project creation |

The service role key and database connection strings required for recovery are stored exclusively in Vercel environment variables and the company password manager — never in plaintext files or source code.

### 4.2 Supabase Storage

If object storage is used in production:
- Supabase Storage is backed up as part of the project backup
- Any critical user-uploaded files should be verified present after a restore test

### 4.3 Vercel Configuration

Vercel does not provide native config backups. Configuration is protected by:
- Infrastructure-as-code approach: all environment variable names (not values) documented in the repo
- Environment variable values stored in the company password manager as a secondary record
- Deployment history retained in Vercel for 90 days — allows rollback to any prior deployment

### 4.4 GitHub Source Code

GitHub repositories are the authoritative backup of all source code. No additional backup is required for code. However:
- The repository must not have force-push to main enabled (enforced via branch protection)
- Critical configuration files (`.github/workflows/`, `policies/`, `procedures/`) are protected by CODEOWNERS

---

## 5. Backup Verification

Backups that are never tested are not reliable. The following verification schedule is mandatory:

| Verification Activity | Frequency | Owner | Evidence Location |
|-----------------------|-----------|-------|------------------|
| Confirm daily backup ran successfully | Monthly | Engineering Lead | `evidence/backups/verification-logs/` |
| Confirm PITR is enabled and window is ≥ 7 days | Quarterly | Engineering Lead | `evidence/backups/verification-logs/` |
| Restore test to staging environment | Quarterly | Engineering Lead | `evidence/backups/verification-logs/` |
| Full DR drill (restore + application smoke test) | Annually | Engineering Lead | `evidence/backups/verification-logs/` |

**Restore test procedure (quarterly):**
1. Navigate to Supabase dashboard → Project → Database → Backups
2. Select a backup from at least 48 hours prior
3. Restore to the staging Supabase project
4. Run a row count query against key tables to verify data integrity
5. Document in the backup verification log (see template in Evidence Collection Procedure §4.5)
6. Commit the log to `evidence/backups/verification-logs/`

A backup verification that fails must be treated as a P2 incident under the Incident Response Policy and escalated to the Engineering Lead immediately.

---

## 6. Backup Security

- Supabase backup snapshots are encrypted at rest by the provider (AES-256)
- Access to the Supabase dashboard (where backups are managed) is restricted to Engineering Lead and Senior Engineers per the Access Control Policy RBAC table
- Backup restore operations in production require Engineering Lead approval and must be documented

---

## 7. Retention Schedule

| Data Type | Retention Period |
|-----------|-----------------|
| Supabase daily snapshots | 30 days (Supabase-managed) |
| PITR logs | 7 days (Supabase-managed, minimum) |
| Backup verification logs | 3 years (stored in `evidence/`) |
| Restore test records | 3 years (stored in `evidence/`) |

---

## 8. Exceptions

Any deviation from this policy (e.g., PITR disabled, retention below 30 days) requires written approval from the Engineering Lead, a documented risk justification, and a compensating control. Exceptions are stored in `evidence/backups/exceptions/`.

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
