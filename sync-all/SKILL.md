---
name: sync-all
description: Updates both the machine-facing CLAUDE.md and the human-readable project documentation in one step. Use at the end of any investigation or feature exploration session.
---

Run both documentation sync skills in sequence:

1. First, run the `sync-claude-md` skill to update CLAUDE.md with architecture insights for Claude
2. Then, run the `sync-docs` skill to update the human-readable project documentation

Each skill has its own criteria for what to include — apply them independently. Do not let the output of one influence the scope of the other.
