---
name: delivery-tickets
description: Use when a team needs to decompose a current approved Product Contract and Technical Design into executable delivery tickets before implementation begins.
---

# Delivery Tickets

## Purpose

Publish a dependency graph of **tracer-bullet vertical slices**. Each delivery ticket makes one narrow, complete behavior independently demonstrable or verifiable while preserving traceability to approved product and technical decisions.

## Tracker resolution

Before reading or mutating tracked artifacts, resolve the project tracker in this order:

1. an explicit instruction from the user for the current work;
2. the repository's root `CONTRIBUTING.md` and any contribution document it explicitly delegates to;
3. the repository's root `README.md`.

A tracker is configured only when the selected source explicitly designates it for product or issue tracking. A Git host, remote URL, installed connector, badge, or incidental tracker link is not enough. When the selected source names multiple trackers without routing this work, or none of the sources configures one, ask one focused question before publishing. Follow the documented project, issue type, hierarchy, labels, and workflow states when they exist.

## Input gate

Require exact references and versions for:

- one current Product Contract with status `Product Approved`;
- one current Technical Design with status `Technical Approved` that names that Product Contract version.

Stop when either artifact is missing, unapproved, superseded, or mismatched. Delivery planning cannot complete product or technical design.

## Workflow

1. **Resolve inputs.** Read both complete artifacts, their revision notes, stable IDs, and tracker relationships. Read repository instructions, domain terminology, ADRs, relevant code, and prior tests when needed to size executable slices.
2. **Build coverage maps.** Account for every Product Contract rule and acceptance criterion and every Technical Design decision. Identify independent observable outcomes rather than implementation layers.
3. **Detect specification gaps.** Route missing observable behavior to a Product Clarification Request and `product-spec`. A newly approved Product Contract version requires `technical-spec` to revise the affected design before ticketing resumes. Route missing engineering decisions directly to a Technical Design revision. Block only affected work; continue drafting unrelated slices when their inputs are complete.
4. **Draft vertical slices.** Apply every vertical-slice invariant below. Give each ticket only genuine blocking edges and keep the executable frontier as wide as the design allows.
5. **Review the graph.** Present the complete proposed breakdown before mutating the tracker. For each ticket show its outcome, covered IDs, relevant technical path, independent verification, and blockers. Ask whether granularity, coverage, and blocking edges are correct; iterate until engineering explicitly approves.
6. **Publish.** Create tickets in dependency order in the resolved tracker so native references can be added. Make them direct children of the Product Contract and relate each to the Technical Design when the tracker supports those relationships. Use native blocking links, configured ready state, and existing labels; fall back to explicit metadata in the body.
7. **Report the frontier.** List every published ticket whose blockers are already complete or empty. Leave both approved input artifacts unchanged.

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

For a wide mechanical refactor, use **expand–contract**:

1. expand by introducing the new form beside the old while keeping the system green;
2. migrate consumers in bounded, independently green batches;
3. contract by removing the old form after every migration batch completes.

When a migration batch cannot be green alone, declare the shared integration boundary and add a final integrate-and-verify ticket. Never disguise technical preparation as user-visible value.

## Delivery ticket

```markdown
# <observable outcome>

Status: <configured ready state>
Parent Product Contract: <tracker reference>@v<integer>
Technical Design: <tracker reference>@v<integer>

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

Finish only when engineering has approved the breakdown; every product criterion and technical decision is mapped; every ticket is vertical or carries a proven bounded exception; every ticket has independent verification; the blocking graph is acyclic; and the tracker exposes the immediately executable frontier.
