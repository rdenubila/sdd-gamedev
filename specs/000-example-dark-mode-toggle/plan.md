---
id: 000
feature: example-dark-mode-toggle
stage: plan
status: planned
created: 2026-01-01
updated: 2026-01-02
---

# Plan — Dark mode toggle (EXAMPLE)

## 1. Current state (inspected)
- `src/styles/tokens.css` defines all colors as CSS variables on `:root` (read the
  file: 24 variables, no dark values yet).
- `src/components/Header.tsx` renders the header; no theme code exists.
- No component uses hard-coded hex colors (searched with `grep -rn "#[0-9a-f]\{6\}" src/`
  → only inside `tokens.css`).

## 2. Technical approach
Drive the theme with a `data-theme` attribute on `<html>` and override the CSS
variables under `[data-theme="dark"]`. Persist to `localStorage`; on first visit
read `prefers-color-scheme`. Apply the attribute with a tiny inline script in
`index.html` **before** first paint to avoid a flash (AC-02).

Alternatives discarded: a CSS-in-JS theme provider (adds a dependency, violates
constitution 1.1) and a `prefers-color-scheme`-only approach (no manual override).

## 3. Components affected / created
| Component | Path | Create or change? | What |
|---|---|---|---|
| Tokens | `src/styles/tokens.css` | Change | Add dark values |
| Theme helper | `src/theme/theme.ts` | Create | get/set/persist theme |
| Header | `src/components/Header.tsx` | Change | Add toggle button |
| HTML shell | `index.html` | Change | Pre-paint inline script |

## 4. Data and interfaces
| Component | Name | Type | Default | Notes |
|---|---|---|---|---|
| theme.ts | `Theme` | `'light' \| 'dark'` | system preference | |
| localStorage | `theme` | string | absent | absent = follow system |

## 5. Detailed design (per unit)
### `theme.ts`
- Logic: `getTheme()` returns stored value or system preference;
  `setTheme(t)` sets `document.documentElement.dataset.theme`, stores it.
- Inputs / outputs: none / `Theme`.

### Header toggle
- Button with `aria-pressed`; click calls `setTheme(next)`.

## 6. Assets, dependencies and configuration needed
- Dark color values approved by design (contrast checked against AA).

## 7. Risks and tooling limits
- Flash of wrong theme if the inline script runs after CSS loads → keep it inline
  and first in `<head>`.
- Private-browsing modes can make `localStorage` throw → wrap in try/catch.

## 8. Planning-complete checklist (gate to `task`)
- [x] Current state inspected and recorded
- [x] Approach aligned with the constitution
- [x] All components / data / logic detailed
- [x] Assets, dependencies and configuration identified
- [x] Risks and manual human steps mapped
- [x] Reviewed and approved by the owner

## 9. Notes / decisions (iterative)
- 2026-01-01: Considered a "follow system" third state; rejected to keep scope small.
- 2026-01-02: Owner approved the plan.
