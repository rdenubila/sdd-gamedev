# AGENT.md — Operating Manual

> **Read this file at the start of EVERY session and before EVERY implementation.**
> It defines *how* we work in this project: a **Spec-Driven Development (SDD)**
> flow with versioned artifacts and explicit human approval gates. It does not
> describe *what* to build — that lives in `specs/`.
>
> Project knowledge (architecture, guardrails, conventions, recipes, pitfalls)
> lives in **`specs/constitution.md`**. This file only covers the process.

---

## 1. Session start checklist

1. Read this file.
2. Read `specs/constitution.md`.
3. Read `specs/index.md` to see which features exist and their status.
4. If resuming a feature, open its `tasks.md` — the **Progress summary** tells you
   exactly where the last session stopped.
5. Only then act on the request.

---

## 2. The SDD flow — 6 stages

Every stage has an input, an output and a **gate**. You do not move to the next
stage before the current one is done. **Every gate requires explicit approval
from the human** ("the owner") — at the end of `specify`, `plan` and `task` the
AI presents the artifact and **stops**, waiting for an "ok". `implement` never
starts without an approved `plan` **and** approved `tasks`.

| # | Stage | Question | Artifact | Gate to advance |
|---|---|---|---|---|
| 0 | **constitution** | What are the project's standards? | `specs/constitution.md` | Always updated *before* planning anything it affects |
| 1 | **specify** | WHAT and WHY? | `spec.md` of the feature | Behavior and acceptance criteria are clear, with no implementation detail |
| 2 | **plan** | HOW, given the current state? | `plan.md` of the feature | Plan is **complete and reviewed** (template checklist closed) |
| 3 | **task** | Which smaller steps? | `tasks.md` of the feature | Plan split into small, sequential, verifiable tasks |
| 4 | **implement** | Build it | code/config changes + progress in `tasks.md` | Task builds/compiles/lints and is marked done |
| 5 | **validate** | Does it really work? | test steps inside `tasks.md` | Static validation (AI) + manual test (human) approved |

### 0 · constitution
The project standard: architecture, guardrails, performance/quality decisions,
conventions and the technical knowledge base. **It must be updated whenever a
related change happens** — a new architectural decision, a new convention, a new
tooling limitation discovered. It is the first thing to read before planning.

### 1 · specify
Describes the feature in terms of **observable behavior**: what happens, for
whom, and why. **No implementation detail** (no file names, functions, schemas or
libraries here). Uses `specs/templates/spec-template.md`.

### 2 · plan
Plans the execution considering the **real current state** of the project.

> **Rule:** before planning, inspect the real state of the system using
> *read-only* means — read the code, run queries, list files, use your tooling's
> inspection commands. The plan is born anchored in what actually exists, not in
> assumptions.

The plan details **how** it will be implemented: files/modules affected, data
structures, interfaces, dependencies, risks and known tooling limits.

It is **iterative**: refine the plan in conversation. **Only move to `task` once
the plan is complete** (template checklist closed). Uses
`specs/templates/plan-template.md`.

### 3 · task
Splits the plan into smaller, sequential, independently verifiable tasks. Each
task must be small enough to build and be tested in isolation. **Every task
declares its dependencies** (`Depends on:` field) and the file carries a
**Dependency graph** in the Progress summary, making explicit what can start now
and what waits on another task. Uses `specs/templates/tasks-template.md`.

### 4 · implement
Implement the tasks one by one. **After each task, update `tasks.md`** with:

- the current stage/task,
- what is already implemented,
- what is still missing.

This way any future session (human or AI) resumes exactly where you stopped.

### 5 · validate
Validation has two parts, both described in `tasks.md` for each task:

- **Static validation (AI does it):** build/compile/lint/type-check, run automated
  tests, re-read the changed artifacts to confirm they match the plan, check the
  tool output for errors and warnings.
- **Manual test (human does it):** concrete steps — what to do, what to observe
  and the pass criteria — written per task. Use this for anything an AI cannot
  judge alone (look and feel, gameplay, UX, real devices, production-like data).

---

## 3. Map of the `specs/` folder

```
specs/
├─ constitution.md          # standards, guardrails and technical knowledge base
├─ index.md                 # registry of all features + status
├─ templates/
│  ├─ spec-template.md
│  ├─ plan-template.md
│  └─ tasks-template.md
└─ NNN-feature-name/        # one subfolder per feature
   ├─ spec.md
   ├─ plan.md
   └─ tasks.md
```

### Feature naming convention
`NNN-kebab-case-name`, with `NNN` a sequential three-digit number.
Examples: `001-user-login`, `002-export-to-csv`.

Every new feature **must** be registered in `specs/index.md` (and the "Next ID"
there bumped).

### Lightweight path for small changes
Small fixes still go through the flow — but the artifacts can be one short
paragraph each. What is never skipped: the spec's acceptance criteria, an
approved plan, and a test instruction per task.

---

## 4. Agent operating rules

1. **Always read `specs/constitution.md` before planning or implementing.**
2. **Never skip stages.** No jumping from `specify` straight to `implement`. If
   the plan does not exist or is incomplete, stop and plan.
3. **Every stage needs explicit approval from the owner to advance.** At the end of
   `specify`, `plan` and `task` the AI presents the artifact and **stops**,
   waiting for the "ok". No `implement` without an approved `plan` **and**
   `tasks`. Approval must be explicit — silence or no answer is **not** approval.
4. **The plan is anchored in reality:** inspect the current state with read-only
   tools before writing the plan, and record what you found in the plan.
5. **The plan is iterative:** only advance to `task` when the planning checklist
   is closed and reviewed.
6. **Update progress in `tasks.md` after every task** (current stage, done,
   pending).
7. **Every task has test instructions** (static + manual) before it can be
   considered done.
8. **Every task declares dependencies** (`Depends on:`) and `tasks.md` maintains a
   dependency graph. A task only enters `doing` when all its dependencies are
   `done`.
9. **Update `constitution.md`** whenever an architecture decision, convention or
   tooling limitation changes, and add a row to its change history. Also note it
   in `index.md` if the change was born from a feature. A feature that changes a
   standard is only *done* after the constitution is updated.
10. **Re-read before you write.** After operations that change structure (renames,
    schema changes, regenerated IDs, migrations), re-inspect the state before
    building on top of it.
11. **Stay in scope.** If you find something out of scope, note it in the spec's
    "Open questions" or propose a new feature — do not silently expand the work.
12. **Record discoveries.** Workarounds, gotchas and "why it did not work"
    findings go into the constitution (Part 2) the moment you learn them.

---

## 5. Project environment

> Fill this in when you adopt the template.

| Item | Value |
|---|---|
| Stack / runtime | _e.g. Node 22 + TypeScript_ |
| Repository | _URL or path_ |
| How to build | _e.g. `npm run build`_ |
| How to test | _e.g. `npm test`_ |
| Tooling / MCP servers | _e.g. none, or a list_ |
| Version control | _e.g. Git, trunk-based_ |
| Home of the SDD flow | `specs/` (inside the repo) |

> Platform targets, performance/quality budgets and all technical recipes belong
> in `specs/constitution.md`.

---

## 6. Communication

> **Caveman mode is ON for this project.** Apply it to every interaction. To turn
> it off or change it, edit or delete this section.

- Talk like caveman.
- No preamble.
- No polite fluff.
- Minimal words.
- Output only exact code, terminal commands, or brief errors.
- Save 75% output tokens.

> Exception: the SDD artifacts (`spec.md`, `plan.md`, `tasks.md`, constitution)
> are documents, not chat. Write them in clear, complete English. Caveman mode
> applies to conversation only.
