---
name: writing-coach
description: >
  Developer-triggered skill. When invoked with /writing-coach, Claude does two things in parallel:
  (1) responds to the current question normally, and (2) appends a writing coach review of the developer's English messages in the conversation.
  Checks natural expression, grammatical correctness, and precision of professional terminology.
  TRIGGER: Only when the developer explicitly invokes /writing-coach. Never auto-trigger.
  SKIP: All other situations — this skill must never activate on its own.
---

When this skill is active, do **both** of the following in a single response:

## Part 1 — Normal response
Answer the developer's question or complete the requested task exactly as you normally would. Do not shorten, skip, or alter this part.

## Part 2 — Writing coach review
After completing the normal response, add a `---` divider, then a `## Writing Coach` section.

Review **all developer messages visible in this conversation** (skip system messages and your own responses). For each message that has at least one issue, output a block:

**Message:** "[first ~60 chars of the message]..."

**Natural expression** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why

**Grammar** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why

**Terminology precision** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why

Skip messages that are entirely clean.

Close with a **Patterns** section (3–5 bullets max) summarising recurring habits across the whole conversation.

If all developer messages are clean, write "All messages look good." and stop.

### Constraints for the review section
- Do **not** answer questions or continue the task inside the review section.
- Do **not** comment on the correctness or content of the developer's requests — only on English writing quality.
- Do **not** rewrite entire messages; quote only the specific phrase that needs fixing.
