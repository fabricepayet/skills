---
name: product-spec
description: Use when a product owner needs to turn a feature idea, problem, clarification, or existing product discussion into an approved Product Contract before technical design begins.
---

# Product Spec

## Purpose

Grill unresolved product decisions and publish a versioned Product Contract that defines **what** the product must guarantee and **why**. The Product Owner owns this artifact; engineering consumes it without changing it.

## Tracker resolution

Before reading or mutating tracked artifacts, resolve the project tracker in this order:

1. an explicit instruction from the user for the current work;
2. the repository's root `CONTRIBUTING.md` and any contribution document it explicitly delegates to;
3. the repository's root `README.md`.

A tracker is configured only when the selected source explicitly designates it for product or issue tracking. A Git host, remote URL, installed connector, badge, or incidental tracker link is not enough. When the selected source names multiple trackers without routing this work, or none of the sources configures one, ask one focused question before publishing. Follow the documented project, issue type, hierarchy, labels, and workflow states when they exist.

## Responsibility boundary

Include actors, outcomes, journeys, observable behavior, business rules, permissions, edge cases, success measures, acceptance criteria, and scope.

Leave architecture, modules, file paths, schemas, APIs, migrations, implementation sequencing, and test strategy to `technical-spec`. Technical feasibility may inform a recommendation, but it never decides product behavior.

## Workflow

1. **Resolve the source.** Read the current conversation and any referenced issue, research, domain glossary, policy, or existing Product Contract. Find environmental facts yourself. Preserve settled decisions and stable requirement IDs.
2. **Build the product decision tree.** Cover the problem and outcome; actors and permissions; happy paths and state transitions; business rules; failure and recovery behavior; edge cases; scope; and measurable success.
3. **Grill the frontier.** Ask at most five numbered questions per round: only independent decisions whose prerequisites are settled. Give a recommended answer with its product trade-off. Skip branches already settled by evidence. Continue until no product decision remains open.
4. **Draft the contract.** Use the output contract below. Acceptance criteria must be observable and independently verifiable. Use stable IDs such as `BR-01`, `US-01`, and `AC-01`; never renumber unchanged items in later versions.
5. **Get Product Owner approval.** Present the complete draft and the intended tracker changes. `Product Approved` requires explicit approval from the Product Owner and an empty Open Product Decisions section.
6. **Publish and lock.** Create or update the Product Contract in the resolved project tracker. Use native workflow states and parent relationships when available; otherwise retain the metadata in the body. Record the approving person and date only when evidenced.

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

Finish only when the tracker contains one current, explicitly approved Product Contract version; prior versions remain auditable; every requirement has a stable ID; and no technical decision or delivery ticket has been smuggled into the artifact.
