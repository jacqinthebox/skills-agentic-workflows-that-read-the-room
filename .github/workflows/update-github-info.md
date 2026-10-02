---
name: update-github-info
description: Review recent official GitHub updates and propose a focused update to Mona's GitHub Info content.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
engine: codex
network:
  allowed:
    - github.blog
    - github.com
    - api.github.com
tools:
  edit:
  web-fetch:
  github:
    mode: gh-proxy
    toolsets: [repos, pull_requests]
    allowed-repos: ${{ github.repository }}
    min-integrity: approved
safe-outputs:
  noop:
  create-pull-request:
    title-prefix: "[github-info] "
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

## Task

1. Read `notes/mona-notes.md` and `site/content/github-info.md` before making decisions.
2. Fetch `https://github.blog/latest/` and `https://github.blog/changelog/` with the web-fetch tool. Consider only recent, verifiable items relevant to Mona's practical GitHub guidance.
3. Update `site/content/github-info.md` only when the sources support a concise, useful change. Keep the existing editorial angle, avoid repeating existing themes, and cite the relevant GitHub Blog or Changelog source in the content.
4. Before proposing a change, check open pull requests for an existing `[github-info]` update. If one is open, or no meaningful update is supported, leave the file unchanged and call `noop` with a brief reason.
5. When there is a meaningful change and no matching open pull request, use `create-pull-request` to propose only `site/content/github-info.md`. Explain the evidence for the update in the PR description and ask Mona to review it. Never write directly to the default branch.
