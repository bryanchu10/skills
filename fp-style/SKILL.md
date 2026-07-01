---
name: fp-style
description: >
  Reviews TypeScript code for if/else-heavy or imperative control flow and suggests idiomatic
  replacements using ts-pattern (exhaustive matching) and neverthrow (Result/Option), plus
  broader FP practices (immutability, pure functions, composition, newspaper-style top-down
  ordering). Can also be invoked before or during writing/editing code, so Claude follows these
  principles while authoring, not only when reviewing afterward.
  TRIGGER when: user explicitly invokes /fp-style — either (1) to review a diff/file for
  if-else-heavy or imperative patterns, or (2) to have Claude write/edit code following FP style
  with ts-pattern/neverthrow.
  SKIP: automatic or unprompted triggering, and folding into /code-review — this skill is
  manual-only and per-project, since style preference varies across projects.
---

## Purpose

Judge and (when asked) rewrite code against a specific, non-dogmatic bar: does a piece of control
flow make failure/invariants **explicit in the type system**, or does it rely on the reader
remembering an early return / guard clause that happened earlier in the function? Prefer
`ts-pattern` and `neverthrow` idioms only where they close that gap — not because `if` is
inherently bad.

## Step 0 — Detect the project's FP stack (always run first)

Read `package.json` in the current project root:

- Look for `ts-pattern` and `neverthrow` in `dependencies`/`devDependencies`, and note their
  **installed version** (check the lockfile for the resolved version if the range is loose).
- If a library isn't present, do not propose APIs from it — see Non-goals.
- If the resolved version is old enough that its API might differ from the latest docs/training
  knowledge, verify the actual exported API (e.g. by checking `node_modules/<pkg>/package.json`
  and its type definitions) before suggesting code, rather than assuming the newest API.

## Core principles

Apply these only when they close the "forgotten guard" gap described in Purpose — not as a
blanket ban on `if`.

1. **Guard clauses → `ts-pattern` `match().with().otherwise()`** when a guard's early return could
   plausibly be forgotten by code written later in the same function, or when branching is really
   pattern matching on a discriminant (tagged union, string literal, enum).
2. **Thrown exceptions / ad-hoc error flags → `neverthrow` `Result`/`Option`** so failure is part
   of the function's return type, not something the reader has to trace through call sites.
3. **Mutation → immutable data.** Flag in-place mutation, reassigned `let`, or side effects hidden
   inside a function that otherwise looks like a pure transform.
4. **Imperative loops → declarative collection ops** (`map`/`filter`/`reduce`/`find`) when the loop
   is really a data transformation rather than genuinely sequential side effects.
5. **Sequential imperative steps → composition/pipelines** when the sequence represents a data
   pipeline.
6. **Function/module ordering → newspaper style.** Within a file, order functions top-down by
   level of abstraction: the entry point / exported orchestration function first, followed by the
   functions it calls, then *their* helpers, and so on — mirroring a newspaper article (headline
   and lede first, supporting detail after). Flag a file where a low-level helper is defined above
   the higher-level function that uses it, forcing the reader to jump backward to understand the
   flow.

## Known pitfall — ts-pattern handlers don't inherit strict object checks

Converting a guard clause to `match().with().otherwise()` only makes the *outer* discriminant
explicit/exhaustive — it does **not** protect values computed *inside* a handler. TypeScript loses
its excess-property ("fresh literal") check when an object literal is returned from a
generically-inferred callback; this is a known, permanently-unfixed TypeScript limitation
(`microsoft/TypeScript#7547`, closed as "Design Limitation"). So a `.with()`/`.otherwise()` handler
can return an object with a wrong or `undefined` field, and none of the following will catch it:
tightening the outer function's declared return type, adding `.exhaustive()` instead of
`.otherwise()`, or wrapping the literal in `satisfies` at the wrong spot — all three were verified
to still let the mismatch through when the value flows from a generic callback.

When reviewing or authoring a `.with()`/`.otherwise()` handler:
- If the handler looks up something that can be missing (array index, `Map.get`, `Record[key]`,
  `.find()`), narrow it **at the point of lookup** — `match(value).with(undefined, () => fallback)
  .otherwise((narrowed) => ...)` — before embedding it anywhere else in the handler.
- Do not let a possibly-`undefined` value travel from the lookup into a larger returned object
  literal unnarrowed, and do not treat a stricter outer return-type annotation as a substitute for
  narrowing at the lookup site — it will not be enforced there.

## Non-goals (do not do these)

- Do not flag a short, single, side-effect-free `if` with no early return — that's not the failure
  mode this skill targets.
- Do not propose `ts-pattern`/`neverthrow`-based rewrites in a project that doesn't already depend
  on them, unless the user explicitly asks to introduce the library.
- Do not activate without explicit invocation, and do not merge this behavior into `/code-review`.

## Mode 1 — Review

Use when invoked to review existing code (a diff, or a specified file/directory).

1. Scope detection mirrors `/code-review`: review the current git diff if no target is specified,
   otherwise the given file(s).
2. For each violation of the Core principles, record:
   - `file:line`
   - one-sentence description of the pattern found
   - a rationale grounded in *which* principle (1-5) applies and *why* — not generic style
     preference
3. Output a findings list ranked most-impactful first, in the same reporting style as
   `/code-review`.
4. Do not auto-apply fixes unless the user explicitly asks for that in the same invocation.

## Mode 2 — Authoring

Use when invoked alongside a write/edit task (e.g. "write X following fp-style", "refactor Y using
fp-style").

1. Run Step 0 first.
2. Apply the Core principles while writing/editing code in this turn, using only APIs that exist
   in the detected library versions.
3. Briefly note (1-2 sentences) which idioms were applied and why, so the change can be verified
   without re-deriving the reasoning from scratch.
