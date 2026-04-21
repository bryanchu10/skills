# decisions/ Directory

If a `decisions/` directory exists in the docs folder, add or update a decision record when:

- A design choice has a non-obvious reason (why A was chosen over B)
- A constraint or trade-off was intentional (a conscious choice, not tech debt)

Do not create `decisions/` automatically — only when the user explicitly wants to record a decision.

## Record format

```markdown
# {Decision title}

**Decision:** One sentence stating what was chosen.

**Context:** What problem or constraint prompted this decision.

**Options considered:** What alternatives were evaluated.

**Rationale:** Why this option was chosen and what trade-offs were accepted.
```
