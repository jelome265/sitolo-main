# 04 Plan

One job: turn selected requirements and investigation evidence into an implementation contract.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|
| Working | Run brief | ../01-select/output/[run-slug]-brief.md | Full file | Approved brief |
| Working | Research | ../02-research/output/[run-slug]-research.md | Full file | Research memo |
| Working | Investigation | ../03-investigate/output/[run-slug]-investigation.md | Full file | Existing-state evidence |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Relevant definitions of done and rules | Non-negotiable constraints |
| Reference | Project docs | ../../../../docs/README.md | Documents identified by investigation | Design authority |
| Reference | Plan guide | references/plan-contract.md | Full file | Plan structure |
| Reference | Phase parts | ../../../../docs/phase_part_execution_contract.md | Full file | Joint part breakdown and stage gates |
| Reference | Code ownership map | ../../../../docs/code_ownership_map.md | Relevant sections | Owner paths and test placement; unresolved package inventory blocks Phase 1 scaffolding |
| Reference | Security context | ../../shared/security-context/CONTEXT.md | Conditional when security_relevant=yes | Applicable controls and security proof |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Process

1. Read the run brief security applicability. If `security_relevant: yes`, carry the selected control IDs into the plan and load the applicable security context.
2. Define affected modules, contracts, ownership, invariants, and state transitions.
3. Define API, database, migration, transaction, concurrency, retry, audit, observability, and rollback impact where applicable.
4. Define tests, including negative/security and failure-mode tests.
5. Identify exact files or bounded areas to change.
6. Propose or update the full phase-part sequence and identify the current part; clearly separate proposed from human-approved status.
7. Build a requirement/control → owning crate/app/path → exact implementation files → exact test files in their owner → commands/evidence matrix. Account for every relevant approved package as affected or `not touched` with a reason.
8. Confirm the current part's write set is explicit and feasible. If ownership is unratified or conflicting source documents leave the owner ambiguous, record the blocker instead of choosing silently.
9. For security-relevant work, include control ID → requirement/threat → implementation change → negative test/evidence → CI/release gate → residual risk/exception.
10. Record unresolved decisions without inventing answers.
11. Audit the plan for scope creep, missing proof, and authority conflicts.
12. Write the implementation plan to output/.

## Human Check

Review the plan, proposed part sequence and current-part write set before implementation. The human must explicitly accept them; a draft plan or agent recommendation is not approval. Edit the plan in place.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Implementation plan | output/[run-slug]-plan.md | Markdown |
