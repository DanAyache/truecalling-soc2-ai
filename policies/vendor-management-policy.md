# Vendor Management Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policies:** Access Control Policy, Secrets Management Policy, Acceptable Use Policy  
**SOC 2 Criteria:** CC9.2 (Vendor and Third-Party Management)

---

## 1. Purpose

Ensure that third-party vendors and service providers who access, process, or store TrueCalling.ai data or systems are assessed for security risk before onboarding and monitored throughout the relationship — preventing vendor-introduced vulnerabilities from compromising customer data or SOC 2 compliance.

## 2. Scope

All third-party vendors, service providers, SaaS tools, APIs, and contractors that:
- Process, store, or transmit TrueCalling.ai customer data or production data
- Have access to TrueCalling.ai systems (GitHub, Supabase, Vercel, Google Workspace)
- Provide infrastructure or security-relevant services

Personal productivity tools that do not handle company or customer data are out of scope.

---

## 3. Vendor Classification

| Tier | Definition | Examples | Due Diligence Required |
|------|-----------|---------|----------------------|
| **Critical** | Processes or stores customer data; required for production operation | Supabase, Vercel, OpenAI, Anthropic, Google Workspace | Full review: SOC 2 report, DPA, annual review |
| **Significant** | Access to internal systems but no direct customer data; or handles employee data | GitHub, 1Password/Bitwarden, Slack | Security assessment, DPA if applicable, annual review |
| **Standard** | No access to production systems or customer data | Design tools, analytics without PII, project management | Basic privacy terms review |

---

## 4. Current Critical Vendors

| Vendor | Service | Tier | SOC 2 / Certifications | DPA in Place | Next Review |
|--------|---------|------|----------------------|-------------|-------------|
| Supabase | PostgreSQL database, Auth, Storage | Critical | SOC 2 Type II | Covered by Terms of Service / DPA | Annual |
| Vercel | Application hosting, edge functions | Critical | SOC 2 Type II | Covered by Terms of Service / DPA | Annual |
| OpenAI | AI model API | Critical | SOC 2 Type II | OpenAI API DPA available | Annual |
| Anthropic | AI model API (Claude) | Critical | Security controls published | Anthropic API Terms | Annual |
| Google Workspace | Email, SSO, Drive | Critical | SOC 2 Type II, ISO 27001 | Google Workspace DPA | Annual |
| GitHub | Source code, CI/CD | Significant | SOC 2 Type II | GitHub DPA | Annual |
| 1Password / Bitwarden | Password manager | Significant | SOC 2 Type II | DPA available | Annual |

This table is the authoritative vendor inventory. The Engineering Lead updates it when vendors are added or removed.

---

## 5. Pre-Onboarding Due Diligence

Before any new **Critical** or **Significant** vendor is approved for use, the Engineering Lead must complete:

### 5.1 Security Assessment Checklist

- [ ] Does the vendor have a current SOC 2 Type II report (or equivalent: ISO 27001, CSA STAR)?
- [ ] Is the vendor's data residency region acceptable (preferably US or EU with SCCs)?
- [ ] Does the vendor offer a Data Processing Agreement (DPA)?
- [ ] Has the vendor had a publicly disclosed breach in the last 24 months? If yes — what was the response?
- [ ] Does the vendor have a published vulnerability disclosure / responsible disclosure policy?
- [ ] Does the vendor support MFA on their admin console?
- [ ] Are encryption at rest and in transit confirmed (AES-256 / TLS 1.2+)?

For **Critical** vendors: all checklist items must be addressed before approval.  
For **Significant** vendors: items 1, 3, 4 are required at minimum.

### 5.2 Approval

New vendor onboarding requires **Engineering Lead approval** documented in a GitHub issue or email trail. Employees may not begin using a new vendor that handles company or customer data without this approval.

The [Acceptable Use Policy §3.6](acceptable-use-policy.md) prohibits use of unapproved SaaS tools for company data.

---

## 6. Contract and DPA Requirements

For **Critical** vendors that process customer data, TrueCalling.ai must have:

1. **Data Processing Agreement (DPA)** — specifying data types processed, purposes, retention, and deletion obligations
2. **Sub-processor list** — vendor must disclose and notify of changes to their sub-processors
3. **Breach notification clause** — vendor must notify TrueCalling.ai within 72 hours of a confirmed breach affecting TrueCalling.ai data
4. **Termination / data deletion clause** — vendor must delete or return data within 30 days of contract termination

Where a vendor provides a standard DPA as part of their Terms of Service (e.g., Supabase, Vercel, OpenAI), that DPA must be reviewed and accepted by the Engineering Lead and recorded in `evidence/vendor/third-party-reviews/`.

---

## 7. Vendor Access Provisioning

When a vendor requires access to TrueCalling.ai systems:

- Follow the Access Control Policy RBAC table — vendor access is granted at the minimum required permission level
- Vendor accounts are provisioned with a defined expiration date matching the contract term
- Vendor accounts must not be shared among multiple individuals at the vendor — each vendor contact gets a named account
- Vendor API keys follow the Secrets Management Policy naming convention: `<vendor>-<purpose>-<env>`
- All vendor access is logged in the quarterly access review

---

## 8. Ongoing Monitoring

### 8.1 Quarterly Access Review

At each quarterly access review, the Engineering Lead verifies:
- Active vendor accounts in each system match current vendor engagements
- No vendors have access beyond their approved scope
- API keys and tokens for vendor integrations are within rotation schedule

### 8.2 Annual Vendor Security Review

Once per year, for each **Critical** vendor, the Engineering Lead:
1. Downloads the vendor's latest SOC 2 report (or equivalent) and reviews for exceptions
2. Confirms DPA is still current and covers current data processing activities
3. Reviews any vendor-disclosed security incidents from the prior year
4. Confirms breach notification contact information is current
5. Documents the review in `evidence/vendor/third-party-reviews/<vendor>-<YYYY>.md`

### 8.3 Vendor Security Alerts

The Engineering Lead monitors vendor security notifications (email lists, status pages, CVE feeds) for:
- Data breaches affecting TrueCalling.ai data
- Critical CVEs in vendor software used in production
- Changes to vendor DPAs or sub-processors

Vendor-disclosed breaches involving TrueCalling.ai data are treated as minimum **P2 incidents** under the Incident Response Policy.

---

## 9. Vendor Offboarding

When a vendor relationship ends:

1. **Revoke all access** within 24 hours of contract termination — follow the same steps as employee offboarding for any system accounts the vendor held
2. **Rotate any secrets** the vendor had access to (API keys, webhook secrets) within 24 hours
3. **Confirm data deletion** — request written confirmation from the vendor that TrueCalling.ai data has been deleted per the contract terms. Retain the confirmation in `evidence/vendor/third-party-reviews/`
4. **Remove from vendor inventory** table in this policy within 5 business days

Vendor offboarding is logged as a comment on the vendor's GitHub issue (if one exists) or as a standalone note in `evidence/vendor/third-party-reviews/`.

---

## 10. Evidence

| Evidence Item | Location | Retention |
|---------------|----------|-----------|
| Pre-onboarding due diligence checklist | `evidence/vendor/third-party-reviews/<vendor>-onboarding-<date>.md` | Duration of relationship + 3 years |
| Vendor SOC 2 reports / certifications | `evidence/vendor/third-party-reviews/<vendor>-<YYYY>.md` | 3 years |
| Accepted DPAs | `evidence/vendor/third-party-reviews/<vendor>-dpa-<YYYY>.pdf` | Duration of relationship + 3 years |
| Annual vendor security review | `evidence/vendor/third-party-reviews/<vendor>-annual-<YYYY>.md` | 3 years |
| Vendor offboarding confirmation | `evidence/vendor/third-party-reviews/<vendor>-offboarding-<date>.md` | 3 years |

---

## 11. Exceptions

Using a vendor that does not meet the requirements of this policy (e.g., no SOC 2 report, no DPA) requires:
- Written risk justification from the Engineering Lead
- Compensating controls documented (e.g., data minimization, additional encryption)
- Time-bounded exception (maximum 90 days) with a remediation plan
- Exception stored in `evidence/vendor/exceptions/`

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
