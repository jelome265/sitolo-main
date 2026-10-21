# Sitolo - ICM Root Router

Sitolo uses filesystem-routed ICM workspaces. The root router is an entry gate: it sends organizational work to the workspace router before any domain or engineering stage is selected.

## Routing

| Task | Go To | Description |
|---|---|---|
| Any organizational work | workspaces/CLAUDE.md | Mandatory first stop; select the smallest bounded work domain |
| Repository change impact | map/CLAUDE.md | System Map routing after primary domain selection, or for an explicit map-only request |
| Canonical project knowledge | docs/README.md | Routes to authoritative project knowledge |
| Workflow integrity | docs/agentic_workflow.md; docs/phase_execution_guide.md | ICM policy and phase routing; mechanical checks are a Phase 1 deliverable and must not be claimed before they exist |
| Code and test ownership | docs/code_ownership_map.md; docs/phase_part_execution_contract.md | Map the approved package/client owner, exact write set and test locations before each part; respect the Phase 0 approval gate and do not silently move behavior between crates |

## Loading

Read this file, then workspaces/CLAUDE.md for organizational work. Do not classify work as Engineering merely because implementation, code, repository, or infrastructure is involved. After the primary domain is selected, follow that workspace's CLAUDE.md and CONTEXT.md; only then follow explicit cross-domain or System Map routes.

## Authority

agent.md remains the detailed engineering governance contract. docs/README.md and the named canonical documents remain authoritative for project knowledge. Workspace files are routing/control surfaces, not competing sources of truth.
