# 05 Implement

One job: implement the approved plan without broadening scope.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|---|
| Working | Implementation plan | ../04-plan/output/[run-slug]-plan.md | Full file | Exact change contract |
| Working | Investigation | ../03-investigate/output/[run-slug]-investigation.md | Full file | Existing-state constraints |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Relevant sections | Engineering rules |
| Reference | Project docs | ../../../../docs/README.md | Documents named by plan | Detailed contracts |
| Reference | Context policy | ../../shared/context-loading.md | Full file | Loading discipline |
| Reference | Implementation guide | references/implementation.md | Full file | Quality constraints |
| Reference | Phase parts | ../../../../docs/phase_part_execution_contract.md | Current-part rules | Only the human-approved write set may be implemented |
| Reference | Code ownership map | ../../../../docs/code_ownership_map.md | Relevant owner/test rows | Keep behavior and tests in their approved crate/app; map awaits Phase 0 acceptance |
| Reference | Security context | ../../shared/security-context/CONTEXT.md | Conditional when security_relevant=yes | Control implementation and required proof |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Process

1. Confirm the current part sequence, write set and explicit human acceptance are recorded; if not, stop before changing product implementation.
2. Inspect the approved write set and actual package/app tree again before editing.
3. Implement each responsibility only in its approved owner. Do not compensate for a missing crate/file by placing its behavior in a neighboring crate, app entry point, Tauri command or React component.
4. Add or update tests in the crate/package/app that owns each behavior; use root `tests/` only for genuine cross-package/system assertions. Do not claim a behavior is implemented if its required owner or test was skipped.
5. Update migrations, API contracts, observability, or documentation only where the approved plan requires them.
6. Run targeted checks for each changed package/app and record exact commands/results; stop and return to Stage 04 if ownership or scope must change.
7. Audit the diff against the approved file write set, ownership map, and security/integrity controls, including each selected security control ID.
8. Write an implementation report naming affected packages and relevant packages not touched, with reasons.
9. Route every implementation-changing run to Stage 06 Audit next. A green targeted check or CI result never waives the semantic audit; CI is evidence, not a substitute for independent audit.

## Human Check

Review the code diff and implementation report. Do not move to audit while material scope deviations remain unexplained.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Implementation report | output/[run-slug]-implementation.md | Markdown |
