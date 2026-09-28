---
id: 000
feature: example-dark-mode-toggle
stage: specify
status: specified
created: 2026-01-01
updated: 2026-01-02
---

# Spec — Dark mode toggle (EXAMPLE)

> **This folder is a worked example** showing what filled-in artifacts look like.
> It is fictional. Delete `specs/000-example-dark-mode-toggle/` and its row in
> `specs/index.md` when you start your first real feature.

## 1. Goal and problem
Users working at night find the bright interface tiring. We want a one-click way
to switch between a light and a dark appearance, and for the choice to be
remembered. Requested by several users this quarter and cheap to deliver now that
colors are centralized.

## 2. Scope
**In scope:**
- A visible toggle in the app header.
- Light and dark appearance for every existing screen.
- Remembering the choice between visits.
- Defaulting to the user's operating-system preference on first visit.

**Out of scope:**
- Custom themes / user-defined colors.
- Per-page theme overrides.
- Redesigning any screen.

## 3. Expected behavior (scenarios)
- When a new user opens the app and their system is set to dark, the app opens in
  dark mode.
- When the user clicks the toggle, the whole interface changes appearance
  immediately, with no page reload.
- When the user returns later, the app opens in the last chosen appearance,
  regardless of the system setting.

## 4. Functional requirements
- FR-01: The header shows a toggle that switches between light and dark.
- FR-02: The chosen appearance applies to all screens without reload.
- FR-03: The choice persists across visits on the same device.
- FR-04: With no saved choice, the system preference decides.

## 5. Acceptance criteria
- [ ] AC-01: Clicking the toggle changes every screen's appearance in under 100 ms.
- [ ] AC-02: Reloading the page keeps the chosen appearance, with no flash of the
  other appearance.
- [ ] AC-03: A fresh browser profile with a dark system setting opens in dark mode.
- [ ] AC-04: Text/background contrast meets WCAG AA in both appearances.

## 6. Dependencies and assumptions
- Colors are already defined as shared design tokens (assumed — verify in plan).

## 7. Open questions
- Should the toggle also offer an explicit "follow system" option? (Decided: no,
  keep it two-state for now.)
