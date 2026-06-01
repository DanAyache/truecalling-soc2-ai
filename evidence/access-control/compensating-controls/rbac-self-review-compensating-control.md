# RBAC Self-Review Compensating Control

**Organization:** TrueCalling.ai
**Version:** 1.0
**Effective Date:** 2026-06-01
**Owner:** Engineering Lead
**Review Cycle:** Annual, or on any trigger listed in §5
**SOC 2 Criteria:** CC6.1, CC6.2, CC1.4 (separation of duties)
**Related Policy:** [Access Control Policy](../../../policies/access-control-policy.md) §8 (Quarterly Access Review); [Information Security Policy](../../../policies/information-security-policy.md) §3 (Roles and Responsibilities)
**Related Risk:** [R-001](../../risk-register/risk-register-2026.md) (Key person dependency)
**Related Procedure:** [Quarterly Access Review template](../../../templates/quarterly-access-review.md)

---

## 1. Purpose

This document records the compensating controls TrueCalling.ai has put in place to address the separation-of-duties weakness inherent in having the Engineering Lead perform quarterly access reviews that include their own access. It is the authoritative reference for auditors and future reviewers explaining why a single platform-layer reviewer model is acceptable for the current observation period, and what controls limit the risk that would otherwise be addressed by an independent platform-layer second reviewer.

This document does not modify any policy. The [Access Control Policy](../../../policies/access-control-policy.md) §8 mandates quarterly access reviews; this document explains how the integrity of those reviews is preserved despite the absence of a second platform-layer reviewer.

The controls listed in §4 are classified as **Verified** (evidenced in this repository or by direct observation), **Inferred** (likely operative but with evidence collection pending), or **Planned** (not yet operative; with defined target operative dates). This classification is published transparently so the compensating-control posture is not overstated.

## 2. The Underlying Control Weakness

### 2.1 What the SOC 2 Criteria Expect

CC6.1 and CC6.2 require entities to authorize, modify, and remove access to information assets with appropriate segregation of duties. The standard implementation pattern is that **the reviewer of an access list must be independent of the reviewee** for privileged accounts. Self-review of one's own access is widely viewed as an insufficient stand-alone control because a self-reviewer can:

- Overlook (intentionally or unintentionally) retained access that exceeds current business need
- Conceal unauthorized grants made to themselves
- Fail to identify access drift between roles

### 2.2 The Current State at TrueCalling.ai

The Engineering Lead is the most-privileged role across GitHub, Supabase, and Vercel. Per the [Access Control Policy](../../../policies/access-control-policy.md) §4 RBAC table, the Engineering Lead holds Owner permissions on all three platforms.

The quarterly RBAC review described in §8 of the same policy is performed by the Engineering Lead. The Engineering Lead is therefore both reviewer and reviewee for the most-privileged platform accounts in the organization.

This weakness is also reflected in Risk Register entry [R-001](../../risk-register/risk-register-2026.md) (key person dependency).

## 3. Why a Second Reviewer Is Not Currently Practical

This section distinguishes between two layers of access review:

- **Platform-layer review** — verifying that access *exists* in GitHub, Supabase, and Vercel against screenshots taken from those platforms. Requires the reviewer to hold Owner-level access on each platform.
- **Evidence-layer review** — verifying the *committed evidence artifacts* (RBAC review records, access tables, signed reviews) against policy ceilings and prior-period state. Requires only repository read access plus policy literacy.

### 3.1 Platform-layer review

A fully independent second platform-layer reviewer is not currently practical for the reasons documented below. Each reason is conditional and is expected to change as the organization grows; the triggers in §5 define when this condition is re-evaluated.

| Reason | Detail |
|--------|--------|
| **Limited candidate pool** | The engineering team is small enough that no other individual currently combines (a) sufficient system literacy to evaluate the Engineering Lead's access against business need, with (b) sufficient independence from the role being reviewed |
| **No second Owner** | No other team member holds Owner-level access for both GitHub and Supabase simultaneously. A reviewer without comparable platform access cannot independently verify what they see in screenshots. *(Adding a second GitHub Owner-level reviewer is tracked as audit finding F-09.)* |
| **Role separation not yet established** | The Authorized Personnel role defined in [Information Security Policy](../../../policies/information-security-policy.md) §3 exists organizationally but has not yet had specific quarterly review delegation formally assigned |
| **External substitute available annually, not quarterly** | The annual SOC 2 readiness review or formal audit provides external independent assessment of RBAC activity, but does not run on the quarterly cadence required for ongoing operations |

### 3.2 Evidence-layer review

An independent evidence-layer reviewer **is in place**. The SOC 2 Program Lead, as defined in [Information Security Policy](../../../policies/information-security-policy.md) §3, performs ongoing evidence-layer review of artifacts committed by the Engineering Lead. The specific scope of this responsibility is recorded in ISP §3 as the authoritative source; this document does not duplicate it.

This evidence-layer review does not substitute for a platform-layer reviewer (the SOC 2 Program Lead does not hold Owner access on the platforms and so cannot independently verify the underlying state). It does provide a meaningful independent check on the *committed evidence*, which is the artifact an auditor evaluates.

This control is recorded as **C-4** in §4.A.

### 3.3 Engineering Lead acknowledgement

The Engineering Lead acknowledges that the absence of a second platform-layer reviewer is a known weakness and accepts the corresponding residual risk for the duration of this compensating control.

---

## 4. Compensating Controls

This section is divided into Verified (§4.A), Inferred (§4.B), and Planned (§4.C) subsections. Each control's classification reflects whether evidence currently exists in this repository or by direct observation. The §4.B and §4.C controls have target dates for transition to Verified status.

### 4.A Verified Controls

These controls are evidenced today by files in this repository or by direct observation.

| # | Control | Evidence |
|---|---------|----------|
| **P-3** | CODEOWNERS file at [.github/CODEOWNERS](../../../.github/CODEOWNERS) covers `/policies/`, `/procedures/`, `/evidence/`, `/.github/workflows/`, `/.github/CODEOWNERS`, `/.github/dependabot.yml`. Code owner is the Engineering Lead per [ISP §3](../../../policies/information-security-policy.md) | Direct file content verified; rule paths visible in repo |
| **D-1** | Git commit history records author, timestamp, and content hash for every commit to `evidence/`, `policies/`, and `procedures/`. Force-pushing would be detectable via branch history | Direct observation of session commits (`6b939f3`, `5080c88`, `2399421`, `c82f82d`, `626b820`, `9245443`). **Caveat:** commits are not GPG-signed; tamper-evidence is structural, not cryptographic |
| **D-4** | Continuous GitHub Actions security scans (Gitleaks, TruffleHog, npm audit) on every PR via [.github/workflows/security.yml](../../../.github/workflows/security.yml) | Workflow file present; scan summary at `evidence/security-scans/gitleaks/2026-05/security-summary-2026-05-31.txt` from 2026-05-31 |
| **D-5** | Quarterly access review records are committed to and retained in the Git history of this repository, ensuring they remain available for inspection by future reviewers, auditors, successors, or delegated personnel | Git history retention is intrinsic to the repository structure; prior quarterly review records (e.g., `evidence/access-control/quarterly-reviews/rbac-2026-05-27/rbac-review-2026-05-27.md`) are visible in `git log` |
| **C-2** | Risk Register entry R-001 (key person dependency) is tracked at [evidence/risk-register/risk-register-2026.md](../../risk-register/risk-register-2026.md) with review date 2026-07-01 | File committed `5080c88` and pushed; quarterly review cadence defined in the register itself |
| **C-4** | SOC 2 Program Lead provides evidence-layer review as a corroborating compensating control; the specific scope of this responsibility is recorded in [ISP §3](../../../policies/information-security-policy.md) | Responsibility recorded in ISP §3 (committed `9245443`); currently operating per SOC 2 Program Lead attestation |

### 4.B Inferred Controls (Evidence to Collect)

These controls are likely operative based on existing repository configuration or organizational evidence, but direct screenshot, log, or attestation evidence has not yet been collected. Target evidence-collection date for all items in this section: **2026-06-30**.

| # | Control | Why inferred | Evidence to collect to upgrade to Verified |
|---|---------|--------------|-------------------------------------------|
| **P-1** | MFA mandated by [Access Control Policy](../../../policies/access-control-policy.md) §5 across GitHub, Supabase, and Vercel. Status by platform at the time of this writing: Engineering Lead MFA enabled on Supabase (per R-002); GitHub and Vercel MFA for Engineering Lead account to be verified; Supabase MFA for other team members outstanding per R-002 (due 2026-06-04) | Policy mandate exists; Engineering Lead's Supabase MFA is asserted in R-002 | (a) Supabase team screenshot showing all members with MFA status; (b) GitHub Engineering Lead account `https://github.com/settings/security`; (c) Vercel MFA confirmation. Per-platform screenshots saved under `evidence/access-control/mfa-enforcement/` |
| **P-2** | Branch protection on `main` requiring PR review before merge | CODEOWNERS would be functionally inoperative without it; [BOOTSTRAP-CHECKLIST.md](../../BOOTSTRAP-CHECKLIST.md) §1.2 requires the screenshot but Section 1 is still ⏳ unchecked | Screenshot per BOOTSTRAP-CHECKLIST.md §1.2 saved at `evidence/access-control/mfa-enforcement/github-branch-protection-main-2026-05-27.png` |
| **P-4** | MFA on the personal GitHub account hosting the repository (`DanAyache`). Organization-level MFA enforcement is not applicable until repository migration to a GitHub organization (audit finding F-13) | Personal account holds the repo and gates the entire SOC 2 evidence trail | Screenshot of `https://github.com/settings/security` for the `DanAyache` account |
| **D-2** | GitHub user security log for the account hosting the repository at `https://github.com/settings/security-log` — captures auth events and privileged settings changes. Full organization-level audit log becomes available only after F-13 (repo transfer to org) is closed | Personal accounts have a user security log by default | Screenshot of the security log for the `DanAyache` account during the observation period |
| **D-3** | Supabase logs (database, authentication, edge function) accessible via project dashboard. Provide records of access events independent of the Engineering Lead's review record | Supabase logs are a standard product feature available on all tiers | Screenshot of Supabase Project → Logs showing available log types and retention window |

### 4.C Planned Controls (Not Yet Operative)

These controls are part of the SOC 2 readiness program plan but are not yet operating. Each has a target operative date. They are listed here so the compensating-control posture includes the planned trajectory rather than being represented by existing controls alone.

| # | Control | Status and target |
|---|---------|-------------------|
| **C-1** | Annual external SOC 2 readiness review or audit | **Planned.** Target Q1 2027 per the SOC 2 readiness program plan. No engagement currently contracted. When in place, will provide independent expert assessment of RBAC activity over the year |
| **C-3** | Each quarterly access review record references this document and re-affirms the compensating control is still in force | **Planned.** Established by §6.1 of this document. Becomes operative starting with the next quarterly review (target 2026-08-27). Requires a one-line edit to [templates/quarterly-access-review.md](../../../templates/quarterly-access-review.md) — separate PR |

---

## 5. Mandatory Re-Evaluation Triggers

This compensating control is re-evaluated at the **earlier** of any of the following triggers. When any trigger fires, the Engineering Lead must either (a) establish a second-reviewer model that meets CC6.1 separation of duties, or (b) document why the trigger does not require that change and obtain external acknowledgement.

| # | Trigger | When |
|---|---------|------|
| T-1 | Team reaches 5 or more individuals with engineering access | Whenever the threshold is crossed |
| T-2 | An Authorized Personnel team member is formally delegated quarterly RBAC review responsibility | Upon delegation |
| T-3 | A second GitHub Owner-level reviewer is added (closes audit finding F-09) | Upon addition |
| T-4 | Any unexplained discrepancy is detected between the GitHub, Supabase, or Vercel logs and a committed RBAC review record | Immediately upon detection |
| T-5 | A SOC 2 audit finding flags self-review as a deficiency | Upon receipt of the finding |
| T-6 | Commencement of a formal external audit engagement requires re-evaluation of this compensating control and documentation of whether an independent reviewer is available | Upon engagement commitment |
| T-7 | Annual review of this document | 2027-06-01 (minimum) |

The Engineering Lead is responsible for monitoring triggers T-1 through T-3 actively; triggers T-4 through T-6 are event-driven; trigger T-7 is calendar-driven and tracked in the [Compliance Calendar](../../../templates/compliance-calendar.md).

## 6. Acknowledgement and Review

### 6.1 Quarterly Review Integration

Each quarterly RBAC review record (per [quarterly-access-review.md](../../../templates/quarterly-access-review.md)) must include the following statement in its reviewer sign-off section:

> *Compensating control acknowledged: this review is performed by the Engineering Lead under the RBAC self-review compensating control documented at `evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md`. No re-evaluation trigger has been observed since the prior review.*

If any trigger from §5 was observed during the quarter, the reviewer must state which trigger and what action was taken instead of the standard sign-off.

### 6.2 Annual Review

This document is reviewed annually (target 2027-06-01) and is re-approved or replaced. Replacement is expected if any §5 trigger has fired by that date.

### 6.3 Approval

| Field | Value |
|-------|-------|
| **Approved By** | Engineering Lead |
| **Approval Date** | 2026-06-01 |
| **Next Review Due** | 2027-06-01 (or earlier if any §5 trigger fires) |
| **Version History** | Tracked via `git log evidence/access-control/compensating-controls/rbac-self-review-compensating-control.md` |

---

*This document records a compensating control, not an exemption. The risk it addresses is acknowledged in Risk Register entry R-001 and is re-evaluated at every quarterly review.*
*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Evidence retention: 3 years*
