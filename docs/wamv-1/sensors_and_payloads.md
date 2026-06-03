# WAMV-1 Sensors and Payloads

## Navigation Sensors

WAMV-1 carries a standard set of navigation sensors for autonomous surface operations.

- **GNSS/INS**
  - Primary GNSS/INS unit mounted on a mast or elevated platform to maximize satellite visibility and reduce multipath.
  - Provides position, velocity, and attitude estimates used by the navigation and control stack.
- **Inertial sensors**
  - IMUs and/or dedicated compasses mounted close to the vehicle center of mass.
  - Provide high-rate orientation and acceleration data for stabilization and control.

Update the specific makes, models, and mounting details here once finalized.

## Perception Sensors

Depending on the mission and configuration, WAMV-1 can be fitted with cameras, LiDAR, or other perception sensors.

- **Cameras**
  - One or more RGB (and optionally depth) cameras mounted to provide forward and/or panoramic views.
  - Used for visual situational awareness, perception algorithms, or competition tasks.
- **LiDAR / other range sensors**
  - Mounting points available for LiDAR (e.g., Velodyne) or other range-sensing devices.
  - Used for obstacle detection, mapping, or docking assistance.

Describe actual sensors and their fields of view once the configuration is fixed.

## Other Payloads

WAMV-1 can carry additional payloads as needed for experiments and field work.

- Custom instrumentation, small payloads, or experimental sensors can be mounted on available rails or brackets.
- Payloads share power and data connections from the main electronics bay as appropriate.

Document any recurring or permanent payloads here.

## Calibration Notes

- Navigation and perception sensors must be calibrated periodically (e.g., compass calibration, camera intrinsics/extrinsics).
- Calibration procedures and acceptance criteria are described in [calibration_and_tests.md](calibration_and_tests.md).
- Record dates of last calibration and any important notes or offsets here for quick reference.
