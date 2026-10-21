# Sitolo Phase Part Execution Contract

**Status:** Binding ICM planning rule for implementation phases 0–20.  
**Purpose:** Split work deliberately so requirements, package responsibilities, files, tests and acceptance evidence are not lost between broad phase milestones.

## 1. Working agreement

Phase breakdown is a joint planning responsibility. The agent investigates and proposes a bounded sequence; the human reviews, edits and agrees to it. The agent must not silently decide the complete part sequence for a phase, jump to later parts, or treat its own proposed breakdown as approved.

Before a phase begins implementation, ICM must produce a phase-part plan and obtain the required human review. The same rule applies when a later phase is first prepared and whenever material scope or architecture changes. Do not pre-implement future parts to be helpful.

A phase is not split merely by file count or crate count. A part should be a coherent, reviewable unit of behavior or foundation work. A feature may touch multiple packages, but each package's responsibility must remain explicit. Splitting work into parts must not split one invariant so that no part owns and tests the complete invariant.

## 2. Required phase-part plan

The Stage 04 Plan artifact must identify:

- **Phase identity:** phase number/name, canonical phase contract, approved phase baseline revision and predecessor-gate status.
- **Part sequence:** proposed part IDs and order, purpose, completion outcome, dependencies, explicit exclusions and stop/decision points.
- **Current part:** the exact part being implemented now, acceptance criteria, upstream evidence and the human approval status.
- **Requirement coverage:** each requirement/control maps to one or more explicit implementation responsibilities; no material requirement may be unassigned.
- **Package/file ownership:** every affected crate, executable app, client, migration/configuration path or generated artifact; exact intended paths; owner and responsibility; and a reason for every relevant package not touched.
- **Test placement:** exact test file/path for each behavior and invariant, located according to the code ownership map; identify unit, package integration, cross-package, database, frontend and end-to-end tests separately.
- **Verification:** exact package/app commands, required CI gates, expected results and the evidence each result should produce.
- **Architecture controls:** dependency direction, API/data contracts, security/tenant boundaries, migration/transaction effects, retries/concurrency, audit/observability and rollback where applicable.
- **Open questions:** assumptions, contradictions, dependencies, decision owner, disposition and which parts/phases are blocked.

Use a table similar to this one in the plan:

| Part | Outcome and requirements | Owning packages / paths | Tests in their owning location | Dependencies and acceptance evidence | Human decision |
|---|---|---|---|---|---|
| P<phase>.<part> | Exact behavior/foundation to establish | Every affected package and exact write-set paths | Exact test files and commands | Prerequisite state, proof and exit condition | Proposed / accepted / revise |

Use stable part identifiers only after agreement. If the phase-part plan changes, show the changed scope and request review before implementing the newly added work.

## 3. Non-negotiable implementation rules

1. Implement each responsibility in its approved owner. Do not put a missing crate's logic in another crate, add one catch-all module to hide unclear ownership, or duplicate the same business rule in API, worker, Tauri and React.
2. The plan's file write set is a boundary, not a suggestion. If a required file is missing from the plan or ownership is unclear, stop and return to Stage 04; do not silently broaden the diff.
3. Do not create empty or speculative crates. A new crate, package, top-level directory or owner requires an approved architectural reason and a map update.
4. Put tests inside the package/app that owns the behavior. The repository root tests/ directory is reserved for real cross-package/system behavior and repository-wide architectural constraints.
5. A cross-package capability must test each relevant public boundary in the relevant package and have a top-level journey test only when that end-to-end interaction itself needs proof. A root integration test does not replace the underlying crate's tests.
6. A phase may leave a package untouched only when its role has been considered and the plan records why. Never omit a package merely because the implementation agent did not inspect it.
7. Every meaningful behavior has explicit tests appropriate to its risk. A package that has no behavioral code may be marked not applicable with a reason; an empty or undiscovered test target is not a pass.
8. A passing aggregate command cannot hide an unbuilt/unexecuted package, a zero-test behavior surface, a skipped frontend app, or missing external infrastructure evidence.

## 4. ICM stage gates

- **01 Select:** select one phase and the current agreed part, or explicitly select a planning-only run to prepare/revise the part sequence. Record the previous part's gate and stop condition. Do not select a later part simply because it is documented.
- **02 Research / 03 Investigate:** verify version-sensitive claims and inspect actual repository state. List existing, missing, duplicate, conflicting and misplaced source/test paths with evidence. Never mistake a planned file for an existing implementation.
- **04 Plan:** construct the requirement-to-owner-to-file-to-test matrix and the phase-part sequence; explicitly mark unresolved architectural decisions. Human review is mandatory before implementation.
- **05 Implement:** change only the approved part write set. Keep implementation and tests in the correct owner. If the contract cannot be met without expanding scope or changing architecture, stop and ask for plan review.
- **06 Audit:** independently compare every requirement and affected owner with the diff, actual package boundaries, tests and dependency graph. Identify omitted crates and responsibilities placed in the wrong owner.
- **07 Remediate:** fix each finding in its owning package and add proof there. If a fix requires a new owner/path/part, return to Stage 04 for an approved plan change.
- **08 Verify:** run and report package-specific and app-specific checks. Distinguish pass, fail, blocked, not run and not applicable; record exact commands and revision. Do not claim a workspace green status when a relevant package or client was skipped.
- **09 Deliver:** report the exact part completed, files and packages affected, tests by owner, residual work, current gate, and the next agreed part or stop condition. Delivery does not approve the next part automatically.

## 5. Phase/part completion

A part can be reported complete only when:

- every in-scope requirement is implemented in its approved owner or explicitly accepted as deferred;
- no required crate/package/file silently dropped its responsibility;
- tests exist in the right package/app and the relevant commands actually ran, or limitations are clearly marked;
- the audit contains no unresolved material finding;
- the verification report identifies the tested revision and package-level results;
- changed architecture/file ownership is reflected in the approved map and relevant contracts;
- the human accepts the handoff before work advances to the next part.

Completing a part does not complete its phase unless the phase contract's entire exit gate is satisfied. A merge, a green unrelated test, or a long implementation report is not a phase/part acceptance decision.

## 6. Phase 1 enforcement foundation

Phase 1 must implement a required mechanical code-ownership/architecture gate from the human-approved Phase 0 map. It must validate actual Cargo package membership and dependency edges, the location of crate-owned tests, use of test-only packages, app-specific Tauri/React and Flutter test/build commands when those targets are due, and package-level check results. The missing checker must be reported as not yet implemented until it exists and is executed successfully.

**The target architecture inventory is not the same as the current workspace inventory.** The Phase 0 map and Phase 1 manifest must record `first_required_phase` and an explicit lifecycle/disposition for every planned package. The checker compares the real workspace against packages due by the current phase and accepted part, while continuing to validate all previously active packages. It must not require a future-phase package prematurely, and it must not let an in-scope missing package's responsibility be implemented in a neighboring crate. No empty or speculative crate is created solely to satisfy a path inventory.

The code ownership map is the human-readable decision view. Phase 1 may create a small structured manifest for machine enforcement after Phase 0 approves the package inventory and lifecycle; do not create parallel manifests that can disagree. Future entries remain visible as planned/deferred/optional until their due part creates real implementation and owner-local tests.