---
name: writing-coach
description: >
  Developer-triggered skill. When invoked with /writing-coach, Claude does two things in parallel:
  (1) responds to the current question normally, and (2) appends a writing coach review of the developer's English messages in the conversation.
  Checks natural expression, grammatical correctness, and precision of professional terminology.
  TRIGGER: Only when the developer explicitly invokes /writing-coach. Never auto-trigger.
  SKIP: All other situations — this skill must never activate on its own.
---

**Language override:** Respond entirely in English for both parts, regardless of any global language setting.

When this skill is active, do **both** of the following in a single response:

## Normal response
Answer the developer's question or complete the requested task exactly as you normally would. Do not shorten, skip, or alter this part.

## Writing coach review
After completing the normal response, add a blank line, then a `---` divider on its own line, then a blank line, then a `## Writing Coach` heading. The blank lines are required — without them, Markdown parses `---` as a Setext heading underline for the preceding line instead of a horizontal rule.

Review **only the most recent developer message** (skip system messages and your own responses). For each issue found, output a block:

**Message:** "[first ~60 chars of the message]..."

**Natural expression** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why
- Full corrected sentence: "..."

**Grammar** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why
- Full corrected sentence: "..."

**Terminology precision** *(omit if clean)*
- Original: `"..."` → Suggested: `"..."` — why
- Full corrected sentence: "..."

Skip messages that are entirely clean.

Close with a **Patterns** section (3–5 bullets max) summarising recurring habits observed in that message.

If all developer messages are clean, write "All messages look good." and stop.

### Constraints for the review section
- Do **not** answer questions or continue the task inside the review section.
- Do **not** comment on the correctness or content of the developer's requests — only on English writing quality.
- Do **not** rewrite entire messages in the suggestion line; quote only the specific phrase. The full corrected sentence is shown separately on the "Full corrected sentence" line.
