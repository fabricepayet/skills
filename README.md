# Skills

A collection of reusable AI agent skills.

## Available skills

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

Install both skills globally from GitHub:

```bash
npx skills add fabricepayet/skills -g \
  --skill explain-pr-changes prevent-feature-abuse
```

This records the GitHub repository, skill paths, and installed versions so the
skills can be updated later:

```bash
npx skills update explain-pr-changes prevent-feature-abuse -g
```

To install only one skill, pass its name to `--skill`. Keep customizations in
this repository: an update replaces the installed copy.

In Codex, both skills are user-invoked and do not activate automatically.
Invoke them as `$explain-pr-changes` or `$prevent-feature-abuse`.

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

Reusable behavior scenarios live in
[`evals/prevent-feature-abuse/evals.json`](evals/prevent-feature-abuse/evals.json).
The latest multi-model results are recorded in
[`model-matrix.json`](evals/prevent-feature-abuse/model-matrix.json). Every pull
request validates the Agent Skills packages and evaluation manifests.

## Inspiration

`explain-pr-changes` was inspired by the semantic diff-reading principles explored
by [Meat](https://github.com/boldsoftware/meat). It is an independent
implementation and does not include Meat's code or transformation engine.

## License

Copyright 2026 Fabrice Payet.

Licensed under the [Apache License 2.0](LICENSE).
