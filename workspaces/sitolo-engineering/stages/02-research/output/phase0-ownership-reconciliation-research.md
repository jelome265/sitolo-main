---
artifact_type: research-memo
status: draft
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "02-research"
researched_on: "2026-10-21"
---

# Phase 0 — Ownership Reconciliation Research

## Research question

How should Sitolo's approved ownership map and Phase 1 enforcement distinguish the complete target architecture from the packages and paths that must exist now, while ensuring a required responsibility is never omitted or moved into a neighboring crate?

## Source hierarchy used

The repository's documented hierarchy gives applicable law and current regulator requirements precedence, followed by current external-provider contracts, approved product/business decisions, security architecture and implementation contracts, the domain model, database/API/integration specifications, testing/observability/deployment, ADRs, and finally implementation convenience.

For this ownership question, the principal repository sources are:

- `docs/system_architecture_design.md` — target architecture and package topology.
- `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` — Phase 1 workspace/build contract and candidate crate list.
- `docs/implementation_plan.md` — phase order and broad dependency chain.
- `docs/ADR-001-025.md` — accepted architecture decisions, including Rust-first backend, Tauri desktop, PostgreSQL authority and modular monolith; the historical Flutter decision is superseded by accepted ADR-026 for React Native.
- `docs/code_ownership_map.md` — proposed package/path/test ownership.
- `docs/phase_part_execution_contract.md` — human-approved write set, package-local tests and part acceptance.
- `docs/testing_strategy.md` and `docs/ci_enforcement.md` — test evidence and CI gate expectations.

## Evidence and findings

### 1. Rust/Cargo package and test boundaries

The official Cargo Book states that a workspace manages multiple packages and that workspace members share one root `Cargo.lock` and build output directory. It also distinguishes a workspace's explicit membership from the broader directory tree. The Rust Book describes unit tests as commonly residing with the code under test and integration tests in a package's `tests/` directory, where each integration test file exercises the library through its public API.

- Cargo Book, Workspaces: https://doc.rust-lang.org/cargo/reference/workspaces.html
- Rust Book, Test Organization: https://doc.rust-lang.org/book/ch11-03-test-organization.html

**Implication for Sitolo:** verify actual Cargo metadata and explicit package paths. A root test suite must not substitute for crate-local behavior tests. The ownership checker must report which package/test target was discovered and executed, not just whether a workspace command returned success.

### 2. Tauri and React separation

Tauri v2 documentation shows a standard project split between a JavaScript/TypeScript frontend and a Rust package under `src-tauri/`; the default layout includes a Tauri configuration, capability definitions, a Rust `lib.rs` and a thin `main.rs` entry point. Tauri also distinguishes mock-runtime unit/integration tests from actual WebDriver end-to-end testing.

- Tauri, Project Structure: https://v2.tauri.app/start/project-structure/
- Tauri, Tests: https://v2.tauri.app/develop/tests/
- Tauri, WebDriver: https://v2.tauri.app/develop/tests/webdriver/

**Implication for Sitolo:** React feature/UI state, native OS integrations, Tauri command capability boundaries and desktop end-to-end journeys need separate owners and evidence. The map now includes `apps/desktop/src-tauri/src/lib.rs` as the app builder/command registration surface and keeps `main.rs` thin (change on this PR branch: commit `7c69ce5effecd6a40ec8482bd25689453a4f336a`).

The lockfile is a decision point: an isolated default Tauri Rust package normally carries its own lockfile, while a Cargo workspace member participates in the root workspace lockfile. Phase 0 must choose one arrangement and Phase 1 must verify it with the actual build tools.

### 3. React code organization

React's official guide recommends splitting components into files when that improves reuse and scanability; it does not mandate one universal source/test directory convention.

- React, Importing and Exporting Components: https://react.dev/learn/importing-and-exporting-components

**Implication for Sitolo:** colocated feature/component tests are a Sitolo convention chosen for ownership visibility, not a React framework requirement. App integration and genuine UI/native end-to-end tests remain in desktop-owned test areas.

### 4. Architecture inventory conflicts

The system architecture's repository topology lists the main libraries `sitolo-domain`, `sitolo-application`, `sitolo-api`, `sitolo-persistence`, `sitolo-sync`, `sitolo-integrations`, `sitolo-jobs`, `sitolo-observability` and `sitolo-auth`, with `sitolo-cli` conditional. The Phase 1 document instead describes a 14-crate candidate list that adds `sitolo-authz`, `sitolo-tenancy`, `sitolo-events`, `sitolo-audit`, `sitolo-config` and `sitolo-testkit`, while omitting `sitolo-jobs` and retaining an optional CLI only in the other document.

**Implication for Sitolo:** do not silently merge the two lists, and do not choose solely based on which document contains more detail. Decide the target responsibilities, dependency direction, first-required phase and whether each proposed library provides a real boundary. Do not create empty crates as placeholders.

### 5. Version-sensitive claim needing Phase 0 disposition

`docs/implementation_plan.md` and the Phase 1 contract state that Rust 1.98.1 is the approved baseline as of 2026-09-05. The official Rust Release Team announced Rust 1.99.1 on 2026-10-15.

- Rust 1.99.1 release announcement (2026-10-15): https://blog.rust-lang.org/2026/10/15/Rust-1.99.1/
- Official release list: https://blog.rust-lang.org/releases/

**Implication for Sitolo:** the older statement is no longer a claim about the latest stable release. This does **not** mean we should automatically change the project pin. Phase 0 must decide whether to retain or update the approved compiler baseline after reviewing compatibility and the repository's validation requirements. No compatibility test has been run in this planning-only research.

## Accepted mobile-framework decision update (2026-10-21)

The product owner explicitly approved React Native as the sole mobile framework and prohibited Flutter. This supersedes the earlier Flutter selection recorded in ADR-002; see the active [ADR-026](../../../../docs/ADR-026-react-native-mobile.md).

Current official React Native sources:

- Tutorial/native components: https://reactnative.dev/docs/tutorial
- Environment setup and distinction between using a framework or the Community CLI: https://reactnative.dev/docs/set-up-your-environment
- Testing overview (static analysis, modular/testable code, Jest in the default template): https://reactnative.dev/docs/testing-overview
- Release announcements: https://reactnative.dev/blog

The official React Native blog lists an announcement for 0.88 dated 2026-10-19. The environment/setup approach and compatible current version must still be checked in Phase 1; this memo does not choose Expo versus Community CLI or pin React Native tooling. Offline persistence, outbox/replay, secure local state and synchronization remain Sitolo-specific correctness requirements rather than framework guarantees.

## Recommendation

Keep a complete human-readable target ownership map, but use a phase-aware machine-readable manifest in Phase 1. The target client decision is React Native mobile plus Tauri/React desktop; do not add Flutter manifests, Dart source or Flutter test targets. Each planned package should have a sole responsibility, path, dependency allowlist, test location and `first_required_phase`; lifecycle/disposition must distinguish planned, active, deferred, optional and retired entries. The gate should fail for missing packages due in the accepted phase/part, not for a mapped future package that is not yet required. Once an owner is due, the missing owner blocks implementation; it never authorizes putting the behavior elsewhere.

Research supports conventions and identifies conflicts. It does not by itself approve Sitolo's crate inventory, lockfile topology, toolchain or current Phase 0 parts.
