---
name: product-spec
description: Turn a feature idea, problem, clarification, or product discussion into an approved Product Contract before technical design.
---

# Product Spec

## Purpose

Grill unresolved product decisions and produce a versioned Product Contract that defines **what** the product must guarantee and **why**, publishing it when authorized. The Product Owner owns this artifact; engineering consumes it without changing it.

## Scope and authority

- A drafting request is complete when the requested draft is returned with unresolved decisions identified; it does not require approval of that draft.
- Creating or updating tracker artifacts requires explicit publication authorization in the current request or established task scope. Product approval confirms the content; it does not authorize an external mutation by itself. Reuse approval of the unchanged version.

## Tracker resolution

Use the tracker designated for the consuming project: current user instructions, then applicable `AGENTS.md` / `CLAUDE.md` and their references, then `CONTRIBUTING.md`, then `README.md`. A Git host or installed connector alone does not designate a tracker. Reuse established choices and project conventions; resolve conflicts by instruction scope.

Drafting needs no tracker. Before a tracked operation, ask only for the missing destination or information needed for that operation. No setup skill or fixed configuration path is required.

## Artifact references

Use complete inputs from the conversation, supplied files, or the tracker, with stable references, exact versions, and evidenced approval where required. Check currency at the authoritative source; conversation-only inputs need no tracker access. Ask for unavailable content or evidence rather than inventing it.

## Responsibility boundary

Include actors, outcomes, journeys, observable behavior, business rules, permissions, edge cases, success measures, acceptance criteria, and scope.

Leave architecture, modules, file paths, schemas, APIs, migrations, implementation sequencing, and test strategy to `technical-spec`. Technical feasibility may inform a recommendation, but it never decides product behavior.

## Workflow

For a draft-only request, return the draft at the review step with pending approval clearly labeled. Continue through approval and publication only when requested; existing approval of unchanged content remains valid.

1. **Resolve the source.** Read the current conversation and any referenced issue, research, domain glossary, policy, or existing Product Contract. Find environmental facts yourself. Preserve settled decisions and stable requirement IDs.
2. **Build the product decision tree.** Cover the problem and outcome; actors and permissions; happy paths and state transitions; business rules; failure and recovery behavior; edge cases; scope; and measurable success.
3. **Grill the frontier.** Ask at most five numbered questions per round: only independent decisions whose prerequisites are settled. Give a recommended answer with its product trade-off. Skip branches already settled by evidence. Ask about unresolved choices that change behavior, scope, or acceptance criteria; reuse settled decisions. An approved contract must resolve those choices.
4. **Draft the contract.** Use the output contract below. Acceptance criteria must be observable and independently verifiable. Use stable IDs such as `BR-01`, `US-01`, and `AC-01`; never renumber unchanged items in later versions.
5. **Get Product Owner approval.** Present the complete draft and, when publication is in scope, the intended tracker changes. `Product Approved` requires explicit approval from the Product Owner and an empty Open Product Decisions section.
6. **Finalize.** When publication is authorized, create or update the Product Contract in the resolved project tracker. Otherwise return the approved artifact and, when requested, a publication plan without mutating the tracker. Use native workflow states and parent relationships when available; otherwise retain the metadata in the body. Record the approving person and date only when evidenced.

## Locked revisions

An approved version is immutable to agents. A new requirement, ambiguity, or Product Clarification Request opens `vNext` as a draft:

- preserve the last approved version as a versioned snapshot or tracker revision;
- change only decisions reopened by the clarification;
- preserve unchanged IDs and record added, changed, and removed IDs;
- require fresh Product Owner approval before `vNext` becomes current.

Engineering feedback can trigger a revision; it cannot approve one. If the tracker cannot enforce locking, enforce it procedurally and make the version explicit in every downstream reference.

## Product Contract

```markdown
# Product Contract — <feature name>

Artifact: Product Contract
Version: <integer>
Status: Draft | Awaiting Product Approval | Product Approved
Owner: <evidenced Product Owner>
Supersedes: <contract reference and version, when applicable>

## Problem and desired outcome
## Actors and permissions
## Scope and user journeys
## Business rules
## User stories
## Acceptance criteria
## Edge cases and failure behavior
## Success measures
## Out of scope
## Open product decisions
## Revision notes
```

Write `None` under Open Product Decisions only when every product branch has a decided answer. Success measures may name the measurement and decision threshold owner without inventing unevidenced numeric targets.

## Completion gate

For an approved handoff, return or publish one current, explicitly approved Product Contract version; prior versions remain auditable; every requirement has a stable ID; and no technical decision or delivery ticket has been smuggled into the artifact. When publication was authorized, verify the tracker contains that exact version.
