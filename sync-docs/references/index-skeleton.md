# index.md Skeleton

The `index.md` inside the human-readable documentation directory (e.g. `docs-for-personal-use/index.md`, not the project root) must maintain the following seven sections in this fixed order. The consistent structure lets anyone familiar with this convention orient themselves in a new project at the same pace.

```markdown
# {Project name}

{One-sentence positioning: what this system is, who uses it, its core value}

## Getting started

Minimal steps to run the service locally. Keep it short — link to a dedicated getting-started file for details. Only include commands and non-obvious prerequisites here.

## Tech stack

Frameworks, languages, databases, and non-obvious infrastructure (e.g. why Redis is required).
This section is **Reference**: list *what is used*, not *why it was designed this way*.

## External services

Backend APIs and third-party services, one per line with a brief description of purpose.
(Omit this section if there are no external dependencies.)

## How it works

Explain key data flows, module boundaries, or non-obvious design behavior.
This section is **Explanation**: describe *why things work this way*, not repeat the technology list.
Use subheadings one level deeper than the section heading — one per topic — so each concept is independently navigable.

## Reading map

For someone entering this project for the first time: which files to read and in what order.
Use an ordered list or a simple table. One sentence per file describing what question it answers.

## Known issues / In progress

(Optional) Confirmed bugs that won't be fixed soon, and ideas not yet implemented.
```
