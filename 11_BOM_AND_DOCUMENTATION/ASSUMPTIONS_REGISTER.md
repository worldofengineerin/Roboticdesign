# Assumptions and dimensions register (Rev B)

No dimension in this project has been measured. The reference video has no scale. Every value is **E** (estimated from video proportions), **C** (calculated from other values or from standards) or **S** (supplied or accepted by the client: the defaults approved on 1 Nov 2026). All values live in `00_SOURCE/params.json`. Changing a value there and re-running the generators updates every part, assembly, macro, drawing and print file.

## Key dimensions

| # | Parameter | Value (mm) | Status | Critical interface | Notes |
|---|---|---|---|---|---|
| D01 | Reach, J2 axis to tool flange, arm stretched | 467 (200 + 180 + 87) | C | – | Client default was 450. It grew because D07 grew. |
| D02 | Robot base plate | 240 x 240 x 8, 4x M5 on 210 square | E | Table / carriage hole pattern | Grew from 200 so the J1 motor fits on the plate |
| D03 | Pedestal Ø / height | 160 / 90 | E | 6808 + 51108 bearing seats | |
| D04 | J2 axis height above table | 190 | E | – | |
| D05 | Upper arm, J2 to J3 | 200 | E | – | |
| D06 | Forearm, J3 to J5 | 180 | E | – | J4 gearbox sits in this length |
| D07 | J5 axis to tool flange face | 87 | C | Tool flange ISO 9409-1-31.5-4-M4 style | Rev A said 60. The J6 motor and gearbox must sit outside the R44 J5 fork hub. |
| D08 | Gripper length, flange to fingertip | 115.6 closed / 113.3 open | C | – | Rev A said 110 |
| D09 | Gripper opening | 4.4 to 40 | C | Pads must clear the workpiece by more than 5 mm | Crank 0° to 29.6°. It holds Ø30 at 20.8°. |
| D10 | Joint shafts | Ø15 gearbox output shafts (6802), Ø8 idler pins | C | Bearing and hub bores | Rev A said Ø8. The gearbox output is now the joint shaft. |
| D11 | Workpiece | Ø30 x 30, Ø16 bore | E | Fixture seats | |
| D12 | Pick seat / place hole | Ø31 x 5 / Ø31 x 12 with 2 mm chamfer | C | 0.5 mm radial clearance | |
| D13 | Joint gearbox | m1, sun 12 / planets 3 x 24 / ring 60, 6:1 | C | Ring = sun + 2 x planet; (60+12)/3 = 24, so 3 planets fit evenly | |
| D14 | Gearbox housing | Ø84 x 35, 6x M4 on PCD 74 | C | Cheek hubs R44 | Rev A said Ø76. Grown for a 3 mm wall between tooth root and bolt. |
| D15 | Printed bearing seat | bearing OD + 0.10 | C | Tune with a test print | |
| D16 | Printed pin hole | pin + 0.20 | C | | |
| D17 | Minimum printed wall | 2.4 (6 x 0.4 mm lines) | C | | Gear teeth are the exception (tooth tips about 1 mm) |
| D18 | Print bed | 220 x 220 x 250 | S | All 42 printed parts fit | Upper arm is printed in two halves |
| D19 | J1 drive | m1.5, ring 96 / pinion 16, 6:1, centre distance 84.15 | C | 0.15 mm backlash | |
| D20 | Motor | NEMA 17, 42.3 square x 48, Ø5 D-shaft, Ø22 pilot | S | Catalogue values | 7 motors in the robot, plus 1 linear axis and 1 conveyor |

## Assumptions to confirm

- Payload of 200 g or less, and a workpiece of Ø30 x 30. Supplied default.
- Each joint gearbox is a single-stage 6:1 planetary, as in the video's exploded view. The ratio cannot be read from the video.
- The gripper is driven by a micro servo (MG90S class) through two meshed gears. The video does not show the actuator.
- The cable harness is shown only as clamps. Cable routing is not modelled.
- Fasteners are ISO standard parts (SOLIDWORKS Toolbox). They are not modelled as custom parts.

## OPEN ISSUE: shoulder and elbow torque (approval needed)

A static check on the model gives these figures. It uses printed parts at 55 % effective density, NEMA 17 motors at 0.36 kg and a 200 g payload.

| Joint | Worst static torque in the pick-and-place cycle | Fully stretched | Available with 6:1 (0.45 N·m holding, 80 % efficiency) |
|---|---|---|---|
| J2 (2 motors) | 7.0 N·m | 8.3 N·m | 4.3 N·m |
| J3 | 2.8 N·m | 2.8 N·m | 2.2 N·m |

**As modelled, J2 and J3 cannot hold the arm.** The video design probably uses a higher reduction than one 6:1 stage. Options for approval:

1. Two-stage planetary at J2 and J3 (36:1). The housing gets about 15 mm longer and the Ø84 diameter stays. Recommended.
2. A NEMA 23 at J2 with a two-stage gearbox at J3.
3. A spring counterbalance on J2.

Stepper torque also falls with speed, so the dynamic margin must be checked once the cycle time is fixed.

## Changes since Rev A (design basis doc)

- Motors: 7, not 6. J2 has a motor on each side, as in the video.
- Gearbox Ø76 → Ø84. Joint shafts are now the Ø15 gearbox output shafts.
- D07 60 → 87, D08 110 → 115.6, base plate 200 → 240.
- Gripper actuator: MG996R class → MG90S class, to fit between the side plates.
- Upper arm printed as two halves, `_A` and `_B`, joined by 4x M5 and 2x Ø6 dowels.
- Pick and place fingertip heights changed so the fingers stay above both fixtures (found by the collision check).
