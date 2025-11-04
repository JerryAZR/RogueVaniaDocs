# Scene Transitions

This document explains how **scene and room transitions** are handled in our
game, using and extending features provided by **Corgi Engine** and
**Edgar Unity**.

---

## 1. Basics

Corgi Engine provides several built-in systems to handle scene transitions,
teleportation, and level management.
These systems include **scripted scene loading functions** and
**ready-to-use components**.

### 1.1 Scene Loading Functions

All Corgi Engine scene loading methods support **loading screens**, which can be
customized to display arbitrary content or disabled entirely.

#### `MMSceneLoadingManager`

`LoadScene(string sceneName)`

- A **static** and minimal function for switching to another scene.
- Use it like Unity’s native `SceneManager.LoadScene`.
- Does **not** require a `LevelManager` instance.
- Does **not** apply fade or disable character controls automatically.

#### `LevelManager`

`GotoLevel(string levelName, bool fadeOut = true, bool save = true)`

- A **member function** that must be called on a **LevelManager instance**
  (usually the singleton).
- Provides extra functionality:
    - Optionally performs a **fade-out/in** transition.
    - Temporarily **disables player character abilities** until the new scene is
      loaded.
    - Handles **save data** if enabled.

This is usually the preferred method when a `LevelManager` instance is
available.

---

### 1.2 Scene Transition Components

Corgi Engine provides a few reusable components that automate transitions when
attached to a `GameObject` in a scene:

#### **FinishLevel**

*Path: `Assets/CorgiEngine/Common/Scripts/Spawn/FinishLevel.cs`*

- Inherits from `ButtonActivated`.
- Attach to a `GameObject` with a `Collider2D`.
- Configure a **target scene name** in the inspector.
- On activation (via **button press** or **trigger enter**), it will load the
  specified scene.
- By default, the player will spawn at the **first `Checkpoint`** found in the
  new scene.

#### **GoToLevelEntryPoint**

*Path: `Assets/CorgiEngine/Common/Scripts/Spawn/GoToLevelEntryPoint.cs`*

- Inherits from `FinishLevel`.
- Adds the ability to specify a **spawn entry point index**.
- If the `LevelManager` in the destination scene finds a
  **registered entry point** with that index, the player will spawn there.
- Otherwise, it falls back to the first checkpoint (same as `FinishLevel`).
- **Note:** *Point of Entry* and *Checkpoint* are two distinct position
  initialization methods. A *Point of Entry* does not need to be a *Checkpoint*,
  nor does a *Checkpoint* need to register as a *Point of Entry*. That being
  said, nothing prevents an object from being both a *Point of Entry* and a
  *Checkpoint*.

#### **Teleporter**

*Path: `Assets/CorgiEngine/Common/ScriptsCinemachine/Environment/Teleporter.cs`*

- Also inherits from `ButtonActivated`.
- Used for **teleportation within the same scene**.
- Attach to a GameObject with a collider and specify another `Teleporter` as the
  **target**.
- On activation, moves the player instantly to the target’s position.

---

### 1.3 LevelManager and Character Bounds

Two additional components play an important role in Corgi’s scene and world management:

- **LevelManager**:
    - Handles scene loading, checkpoint logic, respawns, and player persistence.
    - Register points of entry for `GoToLevelEntryPoint` in the `PointsOfEntry`
      list.
    - Define `Center` and `Extent` (size) of level bounds in the inspector.
    - There is a "Debug Spawn" field that overrides spawn points so we usually
      want to leave it empty.

  > Note: The default implementation is a **singleton**, which may not fit
  > projects using additive scene loading or multiple level contexts (e.g.,
  > multiplayer hosts).

- **Character Level Bounds**:
  Keeps the player within the level and kills or stops them when leaving bounds.
  Player may fail to spawn properly when the selected spawn/entry point is
  outside the level bounds.

  > Known bug: Removing it can cause physics instabilities such as bouncing or
  > clipping through the floor.

---

## 2. Cross-Scene Room Transitions

This setup handles moving between separate Unity scenes — for example,
transitioning between a central hub area and individual room scenes. Each room
is stored in its own scene, and the player can freely enter or exit these rooms
 When returning to the hub, the player should reappear at the specific entrance
 they originally used, rather than the default spawn point.

### Hub -> Room

1. In the **hub scene**, locate each room entrance object.
2. Add a **`BoxCollider2D`** (set as a trigger) and a **`FinishLevel`** component.
3. Configure the **`FinishLevel`** component to load the target room scene as
   the next level.
    - For rooms with a single entrance/exit, this setup is sufficient.
    - If a room has multiple exits and you want to control which door the player
      appears at, use **`GoToLevelEntryPoint`** instead of `FinishLevel`.

### Room -> Hub

1. In the **hub scene**, register each entrance point with the
   **`LevelManager`** in its **"Points of Entry"** list.
2. In the **room scene**, attach a **`BoxCollider2D`** and a
   **`GoToLevelEntryPoint`** component to the exit door.
3. Configure **`GoToLevelEntryPoint`** to:
    - Load the **hub scene** name, and
    - Specify the correct entry index (matching the hub entrance from the
      "Points of Entry" list).

This ensures that when the player returns, they appear at the correct hub entrance.

### Important Note

Both the **hub scene** and all **room scenes** must be included in the
**Build Settings** scene list. Unity cannot load scenes at runtime unless
they’re registered there.

---

## 3. Same-Scene Teleports

Same-scene teleports allow moving between multiple points in the same scene.
This should allowing building multiple rooms within the same scene, minimizing
loading time.

Corgi’s `Teleporter` component can move characters between two positions within
one scene, but by default:

- Adjacent rooms remain **visible/active** instead of being masked out.
- The **camera** is not confined to the new room’s bounds.

The built-in `Room` and `RoomManager` systems in Corgi **appear to support this**,
but documentation is outdated and needs investigation.

**Next steps:**

- Study how `RoomManager` constrains camera and visibility.
- Prototype same-scene room transitions with masking and camera limits.
- Evaluate performance and practicality compared to multi-scene transitions.

**Status:**
🟡 Works, but not perfect yet.

---

## 4. Dungeon Traversal

Before we can discuss how dungeon traversal works, we need to understand how
**dungeon generation** is structured at a high level.

The **Dungeon Generator** takes a `LevelGraph` as input and uses it to build a
level. A `LevelGraph` defines:

- How room nodes are connected, and
- Which room prefabs can be used for each node.

Each dungeon level in a run uses a different randomization seed (and possibly a
different graph), but otherwise, the generation process is identical. Instead of
maintaining dozens of nearly identical Unity scenes, we reuse a
**single base scene** that contains the generator, and we pass in generation
parameters at runtime.

---

### Generation Parameters

- **Base Seed:** A single seed that determines the overall randomization for the
  run.
- **Level Number:** An integer that identifies the current level within the run.
- **Level Graphs:** A list of predefined `LevelGraph` assets, one per dungeon
  level (stored in the generator).

Each level’s generation seed is derived from the base seed:

```csharp
level_seed = base_seed + level_number
```

The generator selects the corresponding `LevelGraph` from the list using the
level number as an index.

---

### Level Transition Flow

#### 1. Entering the Dungeon

When the player enters the dungeon from the hub:

- The game records the **base seed** for the run.
- The next **level number** (usually `0` for the first dungeon level) is stored
  in a **static variable**.
- The dungeon base scene is loaded, and the generator uses the parameters above
  to build the level.

This logic is handled by an **extended version of the `FinishLevel`** component.

---

#### 2. Advancing to the Next Level

When the player reaches the end of a dungeon level:

- An **extended `FinishLevel`** component increments the static level number
  (`level_number += 1`).
- The same **base scene** is reloaded.
- The generator uses the new level number and base seed to build the next level.

This allows seamless multi-level progression without maintaining separate Unity
scenes for each floor.

---

#### 3. Returning to the Hub

At the end of a “meta-level” (i.e., after the final floor of a dungeon branch),
the exit uses a **`GoToLevelEntryPoint`** component instead of `FinishLevel`.

This component:

- Loads the **hub scene**, and
- Positions the player at the correct gate or return point.

---

#### 4. Reverse Traversal (Experimental)

We’ve also implemented **reverse traversal**, though it’s uncertain whether this
will remain in the final game design.

- Interacting with the *entrance* of a dungeon level sends the player back to
  the **previous level’s exit**.
- This is done via an **extended `GoToLevelEntryPoint`** component that:
    - Decrements the level number (`level_number--`), and
    - Sets the target point index to `0` (the registered exit spawn point).
- The exit in each level also registers itself with the `LevelManager` as the
  **first point of entry**, ensuring proper positioning when returning.

---

### Spawn Synchronization and Timing

Because the generator builds rooms dynamically, there’s a risk that the
player might spawn before the designated entry point (entrance or exit) is
registered. To prevent this race condition:

- The **Level Manager** delays player spawning by **two frames**, giving the
  generated level time to register its spawn points and other initialization data.
- This simple delay has proven sufficient to ensure consistent and correct spawn
  positioning.
