---
name: technical-spec
description: Turn a current approved Product Contract into an approved Technical Design before delivery work is decomposed.
---

# Technical Spec

## Purpose

Grill unresolved engineering decisions and produce a versioned Technical Design that explains **how** the approved Product Contract will be delivered, publishing it when authorized. Engineering owns this artifact and cannot change the product contract it consumes.

## Scope and authority

- A drafting request is complete when the requested design draft is returned with unresolved decisions identified; it does not require approval of that draft.
- Creating or updating tracker artifacts requires explicit publication authorization in the current request or established task scope. Engineering approval confirms the design; it does not authorize an external mutation by itself. Reuse approval of the unchanged version.

## Tracker resolution

Use the tracker designated for the consuming project: current user instructions, then applicable `AGENTS.md` / `CLAUDE.md` and their references, then `CONTRIBUTING.md`, then `README.md`. A Git host or installed connector alone does not designate a tracker. Reuse established choices and project conventions; resolve conflicts by instruction scope.

Drafting needs no tracker. Before a tracked operation, ask only for the missing destination or information needed for that operation. No setup skill or fixed configuration path is required.

## Artifact references

Use complete inputs from the conversation, supplied files, or the tracker, with stable references, exact versions, and evidenced approval where required. Check currency at the authoritative source; conversation-only inputs need no tracker access. Ask for unavailable content or evidence rather than inventing it. If publication depends on unpublished parents, plan their publication and reference mapping within the user's authorization.

## Input gate

Require one exact Product Contract reference and version with status `Product Approved`. Confirm it is the current approved version in its authoritative source. Stop when the contract is missing, unapproved, or superseded; state the concrete product action needed to resume.

## Responsibility boundary

Technical decisions include module boundaries, data ownership, schemas, interfaces, integrations, security enforcement, failure handling, concurrency, migrations, observability, rollout, recovery, and test seams.

An externally observable behavior, actor, permission, business rule, acceptance criterion, or scope choice is a product decision. Route missing or contradictory product decisions back to the Product Owner instead of resolving them in the design.

## Workflow

For a draft-only request, return the draft at the review step with pending approval clearly labeled. Continue through approval and publication only when requested; existing approval of unchanged content remains valid.

1. **Resolve inputs.** Read the complete Product Contract, its current status and revision notes, plus any referenced research or policies. Preserve its stable IDs and wording.
2. **Explore the system.** Read applicable instructions and the code, tests, domain terms, or ADRs needed for the affected design. Cite file paths as current evidence, not as an enduring design contract.
3. **Check product completeness.** Map every product requirement and acceptance criterion to a technically realizable responsibility. When implementation requires a new observable product decision, use the clarification loop below.
4. **Build the technical decision tree.** Assess the relevant boundaries, data, interfaces, security, failure handling, rollout, observability, and test seams. Omit dimensions the change does not affect.
5. **Grill the frontier.** Ask at most five numbered questions per round: only independent technical decisions whose prerequisites are settled. Give a recommendation grounded in repository evidence and trade-offs. Find discoverable facts yourself. Resolve routine implementation choices from repository conventions and record them in the design. Ask only about unresolved choices that materially affect architecture, security, compatibility, cost, or rollout.
6. **Draft the design.** Use the output contract below, including only relevant design sections while retaining version, source, approval, and open-decision metadata. Prefer stable module responsibilities and interfaces over implementation task lists. Record important decisions and rejected alternatives.
7. **Get engineering approval.** Present the complete draft and, when publication is in scope, the intended tracker changes. `Technical Approved` requires explicit developer or technical-lead approval, no unresolved material technical decisions, and no unresolved Product Clarification Request.
8. **Finalize.** Only after engineering approval, publish the Technical Design when tracker publication is authorized; otherwise return the approved artifact and, when requested, a publication plan without mutating the tracker. When published, make it a child of the Product Contract when hierarchy is supported and record the exact Product Contract version. Do not create a draft or blocked Technical Design in the tracker, and do not create delivery tickets.

## Product clarification loop

When a product gap blocks design, present a **Product Clarification Request** and publish it only when tracker publication is authorized. It contains:

- Product Contract reference, version, and affected stable IDs;
- the missing, ambiguous, or contradictory observable decision;
- why it blocks the Technical Design;
- options and an engineering recommendation, clearly marked as advice;
- the Product Owner decision required to resume.

Relate the request to the Product Contract and record `Blocks: Technical Design creation`. Do not create, update, or mark a Technical Design artifact while the clarification is unresolved. Stop the affected design branch. Resume only after `product-spec` returns or publishes a newly approved Product Contract version, then compare the version delta and reopen only affected technical branches.

## Technical Design

Use this template for the working draft presented during grilling and approval. The working draft is not a tracker artifact. Publish it only after its status becomes `Technical Approved`.

```markdown
# Technical Design — <feature name>

Artifact: Technical Design
Version: <integer>
Status: Working Draft | Awaiting Technical Approval | Technical Approved
Product Contract: <artifact ID or source reference>@v<integer>
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

List material unresolved choices under Open Technical Decisions; routine choices recorded in the design do not require separate approval. Write `None` when no material choice remains open. Do not invent production limits, capacity, cost, or performance targets; identify the authoritative evidence or decision owner when they are unknown.

## Completion gate

For an approved handoff, return or publish one current, explicitly approved Technical Design version linked to the current approved Product Contract version; every product criterion is accounted for; product behavior is unchanged; and delivery planning has not begun. When publication was authorized, verify the tracker contains that exact version.
