---
name: verification-before-completion
description: Use immediately before claiming software work is fixed, complete, passing, deployable, or ready to merge, especially after code changes, delegated work, or partial validation.
---

# Verification Before Completion

Match every completion claim to fresh evidence of the same scope. Report what was actually verified; keep everything else explicitly unverified.

## Verification gate

1. **Name the claim.** Identify the exact statement you are about to make: the original bug is fixed, a focused test passes, the full suite passes, requirements are met, or the change is deployable.
2. **Choose decisive evidence.** Select the smallest current observation that directly proves that claim. Increase breadth with the blast radius; avoid unrelated build or test work that cannot change the conclusion.
3. **Run and read it.** Execute the complete command or end-to-end check now. Read its full relevant output, exit status, failure count, warnings, and skipped checks. For delegated work, inspect the resulting diff and run the decisive verification independently.
4. **Compare scope.** A focused test supports a focused claim. Only a complete configured suite supports “all tests pass.” Static review does not prove runtime behavior, and type-checking does not prove deployment readiness.
5. **Report precisely.** State the command or observation, its result, and any material validation not run. If evidence fails or cannot be obtained, report the actual status and the next decisive check instead of a success claim.

## Evidence selection

| Claim | Direct evidence |
| --- | --- |
| Original bug fixed | Reproduce the original symptom or run a regression check that fails without the fix and passes with it |
| Changed behavior works | Focused behavior test plus affected integration or type checks |
| All tests pass | Fresh successful run of the complete configured test suite |
| Requirements met | Requirement-by-requirement check against the implementation and relevant tests |
| Migration ready | Generated SQL inspection and successful application to a representative disposable database, with data and schema checks |
| Deployable | Project-defined release checks plus any migration, configuration, integration, or rollback evidence required by the change |

## Boundaries

- Prior runs, plausible diffs, clean logs, reviewer approval, and agent reports are useful context, not fresh verification.
- When environment access prevents decisive verification, say which conclusion remains unverified and why.
- Do not broaden “passes” or “ready” beyond the evidence you can cite.
