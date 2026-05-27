# Offboarding Issue Template

> Copy this into `truecalling-incidents` repo as `.github/ISSUE_TEMPLATE/offboarding.md`
> or use it manually when opening a new offboarding issue.

---

**Title format:** `[OFFBOARDING] <Full Name> — <Last Day>`  
**Example:** `[OFFBOARDING] Alice Martin — 2026-06-30`

---

## Departure Details

**Full Name:**  
**Role:**  
**Last Working Day:**  
**Departure Type:** Voluntary / Involuntary / Contractor end  
**Notified by:** *(manager / HR)*  
**Notification received:** *(date and time)*  
**Access revocation deadline:** *(4 hours from notification — enter exact time)*  
**Linked onboarding issue:** *(link — use to confirm all systems granted)*

> For involuntary terminations: Engineering Lead must be notified **30 minutes before** the departure conversation. Access is revoked before or simultaneously with notification to the employee.

---

## Shared Credentials to Rotate

*(List all shared secrets this person had access to — rotate within 24 hours)*

| Secret | Service | Rotation Due | Rotated? | Timestamp |
|--------|---------|-------------|---------|-----------|
| Supabase service role key | Supabase / Vercel env vars | | | |
| | | | | |

---

## Access Revocation Checklist

*(Work through in order. Check each box only after confirmed complete.)*

**SLA: All boxes checked within 4 hours of departure confirmation.**

#### Google Workspace
- [ ] Account suspended (not deleted)
- [ ] Signed out of all active sessions
- [ ] Removed from all Google Groups
- [ ] Drive files transferred to manager
- [ ] Timestamp:

#### GitHub
- [ ] Removed from org
- [ ] Removal confirmed (user loses access immediately)
- [ ] PATs revoked — check `evidence/api-keys/`
- [ ] OAuth grants removed (if any)
- [ ] If Owner role: org-level secrets rotated
- [ ] Screenshot saved: `evidence/offboarding/<name>-<date>/github-members-after-removal.png`
- [ ] Timestamp:

#### Supabase
- [ ] Removed from project team
- [ ] Removal confirmed (screenshot)
- [ ] If Developer+: service role key rotation triggered (see Shared Credentials above)
- [ ] Screenshot saved: `evidence/offboarding/<name>-<date>/supabase-members-after-removal.png`
- [ ] Timestamp:

#### Vercel
- [ ] Removed from team
- [ ] Project-level tokens revoked
- [ ] Google SSO block confirmed (via Google Workspace suspension)
- [ ] Screenshot saved: `evidence/offboarding/<name>-<date>/vercel-members-after-removal.png`
- [ ] Timestamp:

#### OpenAI API
- [ ] Named key revoked: *(key name)*
- [ ] `evidence/api-keys/openai-keys.md` updated with revocation date
- [ ] Screenshot saved: `evidence/offboarding/<name>-<date>/openai-key-revoked.png`
- [ ] Timestamp:

#### Anthropic / Claude Code
- [ ] Workspace access removed
- [ ] API key revoked and logged
- [ ] Timestamp:

#### Password Manager
- [ ] Removed from company password manager
- [ ] Shared passwords rotated where applicable
- [ ] Vault access log reviewed for exports in last 30 days
- [ ] Timestamp:

#### Other SaaS Tools
*(list from onboarding issue)*
- [ ] *(tool)*: revoked — Timestamp:

---

## Evidence Checklist

*(Collect after each revocation step above)*

- [ ] GitHub org member list — user absent
- [ ] Supabase team list — user absent
- [ ] Vercel team list — user absent
- [ ] OpenAI key status — revoked
- [ ] Google Workspace audit log — last 30 days exported as CSV
- [ ] GitHub org audit log — filtered by `actor:<username>` exported as CSV
- [ ] Credential rotation records — timestamped comments below

**Evidence saved to:** `evidence/offboarding/<full-name>-<YYYY-MM-DD>/`

---

## Credential Rotation Log

*(Comment here for each secret rotated, with timestamp)*

---

## Close Checklist

- [ ] All access revocation boxes checked
- [ ] All credential rotations complete
- [ ] All evidence artifacts saved and committed
- [ ] Manager / HR notified that revocation is complete

**Offboarding complete — all access revoked — verified by:** *(Engineering Lead GitHub handle)*  
**Date / time completed:** *(must be within 4 hours of departure)*

---

*Retain this issue for 3 years from departure date per Offboarding Procedure.*
