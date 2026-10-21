---
artifact_type: implementation-plan
status: human-approved-sequence
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "04-plan"
product_phase: "Phase 0 — Architecture, Contracts and ADR Freeze"
part_sequence_status: "sequence human-approved 2026-10-21; P0.1 write set accepted and implemented 2026-10-21; Stage 06 audit PARTIAL; Stage 07 remediation applied — Stage 08 verification pending"
current_part: "P0.1 — Source of truth and conflict register"
part_status: "remediated; awaiting Stage 08 verification"
---

# Phase 0 — Ownership Reconciliation Plan

## Decision gate

**The product owner approved the eight-part Phase 0 sequence (P0.1–P0.8) on 2026-10-21.** That approval accepts the part ordering, not every architecture recommendation inside those parts. P0.1's exact write set and stop condition were recorded and accepted on 2026-10-21 in [the Stage 04 amendment](phase0-p01-write-set-proposal.md); P0.1 was implemented that day and audited **PARTIAL**, with remediation applied and Stage 08 verification pending. Each later part repeats this gate: its own exact write set and stop condition must be recorded and accepted before its Stage 05 begins. This plan does not authorize implementing a later part by reference to P0.1's acceptance. My role in this workflow is architecture guidance and independent audit; product-code implementation is assigned to the implementation agent. Phase 1 scaffolding remains blocked until the Phase 0 exit gate is accepted.

## Objective

Freeze one coherent, testable ownership model across Rust libraries, executables, data/integration boundaries, clients, generated contracts and test locations. The approved map must make it impossible for an implementation phase to claim a requirement complete while its responsible crate or app is missing or while its code/tests have been moved elsewhere.

## Non-goals

- No feature implementation, Cargo workspace scaffolding, client scaffolding or CI checker implementation in this Phase 0 planning run.
- No empty or speculative crate is created to satisfy a diagram.
- No business logic is duplicated in API handlers, worker startup, Tauri commands or React components.
- No compiler pin, library list or lockfile topology is silently changed on the basis of a new source alone.

## Approved Phase 0 part sequence

Part order and IDs P0.1–P0.8 are approved. The descriptions define planning outcomes, not permission to implement without recording and accepting each selected part's exact write set and stop condition.

| Part (order approved) | Purpose and required outcome | Exact planning/decision surface | Evidence and acceptance | State |
|---|---|---|---|---|
| **P0.1 — Source of truth and conflict register** | Reconcile the approved architecture hierarchy and identify contradictions, obsolete claims, unverified references and current external facts. Produce one conflict/decision register that records source, competing statements, impact, owner and resolution gate. | `docs/system_architecture_design.md`; `docs/implementation_plan.md`; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`; `docs/ADR-001-025.md`; `docs/api_contract.md`; `docs/database_design.md`; `docs/testing_strategy.md`; `docs/ci_enforcement.md`; Stage 02/03 run artifacts. | Each material conflict cites exact evidence. Version-sensitive claims are checked on official sources and are not converted into a product decision without compatibility review. | Sequence approved; **P0.1 write set accepted 2026-10-21**; **P0.1 implemented 2026-10-21** — Stage 06 audit pending |
| **P0.2 — Rust crate inventory and dependency graph** | Decide the complete target crate set and the sole role of each crate. Record allowed/prohibited dependency edges and when each crate first becomes required. Avoid both dropped crates and unnecessary crate-per-noun fragmentation. | `docs/code_ownership_map.md`; `docs/system_architecture_design.md` §7; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §§3–6; relevant accepted ADRs. | Every candidate is either accepted with a single responsibility, deferred/optional with an owner and trigger, or rejected with rationale. Dependency direction has no cycles, duplicate business rules or domain-to-infrastructure edges. | Sequence approved; decisions inside part remain open |
| **P0.3 — Executable and background-work boundaries** | Resolve `crates/sitolo-api` versus `apps/api`, `apps/worker` versus `sitolo-jobs`, and whether `sitolo-cli` is warranted. Distinguish reusable transport/job contracts from composition roots and application use cases. | `docs/code_ownership_map.md`; `docs/system_architecture_design.md` §§7 and relevant worker/API sections; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` §§3, 5 and 14. | Each executable has a small, explicit composition-root scope; each library has independently testable responsibilities. CLI stays deferred without an approved use case. | Proposed; requires human decision |
| **P0.4 — Persistence and cross-cutting service ownership** | Resolve migration path/authority and the boundary among persistence, configuration, integration adapters, sync, event contracts, audit evidence and telemetry. Confirm which cross-cutting items merit a crate versus a module based on reuse and trust/dependency needs. | `docs/code_ownership_map.md`; `docs/database_design.md`; `docs/domain_model.md`; `docs/api_contract.md`; `docs/payment_integration_spec.md`; `docs/mra_eis_integration_spec.md`; `docs/observability_spec.md`; `docs/ci_enforcement.md`. | Exactly one authoritative schema migration sequence; audit records are distinct from diagnostic telemetry; provider DTOs do not leak into the domain; sync does not bypass server authority. | Proposed; requires human decision |
| **P0.5 — Client, native and generated-contract file map** | Freeze React Native mobile, Tauri + React desktop, generated API type/client source, lockfile/package-manager boundaries and all source/test paths. Ensure each native/frontend file has a bounded job and no second business engine. | `docs/code_ownership_map.md`; `docs/system_architecture_design.md`; `docs/api_contract.md`; `docs/ADR-026-react-native-mobile.md`, historical `docs/ADR-002` and `docs/ADR-003`; Tauri/React/React Native test sections of `docs/testing_strategy.md`. | Every source, config, capability, generated path and test home has one owner. The source contract for generated client types is explicit. Tauri workspace membership and lockfile topology are approved, not guessed. | Proposed; requires human decision |
| **P0.6 — Package lifecycle, test placement and CI enforcement contract** | Define the target inventory vs due packages; assign `first_required_phase` and lifecycle for each. Freeze owner-local test policy, package/app command matrix, testkit policy and the manifest requirements that Phase 1's blocking checker must enforce. | `docs/code_ownership_map.md`; `docs/phase_part_execution_contract.md`; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`; `docs/testing_strategy.md`; `docs/ci_enforcement.md`. | A future package can be mapped but absent until due; an in-scope missing owner blocks implementation. Every due package/app must have test locations and specific commands. Root tests are only cross-owner/system/repository-level. | Proposed; requires human decision |
| **P0.7 — Canonical document and ADR reconciliation** | Apply only the approved decisions to the authoritative architecture, Phase 1 contract, code ownership map, ADRs and phase/test contracts. Remove conflicting statements rather than adding another parallel architecture document. Record the accepted code ownership map revision. | `docs/ADR-001-025.md`; `docs/system_architecture_design.md`; `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`; `docs/code_ownership_map.md`; `docs/phase0_architecture_contract_freeze.md`; relevant phase/test/CI indexes. | One decision per concern; superseded statements are explicitly marked; links and phase routes resolve; no implementation phase depends on an unapproved owner. | Sequence approved; depends on P0.2–P0.6 |
| **P0.8 — Independent audit, verification and Phase 0 exit package** | Audit the completed reconciliation against the accepted part sequence, verify references and contradiction removal, state all checks actually run, and prepare the human decision package for Phase 0 exit. | Final diff, updated ownership map/ADRs, `docs/phase_execution_guide.md`, `docs/phase_contract_coverage_register.md`, status pointer, Stage 06–09 outputs. | Revision-attributable evidence, owner map, decision register, unresolved risks with owners, skipped checks and explicit human acceptance. Phase 0 remains open if a material ownership or architecture decision remains unresolved. | Proposed; depends on all prior approved parts |

## Recommended ownership direction — proposal only

This is the starting recommendation to discuss in P0.2–P0.6; none of it is recorded as approved.

### Rust libraries

- **Domain and use cases:** `sitolo-domain` owns pure entities, value objects, aggregate/state transition rules and domain invariants. `sitolo-application` owns use cases, command/query orchestration, ports and workflow/transaction coordination.
- **HTTP transport and process:** `sitolo-api` owns reusable routes/handlers, transport DTOs, request/response mapping and HTTP middleware. `apps/api` owns process startup, configuration hookup, dependency wiring and graceful shutdown. Application/domain rules are not copied into route handlers.
- **Authentication, authorization and scope:** keep the meanings distinct. `sitolo-auth` owns authentication/session/device mechanisms; `sitolo-authz` owns permission decisions and policy evaluation; `sitolo-tenancy` owns organization/branch membership semantics and trusted scope propagation. Confirm whether each deserves a separate crate after checking dependencies and consumers; if approved, each gets its own code and tests in its path.
- **Persistence:** `sitolo-persistence` owns DB adapters, repository implementations and transactions. Recommend one root `migrations/` sequence with logical ownership assigned to persistence, unless evidence shows the toolchain requires a different single canonical location. Never duplicate migration files in two places.
- **External integrations and synchronization:** `sitolo-integrations` owns provider adapters/DTO translation and provider failure handling; `sitolo-sync` owns offline protocol, replay/checkpoint/conflict behavior. Neither owns domain financial truth or replaces backend authorization.
- **Cross-cutting contracts:** `sitolo-events` owns event contracts; `sitolo-audit` owns durable audit semantics/evidence; `sitolo-observability` owns traces/metrics/logging conventions; `sitolo-config` owns typed configuration loading and validation. Keep these separate only if the approved dependency boundaries and consumer needs justify them; do not create them as empty placeholders.
- **Background jobs:** the architecture doc lists `sitolo-jobs` while Phase 1 omits it. Recommendation: retain it in the target map only if it has a genuinely reusable job contract/dispatch/handler role distinct from `apps/worker` startup and `sitolo-application` business use cases. `apps/worker` remains the thin runtime composition root. If we cannot identify that reusable responsibility, mark the crate deferred until the first concrete job rather than inventing one.
- **Testing helpers:** `sitolo-testkit` is test-only and can never be a production dependency. Create it when there are real shared fixtures/builders used by multiple packages; otherwise defer it explicitly. Do not create it merely to fill the inventory.
- **CLI:** `sitolo-cli` remains optional/deferred until a concrete operational/developer use case, security model and test plan are approved.

### Client and repository paths

- Keep `apps/mobile/` as React Native per accepted ADR-026; TypeScript/JavaScript source, colocated tests and the approved mobile E2E harness belong inside that app.
- Keep `apps/desktop/src/` for React/TypeScript UI, feature state and UI tests; keep `apps/desktop/src-tauri/` for native Rust commands, adapters, capability/configuration and native tests. Do not make either side authoritative for server-owned business rules.
- Map `apps/desktop/src-tauri/src/lib.rs` to application setup/command registration and `main.rs` to a thin entrypoint.
- Keep `packages/api-types/` generated from one approved canonical API contract and generation command; never hand-maintain a competing source of truth. The canonical machine-readable API schema path is still a decision.
- Keep `tests/` at repository root for actual cross-package/app journey tests and architecture constraints, not single-crate tests.

## Proposed dependency invariants

These are proposed architectural constraints to validate against the accepted map:

- `sitolo-domain` has no dependency on Axum/HTTP transport, SQLx/DB drivers, queues, filesystem/network implementations, frontend types or provider DTOs.
- `sitolo-application` depends on domain concepts and defines ports; persistence and integration adapters implement those ports. Adapter packages do not own business use cases.
- Executables compose approved libraries; they do not duplicate route, domain or worker business rules.
- Tauri commands provide narrowly scoped native capabilities. They call backend APIs or native adapters, not a second sales/inventory/payment engine.
- `sitolo-testkit`, when it exists, appears only in test/dev dependency graphs.
- The accepted map defines exact allowed edges and rejects cycles. No catch-all `common` crate is added just to avoid deciding ownership.

## Open decisions to settle in the review

1. Approve/revise the candidate package inventory, including `sitolo-jobs`, `sitolo-testkit` and optional `sitolo-cli`.
2. Approve the distinct roles for `sitolo-api` vs `apps/api`, and `sitolo-jobs` vs `apps/worker`.
3. Approve single-root `migrations/` ownership by persistence or select another single location.
4. Approve Tauri `src-tauri` as a member of the root Cargo workspace (one root `Cargo.lock`) or a separately locked Cargo project; no mixed policy.
5. Confirm the approved React Native mobile boundary, React + TypeScript in Tauri desktop, and whether a separate admin-web exists at all. Current recommendation: no separate admin-web until approved need.
6. Select the canonical machine-readable API contract and client-generation command/path.
7. Approve when each package first becomes required, so Phase 1 does not create empty future crates and later phases cannot silently skip required owners.
8. Review the current Rust pin (documented as 1.98.1 on 2026-09-05) against the official 1.99.1 release announced 2026-10-15; decide after compatibility review, not by release-date alone.

## Verification plan

Phase 0 documentation checks:

- Resolve all relative Markdown links in changed/canonical documents.
- Search the target docs for conflicting package lists, contradictory migration paths, duplicate lockfile claims, unowned paths, and stale claims presented as current.
- Confirm that each proposed package/app has a sole responsibility, source path, owner-local test path and allowed dependency edges.
- Confirm every proposed part has an explicit output and acceptance gate.
- Report tool checks as PASS only if run; otherwise BLOCKED/NOT RUN with the exact reason.
- Use real Cargo metadata and build/test commands only after Phase 1 has an actual workspace. Do not call those checks passed in Phase 0.

## Approved next action

The eight-part ordering is accepted. The next work item is P0.1, beginning with a bounded evidence/conflict-register write set. Its exact files and stop condition must be identified before that part is handed to the implementation agent. The crate inventory and detailed ownership recommendations remain open; the ownership map is only partially settled because React Native is the accepted mobile framework while package/workspace/client-tooling decisions remain unresolved. Phase 1 scaffolding remains blocked.

**Stage 04 amendment (P0.1 scope accepted 2026-10-21):** [`phase0-p01-write-set-proposal.md`](phase0-p01-write-set-proposal.md) records the accepted P0.1 deliverable, five-path repository-root-relative write set, nine read-only evidence inputs and stop condition. Its §11 acceptance record authorizes P0.1 scope only; it decides no architecture question, does not start P0.2 and does not unblock Phase 1. This plan's approved sequence text is unchanged.
