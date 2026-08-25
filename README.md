# Skills

A collection of reusable AI agent skills.

## Available skills

### `product-spec`

Grill unresolved product decisions and publish a versioned, Product
Owner-approved Product Contract without making technical decisions.

See [`skills/product-spec/SKILL.md`](skills/product-spec/SKILL.md).

### `technical-spec`

Turn a current approved Product Contract into an engineering-approved
Technical Design, returning missing product decisions to the Product Owner.

See [`skills/technical-spec/SKILL.md`](skills/technical-spec/SKILL.md).

### `delivery-tickets`

Convert approved product and technical artifacts into traceable, independently
verifiable vertical delivery tickets with explicit blocking edges.

See [`skills/delivery-tickets/SKILL.md`](skills/delivery-tickets/SKILL.md).

### `explain-pr-changes`

Turn a GitHub pull request or GitLab merge request into a structured semantic
reading with targeted excerpts. The skill explains behavior, data flow,
architecture, migrations, and tests without turning the result into a defect
review.

See [`skills/explain-pr-changes/SKILL.md`](skills/explain-pr-changes/SKILL.md).

### `prevent-feature-abuse`

Audit or harden product features exposed to excessive requests, uploads,
retries, concurrency, queues, storage growth, external APIs, or AI spend. The
skill maps the complete execution chain and requires measurable limits without
turning temporary throttling into user-data loss.

See [`skills/prevent-feature-abuse/SKILL.md`](skills/prevent-feature-abuse/SKILL.md).

## Installation

Install all skills globally from GitHub:

```bash
npx skills add fabricepayet/skills -g \
  --skill product-spec technical-spec delivery-tickets \
  explain-pr-changes prevent-feature-abuse
```

This records the GitHub repository, skill paths, and installed versions so the
skills can be updated later:

```bash
npx skills update product-spec technical-spec delivery-tickets \
  explain-pr-changes prevent-feature-abuse -g
```

To install only one skill, pass its name to `--skill`. Keep customizations in
this repository: an update replaces the installed copy.

In Codex, these skills are user-invoked and do not activate automatically.
Invoke them explicitly with their `$skill-name`.

## Using the specification workflow

Run the workflow through its approval gates:

```text
$product-spec -> approved Product Contract
$technical-spec -> approved Technical Design
$delivery-tickets -> vertical implementation tickets
```

An approved Product Contract is locked. Missing product behavior creates a
Product Clarification Request and returns to `$product-spec`; technical design
cannot change the contract silently. Delivery tickets are published only after
both artifacts and the proposed ticket graph have been approved.

## Why three separate skills?

Software delivery crosses three distinct decision authorities. Combining them
in one specification makes ownership ambiguous: technical convenience can
silently change product behavior, while ticket planning can introduce design
decisions that nobody reviewed. This workflow places an explicit approval
boundary between each responsibility.

| Stage | Accountable role | Decision authority | Approved artifact |
| --- | --- | --- | --- |
| Product specification | Product Owner | Problem, outcomes, actors, permissions, business rules, observable behavior, acceptance criteria, and scope | Product Contract |
| Technical specification | Technical lead and development team | Architecture, ownership boundaries, data, interfaces, security, reliability, observability, rollout, and test strategy | Technical Design |
| Delivery planning | Technical lead and development team | Vertical slices, execution dependencies, independent verification, and executable frontier | Delivery ticket graph |

The technical owner may be a lead developer, tech lead, architect, or a
developer designated by the team. The workflow defines decision authority, not
a mandatory job title. Developers may advise the Product Owner about cost or
feasibility, but only the Product Owner can approve a change to observable
product behavior.

### Role-scoped grilling

`product-spec` and `technical-spec` each contain their own decision-tree
interview. The product grilling resolves **what and why** without choosing an
implementation. The technical grilling resolves **how** without redefining the
approved product. Both ask only the current frontier of independent questions,
provide recommendations, and continue until no decision in their authority
remains open.

### Handoffs and clarification

Each downstream stage consumes exact, approved artifact versions. When
engineering discovers missing or contradictory product behavior, it creates a
Product Clarification Request instead of changing the Product Contract. The
Product Owner then approves a new contract version, and engineering revises
only the affected part of the Technical Design. Delivery tickets cannot fill
product or technical gaps; they return them to the responsible skill.

This preserves product ownership, engineering autonomy, and traceability while
preventing silent scope changes between discovery and implementation.

## Using `prevent-feature-abuse`

Invoke the skill explicitly with `$prevent-feature-abuse`.

Read-only audit:

```text
Use $prevent-feature-abuse to audit the attachment upload flow for request,
storage, concurrency, retry, and tenant-isolation abuse. Report findings only.
```

Implementation:

```text
Use $prevent-feature-abuse to harden the attachment upload flow against abuse.
Implement the controls and prove each boundary with red-green tests.
```

An audit request remains read-only. A request that explicitly asks to fix or
implement protections authorizes in-scope changes. The skill reports missing
production data instead of inventing quota, retry, retention, capacity, or cost
figures.

## Evaluation

Reusable behavior scenarios live under [`evals`](evals). The latest
multi-model results for `prevent-feature-abuse` are recorded in
[`model-matrix.json`](evals/prevent-feature-abuse/model-matrix.json). Every pull
request validates the Agent Skills packages and evaluation manifests.

## Inspiration

- The specification workflow is an independent adaptation inspired by Matt
  Pocock's [`grilling`, `to-spec`, and `to-tickets` skills](https://github.com/mattpocock/skills).
  Matt Pocock is not affiliated with or responsible for this project. See
  [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the original MIT
  license notice.
- `explain-pr-changes` was inspired by the semantic diff-reading principles
  explored by [Meat](https://github.com/boldsoftware/meat). It is an independent
  implementation and does not include Meat's code or transformation engine.

## License

Copyright 2026 Fabrice Payet.

Licensed under the [MIT License](LICENSE). Third-party notices are listed in
[`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).
