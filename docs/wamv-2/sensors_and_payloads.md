# WAMV-2 Sensors and Payloads

## Navigation Sensors

WAMV-2 carries a navigation sensor suite similar to WAMV-1 for autonomous surface operations.

- **GNSS/INS**
  - Primary GNSS/INS unit mounted on a mast or elevated platform to maximize satellite visibility and reduce multipath.
  - Provides position, velocity, and attitude estimates used by the navigation and control stack.
- **Inertial sensors**
  - IMUs and/or dedicated compasses mounted near the vehicle center of mass.
  - Provide high-rate orientation and acceleration data for stabilization and control.

Update the specific makes, models, and mounting details here once finalized.

## Environmental Sensors

WAMV-2 includes additional environmental sensors not present on WAMV-1.

- **Depth sensor**
  - Under-hull or through-hull depth sensor mounted to provide reliable depth/bathymetry measurements.
  - Document model, range, update rate, and any relevant installation notes.
- **Weather station**
  - Weather station mounted on a mast or bracket with clear exposure.
  - Measures variables such as wind speed/direction, temperature, humidity, and pressure (update as appropriate).

## Payloads

WAMV-2 can carry additional payloads for experiments and demonstrations.

- **Waterwheel**
  - Mounted on a dedicated bracket or frame when installed.
  - Describe the purpose (e.g., energy harvesting, drag experiments) and any instrumentation associated with it.
- **Drone platform**
  - Rigid platform or pad used for operating a small drone from WAMV-2.
  - Note any interfaces for power, communications, or tracking if present.

## Calibration Notes

- Environmental sensors (depth, weather) require initial calibration and periodic checks.
- Depth sensor calibration may involve comparing readings against known depths or reference instruments.
- Weather station calibration may involve cross-checking with nearby reference stations or manufacturer procedures.
- See [calibration_and_tests.md](calibration_and_tests.md) for detailed procedures and acceptance criteria.
