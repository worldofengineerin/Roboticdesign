# Final validation checklist (brief section 9)

**Verified here** = checked on the Python model in this package, with results stored in `export_verification.json` and `00_SOURCE/checks_result.json`.
**Pending (SW)** = can only be confirmed after the SOLIDWORKS macro run, on your machine.

## Model check
| Item | Result |
|---|---|
| Every identified component modelled | Verified here. 72 unique parts; the inventory is in `COMPONENT_REGISTER.csv`. Fasteners are Toolbox items (BOM sheet). |
| No missing physical components | Verified here for the moving and structural parts. Fasteners and cables are not modelled. |
| Valid solids, one body per part | Verified here: 72 of 72 valid, single solid |
| Feature sequence reproduces each part | Verified here. Each part's SOLIDWORKS feature list was replayed in CadQuery and matches the part exactly, except gear tips (arc instead of chord, under 2 mm³). |
| No unresolved sketches / rebuild errors | Pending (SW). The build log reports every feature. |

## Assembly check
| Item | Result |
|---|---|
| Subassemblies positioned | Verified here (HOME pose). In SOLIDWORKS each placement is checked against its bounding-box centre. |
| Only the base fixed; correct DOF | Mate scheme is in `ASSEMBLY_HIERARCHY.md`. Pending (SW): drag-test each joint. |
| No unintended interference: static | Verified here. All-pairs check of every component at all 105 keyframe poses of both cycles: **0 interferences.** The only residue is 0.5–4 mm³ of mesh-tessellation noise at the zero-clearance 6808/51108 bearing bores. |
| No interference while moving | Verified here. Swept check every 0.125 s through the whole 12 s cycle (109 poses) and 14 s cycle (131 poses), every pair of rigid groups: **0 interferences.** |
| Problems found and fixed by these checks | Motor and connector clash in the J1 bracket; carrier shafts too long; idler pins too long; cable clamp on the fork; gripper links clashing with the fingers; fingers hitting the place fixture; workpiece dragged into the conveyor stop; J6 gearbox striking the forearm fork; conveyor cross members through the belt; coupling and shaft overlaps |

## Motion check
| Item | Result |
|---|---|
| Joints, carriage, conveyor, gripper move as intended | Verified here in the kinematic model and the animations (`09_MOTION_SIMULATION/*.mp4`) |
| Workpiece picked and released, full sequence | Verified here (kinematic). Physical contact and gravity: Pending (SW Motion Analysis). |
| Joint torque | **Fails as modelled:** J2 needs 7.0 N·m and has 4.3 available; J3 needs 2.8 and has 2.2. See ASSUMPTIONS_REGISTER. Awaiting a decision. |

## 3D printing check
| Item | Result |
|---|---|
| Watertight meshes (STL and 3MF) | Verified here: 42 of 42 |
| No zero-thickness geometry; walls | Verified here. 2nd-percentile wall is 2.4 mm or more, except gear teeth (1.2–1.3 mm) and pad ribs (0.8 mm). |
| Clearances | Bearing seats +0.1 mm, pins +0.2 mm, moving gaps 0.4 mm or more, J1 gear backlash 0.15 mm |
| Bed fit 220 x 220 x 250 | Verified here: 42 of 42 |

## File check
| Item | Result |
|---|---|
| STEP for every part, plus assemblies | Verified here (72 + 2) |
| Native SLDPRT / SLDASM / SLDDRW exist and rebuild | **Pending (SW).** Not created yet. The macro lint shows no syntax problems, but the macros have not been executed. |
