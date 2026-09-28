# SDD Template — Spec-Driven Development with AI agents

A ready-to-use repository template for **Spec-Driven Development (SDD)**: a
lightweight, document-first workflow where every feature goes through
**specify → plan → task → implement → validate**, with an explicit human approval
at each gate.

It was born from a real project (an indie game built with an AI agent driving the
editor through MCP) and generalized so it works for any stack — web apps, APIs,
games, data pipelines, infrastructure code — and any AI coding tool.

---

## Why SDD?

AI agents are fast, but without structure they guess: they skip requirements,
build on assumptions, lose context between sessions and drift from your
standards. SDD fixes that by making the **artifacts the source of truth**, not the
chat:

- **Intent is written down** (the *spec*) before any code exists.
- **Plans are anchored in reality** — the agent inspects the current state first.
- **Work is chopped into small, verifiable tasks** with declared dependencies.
- **You approve each gate**, so the agent never runs ahead of you.
- **Any session can resume** — progress lives in `tasks.md`, knowledge lives in
  the constitution.
- **Lessons are never lost** — every gotcha discovered goes into the constitution.

---

## The flow at a glance

```
 constitution ──► specify ──► plan ──► task ──► implement ──► validate
   (standards)     WHAT/WHY    HOW     STEPS      BUILD         PROVE
                      ▲          ▲       ▲                       │
                      └── you approve each gate (human "ok") ────┘
```

| # | Stage | Answers | Artifact | Gate to advance |
|---|---|---|---|---|
| 0 | **constitution** | What are our standards? | `specs/constitution.md` | Updated before planning anything it affects |
| 1 | **specify** | What and why? | `spec.md` | Behavior + acceptance criteria clear, no implementation detail |
| 2 | **plan** | How, given today's reality? | `plan.md` | Checklist closed, reviewed and approved |
| 3 | **task** | Which small steps? | `tasks.md` | Small, sequential tasks with dependencies, approved |
| 4 | **implement** | Build it | code + progress in `tasks.md` | Task builds and is marked `done` |
| 5 | **validate** | Does it really work? | test steps in `tasks.md` | Static (AI) + manual (human) validation approved |

The single most important rule: **no implementation without an approved plan and
approved tasks.** Silence is not approval.

---

## What is in this repository

```
.
├─ README.md            # this file
├─ AGENT.md             # the operating manual the AI must read every session
├─ CLAUDE.md            # pointer to AGENT.md (Claude Code)
├─ AGENTS.md            # pointer to AGENT.md (Codex, Copilot agents, Cursor, others)
└─ specs/
   ├─ constitution.md   # standards, guardrails, technical knowledge base
   ├─ index.md          # registry of all features + lifecycle status
   ├─ templates/
   │  ├─ spec-template.md
   │  ├─ plan-template.md
   │  └─ tasks-template.md
   └─ 000-example-dark-mode-toggle/   # a filled-in example (delete it later)
      ├─ spec.md
      ├─ plan.md
      └─ tasks.md
```

- **`AGENT.md`** — *how we work.* The process, the rules, the session-start
  checklist. Project-independent; you rarely edit it except the environment table
  and the optional communication section.
- **`specs/constitution.md`** — *what we know.* Your constraints, budgets,
  conventions, architecture decisions and hard-won technical lessons. Grows with
  the project.
- **`specs/index.md`** — *what exists.* One row per feature with its status.
- **`specs/templates/`** — the skeletons for the three per-feature documents.
- **`specs/NNN-feature-name/`** — one folder per feature with `spec.md`,
  `plan.md`, `tasks.md`.

---

## Quick start

1. **Use the template.** Copy this folder into your project root (or click
   "Use this template" if you host it on GitHub). If your project already has an
   `AGENT.md`, `CLAUDE.md` or `AGENTS.md`, merge the "read AGENT.md first" line
   into it.
2. **Fill in the environment table** in `AGENT.md` (section 5): stack, how to
   build, how to test, tooling.
3. **Write your first constitution.** Open `specs/constitution.md` and replace the
   placeholders — at minimum: the non-negotiable constraints (1.1), the quality
   budget (1.2) and the conventions (1.4). Keep it short at first; it grows.
4. **Delete the example.** Remove `specs/000-example-dark-mode-toggle/` and its row
   in `specs/index.md` (read it first — it shows what "good" looks like).
5. **Start your first feature** with the prompts below.

---

## Working a feature, step by step

### 1. Create the folder
Pick the next ID from `specs/index.md` and create
`specs/001-your-feature/`. Copy the three templates into it (`spec.md`,
`plan.md`, `tasks.md`), fill the front matter, and add a row to `index.md`
(status `draft`). Bump "Next ID".

### 2. Specify
Write *what* and *why* in observable terms: goal, scope (in/out), scenarios,
numbered functional requirements (`FR-01…`) and testable acceptance criteria
(`AC-01…`). **No files, functions or libraries** — if you catch yourself naming
them, it belongs in the plan.
→ The AI presents the spec and **stops**. You review, iterate and say "approved".
Status becomes `specified`.

### 3. Plan
The AI first **inspects the real current state** (reads code, lists files, runs
queries), records it in section 1, then proposes the approach, the components
touched, the data/interfaces, the detailed design, the risks and the manual steps
only a human can do. It is iterative — you and the AI refine it until the
**planning checklist (section 8)** is fully checked and you approve. Status
becomes `planned`.

### 4. Task
The plan is split into small, sequential tasks. Each one has:

- a **goal** tied to `FR-xx` / `AC-xx`,
- ordered **implementation steps**,
- a **static validation** (what the AI verifies: build, lint, tests, re-reading),
- a **manual test** (what a human does, expects and accepts),
- a **`Depends on:`** field, plus a **dependency graph** at the top of the file.

You approve the task list. Status becomes `tasked`.

### 5. Implement
The AI works task by task. A task moves to `doing` only when its dependencies are
`done`. **After every task** the AI updates that task's status and progress and
the *Progress summary*. Status becomes `implementing`.

### 6. Validate
For each task: the AI runs its static validation and reports; you run its manual
test and approve. When every task is `done`, the feature becomes `validating`
and finally `done` after your sign-off.

### 7. Close the loop
Update `constitution.md` with anything the feature changed or taught (new
decision, convention, gotcha, verified flow) and add a row to its change
history. A feature that changes a standard is not finished until the
constitution says so.

---

## Prompts cheat sheet

Copy, adapt, paste. (The AI reads `AGENT.md` automatically if your tool loads
`CLAUDE.md` / `AGENTS.md`; otherwise start with "Read AGENT.md".)

| Moment | Prompt |
|---|---|
| Start a session | `Read AGENT.md, specs/constitution.md and specs/index.md. Then tell me the status of feature 003.` |
| New feature | `New feature: <idea>. Create the next spec folder and run the specify stage. Stop for my approval.` |
| Plan | `Spec 003 is approved. Inspect the current state, then write plan.md. Stop for my approval.` |
| Tasks | `Plan 003 is approved. Break it into tasks with dependencies. Stop for my approval.` |
| Implement | `Tasks 003 are approved. Implement the next available task, validate it statically, update tasks.md, then stop.` |
| Resume | `Resume feature 003 from the Progress summary in tasks.md.` |
| Close | `Feature 003 is validated. Update the constitution and index, then mark it done.` |
| Small fix | `Bug: <description>. Use the lightweight SDD path (one-paragraph spec, short plan, one task). Stop at each gate.` |

---

## Using it with different AI tools

The process is tool-agnostic — it is just Markdown. Point your tool at
`AGENT.md`:

- **Claude Code** — reads `CLAUDE.md` (included), which points to `AGENT.md`.
  Optionally turn the prompts above into custom slash commands
  (`.claude/commands/`).
- **Codex / GitHub Copilot agents / other `AGENTS.md`-aware tools** — read
  `AGENTS.md` (included).
- **Cursor** — add a rule (`.cursor/rules/`) that says "always read `AGENT.md`
  first", or copy its content into a rule file.
- **Anything else** — paste "Read `AGENT.md`, then `specs/constitution.md`" at the
  start of the session.

If your tooling exposes a way to inspect the running system (an MCP server, a CLI,
a database console, a debugger), mention it in `AGENT.md` section 5 and in
constitution 1.3, and require it in the plan stage: *"inspect before you plan."*

---

## Customizing the template

Most of it is meant to be used as-is. The places you are expected to adapt:

| File | What to change |
|---|---|
| `AGENT.md` §5 | Environment table: stack, build, test, tooling |
| `AGENT.md` §6 | How the AI should talk to you. Ships with **caveman mode** on (terse replies, minimal tokens); edit or delete to change |
| `constitution.md` | Everything with a `_placeholder_` — this is *your* project |
| `templates/*.md` | Add domain-specific sections (e.g. "Security review", "Migration plan", "Performance measurement") |
| `index.md` | Add statuses if your team needs them |

Ideas by domain:

- **Web / backend:** add "API contract" and "Migration plan" sections to the plan
  template; put SLOs in constitution 1.2.
- **Games:** put frame-time budgets per target platform in 1.2; make the manual
  test explicitly "play the build"; record engine/tool quirks in Part 2.
- **Data / ML:** add "Dataset and evaluation" to the spec, and metrics
  thresholds to the acceptance criteria.
- **Infrastructure:** make static validation include `plan`/`diff` output and
  policy checks; make the manual test a staged rollout.

---

## Tips and FAQ

**Is this too heavy for small changes?**
Use the lightweight path (see `AGENT.md` §3): each document can be a few lines.
Just never skip the acceptance criteria, the approved plan, or the test
instructions.

**Can the AI edit the constitution by itself?**
It should *propose* and write updates when a decision or lesson emerges, and you
review the diff. The constitution is the team's memory — treat changes like code
review.

**What if the plan turns out to be wrong mid-implementation?**
Stop, set the task to `blocked`, update `plan.md` (log it in section 9) and get
the change approved before continuing. Do not silently improvise.

**How big should a task be?**
Small enough to build and test in isolation — usually something you could review
in one sitting. If the static validation needs three unrelated checks, split it.

**Where do bugs found during validation go?**
Small ones: a new task in the same `tasks.md`. Large or out-of-scope ones: a new
feature folder, referenced from the spec's "Open questions".

**Should spec files be committed?**
Yes. `specs/` is versioned with the code so history, review and rollbacks cover
intent as well as implementation.

---

## Feature lifecycle

```
draft → specified → planned → tasked → implementing → validating → done
                                                          (paused at any point)
```

See `specs/index.md` for the meaning of each status.

---

## License

Use it, fork it, adapt it. Add a license file (for example MIT) that suits how
you want to share your version.
