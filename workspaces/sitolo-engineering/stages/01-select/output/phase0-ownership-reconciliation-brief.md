---
artifact_type: run-brief
status: draft
run_slug: "phase0-ownership-reconciliation-20261021"
product_phase: "Phase 0 — Architecture, Contracts and ADR Freeze"
icm_stage: "01-select"
run_mode: "planning-only"
security_relevant: "yes"
---

# Phase 0 — Ownership Reconciliation Run Brief

## Selection

**Phase-part sequence approval:** The product owner approved the proposed P0.1–P0.8 sequence on 2026-10-21. This accepts the ordering only; each current part still needs an explicit bounded write set and stop condition. The assistant's role is architecture guidance and audit, not product-code implementation. A separate implementation agent owns product-code changes.

- **Product phase:** Phase 0 — Architecture, Contracts and ADR Freeze.
- **ICM run:** `phase0-ownership-reconciliation-20261021`.
- **Current run mode:** Planning-only. This run researches and investigates, then proposes a phase-part sequence and ownership decisions for human review. It does not authorize product implementation or Phase 1 scaffolding.
- **Current part:** None accepted yet. The proposed sequence in Stage 04 is not approved until the human reviews and edits it.
- **Working branch:** `chore/icm-reset-phase-0`.
- **PR under review:** [#1 — ICM reset, code ownership and phase-part enforcement](https://github.com/jelome265/sitolo-main/pull/1).

## Objective

Resolve the competing architecture descriptions into one reviewable ownership decision set, with explicit responsibilities and boundaries for:

1. every Rust crate in the target architecture;
2. API and worker executables and any background-job package;
3. PostgreSQL persistence, migration ownership, event contracts, audit records, telemetry and configuration;
4. the Flutter mobile client and Tauri + React desktop application;
5. API contracts and generated client/type files;
6. package-local, app-local and cross-package test placement;
7. when a target package becomes mandatory, so later-phase crates are mapped without forcing empty placeholders into Phase 1.

The output must give us a clear implementation owner for each responsibility. If ownership is not approved or a package is not yet due, the gap must be explicit; another crate may not silently absorb it.

## Scope

- Review PR #1 for correctness and internal consistency.
- Reconcile `docs/system_architecture_design.md`, `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`, `docs/implementation_plan.md`, relevant ADRs, `docs/code_ownership_map.md`, `docs/phase_part_execution_contract.md`, and `docs/testing_strategy.md`.
- Produce a complete proposed target ownership inventory, high-level dependency rules, path/test map, and bounded Phase 0 part sequence.
- Mark decisions as accepted only when the human actually accepts them.
- Identify current version-sensitive or otherwise stale claims that need verification before they become part of the frozen baseline.

## Non-goals

- Do not implement application behavior, create the Cargo workspace or scaffold client packages in this planning run.
- Do not start Phase 1, create empty future-phase crates, or infer approval from this run's output.
- Do not select package boundaries solely to maximize crate count.
- Do not duplicate the project architecture in a new parallel specification.
- Do not rewrite unrelated documentation or update a pinned toolchain without a compatibility decision and the required verification.

## Security and integrity relevance

Security relevance is **yes** because package boundaries affect the direction of trust, authorization enforcement, tenant isolation, migration privileges, test-only dependencies, client/native capabilities and CI enforcement. The run must preserve server-side authority and forbid domain dependencies on HTTP frameworks, persistence drivers and provider-specific representations.

## Mobile framework constraint

React Native is the sole permitted mobile framework by explicit product-owner decision (ADR-026). Flutter is prohibited and must not appear as an active implementation or testing option. The exact React Native workflow and tooling remain open for Phase 0/1 disposition.

## Success criteria

- Conflicting crate/package inventories are listed with evidence and resolved or presented as explicit decisions.
- Each proposed crate and application has a sole responsibility, owned paths, prohibited responsibilities and dependency boundaries.
- API/worker executable composition is distinct from reusable libraries and business use cases.
- Tauri/React, Flutter, generated API types, migrations and test paths are mapped.
- The enforcement design distinguishes the full target architecture from packages due in the current phase/part.
- The proposed Phase 0 part sequence is bounded and reviewable.
- Each choice is labelled **existing accepted decision**, **recommendation**, **open decision**, or **human-approved**.
- No implementation begins until the human accepts the current part sequence and write set.

## Required handoffs

Stage 02 Research and Stage 03 Investigate produce source-backed evidence. Stage 04 Plan proposes the phase-part sequence and decision register. Stop after the plan for human review; do not route into Stage 05 implementation automatically.
