---
name: draft-commit
description: Analyzes staged git changes and generates a commit message without committing. Accepts an optional language argument: "zh" for Traditional Chinese (Taiwan), defaults to English. Usage: /draft-commit [zh]
---

Run `git diff --staged` and `git diff --staged --stat` to see all staged changes.

Check if the user passed "zh" as an argument:
- **No argument** → write the commit message in **English**
- **"zh"** → write the commit message in **Traditional Chinese (Taiwan)**, using Taiwan-standard terms (e.g. 新增 not 添加, 修正 not 修复)

Based on the staged diff, draft a commit message:
- First line: short imperative summary (≤72 chars)
- Blank line
- Body (if needed): explain *why*, not *what* — the diff already shows what changed

After drafting the message, run `printf '%s' "<commit message>" | pbcopy` to copy it to the clipboard automatically. Then tell the user the message is ready and already copied.

Also display the commit message in a code block.

**Do NOT run `git commit`.**

If there are no staged changes, tell the user and suggest running `git add` first.
