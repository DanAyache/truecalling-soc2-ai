<!--
  PR title must follow conventional commit format (enforced by CI):
  feat|fix|docs|chore|security|policy|refactor|ci: <description>

  SOC2 CC8.1 — All fields below are required for change management evidence.
  One-line descriptions will be flagged during audit. Be specific.
-->

## What changed

<!-- Describe the change clearly. What was added, removed, or modified? -->

## Why

<!-- What problem does this solve, or what requirement does this fulfil? -->

## How it was tested

<!-- How did you verify this works as intended?
     For policy changes: reviewed against SOC2 criteria?
     For code changes: local test, staging deploy, unit tests?
     For schema changes: tested against staging Supabase first? -->

## Change category

<!-- Select one per the Change Management Policy -->
- [ ] Standard — routine code change, bug fix, dependency update
- [ ] Significant — new feature, schema migration, new integration, access control change
- [ ] Security — workflow, policy, CODEOWNERS, secrets, or auth logic change
- [ ] Emergency — post-hoc PR for an incident fix (link incident issue below)

## Rollback plan

<!-- How would this be reversed if it causes a problem in production?
     e.g. "Revert this PR", "Run reverse migration file X", "Redeploy previous Vercel build" -->

## Linked issues

<!-- Link any related GitHub issues. Use "Closes #123" to auto-close on merge. -->

---
*Reviewer: confirm the description, test evidence, and rollback plan are present before approving (SOC2 CC8.1).*
