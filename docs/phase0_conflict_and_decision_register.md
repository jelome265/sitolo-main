---
artifact_type: conflict-and-decision-register
status: open
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "05-implement"
product_phase: "Phase 0 — Architecture, Contracts and ADR Freeze"
phase_part: "P0.1 — Source of truth and conflict register"
evidence_date: "2026-10-21"
inspected_baseline_revision: "64ad7bc05e5276c8abf75e05c74703d55c82256c"
implementation_revision: "251df6c327c1029c6c0db2e496d19ea38ea7e41e"
---

# Phase 0 — Conflict and Decision Register

## 1. What this document is

An evidence and routing artifact. It records where approved sources disagree, what remains undecided, what was checked against an external authority, and what is missing. It is **not** an architecture specification, not a decision, and not proof that any capability exists.

Every row points at a source that already exists. No row adds design prose of its own. Where a source names a section, the section is quoted or paraphrased narrowly enough to be checked against it.

**Authority:** this register is subordinate to `agent.md`'s source hierarchy, to the accepted ADRs, and to each canonical document it cites. If the register and a cited source disagree, the cited source wins and the register row is wrong.

**Nothing here is accepted.** Status values are `open`, `recommendation`, `proposed`, `deferred`, `settled-by-ADR` or `rejected`. The word "accepted" appears only where an ADR already holds that status.

## 2. Repository baseline inspected

| Fact | Value | Evidence |
|---|---|---|
| Inspected baseline revision | `64ad7bc05e5276c8abf75e05c74703d55c82256c` | `git rev-parse HEAD` at inspection time, before this register existed |
| Revision that introduced this register | `251df6c327c1029c6c0db2e496d19ea38ea7e41e` | Commit containing this file |
| Working tree during inspection | Dirty: P0.1 acceptance edits to this run's Stage 04 amendment were uncommitted | `git status` |
| Root `Cargo.toml` / `Cargo.lock` | absent | `Test-Path` returned false |
| Root `rust-toolchain.toml` | absent | `Test-Path` returned false |
| `.github/workflows/` | absent | `Test-Path` returned false |
| `scripts/ci/verify`, `scripts/ci/verify.ps1`, `scripts/ci/check-code-ownership` | absent | `Test-Path` returned false |
| Git-tracked files under `crates/` | none | `git ls-files crates` returned empty |
| Directories under `crates/` | 15, all empty (0 files recursively) | `Get-ChildItem -Recurse -File` returned 0 |

The inspected baseline is the state that was read, not the revision that contains this register. Line references in every row point at the inspected baseline; later commits shift them.

The revision actually verified after remediation is recorded in the Stage 08 verification report, not here.

The repository therefore contains documents, ICM workspaces and empty local directories. It contains no buildable Rust workspace, no client application and no executable CI check. No build, test or lint claim is made anywhere in this register.

## 3. Row vocabulary

| Prefix | Meaning | Requirement |
|---|---|---|
| `C` | Conflict between sources | Each competing statement is cited; a decision owner and resolution gate exist |
| `D` | Decision no approved source settles | Options, recommendation, owner, blocked work and gate exist |
| `V` | External/version claim verified against an official source | Source, date checked, verdict, accountable owner, resolution gate and status recorded; the verdict never decides the question |
| `G` | Gap or integrity defect | Observed state, check performed, owner and disposition |

## 4. Conflicts

### C-01 — Rust package inventory

| Field | Value |
|---|---|
| **Competing statements** | **A.** `docs/system_architecture_design.md` §7 (L376–405) lists `sitolo-domain`, `sitolo-application`, `sitolo-api`, `sitolo-persistence`, `sitolo-sync`, `sitolo-integrations`, `sitolo-jobs`, `sitolo-observability`, `sitolo-auth`, and `sitolo-cli` marked "if needed" (L393). **B.** `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §3 (L81–132) lists `sitolo-domain`, `sitolo-application`, `sitolo-api`, `sitolo-auth`, `sitolo-authz`, `sitolo-tenancy`, `sitolo-persistence`, `sitolo-events`, `sitolo-audit`, `sitolo-observability`, `sitolo-config`, `sitolo-integrations`, `sitolo-sync`, `sitolo-testkit`. |
| **Delta** | A has `sitolo-jobs` and conditional `sitolo-cli`; B has neither. B has `sitolo-authz`, `sitolo-tenancy`, `sitolo-events`, `sitolo-audit`, `sitolo-config`, `sitolo-testkit`; A has none. |
| **Impact** | Phase 1 workspace members; every later phase's owner assignment; the ownership checker's manifest. |
| **Constraint already agreed** | `docs/system_architecture_design.md` L407: "Do not create a crate for every noun." `docs/code_ownership_map.md` §2 item 4: no unsolicited crate proliferation. Merging both lists satisfies neither. |
| **Owner** | Product owner (architecture), on engineering recommendation |
| **Resolution gate** | P0.2 (inventory and dependency graph); P0.3 (executables); P0.6 (lifecycle) |
| **Status** | **open** |

### C-02 — Third repository topology in the testing strategy

| Field | Value |
|---|---|
| **Competing statements** | **A.** `docs/testing_strategy.md` §39 (L780–794) presents a "Suggested repository structure" with `crates/domain/`, `crates/application/`, `crates/infrastructure/`, `crates/api/` and a separate `integration-tests/` directory. **B.** Both C-01 sources use `crates/sitolo-<name>/` and place cross-package tests in root `tests/`. |
| **Delta** | Different crate naming; a `crates/infrastructure/` layer absent from both inventories; an `integration-tests/` root directory absent from both topologies and from `docs/code_ownership_map.md` §5. |
| **Impact** | Where cross-package tests physically live; whether an `infrastructure` umbrella crate is permitted (it would collapse the adapter boundaries both inventories separate). |
| **Mitigating fact** | The passage is labelled "Suggested", so it carries less authority than either topology. It still cannot be left as an unmarked competing layout. |
| **Owner** | Engineering owner, architecture review |
| **Resolution gate** | P0.2 (crate set), P0.6 (test placement) |
| **Status** | **open** |

### C-03 — Migration location versus migration ownership

| Field | Value |
|---|---|
| **Competing statements** | **A.** `docs/system_architecture_design.md` §7 L387 annotates `crates/sitolo-persistence/` as "SQLx repositories and migrations", while L394 places `migrations/` at repository root. **B.** `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §3 L116 lists root `migrations/`; §5.7 (L233–235) gives `sitolo-persistence` "SQLx/PostgreSQL implementation, repositories, transactions and mapping" and does not assign migrations to it. **C.** `docs/code_ownership_map.md` §5 L88: root `migrations/` is "the single approved schema migration sequence if Phase 0 retains this root path. The persistence owner reviews and verifies it." |
| **Delta** | A's crate annotation implies migrations live inside the crate; A/B's tree puts them at root; B's crate description omits them; C proposes root path with persistence logical ownership, explicitly conditional. |
| **Impact** | Phase 5 schema contract; the migration check Phase 1 must implement; duplicate-sequence risk. |
| **Owner** | Engineering owner (persistence), architecture review |
| **Resolution gate** | P0.4 |
| **Status** | **open** |

### C-04 — Rust version claims inside one document

| Field | Value |
|---|---|
| **Competing statements** | **A.** `docs/system_architecture_design.md` L131 and L4168 state Rust **1.98.1** is the current/verified baseline. **B.** The same document, L1497 and L2153, states the current stable line is Rust **1.98.0** as of August 20, 2026. **C.** `docs/implementation_plan.md` §4.1 L136 states Rust **1.98.1** is the approved baseline as of 2026-09-05. **D.** `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §7 L303 states the baseline is Rust **1.98.1**. |
| **Delta** | One document asserts both 1.98.0 and 1.98.1 as current. Separately, 1.98.1 is no longer the latest stable release (see V-01). |
| **Impact** | `rust-toolchain.toml`, CI toolchain, builder image and local environment must resolve to one value (implementation_plan L138–148). |
| **Owner** | Engineering owner; product owner only if the pin changes release obligations |
| **Resolution gate** | P0.7 records the outcome; D-12 holds the compatibility decision |
| **Status** | **open** |

### C-05 — Root `tests/` boundary

**Rejected as a conflict.** Four sources agree rather than compete: `docs/testing_strategy.md` §mobile (L531) reserves root-level tests for journeys crossing apps or infrastructure; `docs/ADR-026-react-native-mobile.md` L23 reserves root `tests/` for genuine cross-application/system assertions; `docs/code_ownership_map.md` §5 L92 and §6 L175 say the same; `docs/phase_part_execution_contract.md` §3.4–3.5 says the same. No contradiction exists. **Status: rejected** — recorded here so the seed does not disappear silently.

## 5. Open decisions

Each decision lists a recommendation. A recommendation is not approval and does not bind a later part.

### D-01 — Target Rust package inventory and lifecycle

- **Question:** which crates exist, with what single responsibility, and in which phase each first becomes required?
- **Why it matters:** Phase 1 scaffolds workspace members from this list; an absent owner later becomes a reason to hide behavior in a neighboring crate.
- **Options:** (A) architecture list; (B) Phase 1 list; (C) curated set that rejects duplicate-role entries and defers speculative ones.
- **Recommendation:** C. Treat `sitolo-jobs` as a candidate only if a reusable job contract exists beyond `apps/worker` startup (D-03); defer `sitolo-cli` (D-09); defer `sitolo-testkit` until shared fixtures exist (D-10); confirm each of `authz`/`tenancy`/`events`/`audit`/`config` against real consumers before accepting it.
- **Out of scope of both lists:** `sitolo-security` exists only as an empty local directory (G-02). It appears in no approved source and must not enter the inventory by observation.
- **Owner:** product owner, on engineering recommendation
- **Blocks:** Phase 1 workspace creation; every phase's owner assignment
- **Gate:** P0.2
- **Status:** open

### D-02 — `crates/sitolo-api` versus `apps/api`

- **Question:** is the reusable HTTP boundary a library with a separate thin composition root, or one executable package?
- **Why it matters:** duplicating routes or business rules between the two is the failure the map exists to prevent.
- **Options:** (A) crate owns routes/DTOs/middleware, `apps/api` owns startup and wiring only; (B) a single `apps/api` package owning both.
- **Evidence:** `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §3 lists both; §5.3 assigns routes/transport to the crate and says it "must not become the business authority". `docs/code_ownership_map.md` §3 proposes (A) but leaves it unratified.
- **Recommendation:** A — it is the only shape that preserves an independently testable transport library.
- **Owner:** engineering owner
- **Blocks:** Phase 1/2 packaging; API transport tests
- **Gate:** P0.3
- **Status:** open

### D-03 — `sitolo-jobs` versus `apps/worker`

- **Question:** does a reusable job-abstraction crate exist, or does `apps/worker` own registration/dispatch plus wiring?
- **Why it matters:** a jobs crate that duplicates application use cases creates a second business layer; removing it later is expensive.
- **Options:** (A) retain `sitolo-jobs` for reusable registration/dispatch/handler infrastructure; (B) defer it until a concrete job needs shared machinery; (C) fold everything into `apps/worker`.
- **Recommendation:** B. Defer with a named trigger, and forbid job business rules in the worker entry point meanwhile (`docs/system_architecture_design.md` L390 lists it; `docs/ADR-020` treats workers as first-class reliability components but does not require a crate).
- **Owner:** engineering owner
- **Blocks:** Phase 1 workspace members; Phase 12/13 background work
- **Gate:** P0.3
- **Status:** open

### D-04 — Migration path and authority

- **Question:** one root `migrations/` sequence owned logically by persistence, or migrations inside `sitolo-persistence`?
- **Why it matters:** two sequences, or an ambiguous owner, breaks Phase 5's migration contract and Phase 1's drift check.
- **Options:** (A) root `migrations/`, persistence owns design and verification; (B) `crates/sitolo-persistence/migrations/`; (C) both (prohibited — competes for authority).
- **Recommendation:** A, exactly as `docs/code_ownership_map.md` §5 L88 proposes conditionally, with a single sequence and no duplicate. Requires rewording the `system_architecture_design.md` L387 annotation.
- **Owner:** engineering owner (persistence)
- **Blocks:** Phase 5; migration CI gate
- **Gate:** P0.4
- **Status:** open

### D-05 — Tauri `src-tauri` workspace membership and lockfile

- **Question:** is `apps/desktop/src-tauri` a member of the root Cargo workspace (one root `Cargo.lock`) or a separately locked Rust project?
- **Why it matters:** two lockfiles with unclear authority defeat reproducibility; excluding it leaves the native shell outside CI coverage.
- **Options:** (A) workspace member, single root lockfile; (B) isolated package with its own committed lockfile and explicit CI commands.
- **Evidence:** `docs/system_architecture_design.md` L381 shows a root `Cargo.lock` and no `src-tauri`; `docs/code_ownership_map.md` §3 L36 and §5 L140 record the decision as open; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §8 (L326–345) governs lockfile drift.
- **Recommendation:** not yet chosen. Both are defensible; the deciding evidence is Phase 1 `cargo metadata` plus the Tauri build path. What must not happen is a mixed policy.
- **Owner:** engineering owner
- **Blocks:** Phase 1 workspace/lockfile setup; desktop build CI
- **Gate:** P0.5
- **Status:** open

### D-06 — Canonical machine-readable API schema and generation command

- **Question:** which file is the canonical contract, and what command generates `packages/api-types`?
- **Why it matters:** generated clients must derive from one source; hand-maintained DTOs are explicitly prohibited.
- **Evidence:** `docs/api_contract.md` §74 L2399–2409 says OpenAPI is the external contract representation and "should be generated or maintained"; no path or command is named. §75 L2430 requires client generation from the canonical contract. `docs/code_ownership_map.md` §5 L87 requires the output to "name its canonical source and generation command". No document currently does (search for `openapi.yaml`, `openapi.json`, `api-types`, `codegen` returned no path).
- **Recommendation:** OpenAPI 3.x as the single machine-readable authority, generated from Rust route definitions where feasible, with the generation command recorded in the map before `packages/api-types/` is created.
- **Owner:** engineering owner (API)
- **Blocks:** Phase 1/2 client generation; desktop and mobile API adapters
- **Gate:** P0.5
- **Status:** open

### D-07 — React Native workflow, tooling and test harness

- **Question:** Community CLI or Expo; package manager and lockfile; compatible React Native/React/Node/Android/iOS versions; SQLite adapter library; E2E harness; native-module strategy.
- **Why it matters:** these choices determine the mobile manifest, build path and CI commands. They do not reopen the framework.
- **Settled upstream:** React Native is the sole framework (ADR-026, accepted 2026-10-21); SQLite is the local operational store (ADR-015).
- **Evidence:** `docs/ADR-026-react-native-mobile.md` §"Decisions still open" L28–35 enumerates exactly these items and forbids pinning by newest release.
- **Recommendation:** decide in P0.5/P0.7 after checking the current supported release matrix; record the capability requirements (offline outbox, secure storage, process death) as the decision criteria rather than popularity.
- **Owner:** engineering owner; product owner for platform support scope
- **Blocks:** `apps/mobile/` scaffolding; Phase 12 offline work
- **Gate:** P0.5
- **Status:** open

### D-08 — Separate `apps/admin-web` client

- **Question:** does an admin web client exist at all?
- **Why it matters:** it creates a third client surface, its own auth story and its own test matrix.
- **Evidence:** `docs/system_architecture_design.md` L398 marks it "optional Next.js/React + TS"; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §3 omits it entirely; `docs/code_ownership_map.md` §5 L86 says do not scaffold without an approved product requirement.
- **Recommendation:** no separate admin-web until a product requirement names its users and workflows; administration belongs to the Tauri desktop app in the interim.
- **Owner:** product owner
- **Blocks:** client inventory; Phase 18 scope
- **Gate:** P0.5
- **Status:** open

### D-09 — `sitolo-cli` disposition

- **Question:** is an operational/developer CLI warranted before a concrete use case exists?
- **Why it matters:** a CLI is a new privilege path with its own authentication and audit obligations.
- **Evidence:** `docs/system_architecture_design.md` L393 marks it "if needed"; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` omits it; `docs/code_ownership_map.md` §4 L61 defers it.
- **Recommendation:** deferred. Revisit only with a named operational use case, security model and test plan.
- **Owner:** engineering owner
- **Blocks:** nothing currently
- **Gate:** P0.2
- **Status:** deferred

### D-10 — `sitolo-testkit` first-required phase

- **Question:** when does a shared test-fixture crate become justified?
- **Why it matters:** creating it before shared fixtures exist produces an empty placeholder; creating it late tempts copy-paste fixtures.
- **Evidence:** `docs/code_ownership_map.md` §4 L59 and §7 L199 require `first_required_phase` and forbid production dependencies on it; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §5.14 L261–263 says "Production crates must never depend on it."
- **Recommendation:** create it in the first part that needs shared builders across two or more packages; until then, keep fixtures local to their owner.
- **Owner:** engineering owner
- **Blocks:** nothing currently
- **Gate:** P0.2 / P0.6
- **Status:** open

### D-11 — Manifest vocabulary for phase-aware enforcement

- **Question:** what exact values do `first_required_phase` and `lifecycle` take, and who may change them?
- **Why it matters:** the Phase 1 checker fails closed on ambiguous inputs; an undefined enum makes the gate either unusable or permissive.
- **Evidence:** `docs/phase_part_execution_contract.md` §6 L78 and `docs/code_ownership_map.md` §7 L198–199 name the fields but not the value domain.
- **Recommendation:** `first_required_phase` ∈ {0…20}; `lifecycle` ∈ {planned, active, deferred, optional, retired}; changes require a plan amendment and human acceptance.
- **Owner:** engineering owner
- **Blocks:** the Phase 1 ownership checker
- **Gate:** P0.6
- **Status:** open

### D-12 — Rust pin: retain 1.98.1 or move to 1.99.1

- **Question:** does the approved baseline stay at 1.98.1 or move to the current stable 1.99.1?
- **Why it matters:** the pin is part of the reproducible build boundary and must match `rust-toolchain.toml`, CI, builder image and local environment.
- **Evidence:** see C-04 and V-01/V-02. 1.98.1 was released 2026-09-03 to fix a vtable-generation miscompilation; 1.99.1 was released 2026-10-15 and fixes unrelated regressions (FFI `bool` return on x86_64, `wcslen` link failures, `rust-lld` on macOS).
- **Recommendation:** no recommendation is made on the pin. A newer stable release does not by itself justify a change; the decision requires a compatibility review of the dependency set, which cannot run until a workspace exists. Until then, keep 1.98.1 and record the discrepancy rather than silently updating.
- **Owner:** engineering owner; product owner informed
- **Blocks:** Phase 1 toolchain configuration (only if changed)
- **Gate:** P0.7, with Phase 1 verification
- **Status:** open

### D-13 — Desktop UI stack ratification

- **Question:** is React + TypeScript the ratified desktop UI stack?
- **Why it matters:** the ownership map, test placement and generated-type strategy for `apps/desktop/src/` depend on it.
- **Evidence:** `docs/ADR-003` L194 selects Tauri with "a constrained TypeScript UI" and does not name a UI framework; `docs/system_architecture_design.md` L397 says "Tauri + TS UI"; `docs/code_ownership_map.md` §3 L35 and §5 L83 record React + TypeScript as a requested target "pending Phase 0 ratification".
- **Recommendation:** ratify React + TypeScript. This is compatible with ADR-003, but because it is currently a request rather than a recorded decision, P0.5 or P0.7 must record it (an ADR-003 amendment is the natural home).
- **Owner:** product owner (client direction); engineering owner (implementation)
- **Blocks:** desktop source map; Phase 1 frontend tooling
- **Gate:** P0.5
- **Status:** open

## 6. Version verification

### V-01 — Is Rust 1.99.1 the current stable release?

| Field | Value |
|---|---|
| Claim under review | Rust 1.98.x is "the current stable release" (`system_architecture_design.md` L131, L1497, L2153, L4168; `implementation_plan.md` L136; `phase1_repository_rust_workspace_ci_deep_implementation.md` L303) |
| Official source consulted | https://blog.rust-lang.org/2026/10/15/Rust-1.99.1/ |
| Checked on | 2026-10-21 |
| Finding | The Rust Release Team published Rust 1.99.1 on 2026-10-15. It fixes an FFI `bool` return miscompilation on x86_64, missing `wcslen` symbol/link failures, and `rust-lld` failing to load `libLLVM` on macOS. |
| Verdict | **stale** — statements that 1.98.x is the latest stable release are no longer current. The statements remain historically accurate about the date they name. |
| Owner | Engineering owner (toolchain baseline) |
| Resolution gate | P0.7 records the outcome of D-12; no pin change without a compatibility review |
| Status | **verification complete; decision open** — the verdict is a factual finding, not a decision |
| Authority limit | This verification does not authorize changing the pin. It establishes only that the "current stable" wording is outdated. The pin decision is D-12. |

### V-02 — Was Rust 1.98.1 released on 2026-09-03 with a vtable fix?

| Field | Value |
|---|---|
| Claim under review | `implementation_plan.md` L136: 1.98.1 released 2026-09-03 to correct a 1.98.0 miscompilation involving trait-object vtable generation |
| Official source consulted | https://blog.rust-lang.org/2026/09/03/Rust-1.98.1/ |
| Checked on | 2026-10-21 |
| Finding | Confirmed. The announcement is dated Sept. 3, 2026 and states 1.98.1 fixes a miscompilation where rustc could generate a trait-object vtable with a null function pointer, leading to undefined behavior. |
| Verdict | **current** for the claim as written. The pin remains defensible on its own terms even though it is no longer the newest release. |
| Owner | Engineering owner (toolchain baseline) |
| Resolution gate | P0.7, D-12 |
| Status | **verification complete; decision open** — confirming the historical release does not confirm the pin |

## 7. Gaps

### G-01 — CI enforcement named in the contract does not exist

| Field | Value |
|---|---|
| Expected | `scripts/ci/verify`, `scripts/ci/verify.ps1`, `scripts/ci/check-code-ownership`, `.github/workflows/{policy,rust,integration,security,artifact,release}.yml` per `docs/ci_enforcement.md` L74–85 |
| Observed | None present. `Test-Path` returned false for each; `.github/workflows/` does not exist |
| Check | Direct filesystem test at this revision |
| Disposition | **NOT RUN / absent.** The Phase 1 ownership checker is a deliverable, not current evidence. No document may describe these as passing. Do not create them in Phase 0. |
| Owner | Phase 1 |
| Status | open |

### G-02 — Fifteen empty crate directories exist locally, including an unmapped crate

| Field | Value |
|---|---|
| Observed | `crates/` contains 15 empty directories: `sitolo-api`, `sitolo-application`, `sitolo-audit`, `sitolo-auth`, `sitolo-authz`, `sitolo-config`, `sitolo-domain`, `sitolo-events`, `sitolo-integrations`, `sitolo-observability`, `sitolo-persistence`, `sitolo-security`, `sitolo-sync`, `sitolo-tenancy`, `sitolo-testkit`. Zero files recursively. Zero git-tracked entries. Not ignored (`.gitignore` does not cover them). |
| Significance | `sitolo-security` appears in **neither** candidate inventory (C-01). Empty directories are invisible to Git and to `git status`, so this state is not part of the repository revision and cannot be reviewed. A future agent could mistake them for an approved inventory. |
| Relationship to the ownership map | `docs/code_ownership_map.md` L9 states the repository "has no product crates/ … tree". That remains true for tracked repository content; it is not true of this working copy. |
| Disposition | Recorded as local scaffolding only. **Not evidence of approval.** Do not add, rename or delete these directories in P0.1; removal requires explicit human instruction because they are unversioned local state. D-01 decides the real inventory. |
| Owner | Engineering owner (local environment hygiene); D-01 |
| Status | open |

### G-03 — Malformed citation markup embedded in canonical documents

| Field | Value |
|---|---|
| Observed | Literal `turn…search…` tokens appear in 14 places in `docs/system_architecture_design.md` (L161, 173, 785, 789, 826, 830, 1497, 1918, 2795, 3602, 3619, 3625, 3643, 6013). The pattern also appears in `database_design.md`, `mra_eis_integration_spec.md`, `observability_spec.md`, `payment_integration_spec.md` and several phase contracts. |
| Significance | These are internal retrieval identifiers, not citations. A canonical document that carries them cannot be treated as externally verified at those points, and one of them (L1497) is the Rust-version statement at issue in C-04. |
| Disposition | Record as an integrity defect. Repairing the corpus is not P0.1 scope; assign to P0.7 for the documents inside the Phase 0 decision set, and to the owning phase for the rest. Do not treat the affected sentences as externally sourced until repaired. |
| Owner | P0.7 (Phase 0 set); phase owners for the remainder |
| Status | open |

### G-04 — Other dated anchors were not re-verified in P0.1

| Field | Value |
|---|---|
| Observed | `implementation_plan.md` §4.2 L152 claims PostgreSQL 18.6 is current as of 2026-09-05. `ADR-026` L75 records a React Native 0.88 announcement dated 2026-10-19, sourced from Stage 02 research. Neither was re-fetched during P0.1. |
| Significance | The part's seed list named only the Rust discrepancy. Claiming these as verified would overstate the work. |
| Disposition | Carry forward with their original dates and mark **not re-verified in P0.1**. Ownership: PostgreSQL anchor → P0.7; React Native release matrix → P0.5/D-07. |
| Owner | P0.5, P0.7 |
| Status | open |

### G-05 — Implementation status names no baseline revision

| Field | Value |
|---|---|
| Observed | `docs/current_implementation_status.md` L8: "**Repository baseline:** The commit/PR that records this reset; update the exact SHA after merge." |
| Significance | Phase 0 exit requires revision-attributable evidence; the status document itself does not identify the revision it describes. |
| Disposition | Update at Stage 09 delivery of a part, or at the Phase 0 exit package. Not a P0.1 write target. |
| Owner | Stage 09 delivery / P0.8 |
| Status | open |

### G-06 — Literal `\n` escape in the documentation index

| Field | Value |
|---|---|
| Observed | `docs/README.md` L14: the ADR-001–025 row ends with the two characters `\n` instead of an actual line break, so the ADR-026 row is glued into the same line and the table row can render incorrectly |
| Origin | Pre-existing. The P0.1 commit added a separate register row and did not introduce or touch this line |
| Significance | Cosmetic to a reader, but it corrupts a table in the index that routes every governing document. Discovered during Stage 06 audit (F-04) and deliberately not fixed there or in P0.1 |
| Disposition | Recorded only. Assigned to P0.7 canonical-document reconciliation, or a separately scoped documentation correction if P0.7 does not cover `docs/README.md`. Do not fix it silently as part of an unrelated change |
| Owner | P0.7 |
| Status | open |

## 8. Settled — recorded, not reopened

| Subject | Decision | Status |
|---|---|---|
| Mobile framework | React Native only; Flutter prohibited | Accepted, ADR-026 (2026-10-21) |
| Mobile local store | SQLite | Accepted, ADR-015 |
| Desktop shell | Tauri with a constrained TypeScript UI | Accepted, ADR-003 |
| Server authority | PostgreSQL is the authority for server state | Accepted, ADR-004 |
| Service shape | Modular monolith before distribution | Accepted, ADR-005 |

## 9. Seed disposition

Every seed from the accepted P0.1 write-set proposal §3.2 is accounted for.

| Seed | Outcome | Row |
|---|---|---|
| Crate inventory disagreement | Confirmed conflict | C-01 |
| `sitolo-api` vs `apps/api`; `sitolo-jobs` vs `apps/worker` | Confirmed decisions | D-02, D-03 |
| Migration root vs persistence ownership | Confirmed conflict | C-03 |
| Tauri workspace membership vs separate lockfile | Confirmed decision | D-05 |
| Canonical machine-readable API schema and generation command | Confirmed decision | D-06 |
| React Native tooling, local persistence, E2E harness | Confirmed decision, narrowed: framework and SQLite are settled; tooling is open | D-07 |
| Separate admin-web existence | Confirmed decision | D-08 |
| `sitolo-cli` and `sitolo-testkit` disposition | Confirmed decisions | D-09, D-10 |
| `first_required_phase` / lifecycle vocabulary | Confirmed decision | D-11 |
| Rust pin 1.98.1 vs 1.99.1 | Confirmed conflict plus two verifications and a decision | C-04, V-01, V-02, D-12 |
| Root `tests/` boundary | **Rejected** — four sources agree; no conflict | C-05 |
| Test/CI claims that cannot run yet | Confirmed gap | G-01 |

Additional rows found during P0.1 that were not in the seed list: C-02 (third topology in the testing strategy), G-02 (empty local crates including an unmapped crate), G-03 (malformed citations), G-04 (unverified anchors), G-05 (status baseline revision), D-13 (desktop UI stack ratification).

## 10. Resolution gates

| Part | Resolves | Produces |
|---|---|---|
| P0.2 | C-01, C-02, D-01, D-09, D-10 | Approved crate inventory, dependency edges, first-required phase |
| P0.3 | D-02, D-03 | Executable and background-work boundaries |
| P0.4 | C-03, D-04 | Persistence, migration and cross-cutting ownership |
| P0.5 | D-05, D-06, D-07, D-08, D-13 | Client, native and generated-contract map |
| P0.6 | D-11 | Lifecycle, test placement and CI enforcement contract |
| P0.7 | C-04, D-12, G-03, G-06 | Canonical document and ADR reconciliation; accepted decisions applied |
| P0.8 | G-01, G-05 | Independent audit, verification and exit package |

A part may add rows. A part may not close a row owned by another part without a plan amendment.

## 11. Change control

- Adding, merging or reclassifying a row requires a plan amendment and human acceptance.
- Marking a row `resolved` requires a recorded decision by its named owner and the corresponding canonical document update.
- A row may be marked `rejected` only with a written reason, as C-05 demonstrates.
- This register is not evidence of implementation. No row carries a build, test or CI result.
