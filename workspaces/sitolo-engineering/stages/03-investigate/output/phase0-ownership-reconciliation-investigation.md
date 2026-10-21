---
artifact_type: investigation
status: draft
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "03-investigate"
reviewed_pr: "https://github.com/jelome265/sitolo-main/pull/1"
reviewed_head: "2d15034ac0df97c02fdcb7610ccb48780236b074"
---

# Phase 0 — Ownership and PR Investigation

## Review scope and evidence

Reviewed PR #1's metadata, the full changed-file list and patch, current review submissions/threads, required ICM contracts and the source architecture/Phase 1/testing documents. The PR was open, not merged and marked mergeable at the inspected head. No submitted reviews or inline review threads were returned. GitHub returned no status checks and no PR workflow runs for commit `2d15034ac0df97c02fdcb7610ccb48780236b074`.

Direct repository-content checks on the PR branch returned 404 for the following expected product paths at the inspected baseline:

- `Cargo.toml` and `Cargo.lock`;
- `crates/sitolo-domain/Cargo.toml`;
- `apps/desktop/package.json` and `apps/desktop/src-tauri/Cargo.toml`;
- `apps/mobile/pubspec.yaml`;
- `scripts/ci/check-code-ownership` and `scripts/ci/verify`.

That supports the conclusion that this branch is currently an architecture/ICM document corpus, not an implemented Rust workspace or runnable Tauri/React or Flutter application. No build, runtime test suite or local checkout was executed as part of this investigation.

## Findings

### F1 — Conflicting target package inventories (Phase 0 decision)

The system architecture's target list and the Phase 1 candidate list are not the same. The former includes `sitolo-jobs` and optional `sitolo-cli`; the latter includes additional `authz`, `tenancy`, `events`, `audit`, `config` and `testkit` crates and does not list `sitolo-jobs`. The source architecture explicitly warns against a crate-per-noun split.

**Disposition:** unresolved intentionally. The human must decide the target set based on distinct responsibility, dependency/trust boundaries, consumers and tests. Every accepted target crate needs a precise purpose and dependency contract. A crate may be introduced in the phase where its first real responsibility is implemented, but that schedule cannot be used to hide its behavior in another package.

### F2 — Phase 1 gate risked treating every future package as immediately required (PR review finding; remediated in docs)

Before remediation, the ownership map's Phase 1 checker requirements said to fail on missing approved packages without defining whether the approved map represented all future packages or only the current phase's required package set. In combination with the broad target inventory, this could force empty placeholders or a false failure before future packages became due.

The ownership map and Phase Part Execution Contract now distinguish the target architecture from packages due by the accepted current phase/part. The proposed manifest has a `first_required_phase` and lifecycle/disposition, and the checker validates missing **due** packages while continuing to validate all previously active packages.

- `docs/code_ownership_map.md` — update commit `eec59c79de566c5cb7a9445eead960668df89352`, then Tauri map addition commit `7c69ce5effecd6a40ec8482bd25689453a4f336a`.
- `docs/phase_part_execution_contract.md` — update commit `894087c9b2a6deb7bc832007f2e658673f50ea14`.

This is a documentation-level remediation, not proof that the checker exists. Its implementation remains a Phase 1 task.

### F3 — Tauri file map omitted an application-library source file (PR review finding; remediated in docs)

The Tauri tree originally listed only `src/main.rs`, commands and adapters. Current Tauri v2 structure documents both `src/lib.rs` and `src/main.rs`, with the library holding application setup and the executable entry delegating to it. The ownership map now explicitly maps the app builder and command registration in `apps/desktop/src-tauri/src/lib.rs` and keeps `main.rs` thin.

Disposition: map corrected; Phase 0 still must ratify whether this is the final source layout and whether the Tauri Rust package joins the root workspace.

### F4 — Toolchain baseline needs a current decision, not automatic updating

The project documents pin Rust 1.98.1 as current on September 5, while Rust 1.99.1 was officially released on October 15, 2026. A newer stable version does not itself establish that Sitolo should change its pin: compiler/dependency compatibility and reproducible verification must be reviewed.

**Disposition:** open Phase 0 decision. No compiler change is authorized by this investigation.

### F5 — Tauri/Rust lockfile and workspace boundary unresolved

A Cargo workspace shares a root lockfile among its members. Standard Tauri projects place a separate Rust package under `src-tauri/` and normally show a lockfile there. These topologies cannot both be treated as the sole lockfile authority without a deliberate workspace decision.

**Disposition:** choose either root-workspace membership with one root lockfile, or an isolated Tauri Rust package with its own lockfile and separately declared CI commands. Verify the selected topology in Phase 1 with Cargo metadata, Tauri build commands, lockfile-change checks and CI.

### F6 — API library vs executable, worker vs jobs, migration ownership, and optional CLI remain decisions

- `crates/sitolo-api` and `apps/api` can be distinct only if the crate owns reusable HTTP routes/transport and the app is a thin composition root; neither may duplicate use cases.
- `apps/worker` owns process lifecycle and dependency wiring. If `sitolo-jobs` is retained, its scope must be reusable job registration/dispatch/handler infrastructure and not a second application-use-case layer. The application owns business workflows.
- The root `migrations/` path is already proposed by the system architecture, while the Phase 1 contract gives migration responsibility to persistence. A single physical migration sequence can remain at root with logical ownership assigned to `sitolo-persistence`; do not keep competing copies.
- `sitolo-cli` remains optional unless there is a concrete approved operational/developer use case.

These are not silently accepted merely because a candidate map describes them.

### F7 — Current PR has no automated verification evidence

No CI status or workflow run was returned for the inspected PR head, and the aggregate verifier/ownership checker does not exist. The PR must not be described as build/test verified. Since this work is documentation and policy, the appropriate available checks are reference resolution, internal-consistency review and explicit verification of unchanged/absent tool paths; no product test pass can be claimed.

## Review conclusion

PR #1 establishes the correct control direction: explicit owner mapping, owner-local tests, bounded parts, stop-and-replan rules, and blocking Phase 1 enforcement. Two ambiguities were found and corrected in the PR branch: phase-aware package expectations, and the Tauri `src/lib.rs` mapping. The architecture inventory, Tauri workspace/lockfile, toolchain pin and parts remain open Phase 0 decisions.

No Phase 0 architecture choice is recorded as approved by this investigation. Stage 04 must present a recommendation and bounded parts for human edit/acceptance; stop there.
