---
id: 000
feature: example-thrown-item-damage-aggro
stage: specify
status: specified
created: 2026-01-01
updated: 2026-01-02
---

# Spec — Damage and aggro from thrown items (EXAMPLE)

> **This folder is a worked example** showing what filled-in artifacts look like.
> It is adapted from a real feature of a stealth game and written with
> **engine-neutral names**, so it reads the same in Unreal, Unity or Godot.
> Delete `specs/000-example-thrown-item-damage-aggro/` and its row in
> `specs/index.md` when you start your first real feature.

## 1. Goal and problem
The player can already pick up and throw items, but a throw has no consequence
for enemies — the item just bounces and lies on the ground. This feature gives
throwing a **tactical, offensive purpose**: hitting an enemy with an item
**deals damage** and, if the enemy is unaware (**patrolling**), **wakes it up
into combat** so it comes for the player. It widens the stealth toolbox (wound
and provoke from a distance, pull one specific enemy) by reusing the combat that
already exists, without introducing new weapons.

## 2. Scope
**In scope:**
- Detect when a thrown item **hits an enemy**.
- Apply damage through the existing hit path, with the same **hit reaction
  (stagger)** as a player melee hit.
- **Non-lethal damage:** a throw lowers health but **never kills** the enemy.
- If the enemy was **patrolling**, switch it to **combat**, targeting the player.
- If the enemy is **already in combat**, only apply damage + hit reaction — do
  not restart its state.
- The item is **consumed on impact** (destroyed, not re-collectable).
- Only the **enemy that was hit** reacts; nearby enemies keep their normal
  perception logic.

**Out of scope:**
- The enemy health/death system (already exists — reused, not rewritten).
- Fine damage balancing per item type or per enemy.
- New impact VFX/SFX beyond what the existing hit path already triggers.
- Group alert / chain aggro of neighbors.
- Changing what happens when the item hits a wall or the floor.
- Throw damage against anything that is not an enemy.

## 3. Expected behavior (scenarios)
- **Hit on a patrolling enemy:** When the player throws an item and it hits a
  patrolling enemy, the enemy takes damage (with a hit reaction), the item
  disappears on impact and the enemy **enters combat**, turns and goes for the
  player.
- **Hit on an already-alerted enemy:** When the item hits an enemy already in
  combat, it takes damage + hit reaction and **continues** combat normally.
- **Non-lethal:** No matter how many items hit the same enemy, throwing alone
  **never kills it** — it is left alive with very low health at most.
- **Item vanishes on hit:** The item disappears when it hits an enemy.
- **Miss / wall:** When the item hits no enemy (wall, floor), behavior is exactly
  as before: no damage, no aggro, no regression of the throw.
- **Neighbors unaffected:** Other enemies nearby do **not** enter combat because
  of this hit.

## 4. Functional requirements
- FR-01: A thrown item must **detect impact** against an enemy.
- FR-02: On impact with an enemy, **apply damage** through the existing hit
  path, including the **hit reaction** used by player melee hits.
- FR-03: Throw damage is **non-lethal**: it must never trigger the death
  sequence. Health may go **negative** (no floor) so a later melee hit can finish
  the enemy.
- FR-04: If the enemy is **patrolling**, transition it to **combat** with the
  player as target.
- FR-05: If the enemy is already **in combat**, **do not restart** its state.
- FR-06: The item is **destroyed** when it hits an enemy.
- FR-07: **Only the enemy that was hit** is provoked; no aggro propagates.
- FR-08: When the throw does **not** hit an enemy, current throw behavior is
  **preserved**.
- FR-09: Multiple hits in the same frame/throw must not **stack** reactions or
  state changes.

## 5. Acceptance criteria
- [ ] AC-01: In a playtest, throwing an item at a patrolling enemy makes it take
  damage and **charge the player**.
- [ ] AC-02: The enemy plays the **hit reaction** when struck (same as a melee hit).
- [ ] AC-03: The item **disappears** on hitting the enemy.
- [ ] AC-04: Throwing many items at one enemy **never kills it**.
- [ ] AC-05: An enemy **already in combat** takes damage + reaction and
  **keeps fighting** without a state restart.
- [ ] AC-06: An item hitting a wall/floor causes **no damage or aggro**, and
  throwing still works as before.
- [ ] AC-07: Hitting one enemy does **not** put neighboring enemies in combat.
- [ ] AC-08: The project builds/compiles with 0 errors and 0 warnings after the
  change.

## 6. Dependencies and assumptions
- Reuses the existing enemy with its health and combat state machine.
- Reuses the existing throw feature (the thrown item is a physics object).
- Enemy states include at least *Patrolling* and one or more combat states; the
  existing melee hit path is triggered by animation events of the player's attacks.
- Non-lethality comes from **not** running the death check on the throw path
  (death is only evaluated by an animation event during melee).

## 7. Open questions
- ~~Lethality~~ → **non-lethal** (decided with the owner).
- ~~What happens to the item~~ → **destroyed on impact**.
- ~~Aggro range~~ → **only the enemy that was hit**.
- ~~Already-alerted enemy~~ → **damage + hit reaction, no state restart**.
- **To resolve in the plan:** exact impact-detection hook (collision hit event vs.
  overlap), and whether the existing hit path already covers damage + reaction +
  waking up from patrol or needs an explicit state change.
