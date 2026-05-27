# Employee Onboarding Procedure

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policy:** Access Control Policy

---

## Purpose

Ensure every new employee or contractor receives access that is correctly scoped to their role, MFA-protected, and documented before they begin work — satisfying SOC 2 CC6.1 and CC6.2.

---

## Scope

All full-time employees, part-time employees, and contractors granted access to any TrueCalling.ai system.

---

## Procedure

### Step 1 — Pre-Arrival (Manager / Engineering Lead, ≥ 1 business day before start)

- [ ] Confirm the hire's role, start date, and required system access with the hiring manager
- [ ] Open an **Onboarding Access Request** as a GitHub issue in `truecalling-incidents` (private repo) using the template below
- [ ] Verify a company email address (Google Workspace) has been created by IT/admin
- [ ] Do not provision any system access until the access request issue is approved by the Engineering Lead

**Access Request Issue Template:**
```
Title: [ONBOARDING] <Full Name> — <Start Date>

Role: 
Systems needed (list each): 
Business justification: 
Requested by: 
Approved by: (Engineering Lead signs off here)
```

---

### Step 2 — Day 1: Identity & MFA Setup (New Hire + Engineering Lead)

- [ ] New hire logs into Google Workspace with company email
- [ ] **MFA enrolled immediately** using an authenticator app (Google Authenticator or Authy)
  - SMS-based MFA is not permitted
  - Engineering Lead verifies MFA is active before any other access is granted
- [ ] New hire completes security awareness training (link shared via onboarding doc) on Day 1
- [ ] New hire signs and returns the **Acceptable Use Policy** acknowledgement (stored in `evidence/onboarding/`)

> **Evidence checkpoint:** Screenshot of MFA enrollment confirmation saved to `evidence/onboarding/<name>-mfa-<date>.png`

---

### Step 3 — Access Provisioning (Engineering Lead)

Access is granted **after** Step 2 is confirmed complete. Grant only what the role requires per the RBAC table in the Access Control Policy.

#### GitHub
- [ ] Invite to GitHub organization at the correct role (`Write` for engineers, `Read` for contractors scoped to specific repos)
- [ ] Confirm MFA is enforced at org level (org setting — non-MFA members are auto-suspended)
- [ ] Add to team(s) matching role (e.g., `engineering`, `contractors`)
- [ ] For contractors: grant access to named repos only — do not grant org-wide access

#### Supabase
- [ ] Invite to Supabase project at correct role:
  - Senior Engineer → `Developer`
  - Engineer → `Viewer` (no direct DB access)
  - Contractor → no Supabase access unless explicitly justified
- [ ] Confirm invite accepted and MFA enabled on Supabase account
- [ ] Do not share service role keys — these live only in Vercel environment variables

#### Vercel
- [ ] Invite to Vercel team at correct role:
  - Senior Engineer → `Member`
  - Engineer → `Viewer`
  - Contractor → no Vercel access unless justified
- [ ] Access is granted via Google SSO only — no standalone Vercel password accounts

#### OpenAI API
- [ ] If role requires API access, create a **named API key** for the individual (not a shared key)
  - Key name format: `<name>-<role>-<YYYY-MM>`
  - Set usage limits appropriate to the role
- [ ] Log the key name and creation date in `evidence/api-keys/openai-keys.md`
- [ ] Do not share existing keys — each person gets their own

#### Claude Code (Anthropic)
- [ ] If role requires Claude Code access, provision via company Anthropic account or approved personal setup
- [ ] Add to any shared Claude organization workspace if applicable
- [ ] Log access grant in the onboarding issue

#### Password Manager
- [ ] Add to company password manager (1Password / Bitwarden) at appropriate vault access level
- [ ] Verify new hire can access only vaults relevant to their role

---

### Step 4 — Verification Checklist (Engineering Lead)

Before closing the onboarding issue, confirm:

- [ ] MFA active on: Google Workspace, GitHub, Supabase (if applicable), Vercel (via SSO)
- [ ] All access matches the approved role in the Access Control Policy RBAC table
- [ ] No access granted beyond what was approved in the request issue
- [ ] Acceptable Use Policy signed and stored
- [ ] API keys are named, scoped, and logged
- [ ] Onboarding issue updated with completion timestamp and Engineering Lead sign-off

> **Evidence checkpoint:** Close the GitHub issue with a comment: `Onboarding complete — <date> — verified by <Engineering Lead GitHub handle>`

---

### Step 5 — 30-Day Access Review

- [ ] At 30 days post-start, Engineering Lead reviews whether access granted matches actual role needs
- [ ] Any over-provisioned access is removed
- [ ] Findings documented as a comment on the original onboarding issue

---

## Evidence Artifacts

| Artifact | Location | Retention |
|----------|----------|-----------|
| Onboarding access request issue | GitHub `truecalling-incidents` repo | Duration of employment + 3 years |
| MFA enrollment screenshot | `evidence/onboarding/` | Duration of employment + 3 years |
| Signed Acceptable Use Policy | `evidence/onboarding/` | Duration of employment + 3 years |
| API key log entry | `evidence/api-keys/openai-keys.md` | Duration of employment + 3 years |

---

## Roles & Responsibilities

| Who | Responsibility |
|-----|---------------|
| Hiring Manager | Initiates onboarding request; confirms role and access needs |
| Engineering Lead | Approves access request; executes provisioning; verifies MFA; closes issue |
| New Hire | Enrolls MFA on Day 1; signs Acceptable Use Policy; completes security training |

---

*Procedure Owner: Engineering Lead — stephane@truecalling.ai*  
*Next Review: 2027-05-27*
