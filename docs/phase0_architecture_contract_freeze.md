# Sitolo — Phase 0: Architecture, Contracts and ADR Freeze

**Role:** Canonical Phase 0 implementation-governance contract  
**Program position:** Phase 0 of 21 (0–20)  
**Lifecycle:** Active restart baseline; Phase 0 is not yet closed  
**Execution mechanism:** The existing ICM engineering pipeline (Stages 01–09)  
**Authority boundary:** This document reconciles the Phase 0 work. It does not replace the authoritative subject-specific documents it references.

## 1. Purpose

Phase 0 establishes a coherent, reviewable model of what Sitolo is, what it must guarantee, how its major parts fit together, and which decisions must be settled before implementation work relies on them.

This restart begins at Phase 0. Files present in the repository are inputs to reconcile, not proof that a phase has been implemented, tested, approved or closed. Do not infer completion from copied code, document length, an existing checklist, or historical run output.

Phase 0 is complete only when the required architecture and contract baseline is coherent, unresolved decisions are explicit, the responsible human has accepted the baseline, and the exit evidence is recorded against a specific repository revision.

## 2. Required inputs

Start with the minimum routed context and then follow only the references needed to resolve a concrete question.

| Input | Canonical location | Role |
|---|---|---|
| Product definition | `docs/sitolo.md` and approved product documents | Product purpose, users and boundaries |
| Architecture | `docs/system_architecture_design.md` | Runtime, module, client, deployment and trust boundaries |
| Domain model | `docs/domain_model.md` | Business language, ownership, invariants and state transitions |
| Data | `docs/database_design.md` | Persistence authority, constraints and data ownership |
| API | `docs/api_contract.md` | External and internal interface contracts |
| Identity and access | `docs/auth_authorization_spec.md`, `docs/security_architecture_design.md` | Authentication, authorization and trust boundaries |
| Security | `docs/threat_model.md`, `docs/security_implementation_spec.md`, `docs/security_control_register.md` | Threats, controls and evidence expectations |
| Continuity and integrations | `docs/sync_protocol.md`, `docs/payment_integration_spec.md`, `docs/mra_eis_integration_spec.md` | Offline, payment and fiscal-provider boundaries |
| Operations and verification | `docs/observability_spec.md`, `docs/deployment_spec.md`, `docs/testing_strategy.md`, `docs/ci_enforcement.md` | Operability, test strategy, release controls |
| Decisions and phase order | `docs/ADR-001-025.md`, `docs/implementation_plan.md` | Existing decisions and the Phase 0–20 dependency sequence |
| Phase routing | `docs/phase_execution_guide.md`, `docs/phase_contract_coverage_register.md`, `docs/phase_part_execution_contract.md` | Phase/part sequence, implementation gates and how to advance |
| Code ownership | `docs/code_ownership_map.md` | Proposed package/client/file ownership and test placement; must be ratified in Phase 0 before Phase 1 scaffolding |

A listed file can be outdated, incomplete or inconsistent. Inspect its content and links before treating it as authoritative. Current external laws, regulator requirements and provider contracts must be rechecked when a decision depends on them.

## 3. Required Phase 0 work

### 3.1 Establish product and business boundaries

- Identify primary users, buyers, payers, approvers and operational actors from approved product/commercial material.
- Define the initial problem, core workflows, first-market constraints, explicit exclusions and assumptions that still require validation.
- Separate approved requirements from commercial hypotheses.
- Route material product ambiguity to the Product workspace. Route commercial assumptions to the Commercial workspace; do not turn either into an engineering fact by inference.

### 3.2 Reconcile architecture and ownership

- Verify the intended client surfaces, backend runtime, persistence authorities and external integration boundaries.
- Confirm domain and module ownership, dependency direction, and the boundary between the modular monolith, workers and adapters.
- Reconcile the conflicting candidate crate/package lists in `docs/system_architecture_design.md` and `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`; do not silently select or combine them.
- Review `docs/code_ownership_map.md` for Rust crate roles, executable entry points, migration ownership/path, Flutter mobile, Tauri + React, generated API types, and test locations. Resolve the package inventory and Tauri workspace/lockfile boundary before Phase 1 scaffolding; record approved decisions in ADRs and retain unresolved blockers explicitly.
- Confirm server-side authority for authorization, tenant/resource scope and financially significant decisions.
- Identify cross-context interactions and their data/API/event contracts.
- Record material architecture decisions in the existing ADR system. Do not silently change a decision because an implementation would be easier.

### 3.3 Reconcile domain, API and data contracts

- Align domain terms, aggregate ownership, invariants, state machines and correction semantics.
- Ensure API contracts identify authentication, authorization, scope, validation, errors, idempotency and compatibility expectations where applicable.
- Ensure the database design expresses required constraints and transaction boundaries; do not imply that a document alone proves enforcement.
- Define external-provider boundaries, unknown-outcome states, retries and reconciliation responsibilities.
- Identify contradictions between documents and route each to the owner of the higher-authority decision.

### 3.4 Reconcile security and operational requirements

- Identify assets, trust boundaries, threats, control ownership and negative-test expectations.
- Establish the requirements for secret handling, least privilege, tenant isolation, audit evidence and sensitive-data redaction.
- Specify observability, bounded resource use, timeouts, retries, cancellation, graceful degradation, health checks and incident response where applicable.
- Specify migration compatibility, backup/restore, rollback and recovery expectations for stateful changes.
- Distinguish telemetry from audit evidence; neither substitutes for authoritative domain state.

### 3.5 Reconcile implementation and release sequence

- Validate that Phases 0–20 follow `docs/implementation_plan.md`.
- Confirm each phase has a canonical contract, required predecessors, exit evidence and an explicit next-phase route in `docs/phase_execution_guide.md`.
- Treat later-phase contracts as target requirements, never as proof the capability exists.
- Define the repository's first implementation baseline and the checks that will protect it in Phase 1.
- Record missing files, broken references, unverified claims and required follow-up work without broad speculative refactoring.

## 4. Required artifacts

The ICM run must produce or update the following artifacts, using the existing stage templates where applicable:

1. **Phase 0 selection brief** — scope, outcomes, exclusions, stakeholders/decision owners, security relevance, and success criteria.
2. **Research memo** — dated authoritative evidence for external or version-sensitive claims; mark research as not required with a reason where appropriate.
3. **Investigation report** — exact source files inspected, conflicts, gaps, broken references and evidence-backed findings.
4. **Phase 0 plan** — ordered reconciliation work, owners/decision points, acceptance checks and dependencies.
5. **Architecture/contract reconciliation record** — canonical source per concern, conflicts resolved or explicitly open, and affected ADRs.
6. **Code ownership decision** — reviewed `docs/code_ownership_map.md`, approved package/client/test boundary decisions, unresolved options and the ADRs or phase gates affected.
7. **Phase-part sequence and approval record** — proposed Phase 0 part sequence, exact current part/write set, human decision, and next agreed part or stop condition.
8. **Phase 0 implementation/reconciliation report** — changes made and deliberately deferred, with file paths.
9. **Audit and remediation records** — findings, severity, fix decisions and unresolved risk acceptance where relevant.
10. **Verification report** — checks run/not run with reasons, results, repository revision and known limitations.
11. **Delivery handoff** — PR/revision, summary, evidence, outstanding decisions and the exact next agreed action.

Artifacts must state facts, assumptions, risks and open decisions separately. A missing artifact is pending; file presence does not prove approval or quality.

## 5. Phase 0 exit criteria

Phase 0 may be marked complete only when every applicable condition below has explicit evidence:

- [ ] Product scope, primary workflows and exclusions have been reviewed and accepted.
- [ ] The architecture, module ownership, client/runtime boundaries and authoritative data responsibilities are mutually consistent.
- [ ] The approved Rust package inventory, API/worker composition roots, migration path, Tauri + React native boundary, Flutter mobile boundary, and package/app test locations have one explicit owner each. Any unresolved choice that affects Phase 1 scaffolding blocks Phase 1 until human disposition.
- [ ] Domain invariants, API contracts, persistence rules and integration boundaries do not contain unresolved material contradictions.
- [ ] Security and operational requirements are routed to their canonical sources and material gaps are recorded.
- [ ] Every material architecture decision is either recorded and approved in an ADR or listed as open with an owner and disposition rule.
- [ ] The Phase 0–20 sequence and phase-to-phase transition rules are explicit.
- [ ] All referenced canonical documents and required ICM routes have been checked; missing tooling or broken paths are recorded and either repaired or explicitly gated.
- [ ] Phase 0 verification evidence names the exact branch/commit, commands actually run, results and skipped checks.
- [ ] The responsible human reviews and accepts the Phase 0 exit package.

Do not fabricate approval, mark an unexecuted check as passed, or close Phase 0 solely because this checklist exists.

## 6. Open decisions

Record decisions that cannot be settled from approved sources in a visible decision register or the existing ADR process. For each, capture the question, why it matters, options/evidence, decision owner, dependency, due condition, and the phases blocked by it.

An unresolved decision may remain open only when the architecture supports deferring it safely and the downstream boundary is explicit. A decision that changes a security invariant, data authority or phase dependency is not silently deferred.

## 7. Transition to Phase 1

**Next phase:** Phase 1 — Repository, Rust workspace and CI foundation.

Phase 1 may begin only after the Phase 0 exit package is reviewed. It consumes the approved architecture and ADR baseline **and the human-approved code ownership map** to establish a reproducible repository/toolchain, approved workspace/package structure, dependency policy, CI gates and supply-chain controls. It must not create crates/apps from an unratified candidate list. The map's machine-readable manifest and required ownership/dependency/test-placement checker belong to Phase 1, after the human-readable ownership decisions are approved. It must not use Phase 1 scaffolding to make unresolved Phase 0 product or architecture decisions implicitly.

## 8. ICM execution rule

Phase 0 is executed through the existing pipeline:

`01 Select → 02 Research → 03 Investigate → 04 Plan → 05 Implement/Reconcile → 06 Audit → 07 Remediate → 08 Verify → 09 Deliver`

The phase sequence and stage sequence are different axes. Completing an ICM stage does not complete a product phase. Apply `docs/phase_part_execution_contract.md`: the agent proposes phase parts, the human reviews/edits/accepts the sequence and current part, and implementation stays within the accepted write set. The ICM run must not resume a prior phase-specific run unless a human explicitly selects it as relevant historical evidence.

