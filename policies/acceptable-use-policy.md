# Acceptable Use Policy

**Organization:** TrueCalling.ai  
**Version:** 1.0  
**Effective Date:** 2026-05-27  
**Owner:** Engineering Lead  
**Review Cycle:** Annual  
**Related Policies:** Access Control Policy, Secrets Management Policy

---

## 1. Purpose

Define the acceptable use of TrueCalling.ai systems, data, and AI tools to protect company assets, customer data, and maintain SOC 2 compliance. This policy applies from Day 1 and acknowledgement is a condition of employment.

## 2. Scope

All employees, contractors, and vendors with access to any TrueCalling.ai system, including personal devices used to access company resources.

---

## 3. Acceptable Use

### 3.1 General

Employees may use company systems for work purposes and incidental personal use that:
- Does not consume significant resources
- Does not expose company data
- Does not violate any section of this policy

### 3.2 Data Handling

- **Customer data** must only be accessed for legitimate work purposes and must not be stored outside approved systems (Supabase, Vercel, company Google Drive)
- **Production data** must never be copied to personal devices, personal cloud storage, or unsanctioned SaaS tools
- **Data classification:** Treat all customer data and API keys as confidential by default; treat internal business data as internal; only marketing/public content is public

### 3.3 AI Tools (Claude Code, OpenAI APIs)

AI tools are powerful and subject to specific controls:

- **Approved AI tools:** Claude Code (Anthropic), OpenAI API, and any tool explicitly approved by the Engineering Lead
- Customer data or production database contents must **not** be pasted into any AI tool prompt unless the tool is explicitly approved for that data type
- AI-generated code that touches authentication, authorization, encryption, or data storage must be reviewed by a human engineer before merge
- Employees are responsible for the output of AI tools they use — AI does not transfer accountability
- Personal AI tool accounts must not be used for company work involving confidential data

### 3.4 GitHub and Source Code

- All work code must be committed to the company GitHub organization — not personal accounts
- Secrets, credentials, API keys, and passwords must **never** be committed to any repository (public or private)
- Force-pushing to `main` or `master` is prohibited without Engineering Lead approval
- Repository visibility must not be changed to public without Engineering Lead approval

### 3.5 Credentials and Authentication

- Credentials are personal and must not be shared under any circumstances
- MFA must remain enabled at all times on all company-connected systems
- If a credential is suspected to be compromised, report it immediately per the Incident Response Policy — do not attempt to quietly rotate without reporting

### 3.6 Third-Party Services

- New SaaS tools that will process company or customer data require Engineering Lead approval before use
- Employees must not grant third-party OAuth access to company GitHub, Supabase, or Vercel without approval
- Free tiers of tools must be assessed for data residency and privacy terms before use with company data

---

## 4. Prohibited Uses

The following are strictly prohibited and may result in immediate access suspension and disciplinary action:

| Category | Prohibited Actions |
|----------|-------------------|
| **Data exfiltration** | Copying customer data to personal storage, sharing production data externally without authorization |
| **Credential abuse** | Sharing passwords or API keys, creating backdoor accounts, bypassing MFA |
| **Unauthorized access** | Accessing systems beyond your assigned role, attempting to elevate privileges |
| **AI misuse** | Submitting customer PII or confidential data to unapproved AI tools |
| **Malicious activity** | Installing malware, running unauthorized scripts on production systems, intentional data destruction |
| **Policy circumvention** | Disabling security controls, bypassing code review, committing directly to protected branches |
| **Reputational harm** | Representing TrueCalling.ai on public platforms in ways not authorized by leadership |

---

## 5. Personal Device Use

Employees who access company systems from personal devices must:

- Keep the device OS and applications up to date
- Use the company-approved password manager — no browser-saved passwords for company accounts
- Not store company data locally without encryption
- Report a lost or stolen device to the Engineering Lead immediately — remote wipe may be initiated for company accounts

---

## 6. Monitoring

TrueCalling.ai reserves the right to monitor activity on company-owned systems and accounts for security and compliance purposes. Logs from GitHub, Supabase, Vercel, and AI tool APIs may be reviewed as part of incident investigations or SOC 2 audits. Employees should have no expectation of privacy on company-owned systems.

---

## 7. Reporting Violations

Suspected policy violations or security incidents must be reported to the Engineering Lead immediately via Slack `#security-incidents` or email. Reports made in good faith will not result in retaliation. Employees who observe a violation and do not report it may be subject to disciplinary action.

---

## 8. Acknowledgement

All employees and contractors must sign an acknowledgement of this policy on or before their first day. The signed acknowledgement is stored in `evidence/onboarding/<name>-<date>/` per the Employee Onboarding Procedure.

Continued access to company systems constitutes ongoing acceptance of this policy.

---

## 9. Violations and Consequences

| Severity | Example | Consequence |
|----------|---------|-------------|
| Minor | Incidental policy gap, promptly self-reported | Documented coaching |
| Moderate | Accidental data exposure, resolved promptly | Formal written warning, access review |
| Severe | Intentional data exfiltration, credential sharing, bypassing controls | Immediate access suspension, potential termination, legal referral |

---

*Policy Owner: Engineering Lead — engineering-lead@truecalling.ai*  
*Next Review: 2027-05-27*
