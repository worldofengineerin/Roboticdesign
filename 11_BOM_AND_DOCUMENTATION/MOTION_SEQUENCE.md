# Motion sequence

Both cycles are fully defined in `00_SOURCE/robot.py` (`keyframes`) and `robot_b.py` (`keyframes_b`). Joint angles come from inverse kinematics with the tool kept vertical. Approach, lift, place and retract are straight vertical moves, sampled every 5 mm. Rendered animations of the model: `09_MOTION_SIMULATION/PICK_AND_PLACE_PACKAGE_A.mp4` and `_B.mp4`.

## Package A (the video cell): 12 s

| Phase | Time (s) | Action | J1 | J2 | J3 | J5 | Gripper |
|---|---|---|---|---|---|---|---|
| Home | 0–0.5 | Arm at home, gripper open (40 mm) | 0 | 0 | 0 | 0 | 29.6° |
| Robot movement | 0.5–2.5 | Swing over the pick fixture, 40 mm above | −31.3 | 49.4 | −37.0 | −12.4 | open |
| Approach | 2.5–3.5 | Straight down until the fingertips are 9 mm above the block | −31.3 | 53.5 | −31.5 | −22.0 | open |
| Gripping | 3.5–4.0 | Fingers close on Ø30 | | | | | 20.8° |
| Transfer | 4.0–5.0 | Straight up 60 mm (out of the 5 mm seat) | −31.3 | 48.1 | −41.1 | −7.0 | closed |
| Transfer | 5.0–7.5 | Swing to above the place fixture | +31.3 | 48.5 | −39.6 | −8.9 | closed |
| Sorting | 7.5–8.5 | Straight down; workpiece seats in the Ø31 x 12 hole, fingertips 2 mm above the block | +31.3 | 54.4 | −30.8 | −23.5 | closed |
| Release | 8.5–9.0 | Fingers open | | | | | 29.6° |
| Return | 9.0–10.0 | Straight up 60 mm | | | | | open |
| Return | 10.0–12.0 | Back to home | 0 | 0 | 0 | 0 | open |

J4 = 0 and J6 = 0 throughout. The fingers open along the radial direction.

## Package B (conveyor, 7th axis, bins): 14 s

| Phase | Time (s) | Action |
|---|---|---|
| Conveyor operation | 0–3.0 | Belt carries the workpiece from x = 0 to the stop at x = 300 (belt top z = 100) |
| Workpiece detection | 3.0 | Workpiece against the stop. The pickup signal is a sensor at the stop (not modelled). |
| Robot + axis movement | 1.0–3.0 | Carriage moves to y = −130 while the arm moves 40 mm above the pickup. J6 = J1 − 90° so the fingers open across the belt. |
| Gripping | 3.0–4.5 | Straight down, close |
| Transfer | 4.5–8.5 | Straight up 50 mm (higher would tip J5 past 0° and bring the J6 gearbox onto the forearm), then a coordinated move: carriage to y = +130 and arm over bin B |
| Sorting | 8.5–10.0 | Straight down into the bin, release 10 mm above the floor |
| Return | 10.0–14.0 | Straight up, carriage and arm back to home; the conveyor can restart |

## Setting the motion study up in SOLIDWORKS

The macro creates an empty motion study, `PICK_AND_PLACE`, in each main assembly. Add the motors by hand in about 10 minutes:

1. Load **SOLIDWORKS Motion** (Tools > Add-ins). It comes with SOLIDWORKS Premium or Simulation Professional. If you only have Standard or Professional, switch the study type to **Basic Motion**. Motors with data points work there too, but contacts and gravity are simplified.
2. For each joint, add a **Rotary Motor** on the child link's cylindrical hub face, relative to the parent link. Choose **Data Points**, then **Displacement**, then **Import** and pick the CSV:
   - Package A: `A_MOTOR_J1_deg.csv` … `A_MOTOR_J6_deg.csv`. Put the gripper motor on `GRIPPER_DRIVE_GEAR_L` relative to `GRIPPER_HOUSING_FRONT` with `A_MOTOR_GRIPPER_deg.csv`.
   - Package B: the `B_MOTOR_*` files. Add a **Linear Motor** on `LINEAR_CARRIAGE_PLATE` along Y with `B_MOTOR_CARRIAGE_mm.csv`.
   - Each file has two columns: time (s) and displacement (degrees or mm) relative to the as-built HOME pose, at 20 Hz.
3. If a joint moves the wrong way, tick **Reverse direction** on that motor. Check at t = 1 s against the animation.
4. Workpiece, Motion Analysis: turn on **Gravity** (−Z). Add **Contacts** between the workpiece and the fixture, both gripper pads and the bins. Use PLA on rubber, µs 0.8. The pads hold the workpiece once the gripper motor closes the fingers.
   - Basic Motion alternative: in Package B, drive the workpiece on the belt with a linear motor using `B_MOTOR_WORKPIECE_X_mm.csv`, and let the gripper take over at t = 4.5 s.
5. Set the study length to 12 s (A) or 14 s (B) and 50 frames per second, then calculate.

The model's collision check of the whole cycle is reported in `VALIDATION_CHECKLIST.md`. You can repeat it in SOLIDWORKS with Tools > Evaluate > Interference Detection at the keyframe poses.
