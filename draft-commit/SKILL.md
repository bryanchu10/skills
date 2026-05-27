---
name: draft-commit
description: Analyzes staged git changes and generates a commit message without committing. Accepts optional arguments: "zh" for Traditional Chinese (Taiwan), "cc" for Conventional Commits style. Arguments can be combined in any order. Usage: /draft-commit [zh] [cc]
---

First, run `git rev-parse --show-toplevel` to get the git root of the current working directory. Use this path as `<repo-root>` for all subsequent git commands.

Run `git -C <repo-root> diff --staged` and `git -C <repo-root> diff --staged --stat` to see all staged changes.

Check the arguments passed by the user:

**Language** (check for "zh"):
- **No "zh"** → write the commit message in **English**
- **"zh"** → write the commit message in **Traditional Chinese (Taiwan)**, using Taiwan-standard terms (e.g. 新增 not 添加, 修正 not 修复)

**Style** (check for "cc"):
- **No "cc"** → plain imperative style. Example:
  ```
  Add retry logic for flaky network calls

  - Retry up to 3 times with exponential backoff
  - Surface final error to caller unchanged
  ```
- **"cc"** → Conventional Commits style (`type(scope): description`). Use the most appropriate type: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`, `ci`. Omit scope if it's not meaningful. Example:
  ```
  feat(network): add retry logic for flaky calls

  - Retry up to 3 times with exponential backoff
  - Surface final error to caller unchanged
  ```

Based on the staged diff, draft a commit message:
- First line: short summary (≤72 chars), in the style determined above
- Blank line
- Body (if needed): bullet list explaining *why*, not *what* — the diff already shows what changed

After drafting the message, run `printf '%s' "<commit message>" | pbcopy` to copy it to the clipboard automatically. Then tell the user the message is ready and already copied.

Also display the commit message in a code block.

**Do NOT run `git commit`.**

If there are no staged changes, tell the user and suggest running `git add` first (in the correct `<repo-root>`).
