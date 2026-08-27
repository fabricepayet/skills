# Skills

A collection of reusable AI agent skills.

## Project management

This repository uses
[GitHub Issues](https://github.com/fabricepayet/skills/issues) for contribution
and project tracking. Branches, commits, and pull requests reference the
corresponding issue when applicable.

## Available skills

### `pragmatic-development-workflow`

Implement an authorized feature, bug fix, refactor, or behavior change with a
workflow sized to its ambiguity and risk. Bounded changes proceed directly;
unresolved decisions about contracts, security, migrations, or rollback pause
only the affected work.

See [`skills/pragmatic-development-workflow/SKILL.md`](skills/pragmatic-development-workflow/SKILL.md).

### `systematic-debugging`

Diagnose bugs and incidents through failing evidence, system boundaries, and
falsifiable hypotheses before choosing a durable fix.

See [`skills/systematic-debugging/SKILL.md`](skills/systematic-debugging/SKILL.md).

### `receiving-code-review`

Evaluate code-review feedback against the code, tests, contracts, and project
rules before applying, adapting, clarifying, or contesting it.

See [`skills/receiving-code-review/SKILL.md`](skills/receiving-code-review/SKILL.md).

### `verification-before-completion`

Match claims such as fixed, passing, complete, mergeable, or deployable to
fresh evidence of the same scope.

See [`skills/verification-before-completion/SKILL.md`](skills/verification-before-completion/SKILL.md).

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

### Replacing Superpowers

Disable Superpowers in every agent host where it is enabled before installing
these workflows. In Codex, set
`plugins."superpowers@claude-plugins-official".enabled` to `false` in
`~/.codex/config.toml`. In Claude Code, set
`enabledPlugins["superpowers@claude-plugins-official"]` to `false` in
`~/.claude/settings.json`. Restart the affected host so its skill list and
session hooks reload.

Install all skills globally from GitHub:

```bash
npx skills add fabricepayet/skills -g \
  --skill pragmatic-development-workflow systematic-debugging \
  receiving-code-review verification-before-completion product-spec \
  technical-spec delivery-tickets explain-pr-changes prevent-feature-abuse
```

This records the GitHub repository, skill paths, and installed versions so the
skills can be updated later:

```bash
npx skills update product-spec technical-spec delivery-tickets \
  explain-pr-changes prevent-feature-abuse pragmatic-development-workflow \
  systematic-debugging receiving-code-review \
  verification-before-completion -g
```

To install only one skill, pass its name to `--skill`. Keep customizations in
this repository: an update replaces the installed copy.

The four development workflow skills can activate automatically when their
focused descriptions match the current task. The specification, delivery,
explanation, and abuse-prevention skills remain user-invoked in Codex through
their `$skill-name`.

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
Product Clarification Request instead of changing the Product Contract or
creating a blocked Technical Design. The Product Owner then approves a new
contract version. Engineering resumes the affected design branches and creates
the Technical Design only after technical approval. Delivery tickets cannot
fill product or technical gaps; they return them to the responsible skill.

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
