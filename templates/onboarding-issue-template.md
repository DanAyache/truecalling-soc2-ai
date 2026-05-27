# Onboarding Issue Template

> Copy this into `truecalling-incidents` repo as `.github/ISSUE_TEMPLATE/onboarding.md`
> or use it manually when opening a new onboarding issue.

---

**Title format:** `[ONBOARDING] <Full Name> — <Start Date>`  
**Example:** `[ONBOARDING] Alice Martin — 2026-06-01`

---

## Access Request

**Full Name:**  
**Role:** *(Engineering Lead / Senior Engineer / Engineer / Contractor / Support)*  
**Start Date:**  
**Employment Type:** Full-time / Part-time / Contractor  
**Contractor end date (if applicable):**  
**Requested by:** *(hiring manager)*  
**Approved by:** *(Engineering Lead signs off here — required before provisioning)*

---

## Systems Required

*(Check all that apply and specify the access level)*

- [ ] GitHub — Role: *(Write / Read / repo-scoped: specify repo)*
- [ ] Supabase — Role: *(Developer / Viewer / none)*
- [ ] Vercel — Role: *(Member / Viewer / none)*
- [ ] OpenAI API — Key name to create: `<name>-<role>-<YYYY-MM>`
- [ ] Anthropic / Claude Code — *(yes / no)*
- [ ] Password Manager — Vaults: *(specify)*
- [ ] Google Workspace — *(pre-created by admin / yes / no)*

**Business justification:** *(why this person needs each system)*

---

## Day 1 Checklist

- [ ] Google Workspace account created and credentials shared securely
- [ ] MFA enrolled on Google Workspace — method: TOTP (no SMS)
- [ ] **Engineering Lead verified MFA active before any other access granted**
- [ ] Security awareness training completed
- [ ] Acceptable Use Policy signed — saved to: `evidence/onboarding/<name>-<date>/aup-signed.pdf`
- [ ] MFA enrollment screenshot saved to: `evidence/onboarding/<name>-<date>/mfa-enrollment-<date>.png`

---

## Access Provisioning Checklist

*(Complete after Day 1 MFA confirmed)*

#### GitHub
- [ ] Org invite sent at correct role
- [ ] MFA confirmed via org settings (green shield)
- [ ] Added to correct team(s): *(specify)*
- [ ] For contractors: scoped to repo(s): *(specify)*

#### Supabase
- [ ] Invite sent at correct role
- [ ] Invite accepted and MFA confirmed
- [ ] Service role key NOT shared — confirmed

#### Vercel
- [ ] Invite sent at correct role (Google SSO only)
- [ ] Access confirmed via Google SSO

#### OpenAI API
- [ ] Named key created: *(key name)*
- [ ] Rotation due: *(90 days from today)*
- [ ] Logged in `evidence/api-keys/openai-keys.md`

#### Anthropic / Claude Code
- [ ] Access provisioned: *(yes / N/A)*
- [ ] Logged in API key inventory

#### Password Manager
- [ ] Added to correct vault(s)
- [ ] Access confirmed by new hire

---

## Verification Sign-off

- [ ] MFA active on all provisioned systems
- [ ] All access matches approved RBAC table in Access Control Policy
- [ ] No access granted beyond approved scope
- [ ] AUP signed and stored
- [ ] API keys named, scoped, and logged

**Onboarding complete — verified by:** *(Engineering Lead GitHub handle)*  
**Date completed:**

---

## 30-Day Review

*Due: *(30 days after start date)**

- [ ] Access reviewed — matches actual role needs
- [ ] Over-provisioned access removed: *(specify if any)*
- [ ] Notes:

**30-day review completed by:** *(handle)*  
**Date:**

---

*Retain this issue for duration of employment + 3 years per Onboarding Procedure.*
