# Backup Verification Log — 2026-05-27 (Test Not Performed)

**Originally Planned:** 2026-05-27
**Current Status:** ⚠️ **Not Performed — Deferred Pending Access**
**Last Updated:** 2026-06-01
**Owner:** Engineering Lead
**SOC 2 Criteria:** A1.2 (Recovery from Disruption)
**Related Risk:** [R-007](../../risk-register/risk-register-2026.md) — rated High (12), Open

---

## 1. Status Statement

The first quarterly Supabase backup restore test for the current SOC 2 observation period was scheduled for 2026-05-27 and **has not been performed** as of the Last Updated date above.

This file is retained at the path already referenced by the [Compliance Calendar](../../../templates/compliance-calendar.md) (Q2 2026 row, Open Item #4) and [Bootstrap Checklist](../../BOOTSTRAP-CHECKLIST.md) (Section 5) in order to:

- Transparently document the open gap rather than represent the control as executed
- Preserve the cross-reference paths used by other governance documents
- Provide the auditable trail showing the deferral was acknowledged, tracked, and assigned a target close date

This is **not** a completed verification log. A completed log will replace this file at the same path once the test is performed.

## 2. Reason for Deferral

Performing the restore test requires:

- Supabase project administrator access to initiate a backup restore to the staging project
- A scheduled maintenance window in which the staging project can be used as a restore target
- Connectivity from a controlled environment to run the post-restore data integrity query

The required administrative access and scheduling prerequisites to perform the restore test have not yet been completed. The blocker is tracked under [Compliance Calendar Open Item #4](../../../templates/compliance-calendar.md) and Risk Register entry [R-007](../../risk-register/risk-register-2026.md).

## 3. Residual Risk

Until the first restore test is completed:

- Recoverability of production data from Supabase backups is **unverified** for the current observation period
- RPO 24h and RTO 4h targets defined in [Backup Policy](../../../policies/backup-policy.md) §5 are **assumed but not empirically confirmed**
- A latent backup configuration failure could go undetected until first invocation in a real incident

The Engineering Lead has accepted this residual risk for the deferral window via the risk register entry R-007.

## 4. Tracking and Cross-References

| Reference | Location | Status |
|-----------|----------|--------|
| Compliance Calendar — Q2 2026 control | [compliance-calendar.md](../../../templates/compliance-calendar.md) | ⏳ Pending |
| Compliance Calendar — Open Item #4 | [compliance-calendar.md](../../../templates/compliance-calendar.md) | ⏳ Blocked — access pending |
| Risk Register — R-007 | [risk-register-2026.md](../../risk-register/risk-register-2026.md) | Open, **12 High**, review due 2026-06-30 |
| Backup Policy reference | [backup-policy.md](../../../policies/backup-policy.md) §5 | — |

## 5. Target Completion

| Milestone | Date |
|-----------|------|
| Target close date for R-007 | **2026-06-30** |
| Next quarterly restore test (per Q3 schedule) | 2026-08-27 |
| Next annual DR drill | 2027-05-27 |

## 6. Mitigation Plan

1. Complete the Supabase administrator access workflow that is currently blocking the test
2. Execute the test per the [backup-verification-log template](../../../templates/backup-verification-log.md), including pre-test checks, restore procedure, data integrity verification, and RPO/RTO assessment
3. **Replace this gap-acknowledgement file with the completed verification log at the same path** (overwrite, not append)
4. Commit with message: `chore: first backup restore test <YYYY-MM-DD>` using the actual test date
5. Update Risk Register R-007 — move to Mitigated/Closed once residual risk is acceptable
6. Update Compliance Calendar — mark the Q2 2026 control completed and close Open Item #4

## 7. No Sign-Off

No sign-off is recorded because no test was performed. The acknowledgement of this gap is captured under R-007 in the Risk Register. A signed verification log will be committed at this path once the test is performed.

---

*This file documents an evidence gap, not a completed control. Evidence retention: 3 years per Backup Policy §6.*
*Owner: Engineering Lead — engineering-lead@truecalling.ai*
