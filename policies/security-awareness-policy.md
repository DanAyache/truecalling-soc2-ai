# Security Awareness Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policies:** Acceptable Use Policy, Access Control Policy, Incident Response Policy  
**SOC 2 Criteria:** CC9.2 (Security Awareness and Communication)

---

## 1. Purpose

Ensure all personnel understand their security responsibilities, recognize common threats, and can respond appropriately — reducing the risk of human-error incidents that compromise customer data or system integrity.

## 2. Scope

All full-time employees, part-time employees, and contractors with access to any TrueCalling.ai system, from their first day through the end of their engagement.

---

## 3. Required Training

### 3.1 Day 1 Onboarding Training

Every new hire and contractor must complete security awareness training **on or before their first day of access** to any company system. No system access is granted until training is confirmed complete.

**Day 1 training covers:**
- Acceptable Use Policy overview and acknowledgement
- Password and credential hygiene (password manager, MFA, no sharing)
- Secret handling — what constitutes a secret, how to store it, what to do if one is exposed
- Recognizing phishing and social engineering attempts
- How to report a security incident (`#security-incidents` Slack channel or Engineering Lead direct message)
- AI tool rules — what data may and may not be submitted to AI tools

**Completion evidence:** Screenshot of training completion or signed acknowledgement, stored in `evidence/onboarding/<name>-<date>/` per the Employee Onboarding Procedure.

### 3.2 Annual Refresher Training

All active personnel complete a security awareness refresher **once per calendar year**, typically scheduled in Q1. The Engineering Lead sends a reminder and tracks completion.

**Annual refresher covers:**
- Review of any policy changes made in the prior year
- Current threat landscape relevant to AI SaaS companies (prompt injection, supply chain attacks, credential stuffing)
- Lessons learned from any internal P1/P2 incidents in the prior year (anonymized where appropriate)
- Regulatory reminders (data handling, customer data classification)
- Updated phishing and social engineering scenarios

**Completion SLA:** All personnel must complete within **30 days** of the annual training being assigned. Non-completion is escalated to the Engineering Lead and logged.

### 3.3 Role-Specific Training (Engineering)

Engineers additionally complete training on:
- Secure coding practices relevant to the stack (Supabase RLS, Next.js/Vercel edge functions, API key handling)
- GitHub security features (secret scanning, Dependabot, CODEOWNERS)
- AI-specific risks: prompt injection, model poisoning, output validation, logging PII
- Incident response procedure walkthrough — what to do if you detect a breach

Engineering-specific training is completed within **30 days of joining** the engineering team and reviewed annually.

### 3.4 Annual Tabletop Exercise

Per the Incident Response Policy §8, the Engineering Lead runs an annual tabletop exercise simulating a realistic incident scenario. All engineering staff participate. Findings are documented and tracked as action items.

---

## 4. Training Content and Delivery

| Training Type | Format | Managed By | Frequency |
|---------------|--------|-----------|-----------|
| Day 1 onboarding | Self-paced doc or video + AUP sign-off | Engineering Lead | On hire |
| Annual refresher | Written module or video + completion acknowledgement | Engineering Lead | Annually (Q1) |
| Engineering role-specific | Internal doc + walkthrough session | Engineering Lead | On join + annually |
| Tabletop exercise | Live session (remote or in-person) | Engineering Lead | Annually |

Training materials are stored in the company Google Drive (internal, not public). The Engineering Lead updates materials annually or when a significant policy change or incident warrants it.

---

## 5. Completion Tracking

The Engineering Lead maintains a training completion log in `evidence/security-awareness/` with:
- Employee name
- Training type completed
- Date completed
- Method of verification (screenshot, signed doc, session attendance)

The log is updated within **2 business days** of each training completion and reviewed at each quarterly access review.

---

## 6. Evidence

| Evidence Item | Location | Retention |
|---------------|----------|-----------|
| Day 1 training completion | `evidence/onboarding/<name>-<date>/` | Employment + 3 years |
| Annual refresher completion log | `evidence/security-awareness/annual-<YYYY>.md` | 3 years |
| Role-specific training records | `evidence/security-awareness/engineering-<YYYY>.md` | 3 years |
| Tabletop exercise findings | `evidence/incidents/tabletop-<YYYY>.md` | 3 years |

---

## 7. Non-Completion Consequences

| Scenario | Action |
|----------|--------|
| Day 1 training not complete | System access is not granted until confirmed complete |
| Annual refresher not complete within 30 days | Engineering Lead escalates to manager; access review initiated |
| Annual refresher not complete within 60 days | Access suspension pending completion |
| Repeated non-compliance | Treated as a policy violation under the Acceptable Use Policy |

---

## 8. Phishing Awareness

TrueCalling.ai may conduct unannounced simulated phishing tests. Employees who click simulated phishing links will be:
1. Notified immediately that it was a test
2. Required to complete targeted phishing awareness training within 5 business days
3. Not subject to disciplinary action for the first incident

Repeated failures in simulated phishing tests trigger a review of that individual's access scope.

---

## 9. Policy Updates and Communication

When a policy or procedure is materially updated, the Engineering Lead notifies all affected personnel within **5 business days** via Slack `#general` or `#engineering`. Personnel acknowledge receipt of material updates by commenting on the relevant GitHub issue or signing an updated acknowledgement form.

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
