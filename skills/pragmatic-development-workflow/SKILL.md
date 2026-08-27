---
name: pragmatic-development-workflow
description: Use when implementing an authorized feature, bug fix, refactor, or behavior change in an existing software project; do not use for read-only analysis, planning, or review.
---

# Pragmatic Development Workflow

Deliver the smallest durable end-to-end change with process proportional to ambiguity and risk. An implementation request authorizes ordinary in-scope local edits and verification without a routine approval round.

## Start from the project

1. Read applicable repository instructions and check the current Git state. Preserve unrelated work.
2. Trace the existing behavior, adjacent patterns, tests, types, data boundaries, and configured validation commands before choosing files to change.
3. Classify the execution shape:
   - **Bounded:** the flow exists, expected behavior is clear, and the change does not redefine a shared contract or operational boundary. State the intended change briefly and implement it directly.
   - **Decision-bound:** security policy, observable behavior, public interfaces, data migration, cross-system coordination, or rollback depends on an unresolved choice. Present the smallest useful design or plan and request only the material decision needed to proceed.

Create a persistent specification or plan only when the user requests it or the repository requires it. Use an internal task plan when dependencies or verification breadth make progress tracking useful.

## Implement a coherent slice

- Change validation, types, domain behavior, persistence, interfaces, and user-visible output together wherever the behavior crosses those layers.
- Follow established project patterns unless evidence shows they violate the current requirement. Improve only nearby structure needed for a durable implementation.
- Prefer maintained libraries and existing abstractions over new infrastructure. Remove superseded paths when compatibility is not required.
- When the cause of a bug is unknown, apply `systematic-debugging` before selecting the fix.

## Choose proof by risk

| Change | Appropriate proof |
| --- | --- |
| Bug fix | Reproduce the original failure when practical, then keep a regression check that distinguishes the fix |
| Behavior change | Write or adapt a focused automated test first when it can express the intended contract, then run affected integration checks |
| Refactor | Establish a passing behavioral baseline, change structure, and prove behavior remains stable |
| UI | Test logic and interactions, then inspect rendered behavior and relevant accessibility states |
| Configuration or generated output | Validate the resulting invariant with the owning tool; do not manufacture a failing unit test |
| Data migration | Use the project migration workflow, inspect generated operations, protect data as required, and apply to a disposable representative database |

Testing is evidence, not ceremony. Increase coverage with blast radius, failure cost, and irreversibility.

## Coordination and completion

- Use subagents only when the user or applicable project/skill instructions request delegation and the work divides into independent responsibilities.
- Pause for a missing material decision, unavailable required authority, destructive external action, or evidence that the requested scope cannot safely deliver the outcome. Resolve ordinary implementation details independently.
- Before reporting success, apply `verification-before-completion`. Lead the final report with the practical result, followed by fresh verification and one concrete unresolved item only when one remains.
