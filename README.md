# Project Orochi: Multi-Board 6-Tool Ender CoreXY Evolution

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT" />
  <img src="https://img.shields.io/badge/Status-V1.0--In%20Development-blue.svg" alt="Status: V1.0 In Development" />
  <img src="https://img.shields.io/badge/Firmware-Klipper-red.svg" alt="Firmware: Klipper" />
  <img src="https://img.shields.io/badge/Built%20on-ZeroG%20Mercury%20One.1-9cf.svg" alt="Built on ZeroG Mercury One.1" />
  <img src="https://img.shields.io/badge/Toolchanger-StealthChanger-9cf.svg" alt="Toolchanger: StealthChanger" />
</p>

<p align="center">
  <i>A budget-friendly, multi-tool 3D printer that grows from a humble Ender 5 Pro into a six-headed fabrication serpent.</i>
</p>

---

## Table of Contents

- [Introduction](#introduction)
- [Core Project Goals](#core-project-goals)
- [Open Source Attributions](#open-source-attributions)
- [Scoped Development Roadmap](#scoped-development-roadmap)
- [Hardware Architecture Breakdown](#hardware-architecture-breakdown)
- [Multi-MCU Driver Matrix](#multi-mcu-driver-matrix)
- [Firmware and Slicer Strategies](#firmware-and-slicer-strategies)
- [Open Source Contribution Guide](#open-source-contribution-guide)
- [License](#license)
- [Safety and Disclaimer](#safety-and-disclaimer)

---

## Introduction

In Japanese mythology, **Orochi** (八岐大蛇) is the eight-headed serpent of Koshi, a single coiled body from which many heads strike at once. Project Orochi channels that same idea into a 3D printer: a single, rock-solid **Ender 5 Pro frame** that drives a swarm of independent toolheads. One gantry, one brain cluster, and up to six hotends that can be swapped mid-print to mix colors and materials without a single manual intervention.

This is not a commercial machine wearing a custom skin. It is a deliberately low-cost, highly recyclable build that takes two proven community frameworks, mates them to stock Creality hardware, and scales out using repurposed control boards instead of expensive dedicated toolchanger electronics. The result is a printer that strikes like the serpent: light, fast, and far more capable than the sum of its budget parts would suggest.

The defining engineering decision is **extreme light-weighting through Bowden extrusion**. By mounting the heavy extruder steppers statically to the frame and feeding the toolheads through long Bowden tubes, the moving carriage stays feather-light. That keeps inertial loads on the CoreXY gantry tiny, which preserves speed and ringing performance even as the tool count climbs.

---

## Core Project Goals

Project Orochi is guided by four non-negotiable principles that shape every hardware and software decision.

- **Cost efficiency above all.** The machine is built from a stock Ender 5 Pro, off-the-shelf MGN12 rails, and recycled controllers. No premium toolhead boards, no CAN bus toolboards, no boutique MCUs are required to reach a six-tool configuration.
- **Recycle stock parts.** Full-sized extruders and stepper motors salvaged from retired Ender 3 machines are reused as the primary extrusion system. Old Creality control boards are brought back to life as slave motion controllers rather than ending up in e-waste.
- **Extreme light-weighting via Bowden.** Keeping every extruder motor off the moving gantry means the docking toolheads carry only a hotend, a groove-mount plate, and a Bowden umbilical. Lower mass equals higher acceleration headroom and better input shaper results.
- **Massive scaling without high-end electronics.** The six-tool target is reached by distributing stepper control across multiple low-cost MCUs and a custom RP2040 hub, proving that a serious toolchanger does not require flagship hardware.

---

## Open Source Attributions

Project Orochi is a **derivative work** and stands on the shoulders of two landmark community projects. We are deeply grateful to their creators and maintainers, and we encourage every Orochi builder to star and support the upstream repositories.

| Project | Role in Orochi | Repository |
| --- | --- | --- |
| **ZeroG Mercury One.1** | CoreXY linear-rail motion conversion for the Ender 5 frame. Orochi reuses the Mercury One.1 gantry, X/Y joints, and belt path design. | [github.com/ZeroGDesign](https://github.com/ZeroGDesign) · [docs.zerog.one](https://docs.zerog.one/) |
| **StealthChanger** | Magnetic kinematic tool-docking system. Orochi uses the StealthChanger shuttle, backplate, and dock concept to detach and park toolheads. | [github.com/DraftShift/StealthChanger](https://github.com/DraftShift/StealthChanger) |

> **Licensing note for builders:** The Mercury One.1 and StealthChanger designs carry their own upstream licenses (CC BY-NC-SA 4.0 for ZeroG documentation/designs and GPL-3.0 for StealthChanger). Orochi's *original* contributions, such as its Klipper configuration, the custom RP2040 hub design, and this documentation, are released under the MIT License defined below. When you print or modify upstream CAD, you remain bound by those upstream licenses. The MIT license in this repository covers Orochi-specific additions only.

---

## Scoped Development Roadmap

The project is intentionally staged so that a working printer exists early and capability is added incrementally. Each version is a superset of the previous one.

### Version 1.0 (Current Target): 2-Tool Bowden Setup

The first milestone delivers a fully functional, printable, and documented two-tool machine using only the **BTT SKR 1.4 Turbo** as the sole MCU.

- CoreXY motion conversion complete using MGN12 rails.
- StealthChanger shuttle and two docks installed on the frame.
- Two lightweight V6-style groove-mount hotends on tool plates.
- Two recycled Ender 3 extruders statically mounted, feeding T0 and T1 over Bowden tubes.
- Klipper managing CoreXY kinematics, tool detection, and macro-driven swapping.
- Single-board wiring: no slave board, no custom PCB yet required.

This stage proves out the kinematics, the docking geometry, and the Bowden strategy before any additional electronics are introduced.

### Version 2.0 (Future Expansion): 6-Tool Multi-Board System

The second milestone scales the platform to its full six-tool vision by bringing a second control board and a custom hub online.

- **Creality 4.2.2 board** promoted to a Klipper slave MCU, with its X, Y, Z, and E stepper ports repurposed to drive extruders E2, E3, E4, and E5.
- Four additional StealthChanger docks and tool plates added to the frame.
- **Custom RP2040 junction PCB** (the Orochi "thermal and docking hub") manufactured and integrated, providing heater mosfets, thermistor inputs, and dock-presence GPIO for the expanded toolset.
- Multi-MCU coordination handled entirely by Klipper, with unified tool macros covering all six tools.

---

## Hardware Architecture Breakdown

### System Topology

At the heart of Orochi is a **multi-MCU topology** coordinated by a single Klipper host. The Raspberry Pi runs Klipper, Moonraker, and a web interface (Mainsail or Fluidd). Every controller board connects to the Pi over USB and is addressed as an independent `[mcu]` section in the Klipper config.

```
                         +---------------------------+
                         |   Raspberry Pi (Host)     |
                         |  Klipper + Moonraker + UI |
                         +---------------------------+
                            |           |           |
                 USB        |   USB     |   USB     |  (future)
                            v           v           v
                  +----------------+ +----------------+ +------------------+
                  | BTT SKR 1.4    | | Creality 4.2.2 | | RP2040 Junction  |
                  | Turbo (Master) | | (Slave MCU)    | | Hub (Future V2)  |
                  +----------------+ +----------------+ +------------------+
                   X, Y (CoreXY)      X->E2           4x 24V heater mosfets
                   Z (bed)            Y->E3           4x thermistor inputs
                   E0 -> T0 extruder  Z->E4           4x dock microswitch GPIO
                   E1 -> T1 extruder  E ->E5
                   T0/T1 heaters      (steppers only)
                   T0/T1 thermistors
```

### Component Roles

- **Raspberry Pi.** Host computer running the Klipper firmware process, Moonraker API, and the web front end. It is the single source of scheduling truth for all tool changes.
- **BTT SKR 1.4 Turbo (Master MCU).** The primary board. Drives the CoreXY X and Y motors, the Z bed axis, and the first two extruders (T0, T1). TMC2209 drivers run in UART mode for current, stealthChop, and sensorless-homing control. In Version 1.0 this is the only board required.
- **Creality 4.2.2 Board (Slave MCU).** A recycled stock board promoted to a Klipper secondary MCU in Version 2.0. Its four onboard stepper drivers (natively X, Y, Z, E) are remapped in Klipper to run extruders E2 through E5. The stock Vref tuning profiles are reused, avoiding a fresh driver calibration pass.
- **Custom RP2040 Junction PCB (Future Open-Source Goal).** A purpose-built "thermal and docking hub" inspired by the timmit99 Klipper Expander. It connects to the Pi over USB and provides:
  - 4x opto-isolated 24V heater mosfet outputs for the expanded tool hotends.
  - 4x thermistor ports with 4.7k pull-up resistors for temperature sensing.
  - GPIO breakout dedicated to dock-presence microswitches for tool-in-dock detection.
  The RP2040 offloads heater and sensor management from the stepper MCUs, keeping each board's job clean and the wiring modular.

### Mechanical Summary

- Frame: stock Ender 5 Pro cube, retained as the structural body.
- Motion: Mercury One.1 CoreXY conversion on MGN12 linear rails.
- Toolheads: lightweight V6-style groove-mount hotends on StealthChanger tool plates.
- Umbilicals: standard long Bowden tubes double as the tool umbilical, carrying filament from the static extruders to each docked hotend.

---

## Multi-MCU Driver Matrix

The table below maps all six tools across the board topology planned for Version 2.0. In Version 1.0, only rows T0 and T1 are populated and everything lives on the SKR 1.4 Turbo.

| Tool | Extruder Stepper | Controlled By | Board Port (Remapped) | Heater | Thermistor | Dock Sensor |
| --- | --- | --- | --- | --- | --- | --- |
| **T0** | E0 | SKR 1.4 Turbo (master) | E0 driver | SKR `heater0` | SKR `TH0` | SKR endstop |
| **T1** | E1 | SKR 1.4 Turbo (master) | E1 driver | SKR `heater1` | SKR `TH1` | SKR endstop |
| **T2** | E2 | Creality 4.2.2 (slave) | X driver | RP2040 mosfet 1 | RP2040 therm 1 | RP2040 GPIO 1 |
| **T3** | E3 | Creality 4.2.2 (slave) | Y driver | RP2040 mosfet 2 | RP2040 therm 2 | RP2040 GPIO 2 |
| **T4** | E4 | Creality 4.2.2 (slave) | Z driver | RP2040 mosfet 3 | RP2040 therm 3 | RP2040 GPIO 3 |
| **T5** | E5 | Creality 4.2.2 (slave) | E driver | RP2040 mosfet 4 | RP2040 therm 4 | RP2040 GPIO 4 |

Notes for builders:

- Each MCU is declared in `printer.cfg` with its own `[mcu]` block and a unique `serial` (or `usb_serial`) path. Klipper fans, heaters, and steppers then reference pins prefixed with the MCU name, for example `stepper_e2: sc:PB9` where `sc` is the slave MCU alias.
- The Creality 4.2.2 board contributes **steppers only** in this design. Heater and thermistor duties for T2 through T5 are handled by the RP2040 hub so that no high-current or analog sensing is forced through the repurposed 8-bit-class board.
- Tool detection and dock presence are read by the RP2040 GPIO (Version 2.0) or the SKR endstops (Version 1.0), feeding the klipper-toolchanger framework.

---

## Firmware and Slicer Strategies

Orochi runs **entirely on Klipper**, which is what makes the multi-MCU design feasible. Klipper treats each board as an independent real-time MCU while the host orchestrates motion, tool changes, and macros.

### Klipper Multi-MCU Coordination

- Each board is a separate `[mcu]` section with its own USB serial path, so the SKR, the Creality slave, and (in V2) the RP2040 hub all share one `printer.cfg`.
- The CoreXY kinematics, Z bed, and extruders are defined against their respective MCU pin aliases.
- Tool changing is managed by the **klipper-toolchanger** framework (the standard companion to StealthChanger), which tracks tool state, dock positions, and gcode offsets per tool.

### Tool-Changing Macros

Macro-driven sequences keep the swap deterministic and recoverable:

- `T0` through `T5` select the active tool, park the previous one at its dock, and pick up the new one via the StealthChanger shuttle.
- Dock approach and departure use slow, constrained moves so the magnetic kinematic coupling engages and releases cleanly.
- A `PARK_TOOL` / `PICKUP_TOOL` pair forms the backbone of every swap, with crash detection wrapping the sequence so a missed dock does not wreck the gantry.

### Pressure Advance for Long Bowdens

Long Bowden tubes introduce significant filament compression lag, which shows up as rounded corners and blobby starts. Each tool gets its own `pressure_advance` value tuned to the length and friction of its specific Bowden path. Because the tubes differ in length per dock, **per-tool pressure advance is mandatory** rather than a single global value.

```ini
# Example per-tool pressure advance (tune per physical Bowden length)
[extruder]
pressure_advance: 0.55
pressure_advance_smooth_time: 0.03
```

### Standby Retraction to Prevent Dock Oozing

A parked hotend left loaded and hot will ooze, leaving blobs in the dock and on the next pickup. Orochi uses standby retraction: when a tool is parked, a small retract is issued and the hotend is dropped to a standby temperature, then re-primed on pickup.

```ini
# Conceptual standby handling inside the park macro
SET_IDLE_TIMEOUT TIMEOUT=3600
SET_GCODE_OFFSET Z=0  ; reset tool offset on park
G92 E0
G1 E-3 F1800          ; retract to avoid ooze in dock
SET_HEATER_TEMPERATURE HEATER=t1 TARGET=150  ; standby temp
```

Combined with a brief purge and wipe on pickup, this keeps the docks clean and the first layer of each tool consistent.

### Slicer Configuration

- Configure one extruder per tool in the slicer (PrusaSlicer/SuperSlicer or Orca Slicer), matching the `T0` to `T5` indices.
- Use the StealthChanger slicer guidance for tool change G-code so the slicer emits the correct `T` commands and standby/prime behavior.
- Assign filament and purge volumes per tool so color and material swaps purge adequately into a chute or brush rather than onto the part.

---

## Open Source Contribution Guide

Orochi is a community project and contributions are welcome. Whether you are fixing a typo, improving a macro, sharing a printed part, or designing the RP2040 hub, the process is the same.

1. **Fork and branch.** Create a feature branch off `main` with a descriptive name (for example `feat/rp2040-hub-schematic`).
2. **Keep it scoped.** One logical change per pull request. Documentation, config, and hardware each belong in their own PR when possible.
3. **Test before you push.** If you change `printer.cfg` or macros, validate them on real hardware or at minimum with `klipper` config checks. Note the hardware version (V1.0 or V2.0) your change targets.
4. **Respect upstream licenses.** Do not embed Mercury One.1 or StealthChanger CAD or config verbatim without attribution. Link to upstream and mark derivative files clearly.
5. **Open the PR.** Describe what changed, why, and which tools/boards it affects. Reference any related issues.

Please be kind and constructive in reviews. This is a budget-build project meant to lower the barrier to multi-material printing, so clarity for newcomers matters as much as technical correctness.

---

## License

Project Orochi is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2026 Project Orochi Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

As noted in [Open Source Attributions](#open-source-attributions), this MIT license covers Orochi's original work only. The Mercury One.1 and StealthChanger upstream designs retain their own licenses, and builders must comply with those when using or modifying the underlying CAD and configurations.

---

## Safety and Disclaimer

This is a do-it-yourself hardware project involving mains-adjacent 24V systems, heated surfaces, and moving machinery. The authors and contributors are not responsible for any damage, injury, or fire resulting from building or operating an Orochi printer.

- Verify all wiring and polarity before applying power.
- Never leave a printing toolchanger unattended, especially during early commissioning.
- The custom RP2040 hub (heater mosfets, thermistors) is an unreleased design goal. Do not manufacture or connect high-current heater circuits without proper isolation, fusing, and testing.
- Respect the upstream licenses of Mercury One.1 and StealthChanger when sharing derivatives.

Build safely, print boldly, and let the serpent strike.
