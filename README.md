# Tactical AI Sandbox — C++/OpenGL (FreeGLUT)

Real‑time, grid‑based combat sandbox featuring two teams (**Blue** vs **Orange**) with autonomous units (Commander, Warriors, Medic, Supplier). The simulation demonstrates FSM‑driven behavior, event‑based coordination, A* pathfinding, visibility/cover reasoning, and simple ballistics (bullets, grenades).

This repo is a Visual Studio solution that runs on Windows with bundled FreeGLUT/GLEW.

---

## ✨ Highlights

- **Units & Roles**: Commander issues goals; **Warriors** fight; **Medic** heals/refills; **Supplier** resupplies ammo/grenades (`Units.*`, `Commander.*`, role classes).
- **AI Architecture**: per‑unit **Finite State Machine** (`State_*`), central **EventBus** (`AIEvents.h`, `EventBus.h`) for CommanderDown, UnderFire, NeedAmmo/NeedHeal, etc.
- **Pathfinding**: modular `AI::Pathfinding` with A* and utility helpers (`Pathfinding.*`, `Node.*`).
- **Visibility & LOS**: grid‑ray casting + unit sight range (`Visibility.*`), **line‑of‑sight** that treats ROCK/TREE as blockers, WATER pass‑through (see `Definitions.h`).
- **Security Map**: risk field updated over time to bias paths away from danger (`SecurityMap.*`).
- **Combat**: hitscan bullets with spread, **grenades** with AoE damage and friendly‑fire checks (`Combat.*`).
- **Renderer & HUD**: minimalist 2D tiles + unit letters; live HUD with FPS, grid stats, and toggles (`Renderer.*`).

---

## 🗂 Project Structure

```
FinalProjectAI/
└─ Graphics/
   ├─ Graphics.sln                # Visual Studio solution
   ├─ Graphics/
   │  ├─ *.cpp / *.h              # Engine, AI, rendering, gameplay
   │  ├─ DLLs & Libs              # freeglut/glew runtime + libs
   │  └─ x64/{Debug,Release}/     # build outputs (exe, pdb, dlls)
   └─ Debug/                      # ready-to-run EXE + DLLs
```

Key modules (non‑exhaustive):
- `Definitions.h` — global constants (grid size, HP/DMG, ammo, grenade params).
- `Grid.*` — map generation, terrain types (EMPTY/ROCK/TREE/WATER/DEPOT).
- `Units.*` — base Unit, stats, path, team/role; `Commander.*`, `Warrior.*`, `Medic.*`, `Supplier.*`.
- `State_*.{h,cpp}` — Idle/Patrol/Attack/Healing/Supplying/Waiting… FSM states.
- `Pathfinding.*`, `Node.*` — A* and path container.
- `Visibility.*` — LOS + per‑unit visibility bitmaps.
- `SecurityMap.*` — danger/risk accumulation & decay.
- `Combat.*` — bullets, grenades, damage, friendly‑fire guardrails.
- `Renderer.*` — grid drawing, unit glyphs, HUD.

---

## ▶️ Build & Run (Windows)

1. Open **`FinalProjectAI/Graphics/Graphics.sln`** in **Visual Studio 2019/2022**.
2. Set platform to **x64**. Choose **Debug** or **Release**.
3. **Build** (Ctrl+Shift+B) and **Run** (F5).
4. Alternatively, run the prebuilt **`Graphics.exe`** from `FinalProjectAI/Graphics/Debug/`.
   - Ensure `freeglut.dll` and `glew32.dll` are beside the `.exe` (already included).

### Prerequisites
- Windows 10/11
- Visual Studio 2019/2022 – Desktop development with C++
- Basic OpenGL support

---

## 🎮 Controls

- **Right‑click** — Toggle **Security Map** overlay (disables Visibility overlay when ON).
- **Left‑click** — Set target cell (row,col) for commander orders/debug actions (printed to console).
- **K** — Toggle **Commander AI** ON/OFF.
- **N** — Rebuild a fresh **test world** (when Game Over) or new round.
- **E** — Exit.
- **X** — Kill the **Blue** commander (test); surviving Blue units switch to autonomous mode.
- **O** — Kill the **Orange** commander (test); surviving Orange units switch to autonomous mode.

> The HUD prints: `FPS`, `Grid` size, `Cell` size, **Security** overlay state, **Visibility** overlay state (team tag), and cell/unit counts.

---

## 🧠 AI Notes

- FSM per unit with message‑driven transitions (e.g., **CommanderDown** triggers autonomous behavior).
- **Visibility** uses integer grid ray‑march; **HasLineOfSight**: ROCK/TREE block, WATER does not (for view/fire).
- **SecurityMap** decays over time (risk “fades”), rises near firefights; path cost blends distance + risk.
- **Combat** rounds apply damage, ammo checks, reload timers; **grenade AoE** applies radial falloff and friendly‑fire rules.
- Constants are centralized in `Definitions.h` to tune HP, damage, ranges, cooldowns, and decay factors.

---

## 🔧 Troubleshooting

- **Missing DLLs**: If running the `.exe` manually, keep `freeglut.dll` & `glew32.dll` next to it.
- **Blank window / no input**: Ensure the active project is **Graphics**, platform **x64**.
- **Build errors from temp paths**: Re‑extract the project to a simple folder and open the `.sln` from there.

---

## 📄 License

Educational/portfolio project. If you fork or publish, credit the original authors and note your changes.

---

## 🙌 Credits

- FreeGLUT / GLEW (bundled)
- Classic pathfinding & state‑machine patterns used for didactic purposes
