# 03 Investigate

One job: understand the existing implementation and authoritative project contracts before design.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|
| Working | Run brief | ../01-select/output/[run-slug]-brief.md | Full file | Scope |
| Working | Research | ../02-research/output/[run-slug]-research.md | Full file when produced | External constraints |
| Reference | Project docs map | ../../../../docs/README.md | Relevant sections | Locate authoritative documents |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Relevant sections | Engineering policy |
| Reference | Context policy | ../../shared/context-loading.md | Full file | Loading discipline |
| Reference | Investigation guide | references/investigation.md | Full file | Evidence checklist |
| Reference | Code ownership map | ../../../../docs/code_ownership_map.md | Relevant rows | Compare actual files/packages with intended owners; distinguish current files from proposals |
| Reference | Phase parts | ../../../../docs/phase_part_execution_contract.md | Investigation requirements | Inventory missing, duplicate and misplaced package/app/test responsibilities |
| Reference | Security context | ../../shared/security-context/CONTEXT.md | Conditional when security_relevant=yes | Threats, controls, tests and evidence |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Process

1. Read the run brief security applicability. If `security_relevant: yes`, load the security context and applicable control IDs before assessing the implementation.
2. Inspect the affected modules, types, repositories, migrations, tests, and workflows.
3. Inventory the actual repository tree and existing package/app manifests. For each relevant planned owner, record what exists, what is missing, where behavior is actually implemented, and where its tests reside. Treat docs/examples as design material, not runtime source.
4. Compare observed files and package boundaries with `docs/code_ownership_map.md` and the accepted phase-part plan; identify omitted responsibilities, duplicate business logic, wrong-owner code/tests and unexpected dependency edges. Mark any unratified target-map row as a proposal, not a defect against an approved architecture.
5. Identify the current source of truth, ownership, trust boundaries, transaction boundaries, and failure modes.
6. Read only the applicable project documents routed by docs/README.md.
7. Search for existing abstractions before proposing new ones.
8. For security-relevant work, record control → implementation → test/evidence gaps and residual risk.
9. Record contradictions, gaps, and reusable implementation patterns.
10. Write findings to output/.

## Human Check

Review the findings and evidence. Edit or correct them before planning.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Investigation findings | output/[run-slug]-investigation.md | Markdown |
