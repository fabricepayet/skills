# CLI access

Use these commands when the matching CLI is available and authorized.
For a numeric target, resolve the repository with `git remote get-url origin`.
For a different repository, pass `-R <repository>`.

## GitHub

```bash
gh pr view <number-or-url> --json number,title,body,baseRefName,headRefName,author,files,additions,deletions,url
gh pr diff <number-or-url> --color=never
```

## GitLab

For `https://<host>/<group>/<project>/-/merge_requests/<iid>`, use the full
repository URL with `-R` so self-hosted targets retain their host.

```bash
glab mr view <iid-or-branch> --output json -R <repository-url>
glab mr diff <iid-or-branch> --raw --color=never -R <repository-url>
```

If CLI authentication fails, report `gh auth login` or `glab auth login` for
the relevant host. An authorized connector or supplied diff may still provide
the evidence. If no source is available, ask for access or the diff; do not
install software merely to perform the explanation.
