# 01 Select

One job: turn the user request into a bounded engineering run.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|
| Working | User request | current task | Full request | Defines desired change |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Sections 0-3 and relevant definitions | Engineering constraints |
| Reference | Context policy | ../../shared/context-loading.md | Full file | Loading discipline |
| Reference | Request guide | references/request-brief.md | Full file | Output shape |
| Reference | Current phase status | ../../../../docs/current_implementation_status.md | Full file | Select the authorized active product phase and phase gate |
| Reference | Phase execution guide | ../../../../docs/phase_execution_guide.md | Selected phase entry and its next-phase rule | Define the phase contract, expected evidence and transition |
| Reference | Phase router | ../../shared/phase-context/CONTEXT.md | Active phase section and canonical contract index | Route the smallest phase-specific context |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Process

1. Classify the request as feature, bug, security, migration, refactor, documentation, or review.
2. Identify the affected capability and likely bounded context.
3. Record requirements, explicit exclusions, open decisions, and success conditions.
4. Determine `security_relevant: yes|no` from affected trust boundaries and control families. Record the rationale and candidate control IDs in the run brief.
5. If security is relevant, route through the security context; do not guess or silently defer applicability.
6. Always route through Stage 02. That stage records whether external research is required.
7. Read current phase status and the phase execution guide. Record the selected product phase, canonical contract, predecessor-gate state, this run's exit criteria, and the next phase. A user request to work on a later phase does not override an unmet prerequisite; record the blocker and route back to the owning phase.
8. Keep product-phase state separate from the ICM stage number. Do not resume a prior phase-specific run unless the human explicitly selects it as relevant historical evidence.
9. Write the run brief to output/.

## Human Check

Review the brief for scope and exclusions. Edit it in place. Do not let later stages invent missing scope or silently change the security applicability decision. A security-relevant run cannot pass planning without a control mapping.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Run brief | output/[run-slug]-brief.md | Markdown |
