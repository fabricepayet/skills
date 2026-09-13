---
name: delivery-tickets
description: Decompose a current approved Product Contract and Technical Design into executable delivery tickets before implementation.
---

# Delivery Tickets

## Purpose

Produce a dependency graph of **tracer-bullet vertical slices** and publish it when authorized. Each delivery ticket makes one narrow, complete behavior independently demonstrable or verifiable while preserving traceability to approved product and technical decisions.

## Scope and authority

- A planning request is complete when the proposed graph is returned for review, with any gaps identified; it does not require approval of that graph.
- Creating or updating tracker tickets requires explicit publication authorization in the current request or established task scope. Engineering approval confirms the graph; it does not authorize external mutations by itself. Reuse approval of the unchanged graph.

## Tracker resolution

Use the tracker designated for the consuming project: current user instructions, then applicable `AGENTS.md` / `CLAUDE.md` and their references, then `CONTRIBUTING.md`, then `README.md`. A Git host or installed connector alone does not designate a tracker. Reuse established choices and project conventions; resolve conflicts by instruction scope.

Drafting needs no tracker. Before a tracked operation, ask only for the missing destination or information needed for that operation. No setup skill or fixed configuration path is required.

## Artifact references

Use complete inputs from the conversation, supplied files, or the tracker, with stable references, exact versions, and evidenced approval where required. Check currency at the authoritative source; conversation-only inputs need no tracker access. Ask for unavailable content or evidence rather than inventing it. If publication depends on unpublished parents, plan their publication and reference mapping within the user's authorization.

## Input gate

Require exact references and versions for:

- one current Product Contract with status `Product Approved`;
- one current Technical Design with status `Technical Approved` that names that Product Contract version.

Stop when either artifact is missing, unapproved, superseded, or mismatched. Delivery planning cannot complete product or technical design.

## Workflow

For a draft-only request, return the draft at the review step with pending approval clearly labeled. Continue through approval and publication only when requested; existing approval of unchanged content remains valid.

1. **Resolve inputs.** Read both complete artifacts, their revision notes, stable IDs, and tracker relationships. Read repository instructions, domain terminology, ADRs, relevant code, and prior tests when needed to size executable slices.
2. **Build coverage maps.** Account for every Product Contract rule and acceptance criterion and every Technical Design decision. Identify independent observable outcomes rather than implementation layers.
3. **Detect specification gaps.** Route missing observable behavior to a Product Clarification Request and `product-spec`. A newly approved Product Contract version requires `technical-spec` to revise the affected design before ticketing resumes. Route missing engineering decisions directly to a Technical Design revision. Block only affected work; continue drafting unrelated slices when their inputs are complete.
4. **Draft vertical slices.** Apply every vertical-slice invariant below. Give each ticket only genuine blocking edges and keep the executable frontier as wide as the design allows.
5. **Review the graph.** Present the complete proposed breakdown before any tracker mutation. For each ticket show its outcome, covered IDs, relevant technical path, independent verification, and blockers. Ask whether granularity, coverage, and blocking edges are correct; iterate until engineering explicitly approves.
6. **Finalize.** When publication is authorized, create tickets in dependency order in the resolved tracker so native references can be added. Otherwise return the approved graph and, when requested, a mutation plan without changing the tracker. When published, make tickets direct children of the Product Contract and relate each to the Technical Design when the tracker supports those relationships. Use native blocking links, configured ready state, and existing labels; fall back to explicit metadata in the body.
7. **Report the frontier.** List every approved ticket whose blockers are already complete or empty, using published references when available. Leave both approved input artifacts unchanged.

## Vertical-slice invariants

Every ordinary delivery ticket must:

- deliver an observable end-to-end outcome through every **relevant** layer, not a database, API, UI, or test layer in isolation;
- be independently demonstrable or verifiable, including when hidden behind a feature flag;
- fit in one fresh implementation context and contain enough stable context to execute;
- map to Product Contract IDs and Technical Design responsibilities;
- have acceptance criteria about behavior, not implementation activity.

Team boundaries do not justify horizontal tickets. Minimize dependencies: a ticket blocks another only when the latter cannot be completed green without it.

## Technical exceptions

A non-vertical ticket is allowed only when no vertical slice can land green without a preparatory change. Record the evidence, affected scope, bounded exit criterion, and downstream slices it unlocks.

For a wide mechanical refactor or inseparable migration batches, read [migration-slices.md](references/migration-slices.md).

## Delivery ticket

```markdown
# <observable outcome>

Status: <configured ready state, or Draft / Approved for unpublished work>
Parent Product Contract: <artifact ID or source reference>@v<integer>
Technical Design: <artifact ID or source reference>@v<integer>

## What this delivers
## Product criteria covered
## Relevant technical path
## Independent verification
## Acceptance criteria
## Blocked by
## Technical exception
```

Omit Technical exception for a normal vertical slice. Prefer stable domain, module, and interface names. Use current file paths only when they disambiguate execution; they are evidence, not the contract.

## Completion gate

For an approved handoff, require engineering approval of the graph, complete coverage of the inputs, the vertical-slice invariants, and an acyclic blocking graph. When publication was authorized, verify the tracker exposes the immediately executable frontier; otherwise return that frontier and any requested mutation plan.
