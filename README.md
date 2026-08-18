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

Clone the repository, then copy the required skill into your agent's skill
directory. For Codex:

```bash
git clone https://github.com/fabricepayet/skills.git
cd skills
cp -R skills/prevent-feature-abuse ~/.codex/skills/
```

Use the equivalent skills directory for another Agent Skills-compatible
client.

## Using `prevent-feature-abuse`

The skill can activate automatically when a request mentions abuse, rate
limiting, quotas, oversized uploads, retry amplification, queue pressure,
external API cost, AI spend, or offline resubmission. In Codex, invoke it
explicitly with `$prevent-feature-abuse` when you want deterministic routing.

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
