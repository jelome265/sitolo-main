# Sitolo — CI Enforcement Contract

**Status:** Required CI policy contract. Each referenced workflow/script must be verified against the current repository revision; a path named here is not proof the executable check exists or has run. See `docs/current_implementation_status.md` for the restart baseline.

## Purpose

CI is a release-control boundary. Local success is useful evidence; protected CI is the authoritative merge gate.

## Required classes of checks

### Repository/policy

- required governance files exist;
- workflow policy is valid;
- dependency and toolchain policy is enforced;
- lockfile and repository invariants are preserved.

### Rust/build

- formatting/checks/build/tests for every approved Cargo package and required app target;
- release/build verification where required;
- dependency policy and audit checks;
- actual package inventory and dependency direction must match the human-approved `docs/code_ownership_map.md`.

### Code ownership and test placement (Phase 1 required gate)

- a required, deterministic ownership checker compares Cargo metadata/package paths/dependency edges with the approved ownership manifest;
- no approved crate/app responsibility may be omitted, hidden in a neighboring crate, or implemented a second time in a different layer;
- each package's own tests run within that package's test target; root `tests/` is reserved for true cross-package/system or repository-wide architecture checks;
- test-only packages must not enter production dependency graphs;
- when present and in scope, Flutter and Tauri + React targets must have their own build/test commands and results;
- the gate fails when an expected package/target/test suite is missing, undiscovered, skipped without an approved reason, or mapped to the wrong owner.

**Current restart baseline:** these rules are mandatory design requirements, not a claim that an ownership checker or Rust/client test targets currently exist. Phase 0 must ratify the map; Phase 1 implements and runs the checker as a required CI control.

### Security

The security ICM path is conditionally enforced for security-relevant Engineering runs through the Stage 01 applicability decision and the control trace carried into Plan, Audit and Verify.

- security test suite/harness;
- static/security checks defined by the security workflow;
- secret/configuration boundary checks;
- relevant negative authorization and isolation tests.

### Integration

- integration tests and contract-level validation where configured;
- PostgreSQL/runtime boundaries where required.

### Agent/workspace integrity

- ICM workspace structure;
- stage contract shape and size;
- required references and output directories;
- system-map routing/twin integrity;
- absence of competing repository-global orchestration structures that violate the chosen architecture.

## Merge rule

A change that fails a required protected check is not release-ready. Agent-generated verification cannot override a failed CI control.

## Evidence

Each check should leave a machine-readable GitHub Actions result and, where useful, an artifact. A green workflow means the specific checked revision passed the configured checks; it does not prove requirements outside the workflow's scope.

## Change discipline

Workflow changes are security-sensitive. Pin third-party actions to immutable revisions, keep permissions least-privileged, avoid printing secrets, and review changes to trigger paths and protected branches.

## ICM relationship

CI verifies the filesystem workflow; CI does not become the workflow engine. The ICM pipeline remains filesystem-routed and human-auditable. CI is the mechanical verification layer around it.

## Canonical implementation references

- `.github/workflows/policy.yml`
- `.github/workflows/rust.yml`
- `.github/workflows/security.yml`
- `.github/workflows/integration.yml`
- `.github/workflows/release.yml`
- `scripts/ci/verify`
- `scripts/ci/verify.ps1`
- `scripts/ci/check-workflow-policy`
- `scripts/ci/check-icm-workspace`
- `scripts/ci/check-security-icm-policy`
- `docs/security_implementation_spec.md`
- `docs/testing_strategy.md`

This document defines the CI policy target; the repository must not describe its listed paths as operational until each file is present and its required check has been exercised.