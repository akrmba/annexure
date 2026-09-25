# Golden Checklist (Definition of Done)

Use this checklist for every PR/merge. If any item is “No”, do not merge.

## Preflight (before building)

- Confirm this work is net-new vs an extension of existing behavior; if extension, identify the existing spec/contract to update.
- Check for overlap with existing feature artifacts under `.specify/specs/*`; if overlap exists, reconcile instead of duplicating.
- Check for contract reuse: search `golden/contracts/*` for an existing interface/data shape before creating a new one.
- If anything is ambiguous or conflicting, STOP and ask (do not guess).

## Rule Lock gate (architecture + contracts)

- No core/domain module imports vendor SDKs; integration logic is isolated to adapters/connectors.
- External interfaces and data shapes are defined/updated only in `golden/contracts/*`.
- Specs reference contracts; specs do not redefine contract content.
- Tenant isolation rules are enforced in code paths and covered by tests (where applicable).
- Data retention behavior is defined and testable (where applicable).

## Style Lock gate (format + lint + types)

- Default style gate has been run:
  - `pre-commit run --all-files`
- `.pre-commit-config.yaml` exists at the repo root.
- Project-specific gate commands are defined for CI and are reproducible locally.

If the stack is UNKNOWN, fill these placeholders and wire them into CI:

- FORMAT_CMD=
- LINT_CMD=
- TYPECHECK_CMD=
- TEST_CMD=

If the stack is known, still keep the placeholders above but also record the concrete commands used by CI (example only; replace with real ones):

- FORMAT_CMD= (e.g., `pnpm format` / `go fmt ./...`)
- LINT_CMD= (e.g., `pnpm lint` / `golangci-lint run`)
- TYPECHECK_CMD= (e.g., `pnpm typecheck`)
- TEST_CMD= (e.g., `pnpm test` / `go test ./...`)

## Behavior Lock gate (tests + verification)

- Unit tests pass.
- Integration/contract tests pass (if present).
- Migrations (if any) are tested and reversible or have an explicit rollback procedure.
- No reduction in security posture (authz, encryption, logging, isolation) without an approved design change.

## Release readiness (when shipping)

- Rollout plan exists (feature flags, staged rollout, or version bump).
- Monitoring signals exist (logs/metrics/traces as applicable).
- A rollback path exists and is documented.

## Documentation

- Public interfaces are documented via contracts.
- Operator-only steps are documented only in `golden/OPERATOR_TASKS.md`.
- No secrets appear in docs, examples, or screenshots.
