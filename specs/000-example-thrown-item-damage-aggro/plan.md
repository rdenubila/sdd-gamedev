---
id: 000
feature: example-thrown-item-damage-aggro
stage: plan
status: planned
created: 2026-01-01
updated: 2026-01-02
---

# Plan — Damage and aggro from thrown items (EXAMPLE)

> Written with engine-neutral names. **Engine mapping** for the names below:
> *ThrowableItem* = Blueprint actor (Unreal) / prefab with a script (Unity) /
> scene with a `RigidBody` script (Godot); *Enemy* = Character Blueprint /
> MonoBehaviour prefab / `CharacterBody` scene; *collision hit event* =
> `OnComponentHit` / `OnCollisionEnter` / `body_entered`.

## 1. Current state (inspected)
- **Throw:** the player's `ThrowItem` function spawns a `ThrowableItem`, enables
  physics simulation on its mesh and sets its velocity. So the "projectile" is a
  `ThrowableItem` whose mesh is simulating physics — impact detection belongs there.
- **ThrowableItem:** components `Mesh`, `PromptWidget`, a noise/perception stimuli
  source. It does not tick. The **same class** is used for pickups resting in the
  world and for thrown projectiles, so we must tell "was thrown" from "is resting".
- **Enemy:** health is an integer `Health`. `DecrementHealth` subtracts 1 with no
  death check. `CheckIsDead` decides death and is **only called by an animation
  event** during melee — death is not evaluated at damage time.
- The enemy's `TakeHit` interface event is an **empty stub** called by the player's
  melee animation events. Overriding it risks regressing melee.
- The enemy AI is a state machine with global transitions on events: sending the
  *StartAttack* event moves any state (including Patrol) to *Attacking*, which
  reads `TargetActor`. A wrapper `StartAttackFn` already sets the state, sends
  that event and adds the attack indicator; it needs `TargetActor` set first.
- A patrol marker (`Enemy.State.isPatrolling`) identifies patrolling enemies.
- There is **no standalone hit-reaction animation** for the enemy; a reaction clip
  exists on the player's skeleton and the enemy uses a different skeleton with the
  same rig base.

## 2. Technical approach
Reuse everything that exists, with a **dedicated path** for throws that never
touches melee:

1. **Impact detection on `ThrowableItem`** (no new actor): enable hit events on
   the mesh and handle the collision hit event. If the other object is an enemy
   **and** the item `bWasThrown`, apply the effect and **destroy the item**.
   Anything else does nothing, so a miss behaves exactly like today.
   No tick and no extra overlap volume — cheap for low-end targets (constitution 1.2).
2. **Effect on the enemy through a new function `ReceiveThrowHit`** (not through
   the melee `TakeHit`):
   - **Non-lethal damage:** subtract `ThrowDamage` from health and **do not** call
     `CheckIsDead`. Health may go negative; a later melee hit finishes the enemy.
   - **Hit reaction:** play the reaction montage on the enemy mesh.
   - **Wake from patrol:** if the enemy has the patrol marker, set `TargetActor` to
     the player and call `StartAttackFn`. If it is already in combat, only damage
     and reaction — no state restart.
3. **Animation compatibility (manual step for the owner):** register the enemy
   skeleton as compatible with the player skeleton so the reaction clip plays on
   it — no retargeting and no duplicated assets.

Alternatives discarded: overriding `TakeHit` (melee regression risk), a tick or
overlap volume on the item (costlier than the collision hit event), per-enemy
retargeting (more assets to maintain than skeleton compatibility).

## 3. Components affected / created
| Component | Path | Create or change? | What |
|---|---|---|---|
| ThrowableItem | `Items/ThrowableItem` | Change | `bWasThrown` flag; hit event → detect enemy, call `ReceiveThrowHit`, destroy self |
| Player | `Characters/Player` | Change | in `ThrowItem`, set `bWasThrown = true` on the spawned item |
| Enemy | `AI/Enemy` | Change | new function `ReceiveThrowHit(HitLocation)`; config variables |
| Enemy skeleton | `Characters/Enemy/Skeleton` | **Manual (owner)** | mark compatible with the player skeleton |

## 4. Data and interfaces
| Component | Name | Type | Default | Notes |
|---|---|---|---|---|
| ThrowableItem | `bWasThrown` | bool | false | exposed on spawn so the thrower can set it |
| Enemy | `HitReactionMontage` | animation asset ref | player's hit reaction | preserve the asset value when recreating the play node (constitution Part 2) |
| Enemy | `ThrowDamage` | int | 1 | damage per hit; no health floor |

> No new array variables, so the tooling's array-element-type limitation does not apply.

## 5. Detailed design (per unit)
### ThrowableItem — collision hit event
- Logic: `Branch(bWasThrown)` → cast the other object to Enemy → on success call
  `ReceiveThrowHit(hit location)` → destroy self.
- Precondition: the mesh must generate hit events while simulating physics.
- Miss path: cast fails or `bWasThrown` is false → do nothing.

### Enemy — `ReceiveThrowHit(HitLocation)`
- Guard: return early if the enemy is already dead (invalid or health check).
- Damage: `Health = Health - ThrowDamage`. **No `CheckIsDead`.**
- Reaction: play `HitReactionMontage` on the mesh.
- Aggro: `if HasTag(isPatrolling)` → `TargetActor = player` → `StartAttackFn`.
- Anti-stacking (FR-09): destroying the item on its first enemy hit prevents a
  second hit from the same item in the same frame.

### Player — `ThrowItem`
- After spawning the item and before enabling physics: `Set bWasThrown = true`
  on the spawned reference.

## 6. Assets, dependencies and configuration needed
- Enemy skeleton compatibility registered with the player skeleton (manual step).
- The reaction montage available in the enemy's animation blueprint slot chain.

## 7. Risks and tooling limits
- **Hit events not firing** if the mesh does not generate collision events →
  verify the flag in the static validation of T001.
- **Casting to the wrong type:** the concrete enemy may be a subclass — cast to
  the common enemy base.
- **Reaction not visible on other enemy types** until their skeletons are also
  marked compatible (logic is generic; only the animation is missing) — accepted
  for this delivery.
- Editor tooling limitation: connecting nodes through struct sub-members is not
  supported — use a *Break Hit Result* function node instead (constitution Part 2).

## 8. Planning-complete checklist (gate to `task`)
- [x] Current state inspected and recorded
- [x] Approach aligned with the constitution
- [x] All components / data / logic detailed
- [x] Assets, dependencies and configuration identified
- [x] Risks and manual human steps mapped
- [x] Reviewed and approved by the owner

## 9. Notes / decisions (iterative)
- 2026-01-01: First draft overrode `TakeHit`; dropped after finding it is the melee hook.
- 2026-01-02: Owner chose skeleton compatibility over retargeting. Only the first
  enemy type is covered in this delivery. Plan approved.
