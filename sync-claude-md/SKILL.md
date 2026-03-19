---
name: sync-claude-md
description: Reviews the current conversation, identifies insights worth documenting, and updates CLAUDE.md. Use at the end of an analysis session.
---

Review this conversation and update CLAUDE.md with insights worth preserving. Only include content that meets all three criteria:

1. **Non-obvious**: Cannot be directly read from the code in one glance
2. **Cross-cutting**: Involves relationships between files, or explains why something works a certain way
3. **Durable**: Will still be relevant in future conversations, not specific to this session's task

Do NOT add:
- Details already visible in the code
- Speculation or unverified assumptions
- Content already covered in CLAUDE.md

Update CLAUDE.md by editing existing sections if the new content belongs there, or adding new sections if needed. Do not rewrite content that hasn't changed.

After updating, briefly summarize what was added and why it met the criteria.
