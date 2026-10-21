# ADR-026 — React Native as the Sole Mobile Framework

**Status:** Accepted  
**Decision date:** 2026-10-21  
**Decision owner:** Sitolo product owner  
**Approval record:** Explicit product direction in the Phase 0 ownership reconciliation discussion on 2026-10-21  
**Supersedes:** ADR-002's accepted Flutter selection  
**Scope:** Sitolo mobile application framework and mobile code ownership

## Context

Sitolo needs a dedicated mobile client for Android-first merchant workflows, local continuity, unreliable networks, secure device integration and offline command synchronization. The architecture corpus previously named Flutter as the mobile framework. The product owner has explicitly prohibited Flutter and selected React Native instead.

## Decision

**React Native is the only permitted mobile application framework for Sitolo. Flutter is not allowed in this project.**

- Mobile presentation and interaction use React Native components and TypeScript/JavaScript.
- Mobile UI, feature orchestration, local data access, client sync adapters, app configuration and tests live under `apps/mobile/`.
- Platform-specific Android/iOS integration and any approved native modules remain owned by the mobile app boundary. Use only narrow native capabilities needed by approved requirements.
- Durable local persistence, offline command/outbox handling, replay, checkpoints, conflict policy and local sensitive-data protection remain explicit architectural responsibilities. React Native does not itself establish offline correctness or authorize financial operations.
- Rust backend contracts remain authoritative for server-side business rules, tenant access, inventory, sales, payments, reconciliation and fiscal state. The mobile application may provide presentation validation, but it must not create a competing source of business truth.
- Mobile tests stay inside `apps/mobile/`. Unit and component tests remain close to their source; full device/E2E tests live in a dedicated mobile-owned test area. Root `tests/` is reserved for genuine cross-application/system assertions.
- React Native and Tauri both use React-related frontend technology, but their rendering and platform boundaries differ. The desktop webview UI is not a shared implementation of the mobile UI. Share only deliberate, platform-neutral contracts or utilities with an approved owner; do not force a common UI package.

## Decisions still open

This ADR does not silently choose the React Native workflow or supporting libraries. The following must be decided and pinned through Phase 0/Phase 1 review as applicable:

- React Native Community CLI versus an approved React Native framework such as Expo, based on native requirements, offline behavior, build/release control and supported capabilities.
- TypeScript strictness, package manager and lockfile, current compatible React Native/React/Node/Android/iOS toolchains.
- Navigation, state management, secure credential storage and the SQLite/local-persistence library.
- Device/E2E testing framework and native-module strategy.

No mobile manifest or native project should be scaffolded until the accepted Phase 0 part sequence assigns the decisions and Phase 1 is authorized. Do not pin a version simply because it is the newest release; verify the supported release matrix and all build requirements before selecting the baseline.

## Ownership and test contract

Proposed mobile source map, subject to the agreed Phase 0 ownership baseline:

```text
apps/mobile/
  package.json                   # dependencies and reviewed scripts
  <one package-manager lockfile> # selected and pinned in Phase 1
  src/
    app/                         # bootstrap and navigation composition
    features/<feature>/          # feature screens/components/state, colocated tests
    shared/                      # genuinely shared UI/client utilities
    data/                        # approved API/local persistence adapters
    sync/                        # mobile protocol client only; server remains authoritative
  android/                       # native project/config when required by selected workflow
  ios/                           # native project/config when required by selected workflow
  e2e/                           # full device journeys using the approved harness
```

This is a mapped target, not permission to create every directory in advance. Each part's plan must name exact paths and tests. Business rules must not migrate into a convenient mobile component because a backend crate or API is missing.

Required proof includes unit/component tests in the mobile package, tests of local persistence/migrations and sync/replay behavior, and real emulator/device coverage for platform-specific storage, lifecycle, network transitions and process death where relevant. UI mocks do not prove native storage or device lifecycle behavior.

## Consequences

- The prior Flutter selection is superseded; Flutter/Dart manifests, source trees, toolchains and tests must not be added to the active design.
- Phase 0 ownership/path/test maps and the testing strategy must reflect React Native before the mobile application is scaffolded.
- Phase 1 owns the reproducible package/toolchain/build/test setup after those choices are approved.
- The framework choice does not authorize duplicating server-owned business logic or changing the backend's authority.

## References

- React Native, Learn the Basics: https://reactnative.dev/docs/tutorial
- React Native, Environment setup: https://reactnative.dev/docs/set-up-your-environment
- React Native, Testing overview: https://reactnative.dev/docs/testing-overview
- React Native, Platform-specific code: https://reactnative.dev/docs/platform-specific-code
- React Native release announcements: https://reactnative.dev/blog

Use current official documentation when Phase 1 selects the framework version and native toolchain. As of the Phase 0 research conducted on 2026-10-21, the official React Native blog lists a 0.88 announcement dated 2026-10-19; this is evidence to review the current release matrix, not an automatic version-pin decision.
