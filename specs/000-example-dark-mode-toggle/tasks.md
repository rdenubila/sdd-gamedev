---
id: 000
feature: example-dark-mode-toggle
stage: implement
status: implementing
created: 2026-01-02
updated: 2026-01-03
---

# Tasks — Dark mode toggle (EXAMPLE)

## Status legend
`todo` · `doing` · `blocked` · `done`

---

## Progress summary
- **Current stage:** implementing — Task 002
- **Completed:** 1 / 3
- **Already implemented:** dark token values added and contrast-checked (T001).
- **Still missing:** theme helper + pre-paint script (T002), header toggle (T003).

### Dependency graph
- T001 <- — (can start now)
- T002 <- T001
- T003 <- T002

---

## Task 001 — Add dark token values
**Status:** done
**Depends on:** —

**Goal:** Provide dark values for all 24 color variables (supports FR-02, AC-04).

**Implementation steps:**
1. In `src/styles/tokens.css`, add a `[data-theme="dark"]` block overriding every variable.
2. Keep variable names unchanged.

**Static validation (AI):**
- `npm run build` passes with 0 errors.
- A script lists all variables in `:root` and confirms each has a dark override.
- Contrast script reports ≥ 4.5:1 for every text/background pair.

**Manual test (human):**
- Steps: in devtools, set `<html data-theme="dark">`.
- Expected: every screen is readable and consistent.
- Pass criterion: no unreadable text, no white patches.

**Progress:**
- 2026-01-03: Done. 24/24 overrides; lowest contrast 4.8:1. Owner approved.

---

## Task 002 — Theme helper and pre-paint script
**Status:** doing
**Depends on:** T001

**Goal:** Persist and restore the theme without a flash (FR-03, FR-04, AC-02, AC-03).

**Implementation steps:**
1. Create `src/theme/theme.ts` with `getTheme()` / `setTheme()` (try/catch around storage).
2. Add the inline script to `index.html` that sets `data-theme` before first paint.

**Static validation (AI):**
- Build and unit tests pass (`theme.test.ts` covers stored value, system fallback,
  storage failure).

**Manual test (human):**
- Steps: (a) fresh profile, system dark → open app; (b) set light, reload.
- Expected: (a) opens dark; (b) stays light, no flash.
- Pass criterion: both behave as expected in Chrome and Firefox.

**Progress:**
- 2026-01-03: `theme.ts` written and tests passing; inline script still to do.

---

## Task 003 — Header toggle
**Status:** todo
**Depends on:** T002

**Goal:** Visible toggle in the header (FR-01, AC-01).

**Implementation steps:**
1. Add a button in `Header.tsx` with `aria-pressed`, wired to `setTheme`.

**Static validation (AI):**
- Build, lint and component test pass.

**Manual test (human):**
- Steps: click the toggle on three different screens.
- Expected: immediate switch, no reload.
- Pass criterion: switch feels instant (< 100 ms) on every screen.

**Progress:**
- _—_
