# Azure Control Coverage — Gap Analysis

**Organization:** TrueCalling.ai
**Version:** 1.0
**Date:** 2026-06-02
**Owner:** Engineering Lead
**Finding ID:** F-16 (Azure control coverage) — see [Findings & Remediation Register](../../findings/findings-remediation-register.md)
**SOC 2 Criteria:** CC6.1, CC6.2, CC6.6, CC6.7, CC7.1, CC7.2, CC8.1, A1.2
**Related Risk:** R-016 (Azure migration control coverage) — to be added to the Risk Register

---

## 1. Why this document exists

The project state declares that **production is migrating from Vercel to Microsoft Azure**, that **Azure is the primary hosting platform**, and that **secrets now reside in Azure Key Vault**. However, every committed policy, procedure, and piece of evidence in this repository was authored against a **Vercel + Supabase + GitHub** stack. The word "Azure" did not appear anywhere in the control set prior to this document.

This is a **scope/coverage gap**: the documented control environment describes infrastructure that is being replaced. An external SOC 2 auditor scopes the audit to the **production system as it actually runs**. If production runs on Azure, then Azure access control, secret management, logging, change management, and backup/recovery are **in scope**, and the absence of Azure controls is a finding regardless of how strong the Vercel-era controls are.

This analysis maps each in-scope Trust Service Criterion to: the **current (pre-Azure) control**, the **required Azure control**, the **gap**, the **evidence** an auditor will expect, and the **owner/target**.

---

## 2. Migration status assumptions (confirm before audit)

These must be confirmed by the Engineering Lead; the gap ratings below assume the answer is "migration in progress."

| # | Question | Answer (fill in) |
|---|----------|------------------|
| A | Is production **live** on Azure today, still on Vercel, or split? | ☐ |
| B | Which Azure compute hosts the app (App Service / Container Apps / AKS / Functions)? | ☐ |
| C | Does Supabase remain the database, or is data moving to Azure (PostgreSQL Flexible Server)? | ☐ |
| D | Is identity Entra ID (Azure AD), and is Conditional Access available on the tenant? | ☐ |
| E | Azure region(s) in use (EU residency relevant for confidentiality/availability)? | ☐ |
| F | Is source/CI staying on GitHub, or moving to Azure DevOps? | ☐ |

Until A–F are answered, treat **both** Vercel-era and Azure controls as in scope (dual-running period).

---

## 3. Control coverage gap matrix

Legend — **Gap:** 🔴 None in place · 🟠 Partial / policy-only · 🟢 Covered.

| SOC 2 | Control area | Current control (pre-Azure) | Required Azure control | Gap | Evidence auditor expects |
|-------|--------------|-----------------------------|------------------------|-----|--------------------------|
| CC6.1 | Logical access / authn | GitHub org MFA; Supabase/Vercel logins (Access Control Policy §5) | **Entra ID** as IdP; **MFA via Conditional Access** enforced for all portal/CLI access; RBAC role assignments at subscription/RG scope | 🔴 | Entra Conditional Access policy screenshot; IAM role-assignment export per subscription/RG |
| CC6.1 | Privileged access | "Owner on GitHub/Vercel/Supabase" RBAC table | Azure **Owner/Contributor** assignments minimized; **PIM** (Privileged Identity Management) for just-in-time elevation if available; break-glass account documented | 🔴 | Role-assignment list; PIM config or documented compensating control |
| CC6.2 | Provisioning / deprovisioning | Onboarding/offboarding issue templates (4h SLA) | Entra ID joiner/mover/leaver applied to Azure roles; disable account → removes Azure access | 🟠 | One executed onboarding + offboarding showing Azure role grant/revoke |
| CC6.6 | Network boundary | Vercel-managed edge | **NSGs / Azure Firewall / Front Door + WAF**; private endpoints for Key Vault & DB; no public admin surface | 🔴 | Network topology + WAF/NSG config screenshots |
| CC6.7 | Secret management | Vercel env vars; Gitleaks/TruffleHog in CI; Secrets Mgmt Policy §3 | **Azure Key Vault** with **RBAC access**, **soft-delete + purge protection**, **diagnostic logging to Log Analytics**, rotation policies; app reads secrets via **managed identity** (no secrets in app settings) | 🟠 | Key Vault config screenshots (soft-delete/purge-protection/RBAC); Key Vault diagnostic settings; secret inventory (F-14 §3.8) |
| CC6.7 | Non-human credentials | GitHub PAT guidance | **Managed identities / OIDC federation** preferred over service-principal client secrets; any SP secrets have expiry and are inventoried | 🟠 | Entra app-registration export with credential expiry (F-14 §3.9) |
| CC7.1 | Vulnerability mgmt | npm audit + Dependabot | **Microsoft Defender for Cloud** (CSPM + workload protection); container image scanning if AKS/ACR | 🔴 | Defender for Cloud secure-score + recommendations export |
| CC7.2 | Monitoring / detection | GitHub Actions history; Supabase logs | **Azure Monitor + Log Analytics**; **Defender alerts**; activity log retained; diagnostic settings on Key Vault, app, DB | 🔴 | Log Analytics workspace + diagnostic-settings screenshots; sample alert |
| CC7.3 / 7.4 | Incident response | Incident Response Policy; `truecalling-incidents` repo | IR runbook references Azure alert sources and Azure account-recovery path | 🟠 | IR policy updated with Azure alert/escalation; one drill referencing Azure |
| CC8.1 | Change management | GitHub PR + CODEOWNERS + `security.yml`; branch protection | Deployment approvals/environments for Azure deploys; IaC (Bicep/Terraform) under PR review; pipeline identity is OIDC | 🟠 | Deployment-approval config; IaC repo/PR sample |
| A1.1 | Capacity / availability | Vercel platform | Azure autoscale / SKU sizing; availability target documented | 🟠 | Scaling config; availability design note |
| A1.2 | Backup & recovery | Supabase daily backups + PITR (untested — R-007) | **Azure Backup** / DB geo-redundant backups; documented RPO/RTO for Azure; **tested restore** | 🔴 | Backup config screenshot + completed Azure restore-test log |

---

## 4. Document/control artifacts that must change for Azure

These are concrete edits required so the documented control set matches the Azure system. Each is tracked as a sub-item of **F-16** in the register. *(Per current sprint scope, only item 1 is executed now; the rest are queued.)*

| # | Artifact | Required change | Status |
|---|----------|-----------------|--------|
| 1 | [Secrets Management Policy](../../policies/secrets-management-policy.md) §3 | Add **Azure Key Vault** as an approved secret store; note Vercel env vars are deprecated as production moves to Azure | ✅ Done 2026-06-02 (v1.1) |
| 2 | [Access Control Policy](../../policies/access-control-policy.md) §2, §4, §5 | Add Azure/Entra ID to scope; add Azure roles to the RBAC ceiling table; document Conditional Access MFA | ☐ Queued |
| 3 | [Disaster Recovery & BC procedure](../../procedures/disaster-recovery-and-business-continuity.md) | Add Azure backup/restore + Azure account-recovery path | ☐ Queued |
| 4 | [Risk Register](../risk-register/risk-register-2026.md) | Add **R-016** (Azure migration control coverage); revisit R-005/R-007/R-008 for Azure | ☐ Queued |
| 5 | [Vendor Management](../../policies/vendor-management-policy.md) + F-06 | Add **Microsoft Azure** to the sub-processor list; collect Azure/Microsoft SOC 2 + DPA | ☐ Queued |
| 6 | [README](../../README.md) | Reflect Azure as primary platform once migration confirmed | ☐ Queued |
| 7 | [API Key Inventory](../api-keys/openai-keys.md) §3.8–3.10 | Azure Key Vault + Entra identities + Azure DevOps | ✅ Done 2026-06-02 (F-14) |

---

## 5. Priority recommendations (SOC 2 Type I)

For a **Type I** (point-in-time **design**) opinion, the controls must be **designed and implemented** as of the report date — they do not yet need an operating-history. Therefore the fastest path to Type I readiness on Azure is:

1. **Enable and screenshot the Azure security baseline now:** Conditional Access MFA, Key Vault soft-delete + purge protection + RBAC + diagnostic logging, Defender for Cloud (free tier CSPM), and diagnostic settings → Log Analytics. These are configuration changes that establish *design* immediately.
2. **Inventory Azure secrets and identities** (F-14 §3.8–3.9).
3. **Document the Azure control design** in the policies/procedures listed in §4.
4. **Run one Azure restore test** to close the A1.2 design gap (and R-007).
5. **Add Microsoft Azure to vendor due-diligence** (Azure SOC 2 + DPA — Microsoft publishes these via the Service Trust Portal).

---

*Owner: Engineering Lead — engineering-lead@truecalling.ai*
*Next review: at migration cut-over, or 2026-09-01, whichever is first*
*Evidence retention: 3 years*
