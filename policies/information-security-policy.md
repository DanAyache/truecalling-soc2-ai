# Information Security Policy

**Organization:** TrueCalling.ai
**Version:** 1.0
**Effective Date:** 2026-06-01
**Owner:** Engineering Lead
**Review Cycle:** Annual
**SOC 2 Criteria:** CC1.1, CC1.2, CC1.3, CC1.4, CC1.5, CC2.1, CC2.2, CC2.3, CC3.1, CC3.2, CC3.3, CC3.4, CC5.1, CC5.2, CC5.3

---

## 1. Purpose

This Information Security Policy (ISP) is the umbrella policy that establishes TrueCalling.ai's commitment to protecting the confidentiality, integrity, and availability of customer data, AI model assets, and the systems that process them. It defines the security governance framework that all subordinate policies, procedures, and controls operate under, and it documents management's commitment to implement and maintain controls aligned with the SOC 2 Trust Services Criteria.

All other policies in the [policies/](.) directory derive their authority from this document.

## 2. Scope

This policy applies to:

- **People:** All employees, contractors, founders, and third parties who access TrueCalling.ai systems or data
- **Systems:** Production and staging environments, including Supabase (database and authentication), Vercel (hosting and edge functions), GitHub (source code and CI/CD), Google Workspace (identity and email), and any SaaS tool used in production operations
- **Data:** Customer data, call records, AI model outputs, authentication credentials, source code, audit evidence, and any other data classified as Confidential or Restricted
- **Lifecycle:** All phases — design, build, deploy, operate, and decommission

This ISP applies regardless of location, device, or network from which TrueCalling.ai systems or data are accessed.

---

## 3. Roles and Responsibilities

| Role | Responsibilities |
|------|-----------------|
| **Engineering Lead (Policy Owner)** | Owns this ISP and all subordinate policies. Approves exceptions. Chairs the annual policy review. Maintains the SOC 2 evidence program and serves as the primary point of contact for auditors. Acts as Incident Commander for P1/P2 incidents. |
| **Authorized Personnel** | Personnel formally delegated specific security responsibilities by the Engineering Lead. Implement and enforce controls defined in subordinate policies. Lead delegated control areas (e.g., backup verification, access reviews). Serve as Incident Commanders for P3/P4 incidents when designated. |
| **All Engineers and Contractors** | Comply with all policies. Report suspected security incidents per the [Incident Response Policy](incident-response-policy.md). Complete annual security awareness training. Acknowledge the [Acceptable Use Policy](acceptable-use-policy.md). |
| **Vendors and Third Parties** | Meet the security requirements defined in the [Vendor Management Policy](vendor-management-policy.md). Provide attestation or audit reports as requested. |

The Engineering Lead is the accountable party for the overall security program. Operational responsibilities may be delegated but accountability cannot.

---

## 4. Policy Framework

TrueCalling.ai's information security program is governed by this ISP and implemented through eight subordinate policies. Each subordinate policy elaborates on a specific control domain and inherits the scope, principles, and governance defined here.

| # | Subordinate Policy | Control Domain | Primary SOC 2 Criteria |
|---|-------------------|---------------|----------------------|
| 1 | [Access Control Policy](access-control-policy.md) | Logical access, RBAC, authentication, provisioning/deprovisioning | CC6.1, CC6.2, CC6.3 |
| 2 | [Acceptable Use Policy](acceptable-use-policy.md) | Permitted use of systems, data, and credentials by personnel | CC1.1, CC1.4, CC2.2 |
| 3 | [Backup Policy](backup-policy.md) | Backup frequency, retention, restore testing, RPO/RTO | A1.2, A1.3 |
| 4 | [Change Management Policy](change-management-policy.md) | Code review, deployment, infrastructure change control | CC8.1 |
| 5 | [Incident Response Policy](incident-response-policy.md) | Detection, triage, containment, post-mortem, disclosure | CC7.1, CC7.2, CC7.3, CC7.4, CC7.5 |
| 6 | [Secrets Management Policy](secrets-management-policy.md) | Storage, rotation, scoping, and revocation of secrets | CC6.1, CC6.7 |
| 7 | [Security Awareness Policy](security-awareness-policy.md) | Annual training, role-specific training, attestation | CC1.4, CC2.2, CC2.3 |
| 8 | [Vendor Management Policy](vendor-management-policy.md) | Vendor due diligence, contractual security, ongoing review | CC9.2 |

### 4.1 Hierarchy and Precedence

This ISP takes precedence over all subordinate policies. Where a subordinate policy is silent on a topic, the principles defined in this ISP apply by default. Where a subordinate policy conflicts with this ISP, this ISP controls and the subordinate policy must be reconciled at the next review.

### 4.2 Supporting Documents

The policy framework is supported by:

- **Procedures** in [procedures/](../procedures/) — operational how-to documents that implement policy requirements
- **Templates** in [templates/](../templates/) — standardized forms for recurring controls (RBAC reviews, backup verification, onboarding/offboarding, incidents)
- **Evidence** in [evidence/](../evidence/) — tamper-evident records of control execution, retained per defined audit evidence retention requirements
- **Compliance Calendar** at [templates/compliance-calendar.md](../templates/compliance-calendar.md) — authoritative schedule of recurring control obligations

### 4.3 Guiding Principles

All TrueCalling.ai security controls are designed around the following principles:

- **Least Privilege** — Access is denied by default and granted explicitly per the [Access Control Policy](access-control-policy.md)
- **Defense in Depth** — Multiple layers of preventive and detective controls
- **Evidence by Default** — Every control execution produces auditable evidence retained in [evidence/](../evidence/)
- **Separation of Duties** — No single person may both approve and implement a privileged change, per the [Change Management Policy](change-management-policy.md)
- **Continuous Monitoring** — Security scanning, dependency review, and access review run on a defined cadence and are not skipped

---

## 5. Compliance and Exceptions

### 5.1 Compliance

Compliance with this ISP and all subordinate policies is **mandatory** for all personnel within scope. Violations may result in:

- Access revocation
- Disciplinary action up to and including termination
- Legal action for willful misconduct or breach of contract
- Reporting to law enforcement or regulators where required by law

### 5.2 Monitoring

The Engineering Lead monitors compliance through:

- Quarterly access reviews ([Access Control Policy](access-control-policy.md) §8)
- Quarterly backup verification tests ([Backup Policy](backup-policy.md))
- Continuous GitHub Actions security scans (Gitleaks, TruffleHog, npm audit)
- Annual policy review (this document, §6)
- Audit evidence collection per the Compliance Calendar

Any control that is missed or fails its SLA must be logged as an issue in the private `truecalling-incidents` GitHub repository with a remediation plan and due date.

### 5.3 Exceptions

Any deviation from this ISP or a subordinate policy requires a documented exception. Exceptions are granted only when:

- The exception is necessary for a defined business reason
- A compensating control is in place that achieves a similar security outcome
- The risk of the exception has been assessed and is acceptable
- The exception has a defined expiration date (maximum 12 months)

**Process:**

1. The requester opens an exception request in the private `truecalling-incidents` repository, including: the policy section to be excepted, business justification, compensating control, risk assessment, and proposed expiration date
2. The Engineering Lead reviews and approves or denies in writing within a reasonable timeframe, normally within 5 business days
3. Approved exceptions are tracked in [evidence/exceptions/](../evidence/exceptions/) and re-reviewed at expiration
4. Exceptions are reported as a known finding in any subsequent SOC 2 readiness review

Exceptions to controls required by SOC 2 Trust Services Criteria may impact the audit opinion and require explicit acknowledgement from the Engineering Lead.

---

## 6. Review and Approval

### 6.1 Review Cycle

This ISP is reviewed and re-approved **annually**, and additionally upon any of the following triggers:

- A material change to TrueCalling.ai's business, systems, or data scope
- A SOC 2 audit finding that affects governance
- A P1 incident with policy-level root cause
- A change to applicable laws or regulations (e.g., a new jurisdiction)

The next scheduled review date is recorded in the [Compliance Calendar](../templates/compliance-calendar.md) under "Annual Controls."

### 6.2 Review Procedure

At each review the Engineering Lead must:

1. Confirm that all subordinate policies are still in effect, current, and consistent with this ISP
2. Reconcile any conflicts surfaced since the last review
3. Update SOC 2 criteria mappings if the criteria, scope, or audit period has changed
4. Update version, effective date, and next review date
5. Commit the revised policy with a conventional commit message: `policy: annual ISP review <YYYY-MM-DD>`
6. Communicate material changes to all personnel via email and acknowledgement tracking

### 6.3 Approval

This policy is approved by the Engineering Lead on behalf of TrueCalling.ai management. Subordinate policies must also be reviewed annually under the same process.

| Field | Value |
|-------|-------|
| **Approved By** | Engineering Lead |
| **Approval Date** | 2026-06-01 |
| **Next Review Due** | 2027-06-01 |
| **Version History** | Tracked via `git log policies/information-security-policy.md` |

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
*SOC 2 Criteria: CC1 (Control Environment), CC2 (Communication and Information), CC3 (Risk Assessment), CC5 (Control Activities)*
