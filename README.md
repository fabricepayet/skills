# Skills

A collection of reusable AI agent skills.

## Available skills

### `read-pr-changes`

Turn a GitHub pull request or GitLab merge request into a structured semantic
reading with targeted excerpts. The skill explains behavior, data flow,
architecture, migrations, and tests without turning the result into a defect
review.

See [`skills/read-pr-changes/SKILL.md`](skills/read-pr-changes/SKILL.md).

## Installation

Copy the skill directory into your agent's skill directory. For Codex:

```bash
cp -R skills/read-pr-changes ~/.codex/skills/
```

## Inspiration

`read-pr-changes` was inspired by the semantic diff-reading principles explored
by [Meat](https://github.com/boldsoftware/meat). It is an independent
implementation and does not include Meat's code or transformation engine.

## License

Copyright 2026 Fabrice Payet.

Licensed under the [Apache License 2.0](LICENSE).
