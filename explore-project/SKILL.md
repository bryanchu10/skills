---
name: explore-project
description: Systematically explore an unfamiliar codebase, build both machine-facing (CLAUDE.md) and human-readable documentation, and fill in unknowns from code before asking the user. Use when onboarding to a new project.
---

Systematically explore this codebase and build up documentation as you go. Follow these phases in order.

---

## Phase 1: Build the map

Read the directory structure, `package.json`, key config files, and any existing README or docs. Identify:
- Tech stack and framework
- Top-level directory layout and what each directory is for
- Entry points (pages, routes, or app shell)
- How the app is deployed

Do not deep-dive into any single feature yet. The goal is a complete map, not depth.

---

## Phase 2: Set up CLAUDE.md

Check if a `.claude/CLAUDE.md` exists.

- **If it exists**: Read it. Evaluate whether it reflects the whole project or just one part. Update it to be a project-wide overview if needed.
- **If it doesn't exist**: Create one.

CLAUDE.md should cover:
- Tech stack
- Directory structure with annotations
- Routing overview
- API / data-fetching architecture
- Component or module conventions
- State management structure
- Key gotchas — non-obvious decisions that would confuse someone reading the code

Keep CLAUDE.md at the overview level. Create `.claude/rules/` sub-documents for deep dives into specific areas.

---

## Phase 3: Identify knowledge gaps

Based on what you've read, list the areas you don't yet understand:
- Features with complex or non-obvious logic
- Services or APIs whose purpose is unclear
- Patterns that appear repeatedly but haven't been explained

Present this list to the user and ask which areas to prioritize.

---

## Phase 4: Deep-dive by feature area

For each priority area:

1. **Read the code first** — find the relevant files, read them, trace the data flow
2. **Infer before asking** — only ask the user about things genuinely not inferable from code (business rules, external system behavior, historical decisions)
3. **Update `.claude/rules/`** — create or update the relevant rules file with architecture insights
4. **Note what belongs in human docs** — flag content that explains the "what" and "why" for a human reader

---

## Phase 5: Create human-readable documentation

Look for an existing human-readable docs directory. Check for these names in order:
- `docs-for-personal-use/`
- `docs/`
- `wiki/`

If one exists, use it. If none exists, propose a location to the user before creating files.

Within each file:
- Write for someone new to the project
- Explain "what" and "why", not just "how"
- Mark unknowns with ⚠️ rather than guessing
- Fill in ⚠️ items from code before asking the user

---

## Phase 6: Sync

Run `/sync-all` to ensure both CLAUDE.md and the human docs reflect everything learned in this session.

---

## Principles throughout

- **Read code before asking**: If something is inferable from the codebase, infer it. Only ask the user about business context, historical decisions, or external system behavior that cannot be read from code.
- **Map first, depth second**: Avoid going deep on one area before understanding the overall shape of the project.
- **Separate audiences**: Machine-facing docs (`.claude/`) are for navigating code; human-facing docs are for understanding the product.
