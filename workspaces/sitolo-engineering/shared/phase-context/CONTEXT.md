# Phase Context Router

**Current active product phase:** Phase 0 — Architecture, Contracts and ADR Freeze.  
**Current ICM action:** Start a fresh Stage 01 Select run.  
**Canonical phase sequence:** `docs/implementation_plan.md`.  
**Phase-specific direction:** `docs/phase_execution_guide.md`.

This file is an ICM Layer 3 routing surface. It selects the relevant phase contract; it does not override the implementation plan or any authoritative domain/security/provider source.

For every implementation-phase run, also apply `../../../../docs/phase_part_execution_contract.md`. Load `../../../../docs/code_ownership_map.md` when planning or changing package/app/file ownership. That map is a Phase 0 proposal until the human accepts its decisions; do not scaffold candidate crates or move responsibilities between them before then.

## Active phase

| Phase | Canonical contract | Load when | Expected next destination |
|---|---|---|---|
| **0 — ACTIVE** | `../../../../docs/phase0_architecture_contract_freeze.md` | Every new implementation-program run during the restart baseline | Phase 1 only after Phase 0 exit is reviewed and accepted; package map must be ratified first |

Read the Phase 0 contract and the phase execution guide, then route only the relevant canonical inputs listed by the Phase 0 contract. Do not load all future-phase contracts by default.

## Phase contracts — reference index

All paths below are relative to this file.

| Phase | Contract | Scope |
|---:|---|---|
| 0 | `../../../../docs/phase0_architecture_contract_freeze.md` | Architecture, contracts and ADR freeze |
| 1 | `../../../../docs/phase1_repository_rust_workspace_ci_deep_implementation.md` | Repository, Rust workspace, CI and supply chain |
| 2 | `../../../../docs/phase2_config_secrets_logging_errors_telemetry_implementation.md` | Configuration, secrets, errors and telemetry |
| 3 | `../../../../docs/phase3_identity_sessions_mfa_device_identity_implementation.md` | Identity, sessions, MFA and device identity |
| 4 | `../../../../docs/phase4_tenant_organization_branch_iam_implementation.md` | Tenant, organization, branch and IAM foundations |
| 5 | `../../../../docs/phase5_postgresql_schema_migrations_constraints_rls_implementation.md` | PostgreSQL schema, migrations, constraints and RLS |
| 6 | `../../../../docs/phase6_authorization_engine_policy_enforcement_implementation.md` | Authorization engine and policy enforcement |
| 7 | `../../../../docs/phase7_security_test_framework_implementation.md` | Security test framework |
| 8 | `../../../../docs/phase8_product_catalogue_implementation.md` | Product catalogue and pricing |
| 9 | `../../../../docs/phase9_inventory_ledger_implementation.md` | Inventory ledger |
| 10 | `../../../../docs/phase10_pos_sales_implementation.md` | POS and sales |
| 11 | `../../../../docs/phase11_payments_reconciliation_implementation.md` | Payments and reconciliation |
| 12 | `../../../../docs/phase12_offline_synchronization_implementation.md` | Offline synchronization |
| 13 | `../../../../docs/phase13_procurement_suppliers_implementation.md` | Procurement and suppliers |
| 14 | `../../../../docs/phase14_returns_refunds_cash_implementation.md` | Returns, refunds and cash |
| 15 | `../../../../docs/phase15_mra_eis_implementation.md` | MRA EIS integration and certification |
| 16 | `../../../../docs/phase16_reporting_exports_implementation.md` | Reporting and exports |
| 17 | `../../../../docs/phase17_billing_entitlements_implementation.md` | Billing and entitlements |
| 18 | `../../../../docs/phase18_admin_support_implementation.md` | Administration and support |
| 19 | `../../../../docs/phase19_hardening_performance_dr_implementation.md` | Performance, hardening and disaster recovery |
| 20 | `../../../../docs/phase20_production_certification_implementation.md` | Production certification |

## Historical records

The `workspaces/sitolo-engineering/runs/pr009-part8/` directory is a prior execution record. Keep it for traceability; it is not the current run and does not select Phase 4 as active. Do not resume it unless the human explicitly requests that historical run.

Part-specific Phase 4 contracts and status snapshots remain reference/history for their original scope. They are not the active phase pointer for this restart.

## Phase transition rule

The active product phase changes only when:
1. The current phase's canonical exit criteria are met with revision-attributable evidence.
2. Required security, operational and human review gates have passed.
3. `docs/current_implementation_status.md` and the phase contract register are updated consistently.
4. The next phase and its exact prerequisites are recorded in the delivery handoff.

A future-phase contract's existence never authorizes that phase to start. If a gate is blocked, keep the current phase open and report the blocker instead of routing forward.
