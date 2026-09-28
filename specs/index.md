# index.md — Feature Registry

> Central registry of all features in the SDD flow. Each feature has a subfolder
> `NNN-kebab-case-name/` with `spec.md`, `plan.md` and `tasks.md`. Update this
> file when you create a new feature and whenever a feature's status changes.

## Feature lifecycle

```
draft → specified → planned → tasked → implementing → validating → done
```

| Status | Meaning |
|---|---|
| `draft` | Folder created, spec still incomplete |
| `specified` | `spec.md` closed (what/why + acceptance criteria), approved |
| `planned` | `plan.md` complete and reviewed (gate closed), approved |
| `tasked` | `tasks.md` generated from the plan, approved |
| `implementing` | Tasks being built |
| `validating` | Implementation done; in static validation + manual test |
| `done` | Validated and approved |
| `paused` | Work suspended (write the reason in `plan.md`) |

## Features

| ID | Feature | Status | Last update |
|---|---|---|---|
| 000 | [example-dark-mode-toggle](./000-example-dark-mode-toggle/) — **EXAMPLE, delete when you start** | implementing | 2026-01-01 |

<!--
When adding a feature, use a row like this:
| 001 | [user-login](./001-user-login/) | specified | 2026-01-15 |
-->

## Next ID

`001`
