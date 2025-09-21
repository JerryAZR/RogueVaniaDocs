# 🎮 Game Overview -- \[Working Title]

## **Elevator Pitch**

A **multiplayer roguevania** inspired by *Dead Cells* and *Hollow Knight*.
Players explore procedurally generated dungeons, fight challenging enemies and
bosses with tight platformer combat, and progress through a branching world
using checkpoints and meta-progression.

---

## **Core Gameplay Pillars**

1. **Exploration**

    - Dungeons are generated using *Dead Cells*-style room graphs (via Edgar Unity).
    - Each run feels fresh, while handcrafted rooms maintain quality and variety.

2. **Combat**

    - Fast-paced, melee-focused combat built around **attack, dodge, and parry**.
    - Skills and abilities come from items equipped in a **skill grid system**,
      allowing for both active and passive customization.
    - Ranged combat will be added later but won’t involve free aiming (fits
      controller-first design).

3. **Multiplayer**

    - Cooperative play: explore and fight together in the same world.
    - **Story progression follows the host**, with options to add “spoiler
      alerts” for guests rather than blocking content.
    - Single-scene multiplayer is the baseline; multi-scene support is a stretch
      goal.

---

## **Progression**

- **Meta-progression (between runs):**

    - Unlock new items and skills.
    - Expand bag size for more skill combinations.
    - Gain stronger starting loadouts.

- **Checkpoints (within runs):**

    - After clearing a section, players reach a **hub/safe area**.
    - From here, they can:
        - Continue to the next dungeon with their current items.
        - Restart from the beginning if they’re unhappy with their build.
    - If players fail in the next section, this is also where they restarts.
    - (⚠️ Design note: Some feel “rerolling items in early levels” is core to
      roguelikes -- we may refine this system to preserve that fun.)

---

## **Structure of a Run**

1. Start in a hub or checkpoint.
2. Explore randomized dungeons filled with enemies, traps, and secrets.
3. Gather items/skills to expand combat options.
4. Defeat the boss to unlock the next section.
5. Repeat until story completion or defeat.

---

## **Tone / Setting**

- Still undecided.
- Needs to support a mix of procedural environments (dungeons, biomes) and
  handcrafted hub/safe areas.
- Will heavily influence enemy, item, and NPC design.

---

## **Key Differentiators**

- **Checkpoint system** -> Players skip trivial early content once mastered,
  diving directly into new challenges.
- **Multiplayer co-op** → Rare in roguevania design, giving the genre a social
  element.
- **Bag-based skill system** → Customizable triggers and effects make builds
  more dynamic than traditional static upgrades.
