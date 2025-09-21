# ⚔️ Skill Grid – Overview

## **Core Idea**

The **Skill Grid** is a programmable system that lets players define their own
active and passive abilities. Instead of fixed skill trees, abilities emerge
from how players arrange and connect blocks within the grid. This creates
flexible builds, encourages experimentation, and makes progression (larger
grids, more block types) meaningful.

---

## **Core Components**

1. **Trigger Blocks**

    - Activate when certain **conditions** are met.
    - Two main categories:
        - **Input Triggers** -> bound to key/button presses (function like
          traditional active skills).
        - **Event Triggers** -> tied to gameplay events (e.g. *on damage taken*,
          *on perfect parry*, *on kill*).
    - Can connect to one or more Action Blocks.

2. **Action Blocks**

    - Execute effects when activated by a Trigger.
    - Examples:
        - Heal the player.
        - Damage nearby enemies.
        - Summon a projectile or shield.
    - Serve as the “payload” of the skill system.

3. **Advanced/Utility Blocks (Future Expansion)**

    - Add variety, creativity, and depth:
        - **Relay / Chain Trigger** -> activates when another Trigger fires.
        - **Copy Block** -> duplicates the behavior of an adjacent block.
        - **Modifier Blocks** -> alter nearby Actions (e.g. convert damage to
          fire, increase radius, add poison).

---

### **Skill Types Supported**

- **Active Skills** -> Player presses a button, the bound Trigger fires, linked
  Actions execute.
- **Reactive Skills** -> Gameplay events automatically trigger Actions (like
  passives).

---

### **Early Scope vs. Long-Term**

- **Early versions** will focus on the basics:
    - A small set of Input and Event Triggers.
    - A few versatile Actions (heal, damage, buff).
- **Later expansions** can introduce fun/complex blocks for creative expression.

---

## ⚖️ Balancing the Skill Grid

### **The Core Problem**

- Players can chain Triggers and Actions to create loops.
- Loops can be **internal** (Trigger A -> B -> A) or **external** (Trigger ->
  Event -> other Trigger -> first Trigger again).
- While we want to allow clever combos and “power moments,” we must avoid
  infinite spam that trivializes gameplay.

---

### **Mitigation Systems**

#### 1. **Mana System (Resource Cost)**

- **Each Action Block consumes mana** when executed.
- If insufficient mana:
    - Option A: Action sits in a queue until mana regenerates.
    - Option B: Action is discarded.
- **Mana regenerates slowly over time.**
- Benefits:
    - Familiar RPG mechanic (easy to explain).
    - Prevents infinite loops by draining resources.
- Risks:
    - Strong mana regen builds might still break balance unless capped.

---

#### 2. **Overheat System (Execution Cost)**

- **Each Action Block generates heat.**
- Heat builds up with rapid or repeated activations.
- If total heat exceeds threshold:
    - All queued actions are cleared.
    - Triggers deactivate for a cooldown window.
    - (Optional) Player takes backlash damage.
- **Heat dissipates gradually over time.**
- Benefits:
    - Encourages pacing and rhythm in combos.
    - “Overheat shutdown” feels thematic (fits tech or magic equally well).
- Risks:
    - May frustrate players if it feels too punishing. Needs good
      feedback/telegraphing.

---

#### 3. **Trigger Rate Limits (Throttle)**

- Put a **per-trigger cooldown** (e.g. Trigger A can only fire once every 0.5s).
- Ensures loops can’t run infinitely fast.
- Benefits:
    - Simple, predictable cap.
- Risks:
    - Feels restrictive — undermines the “programmable” fantasy.

---

#### 4. **Decay / Diminishing Returns**

- Repeatedly triggering the same loop reduces effectiveness:
    - Each repeat deals less damage / heals less until it bottoms out.
- Benefits:
    - Keeps combos fun for a while, then self-limiting.
- Risks:
    - Harder for players to understand (“why did my damage shrink?”).
    - Harder to program because actions are still getting executed.

---

#### 5. **External Anchors (Event-Only Chains)**

- Restrict some triggers to **external events only** (damage taken, parry, kill).
- They cannot be re-triggered by other triggers.
- Benefits:
    - Loops can’t self-sustain forever.
- Risks:
    - Adds hidden rules players must learn.
    - Does not **fundamentally** prevent infinite loops.
    - Requires careful design of triggers.

---

### **Design Approach Recommendation**

- **Hybrid system**:
    - Start with **Mana and/or Overheat** as the main balancing axes.
    - If not enough, add **light rate limits** on a per-trigger basis.
- This gives:
    - Resource management (mana).
    - Combo pacing (overheat).
    - Safety net (rate limit).
- Allows crazy combos, but forces players to think about *when* to unleash them.

---

## Minimal Example

![](./drawio/skill_grid_example.drawio)

In this example, there are 3 trigger blocks and 4 action blocks.

- When player presses the *skill 1* button, the two actions above are executed
  in order: First a fireball is shot, then the player gets an ATK buff.
- When player presses the *skill 2* button, the five blocks in the row above are
  activated from left to right, but there are only two valid action blocks
  within: First a fireball is shot, then the player gets an DEF buff.
- When the player character is healed in any way, the two blocks on the right of
  *Trigger (On Heal)* are activated: An ATK buff followed by a heal action
    - Note that the heal action would re-trigger the *Trigger (On Heal)*,
      causing a loop. This is where the balancing mechanic discussed above kicks
      in.
