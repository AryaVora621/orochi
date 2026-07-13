# Checkpoint (last updated 2026-07-13)

**Project:** Project Orochi - Multi-Board 6-Tool Ender CoreXY Evolution
**State:** PLANNING ONLY. No physical hardware work, printing, wiring, or firmware flashing has started.
**Repo:** https://github.com/AryaVora621/orochi.git  (local: `/Users/aryavora/Desktop/Personal Projects/orochi`)
**Branch:** `main` (tracks `origin/main`). All work pushed.

---

## What Exists (repo file map)
- `README.md` - Publication-ready project README. Contains intro (Orochi serpent theme), goals, open-source attributions (ZeroG Mercury One.1 + StealthChanger), V1.0/V2.0 roadmap, Mermaid system-topology diagram, multi-MCU driver matrix (T0-T5), firmware/slicer strategy, contribution guide, MIT license text.
- `LICENSE` - MIT License (copyright "Project Orochi Contributors", 2026). Note: GitHub's auto-generated LICENSE was rebased under; conflict resolved to keep this text.
- `.gitignore` - Excludes `.opencode/`, `.DS_Store`, editor/OS noise.
- `docs/V1.0-Dual-Extruder-Design.md` - Detailed V1.0 design plan: mechanical layout, SKR 1.4 Turbo pin map, power budget, Klipper file layout, calibration plan, tool-change sequence, BOM, risks, acceptance criteria. Includes a V1.0 Mermaid signal/pneumatic-flow diagram.
- `TASK_QUEUE.md` - Open / In-Progress / Done task tracking for V1.0 and V2.0.
- `CHECKPOINT_LAST.md` - This file.

## Git History (top commits)
- `4495b77` docs: add Mermaid topology to README and V1.0 dual-extruder design plan
- `0ae957c` Initial commit: Project Orochi README, MIT LICENSE, and .gitignore
- `4a51810` Initial commit (GitHub auto-generated LICENSE)

---

## Key Design Decisions (locked in planning)
- Derivative of **ZeroG Mercury One.1** (CoreXY/MGN12 conversion) + **StealthChanger** (magnetic kinematic dock). Both retain upstream licenses (CC BY-NC-SA 4.0 for ZeroG, GPL-3.0 for StealthChanger); Orochi's MIT license covers ONLY its original additions (configs, RP2040 hub design, docs).
- **V1.0 (current target):** 2-tool Bowden on a single BTT SKR 1.4 Turbo. T0/T1 use SKR E0/E1 extruders, HE0/HE1 heaters, TH0/TH1 thermistors, spare endstop ports for 2 dock microswitches.
- **V2.0 (future):** scale to 6 tools. Creality 4.2.2 board as Klipper slave MCU (its X/Y/Z/E ports remapped to extruders E2-E5, steppers only) + custom RP2040 junction PCB (4x 24V heater mosfets, 4x thermistors w/ 4.7k pull-up, dock-presence GPIO).
- **Strategy:** extreme light-weighting via static recycled Ender 3 extruders + long Bowden tubes (umbilical). Per-tool pressure advance required (differing tube lengths). Standby retract to prevent dock ooze.
- Tool changing via **klipper-toolchanger** framework.

## Open Human Decisions (need user input before build)
- Confirm exact PSU wattage on the specific Ender 5 Pro unit before loading two hotends simultaneously.
- Reverse-Bowden vs single Bowden at the extruder inlet (decide at first bench test).

---

## Next Action After Context Clear
Resume from `TASK_QUEUE.md` -> V1.0 hardware sourcing/printing. Two optional doc paths if user wants repo to carry runnable artifacts sooner:
1. Author skeleton `printer.cfg` + `T0.cfg` + `T1.cfg` + `toolchanger.cfg` (illustrative, not yet validated on hardware).
2. Begin V2.0 RP2040 hub schematic (KiCad) as a future open-source goal.

## Notes for Resuming Session
- Working directory is the `orochi` folder (its own git repo, NOT the parent "Personal Projects" repo).
- No em dashes allowed in output (project CLAUDE.md rule); verify before committing docs.
- Upstream URLs verified: ZeroG -> github.com/ZeroGDesign (docs.zerog.one); StealthChanger -> github.com/DraftShift/StealthChanger.
