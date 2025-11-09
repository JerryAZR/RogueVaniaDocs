# 🧠 Design Decisions

This document records high-level design and technical choices made during early
development.
Each section should summarize the **current decision**, **rationale**, an
**open questions** to revisit later.

---

## 🎮 1. Core Game Identity

**Goal:** Describe what the game _is_ and what defines its core experience.

- **Genre & Inspiration:**
  Multiplayer roguevania inspired by Dead Cells and Hollow Knight.
  Start as single-player.

- **Core Loop Summary:**
  _e.g., Explore → Fight → Checkpoint → Upgrade → Repeat._

- **Primary Pillars:**
    - Combat Feel
    - Customizable Skills
    - Story Integration
    - Meta-Progression

- **Rationale / Comments:**
    - TBD

- **Open Questions:**
    - TBD

---

## 🧩 2. Primary Scenes

**Goal:** Define how scenes, levels, and transitions are managed.

### Main Menu

Where players start games and manages saves

### Hub / Safe Area

### Dungeon

Each scene contains a sub-level. Several sub-levels are chained to form a meta-
level. Clearing a meta-level for the first time unlocks new safe area and
advances story progression.

Dungeons are generated procedurally using the Edgar Pro package.

Boss rooms can be standalone scenes or part of dungeons.

---

## 📦 3. Data & Asset Structure

**Goal:** Establish how gameplay data and assets are represented and loaded.

| Aspect | Decision |
|--------|-----------|
| Item & Skill Definitions | ScriptableObjects |
| Enemy Definitions | Prefabs / ScriptableObjects |
| NPC Definitions | Prefabs / ScriptableObjects |
| Scene Assets | Unity Scenes |
| Asset Loading Strategy | Unity built-in (evolve later) |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## ⚔️ 4. Player Control & Combat

**Goal:** Decide how movement and combat systems are implemented.

| Element | Decision |
|----------|-----------|
| Controller Framework | Corgi Engine |
| Input System | Unity Input System (Corgi Input Manager) |
| Core Combat Style | Melee-focused, limited ranged |
| Parry / Dodge / Skills | Basic set planned for prototype |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🧠 5. Skill Grid System

**Goal:** Define how customizable skills are represented and executed.

| Component | Description |
|------------|--------------|
| Trigger Blocks | |
| Action Blocks | |
| Resource / Balance Mechanic | Mana / Overheat |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 💾 6. Save & Progression System

**Goal:** Define what data is saved and how progression works.

| Category | Description |
|-----------|--------------|
| Meta Progression | Unlocks, upgrades, bag expansion |
| Run Data | Inventory, skill grid, temporary stats |
| Save Format | JSON |
| Checkpoints | Hub areas or safe levels between dungeons |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🧰 7. Code Architecture & Communication

**Goal:** Outline core architectural patterns to ensure scalability and decoupling.

| Aspect | Decision |
|--------|-----------|
| Messaging / Event System | Direct references / C# Events |
| System Management | GameManager that holds all persistent singletons |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🎨 8. Art & UX Direction

**Goal:** Define visual and UI goals early for consistency.

| Aspect | Decision |
|--------|-----------|
| Visual Style | Likely 3D low-poly |
| Camera | 2D side-scroller |
| UI Style | Any |
| Input Target | keyboard first + controller |

- **Rationale / Comments:**
    - Compared to pixel arts, it's easier to find low-poly assets with consistent style
    - We could even make our own models

- **Open Questions:**
  -

---

## 🏗️ 9. Technical Workflow

**Goal:** Define development and collaboration conventions.

| Area | Decision |
|------|-----------|
| Version Control | Git / GitHub |
| Package Management | Unity package manager |
| Folder Structure | (to be filled) |
| Build Targets | Windows first / Apple optional / Linux later |
| Level Authoring | Edgar Unity + handcrafted room prefabs |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🔮 10. Known Technical Risks / Research Topics

**Goal:** Track areas that need investigation or pose integration risk.

| Topic | Notes / Owner / Status |
|--------|------------------------|
| Skill Grid performance | |
| Save / serialization format | |
| Edgar Unity procedural API | |

- **Rationale / Comments:**
  -

## 11. Dungeon Tile Types

- Basic
- Spikes
- Moving Platforms
- Ladders

---

## ✅ 12. Next Steps / Action Items

| Owner | Task | Priority | Notes |
|--------|-------|-----------|-------|
| | | | |

---

> **Tip:** Keep this document concise — it should evolve _with_ the project, not become a wiki.
> Each section represents a decision snapshot you can revisit as the design matures.
