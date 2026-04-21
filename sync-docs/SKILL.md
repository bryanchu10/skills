---
name: sync-docs
description: Updates human-readable project documentation from the current conversation. Use when the user says "sync docs", "update the docs", "document this", or at the end of a feature exploration or investigation session. Also invoked automatically by the sync-all skill.
---

Review this conversation and update the relevant human-readable documentation files with insights worth preserving.

Only include content that meets all three criteria:

1. **Accurate**: Verified against actual code in this conversation, not speculation
2. **Human-relevant**: Explains the "what" and "why" of a feature in plain language, not internal implementation details that only matter for AI code navigation
3. **Durable**: Will still be relevant in the future, not specific to this session's task

Do NOT add:
- Details already visible in the documentation
- Speculation or unverified assumptions
- Internal architecture details that belong in machine-facing docs (e.g. `.claude/rules/`) instead

## How to proceed

1. Identify where the project's human-readable documentation lives (e.g. a `docs/`, `docs-for-personal-use/`, or `wiki/` directory)
2. Ensure `index.md` in that directory follows the skeleton in [references/index-skeleton.md](references/index-skeleton.md)
3. Find the file(s) most relevant to what was discussed in this conversation
4. Update those files — edit existing sections if the content fits, add new sections if needed, remove placeholder markers (like ⚠️) when the information is now known
5. If a design decision with non-obvious rationale was discussed, follow [references/decisions-format.md](references/decisions-format.md)
6. If no existing file fits, create a new one in the appropriate directory
7. Do not rewrite content that hasn't changed

After updating, briefly summarize what was added and why.
