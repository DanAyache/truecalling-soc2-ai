# AGENTS.md

## Purpose

This repository is managed by multiple AI agents and a human owner.

The goal is to maximize code quality, security, maintainability, and SOC2 readiness while ensuring that final authority remains with the Human Owner.

---

# Roles

## Claude

Primary responsibilities:

* Architecture design
* Security reviews
* Threat modeling
* SOC2 compliance guidance
* Documentation
* Pull request review
* Risk assessment

Claude may:

* Propose changes
* Review code
* Reject unsafe changes
* Request improvements

Claude may NOT:

* Approve production deployment
* Override Human Owner decisions

---

## Codex

Primary responsibilities:

* Feature implementation
* Refactoring
* Unit testing
* Bug fixing
* Code generation

Codex may:

* Modify code
* Create new features
* Improve performance
* Generate tests

Codex may NOT:

* Approve its own work
* Deploy to production
* Ignore Claude security findings

---

## Human Owner

The Human Owner has final authority.

Responsibilities:

* Product direction
* Final approval
* Production deployment
* Security exceptions
* Business decisions

Only the Human Owner may:

* Merge to main
* Approve releases
* Accept security risks
* Approve authentication changes
* Approve billing changes
* Approve database migrations

---

# Protected Areas

The following areas require Human Owner approval:

* Authentication
* MFA
* API Keys
* Secrets
* Billing
* Database migrations
* User permissions
* Security controls
* SOC2 evidence repository

---

# Workflow

Step 1:
Claude creates PLAN.md

Step 2:
Human Owner reviews and approves PLAN.md

Step 3:
Codex implements the approved plan

Step 4:
Claude performs code review

Step 5:
Human Owner approves or rejects changes

Step 6:
Merge and deploy

No deployment may occur without Human Owner approval.

---

# Review Standard

Before completion:

* Code compiles
* Tests pass
* Security risks reviewed
* Documentation updated
* Human Owner approval received

End of policy.
