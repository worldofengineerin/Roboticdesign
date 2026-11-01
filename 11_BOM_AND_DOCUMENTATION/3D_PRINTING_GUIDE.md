# 3D printing guide

There are 42 printable parts: 31 in Package A and 11 in Package B. Each one is in `10_3D_PRINT_FILES/STL` and `/3MF`, already turned to its print orientation and placed on the bed at z = 0. All 42 meshes were checked here: watertight, consistent winding, volume within 0.2 % of the CAD solid, and every part fits a 220 x 220 x 250 mm bed (`export_verification.json`).

## Material and settings by group

| Group | Parts | Material | Walls / infill | Layer | Supports |
|---|---|---|---|---|---|
| Structural links | Pedestal, turntable, shoulder, upper arm A/B, elbow, forearm, wrist, brackets | PETG (ASA if the cell is warm) | 4 walls, 40 % gyroid | 0.2 mm | Only under motor and bearing pockets that face down |
| Gears | J1 ring and pinion, gearbox sun / planets / ring, carrier, gripper drive gears | PA-CF best; PETG acceptable for a prototype | 100 % | 0.12–0.16 mm | None (printed flat) |
| Gearbox housings | Input housing, output cover | PETG | 5 walls, 50 % | 0.2 mm | None |
| Small functional parts | Links, fingers, mount plate, gripper housing, clamps, covers, fixtures | PETG | 4 walls, 40 % | 0.16 mm | None |
| Grip pads | `GRIPPER_PAD` x2 | TPU 95A | 3 walls, 30 % | 0.2 mm | None, ribs up |
| Workpiece | `WORKPIECE_CYLINDER` | PLA | 3 walls, 20 % | 0.2 mm | None |
| Package B | Carriage, end supports, brackets, rollers, tensioners, legs, stop, bins, end stops | PETG | 4 walls, 30–40 % | 0.2 mm | Rollers print upright; the stop prints on its back |

## Fits and features

- **Bearing seats** are bearing OD + 0.10 mm, so 6808 → Ø52.1, 51108 → Ø60.1, 6802 → Ø24.1 and 608 → Ø22.1. Print `GBX_OUTPUT_COVER` first as a fit test and adjust the offset in `params.json` (`bearing_fit`) if needed.
- **Pins** go into holes at pin + 0.2 mm (`pin_clear`). Pins are Ø3 in the gripper, Ø5 for planets and Ø8 for idlers, all ISO 2338 m6.
- **Threads.** Never print threads. Use heat-set inserts: M3 (Ø4.0 hole), M4 (Ø5.6) and M5 (Ø6.4). Through-holes are Ø3.4 / 4.5 / 5.5.
- **Gears.** Module 1 is the smallest module that prints reliably with a 0.4 mm nozzle. There is 0.15 mm backlash on the J1 gear pair. Planetary gears are modelled with zero backlash. If they bind, scale the planet tooth thickness down by 0.1 mm in the slicer (horizontal expansion −0.05).
- **Upper arm.** It is longer than the bed (280 mm), so it is split at z = 45 mm into `ROBOT_UPPER_ARM_A` and `ROBOT_UPPER_ARM_B`. Join them with 4x M5 x 40 into heat-set inserts in B and 2x Ø6 x 16 dowels. Print both halves on their side for strength along the arm.
- **Minimum wall** is 2.4 mm everywhere except gear tooth tips (1.0–1.3 mm) and the TPU pad ribs (0.8 mm, intended).
- **Fillets** of 2–6 mm are modelled at bracket roots and link corners. Overhangs stay at 45° or less, except bridged holes, which print without support up to Ø16.

## Bought parts (do not print)

NEMA 17 steppers (7, plus 2 in Package B), bearings (6808, 51108, 12x 6802, 18x MR105, 608 in Package B), Ø8/Ø5/Ø3 dowel pins, the MG90S servo, MGN12 rails and blocks, the T8x8 lead screw and nut, couplings, 2020/2040 extrusions, the belt and all ISO fasteners. See `BOM.xlsx` for the full list.

Rough estimate: Package A is about 73 h of printing and 2.7 kg of filament; Package B is about 16 h and 0.6 kg. This assumes 55 % effective density and an average flow of 8 mm³/s with a 0.4 mm nozzle. Your slicer's estimate will be more accurate.
