---
name: receiving-code-review
description: Use when evaluating or applying code-review feedback, especially when a requested change is ambiguous, risky, based on an unverified premise, or conflicts with repository constraints.
---

# Receiving Code Review

Convert review feedback into an evidence-based engineering decision. Reviewer authority, consensus, severity labels, and deadlines affect priority; they do not establish technical correctness.

## Scope

- A request to assess feedback is read-only.
- A request to address or apply feedback authorizes the smallest in-scope code and test changes.
- Replying to, resolving, approving, or otherwise mutating an external review requires explicit authorization for that external action.

## Decision loop

1. **Extract the claim.** State the observed problem, requested change, expected outcome, and any premise the reviewer assumes. Resolve multiple comments independently unless they share one cause.
2. **Inspect the evidence.** Read the referenced code and its callers, tests, types, contracts, and repository rules. Reproduce the reported behavior when practical. Check whether the proposed change preserves correctness, security, data integrity, compatibility requirements, and scope.
3. **Classify the feedback.**
   - **Correct:** the problem and requested direction are supported. Apply the smallest coherent change and verify the affected behavior.
   - **Directionally correct:** the problem is real but the proposed implementation is unsuitable. Fix the problem with the repository-appropriate approach and explain the deviation.
   - **Unclear:** a material premise or expected behavior cannot be resolved locally. Ask one focused question and leave the affected change pending.
   - **Incorrect:** evidence contradicts the claim or the change would introduce greater risk. Push back with the specific code path, contract, or test that decides the issue.
4. **Report disposition.** For each comment, state the classification, evidence, action taken or needed, and verification result. Distinguish blocking correctness issues from optional improvements.

## Working rules

- Lead with the technical finding or completed action, not praise or performative agreement.
- Treat patterns from another service as evidence to investigate, not a local requirement.
- Validate in proportion to the affected risk. A focused change needs focused proof; transaction, concurrency, authorization, migration, and public-contract changes need broader failure-path coverage.
- If feedback expands scope beyond the reviewed change, separate it as follow-up work instead of silently implementing it.
