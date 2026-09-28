---
id: NNN
feature: kebab-case-name
stage: plan
status: draft            # draft | planned
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Plan — <Feature Name>

> **Stage: plan.** How to implement it, given the **real current state** of the
> project. This is an **iterative** process — only advance to `task` when the
> checklist in section 8 is closed. Read `constitution.md` before deciding on
> architecture.
>
> **Before writing this plan**, inspect the current state using read-only means
> (read the code, list files, query the system, use your tooling's inspection
> commands) and record what you found in section 1.

## 1. Current state (inspected)
_What already exists and is relevant: modules, functions, data, configuration.
Cite what you actually read so the plan is anchored in reality._

-

## 2. Technical approach
_The chosen strategy and why. Alternatives considered and discarded. Alignment
with the constitution's principles (constraints, budgets, conventions)._

-

## 3. Components affected / created
| Component | Path | Create or change? | What |
|---|---|---|---|
| | | | |

## 4. Data and interfaces
_Data structures, variables, schemas, API contracts, configuration keys —
name, type, default, notes. Mark anything the tooling cannot create and that
needs a manual step by the human._

| Component | Name | Type | Default | Notes |
|---|---|---|---|---|
| | | | | |

## 5. Detailed design (per unit)
_One block per function/module/graph/endpoint. Describe the logic and the wiring
between pieces precisely enough that implementing it is mechanical._

### <Unit A>
- Logic:
- Inputs / outputs:
- Calls / connections:

## 6. Assets, dependencies and configuration needed
_Libraries, assets, environment variables, permissions, migrations, etc. that
must exist._

-

## 7. Risks and tooling limits
_Known limitations that affect this plan (see constitution 1.3 / Part 2) and
the manual step for the human when the tooling cannot do it._

-

## 8. Planning-complete checklist (gate to `task`)
- [ ] Current state inspected and recorded
- [ ] Approach aligned with the constitution
- [ ] All components / data / logic detailed
- [ ] Assets, dependencies and configuration identified
- [ ] Risks and manual human steps mapped
- [ ] Reviewed and approved by the owner

## 9. Notes / decisions (iterative)
_Log of planning iterations and decisions made._

- YYYY-MM-DD:
