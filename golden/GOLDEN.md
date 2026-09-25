# Annexure — Golden Rules (Repo Law)

This file defines the non-negotiable “golden gates” for how this repository is changed, verified, and shipped.

## Source of truth order (highest → lowest)

1. `golden/contracts/*`
2. `.specify/memory/constitution.md`
3. `golden/RULES.md`
4. `golden/CHECKLIST.md`
5. `.specify/specs/*` (feature artifacts)

If any lower-priority document conflicts with a higher-priority document, stop and resolve the conflict before proceeding.

## The three locks (must always hold)

### 1) Style Lock
All changes must pass formatting and repository-wide style checks.

### 2) Rule Lock
All changes must respect architecture constraints, boundaries, and the contract system in `golden/contracts/*`.

### 3) Behavior Lock
All changes must pass tests and verification steps proving the system still behaves as intended.

If any lock fails, do not merge.

## Contracts are law

- All external interfaces and data shapes live in `golden/contracts/*`.
- Specs and plans may reference contracts, but must never redefine them.
- Any contract change requires an explicit migration plan, compatibility note, and versioning decision (compatible vs breaking).

## Stop-and-ask rule (mandatory)

Stop and ask for clarification when any of the following occurs:
- Ambiguous requirements that affect user-visible behavior, security, compliance, cost, or performance.
- Conflicts between specs, contracts, constitution, or existing implementation.
- A change would bypass any of the three locks.
- A vendor/SDK choice would bleed into core/domain logic.

## Architecture boundaries (generic)

- Core/domain logic must not directly import vendor SDKs (CRM/ERP, cloud storage, email, chat, payments, analytics, etc.).
- Integrations must be implemented as adapters at the boundary (e.g., `adapters/`, `integrations/`, `connectors/`), with interfaces owned by the core.
- New integrations must be swappable: a new connector should not require rewriting core logic.

## Configuration and secrets

- Configuration is via environment variables only (no hardcoded environment-specific settings).
- Secrets must never be committed (including in examples, tests, fixtures, or screenshots).
- Any sample config files must contain placeholders only.

## Multi-tenant and data protection (generic)

- Tenant isolation is a first-class constraint: all data access must be scoped and test-covered.
- If the system processes personal data on behalf of customers, the repository must support controller–processor contract obligations operationally (security measures, audit support, deletion/return at end of service, and assistance with breach/DPIA obligations). [web:18]
- Data handling must follow applicable data protection principles (e.g., purpose limitation, data minimisation, storage limitation) and enforce retention controls in product and operations. [web:11]

## Repository structure expectations

- Golden gates (this law): `golden/`
- Contracts: `golden/contracts/`
- Checklists and operator runbooks: `golden/CHECKLIST.md`, `golden/OPERATOR_TASKS.md`
- Constitution: `.specify/memory/constitution.md`
- Specs and feature artifacts: `.specify/specs/`

## Change control

- Every change must reference a spec or decision record (or create one) unless it is a trivial doc/typo fix.
- Breaking changes require: contract versioning decision, migration plan, and explicit rollout/rollback steps.
- No “temporary” bypasses of gates; if a gate is wrong, fix the gate first.
