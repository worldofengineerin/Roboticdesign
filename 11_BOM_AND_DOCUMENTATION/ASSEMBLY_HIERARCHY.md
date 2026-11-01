# Assembly hierarchy and mate scheme

```
MAIN_SYSTEM.SLDASM                      Package A: the cell from the video   (07_MAIN_ASSEMBLY)
├── MOUNTING_STRUCTURE.SLDASM          FIXED. Table, pick and place fixtures, 4 levelling feet
├── ROBOT_ASSEMBLY.SLDASM              flexible. Locked to the table through ROBOT_BASE_PLATE
│   ├── PLANETARY_GEARBOX_J2.SLDASM x2 flexible (long output shaft)  J2 left and right
│   └── PLANETARY_GEARBOX.SLDASM x4    flexible                       J3, J4, J5, J6
├── GRIPPER_ASSEMBLY.SLDASM            flexible. Locked to ROBOT_TOOL_FLANGE
└── WORKPIECE_CYLINDER                 free (6 DOF) for Motion Analysis contact

MAIN_SYSTEM_FULL.SLDASM                 Package B: adds the brief's extras
├── MOUNTING_STRUCTURE_B.SLDASM        FIXED. 900 x 1000 table, 3 sorting bins, guard posts and panels
├── LINEAR_AXIS_ASSEMBLY.SLDASM        flexible. Locked to the table
├── CONVEYOR_ASSEMBLY.SLDASM           flexible. Locked to the table
├── ROBOT_ASSEMBLY.SLDASM              flexible. Locked to LINEAR_CARRIAGE_PLATE
├── GRIPPER_ASSEMBLY.SLDASM            flexible. Locked to ROBOT_TOOL_FLANGE
└── WORKPIECE_CYLINDER                 free
```

In each assembly only one component is fixed: the table in the main assemblies, and the first part in each subassembly. Parts that do not move relative to each other use a **Lock** mate to the main part of their link. Moving joints use reference axes (`AX_*`) and the renamed standard planes `PL_XY`, `PL_XZ` and `PL_YZ`, so every mate is independent of SOLIDWORKS' language setting.

## Degrees of freedom

| Joint | Mates | Free motion |
|---|---|---|
| J1 base | Coincident `AX_J1` (pedestal / turntable) + distance 100 between the `PL_XY` planes; gear mate between pinion and ring, 16:96 | Rotation about Z |
| J2 shoulder | Coincident `AX_J2` + coincident `PL_XZ` | Rotation about Y |
| J3 elbow | Coincident `AX_J3` + coincident `PL_XZ` | Rotation about Y |
| J4 forearm roll | Coincident `AX_J4` + distance 80 between the `PL_YZ` planes | Rotation about the forearm axis |
| J5 wrist | Coincident `AX_J5` + coincident `PL_XZ` | Rotation about Y |
| J6 tool roll | Coincident `AX_J6` + distance 81 between the `PL_XY` planes | Rotation about the tool axis |
| Gearboxes | Housing locked to the parent link, carrier locked to the child link; inside: sun / carrier / planets revolute, gear mates sun–planet (12:24) and planet–ring (24:60) | Internal gears follow the joint |
| Gripper | Drive gears revolute in the housing, gear mate 20:20 (synchronised fingers), limit angle 0–30° on the crank; four-bar links revolute; fingers revolute on the crank eyes, coaxial with the link ends | One DOF: open / close |
| Linear axis (B) | Distance mates to fix the carriage in X and Z; limit distance 210–510 mm (±150 mm travel); screw mate T8 lead 8 mm | Translation along Y |
| Conveyor (B) | Drive roller and idler roller revolute; gear mate 1:1 stands in for the belt | Roller rotation |

## Joint limits for the controller (design estimates)

J1 ±150°, J2 −45° to +90°, J3 −120° to +60°, J4 ±180°, J5 ±110°, J6 ±360°. The pick-and-place cycle uses J1 ±31.3°, J2 0 to 54.4°, J3 −41.1° to 0, J5 −23.5° to 0 (Package A).

Angle mates in SOLIDWORKS only range from 0 to 180°, so limit-angle mates are used only where the range is one-sided (the gripper crank). The joint limits above are enforced by the motion profile.
