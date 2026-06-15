# SOC 2 Repository Review Report

**Repository:** `c:\Users\Lea Hattab\Desktop\SOC2`  
**Reviewer:** Codex  
**Review date:** 2026-06-08  
**Scope:** Read-only review of the SOC 2 workspace, including the top-level repository, `truecalling-soc2-ai`, `truecalling-incidents`, and current governance/evidence documents.  
**Changes made:** This report only. No evidence, policy, code, workflow, configuration, findings, risk, or existing documentation files were modified.

---

## Executive Summary

The repository is a strong SOC 2 readiness workbench, but it is not yet audit-ready. The main issue is not policy design; it is evidence completeness, evidence integrity, and scope consistency.

The highest-risk blockers are:

1. Supabase MFA remains unresolved/evidence-pending for privileged accounts.
2. API key inventory is still empty.
3. Backup restore capability has not been tested.
4. Scope documents still describe Azure as current production while later decisions say Azure is future-state/out of Type I scope.
5. Policy and scope approval records are incomplete.
6. Google Workspace is treated as in-scope in several documents, but access/MFA/RBAC evidence is absent.
7. The repository topology has nested git repositories without `.gitmodules`, plus a large vendored codebase inside the compliance repo.

The program is candid about many of these gaps, which is positive. The remaining risk is that the repository currently contains contradictory audit narratives that an auditor could treat as control-design weakness or evidence unreliability.

---

## Audit Risks

### AR-01 - Type I scope is not signed and still has placeholders

**Severity:** Critical  
**Files:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`

The scope document still contains:

- `Decision date: [YYYY-MM-DD]`
- `Approved by: [name, role]`
- Azure migration answers `[answer]`
- Type II target `[Q_ 20__]`

This directly affects F-01 and prevents a clean Type I boundary. A SOC 2 report depends on a clearly approved system description and as-of scope.

**Risk:** Auditor cannot rely on scope as management-approved evidence.

**Recommended action:** Complete and approve the scope document using actual approval date, business title, and final Azure/legacy scope language.

### AR-02 - Azure scope contradicts Owner decisions and current production claims

**Severity:** Critical  
**Files:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`, `truecalling-soc2-ai/evidence/api-keys/openai-keys.md`, `OWNER_DECISION_REVIEW.md`

The scope document says Azure is the "primary production platform" and production secrets reside in Azure Key Vault. `openai-keys.md` repeats that secrets now reside in Azure Key Vault. Later governance material says Azure is future-state and outside the Type I boundary.

**Risk:** This is a material audit-boundary inconsistency. If Azure is in scope, F-16 remains a Type I blocker. If Azure is out of scope, the documents overstate current production architecture.

**Recommended action:** Reframe Azure consistently as future-state unless production data/secrets are actually there today. If any production secret exists in Azure, keep Azure in scope and evidence all Azure controls.

### AR-03 - Findings and risk registers are out of sync

**Severity:** High  
**Files:** `truecalling-soc2-ai/findings/findings-remediation-register.md`, `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`

Examples:

- Findings register references R-016 for F-16, but R-016 is not present in the risk register.
- Risk register R-013 still references F-04, while findings register says F-14 is canonical.
- Findings register allows `Deferred`; risk register status legend does not.

**Risk:** Auditor sees weak governance hygiene and unreliable issue tracking.

**Recommended action:** Add R-016 or remove dependent references, align status values, and correct stale finding aliases.

### AR-04 - Policy approval records are missing for most policies

**Severity:** High  
**Files:** `truecalling-soc2-ai/policies/*.md`

Only the Information Security Policy has a full approval block. The other policies have effective dates and owners but no formal approval record.

**Risk:** CC1.x management commitment evidence is incomplete.

**Recommended action:** Add consistent management approval blocks to the eight subordinate policies using actual approval date and approver title.

### AR-05 - Compliance calendar still says Type II observation period started

**Severity:** Medium  
**File:** `truecalling-soc2-ai/templates/compliance-calendar.md`

The calendar states `SOC 2 Observation Period Start: 2026-05-27` and Q2 2026 is the "First quarter of observation period." Other documents say Type II is deferred and current engagement is Type I.

**Risk:** Creates confusion about whether operating effectiveness evidence is expected now.

**Recommended action:** Change calendar framing to Type I readiness calendar or explicitly mark Type II observation as not started.

---

## Security Risks

### SR-01 - Supabase MFA gap remains unresolved for privileged users

**Severity:** Critical  
**Files:** `truecalling-soc2-ai/evidence/access-control/mfa-enforcement/supabase-mfa-remediation-2026-06.md`, `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`

The remediation record says MFA was disabled for both privileged Supabase members and remains evidence-pending. The risk register still has R-002 in progress.

**Risk:** Production data access may be protected only by passwords for privileged accounts.

**Recommended action:** Require both members to enable TOTP MFA, capture member-level and team-level evidence, then update R-002 and F-19 only after evidence proves the control.

### SR-02 - API key inventory is empty

**Severity:** Critical  
**Files:** `truecalling-soc2-ai/evidence/api-keys/openai-keys.md`, `truecalling-soc2-ai/evidence/api-keys/api-key-inventory-worksheet.md`

The inventory states no provider has been enumerated and Active Keys has no rows.

**Risk:** No reliable way to detect stale, over-privileged, orphaned, or unrotated credentials.

**Recommended action:** Populate metadata-only inventory for all in-scope providers, reconcile screenshots, and update the findings/risk registers.

### SR-03 - Potential exposed Supabase API-key screenshot

**Severity:** High  
**Files:** `truecalling-soc2-ai/evidence/api-keys/screenshots/supabase-api-settings-2026-06.png`

A prior plan flags this screenshot as exposing the Supabase anon JWT value. The file is currently untracked in `truecalling-soc2-ai`.

**Risk:** If committed, the repository may contain secret or token material. Even anon keys can require rotation depending on exposure and project configuration.

**Recommended action:** Verify the screenshot visually, replace with redacted evidence, and rotate/purge if a live sensitive value was committed anywhere.

### SR-04 - Backup restore capability is unverified

**Severity:** High  
**File:** `truecalling-soc2-ai/evidence/backups/verification-logs/2026-05-27-backup-verification.md`

The backup file explicitly says the test was not performed and restore capability is unverified.

**Risk:** A1.2 recovery claims are not evidenced.

**Recommended action:** Execute restore to staging, document RPO/RTO and integrity checks, and only then mark R-007/F-20 mitigated.

### SR-05 - Key-person and ownership concentration

**Severity:** High  
**Files:** `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`, `truecalling-soc2-ai/.github/CODEOWNERS`

The risk register tracks single-owner dependency, personal GitHub account hosting, and need for a second owner. CODEOWNERS maps all sensitive paths to one GitHub user.

**Risk:** Weak separation of duties, continuity, and access resilience.

**Recommended action:** Add a second owner or documented break-glass owner and update CODEOWNERS/governance evidence accordingly.

### SR-06 - Product security risks are registered but not evidenced

**Severity:** Medium  
**File:** `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`

Prompt injection, provider outage/fallback, LLM data-use, and data-retention risks are documented but remain open or in progress.

**Risk:** AI-specific threats may be known but not controlled/evidenced.

**Recommended action:** Add concrete test evidence for prompt injection, provider fallback/degraded mode, data retention, and vendor data-use attestations.

---

## Missing Evidence

### ME-01 - Onboarding/offboarding evidence folders are empty

**Severity:** High  
**Files:** `truecalling-soc2-ai/evidence/onboarding/.gitkeep`, `truecalling-soc2-ai/evidence/offboarding/.gitkeep`

The evidence folders contain only `.gitkeep`, and F-07 remains open.

**Missing:** Executed onboarding package, executed offboarding package or synthetic offboarding test, access provisioning/deprovisioning timestamps, MFA enrollment, and acceptable-use acknowledgment.

### ME-02 - Vendor due diligence evidence is absent

**Severity:** High  
**Files:** `truecalling-soc2-ai/evidence/vendor/third-party-reviews/.gitkeep`, `truecalling-soc2-ai/evidence/vendor/exceptions/.gitkeep`

Vendor folders are empty while F-06 and R-012 require SOC 2 reports/DPAs for critical sub-processors.

**Missing:** SOC 2 reports, DPAs, vendor risk reviews, exceptions, and approval records.

### ME-03 - Google Workspace evidence is missing despite being in scope

**Severity:** High  
**Files:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`, `truecalling-soc2-ai/policies/vendor-management-policy.md`, `truecalling-soc2-ai/procedures/employee-onboarding-procedure.md`

Google Workspace appears in system scope, vendor scope, onboarding/offboarding procedures, and internal operations, but no Workspace user list, MFA enforcement screenshot, admin role review, SSO evidence, or audit-log export is present.

**Missing:** Google Workspace user/RBAC review, MFA enforcement, admin role list, SSO configuration, and offboarding audit-log sample.

### ME-04 - Change-management evidence folder is empty

**Severity:** Medium  
**File:** `truecalling-soc2-ai/evidence/change-management/.gitkeep`

The workflow supports change-management evidence, but there is no committed sample PR evidence pack or branch-protection evidence in this folder.

**Missing:** Representative PR approvals, branch-protection settings, required-status-check proof, and CODEOWNERS enforcement evidence.

### ME-05 - npm-audit and TruffleHog evidence folders are empty

**Severity:** Medium  
**Files:** `truecalling-soc2-ai/evidence/security-scans/npm-audit/.gitkeep`, `truecalling-soc2-ai/evidence/security-scans/trufflehog/.gitkeep`

Gitleaks evidence exists, and Dependabot evidence exists, but npm-audit and TruffleHog local evidence folders contain only `.gitkeep`.

**Missing:** Monthly exported artifacts/logs for npm audit and TruffleHog.

### ME-06 - Incident/postmortem operating evidence is minimal

**Severity:** Medium  
**Files:** `truecalling-soc2-ai/evidence/incidents/`, `truecalling-incidents/`

The incident repo private screenshot exists, but there are no sample incident issues, postmortems, tabletop results, or linked incident records in the SOC 2 evidence folder.

**Missing:** At least one tabletop or no-incident attestation, incident issue template execution proof, and postmortem evidence if any P1/P2 occurred.

### ME-07 - Data classification and retention/erasure schedule are absent

**Severity:** Medium  
**File:** `truecalling-soc2-ai/findings/findings-remediation-register.md`

F-23 remains open. This matters because the scoped data includes call recordings, transcripts, and voice-biometric data.

**Missing:** Data classification standard, retention schedule, deletion/erasure workflow, and owner approval.

---

## Documentation Inconsistencies

### DI-01 - README says "Policies (8 total)" but there are 9 policy files

**Severity:** Low  
**File:** `truecalling-soc2-ai/README.md`

The README lists 8 policies, but the repository has 9 policy files, including `acceptable-use-policy.md`.

### DI-02 - Azure is current production in some documents and future-state in others

**Severity:** Critical  
**Files:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`, `truecalling-soc2-ai/evidence/api-keys/openai-keys.md`, `OWNER_DECISION_REVIEW.md`, `DECISIONS.md`

This is the largest documentation inconsistency and should be corrected before any auditor-facing package is assembled.

### DI-03 - Vendor lists differ across documents

**Severity:** Medium  
**Files:** `truecalling-soc2-ai/SOC2-ENGAGEMENT-SCOPE.md`, `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`, `truecalling-soc2-ai/policies/vendor-management-policy.md`, `truecalling-soc2-ai/procedures/disaster-recovery-and-business-continuity.md`

Different files mention different vendor sets: OpenAI, Anthropic, Supabase, Vercel, GitHub, Twilio, FullEnrich, Microsoft Azure, Google Workspace, Google, 1Password, Slack.

**Risk:** Vendor due diligence scope is unclear.

### DI-04 - R-013 is Twilio-specific but API inventory risk is broader

**Severity:** Medium  
**File:** `truecalling-soc2-ai/evidence/risk-register/risk-register-2026.md`

R-013 describes Twilio credential compromise, while F-14/API inventory applies to all providers.

**Risk:** API key remediation can be under-scoped.

### DI-05 - Evidence collection procedure says not to overwrite corrected evidence, but backup gap file says replacement

**Severity:** Medium  
**Files:** `truecalling-soc2-ai/procedures/evidence-collection-procedure.md`, `truecalling-soc2-ai/evidence/backups/verification-logs/2026-05-27-backup-verification.md`

The evidence procedure says corrected evidence should add a new file with a correction note rather than overwrite originals. The backup gap record says the completed log will replace the file at the same path.

**Risk:** Evidence integrity expectations are inconsistent.

### DI-06 - Approval identities/titles are not consistently resolved

**Severity:** Medium  
**Files:** `AGENTS.md`, `APPROVAL_PACKAGE.md`, `OWNER_DECISION_REVIEW.md`, policy files

The internal role "Human Owner" is not the same as an auditor-facing management title. Approval package material recommends resolving this, but policy files still lack approved signer blocks.

### DI-07 - Root-level PLAN.md has mojibake/encoding artifacts

**Severity:** Low  
**File:** `PLAN.md`

The document contains garbled characters in headings and diagrams. This is not a control failure, but it weakens readability and presentation quality.

---

## Technical Debt

### TD-01 - Nested git repositories are committed as gitlinks without `.gitmodules`

**Severity:** High  
**Files:** top-level git index, missing `.gitmodules`

`truecalling-soc2-ai` and `truecalling-incidents` are mode `160000` gitlinks, but `.gitmodules` is absent.

**Impact:** A clone of the top-level repo will not reproduce the nested repositories correctly.

### TD-02 - Large vendored third-party tree inside compliance repo

**Severity:** Medium  
**Path:** `truecalling-soc2-ai/claude-code-plugins-plus-skills/`

This tree is very large and code-heavy compared with the compliance repository. The findings register already tracks this as F-21.

**Impact:** Larger scan surface, noisier diffs, harder audit review, and possible confusion about what is in scope.

### TD-03 - Dirty/untracked evidence state

**Severity:** Medium  
**Path:** `truecalling-soc2-ai/evidence/`

`git -C truecalling-soc2-ai status --short` shows multiple untracked evidence screenshots and a modified nested code tree.

**Impact:** Current evidence state is not committed and may not be reproducible or reviewed.

### TD-04 - Duplicate evidence/artifact locations

**Severity:** Medium  
**Paths:** repository root and `truecalling-soc2-ai/evidence/access-control/`

RBAC screenshots and exports exist both at the workspace root and under the evidence tree.

**Impact:** Two sources of truth and higher chance of stale evidence.

### TD-05 - `truecalling-incidents-setup` duplicates incident template content

**Severity:** Low  
**Paths:** `truecalling-incidents/`, `truecalling-incidents-setup/`

The setup directory appears to duplicate issue templates while the actual incident repo is a nested git repo.

**Impact:** Unclear canonical source for templates.

### TD-06 - Root repository has no top-level `.gitignore`

**Severity:** Low  
**Path:** root repository

The workspace has untracked `.vscode`, workbooks, PDFs, reports, and artifacts.

**Impact:** Higher chance of accidental local/editor or sensitive artifact commits.

---

## Priority Remediation Order

1. Resolve scope: Azure future-state vs current production, Google Workspace in/out, F-16 blocker status, R-016.
2. Close live security blockers: Supabase MFA, exposed screenshot review, API key inventory, backup restore test.
3. Add missing approvals: scope doc and eight subordinate policies.
4. Collect missing evidence: Google Workspace, onboarding/offboarding, vendor due diligence, change-management proof, npm-audit/TruffleHog exports.
5. Reconcile registers: F-14/F-04, R-013 scope, R-016, `Deferred` status semantics.
6. Clean documentation drift: README policy count, Type I/Type II calendar language, vendor lists, evidence correction procedure.
7. Address repository hygiene: submodules/gitlinks, vendored tree, duplicate artifacts, root `.gitignore`.

---

## Audit Readiness Assessment

**Current readiness:** Not audit-ready for SOC 2 Type I as of this review.

**Why:** The control design is mostly documented, but management approval, evidence completion, and scope consistency are not yet strong enough for a clean auditor-facing package.

**Most defensible path:** Treat the current production boundary as Vercel/Supabase/GitHub/Google Workspace unless Azure is truly live. Defer Azure to a future milestone, but remove all current-production Azure claims first. Then close F-19, F-14, F-20, F-01/A-6, and collect the Google Workspace and vendor evidence created by the final boundary decision.
