# TowFish Hardware

## Structure and Mounting

The TowFish is a towed underwater sensor body designed to carry magnetic, electric-field, attitude, pressure, and health-monitoring electronics. It is intended to be towed behind a surface vessel or autonomous surface vehicle so that sensitive sensors are separated from the towing platform's motors, batteries, and ferromagnetic materials.

The platform consists of a streamlined tow body with internal pressure-vessel electronics and external stabilizing features. The body is configured to support hydrodynamic stability while maintaining adequate internal volume for electronics, batteries, connectors, and payload wiring.

Important hardware features include:

- Main TowFish body / pressure-vessel assembly.
- Tow cable used for both mechanical towing and electrical/telemetry connections.
- Stabilizing fins for passive directional stability.
- Internal electronics mounting structure.
- Bulkhead connectors and interconnect wiring between pressure vessels and external sensors.
- Ballast and buoyancy elements used to tune trim and stability.

## Tow Attachment

The TowFish is towed using a cable that also carries electrical and/or telemetry connections. The towing cable must be treated as both a mechanical load path and an electrical harness. During deployment, avoid sharp bends, kinks, crushing, or uncontrolled twisting.

Operational considerations:

- Inspect the tow cable before every deployment.
- Verify strain relief at the TowFish and tow-vehicle ends.
- Avoid applying tow loads through delicate signal connectors.
- Monitor cable angle and tension during turns.
- Avoid sudden acceleration, high-speed turns, and propeller contact.

## Stability and Trim

TowFish stability is important because magnetic and electric-field data quality depends on the position and attitude of the sensors. Excessive roll, pitch, yaw oscillation, or spinning can make magnetic anomalies harder to interpret and can complicate post-processing.

Stability is evaluated using onboard IMU data. Important motion indicators include:

- Roll angle and roll oscillation.
- Pitch angle and pitch oscillation.
- Yaw stability.
- Linear acceleration.
- Gyroscope output.
- Pressure/depth response during tow.

Past testing has shown that tow speed, ballast, buoyancy, tether length, and fin condition can all affect motion stability.

## Payload Mounts

The TowFish supports multiple payload and sensor systems:

- Twinleaf / QuSpin magnetometer system.
- BNO055 IMU attitude sensor.
- MS5837 pressure/depth sensor.
- Electric-field sensing channels.
- Leak detection and temperature monitoring.
- Battery-voltage monitoring.

The magnetometer payload is connected through a Twinleaf microSAM-style interface using a CAT5/RJ45-style cable into an Ethernet-to-USB-C adapter. The RJ45 connector is used as a physical connector and does not imply normal Ethernet networking.

## Pressure Vessel Notes

The TowFish electronics are housed in pressure-vessel sections. Opening a pressure vessel should be treated as a maintenance operation, not a routine field adjustment.

Before deployment:

- Confirm pressure vessels are closed and sealed.
- Check O-rings and sealing surfaces when opened.
- Verify desiccant and moisture protection if installed.
- Confirm leak sensor output before entering the water.
- Confirm cable penetrators and bulkhead connectors are secure.

## Handling Notes

- Do not lift the TowFish by sensor cables.
- Protect fins during transport and recovery.
- Keep magnetic tools, steel parts, and high-current electronics away from the magnetometer during calibration and baseline checks.
- Do not tow until live telemetry has been verified.
