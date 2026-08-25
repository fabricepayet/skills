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
