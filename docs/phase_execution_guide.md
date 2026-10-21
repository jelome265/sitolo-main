# Sitolo — Phase Execution and Transition Guide

**Purpose:** Direct ICM to the right contract, work, exit evidence and next phase.  
**Scope:** Implementation Phases 0–20 inclusive.  
**Active restart position:** Phase 0 — Architecture, Contracts and ADR Freeze.  
**Authority:** `docs/implementation_plan.md` owns phase order and objectives; each linked phase contract owns its implementation requirements; higher-authority product, security, domain, provider and regulatory sources retain their respective authority.

## 1. How to use this guide

At the beginning of each phase, use ICM Stage 01 to create a new bounded run brief. Read this guide, the canonical contract for the selected phase, its stated prerequisites and only the routed source material. Do not load every future-phase document by default.

Each phase entry below specifies:

- **Purpose:** the capability/boundary the phase establishes.
- **Start from:** the governing contract and upstream prerequisites.
- **Work and evidence:** what the phase must establish and prove, not merely describe.
- **Exit / next:** the gate that must be accepted before moving forward.

The default sequence is Phase 0 → 1 → 2 → … → 20. A phase does not advance because a later contract exists, a checklist is ticked, a PR merges, or an implementation compiles. If a dependency is not satisfied, leave the phase open and record the exact blocker. Parallel work is permitted only when the implementation plan and owners agree that the dependency is safe; it cannot bypass a phase gate.

Every consequential phase run uses the existing ICM pipeline:

`01 Select → 02 Research → 03 Investigate → 04 Plan → 05 Implement → 06 Audit → 07 Remediate → 08 Verify → 09 Deliver`

Research and remediation are decision gates; their output may explicitly state why work is not required. Human review remains required at the stage handoffs defined by ICM.

## Phase parts and code ownership (applies to every phase)

Before implementation in any Phase 0–20, ICM must use [`docs/phase_part_execution_contract.md`](phase_part_execution_contract.md) to propose a bounded part sequence and an explicit current-part write set. The agent investigates and proposes; **the human reviews, edits and accepts the sequence and current part**. Do not start implementation merely because a phase or part is documented. A plan must map each in-scope requirement to its approved crate/app/path, exact implementation files, exact test files and verification commands. Every other relevant package must be marked affected or `not touched` with a reason.

Use [`docs/code_ownership_map.md`](code_ownership_map.md) for proposed Rust, API/worker, migration, React Native, Tauri + React, generated-client and test ownership. Its package inventory is a **Phase 0 proposal**, not an approved list; conflicting source layouts must be resolved before Phase 1 scaffolding. Do not put behavior from an omitted crate in a convenient neighboring crate, move tests to the repository root to avoid crate-local test ownership, create empty crates, or duplicate domain rules in the frontend/native shell. If actual repository state contradicts the accepted plan, stop at the planning gate and revise it with the human.

Part acceptance is not phase acceptance. The delivery handoff names the exact next **agreed** part or a stop/blocker condition; delivery never self-approves the next part.

## 2. Phase progression

### Phase 0 — Architecture, Contracts and ADR Freeze

- **Canonical contract:** [Phase 0 contract](phase0_architecture_contract_freeze.md)
- **Purpose:** Reconcile product scope, domain, runtime architecture, data/API contracts, security, operations, ADRs and the full phase sequence into one approved implementation baseline.
- **Start from:** The Phase 0 contract and the smallest relevant set of canonical design sources. Treat copied content as material to inspect, not inherited completion evidence.
- **Work and evidence:** Produce the Phase 0 selection brief, research memo where needed, source-backed investigation, contract/ADR reconciliation record, proposed code/package/client/test ownership map reconciliation, plan, audit/remediation decisions and revision-attributable verification report. Resolve the competing Rust package lists and the API/worker, migrations, Tauri/React and test-placement choices before Phase 1 can scaffold the target layout.
- **Exit / next:** Human-approved Phase 0 exit package; material decisions are approved or explicitly owned and safely deferred. Then start **Phase 1**. Do not start implementation work that depends on a disputed architecture decision.

### Phase 1 — Repository, Rust Workspace and CI

- **Canonical contract:** [Phase 1 contract](phase1_repository_rust_workspace_ci_deep_implementation.md)
- **Purpose:** Create a reproducible engineering foundation: pinned toolchain, workspace/package boundaries, lockfile, dependency/supply-chain policy and required CI checks.
- **Start from:** Approved Phase 0 architecture and ADRs; record actual repository topology and tool availability.
- **Work and evidence:** Establish deterministic local/CI commands, formatting/lint/build/test gates, dependency and secret scanning, least-privileged CI permissions, reproducible artifact inputs, and failure behavior for required checks. Convert the approved ownership map into one machine-readable manifest and a required `scripts/ci/check-code-ownership` (or human-approved equivalent) CI gate. Verify the actual Cargo package inventory/dependency edges, package-local tests, testkit dependency restrictions, and Tauri/React + React Native build/test commands so a workspace-level green result cannot hide an omitted package or skipped client.
- **Exit / next:** A clean checkout reproduces the declared toolchain and required checks; missing tools are not reported as passing. Then start **Phase 2**.

### Phase 2 — Configuration, Secrets, Errors and Telemetry

- **Canonical contract:** [Phase 2 contract](phase2_config_secrets_logging_errors_telemetry_implementation.md)
- **Purpose:** Establish safe runtime configuration, isolated secret handling, structured error semantics, logging, tracing, metrics and health/readiness behavior.
- **Start from:** Phase 1 repository/CI controls and Phase 0 security/operability contracts.
- **Work and evidence:** Validate configuration at startup; redact sensitive values; make error mapping stable and non-leaking; bound telemetry cardinality; test poisoned/invalid config, initialization failures and unavailable telemetry dependencies.
- **Exit / next:** Runtime starts fail safely on invalid mandatory configuration, errors are controlled, and telemetry/health behavior has executable evidence. Then start **Phase 3**.

### Phase 3 — Identity, Sessions, MFA and Device Identity

- **Canonical contract:** [Phase 3 contract](phase3_identity_sessions_mfa_device_identity_implementation.md)
- **Purpose:** Establish trusted identities, authentication/session lifecycle, recovery, MFA where required, device registration and revocation.
- **Start from:** Phase 0 auth/security/API contracts and Phase 2 configuration, error and telemetry primitives.
- **Work and evidence:** Define token/session validity, rotation/revocation, account recovery, device binding and lost-device behavior; test expired, replayed, revoked and malformed credentials plus unauthorized flows.
- **Exit / next:** Authentication state and revocation are enforced server-side with negative tests and audit/telemetry evidence. Then start **Phase 4**.

### Phase 4 — Tenant, Organization, Branch and IAM Foundations

- **Canonical contract:** [Phase 4 contract](phase4_tenant_organization_branch_iam_implementation.md)
- **Purpose:** Model organizations, memberships, branches/resources, actor relationships and the first trusted scope boundaries.
- **Start from:** Phase 3 identity/device context plus approved domain, API, database and security contracts.
- **Work and evidence:** Enforce membership/state transitions and resource scope at trusted boundaries; cover cross-tenant and cross-branch negative cases; define where generalized policy enforcement belongs in Phase 6 and database enforcement belongs in Phase 5.
- **Exit / next:** The organization and scope model is contract-consistent and its implemented claims have current negative evidence. Then start **Phase 5**. Historical part-specific run records do not select active work.

### Phase 5 — PostgreSQL Schema, Migrations, Constraints and RLS

- **Canonical contract:** [Phase 5 contract](phase5_postgresql_schema_migrations_constraints_rls_implementation.md)
- **Purpose:** Make PostgreSQL's schema, constraints, roles and row-level security enforce the required authoritative data boundary.
- **Start from:** Approved database/domain design and Phase 4 tenant/resource semantics; Phase 1 CI/test infrastructure.
- **Work and evidence:** Review migration locking/compatibility/rollback; least-privileged database roles; constraints and indexes; transaction-local scope; real PostgreSQL tests for RLS, isolation, privileges and failure paths. Mocks alone cannot prove PostgreSQL behavior.
- **Exit / next:** Required migrations and database security properties are verified against a real supported PostgreSQL instance with attributable evidence. Then start **Phase 6**.

### Phase 6 — Authorization Engine and Policy Enforcement

- **Canonical contract:** [Phase 6 contract](phase6_authorization_engine_policy_enforcement_implementation.md)
- **Purpose:** Enforce deny-by-default, object/property/state authorization, scoped permissions and approval/separation-of-duty policies.
- **Start from:** Phase 3 identity, Phase 4 resource semantics, Phase 5 database boundaries and the canonical auth/security contracts.
- **Work and evidence:** Inventory protected actions and routes; map policy decisions to trusted actor/resource context; reject missing or ambiguous policy; test direct object references, role escalation, stale membership, scope mismatch and state-transition violations.
- **Exit / next:** Protected operations are mapped and enforcement is demonstrated by positive and negative tests. Then start **Phase 7**.

### Phase 7 — Security Test Framework

- **Canonical contract:** [Phase 7 contract](phase7_security_test_framework_implementation.md)
- **Purpose:** Make security verification reusable, executable and required in the delivery pipeline.
- **Start from:** Threat model, security controls, CI enforcement, testing strategy and the surfaces established in Phases 2–6.
- **Work and evidence:** Establish fixtures and reusable negative-test helpers; add authorization/tenant regression tests, dependency/security scanning, relevant fuzz/property tests, mutation or equivalent test-quality evidence and fail-closed CI reporting.
- **Exit / next:** Required security checks actually execute, failures fail the gate, and coverage limitations are visible. Then start **Phase 8**; security testing continues in every subsequent phase.

### Phase 8 — Product Catalogue and Pricing

- **Canonical contract:** [Phase 8 contract](phase8_product_catalogue_implementation.md)
- **Purpose:** Establish product/SKU identity, units and conversions, barcodes where applicable, price lists and controlled pricing/promotions.
- **Start from:** Approved product/domain/API/data contracts plus Phases 5–7 persistence and authorization controls.
- **Work and evidence:** Define uniqueness, tenant ownership, unit precision, effective-dated price rules, validation and concurrency behavior; preserve price snapshots needed to explain finalized transactions.
- **Exit / next:** Catalogue and pricing rules are deterministic and tested across invalid units, duplicate identifiers, price changes and unauthorized edits. Then start **Phase 9**.

### Phase 9 — Inventory Ledger

- **Canonical contract:** [Phase 9 contract](phase9_inventory_ledger_implementation.md)
- **Purpose:** Establish authoritative stock movements, balances and explainable inventory history.
- **Start from:** Phase 8 catalogue/unit contracts and Phase 5–7 persistence/security foundations.
- **Work and evidence:** Model receipts, issues, transfers, counts and adjustments as explicit movements; establish transaction/concurrency rules, costing where required, and the distinction between ledger facts and derived balances.
- **Exit / next:** Concurrent and duplicate movements cannot silently corrupt stock; balances can be reconciled to movements and corrections remain explainable. Then start **Phase 10**.

### Phase 10 — POS and Sales

- **Canonical contract:** [Phase 10 contract](phase10_pos_sales_implementation.md)
- **Purpose:** Execute a sale as a controlled business command that composes product, price, stock, tender, audit and downstream intent.
- **Start from:** Catalogue/pricing, inventory, authorization, persistence, idempotency and event/audit contracts.
- **Work and evidence:** Define draft/finalized states, atomic finalization, price/tax/discount authority, register/tender rules, duplicate submission behavior, concurrent stock contention and crash/unknown-outcome handling.
- **Exit / next:** Sale finalization is atomic, idempotent and explainable, with real transaction/failure tests. Then start **Phase 11**.

### Phase 11 — Payments and Reconciliation

- **Canonical contract:** [Phase 11 contract](phase11_payments_reconciliation_implementation.md)
- **Purpose:** Track payment intent, attempts, provider responses, webhooks, unknown outcomes and reconciliation without pretending external systems are synchronous or infallible.
- **Start from:** Sale/tender semantics, provider contracts, security controls and persistence/idempotency primitives.
- **Work and evidence:** Isolate provider adapters; enforce timeouts and bounded retry; verify webhook authenticity and replay handling; preserve provider references; reconcile mismatches; never blindly repeat a payment with an unknown outcome.
- **Exit / next:** Success, decline, timeout, duplicate notification and ambiguous outcomes have durable tested transitions and reconciliation evidence. Then start **Phase 12**.

### Phase 12 — Offline Synchronization

- **Canonical contract:** [Phase 12 contract](phase12_offline_synchronization_implementation.md)
- **Purpose:** Support bounded offline continuity while keeping server authority, authorization and financial truth intact.
- **Start from:** Defined device identity/revocation, sale/inventory commands, local persistence, server APIs and idempotency rules.
- **Work and evidence:** Define local journal, outbox, push/pull, checkpoints, replay, conflict policy, schema/version compatibility and revocation behavior; test process death, network changes, retries, duplicates and stale devices.
- **Exit / next:** Accepted commands are durable and replay-safe; checkpoints move only after durable acceptance; conflicts are explicit rather than silently overwriting money or stock. Then start **Phase 13**.

### Phase 13 — Procurement and Suppliers

- **Canonical contract:** [Phase 13 contract](phase13_procurement_suppliers_implementation.md)
- **Purpose:** Manage supplier records, purchase orders, approvals, goods receipt, discrepancies and procurement evidence.
- **Start from:** Catalogue units, inventory ledger, tenant/authorization controls and approved supplier workflow.
- **Work and evidence:** Separate ordered, received and accepted quantities; define approval and partial receipt states; connect accepted receipts to inventory movements without duplicate posting; preserve supplier and receipt references.
- **Exit / next:** Procurement-to-receipt transitions are auditable and replay-safe, and inventory reconciliation is tested. Then start **Phase 14**.

### Phase 14 — Returns, Refunds and Cash

- **Canonical contract:** [Phase 14 contract](phase14_returns_refunds_cash_implementation.md)
- **Purpose:** Correct prior operations through controlled returns, voids/reversals, refunds, register closing and cash variance workflows.
- **Start from:** Finalized sale, inventory, payment, audit and register/tender contracts.
- **Work and evidence:** Use compensating operations rather than destructive edits; define refundable/returnable entitlement, approval limits, cash drawer state, reconciliation and duplicate-request behavior.
- **Exit / next:** Corrections preserve original facts and link to the original operation; cash and inventory effects are atomic/idempotent with failure tests. Then start **Phase 15**.

### Phase 15 — Malawi Revenue Authority Electronic Invoicing (MRA EIS)

- **Canonical contract:** [Phase 15 contract](phase15_mra_eis_implementation.md)
- **Purpose:** Implement the approved fiscal integration and evidence required for the applicable MRA Electronic Invoicing System pathway.
- **Start from:** Current official MRA requirements and provider access/certification contracts, plus sale/payment/security/deployment boundaries.
- **Work and evidence:** Verify current external requirements; implement explicit submission, response, rejection, timeout and reconciliation states; protect signing keys/credentials; preserve immutable request/response evidence and certification records.
- **Exit / next:** Applicable certification and integration evidence is current, attributable and accepted by the responsible authority/owner. Do not claim regulatory approval from local tests alone. Then start **Phase 16**.

### Phase 16 — Reporting and Exports

- **Canonical contract:** [Phase 16 contract](phase16_reporting_exports_implementation.md)
- **Purpose:** Produce explainable operational/financial read models, reports and controlled exports from authoritative domain facts.
- **Start from:** Approved report definitions, domain/data ownership, authorization and observability contracts.
- **Work and evidence:** Define metric semantics, time boundaries and reconciliation; enforce tenant/role scope, row/size/time limits, export access controls and sensitive-data redaction.
- **Exit / next:** Report totals reconcile to their source facts and unauthorized or oversized exports are prevented with evidence. Then start **Phase 17**.

### Phase 17 — Billing and Entitlements

- **Canonical contract:** [Phase 17 contract](phase17_billing_entitlements_implementation.md)
- **Purpose:** Manage Sitolo subscriptions, invoices, collections, plan entitlements and usage-based limits independently from merchant payment transactions.
- **Start from:** Approved commercial pricing/packaging decisions and Phase 11 payment/reconciliation primitives, with a clear distinction between merchant funds and Sitolo subscription funds.
- **Work and evidence:** Define subscription lifecycle, grace/suspension states, idempotent collection, invoice/entitlement reconciliation and what happens when billing services are unavailable.
- **Exit / next:** Subscription and entitlement state is authoritative, auditable and reconciled without silently blocking data export or violating approved customer terms. Then start **Phase 18**.

### Phase 18 — Administration and Support

- **Canonical contract:** [Phase 18 contract](phase18_admin_support_implementation.md)
- **Purpose:** Provide controlled operational administration and customer support without a hidden superuser or implicit tenant bypass.
- **Start from:** Security control/incident policy, tenancy and authorization contracts, privacy constraints, support/commercial operating model.
- **Work and evidence:** Enforce just-in-time and time-bounded access, purpose limitation, approvals, audit trails, break-glass handling, queue isolation and incident escalation.
- **Exit / next:** Privileged actions are explicit, least-privileged, attributable and tested; support cannot bypass tenant/data boundaries without an approved controlled mechanism. Then start **Phase 19**.

### Phase 19 — Hardening, Performance and Disaster Recovery

- **Canonical contract:** [Phase 19 contract](phase19_hardening_performance_dr_implementation.md)
- **Purpose:** Demonstrate that integrated capabilities meet agreed performance, availability, recovery, rollback and operational requirements.
- **Start from:** Implemented upstream capabilities and agreed workload/SLO assumptions; do not fabricate targets where owners have not approved them.
- **Work and evidence:** Run representative load/soak/failure tests; measure hot paths, resource bounds and degradation; verify backup/restore, recovery-point/time objectives, migration recovery and rollback procedures.
- **Exit / next:** Measured results meet approved thresholds, recovery drills succeed, and material risks have owners and dispositions. Then start **Phase 20**.

### Phase 20 — Production Certification

- **Canonical contract:** [Phase 20 contract](phase20_production_certification_implementation.md)
- **Purpose:** Decide whether one exact release candidate is ready for production from complete technical, security, operational and applicable compliance evidence.
- **Start from:** Verified upstream phase exits, release governance, external requirements and a pinned candidate artifact/revision.
- **Work and evidence:** Recheck required security/integration gates; map evidence to the exact candidate; verify SBOM/provenance, migration and rollback, operational readiness, monitoring, incident response and applicable regulator/provider approval.
- **Exit / next:** Human release authority approves or rejects the exact candidate. If approved, deliver the release handoff and monitoring/rollback plan. If rejected, route findings to the owning phase and repeat the required audit/remediation/verification. Phase 20 has no automatic next implementation phase.

## 3. Status and transition rules

Use one authoritative status pointer in `docs/current_implementation_status.md` and keep it consistent with the phase contract register and ICM phase router.

Recommended phase lifecycle:

`PLANNED → IN_PROGRESS → IMPLEMENTED → TESTED → SECURITY_VERIFIED (where applicable) → OPERATIONALLY_VERIFIED (where applicable) → REVIEWED → COMPLETE`

A phase may not jump to COMPLETE. Each phase exit records:

- phase and contract revision;
- implementation/reconciliation revision;
- checks actually executed and results;
- skipped checks and reasons;
- security, integration and operational evidence as applicable;
- unresolved risks/decisions and accepted owners;
- human decision and timestamp;
- explicit next phase or a reason the program stops.

A reopened finding reopens the relevant gate. Fixes that affect behavior return to the appropriate audit stage before verification. History remains history; it is not current state.

## 4. No phase skipping

Do not move to Phase N+1 while mandatory Phase N exit conditions remain open. Later contracts may be prepared ahead for planning, but must not drive implementation ahead of unresolved predecessors. An exception requires a documented dependency analysis, owner approval and an ADR where architecture/dependency semantics change. Security, tenant isolation and financial-integrity constraints cannot be waived for convenience.
