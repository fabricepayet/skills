---
name: explain-pr-changes
description: Explain an existing GitHub PR or GitLab MR through its intent, behavior, and decisive code excerpts.
---

# Explain PR Changes

Explain what changed and why it matters, organized by concept rather than file order. This is a read-only explanation; when defect review is also requested, handle that as a separate review.

## Retrieve evidence

Resolve a supplied URL, repository plus number, or number in the current repository. Ask for the repository only when the target remains ambiguous. Use an available authorized connector, CLI, or supplied diff. For platform CLI commands, read [cli-access.md](references/cli-access.md). A missing CLI alone is not a blocker; use another available evidence source without bypassing access controls.

Obtain the metadata and complete diff when available. Inspect all changed files before claiming a complete reading; when output is truncated, retrieve the missing parts. If access is incomplete, explain the supplied portion and state the limits rather than inventing context. Read nearby code only when needed to interpret a change.

## Explain the behavior

Connect the old flow to the new flow: inputs, transformations, decisions, contracts, side effects, lifecycle, and tests. Check the PR/MR description against the actual diff. Treat moved behavior as relocation and group repetitive renames or generated changes.

Select a few excerpts that establish non-obvious behavior, conditions, computations, data shapes, or test outcomes. Omit routine imports, formatting, repeated fixtures, and equivalent call sites.

Copy quoted code exactly from the diff, with file and changed-line references. Keep diff markers when useful. Do not regenerate code or insert ellipses that are absent from the source; choose smaller exact excerpts instead.

## Deliver the reading

Use the user's language and requested format. For a small change, a short explanation and one decisive excerpt may suffice. For larger changes, cover intent, the before/after flow, important decisions, decisive excerpts, behavior-defining tests, and reading limits. Mention omitted mechanical changes only when their scope matters.

Keep the explanation distinct from findings, severity ratings, remediation, and merge approval. Reading a PR/MR does not authorize commenting, labeling, editing, or merging it.
