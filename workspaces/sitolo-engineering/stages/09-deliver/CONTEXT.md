# 09 Deliver

One job: prepare the verified change for review and controlled integration.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|
| Working | Run brief | ../01-select/output/[run-slug]-brief.md | Full file | Scope |
| Working | Plan | ../04-plan/output/[run-slug]-plan.md | Full file | Intended result |
| Working | Implementation | ../05-implement/output/[run-slug]-implementation.md | Full file | Change summary |
| Working | Audit | ../06-audit/output/[run-slug]-audit.md | Full file | Findings status |
| Working | Verification | ../08-verify/output/[run-slug]-verification.md | Full file | Evidence |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Git and review rules | Delivery requirements |
| Reference | Delivery guide | references/delivery.md | Full file | Handoff format |
| Reference | Current phase status | ../../../../docs/current_implementation_status.md | Full file | Reconcile phase status without assuming completion |
| Reference | Phase execution guide | ../../../../docs/phase_execution_guide.md | Selected phase exit and next-phase rule | Ensure the handoff directs the next allowed phase |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Process

1. Confirm verification is complete and no material audit finding remains open.
2. Inspect the final diff for scope, secrets, generated artifacts, and unrelated changes.
3. Prepare a factual commit/PR summary with tests and known limitations.
4. Commit and open the review request only through the project's normal Git workflow.
5. Record the selected product phase, canonical contract, exit-gate decision, revision-attributable evidence and explicit next phase/stop condition in the handoff.
6. Update current phase status only when the required phase exit decision has actually been accepted. A completed ICM run, passing checks or merged PR is not by itself a phase transition.
7. Do not merge merely because the stage is complete.

## Human Check

Review the final diff and delivery summary. Human approval remains the final integration decision.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Delivery handoff | output/[run-slug]-delivery.md | Markdown |
