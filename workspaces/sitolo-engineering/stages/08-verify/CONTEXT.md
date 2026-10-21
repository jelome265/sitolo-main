# 08 Verify

One job: produce reproducible evidence that the approved implementation and re-audit satisfy the required gates.

## Inputs

| Kind | Source | File/Location | Section/Scope | Why |
|---|---|---|---|
| Working | Latest audit | ../06-audit/output/[run-slug]-audit.md | Full file | Required findings state |
| Working | Remediation | ../07-remediate/output/[run-slug]-remediation.md | Full file when remediation occurred | Corrections and proof |
| Working | Implementation | ../05-implement/output/[run-slug]-implementation.md | Full file | Claimed change |
| Reference | Governance | ../../shared/governance-context/CONTEXT.md | Definitions of done | Verification requirements |
| Reference | Project docs | ../../../../docs/README.md | Applicable testing/deployment docs | Required evidence |
| Reference | Phase parts | ../../../../docs/phase_part_execution_contract.md | Part completion gates | Verify the accepted part, not an assumed larger scope |
| Reference | Code ownership map | ../../../../docs/code_ownership_map.md | Affected package/app/test rows | Report per-owner package and client results |
| Reference | Verification guide | references/verification.md | Full file | Gate selection |
| Reference | Security control register | ../../../../docs/security_control_register.md | Applicable control IDs | Security proof and release status |
| Reference | Security context | ../../shared/security-context/CONTEXT.md | Conditional when security_relevant=yes | Security verification and current standards |

**Do NOT load:** unrelated stage contracts, unrelated reference families, the full documentation corpus, or the full `agent.md`; use the routed governance context and exact inputs only.

## Package-level and client-level verification

Apply `../../../../docs/phase_part_execution_contract.md` and the relevant rows in `../../../../docs/code_ownership_map.md`. Report each affected package/app's required commands and outcomes separately. Distinguish `PASS`, `FAIL`, `BLOCKED`, `NOT RUN` and `N/A`; a zero-test or undiscovered test target is not evidence of behavior. A green workspace command cannot hide an unbuilt crate, unexecuted package tests, or a skipped React/Tauri or Flutter client. Use root tests only for genuine cross-owner behavior.

## Process

1. Read security applicability and control IDs from the latest plan/audit. If `security_relevant: yes`, load the security context and control register before selecting gates.
2. Determine required targeted checks from the plan and audit.
3. Check whether `./scripts/ci/verify` exists at this revision. If it exists, run it and record the exact result; if it is absent, mark the aggregate gate `BLOCKED / NOT RUN` and record the missing path. Never claim a missing script passed.
4. Run additional integration/security/contract tests required by the change.
5. For security-relevant work, record control ID → test/gate → result → evidence reference → limitation.
6. Record exact commands, results, environment limitations, and unverified claims.
7. Confirm that the latest audit has no unresolved material findings and no applicable security control remains unproven.
8. Write the verification report to output/.
9. If verification fails, record the failure and hand control back to 07-remediate; otherwise continue to 09-deliver.

## Human Check

Review the evidence. A green test is evidence only for what it actually exercised.

## Outputs

| Artifact | Location | Format |
|---|---|---|
| Verification report | output/[run-slug]-verification.md | Markdown |
