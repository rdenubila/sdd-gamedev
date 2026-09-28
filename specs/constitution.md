# constitution.md — Project Standards

> **Source of truth for the project's standards, guardrails and technical
> knowledge.** Read it before planning or implementing any feature (see
> `AGENT.md`).
>
> **Maintenance rule:** this file must be **updated whenever a related change
> happens** — a new architecture decision, a new convention, a newly consolidated
> recipe or a newly discovered limitation. A feature that changes a standard
> written here is only considered done after this constitution is updated.
>
> Structure: **Part 1** holds the non-negotiable principles. **Part 2** is the
> technical/operational knowledge base (recipes, tooling quirks, verified flows,
> pitfalls).
>
> **How to adopt this template:** replace every `_italic placeholder_` with your
> project's reality and delete the hint blocks (`> Hint: ...`). Keep the
> structure — the numbering is referenced from the plan and tasks templates.

---

# PART 1 — Principles and Guardrails

## 1.1 Non-negotiable constraints

> Hint: the rules that, if broken, make a change wrong regardless of how well it
> works. Keep it short (3–8 items). Examples: "No new runtime dependencies without
> approval", "All public APIs are backwards compatible", "Blueprint-only — no
> C++", "Accessibility level AA is mandatory", "Never store PII in logs".

- _Constraint 1 — and why it exists._
- _Constraint 2._
- _Constraint 3._

## 1.2 Quality and performance budget

> Hint: measurable limits every decision is weighed against. Examples: "p95 API
> latency < 200 ms", "bundle size < 250 kB", "60 fps on the lowest target
> device", "test coverage of new code ≥ 80%".

| Budget | Target | How it is measured |
|---|---|---|
| _e.g. Response time_ | _e.g. p95 < 200 ms_ | _e.g. load test script_ |

## 1.3 Tooling guardrails and known limits

> Hint: things your tools (build system, framework, MCP servers, CI, the AI agent
> itself) cannot do or do badly. When planning, treat these as design constraints:
> if a plan depends on something the tooling cannot do, the plan must include
> the manual step for the human.

- _Limitation → workaround._

## 1.4 Conventions

> Hint: naming, folder structure, code style, commit messages, branch names,
> asset/module conventions. Anything you would otherwise repeat in code review.

- **Names:** _..._
- **Folders:** _..._
- **Code style:** _..._
- **SDD feature names:** `NNN-kebab-case-name`.

## 1.5 Architecture decisions

> Hint: lightweight decision log. One row per decision that shapes future work.
> Link the feature that produced it.

| Date | Decision | Why | Alternatives rejected | Origin |
|---|---|---|---|---|
| _YYYY-MM-DD_ | _..._ | _..._ | _..._ | _feature / discussion_ |

---

# PART 2 — Technical Knowledge Base

> Operational lessons and recipes consolidated from real sessions. Keep entries
> short, concrete and reproducible. Literal names (functions, flags, node/pin
> names, commands) stay in their original form.

## 2.1 Map of key components

> Hint: the "where is what" of the project — the components a newcomer (or a
> fresh AI session) must know about, with their paths and one-line
> responsibilities. Mark deprecated ones explicitly so nobody builds on them.

| Component | Path | Responsibility | Status |
|---|---|---|---|
| _e.g. AuthService_ | _e.g. `src/auth/service.ts`_ | _Issues and validates sessions_ | _canonical / deprecated_ |

## 2.2 Operational lessons

> Hint: one subsection per lesson. Use the recipe format below. This is the most
> valuable part of the file over time — it stops the same mistake from being
> made twice.

### _Lesson title_ (template — copy for each lesson)

- **Context:** _when this comes up._
- **Problem / symptom:** _what goes wrong, with the exact error if any._
- **Solution:** _the recipe that works, exact commands/snippets._
- **Why:** _root cause, if known._
- **Origin:** _feature / date._

## 2.3 Verified flows

> Hint: end-to-end flows that were implemented **and** verified, described step by
> step (inputs → steps → outputs). Future plans can reuse them as building
> blocks.

### _Flow name_

1. _Step_
2. _Step_

## 2.4 Common pitfalls

> Hint: short "do not do this" list, each with the reason.

- _Pitfall — reason._

## 2.5 Latest verified builds

> Hint: the last known-good build/test/compile result per area, to make
> regressions easy to spot. Example: `2026-01-10 — backend: 212 tests, 0 failures`.

- _—_

---

## Change history

| Date | Change | Origin |
|---|---|---|
| _YYYY-MM-DD_ | Constitution created; SDD flow adopted | Adoption of Spec-Driven Development |
