---
id: 000
feature: example-thrown-item-damage-aggro
stage: implement
status: implementing
created: 2026-01-02
updated: 2026-01-04
---

# Tasks — Damage and aggro from thrown items (EXAMPLE)

## Status legend
`todo` · `doing` · `blocked` · `done`

---

## Progress summary
- **Current stage:** implementing — Task 003
- **Completed:** 2 / 5
- **Already implemented:** skeleton compatibility registered and verified (T001);
  `bWasThrown` flag added and set by the throw (T002).
- **Still missing:** `ReceiveThrowHit` on the enemy (T003), impact detection on the
  item (T004), regression pass on miss/wall and neighbors (T005).

### Dependency graph
- T001 <- — (can start now)
- T002 <- — (can start now)
- T003 <- T001
- T004 <- T002, T003
- T005 <- T004

---

## Task 001 — Skeleton compatibility (manual, owner)
**Status:** done
**Depends on:** —

**Goal:** Let the enemy play the player's hit-reaction clip (supports FR-02, AC-02).

**Implementation steps:**
1. Owner: open the enemy skeleton, add the player skeleton to its compatible list.
2. AI: register the reverse direction if the tooling supports it.

**Static validation (AI):**
- The tooling reports the skeletons as compatible (bone counts differ but names match).

**Manual test (human):**
- Steps: preview the reaction clip on the enemy mesh in the animation editor.
- Expected: the clip plays with no broken limbs.
- Pass criterion: no visible deformation.

**Progress:**
- 2026-01-03: Done. Compatible in both directions; preview looks correct. Owner approved.

---

## Task 002 — `bWasThrown` flag on the item
**Status:** done
**Depends on:** —

**Goal:** Tell a thrown projectile apart from a resting pickup (FR-08).

**Implementation steps:**
1. Add `bWasThrown` (bool, default false, exposed on spawn) to `ThrowableItem`.
2. In the player's `ThrowItem`, set it to true on the spawned item before physics starts.

**Static validation (AI):**
- Both assets compile with 0 errors and 0 warnings.
- Re-read the throw function: the set node sits between spawn and physics.

**Manual test (human):**
- Steps: throw an item at a wall.
- Expected: exactly as before — item bounces, no errors in the log.
- Pass criterion: no visible change in throw behavior.

**Progress:**
- 2026-01-03: Done. Compile clean. Throw regression check passed.

---

## Task 003 — `ReceiveThrowHit` on the enemy
**Status:** doing
**Depends on:** T001

**Goal:** Non-lethal damage, hit reaction and wake-from-patrol (FR-02..FR-05, AC-01, AC-02, AC-04, AC-05).

**Implementation steps:**
1. Add variables `HitReactionMontage` and `ThrowDamage`.
2. Create function `ReceiveThrowHit(HitLocation)` with the dead-guard, damage
   (no death check), reaction and the patrol branch that sets `TargetActor` and
   calls `StartAttackFn`.

**Static validation (AI):**
- Enemy compiles with 0 errors and 0 warnings.
- Re-read the function: no path reaches `CheckIsDead`; the patrol branch calls
  `StartAttackFn` after `TargetActor` is set.

**Manual test (human):**
- Steps: call the function from a debug key on a patrolling enemy and on one
  already in combat.
- Expected: patrolling enemy staggers and charges; combat enemy staggers and keeps fighting.
- Pass criterion: no death, no state restart, no errors in the log.

**Progress:**
- 2026-01-04: Variables added; function skeleton and damage done. Reaction and
  patrol branch still to wire.

---

## Task 004 — Impact detection on the item
**Status:** todo
**Depends on:** T002, T003

**Goal:** Connect a real throw to the enemy effect and consume the item (FR-01, FR-06, FR-09, AC-01, AC-03).

**Implementation steps:**
1. Enable hit events on the item's mesh.
2. Add the collision hit handler: `Branch(bWasThrown)` → cast to enemy →
   `ReceiveThrowHit` → destroy self.

**Static validation (AI):**
- Item compiles with 0 errors and 0 warnings; wiring matches the plan.

**Manual test (human):**
- Steps: throw an item at a patrolling enemy from distance.
- Expected: item vanishes on impact, enemy staggers and charges.
- Pass criterion: AC-01, AC-02 and AC-03 all observed.

**Progress:**
- _—_

---

## Task 005 — Regression pass and cleanup
**Status:** todo
**Depends on:** T004

**Goal:** Prove nothing else changed (FR-03, FR-07, FR-08, AC-04, AC-06, AC-07, AC-08).

**Implementation steps:**
1. No new code unless a regression is found.
2. Update the constitution: record the "throws never call the death check"
   decision and the hit-event lesson; update `index.md`.

**Static validation (AI):**
- Full build passes; all touched assets compile clean.

**Manual test (human):**
- Steps: (a) throw at a wall/floor; (b) throw 10 items at one enemy; (c) throw at
  one enemy next to another patrolling enemy.
- Expected: (a) no damage/aggro; (b) enemy never dies; (c) neighbor stays on patrol.
- Pass criterion: AC-04, AC-06 and AC-07 approved.

**Progress:**
- _—_
