# Evidence Collection Procedure

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policies:** Access Control Policy, Incident Response Policy  
**Related Procedures:** Employee Onboarding, Employee Offboarding

---

## 1. Purpose

Define how TrueCalling.ai collects, stores, owns, and presents evidence of security controls to satisfy SOC 2 Type II audit requirements. Evidence must be complete, timestamped, tamper-evident, and retrievable within 48 hours of an auditor request.

---

## 2. SOC 2 Trust Service Criteria Covered

| Criteria | Description | Primary Evidence Categories |
|----------|-------------|----------------------------|
| CC6.1 | Logical access controls | Access reviews, RBAC screenshots, MFA records |
| CC6.2 | Provisioning & deprovisioning | Onboarding/offboarding issues and screenshots |
| CC6.7 | Transmission protection & secret detection | Security scan artifacts (Gitleaks, TruffleHog) |
| CC7.1 | Vulnerability identification | npm audit reports, Dependabot alerts |
| CC7.2 | Monitoring for anomalies | GitHub Actions logs, Supabase audit logs |
| CC8.1 | Change management | PR records, deployment logs, branch protection |
| A1.2 | Backup and recovery | Backup verification logs |

---

## 3. Evidence Storage Structure

All evidence lives in the `evidence/` directory in this repository, organized by category. The folder structure is the canonical location — do not store SOC 2 evidence in personal drives, Slack, or email.

```
evidence/
├── access-control/
│   ├── quarterly-reviews/          # Quarterly RBAC review records
│   └── mfa-enforcement/            # Screenshots of MFA enforcement settings
├── onboarding/                     # One subfolder per person: <name>-<YYYY-MM-DD>
├── offboarding/                    # One subfolder per person: <name>-<YYYY-MM-DD>
├── api-keys/
│   └── openai-keys.md              # Named key log with creation and revocation dates
├── security-scans/
│   ├── gitleaks/                   # Gitleaks SARIF and summary artifacts
│   ├── npm-audit/                  # npm audit JSON reports
│   └── trufflehog/                 # TruffleHog scan summaries
├── backups/
│   └── verification-logs/          # Supabase backup verification records
├── incidents/                      # Post-mortems; cross-reference GitHub issue IDs
├── change-management/              # PR merge records for significant changes
└── vendor/
    └── third-party-reviews/        # Annual vendor access review records
```

Files must be named descriptively with dates:  
`<category>-<YYYY-MM-DD>.<ext>` (e.g., `rbac-review-2026-06-01.pdf`)

---

## 4. Evidence Categories

### 4.1 Access Control Evidence

**What to collect:** Proof that access is scoped correctly and reviewed regularly.

| Evidence Item | Source | Frequency | Collection Method |
|---------------|--------|-----------|------------------|
| RBAC member list — GitHub | GitHub Org → Settings → Members | Quarterly | Export member list as CSV or screenshot |
| RBAC member list — Supabase | Supabase → Project Settings → Team | Quarterly | Screenshot of team members and roles |
| RBAC member list — Vercel | Vercel → Team Settings → Members | Quarterly | Screenshot of team members and roles |
| MFA enforcement setting — GitHub | Org → Settings → Authentication security | Quarterly | Screenshot showing "Require two-factor authentication" enabled |
| MFA enforcement setting — Vercel | Enforced via Google SSO — screenshot SSO config | Quarterly | Screenshot |
| Named API key inventory | `evidence/api-keys/openai-keys.md` | On change | Update log at each provisioning/revocation |

**Storage:** `evidence/access-control/quarterly-reviews/rbac-<YYYY-MM-DD>/`

**How to run the quarterly review:**
1. Pull current member lists from GitHub, Supabase, and Vercel
2. Compare against the approved RBAC table in the Access Control Policy
3. Remove any over-provisioned access within 5 business days
4. Save all screenshots and the comparison notes to the quarterly folder
5. Commit the evidence folder update with message: `chore: quarterly access review <YYYY-MM-DD>`

---

### 4.2 Onboarding Evidence

**What to collect:** Proof that each new hire received correctly-scoped access with MFA before starting work.

| Evidence Item | Source | Collection Method |
|---------------|--------|------------------|
| Onboarding access request issue | GitHub `truecalling-incidents` | Link in evidence index; issue is the primary record |
| MFA enrollment confirmation | Authenticator app setup screen | Screenshot saved on Day 1 |
| Signed Acceptable Use Policy | Returned by new hire | PDF saved to `evidence/onboarding/<name>-<date>/` |
| Security training completion | Training platform certificate or email | Screenshot or PDF |
| Post-provisioning access confirmation | Screenshots of each system showing new user present | Captured by Engineering Lead |

**Storage:** `evidence/onboarding/<full-name>-<YYYY-MM-DD>/`

**Timing:** All artifacts collected on Day 1 or within 1 business day. The onboarding GitHub issue is not closed until all items are confirmed.

---

### 4.3 Offboarding Evidence

**What to collect:** Proof that all access was revoked within the 4-hour SLA.

| Evidence Item | Source | Collection Method |
|---------------|--------|------------------|
| Offboarding issue with timestamps | GitHub `truecalling-incidents` | Link in evidence index |
| GitHub org member list — user absent | GitHub Org → Members | Screenshot after removal |
| Supabase team list — user absent | Supabase → Project Settings → Team | Screenshot after removal |
| Vercel team list — user absent | Vercel → Team Settings → Members | Screenshot after removal |
| OpenAI API key revocation | Platform → API Keys | Screenshot showing key status "Revoked" |
| Google Workspace audit log | Admin Console → Reports → Audit → filter by user | Export last 30 days as CSV |
| GitHub org audit log | Org → Settings → Audit Log → filter by actor | Export filtered log as CSV |
| Credential rotation confirmation | Offboarding issue comments | Timestamped comment per rotated credential |

**Storage:** `evidence/offboarding/<full-name>-<YYYY-MM-DD>/`

**Timing:** Screenshots captured immediately after each revocation step. Logs exported before account deletion. All artifacts committed within 2 business days of departure.

---

### 4.4 GitHub Actions Security Scan Evidence

**What to collect:** Proof that automated security scans run continuously and findings are actioned.

| Evidence Item | Source | Frequency | Collection Method |
|---------------|--------|-----------|------------------|
| Gitleaks scan result | GitHub Actions artifact: `soc2-security-summary-*` | Every push + nightly | Auto-uploaded by workflow; retained 1 year |
| npm audit report | GitHub Actions artifact: `npm-audit-reports` | Every push + nightly | Auto-uploaded by workflow; retained 90 days |
| TruffleHog scan result | GitHub Actions job log | Every push + nightly | Captured in `soc2-security-summary-*` artifact |
| SARIF upload — Security tab | GitHub Org → Security → Code scanning | Continuous | GitHub retains natively; screenshot quarterly |
| Dependabot alert status | GitHub Org → Security → Dependabot | Monthly | Screenshot of open/closed alerts |

**Storage:** GitHub Actions artifacts are the primary record (retained per workflow config). For auditor export, download and store in `evidence/security-scans/<tool>/<YYYY-MM>/`.

**How to retrieve for audit:**
1. GitHub repo → Actions → select `SOC2 Security Checks` workflow
2. Filter by date range matching audit period
3. Download artifacts: `soc2-security-summary-*` and `npm-audit-reports`
4. Save to `evidence/security-scans/` with date-stamped folder

**Finding remediation:** Any High/Critical finding from a scan must be linked to a GitHub issue or PR for remediation. The issue number is recorded in the evidence folder as `findings-<YYYY-MM-DD>.md` with status (open/resolved).

---

### 4.5 Backup Evidence

**What to collect:** Proof that Supabase backups are running and periodically verified as restorable.

| Evidence Item | Source | Frequency | Collection Method |
|---------------|--------|-----------|------------------|
| Automated backup confirmation | Supabase → Project → Database → Backups | Monthly | Screenshot of backup list showing recent successful backup |
| Backup restore test | Supabase restore to staging environment | Quarterly | Document: start time, end time, row count verified, outcome |
| Point-in-time recovery (PITR) status | Supabase dashboard | Quarterly | Screenshot showing PITR enabled and retention window |

**Storage:** `evidence/backups/verification-logs/<YYYY-MM-DD>-backup-verification.md`

**Backup verification log template:**
```
Date: 
Performed by: 
Backup tested: (date of backup snapshot used)
Restore target: (staging project ID)
Verification method: (row count / spot check / full query)
Result: PASS / FAIL
Notes:
```

---

### 4.6 Vulnerability Scan Evidence

**What to collect:** Proof that known vulnerabilities in dependencies are identified and remediated.

| Evidence Item | Source | Frequency | Collection Method |
|---------------|--------|-----------|------------------|
| npm audit report | GitHub Actions artifact | Per push + nightly | Auto-retained in Actions artifacts |
| Dependabot PR history | GitHub repo → Pull Requests (filter: Dependabot) | Monthly | Screenshot of merged/open Dependabot PRs |
| Critical/High CVE resolution record | GitHub issue or PR linked to CVE | Per finding | Issue must reference CVE ID and resolution date |
| SARIF code scanning results | GitHub Security → Code scanning alerts | Continuous | GitHub-native; export quarterly for audit package |

**SLA for remediation:**
- Critical CVE (CVSS ≥ 9.0): remediate within **7 days**
- High CVE (CVSS 7.0–8.9): remediate within **30 days**
- Medium/Low: remediate in next planned release

Unresolved findings beyond SLA must have a documented exception approved by the Engineering Lead, stored in `evidence/security-scans/exceptions/`.

**Storage:** `evidence/security-scans/npm-audit/<YYYY-MM>/` and `evidence/security-scans/dependabot/<YYYY-MM>/`

---

## 5. Retention Schedule

| Evidence Category | Minimum Retention |
|-------------------|------------------|
| Access control (RBAC reviews, MFA records) | 3 years |
| Onboarding records | Duration of employment + 3 years |
| Offboarding records | 3 years from departure |
| GitHub Actions security scan artifacts | 1 year (configured in workflow) |
| npm audit reports | 90 days (GitHub Actions) + monthly exports retained 1 year |
| Backup verification logs | 3 years |
| Vulnerability scan reports | 3 years |
| Incident post-mortems | 3 years |
| Signed policies / AUP | Duration of employment + 3 years |

Files should not be deleted before the minimum retention period. At expiry, Engineering Lead confirms deletion is appropriate before removing.

---

## 6. Ownership

| Evidence Category | Primary Owner | Backup Owner |
|-------------------|---------------|--------------|
| Access control reviews | Engineering Lead | Senior Engineer |
| Onboarding evidence | Engineering Lead | Hiring Manager |
| Offboarding evidence | Engineering Lead | HR / Manager |
| GitHub Actions artifacts | Automated (workflow) | Engineering Lead |
| Backup verification | Engineering Lead | Senior Engineer |
| Vulnerability scans | Automated (workflow) | Engineering Lead |
| Incident evidence | Incident Commander | Engineering Lead |

The Engineering Lead is accountable for the completeness of the evidence folder at all times. During an audit, no evidence should be newly created — it should already exist in `evidence/`.

---

## 7. Audit Preparation

### 7.1 Audit Readiness Checklist (run 4 weeks before audit window)

- [ ] Confirm `evidence/` folder structure matches this procedure
- [ ] Verify all quarterly access reviews for the audit period are present and complete
- [ ] Download GitHub Actions artifacts for the audit period and store locally in `evidence/security-scans/`
- [ ] Confirm all onboarding/offboarding issues from the audit period are closed with evidence attached
- [ ] Confirm backup verification logs cover the audit period (at least quarterly)
- [ ] Confirm all High/Critical CVEs from the audit period have resolution records
- [ ] Review `evidence/incidents/` — every P1/P2 incident has a post-mortem
- [ ] Confirm the `evidence/api-keys/openai-keys.md` log is current
- [ ] Export Supabase and GitHub audit logs for the full audit period
- [ ] Prepare evidence index (see 7.2)

### 7.2 Evidence Index

Before the audit, create `evidence/audit-<YYYY>-index.md` mapping each SOC 2 criteria to its evidence files:

```markdown
# Audit Evidence Index — <Year>

## CC6.1 — Logical Access Controls
- evidence/access-control/quarterly-reviews/rbac-2026-03-01/
- evidence/access-control/quarterly-reviews/rbac-2026-06-01/
- evidence/access-control/mfa-enforcement/

## CC6.2 — Provisioning & Deprovisioning
- evidence/onboarding/ (list names and dates)
- evidence/offboarding/ (list names and dates)

## CC6.7 — Secret & Credential Protection
- evidence/security-scans/gitleaks/
- evidence/security-scans/trufflehog/

## CC7.1 — Vulnerability Identification
- evidence/security-scans/npm-audit/
- evidence/security-scans/dependabot/

## CC7.2 — Monitoring
- GitHub Actions run history (link to repo → Actions)
- evidence/incidents/

## CC8.1 — Change Management
- GitHub PR history (link to repo → Pull Requests)
- evidence/change-management/

## A1.2 — Backup & Recovery
- evidence/backups/verification-logs/
```

### 7.3 Auditor Access

Auditors are granted **read-only** access to the evidence folder via a temporary GitHub collaborator invite scoped to this repository only. Access is revoked within 5 business days of audit completion, following the offboarding procedure for contractor access.

---

## 8. Evidence Integrity

- All evidence in `evidence/` is committed to git, providing an immutable timestamp via commit history
- Screenshots must be unedited originals (no cropping that removes timestamps or usernames)
- Log exports must be unmodified CSV/JSON files from the source system
- If an evidence file needs to be corrected, add a new file with a note explaining the correction — do not overwrite the original

---

*Procedure Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
