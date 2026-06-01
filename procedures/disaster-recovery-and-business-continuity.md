# Disaster Recovery and Business Continuity Procedure

**Organization:** TrueCalling.ai
**Version:** 1.0
**Effective Date:** 2026-06-01
**Owner:** Engineering Lead
**Review Cycle:** Annual, and ad-hoc following any P1/P2 disruption or material change to backup architecture
**Related Policies:** [Backup Policy](../policies/backup-policy.md), [Incident Response Policy](../policies/incident-response-policy.md), [Information Security Policy](../policies/information-security-policy.md), [Access Control Policy](../policies/access-control-policy.md), [Vendor Management Policy](../policies/vendor-management-policy.md), [Secrets Management Policy](../policies/secrets-management-policy.md), [Change Management Policy](../policies/change-management-policy.md)
**SOC 2 Criteria:** A1.2, A1.3 (System availability and recovery), CC7.4, CC7.5 (System continuity and incident response), CC2.3 (External communication), CC9.1 (Risk mitigation activities)

---

## 1. Purpose

This procedure documents how TrueCalling.ai recovers customer-facing service, customer data, and internal operations following a disruption. It implements the Recovery Point Objective and Recovery Time Objective defined in the [Backup Policy](../policies/backup-policy.md) §3, coordinates with the [Incident Response Policy](../policies/incident-response-policy.md), and addresses residual risks recorded in the [Risk Register](../evidence/risk-register/risk-register-2026.md) — in particular R-001 (key person dependency), R-007 (untested backup restore), R-008 (customer data exposure), R-010 (LLM provider outage), R-013 (Twilio credential compromise), and R-014 (supply chain).

## 2. Scope

This procedure applies to disruptions affecting:

- **Customer-facing services** — agent calls, transcripts, dashboard
- **Production data** — Supabase PostgreSQL database, Supabase Storage, Vercel-hosted application state
- **Internal operations** — GitHub source code and CI/CD, Google Workspace, the `truecalling-incidents` private GitHub repository, and the company password manager
- **Third-party dependencies** — LLM providers (Anthropic, OpenAI), Twilio, FullEnrich, and other vendors documented under the [Vendor Management Policy](../policies/vendor-management-policy.md)

Out of scope: routine deployment rollbacks (handled by [Change Management Policy](../policies/change-management-policy.md)) and personal device loss not affecting production systems.

---

## 3. Definitions

| Term | Definition | Source |
|------|-----------|--------|
| **RPO** (Recovery Point Objective) | Maximum acceptable data loss measured backwards from disruption — **24 hours** | [Backup Policy §3](../policies/backup-policy.md) |
| **RTO** (Recovery Time Objective) | Maximum acceptable downtime measured forwards from disruption — **4 hours** | [Backup Policy §3](../policies/backup-policy.md) |
| **DR** (Disaster Recovery) | Technical restoration of systems and data | This procedure |
| **BCP** (Business Continuity Plan) | Sustained operation of business-critical functions during disruption | This procedure |
| **PITR** (Point-in-Time Recovery) | Database recovery to a chosen timestamp within the configured window | Supabase platform |
| **Incident Commander** | Person responsible for coordinating response, per the [Incident Response Policy](../policies/incident-response-policy.md) | [Incident Response Policy](../policies/incident-response-policy.md) |

> No Maximum Tolerable Period of Disruption (MTPD) is defined in this procedure. Recovery is measured exclusively against the RPO and RTO defined in [Backup Policy §3](../policies/backup-policy.md). Establishing a separate MTPD may be considered at a future review.

---

## 4. Roles and Responsibilities

| Role | DR/BCP Responsibilities |
|------|------------------------|
| **Engineering Lead** | Acts as Incident Commander for P1/P2 disruptions per [Incident Response Policy](../policies/incident-response-policy.md); authorises restore execution; performs technical recovery steps; signs off post-disruption review |
| **SOC 2 Program Lead** | Tracks evidence of recovery activity; coordinates external communication with customers, auditors, and prospective auditors per [Information Security Policy §3](../policies/information-security-policy.md); ensures the post-disruption review record is committed |
| **Authorized Personnel** | Execute delegated recovery steps when designated by the Engineering Lead; act as Incident Commander for P3/P4 disruptions when assigned |
| **All Engineers and Contractors** | Report observed disruptions per the [Incident Response Policy](../policies/incident-response-policy.md); follow Incident Commander direction during active recovery |

The Engineering Lead is accountable for technical recovery. The SOC 2 Program Lead is accountable for the evidence trail. Operational steps may be delegated; accountability cannot.

---

## 5. Recovery Objectives

| Objective | Target | Source |
|-----------|--------|--------|
| RPO | 24 hours | [Backup Policy §3](../policies/backup-policy.md) |
| RTO | 4 hours | [Backup Policy §3](../policies/backup-policy.md) |

A disruption that threatens either of these objectives is automatically classified as **P1** under the [Incident Response Policy](../policies/incident-response-policy.md).

---

## 6. Communication and Escalation

### 6.1 Internal Coordination Channel

The designated internal channel for DR/BCP coordination is **`#security-incidents`** in the TrueCalling.ai Slack workspace, consistent with the [Incident Response Policy](../policies/incident-response-policy.md), [Acceptable Use Policy](../policies/acceptable-use-policy.md), [Security Awareness Policy](../policies/security-awareness-policy.md), and [Change Management Policy](../policies/change-management-policy.md), all of which reference the same channel as the canonical incident channel. Slack is documented as an active TrueCalling.ai vendor in the [Vendor Management Policy](../policies/vendor-management-policy.md).

> **Classification of this control:** INFERRED-strong. The channel is referenced consistently as the canonical incident channel in four policies and Slack itself is treated as an active vendor under the Vendor Management Policy. However, operational screenshot evidence of the channel's existence has not yet been collected.
>
> **Follow-up action:** Capture a screenshot of `#security-incidents` and store at `evidence/incidents/security-incidents-channel-<YYYY-MM-DD>.png` during a future evidence collection cycle to upgrade this control to VERIFIED. Until that screenshot is collected, the channel is treated as authoritative on the basis of policy and vendor documentation.

[Incident Response Policy §4.1](../policies/incident-response-policy.md) lists the `#security-incidents` Slack channel and the `truecalling-incidents` private GitHub repository as equivalent reporting destinations; either may serve as the active coordination channel during a disruption.

### 6.2 External Communication (Customers)

Customer communication during a disruption is conducted by email through Google Workspace. No public status page is currently deployed.

> **Classification:** ASSUMED. No customer-availability notification SLA, customer communication template library, or canonical customer distribution list is currently documented in the repository. The [Incident Response Policy §5](../policies/incident-response-policy.md) defines a 72-hour customer notification SLA, but only for *confirmed breach with unauthorized data access* — not for availability disruption. The [Incident Response Policy §4.7](../policies/incident-response-policy.md) defines a 5-business-day internal post-mortem window, but does not specify a customer-facing summary timing.
>
> **Follow-up action:** Establish customer-availability notification SLAs (first notification, status update cadence, restoration notice, post-incident summary), draft customer communication templates, and document the customer distribution list under `templates/dr-customer-communication-templates.md` before the next annual DR drill. Pending that work, customer communication during a disruption is at the discretion of the Engineering Lead with SOC 2 Program Lead coordination.

### 6.3 External Communication (Vendors and Regulators)

- Affected vendors (Supabase, Vercel, Anthropic, OpenAI, Twilio, FullEnrich, Google Workspace) are contacted via their support portals as required for recovery
- Regulatory notification, if applicable under GDPR or state privacy laws, is coordinated with legal counsel per [Incident Response Policy §5](../policies/incident-response-policy.md)

> **Classification:** The legal-counsel coordination requirement is VERIFIED per IR Policy §5. The procedural split between Engineering Lead and SOC 2 Program Lead for the regulatory determination is INFERRED from the [ISP §3](../policies/information-security-policy.md) role model and is not explicitly stated in the IR Policy.

### 6.4 Escalation Path

The escalation path follows the [Incident Response Policy](../policies/incident-response-policy.md) §5. The Engineering Lead serves as Incident Commander for P1/P2; if the Engineering Lead is unreachable, see §8.8 (Key Personnel Unavailable).

---

## 7. Recovery Procedures

### 7.1 Supabase Database Recovery

Primary tools: Supabase dashboard → Project → Database → Backups (daily snapshots) and Point-in-Time Recovery.

**Procedure:**

1. Engineering Lead authenticates to Supabase with MFA
2. Identify the most recent uncompromised backup point — typically a snapshot from before the disruption, or a PITR timestamp within the configured 7-day minimum window per [Backup Policy §4.1](../policies/backup-policy.md)
3. Restore to the staging Supabase project first to verify integrity, unless the disruption is the loss of staging itself
4. Run row-count and integrity queries against key tables
5. If integrity verified, restore to production
6. Validate application connectivity from Vercel
7. Log the restoration in `evidence/backups/verification-logs/<YYYY-MM-DD>-restore.md`

> **Classification:** Steps derive from the [Backup Policy §5](../policies/backup-policy.md) restore test procedure (VERIFIED in policy). The procedure has not yet been operationally executed in the current observation period per R-007 in the [Risk Register](../evidence/risk-register/risk-register-2026.md) — restore *capability* is INFERRED from Supabase platform documentation pending the first quarterly restore test.

### 7.2 Supabase Storage Recovery

If object storage is in production use:

1. Restoration is bundled with the Supabase project backup (see §7.1)
2. Spot-check the presence of critical user-uploaded files after restore
3. Document outcome alongside the §7.1 restoration log

### 7.3 Vercel Application Recovery

Vercel does not provide native configuration backup. Recovery uses deployment history (retained 90 days per [Backup Policy §4.3](../policies/backup-policy.md)).

1. From the Vercel dashboard, identify the last known-good deployment
2. Promote that deployment, or redeploy from a specific Git commit
3. Verify environment variables — values are also stored in the company password manager as a secondary record per [Backup Policy §4.3](../policies/backup-policy.md)
4. Run a smoke test against production endpoints

### 7.4 GitHub Source Recovery

GitHub is authoritative for source code; no separate backup is maintained per [Backup Policy §4.4](../policies/backup-policy.md).

1. If repository corruption is suspected, clone from any developer's local working copy and force-push only after Engineering Lead verification and team confirmation
2. If access is lost, contact GitHub Support; once F-09 is closed, the second Owner-level reviewer also serves as a recovery escalation point
3. After recovery, re-verify CODEOWNERS, branch protection, and required PR review settings against the bootstrap checklist

### 7.5 Out-of-Provider Backups — Not Currently Implemented

As of 2026-06-01, TrueCalling.ai's backup architecture relies exclusively on Supabase-managed daily snapshots and PITR. No independent off-site backup repository (e.g., periodic export to S3, GCS, or a separate Supabase project) exists.

**Residual risk:** A full Supabase tenant compromise, account-level lockout, or sustained provider outage has no out-of-band data recovery path. This residual risk is acknowledged and tracked under R-007 in the [Risk Register](../evidence/risk-register/risk-register-2026.md) and is to be re-evaluated at the next quarterly risk review.

---

## 8. Disruption Scenarios

Each scenario lists detection signals, severity guidance, action, and applicable [Risk Register](../evidence/risk-register/risk-register-2026.md) entries.

### 8.1 Supabase Database Outage

- **Detection:** Application errors; Supabase status page; monitoring alerts
- **Severity:** P1 if customer-facing impact > 30 minutes
- **Action:** Confirm via Supabase status page; if sustained, await provider resolution and open a Supabase support ticket; if data integrity is suspected, execute §7.1 restore against last known-good PITR point
- **Risk reference:** R-007, R-008

### 8.2 Vercel Outage

- **Detection:** Site unavailability; Vercel status page
- **Severity:** P1 if customer-facing
- **Action:** Confirm via Vercel status page; if platform-wide, no direct mitigation — communicate per §6.2; if account-specific, contact Vercel Support; if deployment-specific, execute §7.3 rollback

### 8.3 GitHub Outage

- **Detection:** Inability to access repository; GitHub status page
- **Severity:** P2 unless coupled with active recovery (then P1)
- **Action:** GitHub outages do not directly affect production runtime; defer non-urgent changes; if active recovery is in progress, use cached local clones; consider GitHub Support escalation if sustained

### 8.4 LLM Provider Outage or Breaking Change

- **Detection:** Elevated error rate from agent runtime; Anthropic or OpenAI status page; provider deprecation notice
- **Severity:** P1 if customer-facing call quality is degraded; P2 if internal-only
- **Action:** Both Anthropic and OpenAI integrations exist in the agent runtime, but provider failover requires a manual configuration or deployment change — **automatic runtime failover is not currently implemented**. Engineering Lead executes the configuration change or deployment switch and communicates degraded-mode user experience per §6.2 as appropriate
- **Risk reference:** R-010

> **Improvement target:** Implement automatic provider failover in the agent runtime. Tracked under R-010 in the [Risk Register](../evidence/risk-register/risk-register-2026.md) and listed in §11.2 below.

### 8.5 Twilio Outage or Credential Compromise

- **Detection:** Call setup failures; Twilio status page; unexpected billing alerts; geographic permission violations
- **Severity:** P1 for credential compromise; P1 for outage with customer impact
- **Action:** For outage — wait or contact Twilio Support and communicate per §6.2. For credential compromise — rotate the API key per [Secrets Management Policy](../policies/secrets-management-policy.md), enable geographic permission restrictions, review recent activity for fraud, and log the incident in the `truecalling-incidents` repository.
- **Risk reference:** R-013

### 8.6 Vendor Lockout (Account-Level)

- **Detection:** Inability to authenticate to Supabase, Vercel, GitHub, or Google Workspace
- **Severity:** P1
- **Action:** Engineering Lead initiates vendor account recovery via vendor support; SOC 2 Program Lead documents the incident; if the Engineering Lead is the locked-out account holder, escalate to the second GitHub Owner (when F-09 is closed) and vendor support direct
- **Risk reference:** R-001

### 8.7 Secrets Compromise

- **Detection:** Gitleaks or TruffleHog CI alert; vendor abuse alert; manual discovery
- **Severity:** P1
- **Action:** Follow the [Incident Response Policy](../policies/incident-response-policy.md) and [Secrets Management Policy](../policies/secrets-management-policy.md) revocation procedure; rotate affected credentials; evaluate blast radius across all systems sharing the credential; log in the `truecalling-incidents` repository
- **Risk reference:** R-005

### 8.8 Key Personnel Unavailable

- **Detection:** Engineering Lead non-responsive during an active P1/P2 disruption, or planned absence without designated coverage
- **Severity:** P2 absent active disruption; P1 if compounding an active P1
- **Action:** The SOC 2 Program Lead may initiate vendor support escalation, customer communication per §6.2, and coordination of any Authorized Personnel. Restore execution against production data systems requires personnel with operational platform access; current platform access is documented in the quarterly access review evidence at `evidence/access-control/quarterly-reviews/` and per [ISP §3](../policies/information-security-policy.md) the SOC 2 Program Lead's role scope is evidence review and audit coordination, not platform-level operational access.
- **Risk reference:** R-001

> **Classification:** The activation of this scenario by Engineering Lead non-responsiveness is consistent with the 4-hour RTO defined in [Backup Policy §3](../policies/backup-policy.md), which would be threatened by an unresponsive Incident Commander for that duration. No standalone key-person responsiveness SLA is defined in any policy; the trigger is INFERRED from the RTO and from R-001 in the [Risk Register](../evidence/risk-register/risk-register-2026.md).

> **Planned mitigations** (tracked in the [Risk Register](../evidence/risk-register/risk-register-2026.md) under R-001; these do not constitute a formal compensating control at this stage):
>
> - **F-09** — Add a second GitHub Owner-level reviewer
> - **Cross-training** — Walk Authorized Personnel through backup verification and access review procedures
> - **Documentation improvement** — Continue to record operational steps in `procedures/` so that a future delegate can execute recovery with reference material rather than tacit knowledge
>
> A formal compensating control analogous to the [RBAC self-review compensating control](../evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md) is **not** established for key-person scenarios at this stage; the dependency is managed through R-001 mitigation work.

---

## 9. Testing and Drills

| Drill Type | Frequency | Owner | Evidence Location | Source |
|------------|-----------|-------|-------------------|--------|
| **Quarterly restore test** — restore to staging, verify integrity | Quarterly | Engineering Lead | `evidence/backups/verification-logs/` | [Backup Policy §5](../policies/backup-policy.md) |
| **Annual DR drill** — full restore + application smoke test | Annually | Engineering Lead | `evidence/backups/verification-logs/` | [Backup Policy §5](../policies/backup-policy.md) |
| **Annual incident response tabletop** — scenario-based exercise | Annually | Engineering Lead | `evidence/incidents/tabletop-<year>.md` | [Incident Response Policy §8](../policies/incident-response-policy.md) and [Compliance Calendar](../templates/compliance-calendar.md) Q4 entry |
| **Annual Key Personnel Unavailable tabletop** — exercise §8.8 | Annually, co-located with the IR tabletop | SOC 2 Program Lead | `evidence/incidents/tabletop-<year>.md` (same record as IR tabletop) | Introduced by this procedure — see §11.2 |

> **Classification:** The quarterly restore test and the annual DR drill are mandated by [Backup Policy §5](../policies/backup-policy.md) (VERIFIED). The annual incident response tabletop is mandated by [Incident Response Policy §8](../policies/incident-response-policy.md) and is already on the [Compliance Calendar](../templates/compliance-calendar.md) for 2026-12-31 (VERIFIED). The Backup Policy DR drill and the IR Policy tabletop are distinct exercises: the former is a full technical restore, the latter is a scenario-based paper exercise. The Key Personnel Unavailable tabletop is introduced by this procedure (ASSUMED until added to the Compliance Calendar — see §11.2).

A drill that uncovers a gap must be logged as a P2 finding in the `truecalling-incidents` repository with a remediation plan and a due date.

---

## 10. Post-Disruption Review

After any P1 or P2 disruption, a post-incident review must be completed within **5 business days** per [Incident Response Policy §4.7](../policies/incident-response-policy.md) and the [Compliance Calendar](../templates/compliance-calendar.md) Event-Triggered Controls. The review record is committed under `evidence/incidents/<YYYY-MM-DD>-<short-name>/`.

The IR Policy §4.7 post-mortem requires: timeline, root cause, impact, what worked / what didn't, action items. This procedure extends those required items with DR-specific content (items 2, 5, 6, and 8 below are DR-specific extensions introduced by this procedure):

1. Timeline — detection, declaration, containment, recovery, resolution *(per IR §4.7)*
2. RPO and RTO actuals versus targets *(DR-specific extension)*
3. Affected systems, data, and customers *(per IR §4.7)*
4. Root cause and contributing factors *(per IR §4.7)*
5. Recovery procedures executed (which §7 / §8 sections were invoked) *(DR-specific extension)*
6. Customer communication record (copies of emails sent under §6.2) *(DR-specific extension)*
7. Corrective actions, with named owner and due date *(per IR §4.7)*
8. Risk register updates — any new risk identified or any existing risk re-scored *(DR-specific extension)*

The Engineering Lead approves the technical findings; the SOC 2 Program Lead approves the evidence trail.

---

## 11. Maintenance and Review

### 11.1 Review Cycle

This procedure is reviewed **annually** and additionally upon any of the following triggers:

- A P1 disruption (review within 30 days of resolution)
- A material change to backup architecture (e.g., addition of an off-site backup repository, change of database provider)
- A change to RPO or RTO targets in the [Backup Policy](../policies/backup-policy.md)
- A change to the LLM provider failover model (e.g., automatic failover is implemented — §8.4)
- A change to the SOC 2 audit scope

### 11.2 Open Improvements

The following items are intended to upgrade the classification of controls in this procedure over time. Tracked in the [Compliance Calendar](../templates/compliance-calendar.md) and the [Risk Register](../evidence/risk-register/risk-register-2026.md).

| # | Improvement | Current State | Target State | Tracking |
|---|-------------|---------------|--------------|----------|
| 1 | Screenshot evidence of `#security-incidents` Slack channel | INFERRED-strong (§6.1) | VERIFIED | Next evidence collection cycle |
| 2 | Customer-availability notification SLAs (first notification, cadence, restoration notice, post-incident summary) | ASSUMED — not yet defined (§6.2) | Defined SLAs documented | Before next annual DR drill |
| 3 | Customer communication templates and distribution list | ASSUMED — not yet defined (§6.2) | Documented under `templates/dr-customer-communication-templates.md` | Before next annual DR drill |
| 4 | Off-site backup repository (separate provider) | Not implemented (§7.5) | Implemented and tested | R-007 quarterly review |
| 5 | Automatic LLM provider failover (§8.4) | Manual | Automatic and runtime-tested | R-010 quarterly review |
| 6 | Operational execution of the §7.1 restore procedure | INFERRED (§7.1) | VERIFIED via first quarterly restore test | R-007 quarterly review |
| 7 | Second GitHub Owner-level reviewer | Not established | Established | F-09 |
| 8 | Customer status page | Not deployed (§6.2) | Deployed and documented | Annual review |
| 9 | Add the annual Key Personnel Unavailable tabletop to the Compliance Calendar | ASSUMED — introduced by this procedure (§9) | Listed on Compliance Calendar with owner and due date | Next Compliance Calendar update |

---

## 12. Approval

| Field | Value |
|-------|-------|
| **Approved By** | Engineering Lead |
| **Approval Date** | 2026-06-01 |
| **Next Review Due** | 2027-06-01 |
| **Version History** | Tracked via `git log procedures/disaster-recovery-and-business-continuity.md` |

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
*SOC 2 Criteria: A1.2, A1.3, CC7.4, CC7.5, CC2.3, CC9.1*
