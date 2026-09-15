# Grasp mechanism

Two finger levers on parallel pivots, geared to each other so they move
symmetrically. The operator squeezes them by hand.

**There is no motor and no CAN on this axis.** The levers are purely an input
device: a single magnetic rotary encoder on the lever shaft measures how far
they have been squeezed, and the console software turns that one number into the
patient-side instrument's jaw command. The two wrist axes above it are actuated;
this one is not.

## Parts

| part | file stem | role |
|---|---|---|
| Grasp lever 1 | `grasp_lever_1` | finger lever; gear teeth mesh with lever 2 |
| Grasp lever 2 | `grasp_lever_2` | finger lever; mirror of lever 1 |
| Grasp mount plate | `grasp_mount_plate` | carries the lever pair |
| Grasp pivot pins | `grasp_pin_1`, `grasp_pin_2` | lever pivots, 2 mm dia x 10.5 mm |
| Grasp sensor base | `grasp_sensor_base` | levers pivot in its centre holes; the encoder mounts on top |
| Grasp cover | `grasp_cover` | lever housing |
| MCU base | `mcu_base` | carries the XIAO board |

CAD for the two levers is in `../CAD/grasp_module/` (`grasp_lever_1`,
`grasp_lever_2`). `grasp_sensor_carrier` in the same folder holds the sensing
parts as three solids: the magnet (5 mm dia x 2 mm), the sensor IC and its PCB.
The other printed parts of the module are released as STL only.

## Kinematics

| joint | type | parent | axis | actuated |
|---|---|---|---|---|
| `gear_1_joint` | revolute | `link_2dof` | -X in the `link_2dof` frame | no — hand-driven |
| `gear_2_joint` | revolute | `link_2dof` | -X in the `link_2dof` frame | no — hand-driven |

The gear teeth constrain the pair, so one angle describes both. The software
publishes one lever angle and mirrors it onto the other (`gear_2 = -gear_1`).

**No joint limits are set in the CAD;** the travel is bounded by the parts
themselves. The URDF limits each lever joint to ±30° (0.5236 rad). Measured at
the encoder, the full travel has come out between about **20 and 36 degrees**
depending on the unit, so treat the range as something to measure per unit, not
a design constant.

The controller loads the URDF with both lever joints **locked at neutral**. That
is not bookkeeping: the dynamics library counts every unlocked joint, so leaving
the levers free changes the model's DOF count and breaks code that assumes the
arm's. Any joint added to the URDF beyond the wrist has to be locked the same way.

## Sensing

| | |
|---|---|
| encoder | AS5600-class magnetic rotary, 12-bit, 4096 counts per revolution |
| mounting | on the lever shaft, carried by `grasp_sensor_base` |
| front end | Seeed XIAO nRF52840 Sense on `mcu_base`, USB `2886:8045` |
| stream | about 167 Hz, CSV over USB CDC; the angle field is already in degrees |
| normalised output | 0.0 = fully open, 1.0 = fully closed |

Two things about this sensor decide whether the grasp feels right:

**The magnet wraps at 0/360.** The host driver unwraps the reading into a
cumulative angle, but if the lever's working range straddles the wrap point, a
squeeze right at the boundary makes the jaw jump open. Mount the magnet so the
whole travel sits away from the seam.

**The endpoints are per-unit, not per-design.** Fully-open and fully-closed
angles come from a calibration run on each assembled gripper. So do the sign and
the scale, because they depend on how the sensor is wired and which way the
magnet faces. None of these belong in the CAD or the URDF.

## Build checks

- The two levers move symmetrically. A half-tooth offset at assembly is a
  permanent bias the software cannot see.
- The encoder reading sweeps smoothly across the full lever travel. A reading
  that never moves by more than one count means the magnet is too far away or
  not centred.
- The travel does not cross the 0/360 seam.
