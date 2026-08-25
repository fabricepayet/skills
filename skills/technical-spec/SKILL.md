---
name: technical-spec
description: Use when a developer or technical lead needs to turn a current, approved Product Contract into an approved Technical Design before delivery work is decomposed.
---

# Technical Spec

## Purpose

Grill unresolved engineering decisions and publish a versioned Technical Design that explains **how** the approved Product Contract will be delivered. Engineering owns this artifact and cannot change the product contract it consumes.

## Tracker resolution

Before reading or mutating tracked artifacts, resolve the project tracker in this order:

1. an explicit instruction from the user for the current work;
2. the repository's root `CONTRIBUTING.md` and any contribution document it explicitly delegates to;
3. the repository's root `README.md`.

A tracker is configured only when the selected source explicitly designates it for product or issue tracking. A Git host, remote URL, installed connector, badge, or incidental tracker link is not enough. When the selected source names multiple trackers without routing this work, or none of the sources configures one, ask one focused question before publishing. Follow the documented project, issue type, hierarchy, labels, and workflow states when they exist.

## Input gate

Require one exact Product Contract reference and version with status `Product Approved`. Confirm it is the current approved version in the tracker. Stop when the contract is missing, unapproved, or superseded; state the concrete product action needed to resume.

## Responsibility boundary

Technical decisions include module boundaries, data ownership, schemas, interfaces, integrations, security enforcement, failure handling, concurrency, migrations, observability, rollout, recovery, and test seams.

An externally observable behavior, actor, permission, business rule, acceptance criterion, or scope choice is a product decision. Route missing or contradictory product decisions back to the Product Owner instead of resolving them in the design.

## Workflow

1. **Resolve inputs.** Read the complete Product Contract, its current status and revision notes, plus any referenced research or policies. Preserve its stable IDs and wording.
2. **Explore the system.** Read repository instructions, domain terminology, ADRs, relevant code, runtime constraints, integrations, and prior tests. Cite file paths as current evidence, not as an enduring design contract.
3. **Check product completeness.** Map every product requirement and acceptance criterion to a technically realizable responsibility. When implementation requires a new observable product decision, use the clarification loop below.
4. **Build the technical decision tree.** Cover system boundaries and ownership; data and migrations; interfaces and integrations; security and privacy; failure, retry, concurrency, and idempotency; observability and operations; rollout and recovery; and test seams.
5. **Grill the frontier.** Ask at most five numbered questions per round: only independent technical decisions whose prerequisites are settled. Give a recommendation grounded in repository evidence and trade-offs. Find discoverable facts yourself. Continue until no technical decision remains open.
6. **Draft the design.** Use the output contract below. Prefer stable module responsibilities and interfaces over implementation task lists. Record important decisions and rejected alternatives.
7. **Get engineering approval.** Present the complete draft and intended tracker changes. `Technical Approved` requires explicit developer or technical-lead approval, an empty Open Technical Decisions section, and no unresolved Product Clarification Request.
8. **Publish and lock.** Only after engineering approval, create the Technical Design in the resolved tracker as a child of the Product Contract when hierarchy is supported. Record the exact Product Contract version. Do not create a draft or blocked Technical Design in the tracker, and do not create delivery tickets.

## Product clarification loop

When a product gap blocks design, present and then publish a **Product Clarification Request** containing:

- Product Contract reference, version, and affected stable IDs;
- the missing, ambiguous, or contradictory observable decision;
- why it blocks the Technical Design;
- options and an engineering recommendation, clearly marked as advice;
- the Product Owner decision required to resume.

Relate the request to the Product Contract and record `Blocks: Technical Design creation`. Do not create, update, or mark a Technical Design artifact while the clarification is unresolved. Stop the affected design branch. Resume only after `product-spec` publishes a newly approved Product Contract version, then compare the version delta and reopen only affected technical branches.

## Technical Design

Use this template for the working draft presented during grilling and approval. The working draft is not a tracker artifact. Publish it only after its status becomes `Technical Approved`.

```markdown
# Technical Design — <feature name>

Artifact: Technical Design
Version: <integer>
Status: Working Draft | Awaiting Technical Approval | Technical Approved
Product Contract: <tracker reference>@v<integer>
Owner: <evidenced developer or technical lead>
Supersedes: <design reference and version, when applicable>

## Summary and constraints
## Current system evidence
## Architecture and ownership boundaries
## Data model and migrations
## Interfaces and integrations
## Security and privacy enforcement
## Failure handling, concurrency, retry, and idempotency
## Observability and operations
## Rollout and recovery
## Test strategy and seams
## Decisions and rejected alternatives
## Risks and mitigations
## Out of scope
## Open technical decisions
## Revision notes
```

Write `None` under Open Technical Decisions only when every technical branch has a decided answer. Do not invent production limits, capacity, cost, or performance targets; identify the authoritative evidence or decision owner when they are unknown.

## Completion gate

Finish only when the tracker contains one current, explicitly approved Technical Design version linked to the current approved Product Contract version; every product criterion is accounted for; product behavior is unchanged; and delivery planning has not begun.
