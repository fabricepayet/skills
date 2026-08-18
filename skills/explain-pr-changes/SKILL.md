---
name: explain-pr-changes
description: "Use when a user wants to understand, summarize, abridge, or get a semantic reading of an existing GitHub pull request or GitLab merge request by number, URL, or repository reference, without asking for defect findings."
---

# Explain PR Changes

Turn a GitHub PR or GitLab MR into a structured semantic reading. Explain what
changed and why it matters to the program; do not search for defects or propose
fixes.

## Keep the scope separate from code review

- Describe intent, behavior, data flow, control flow, algorithms, architecture,
  lifecycle, compatibility, migrations, and tests.
- Do not produce findings, severity levels, security analysis, remediation, or
  approval advice.
- When the user also requests defect review, keep the semantic reading separate
  and route the defect-oriented work to the applicable review skill.
- Remain read-only. Never comment, approve, close, merge, label, edit, or
  otherwise mutate the PR/MR or repository.

## Resolve the target

Accept any of these forms:

- a complete GitHub PR or GitLab MR URL;
- a repository plus PR/MR identifier;
- a numeric identifier in the current Git repository.

Resolve the target as follows:

1. Detect GitHub or GitLab from a supplied URL.
2. For a numeric identifier, run `git remote get-url origin` and detect the
   platform and repository from the remote.
3. If a number cannot be tied safely to a repository, ask for the repository or
   complete URL. Ask only then.
4. For a GitLab URL such as
   `https://gitlab.com/group/project/-/merge_requests/123`, extract repository
   `group/project` and IID `123`; pass them through `glab ... -R group/project`.

## Retrieve evidence read-only

Before retrieving anything, verify that the CLI for the detected platform is
available with `command -v gh` or `command -v glab`. If it is missing, name the
required CLI and stop with its official installation page as the next action.
Do not install software unless the user explicitly asks.

Retrieve metadata, changed files, and the complete diff before analyzing it.
Use `-R <repository>` when the target repository is not the current one.

GitHub:

```bash
gh pr view <number-or-url> --json number,title,body,baseRefName,headRefName,author,files,additions,deletions,url
gh pr diff <number-or-url> --color=never
```

GitLab:

```bash
glab mr view <iid-or-branch> --output json
glab mr diff <iid-or-branch> --raw --color=never
```

If authentication fails, report the concrete next action: `gh auth login` or
`glab auth login`. Do not substitute unauthenticated web scraping when the CLI
can identify the access problem.

Inspect nearby repository context only when it can change the interpretation
of a semantic anchor. Use read-only operations such as `rg`, `sed`, `git show`,
`gh api`, or `glab api`; do not modify the worktree. Do not over-investigate
lines that are already clear from the diff.

When command output is too large or truncated, save the raw diff to a temporary
file, split it at `diff --git` file boundaries, and inspect every part. Group
related files for analysis, then discard the temporary file after the reading
is complete.

## Analyze in two passes

### Pass 1: Build the whole-request map

Reason across all files before selecting excerpts. Identify:

- the request's stated intent and the intent evidenced by code;
- the previous flow and the new flow;
- where data originates, how it changes, and where it goes;
- the contracts, conditions, transformations, and observable effects;
- cross-file moves, lifecycle changes, migrations, and compatibility
  boundaries;
- the tests that specify distinctive behavior.

Do not assume the PR/MR description is accurate when the diff shows something
different. State the evidenced behavior neutrally.

### Pass 2: Select decisive evidence

Keep exact excerpts that expose at least one of these anchors:

- a changed contract, public behavior, or data shape;
- a behavior-changing condition, branch, exception boundary, or return path;
- a non-obvious computation, normalization, lookup, mutation, or dispatch;
- an observable effect, state transition, emitted response, or external call;
- an algorithmic, architectural, lifecycle, compatibility, or migration
  decision;
- a test's scenario identity, distinctive stimulus, and decisive outcome.

Compress or omit:

- imports, formatting, unchanged context, and generated outputs;
- repeated renames and equivalent call-site migrations after one useful anchor;
- routine error-message construction when error identity and control flow did
  not change;
- repetitive setup, fixtures, cases, teardown, and equivalent assertions;
- prose that restates the issue without adding a contract or rationale.

Describe moved behavior as a relocation. Do not present its deletion and
addition as two unrelated changes. If uncertainty remains, retain the relevant
concept or state the uncertainty under reading limits.

## Quote evidence faithfully

- Copy quoted code exactly from the retrieved diff, including `+` and `-` diff
  markers when they help show the transition.
- Never regenerate, normalize, complete, or synthetically abridge quoted code.
- Never insert `...` inside an excerpt unless those characters exist in the
  original diff.
- Introduce every excerpt with its concept, file path, and changed-line location
  when the hunk provides one.
- Prefer a few decisive excerpts over a long pseudo-diff.

## Write the structured reading

Use the user's language. The English headings below define the canonical
structure; translate them when the user's language differs. Include only
relevant sections, in this order:

```markdown
# PR/MR Reading — <title>

## Intent
One to three sentences describing the purpose evidenced by the change.

## Change Map
Previous flow → transformation introduced → new observable effect.

## Important Decisions
Contracts, conditions, algorithms, architecture, lifecycle, migration, or
compatibility decisions.

## Decisive Excerpts
Exact excerpts grouped by concept, with file and changed-line references.

## Tests That Define Behavior
Distinctive scenario, stimulus, and expected result.

## Hidden Mechanical Changes
Categories intentionally omitted and their approximate scope when known.

## Reading Limits
Missing context or interpretations that the available evidence cannot settle.
```

Organize by concept rather than file order. Scale the response to semantic
density, not raw diff size. For large changes, merge the file-group analyses
into one cross-file map instead of emitting separate mini-reports.

Do not add a code-quality verdict, findings section, recommendation list, or
approval decision. End after the structured reading.
