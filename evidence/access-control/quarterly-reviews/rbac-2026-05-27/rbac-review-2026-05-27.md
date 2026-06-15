# RBAC Quarterly Access Review

**Review Date:** 2026-05-27  
**Reviewer:** Engineering Lead  
**Review Type:** Initial bootstrap (first quarterly review)  
**Next Review Due:** 2026-08-27  
**SOC 2 Criteria:** CC6.1, CC6.2  
**Reference:** Access Control Policy §8

---

## Instructions

1. Pull current member lists from GitHub, Supabase, and Vercel
2. Fill in each table below with the actual members and their roles
3. Compare each role against the approved RBAC ceiling in the Access Control Policy
4. Flag any over-provisioned access in the Issues section
5. Sign off at the bottom and commit

Screenshots for each system are saved alongside this file.

---

## GitHub — Organization Members

**Source:** [github.com → truecalling-ai → Settings → Members](https://github.com/organizations/truecalling-ai/settings/members)  
**Screenshot:** `github-members-2026-05-27.png`

| Username      | Display Name    | Org Role | MFA Enabled | RBAC Ceiling (from Policy) | Match? |
| ------------- | --------------- | -------- | ----------- | -------------------------- | ------ |
| CohenYarone01 | Yarone Cohen    | Owner    | Yes         | Engineering Lead / Admin   | Yes    |
| rbzil         | Raphael Bouaziz | Member   | Yes         | Product Manager / Member   | Yes    |

---

## GitHub Access Justification

**User:** Raphael Bouaziz  
**Role:** Product Manager  
**Repository Access:** Member (limited operational access)

**Business Justification:**  
Requires repository visibility to review product implementation progress, coordinate releases, and collaborate with engineering on product delivery activities.

**Reviewed By:** Yarone Cohen  
**Review Date:** 2026-05-28

**Decision:**  
Access reviewed and approved as appropriate under the principle of least privilege.

---

**Team memberships reviewed:** No teams configured — all access is at the org membership level.

> ### ⚠️ Correction (appended 2026-06-14) — Finding F-29
>
> The GitHub "MFA Enabled: Yes" entries in the table above are not corroborated by the authoritative
> source. The committed GitHub organization members export (`export-Truecalling-ai-GitHub-2026-05-27.csv`,
> container root) records `tfa_enabled=false` / `tfa_level=disabled` for both CohenYarone01 and rbzil on
> the same review date (2026-05-27). The committed `github-mfa-enabled-2026-05-27.png` shows a GitHub
> two-factor settings page with an authenticator configured, but the capture is unattributed — GitHub's
> security-settings page does not display the account identifier, so it cannot be tied to either member.
>
> Because the export is a point-in-time snapshot that may be stale, and the only "enabled" artifact is
> unattributed, the correct status is unverified — not "disabled" and not "enabled": GitHub MFA for both
> members is not established by any current, attributed, authoritative artifact.
>
> Corrected status (2026-06-14): GitHub MFA = unverified, pending re-evidence with an attributed current
> export or per-member screenshot (the org "Require two-factor authentication" setting is preferred).
> Tracked as F-29. Original table rows retained above unaltered to preserve the dated evidence trail.

> ### ⚠️ Correction (appended 2026-06-14) — Access-review population completeness
>
> The GitHub members table above enumerated the two accounts from the 2026-05-27 export (CohenYarone01,
> rbzil) and did not include DanAyache, which is the repository owner / primary administrator. An access
> review should cover all privileged identities.
>
> Action: include DanAyache in the next access review's population. Its MFA state, least-privilege
> determination, and the repository's organizational ownership model are not asserted here and are to be
> established from an authoritative source (e.g., a current GitHub members/security export) at that review.
> Original table retained above unaltered.

---

## Supabase — Project Team

**Source:** [supabase.com → Organization Settings → Team](https://supabase.com/dashboard)  
**Screenshot:** `supabase-members-2026-05-27.png`

| Email                                                             | Name                  | Supabase Role | MFA Enabled | RBAC Ceiling (from Policy) | Match? |
| ----------------------------------------------------------------- | --------------------- | ------------- | ----------- | -------------------------- | ------ |
| [cohen.yarone@icloud.com](mailto:cohen.yarone@icloud.com)         | Yarone Cohen          | Owner         | No          | Engineering Lead / Owner   | Yes    |
| [patrick@sarona-partners.com](mailto:patrick@sarona-partners.com) | Patrick Simon Bouaziz | Administrator | No          | Executive / Administrator  | Yes    |

---

## Supabase Access Justification

**User:** Patrick Simon Bouaziz  
**Role:** Executive / Administrator  
**Supabase Role:** Administrator

**Business Justification:**  
Requires administrative access for operational oversight, vendor coordination, incident response support, and business continuity management.

**Reviewed By:** Yarone Cohen  
**Review Date:** 2026-05-28

**Decision:**  
Access reviewed and approved as appropriate under the principle of least privilege.

---

## Vercel — Team Members

**Source:** [vercel.com → Team Settings → Members](https://vercel.com/dashboard)  
**Screenshot:** `vercel-members-2026-05-27.png`

| Email                                                     | Name         | Vercel Role | RBAC Ceiling (from Policy) | Match? |
| --------------------------------------------------------- | ------------ | ----------- | -------------------------- | ------ |
| [cohen.yarone@icloud.com](mailto:cohen.yarone@icloud.com) | Yarone Cohen | Owner       | Engineering Lead / Owner   | Yes    |

---

## Vercel Access Justification

**User:** Yarone Cohen  
**Role:** CEO / Engineering Lead  
**Vercel Role:** Owner

**Business Justification:**  
Requires administrative access for platform management, deployment oversight, billing administration, incident response, and business continuity operations.

**Reviewed By:** Yarone Cohen  
**Review Date:** 2026-05-28

**Decision:**  
Access reviewed and approved as appropriate under the principle of least privilege.

---

## API Key Review

**Source:** `evidence/api-keys/openai-keys.md`

> ### ⚠️ Correction (appended 2026-06-02) — Finding F-18 (audit C-3)
>
> The four checkboxes below, as originally signed on 2026-05-28, **overstated** the work performed. They asserted that provider API keys had been reviewed and that "no unused or unauthorized API keys" existed. In fact, as of this review date the **API Key Inventory ([openai-keys.md](../../../api-keys/openai-keys.md)) had not enumerated any provider** — so no key population existed to review against. The attestation was therefore unsupported.
>
> This correction is appended (rather than the original being deleted) to preserve the integrity of the dated evidence trail. The original checkboxes are struck through and replaced with the accurate status below. Tracked as **F-18** in the [Findings & Remediation Register](../../../../findings/findings-remediation-register.md).
>
> **Corrected status (2026-06-02):**
>
> - [ ] ~~GitHub personal access tokens reviewed~~ → **Not performed.** Pending F-14 enumeration.
> - [ ] ~~Supabase service role keys reviewed~~ → **Not performed.** Pending F-14 enumeration.
> - [ ] ~~Vercel deployment tokens reviewed~~ → **Not performed.** Pending F-14 enumeration.
> - [ ] ~~No unused or unauthorized API keys identified~~ → **Cannot be asserted** until the inventory is populated.
>
> **Resolution path:** The API key review will be performed against the populated inventory when **F-14** completes (target 2026-06-30). At that point this section will be re-signed with an accurate attestation, or carried into the next quarterly review (2026-08-27), whichever comes first.

*Original (uncorrected) checkboxes — retained for audit trail, superseded by the correction above:*

- [x] ~~GitHub personal access tokens reviewed~~
- [x] ~~Supabase service role keys reviewed~~
- [x] ~~Vercel deployment tokens reviewed~~
- [x] ~~No unused or unauthorized API keys identified~~

---

## Issues Identified

| # | System   | Issue                                                   | Action Required                                        | Due Date   | Resolved? |
|---|----------|---------------------------------------------------------|--------------------------------------------------------|------------|-----------|
| 1 | Supabase | MFA disabled for both members (Owner and Administrator) | Enable MFA for Yarone Cohen and Patrick Simon Bouaziz  | 2026-06-04 | No        |

**Note:** Supabase Free plan does not enforce MFA at the organization level. Both members have been notified to enable MFA on their personal Supabase accounts. Remediation due within 5 business days per Access Control Policy §8.

---

## Reviewer Sign-Off

**Reviewed By:** Yarone Cohen  
**GitHub Handle:** @CohenYarone01  
**Review Date:** 2026-05-28

**Approval Status:** Approved

**Notes:**  
Quarterly RBAC review completed for GitHub, Supabase, and Vercel. Access permissions were reviewed against approved RBAC ceilings and verified as appropriate under the principle of least privilege.

---

*Next quarterly review due: 2026-08-27*  
*Evidence retained for: 3 years*
