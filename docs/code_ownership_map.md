# Sitolo Repository Code, Package, Client and Test Ownership Map

**Status:** Phase 0 proposal — React Native mobile selection is accepted by ADR-026; remaining package, path and dependency decisions require joint review before this map becomes fully approved.  
**Purpose:** Give every implementation responsibility one explicit home and prevent code or tests from being moved into a convenient but incorrect crate.  
**Authority:** This map becomes binding after Phase 0 reconciles the package-list conflicts below. Until then, it is a decision and routing aid, not permission to scaffold either candidate layout.

## 1. Verified repository baseline

At the inspected restart revision 3b2af5440d3d2f04d0ba653fdefbbf26bdfd975f on the ICM reset branch, the repository has no product crates/, apps/, tests/, migrations/ or src-tauri/ tree, and no root Cargo.toml, Cargo.lock, or app package.json. The repository contains architecture specifications, phase contracts, ICM workspaces, instructions and skill/reference material. Some reference files contain example source, but they are not a runnable Sitolo crate.

**Therefore, no product crate or Tauri/React app is currently verified as implemented.** Target paths below are planned ownership, not claims that those files exist, compile or have tests. Phase 0 must settle the target layout before Phase 1 scaffolds it.

## 2. Rules that apply to every phase and part

1. **One responsibility, one authoritative implementation.** Implement a business rule in its owning layer. Do not implement a missing crate's behavior inside an unrelated crate merely to make the feature appear complete. A caller may orchestrate or adapt the behavior; it must not silently re-implement it.
2. **Map before writing.** Every implementation part must name the requirements, owning packages, exact target paths, expected tests and verification commands before implementation starts.
3. **No unowned or omitted package work.** The plan must account for every approved package affected by the capability. Other packages may be marked not touched with a reason; they must not disappear from the analysis. A phase does not need to modify every package.
4. **No unsolicited crate proliferation.** Do not create an empty crate or split a crate solely because a folder is large. New packages or ownership changes require an explicit architectural decision and a revised map.
5. **No hidden ownership moves.** If investigation shows that a requirement belongs to another package, stop and amend the plan for human review. Do not bury it in the current package to avoid a dependency or a phase boundary.
6. **Tests live with their owner.** A package's unit and package-level integration tests belong inside that package. Repository-level tests are reserved for behavior that genuinely crosses package/app boundaries or validates repository-wide architecture.
7. **Test claims are local and attributable.** Record which package/app was tested, the exact command, result, relevant test names, and revision. A successful workspace command does not prove a particular behavior unless that behavior was exercised. A zero-test result is not evidence of implemented behavior.
8. **Generated contracts stay generated.** Generated clients or type bindings must identify their canonical source and generation command. Do not hand-maintain a second authoritative copy.
9. **Part boundaries are co-designed.** The agent may propose a phase part breakdown, but implementation does not start until the human reviews and agrees to the part sequence and the current part's write set. See the Phase Part Execution Contract.

## 3. Architecture conflicts Phase 0 must resolve

The current target documents are not fully aligned. Do not silently merge their package lists or build both versions.

| Conflict | Existing evidence | Required disposition |
|---|---|---|
| Rust package inventory | system_architecture_design.md §7 lists sitolo-jobs and an optional sitolo-cli; phase1_repository_rust_workspace_ci_deep_implementation.md §3/§5 instead lists sitolo-authz, sitolo-tenancy, sitolo-events, sitolo-audit, sitolo-config, and sitolo-testkit, alongside a different app/package layout. | Phase 0 must approve one package inventory and dependency graph. The table in §4 is a candidate ownership proposal, not an approved manifest. |
| API crate vs executable | Phase 1 proposes both crates/sitolo-api and apps/api. | Keep distinct responsibilities: the crate owns reusable HTTP routing/transport behavior; the app is a thin process entry point and composition root. Do not duplicate handlers or business logic. Ratify the exact package shape in Phase 0. |
| Worker executable vs jobs crate | The Phase 1 tree has apps/worker; the system architecture tree also names sitolo-jobs. | Phase 0 decides whether a separate reusable jobs package is justified. Until then, neither create both copies nor hide job business rules in the worker entry point. |
| Migration location vs ownership | The system architecture tree places migrations/ at repository root; the Phase 1 contract assigns migration responsibility to persistence. | Make logical ownership explicit. If the root path is retained, persistence owns schema/migration design and verification even though the files physically live at migrations/. Do not add a competing migrations directory. |
| Desktop frontend | The system architecture specifies a Tauri shell with a TypeScript UI and separately mentions optional React/Next.js for admin web. The current user direction requests React for the Tauri UI. | Proposed reconciliation: React + TypeScript lives under apps/desktop; optional apps/admin-web is a separate surface and is not created without an approved product requirement. Record the final choice in Phase 0/ADR. React Native is the sole permitted mobile framework per ADR-026; Flutter is not allowed. |
| Tauri Rust package/lockfile | A root Cargo workspace normally shares a workspace lockfile; Tauri's documented setup places its Rust application package/config under src-tauri/ and recommends a committed lockfile for app builds. | Decide whether apps/desktop/src-tauri is a member of the root workspace or an isolated package. Do not accidentally create competing lockfiles or leave the package outside intended CI coverage. |

## 4. Candidate Rust package ownership

The following responsibilities are derived from the existing Phase 1 workspace proposal. They must be reconciled with the system-architecture alternatives in §3 and approved during Phase 0 before Phase 1 uses them as its workspace member list.

For every approved crate, the crate owns its Cargo.toml, src/ implementation and crate-local tests. src/lib.rs is the library entry point unless an approved package design requires a binary target. Add only modules needed for the agreed behavior; the paths below show ownership, not a requirement to create every file in advance.

| Candidate package | Sole primary responsibility | Typical owned source paths | Must not become |
|---|---|---|---|
| crates/sitolo-domain (sitolo-domain) | Pure domain entities/value objects, aggregates, state transitions, invariant checks, business policies and domain errors. | crates/sitolo-domain/src/lib.rs; src/entities/, src/value_objects/, src/aggregates/, src/policies/, src/state_machines/, src/errors.rs as needed. | An HTTP, database, queue, filesystem or provider adapter; a home for Axum/SQLx/provider DTOs. |
| crates/sitolo-application (sitolo-application) | Use cases, commands/queries, application orchestration, ports and transaction/workflow coordination. | crates/sitolo-application/src/lib.rs; src/commands/, src/queries/, src/use_cases/, src/ports/ as needed. | An HTTP framework or the implementation of persistence/provider adapters. |
| crates/sitolo-api (sitolo-api) | Reusable HTTP boundary: routes/handlers, transport DTOs, request decoding/validation, auth-context extraction, response/error mapping and API compatibility behavior. | crates/sitolo-api/src/lib.rs; src/routes/, src/handlers/, src/dto/, src/middleware/ as needed. | The business authority, a repository layer, or a second copy of application/domain rules. |
| crates/sitolo-auth (sitolo-auth) | Authentication mechanisms and abstractions: credentials, sessions, MFA/device identity integration as approved by phase contracts. | crates/sitolo-auth/src/ and its own tests. | General authorization policy or tenant-specific data access unless its approved contract assigns a narrow interface. |
| crates/sitolo-authz (sitolo-authz) | Permission decisions, policy evaluation, resource/action authorization and step-up requirements. | crates/sitolo-authz/src/ and its own tests. | Authentication/session implementation or client-only enforcement. |
| crates/sitolo-tenancy (sitolo-tenancy) | Tenant/organization/branch membership semantics and trusted scope-context propagation. | crates/sitolo-tenancy/src/ and its own tests. | A bypassable convenience wrapper or duplicated tenant resolution in handlers. |
| crates/sitolo-persistence (sitolo-persistence) | SQLx/PostgreSQL access, repository implementations, database mapping, transaction implementation and migration-aware persistence behavior. | crates/sitolo-persistence/src/; approved migrations live only in the single Phase 0-approved location (candidate: root migrations/). | Domain policy, provider orchestration or fake in-memory implementations used to claim PostgreSQL behavior is verified. |
| crates/sitolo-events (sitolo-events) | Internal event/integration-event contracts and approved outbox-facing event representations; keep domain events, integration events, provider evidence and audit records conceptually distinct. | crates/sitolo-events/src/ and its own tests. | A dump of unrelated event structs or a substitute for transactional persistence/audit. |
| crates/sitolo-audit (sitolo-audit) | Durable audit semantics, audit records and evidence contracts. | crates/sitolo-audit/src/ and its own tests. | General debug logging or a second source of domain truth. |
| crates/sitolo-observability (sitolo-observability) | Structured tracing/metrics conventions, correlation identifiers and approved telemetry integration/redaction. | crates/sitolo-observability/src/ and its own tests. | Business audit storage, high-cardinality data dumps or credentials/PII leakage. |
| crates/sitolo-config (sitolo-config) | Typed configuration loading, parsing, validation and startup configuration errors. | crates/sitolo-config/src/ and its own tests. | Secret values committed to the repository or scattered ad-hoc environment parsing across applications. |
| crates/sitolo-integrations (sitolo-integrations) | Provider adapters and normalization at external boundaries (payments, MRA EIS and other approved providers). | crates/sitolo-integrations/src/; provider-specific modules under that crate. | Provider-specific types leaking into the domain model or unverified provider success. |
| crates/sitolo-sync (sitolo-sync) | Domain-aware offline synchronization protocol, replay/checkpoint semantics, conflict handling and version compatibility. | crates/sitolo-sync/src/ and its own tests. | Generic cache synchronization that bypasses server authorization or financial invariants. |
| crates/sitolo-testkit (sitolo-testkit) | Reusable fixtures, test builders and test-only infrastructure shared by tests. | crates/sitolo-testkit/src/; consumed as a development/test dependency only. | A runtime dependency of any production package or a place to store production code because it is convenient to reuse. |
| crates/sitolo-jobs (sitolo-jobs) | **Unresolved alternative.** A reusable job abstraction only if Phase 0 approves a need beyond the apps/worker executable and existing application/event/persistence boundaries. | No path is authorized until Phase 0 decides. | A duplicate of apps/worker or a reason to move domain behavior out of its owner. |
| crates/sitolo-cli (sitolo-cli) | **Deferred optional alternative.** Operational/developer CLI only when a concrete approved use case exists. | No path is authorized until a phase requires it. | An empty scaffold or a second path for production business decisions. |

### Dependency boundaries

The intended architecture is a modular monolith, not a crate-per-noun design.

- sitolo-domain must stay independent of Axum, SQLx, database drivers, HTTP clients, Redis clients, cloud SDKs and provider-specific types.
- sitolo-application may depend on the domain and declare ports. Infrastructure packages implement those ports.
- API and worker executables compose approved dependencies; they do not re-implement capabilities owned by libraries.
- Persistence and integration adapters translate infrastructure/provider representations into approved application/domain contracts.
- sitolo-testkit is test-only. Production dependency edges to it are prohibited.
- Cycles and duplicate business rules are architecture failures, not reasons to add a generic common crate.

The final dependency allowlist must be approved in Phase 0 and mechanically checked in Phase 1.

## 5. Application and repository-level path ownership

| Path | Owns | Does not own |
|---|---|---|
| apps/api/ | API process startup, dependency wiring, runtime configuration hookup and graceful startup/shutdown. | Route-specific business rules, SQL queries written in main.rs, duplicated policy enforcement. |
| apps/worker/ | Worker process startup, task registration/dispatch, runtime lifecycle and dependency wiring. | Job-specific domain decisions or replacement implementations of application use cases. |
| apps/mobile/ | The React Native mobile application: reviewed package manifest and one lockfile; TypeScript/JavaScript under src/; feature modules and colocated tests; API/local-persistence/sync adapters; platform-specific Android/iOS projects and native modules when required by the accepted workflow; mobile E2E tests inside this app. Follow ADR-026. | Flutter/Dart, a duplicate server business engine, direct authority over server-owned financial/inventory/tenant state, or mobile UI as a substitute for server authorization. |
| apps/desktop/ | Tauri desktop application package and React + TypeScript UI, per the requested target pending Phase 0 ratification. | A second Rust business engine or direct authority over server-owned financial, inventory, identity or tenant state. |
| apps/desktop/src-tauri/ | Tauri Rust package, `src/lib.rs` app builder/command registration and native composition, thin `src/main.rs` entry point, narrow commands, capability configuration and device adapters (printing/scanner/OS credential/filesystem features only when approved). | General business use cases, arbitrary shell/process access, unrestricted filesystem commands or database/provider business logic. |
| apps/desktop/src/ | React UI: app bootstrap, route composition, feature pages/components/hooks, typed API/command bridge, UI state, formatting and interaction. | Domain authority, SQL, credentials, manual copies of server permission rules or ungenerated duplicate API truth. |
| apps/admin-web/ | Optional separate React/Next.js admin-web client only after an explicit product decision. | A required part of the Tauri desktop app or a hidden second backend. Do not scaffold now merely because it appears in an architecture sketch. |
| packages/api-types/ | Generated API types/client artifacts, when approved; output must name its canonical source and generation command. | Hand-authored competing DTOs or business logic. |
| migrations/ | The single approved schema migration sequence if Phase 0 retains this root path. The persistence owner reviews and verifies it. | A second copy inside the persistence crate or a place for unrelated SQL/query dumps. |
| config/ | Non-secret configuration examples and deployment profiles. | Secret values or runtime defaults that contradict typed configuration validation. |
| .cargo/, root Cargo.toml, root Cargo.lock, rust-toolchain.toml, rustfmt.toml, clippy.toml, deny.toml | Workspace/toolchain/dependency/build policy as approved in Phase 0 and implemented in Phase 1. | Business behavior. Do not include build outputs or create competing workspace policy files. |
| .github/workflows/, .github/CODEOWNERS, scripts/ci/ | Required checks, repository ownership routing and deterministic validation. Phase 1 creates the missing enforcement scripts. | A textual declaration that a check passed when it was not run or an optional/non-blocking substitute for a required gate. |
| tests/ at repository root | Only true cross-package, cross-app, whole-workflow and repository-wide architecture/contract tests. | Unit tests or a single crate's integration tests that belong inside that crate. |
| fuzz/ | Fuzz targets for approved parsers/protocol boundaries, linked to their owning package and corpus. | The only test evidence for ordinary business invariants. |
| infra/ | Deployment/runtime infrastructure definitions, least-privilege configuration, operational runbooks and environment wiring. | Application domain behavior. |
| docs/ | Human-readable contracts, ADRs, phase plans, test strategy and evidence. | A substitute for source code or executable verification. |
| map/ and workspaces/ | ICM routing, context contracts, System Map and per-run artifacts according to their own ICM contracts. | Product runtime source or duplicate implementation specs. |

### React Native mobile proposed source tree

The complete mobile decision is in [ADR-026](ADR-026-react-native-mobile.md). This is a target map, not a command to scaffold all files before the accepted phase part requires them.

~~~text
apps/mobile/
  package.json                    # package scripts/dependencies; chosen workflow decides exact setup
  <one package-manager lockfile>  # package manager chosen and pinned in Phase 1
  src/
    app/                          # bootstrap and navigation composition
    features/<feature>/           # feature modules with colocated *.test.ts(x)
    shared/                       # genuinely shared presentation/client utilities
    data/                         # API and local persistence adapters
    sync/                         # client-side protocol adapters; server remains authoritative
  android/                        # native project/config when required by selected workflow
  ios/                            # native project/config when required by selected workflow
  e2e/                            # full-device journeys with the approved harness
~~~

Do not choose Expo versus Community CLI, or create speculative native directories, from this map alone; those setup choices are assigned to the Phase 0/Phase 1 decision gate.

### Tauri + React proposed source tree

This is a target map, not current files to create all at once. Create only the paths required by an approved phase part.

~~~text
apps/desktop/
  package.json                  # UI dependencies and approved package scripts
  <one package-manager lockfile> # exact name/tool is selected and pinned in Phase 1
  index.html                    # frontend entry document
  vite.config.ts                # bundling/dev server only if Vite is approved
  tsconfig.json                 # TypeScript compiler policy
  src/
    main.tsx                    # React bootstrap only
    app/                        # app shell and route composition
    features/<feature>/         # feature pages, components, hooks, UI state and colocated *.test.tsx/*.test.ts
    shared/components/           # genuinely shared UI components
    shared/api/                 # generated or typed API client boundary
    shared/tauri/               # narrow, typed wrapper around approved native commands
    shared/lib/                 # non-domain utilities with an identified owner
    styles/                     # frontend styling
  src-tauri/
    Cargo.toml                  # native-shell package; workspace membership requires Phase 0 decision
    build.rs                    # Tauri build integration if required by the selected scaffold
    tauri.conf.json             # Tauri app/build configuration
    capabilities/               # least-privilege window/plugin capabilities
    permissions/                # custom permission definitions only when needed
    icons/                      # app assets; not runtime business logic
    gen/schemas/                # generated Tauri schemas/permissions; regenerate, do not hand-maintain
    src/lib.rs                  # app builder, command registration and native composition
    src/main.rs                 # thin desktop process entry point calling the app library
    src/commands/               # small, capability-scoped invoke handlers
    src/adapters/               # printing/scanner/filesystem/OS credential adapters as required
    tests/                      # native package integration tests
  tests/e2e/                    # true desktop end-to-end journeys
~~~

**Frontend ownership rule:** keep React UI and UI state in apps/desktop/src/; keep native OS integration in apps/desktop/src-tauri/; keep server-owned business rules in the approved Rust backend/domain packages. A React handler may validate presentation input for usability, but the backend remains authoritative. A Tauri command may expose a specific OS capability, but it must not become an unrestricted proxy or a second place to implement sales, inventory, authorization, payments or reconciliation.

Do not place all screens in App.tsx, a universal utils/ file, or one generic hook. Feature-specific code remains inside its feature directory; shared code is extracted only when it is actually shared and has an identified owner.

## 6. Test placement contract

| Test intent | Required home | Examples |
|---|---|---|
| Rust unit test of a module's behavior, including private implementation details where justified | In the same Rust source module/file under #[cfg(test)] | crates/sitolo-domain/src/money.rs contains its money unit tests. |
| Public API integration test for one crate | That crate's tests/ directory | crates/sitolo-domain/tests/public_contract.rs; crates/sitolo-persistence/tests/postgres_rls.rs. |
| Persistence behavior requiring PostgreSQL | crates/sitolo-persistence/tests/ and its approved disposable PostgreSQL harness | Migration, constraints, transaction rollback, RLS, locking, deadlocks and concurrency. In-memory mocks do not prove PostgreSQL behavior. |
| Cross-crate contract or architecture boundary | Repository tests/ or scripts/ci/, because the asserted boundary crosses packages | Dependency graph check, API-to-application-to-persistence contract, testkit runtime-dependency prohibition. |
| API executable smoke test spanning process wiring | apps/api/tests/ | Startup/health/routing smoke test that truly requires the executable. Handler behavior still belongs in sitolo-api tests. |
| Worker executable wiring/lifecycle | apps/worker/tests/ | Registration/startup/shutdown behavior. Domain/job behavior remains in its owning library's tests. |
| React component/hook/feature unit test | Alongside that component/hook/feature under apps/desktop/src/, using one approved naming convention such as *.test.ts/*.test.tsx | src/features/catalogue/ProductRow.test.tsx; src/features/sales/useSaleDraft.test.ts. |
| React app-level integration test | Inside the desktop frontend package under its source test area | Routing, component composition and typed command/API bridge integration. |
| Tauri native command and adapter tests | apps/desktop/src-tauri/src/ unit modules or apps/desktop/src-tauri/tests/ | Capability-scoped command validation, adapter error mapping and native integration boundaries. |
| Full Tauri UI/native journey | apps/desktop/tests/e2e/ | Receipt/reprint workflow, app startup, permission-restricted command behavior. Use a Tauri-supported end-to-end harness; frontend-only mocks do not prove native behavior. |
| React Native mobile unit/component test | Colocated with TypeScript/JavaScript feature/module code under apps/mobile/src/ using the approved Jest/React Native test setup | Mobile feature behavior and tests stay in apps/mobile; do not move them to root tests/. |
| Reusable test builders and fixtures | sitolo-testkit only after approved, imported from [dev-dependencies] | No production package may depend on testkit. |
| Real multi-surface merchant journey | Root tests/ only when the scenario genuinely crosses multiple packages/apps/infrastructure | Complete sale to inventory/payment/outbox, offline replay, cross-tenant negative journeys. |

Rust's documented conventions put unit and documentation tests in Rust source files and integration-style tests under a package's tests/; Cargo runs tests at package/workspace scope. Sitolo adopts that package-local boundary to keep ownership visible. Sources: [Cargo test guide](https://doc.rust-lang.org/cargo/guide/tests.html), [Rust Book: test organization](https://doc.rust-lang.org/book/ch11-03-test-organization.html).

React does not require one universal directory convention for every project. Sitolo's local rule is that the tests live with the feature/component they prove, while cross-feature app integration and real desktop E2E tests use the app package's own test areas. Tauri distinguishes mock-runtime unit/integration testing from actual end-to-end testing, so one is never reported as proof of the other. Sources: [React: importing and exporting components](https://react.dev/learn/importing-and-exporting-components), [Tauri testing](https://v2.tauri.app/develop/tests/), [Tauri WebDriver testing](https://v2.tauri.app/develop/tests/webdriver/).

## 7. Required enforcement

### At planning time — immediate ICM rule

Every phase part's approved plan must include an ownership-and-test matrix with these columns:

| Requirement / control | Owning package or path | Exact implementation file(s) | Exact test file(s) | Command / evidence | Dependency / reason not touched |
|---|---|---|---|---|---|

No implementation begins while a required behavior has no owner, while the crate inventory is unresolved, or while a test has been assigned to a convenient but incorrect package. Record all other approved packages as either affected or not touched with a reason.

### At repository foundation — Phase 1 deliverable

Phase 1 must turn the approved Phase 0 map into deterministic enforcement. Implement `scripts/ci/check-code-ownership` (or an explicitly approved equivalent) and make it a required CI gate. The map describes the **target ownership inventory**; it does not mean every future-phase crate must be created as an empty placeholder in Phase 1. The Phase 0 decision must identify when each package first becomes required.

The machine-readable manifest must distinguish at least:

- `first_required_phase`: the first phase whose accepted part requires the package to exist and own real implementation;
- `lifecycle`: `planned`, `active`, `deferred`, `optional`, or `retired`, with human/ADR disposition for deferred or optional entries;
- `path` and sole responsibility;
- allowed dependency edges and prohibited edges;
- the package/app's test locations and required verification commands.

The checker must at minimum:

1. enumerate actual Cargo packages with Cargo metadata and compare them with manifest entries due by the current phase and accepted part; continue checking all already-active workspace packages;
2. detect unexpected packages, missing **due** packages, duplicate ownership, prohibited dependency edges and unmapped source/test directories;
3. enforce the approved dependency allowlist, including domain purity and the prohibition on production dependencies on testkit;
4. keep crate-owned test paths within the owning crate and reject root-level tests that actually belong to one package;
5. verify Tauri/React source and tests remain inside the desktop package, native commands stay inside `src-tauri`, and each in-scope app has its required test/build command wired to CI;
6. verify each required package's test/lint/build target is actually invoked and report package-level results rather than hiding missing targets behind a workspace-level green status;
7. fail closed if its inputs are missing, malformed or ambiguous.

A future package may be absent before `first_required_phase`; that absence is not a failure and is not permission to implement its responsibility in another package. When a phase/part needs that responsibility, an absent owner blocks implementation until the plan and package lifecycle are reviewed. Deferred or optional packages must not disappear from the map: their disposition remains visible and downstream work that depends on them remains blocked until resolved.

The checker must consume a structured manifest generated or maintained from the **human-approved** map. Do not parse arbitrary prose as if it were a reliable manifest. Do not create this validator or claim that it runs during Phase 0; its executable implementation and CI integration belong to Phase 1.

The inventory itself is not a substitute for semantic review. Stage 06 Audit must still compare each requirement with its actual owning implementation and tests.

## 8. Approval and change control

Before this document becomes the approved implementation map, Phase 0 must resolve the conflicts in §3, review crate/module dependency direction, ratify the Tauri React boundary and test-placement policy, and record the decisions in the Phase 0 reconciliation artifact and relevant ADRs. A later part may not silently revise ownership. Any justified ownership change must update this map, the machine-readable enforcement manifest, affected tests, phase-part plan and architecture evidence together.

The current phase remains Phase 0 until the human accepts that package and file ownership baseline.