# Sitolo — Current Implementation Status

**Status:** Restart baseline  
**Current product phase:** Phase 0 — Architecture, Contracts and ADR Freeze  
**Phase lifecycle:** IN_PROGRESS (restart/reconciliation work); Phase 0 exit gate remains open  
**Current ICM stage:** Start a fresh Stage 01 Select run  
**Last status reset:** 2026-10-21  
**Repository baseline:** The commit/PR that records this reset; update the exact SHA after merge.

## Active work

Sitolo's implementation program starts at **Phase 0** in this repository. The current objective is to reconcile the copied project corpus, settle/record material architecture and contract decisions, confirm phase dependencies, and establish a reviewed Phase 0 exit package.

This status is an explicit restart baseline. Existing files, earlier phase contracts, code, PR-specific notes, test claims or run artifacts do not by themselves prove that any phase is complete in this repository. Claims require current, revision-attributable evidence and the required human gate.

## Authoritative phase routing

1. [Phase 0 contract](phase0_architecture_contract_freeze.md) — current active phase contract.
2. [Phase execution and transition guide](phase_execution_guide.md) — purpose, inputs, expected evidence, exit gate and next phase for every Phase 0–20.
3. [Implementation plan](implementation_plan.md) — canonical phase order and program dependencies.
4. [Phase contract coverage register](phase_contract_coverage_register.md) — contract coverage and reconciliation; does not replace product or technical authority.
5. [Engineering phase-context router](../workspaces/sitolo-engineering/shared/phase-context/CONTEXT.md) — ICM reference routing.

## Phase 0 exit gate

Phase 0 remains open until the responsible human accepts the evidence package described in the Phase 0 contract. Outstanding conflicts, unapproved decisions, missing references and checks that could not run must remain visible. Do not begin Phase 1 as active implementation work until the Phase 0 exit conditions are met.

## ICM execution position

The ICM execution pipeline remains independent of product phase numbering:

```text
01-select → 02-research → 03-investigate → 04-plan
→ 05-implement → 06-audit → 07-remediate → 08-verify → 09-deliver
```

Begin a **new** Stage 01 Select run for Phase 0. Do not resume a previous phase-specific run by default. Prior run folders are historical records unless a human explicitly selects them as supporting evidence.

## Evidence rule

A document contract describes requirements. A status entry describes the selected state. Only current source, reproducible checks, review and revision-attributable evidence support implementation/verification claims. If a required command or referenced script is absent, mark it **not run / blocked**, record the exact path, and address it within the owning phase.
