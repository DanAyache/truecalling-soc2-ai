# Incident Response Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual

---

## 1. Purpose

This policy defines how TrueCalling.ai identifies, contains, investigates, and recovers from security incidents to minimize harm to customers, protect AI model integrity, and meet SOC 2 Type II requirements.

## 2. Scope

All systems and data operated by TrueCalling.ai including: Supabase (database/auth), Vercel (hosting/edge functions), GitHub (source code), AI model endpoints, third-party SaaS integrations, and any system processing customer or production data.

---

## 3. Severity Levels

| Severity | Definition | Examples | Response SLA |
|----------|------------|---------|--------------|
| **P1 — Critical** | Active breach, data exfiltration, or complete service outage | Unauthorized DB access, leaked service keys, AI model poisoning, production down | Respond: 15 min / Resolve or contain: 4 hr |
| **P2 — High** | Potential breach or significant degradation | Suspicious auth activity, failed MFA bypass attempts, Vercel deployment anomaly, AI output manipulation suspected | Respond: 1 hr / Resolve or contain: 24 hr |
| **P3 — Medium** | Policy violation or isolated anomaly with no confirmed data exposure | Unauthorized repo access, misconfigured Supabase RLS rule, dependency vulnerability (CVSS ≥ 7) | Respond: 4 hr / Resolve: 72 hr |
| **P4 — Low** | Informational or low-risk finding | Dependency vulnerability (CVSS < 7), failed login spike (non-targeted), access policy gap | Respond: next business day / Resolve: 2 weeks |

---

## 4. Incident Response Phases

### 4.1 Detection & Reporting

Incidents may be detected via:
- Supabase audit logs / database alerts
- Vercel deployment logs and error rates
- GitHub security alerts (Dependabot, secret scanning)
- AI model monitoring (anomalous output rates, prompt injection attempts)
- Customer reports or third-party disclosure

**Any team member who suspects an incident must report it immediately** by opening an incident ticket (GitHub issue in the private `truecalling-incidents` repo or designated Slack channel `#security-incidents`) with: what was observed, when, and on which system.

### 4.2 Triage & Severity Assignment

The first responder (Engineering Lead or on-call engineer) triages within the SLA for the suspected severity and:

1. Assigns a severity level (P1–P4)
2. Opens the incident record with timestamp, initial description, and affected systems
3. Notifies the Incident Commander (IC) — Engineering Lead for P1/P2, senior engineer for P3/P4

### 4.3 Containment

Containment actions taken immediately upon confirmation:

| System | Containment Action |
|--------|-------------------|
| Supabase | Revoke compromised keys; enable RLS lockdown; pause affected API routes |
| Vercel | Roll back deployment; disable affected environment variables; block IP via edge config |
| GitHub | Revoke compromised tokens/PATs; remove unauthorized collaborators; enable branch protection |
| AI endpoints | Rate-limit or disable affected model routes; flag affected sessions for review |
| User accounts | Force-logout affected sessions; disable compromised accounts; reset credentials |

Containment decisions are documented in the incident record in real time.

### 4.4 Evidence Collection

Evidence must be preserved before remediation changes overwrite logs. Collect and attach to the incident record:

- **Supabase:** Export audit logs from the dashboard (Auth > Logs, Database > Logs) for the relevant time window; snapshot affected table data if tampered
- **Vercel:** Export runtime logs and deployment history for affected functions/projects
- **GitHub:** Export audit log (Organization > Settings > Audit Log); capture commit SHAs, PR history, and Actions run logs
- **AI systems:** Save raw request/response logs for affected sessions; capture prompt injection payloads if applicable
- **Network/auth:** Capture IP addresses, user agents, and timestamps of anomalous requests
- **Screenshots:** Capture any dashboards or alerts showing the anomaly at time of detection

Evidence files are stored in the incident record with immutable timestamps. Do not modify source systems until evidence is preserved.

### 4.5 Eradication & Remediation

After containment and evidence collection:

1. Identify and remove the root cause (e.g., revoke leaked key, patch vulnerability, remove unauthorized access)
2. Rotate all credentials that may have been exposed
3. Apply and verify the fix in a non-production environment before deploying
4. Deploy the fix via a reviewed GitHub pull request (no direct pushes to main during an active incident unless required for immediate containment)

### 4.6 Recovery

1. Restore affected services in a controlled sequence, starting from lowest-risk components
2. Monitor Supabase, Vercel, and AI endpoints for 24 hours post-recovery for signs of re-compromise
3. Confirm with the IC that the incident is resolved before closing

### 4.7 Post-Incident Review

A written post-mortem is required for all P1 and P2 incidents, due within **5 business days** of resolution. Template:

- **Timeline** of events from detection to resolution
- **Root cause** analysis
- **Impact** — systems affected, data exposure scope, customer impact
- **What worked / what didn't** in the response
- **Action items** with owner and due date (prioritized in next sprint)

Post-mortems are stored in the `truecalling-incidents` repo and reviewed by the Engineering Lead.

---

## 5. Escalation Process

```
Detection
    │
    ▼
First Responder (any engineer)
    │  ─ Triage & assign severity
    │  ─ Open incident record
    │
    ├── P3/P4 ──► Senior Engineer (IC)
    │                │
    │                └── Resolve within SLA; Engineering Lead notified
    │
    └── P1/P2 ──► Engineering Lead (IC) — notify within 15 min
                     │
                     ├── Confirmed breach / data exposure
                     │       └── Notify CEO within 1 hr
                     │           └── Legal/privacy counsel within 2 hr
                     │               └── Customer notification (if required) within 72 hr
                     │
                     └── No confirmed exposure
                             └── Resolve internally; document outcome
```

**Customer notification** is required when a breach results in unauthorized access to customer data. Notification is sent within 72 hours of confirmation, including: what happened, what data was affected, what TrueCalling.ai has done, and recommended customer actions.

**Regulatory notification** (if applicable under GDPR or state privacy laws) is coordinated with legal counsel.

---

## 6. Roles & Responsibilities

| Role | Responsibility |
|------|---------------|
| Any team member | Report suspected incidents immediately |
| First Responder | Triage, severity assignment, initial containment |
| Incident Commander (IC) | Owns the incident end-to-end; coordinates response |
| Engineering Lead | IC for P1/P2; approves customer and regulatory notifications |
| CEO | Notified on confirmed P1 breaches; approves external communications |

---

## 7. Incident Record Requirements

Every incident (P1–P4) must have a documented record containing:

- [ ] Unique incident ID and severity
- [ ] Detection timestamp and detection method
- [ ] Affected systems and data types
- [ ] Containment actions with timestamps
- [ ] Evidence artifacts collected
- [ ] Remediation steps taken
- [ ] Resolution timestamp and confirmed resolution method
- [ ] Post-mortem link (P1/P2 only)

Records are retained for a minimum of **3 years**.

---

## 8. Testing

The incident response process is tested at least **annually** via a tabletop exercise simulating a realistic scenario (e.g., leaked Supabase service key, AI prompt injection attack). Findings from the exercise are documented and tracked as action items.

---

*Policy Owner: Engineering Lead — stephane@truecalling.ai*  
*Next Review: 2027-05-27*
