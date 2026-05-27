# Employee Offboarding Procedure

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policy:** Access Control Policy, Incident Response Policy

---

## Purpose

Ensure all access is revoked completely and promptly when an employee or contractor leaves, protecting customer data and maintaining SOC 2 CC6.2 and CC6.3 compliance.

---

## Scope

All departing full-time employees, part-time employees, and contractors.

---

## SLA

| Action | Deadline |
|--------|----------|
| All access revoked | Within **4 hours** of departure confirmation |
| Shared credentials rotated | Within **24 hours** of departure |
| Offboarding issue closed with evidence | Within **2 business days** |

Involuntary terminations: access is revoked **before** or **simultaneously with** the departure conversation.

---

## Procedure

### Step 1 — Trigger & Notification (Manager / HR)

- [ ] Manager or HR notifies Engineering Lead via direct message (Slack or email) **on or before the last day**
  - For involuntary terminations: notify Engineering Lead at least 30 minutes before the conversation
- [ ] Engineering Lead opens an **Offboarding Issue** in `truecalling-incidents` (private repo) immediately:

**Offboarding Issue Template:**
```
Title: [OFFBOARDING] <Full Name> — <Last Day>

Role: 
Last working day: 
Departure type: voluntary / involuntary / contractor end
Systems to revoke (confirm from onboarding issue): 
Shared credentials to rotate: 
Notified by: 
Engineering Lead assigned: 
```

- [ ] Link to the original onboarding issue for the list of systems granted

---

### Step 2 — Immediate Access Revocation (Engineering Lead, within 4 hours)

Work through each system in order. Check each box only after confirming revocation is complete.

#### Google Workspace
- [ ] Suspend the account (do not delete immediately — preserves email and Drive for handover)
- [ ] Sign out of all active sessions: Admin Console → User → Security → Sign out of all sessions
- [ ] Remove from all Google Groups
- [ ] Transfer ownership of any shared Drive files or docs to the manager

#### GitHub
- [ ] Remove from GitHub organization: Org Settings → Members → Remove
- [ ] Verify removal has been applied (user loses all org repo access immediately upon removal)
- [ ] Revoke any Personal Access Tokens (PATs) the user created: check `evidence/api-keys/` log
- [ ] Remove from any GitHub Apps or OAuth grants if applicable
- [ ] If user had `Owner` role: rotate any org-level secrets they had access to

#### Supabase
- [ ] Remove from Supabase project: Project Settings → Team → Remove member
- [ ] Confirm removal reflected in the member list (screenshot)
- [ ] If user had `Developer` or higher role: rotate the service role key and update Vercel environment variables within 24 hours

#### Vercel
- [ ] Remove from Vercel team: Team Settings → Members → Remove
- [ ] Revoke any project-level tokens associated with the user
- [ ] Access via Google SSO is automatically blocked once Google Workspace account is suspended

#### OpenAI API
- [ ] Revoke the user's named API key: Platform → API Keys → Revoke
  - Key naming convention: `<name>-<role>-<YYYY-MM>`
- [ ] Update `evidence/api-keys/openai-keys.md` with revocation date
- [ ] If the user had access to a shared key (non-compliant — flag for audit): rotate that key immediately

#### Claude Code / Anthropic
- [ ] Remove from company Anthropic organization or workspace if applicable
- [ ] Revoke any API keys provisioned for the user
- [ ] Update the API key log with revocation date

#### Password Manager
- [ ] Remove from company password manager (1Password / Bitwarden)
- [ ] Rotate any passwords the user had access to that are shared with the team
- [ ] Audit vault access logs for any credential exports in the 30 days prior to departure

#### Other SaaS Tools
- [ ] Review the onboarding issue for any additional tools granted
- [ ] Revoke access to each; document in the offboarding issue

---

### Step 3 — Credential Rotation (Engineering Lead, within 24 hours)

Rotate any shared secrets the departing user had access to:

- [ ] Supabase service role key → update in Vercel environment variables → redeploy
- [ ] Any GitHub Actions secrets the user could read (check Actions secrets list)
- [ ] Any shared API keys not already revoked in Step 2
- [ ] Webhook signing secrets if applicable

Document each rotation in the offboarding issue with timestamp.

> If a P1/P2 incident is suspected (e.g., involuntary termination with potential malicious intent), initiate the Incident Response Policy immediately and rotate all credentials before notifying the departing employee.

---

### Step 4 — Evidence Collection

- [ ] Screenshot of GitHub org member list confirming user is not present
- [ ] Screenshot of Supabase team member list confirming user is not present
- [ ] Screenshot of Vercel team member list confirming user is not present
- [ ] Screenshot or export of OpenAI API key list showing key is revoked
- [ ] Export of Google Workspace audit log for the user's last 30 days of activity (Admin Console → Reports → Audit → Filter by user)
- [ ] GitHub audit log export filtered by the departing user's account (Org → Settings → Audit Log → filter: `actor:<username>`)
- [ ] Confirmation of all credential rotations with timestamps

Store all evidence in `evidence/offboarding/<name>-<date>/`.

---

### Step 5 — Data & Equipment Handling

- [ ] Confirm any work in progress has been committed and pushed to GitHub (no orphaned local branches)
- [ ] Retrieve company-owned equipment per HR process
- [ ] If user had local copies of production data or API keys: confirm deletion or verify no data was stored outside approved systems
- [ ] Google Workspace account: after 30-day hold, export and delete per data retention policy

---

### Step 6 — Close Offboarding Issue

- [ ] All Step 2 checkboxes confirmed complete
- [ ] All Step 3 credential rotations confirmed complete
- [ ] All Step 4 evidence artifacts stored in `evidence/offboarding/`
- [ ] Close the GitHub issue with: `Offboarding complete — <date> — all access revoked — verified by <Engineering Lead GitHub handle>`
- [ ] Notify manager/HR that access revocation is complete

---

### Step 7 — Quarterly Access Review Update

At the next quarterly access review, confirm:
- [ ] Departed user does not appear in any system's member list
- [ ] Any lingering access missed during offboarding is caught and removed
- [ ] Offboarding issue is linked in the quarterly review record

---

## Evidence Artifacts

| Artifact | Location | Retention |
|----------|----------|-----------|
| Offboarding issue | GitHub `truecalling-incidents` repo | 3 years |
| System revocation screenshots | `evidence/offboarding/<name>-<date>/` | 3 years |
| Google Workspace audit log export | `evidence/offboarding/<name>-<date>/` | 3 years |
| GitHub org audit log export | `evidence/offboarding/<name>-<date>/` | 3 years |
| API key revocation log update | `evidence/api-keys/openai-keys.md` | 3 years |
| Credential rotation records | Offboarding issue comments | 3 years |

---

## Roles & Responsibilities

| Who | Responsibility |
|-----|---------------|
| Manager / HR | Notifies Engineering Lead on or before last day; confirms equipment return |
| Engineering Lead | Owns entire revocation process; collects evidence; closes issue |
| Any Engineer | May assist with specific system revocations if delegated — Engineering Lead remains accountable |

---

## Escalation

If access cannot be revoked within 4 hours (system outage, account dispute, etc.), treat as a P2 incident under the Incident Response Policy and notify the Engineering Lead immediately.

---

*Procedure Owner: Engineering Lead — stephane@truecalling.ai*  
*Next Review: 2027-05-27*
