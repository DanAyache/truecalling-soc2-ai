# Security Policy — TrueCalling.ai

## Reporting a Vulnerability

If you discover a security vulnerability in TrueCalling.ai's systems, services, or infrastructure, please report it responsibly. We take all reports seriously and will respond promptly.

**Contact:** [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai)  
**Backup contact:** [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai)  
**PGP key:** *(not currently published — plain email is acceptable)*

Please do **not** open a public GitHub issue for security vulnerabilities. Use the email addresses above.

---

## What to Include in Your Report

To help us triage and reproduce the issue quickly, include:

- A description of the vulnerability and its potential impact
- The affected system or endpoint (e.g., the TrueCalling.ai web app, API, authentication flow)
- Step-by-step reproduction instructions
- Any proof-of-concept code, screenshots, or request/response samples
- Your name and contact information (for coordinated disclosure credit)

---

## Our Response Commitment

| Milestone | Target SLA |
|-----------|-----------|
| Acknowledgement of report | 2 business days |
| Initial severity assessment | 5 business days |
| Remediation for Critical / High findings | 7 days (Critical) / 30 days (High) |
| Remediation for Medium / Low findings | Next planned release |
| Coordinated disclosure notification | Before public disclosure |

We will keep you updated at each milestone. If you do not receive an acknowledgement within 2 business days, follow up at [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai).

---

## Scope

**In scope:**
- TrueCalling.ai web application and APIs
- Authentication and session management
- Data storage (user data, call records, AI outputs)
- Infrastructure components affecting confidentiality, integrity, or availability of customer data

**Out of scope:**
- Denial-of-service attacks (volumetric, application-layer)
- Social engineering of TrueCalling.ai employees
- Physical security attacks
- Vulnerabilities in third-party services (report these directly to the vendor)
- Issues requiring physical access to a user's device
- Previously known vulnerabilities without a new exploitation vector

---

## Coordinated Disclosure Policy

We follow responsible disclosure:

1. Report the vulnerability privately using the contact above
2. Give us reasonable time to assess and remediate before public disclosure
3. We will work with you to agree on a disclosure timeline — typically 90 days from acknowledgement for non-critical issues, shorter for critical
4. We will publicly credit you in our disclosure unless you prefer to remain anonymous

We will not pursue legal action against researchers who follow this policy and act in good faith.

---

## Security Contacts

| Role | Contact |
|------|---------|
| Security reports | [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai) |
| Engineering Lead | [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai) |
| Incident escalation | [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai) |

---

## Internal Security Processes

TrueCalling.ai maintains a SOC 2 readiness and compliance program. Policies, procedures, and audit evidence are maintained in a private repository and are undergoing continuous implementation and review. For audit inquiries or enterprise security questionnaires, contact [engineering-lead@truecalling.ai](mailto:engineering-lead@truecalling.ai).

---

*Policy Owner: Engineering Lead*  
*Effective: 2026-05-31*  
*SOC 2 Criteria: CC7.3 (vulnerability disclosure and remediation)*
