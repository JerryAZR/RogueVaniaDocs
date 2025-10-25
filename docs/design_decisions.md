# 🧠 Design Decisions

This document records high-level design and technical choices made during early development.
Each section should summarize the **current decision**, **rationale**, and **open questions** to revisit later.

_Last updated: (fill in date)_

---

## 🎮 1. Core Game Identity

**Goal:** Describe what the game _is_ and what defines its core experience.

- **Genre & Inspiration:**
  _e.g., Multiplayer roguevania inspired by Dead Cells and Hollow Knight._

- **Core Loop Summary:**
  _e.g., Explore → Fight → Checkpoint → Upgrade → Repeat._

- **Primary Pillars:**
    - [ ] Combat Feel
    - [ ] Customizable Skills
    - [ ] Story Integration
    - [ ] Meta-Progression

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🧩 2. Scene & Game Flow Structure

**Goal:** Define how scenes, levels, and transitions are managed.

| Element | Description |
|----------|--------------|
| Main Menu | |
| Hub / Safe Area | |
| Dungeon | |
| Checkpoint / Boss Room | |

- **Data Persistence:**
  _What persists across scenes? (e.g., unlocked items, skill grid, meta progression)_

- **Scene Loading Approach:**
  _Single-scene or additive loading plan._

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 📦 3. Data & Asset Structure

**Goal:** Establish how gameplay data and assets are represented and loaded.

| Aspect | Decision |
|--------|-----------|
| Item & Skill Definitions | ScriptableObjects / JSON / other |
| Enemy / NPC Definitions | |
| Scene Assets | |
| Asset Loading Strategy | Unity built-in / Addressables / custom |

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
| Input System | Unity Input System / legacy |
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
| Resource / Balance Mechanic | Mana / Overheat / TBD |

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
| Save Format | JSON / ScriptableObject / custom |
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
| Messaging / Event System | Direct references / C# Events / Scriptable Signals |
| System Management | Central GameContext / Managers / Service Locator |
| Dependency Direction | Gameplay → Systems (one-way) |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🎨 8. Art & UX Direction

**Goal:** Define visual and UI goals early for consistency.

| Aspect | Decision |
|--------|-----------|
| Visual Style | Pixel art / vector / hybrid |
| Camera | 2D side-scroller |
| UI Style | Minimal / stylized / immersive |
| Input Target | Controller-first / keyboard fallback |

- **Rationale / Comments:**
  -

- **Open Questions:**
  -

---

## 🏗️ 9. Technical Workflow

**Goal:** Define development and collaboration conventions.

| Area | Decision |
|------|-----------|
| Version Control | Git / Repo structure / branching |
| Package Management | Manual import / Git submodules |
| Folder Structure | (to be filled) |
| Build Targets | Windows first / others later |
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
| Corgi + FishNet integration | |
| Skill Grid performance | |
| Save / serialization format | |
| Scene stacking feasibility | |
| Edgar Unity procedural API | |

- **Rationale / Comments:**
  -

---

## ✅ 11. Next Steps / Action Items

| Owner | Task | Priority | Notes |
|--------|-------|-----------|-------|
| | | | |

---

> **Tip:** Keep this document concise — it should evolve _with_ the project, not become a wiki.
> Each section represents a decision snapshot you can revisit as the design matures.
