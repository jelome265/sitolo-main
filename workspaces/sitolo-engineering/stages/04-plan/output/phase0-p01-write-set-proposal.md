---
artifact_type: implementation-plan-amendment
status: accepted-scope-only
run_slug: "phase0-ownership-reconciliation-20261021"
icm_stage: "04-plan"
product_phase: "Phase 0 — Architecture, Contracts and ADR Freeze"
current_part: "P0.1 — Source of truth and conflict register"
part_sequence_status: "sequence human-approved 2026-10-21; P0.1 write set accepted 2026-10-21 — scope only"
security_relevant: "yes"
---

# P0.1 — Source of Truth and Conflict Register: Accepted Write Set

## 1. Purpose of this document

Stage 04 produced an approved P0.1–P0.8 ordering but left P0.1 at *"Sequence approved; part not started"*, with the exact write set and stop condition unrecorded. `docs/phase_part_execution_contract.md` §2 requires that record before Stage 05 begins.

This amendment supplies it. It is a plan, not an approval of any decision inside it: it does not resolve an architecture question, does not move Phase 0 toward closure and does not authorize product code. The product owner accepted the scope boundary on 2026-10-21 (§11), which authorizes Stage 05 to write the register and nothing beyond it.

## 2. Part control

| Field | Value |
|---|---|
| Product phase | Phase 0 — Architecture, Contracts and ADR Freeze (`docs/phase0_architecture_contract_freeze.md`) |
| Part ID | **P0.1** |
| Part outcome | One conflict/decision register with cited evidence and an explicit resolution gate per row |
| Predecessor gate | P0.1–P0.8 sequence approved 2026-10-21. No Stage 05 has run for this part. |
| Work nature | Documentation reconciliation only. No product code, workspace or client artifact. |
| Approval required from | Product owner (architecture/ownership decisions); engineering owner where a row is purely mechanical |
| Approval status | **Accepted 2026-10-21 — P0.1 scope only** (§11). No row inside the register is thereby decided. |

## 3. Deliverable

**`docs/phase0_conflict_and_decision_register.md`** — a citation-only register. It records *that* sources disagree and *who* must decide; it does not restate architecture, and it adds no new design prose. Every row points at an existing authoritative document and section.

This satisfies Phase 0 contract §4 artifact 5 (architecture/contract reconciliation record) and §6 (visible decision register) without creating a second source of truth.

### 3.1 Row schema

| ID family | Meaning | Required fields |
|---|---|---|
| `C-nn` | Conflict: two or more sources state incompatible things | Concern; each competing statement with file + section; impact (owning crate/app/phase); decision owner; resolution gate (which P0.x part settles it); status |
| `D-nn` | Decision: no approved source settles it | Question; why it matters; options with evidence for and against; recommended option (labelled **recommendation**, never **accepted**); decision owner; blocked phases/parts; due condition; status |
| `V-nn` | External/version-sensitive claim needing verification | Claim as written; source document; official source consulted; date checked; verdict (current / stale / unverified); accountable owner; resolution gate; status (the verification result and the decision status are recorded separately); does not itself authorize a change |
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

### 3.3 Part conditions (accepted 2026-10-21)

1. The register is an **evidence and routing artifact**, not a second architecture specification. It carries citations and disposition status, never new design prose.
2. Every conflict, open decision, version claim or gap carries precise source evidence, an accountable owner, a resolution gate and a status.
3. Every seed in §3.2 is either evidenced or **rejected as a genuine conflict, with the reason written down**. A seed cannot disappear silently.
4. For the Rust version discrepancy, P0.1 records and re-evidences the competing claims. It does not choose the toolchain pin; that is a later compatibility decision.
5. The same restraint applies to crate inventories, API schema authority, migration placement, Tauri workspace/lockfile boundaries and client tooling: describe the disagreement, name the decider, do not settle it.

## 4. Exact write set

Five paths. Nothing else may change in P0.1.

**Path convention:** every path below is relative to the **repository root**, not to this ICM workspace. Both workspace paths live under `workspaces/sitolo-engineering/stages/<stage>/output/` and must be written in full; a path resolved against `workspaces/sitolo-engineering/` produces a different, wrong location. A wrong base is a write-set violation, not a typo.

**Path verification, 2026-10-21, before acceptance:** `git rev-parse --show-toplevel` returned the repository root. Rows 3, 4 and 5 exist as files. Rows 1 and 2 do not exist and both parent directories are present — the expected state for a create.

| # | Path (repository-root-relative) | Action | Bounded scope |
|---|---|---|---|
| 1 | `docs/phase0_conflict_and_decision_register.md` | **Create** | The deliverable in §3. Register only — no architecture narrative. |
| 2 | `workspaces/sitolo-engineering/stages/05-implement/output/phase0-ownership-reconciliation-p01-implementation.md` | **Create** | Stage 05 implementation report: files changed, rows added, checks run, limitations. |
| 3 | `docs/README.md` | **Modify** | One row in *Governing / current-state documents* indexing the register. No other edit. |
| 4 | `docs/phase0_architecture_contract_freeze.md` | **Modify** | §6 gains a pointer to the register as the decision home; §4 artifact 5 gains a one-clause note that the register is the reconciliation record. No criterion is changed or ticked. |
| 5 | `workspaces/sitolo-engineering/stages/04-plan/output/phase0-ownership-reconciliation-plan.md` | **Modify** | Frontmatter status plus the P0.1 row's state cell and one link to this proposal. The approved sequence text is left intact. |

### 4.1 Read-only in P0.1 (must not be edited)

The nine accepted architecture and contract inputs:

| # | Path | Why it is evidence, not a write target |
|---|---|---|
| 1 | `docs/system_architecture_design.md` | Candidate inventory under dispute |
| 2 | `docs/implementation_plan.md` | Phase order and toolchain statement under review |
| 3 | `docs/phase1_repository_rust_workspace_ci_deep_implementation.md` | Competing candidate inventory and workspace rules |
| 4 | `docs/ADR-001-025.md` | Accepted-decision record; historical Flutter text is not reopened |
| 5 | `docs/api_contract.md` | API authority assertion |
| 6 | `docs/database_design.md` | Migration and persistence authority assertion |
| 7 | `docs/testing_strategy.md` | Test-placement assertions under comparison |
| 8 | `docs/ci_enforcement.md` | Gate requirements that cannot yet run |
| 9 | `docs/code_ownership_map.md` | Phase 0 proposal; P0.1 must not ratify it by editing it |

`docs/ADR-026-react-native-mobile.md`, `docs/phase_contract_coverage_register.md` and the Stage 02/03 run artifacts are likewise outside the write set. ADR-026 is settled authority and is not reopened by this part.

**Why this rule matters:** these files are the *subject* of the register. Editing one while writing the register would apply an undecided decision and destroy the evidence the register exists to preserve.

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
| Write-set containment | `git diff --name-only` compared against the five repository-root-relative paths in §4 | PASS/FAIL — any added, missing or wrongly-rooted path is a FAIL |
| Reference integrity | Resolve every relative Markdown link added in this part | PASS/FAIL per link |
| No duplicated specification | Review: every claim in the register is a citation, not new design text | PASS/FAIL |
| No premature decision | Grep the register for `accepted`, `approved`, `resolved` outside the ADR-026 settled row | PASS/FAIL |
| Seed rows evidenced | Each §3.2 seed either appears with a citation or is recorded as rejected with a reason | PASS/FAIL |
| Build/test claims | Not applicable — no workspace exists | **NOT RUN / N-A**, stated explicitly |

`scripts/ci/verify` and `scripts/ci/check-code-ownership` do not exist at this revision (Stage 03 F7). P0.1 must not cite them as passing, and must not create them — that is a Phase 1 deliverable.

## 7. Stage route after this part

`05 Implement → 06 Audit → (07 Remediate only if findings) → 08 Verify → 09 Deliver`

Stage 06 is required even though the diff is documentation. It audits two things independently and reports them separately:

1. **Evidence quality** — each cited source actually says what the row claims, at the section named, and the row does not paraphrase it into a stronger claim.
2. **Decision containment** — no unresolved decision was silently converted into an accepted one, and no canonical input was edited to match the register instead of the register recording the input.

Skipping the audit would let a pre-decided architecture claim reach P0.2 as if it were evidence.

Completing P0.1 does not complete Phase 0 and does not unblock Phase 1.

## 8. Stop condition

P0.1 is complete when all of the following are true, and stops immediately if any is violated:

**Complete when**
1. The register exists at the §4 path with `C-`, `D-`, `V-`, `G-` rows and the §3.1 fields populated.
2. Every seed in §3.2 is either evidenced or rejected with a written reason.
3. Every decided item is labelled **recommendation** or **proposed**; nothing is marked accepted.
4. `docs/README.md` indexes the register and `docs/phase0_architecture_contract_freeze.md` points to it.
5. The Stage 05 implementation report records the exact checks run and their results.
6. `git diff --name-only` lists exactly the five repository-root-relative paths in §4 — no additions, no omissions, no path resolved against the ICM workspace instead of the repository root.

**Stop and return to Stage 04 before editing, if**
- the work requires a change outside the accepted write set — return to Stage 04, revise this proposal and obtain acceptance **before making that change**;
- a canonical input from §4.1 appears to need editing;
- a conflict cannot be recorded without choosing a winner;
- Stage 02/03 evidence is contradicted by the current files;
- a row needs product, commercial or legal authority that has not been obtained;
- the register starts accumulating architecture explanation instead of citations.

**After Stage 05, any unexpected path in the diff also blocks completion.** An out-of-boundary change is not repaired by deleting the file afterwards or by describing it in the report as incidental: the diff is the evidence, and a diff that does not match §4 fails the part.

Re-planning is a correct outcome. Broadening the diff to "finish" P0.1 is not.

## 9. Risks and mitigations

| Risk | Consequence | Mitigation |
|---|---|---|
| Register drifts into a second architecture document | Two competing sources; exactly the failure Phase 0 exists to fix | Citation-only rule; §6 "no duplicated specification" check; P0.7 owns canonical edits |
| P0.1 silently settles inventory or lockfile questions | P0.2–P0.6 become rubber stamps; Phase 1 scaffolds an unratified map | §4.1 read-only inputs; §6 premature-decision grep; Stage 06 audit |
| Empty speculative crates appear "to satisfy the register" | Violates `docs/phase_part_execution_contract.md` §3.3 | §4.2 exclusion; no workspace exists to scaffold anyway |
| Version claims are copied forward unchecked | Stale pin or tooling becomes a frozen "fact" | `V-nn` requires official source + date; no compatibility claim without a run |
| Human acceptance is assumed from silence | Implementation proceeds on an unapproved write set | §11 records an explicit dated acceptance; a later part cannot inherit it |

## 10. Decisions recorded (2026-10-21)

| # | Decision | Verdict | Direction recorded |
|---|---|---|---|
| A | Deliverable path and name | **Accepted** | `docs/phase0_conflict_and_decision_register.md` |
| B | Five-path write set in §4 | **Revised, then accepted** | Paths must be repository-root-relative and match the real ICM layout. §4 now states the convention and records the pre-acceptance path check. |
| C | Read-only rule for the nine canonical inputs in §4.1 | **Accepted** | All nine architecture and contract inputs stay untouched during P0.1. |
| D | Stop condition and mandatory Stage 06 audit | **Accepted with amendment** | Outside-boundary work stops and returns to Stage 04 for a revised proposal *before* the edit; after Stage 05 an unexpected diff path also blocks completion; Stage 06 audits evidence quality and decision containment independently. |
| E | ADR references only | **Accepted** | The register may identify an ADR a later part must create or amend; P0.1 never writes its decision text. |

## 11. Acceptance record

| Field | Value |
|---|---|
| Accepted by | Product owner |
| Accepted on | 2026-10-21 |
| Decisions accepted | A, C, E accepted as proposed; B revised (repository-root-relative paths) then accepted; D accepted with tightened stop condition and post-Stage-05 diff rule |
| Edits requested to this proposal | §4 path convention and verification; §8 stop wording; §3.3 part conditions; §7 Stage 06 dual audit scope |
| Resulting part status | **Accepted — P0.1 scope only** |

**Scope boundary of this acceptance.** It authorizes Stage 05 to produce the register within the five paths above. It does not approve, resolve or pre-dispose any `C-`, `D-`, `V-` or `G-` row; the crate inventory, API schema authority, migration placement, Tauri workspace/lockfile boundary, client tooling and Rust pin all remain open. It does not begin P0.2, does not close Phase 0, and does not unblock Phase 1 scaffolding. Each later part repeats this gate for its own write set.
