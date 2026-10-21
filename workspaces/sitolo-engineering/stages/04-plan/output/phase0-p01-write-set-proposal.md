---
artifact_type: implementation-plan-amendment
status: proposed-awaiting-human-acceptance
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "04-plan"
product_phase: "Phase 0 — Architecture, Contracts and ADR Freeze"
current_part: "P0.1 — Source of truth and conflict register"
part_sequence_status: "sequence human-approved 2026-10-21; P0.1 write set proposed, not accepted"
security_relevant: "yes"
---

# P0.1 — Source of Truth and Conflict Register: Write-Set Proposal

## 1. Purpose of this document

Stage 04 produced an approved P0.1–P0.8 ordering but left P0.1 at *"Sequence approved; part not started"*, with the exact write set and stop condition unrecorded. `docs/phase_part_execution_contract.md` §2 requires that record before Stage 05 begins.

This amendment supplies it. It is a plan, not an approval: it does not resolve any architecture decision, does not move Phase 0 toward closure, and does not authorize product code. The record becomes binding only when the responsible human accepts the write set and stop condition in §11.

## 2. Part control

| Field | Value |
|---|---|
| Product phase | Phase 0 — Architecture, Contracts and ADR Freeze (`docs/phase0_architecture_contract_freeze.md`) |
| Part ID | **P0.1** |
| Part outcome | One conflict/decision register with cited evidence and an explicit resolution gate per row |
| Predecessor gate | P0.1–P0.8 sequence approved 2026-10-21. No Stage 05 has run for this part. |
| Work nature | Documentation reconciliation only. No product code, workspace or client artifact. |
| Approval required from | Product owner (architecture/ownership decisions); engineering owner where a row is purely mechanical |
| Approval status | **Proposed — awaiting acceptance** |

## 3. Deliverable

**`docs/phase0_conflict_and_decision_register.md`** — a citation-only register. It records *that* sources disagree and *who* must decide; it does not restate architecture, and it adds no new design prose. Every row points at an existing authoritative document and section.

This satisfies Phase 0 contract §4 artifact 5 (architecture/contract reconciliation record) and §6 (visible decision register) without creating a second source of truth.

### 3.1 Row schema

| ID family | Meaning | Required fields |
|---|---|---|
| `C-nn` | Conflict: two or more sources state incompatible things | Concern; each competing statement with file + section; impact (owning crate/app/phase); decision owner; resolution gate (which P0.x part settles it); status |
| `D-nn` | Decision: no approved source settles it | Question; why it matters; options with evidence for and against; recommended option (labelled **recommendation**, never **accepted**); decision owner; blocked phases/parts; due condition; status |
| `V-nn` | External/version-sensitive claim needing verification | Claim as written; source document; official source consulted; date checked; verdict (current / stale / unverified); does not itself authorize a change |
| `G-nn` | Gap: missing file, broken reference or unverifiable claim | Expected path; observed state; evidence of the check; owner; disposition |

### 3.2 Expected seed contents

These are the conflicts and decisions already evidenced in Stage 02/03. P0.1 must confirm each against the current files and may add or reject rows — it must not assume this list is complete.

| Seed | Expected row | Evidence already recorded |
|---|---|---|
| Crate inventory disagreement | `C-01`, `D-01` | `system_architecture_design.md` target library list vs the Phase 1 candidate list (Stage 02 §4, Stage 03 F1) |
| `sitolo-api` vs `apps/api`; `sitolo-jobs` vs `apps/worker` | `C-02`, `D-02`, `D-03` | Stage 03 F6 |
| Migration root vs persistence ownership | `C-03`, `D-04` | Stage 03 F6 |
| Tauri workspace membership vs separate lockfile | `C-04`, `D-05` | Stage 02 §2, Stage 03 F5 |
| Canonical machine-readable API schema and generation command | `D-06` | Stage 04 plan open decision 6 |
| React Native tooling, local persistence, E2E harness (framework itself is **settled** by ADR-026) | `D-07` | Stage 03 F8, Stage 02 mobile section |
| Separate admin-web existence | `D-08` | Stage 04 plan open decision 5 |
| `sitolo-cli` and `sitolo-testkit` disposition | `D-09`, `D-10` | Stage 04 plan recommendation, Stage 03 F6 |
| `first_required_phase` / lifecycle vocabulary for the future manifest | `D-11` | `docs/phase_part_execution_contract.md` §6 |
| Rust pin 1.98.1 vs 1.99.1 | `D-12`, `V-01` | Stage 02 §5, Stage 03 F4 |
| Root `tests/` boundary and owner-local test placement | `C-05` or confirm no conflict | `docs/testing_strategy.md` vs `docs/phase_part_execution_contract.md` §3.4–3.5 |
| Test/CI claims that cannot run yet | `G-01`, `G-02` | Stage 03 F7 — no checker, no CI evidence |

Rows whose subject is settled by an accepted ADR (React Native per ADR-026) are recorded as **settled**, not reopened.

## 4. Exact write set

Five paths. Nothing else may change in P0.1.

| # | Path | Action | Bounded scope |
|---|---|---|---|
| 1 | `docs/phase0_conflict_and_decision_register.md` | **Create** | The deliverable in §3. Register only — no architecture narrative. |
| 2 | `workspaces/sitolo-engineering/stages/05-implement/output/phase0-ownership-reconciliation-p01-implementation.md` | **Create** | Stage 05 implementation report: files changed, rows added, checks run, limitations. |
| 3 | `docs/README.md` | **Modify** | One row in *Governing / current-state documents* indexing the register. No other edit. |
| 4 | `docs/phase0_architecture_contract_freeze.md` | **Modify** | §6 gains a pointer to the register as the decision home; §4 artifact 5 gains a one-clause note that the register is the reconciliation record. No criterion is changed or ticked. |
| 5 | `workspaces/sitolo-engineering/stages/04-plan/output/phase0-ownership-reconciliation-plan.md` | **Modify** | Frontmatter status plus the P0.1 row's state cell and one link to this proposal. The approved sequence text is left intact. |

### 4.1 Read-only in P0.1 (must not be edited)

`docs/system_architecture_design.md`, `docs/implementation_plan.md`, `docs/phase1_repository_rust_workspace_ci_deep_implementation.md`, `docs/ADR-001-025.md`, `docs/ADR-026-react-native-mobile.md`, `docs/api_contract.md`, `docs/database_design.md`, `docs/testing_strategy.md`, `docs/ci_enforcement.md`, `docs/code_ownership_map.md`, `docs/phase_contract_coverage_register.md`, plus the Stage 02/03 outputs.

**Why this rule matters:** these files are the *subject* of the register. Editing one while writing the register would apply an undecided decision and destroy the evidence the register exists to preserve. `docs/code_ownership_map.md` in particular is a Phase 0 proposal; P0.1 must not ratify it by editing it.

### 4.2 Explicit exclusions

- No Cargo workspace, `Cargo.toml`, crate, app, package, migration, CI script or checker.
- No new ADR; the register may name an ADR that is *required*, but only the ADR process records an accepted decision.
- No resolution or status upgrade of any `C-`/`D-` row beyond **open**, **recommendation** or **proposed**.
- No tick of any Phase 0 exit criterion.
- No change to `docs/current_implementation_status.md` or the phase-context router — status pointers move at Stage 09 delivery.
- No toolchain pin change, no mobile tooling selection, no package inventory decision.

## 5. Requirement-to-artifact mapping

| P0.1 requirement (Stage 04 plan) | Register rows that satisfy it | Acceptance evidence |
|---|---|---|
| Every material conflict cites exact evidence | `C-nn` with file + section per statement | Spot-check each row against the named section |
| Conflicts are resolved *or* explicitly open with owner and gate | `C-nn` status + owner + resolution gate column | No row with a blank owner or gate |
| Version-sensitive claims checked on official sources | `V-nn` + date checked | Source URL and date present; no claim of a compatibility review that did not run |
| Ownership gaps visible, not absorbed | `G-nn` | Each gap names the part/phase that must close it |
| Nothing pre-decided | Whole-file review | No row marked accepted; no canonical input edited |

## 6. Verification for this part

Documentation-only, so no Cargo command is applicable and none may be claimed.

| Check | Method | Reporting |
|---|---|---|
| Write-set containment | `git diff --name-only` compared to §4 | PASS/FAIL — any extra path is a FAIL |
| Reference integrity | Resolve every relative Markdown link added in this part | PASS/FAIL per link |
| No duplicated specification | Review: every claim in the register is a citation, not new design text | PASS/FAIL |
| No premature decision | Grep the register for `accepted`, `approved`, `resolved` outside the ADR-026 settled row | PASS/FAIL |
| Seed rows evidenced | Each §3.2 seed either appears with a citation or is recorded as rejected with a reason | PASS/FAIL |
| Build/test claims | Not applicable — no workspace exists | **NOT RUN / N-A**, stated explicitly |

`scripts/ci/verify` and `scripts/ci/check-code-ownership` do not exist at this revision (Stage 03 F7). P0.1 must not cite them as passing, and must not create them — that is a Phase 1 deliverable.

## 7. Stage route after this part

`05 Implement → 06 Audit → (07 Remediate only if findings) → 08 Verify → 09 Deliver`

Stage 06 is required even though the diff is documentation: the audit question is not "do the docs build" but "did any row smuggle an undecided decision into the record." Skipping it would let a pre-decided architecture claim reach P0.2 as if it were evidence.

Completing P0.1 does not complete Phase 0 and does not unblock Phase 1.

## 8. Stop condition

P0.1 is complete when all of the following are true, and stops immediately if any is violated:

**Complete when**
1. The register exists at the §4 path with `C-`, `D-`, `V-`, `G-` rows and the §3.1 fields populated.
2. Every seed in §3.2 is either evidenced or rejected with a written reason.
3. Every decided item is labelled **recommendation** or **proposed**; nothing is marked accepted.
4. `docs/README.md` indexes the register and `docs/phase0_architecture_contract_freeze.md` points to it.
5. The Stage 05 implementation report records the exact checks run and their results.
6. `git diff --name-only` matches §4 exactly.

**Return to Stage 04 and stop, without editing, if**
- a required change falls outside §4, or a canonical input from §4.1 appears to need editing;
- a conflict cannot be recorded without choosing a winner;
- Stage 02/03 evidence is contradicted by the current files;
- a row needs product, commercial or legal authority that has not been obtained;
- the register starts accumulating architecture explanation instead of citations.

Re-planning is a correct outcome. Broadening the diff to "finish" P0.1 is not.

## 9. Risks and mitigations

| Risk | Consequence | Mitigation |
|---|---|---|
| Register drifts into a second architecture document | Two competing sources; exactly the failure Phase 0 exists to fix | Citation-only rule; §6 "no duplicated specification" check; P0.7 owns canonical edits |
| P0.1 silently settles inventory or lockfile questions | P0.2–P0.6 become rubber stamps; Phase 1 scaffolds an unratified map | §4.1 read-only inputs; §6 premature-decision grep; Stage 06 audit |
| Empty speculative crates appear "to satisfy the register" | Violates `docs/phase_part_execution_contract.md` §3.3 | §4.2 exclusion; no workspace exists to scaffold anyway |
| Version claims are copied forward unchecked | Stale pin or tooling becomes a frozen "fact" | `V-nn` requires official source + date; no compatibility claim without a run |
| Human acceptance is assumed from silence | Implementation proceeds on an unapproved write set | §11 requires an explicit dated acceptance |

## 10. Decisions requested

| # | Decision | Recommendation | Alternative |
|---|---|---|---|
| A | Deliverable path and name | `docs/phase0_conflict_and_decision_register.md` | `docs/phase0_conflicts_and_decisions.md`; or keep it as a run artifact under `stages/05-implement/output/` (rejected: not discoverable to later phases) |
| B | Write set of five paths in §4 | Accept as the boundary | Add/remove a path now; after acceptance any addition returns to Stage 04 |
| C | Read-only rule for the ten canonical inputs in §4.1 | Accept — resolution happens in P0.2–P0.7 | Allow targeted edits, which must then name the part that owns each decision |
| D | Stop condition and post-part route in §7–§8 | Accept, including mandatory Stage 06 audit | Accept with audit waived for documentation-only parts (not recommended) |
| E | Register may name a required ADR without writing it | Accept | Permit draft ADR text inside the register (not recommended — conflates record and decision) |

## 11. Acceptance record

| Field | Value |
|---|---|
| Accepted by | *(pending)* |
| Accepted on | *(pending)* |
| Decisions accepted | *(A–E, or the edited subset)* |
| Edits requested to this proposal | *(pending)* |
| Resulting part status | Proposed → **not yet accepted** |

Until this table carries a human name and date, P0.1 is proposed only, Stage 05 must not start, and Phase 1 remains blocked.
