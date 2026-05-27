# Incident Issue Template

> Copy this into `truecalling-incidents` repo as `.github/ISSUE_TEMPLATE/incident.md`
> or use it manually when opening a new incident issue.

---

**Title format:** `[INCIDENT-<ID>] <P1/P2/P3/P4> — <brief description>`  
**Example:** `[INCIDENT-001] P2 — Suspicious auth activity on Supabase project`

---

## Incident Record

**Incident ID:** INC-  
**Severity:** P1 / P2 / P3 / P4  
**Status:** Open / Contained / Resolved  
**Incident Commander:** *(Engineering Lead for P1/P2, Senior Engineer for P3/P4)*  
**Detection Timestamp (UTC):**  
**Detection Method:** *(Supabase alert / GitHub alert / Customer report / Internal discovery / Other)*

---

## Affected Systems

- [ ] Supabase (database / auth)
- [ ] Vercel (hosting / edge functions)
- [ ] GitHub (source code / workflows)
- [ ] AI endpoints (OpenAI / Anthropic)
- [ ] User accounts
- [ ] Other: *(specify)*

**Customer data potentially exposed:** Yes / No / Unknown  
**Data types involved:** *(if applicable)*

---

## Timeline

| Timestamp (UTC) | Event |
|----------------|-------|
| | Incident detected |
| | Incident reported to Engineering Lead |
| | Severity assigned |
| | Containment started |
| | Containment complete |
| | Evidence collection complete |
| | Remediation deployed |
| | Incident resolved |

---

## Containment Actions

*(Document each containment action taken with timestamp)*

- 

---

## Evidence Collected

- [ ] Supabase audit logs exported — saved to: `evidence/incidents/<id>/`
- [ ] Vercel runtime logs exported
- [ ] GitHub audit log exported
- [ ] AI session logs saved (if applicable)
- [ ] Screenshots of anomalous state at detection
- [ ] IP addresses / user agents of anomalous requests logged

**Evidence location:** `evidence/incidents/INC-<id>-<YYYY-MM-DD>/`

---

## Root Cause

*(Completed during or after remediation)*

---

## Remediation Steps

*(Document each remediation action with timestamp)*

- 

---

## Resolution

**Resolved timestamp (UTC):**  
**Confirmed by:** *(Engineering Lead)*  
**Post-mortem required:** Yes (P1/P2) / No (P3/P4)  
**Post-mortem due:** *(5 business days from resolution — P1/P2 only)*  
**Post-mortem link:** *(when complete)*

---

## Customer / Regulatory Notification

*(P1/P2 with confirmed data exposure only)*

- [ ] CEO notified — timestamp:
- [ ] Legal / privacy counsel notified — timestamp:
- [ ] Customer notification sent — timestamp:
- [ ] Regulatory notification required: Yes / No — *(jurisdiction, if yes)*

---

*Retain this record for minimum 3 years per Incident Response Policy §7.*
