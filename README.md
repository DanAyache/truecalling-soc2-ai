# TrueCalling.ai — SOC 2 Policy & Evidence Repository

This repository is the authoritative source for TrueCalling.ai's SOC 2 Type II compliance program. It contains all security policies, operational procedures, reusable templates, and collected audit evidence.

**Observation period initiated:** 2026-05-27  
Controls are currently under active implementation, remediation, and evidence collection as part of the SOC 2 readiness program.  
**Target audit window:** Q1 2027 (minimum 6-month observation period)  
**Repository visibility:** Private  
**Evidence integrity:** All artifacts are committed to git — timestamps are verifiable via commit history.

---

## Repository Structure

```
truecalling-soc2-ai/
├── policies/               # Security policies (effective 2026-05-27)
├── procedures/             # Operational procedures for onboarding, offboarding, evidence collection
├── findings/               # Findings & Remediation Register (single source of truth for audit findings)
├── evidence/               # Collected audit evidence, organized by category
│   ├── access-control/     # RBAC reviews, MFA enforcement screenshots
│   ├── api-keys/           # API key inventory (F-14), collection worksheet, request work-paper
│   ├── azure/              # Azure control coverage gap analysis (F-16) + Azure evidence
│   ├── backups/            # Backup restore test logs
│   ├── incidents/          # Post-mortems and incident cross-references
│   ├── onboarding/         # Per-person onboarding evidence packages
│   ├── offboarding/        # Per-person offboarding evidence packages
│   ├── risk-register/      # Risk register
│   ├── security-awareness/ # Annual training completion logs
│   ├── security-scans/     # GitHub Actions scan artifacts (Gitleaks, TruffleHog, npm audit)
│   ├── vendor/             # Annual third-party vendor review records
│   └── BOOTSTRAP-CHECKLIST.md  # First-round evidence collection tracker
├── templates/              # Reusable forms for recurring controls
└── .github/
    ├── workflows/security.yml   # SOC2 Security Checks CI (Gitleaks, TruffleHog, npm audit)
    ├── CODEOWNERS               # Mandatory Engineering Lead review on all changes
    └── dependabot.yml           # Automated dependency vulnerability alerts
```

---

## Policies (8 total — all effective 2026-05-27)

| Policy | File | SOC 2 Criteria |
|--------|------|----------------|
| Information Security Policy | [policies/information-security-policy.md](policies/information-security-policy.md) | CC1.1, CC1.2 |
| Access Control Policy | [policies/access-control-policy.md](policies/access-control-policy.md) | CC6.1, CC6.2 |
| Secrets Management Policy | [policies/secrets-management-policy.md](policies/secrets-management-policy.md) | CC6.7 |
| Change Management Policy | [policies/change-management-policy.md](policies/change-management-policy.md) | CC8.1 |
| Incident Response Policy | [policies/incident-response-policy.md](policies/incident-response-policy.md) | CC7.2, CC7.3 |
| Backup Policy | [policies/backup-policy.md](policies/backup-policy.md) | A1.2 |
| Security Awareness Policy | [policies/security-awareness-policy.md](policies/security-awareness-policy.md) | CC9.2 |
| Vendor Management Policy | [policies/vendor-management-policy.md](policies/vendor-management-policy.md) | CC9.2 |

---

## Evidence Quick Reference

| SOC 2 Criteria | Evidence Location |
|----------------|------------------|
| CC6.1 — Logical access controls | `evidence/access-control/` |
| CC6.2 — Provisioning / deprovisioning | `evidence/onboarding/`, `evidence/offboarding/` |
| CC6.7 — Secret & credential protection | `evidence/security-scans/gitleaks/`, `evidence/api-keys/` |
| CC7.1 — Vulnerability identification | `evidence/security-scans/npm-audit/` |
| CC7.2 — Monitoring | GitHub Actions run history, `evidence/incidents/` |
| CC8.1 — Change management | GitHub PR history, `.github/workflows/security.yml` |
| CC9.2 — Security awareness | `evidence/security-awareness/` |
| A1.2 — Backup & recovery | `evidence/backups/verification-logs/` |
| CC3/CC4 — Findings & remediation tracking | `findings/findings-remediation-register.md` |

---

## Recurring Controls Calendar

See [templates/compliance-calendar.md](templates/compliance-calendar.md) for all scheduled controls with due dates.

Key recurring obligations:

| Control | Frequency | Next Due |
|---------|-----------|---------|
| RBAC quarterly access review | Quarterly | 2026-08-27 |
| Backup restore test | Quarterly | 2026-08-27 |
| Security scan evidence export | Monthly | 2026-06-30 |
| API key rotation review | Quarterly | 2026-08-27 |
| Security awareness training | Annual | 2027-05-27 |
| Annual tabletop exercise | Annual | 2026-12-31 |

---

## Automated Security Controls

The `SOC2 Security Checks` workflow ([.github/workflows/security.yml](.github/workflows/security.yml)) runs on every push to `main` and nightly at 02:00 UTC:

| Scan | Tool | Artifact | Criteria |
|------|------|----------|---------|
| Secret scanning | Gitleaks v2.3.9 | `soc2-security-summary-*` (365-day retention) | CC6.7 |
| Secret scanning (entropy) | TruffleHog v3.95.3 | Included in summary artifact | CC6.7 |
| Dependency vulnerabilities | npm audit | `npm-audit-reports` (90-day retention) | CC7.1 |
| PR validation | Custom | CI log | CC8.1 |

---

## For Auditors

Access may be granted as a read-only GitHub collaborator scoped to this repository for the duration of an audit engagement, subject to management approval. Contact **[engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai)** to request access.

Evidence is organized so each SOC 2 Trust Service Criteria maps directly to a folder in `evidence/`. Start with the Evidence Quick Reference table above.

All evidence files are unmodified originals committed to git. The commit timestamp is the authoritative collection date. Do not request evidence outside this repository — if an artifact is not here, it has not been collected yet.

---

*Repository Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Last updated: 2026-05-31*
