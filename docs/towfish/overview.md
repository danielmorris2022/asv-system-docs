# TowFish Overview

The TowFish is a towed underwater sensor platform used for electromagnetic and magnetic-field survey work. It is designed to place sensitive sensors away from the towing vehicle so that vehicle motors, power electronics, batteries, and ferromagnetic materials have less influence on the collected data.

The current TowFish system is being integrated with the FAU WAM-V platform for autonomous surface-vessel survey operations. The WAM-V tows the TowFish through predefined survey patterns while the TowFish records onboard sensor data. The system is intended to support magnetic anomaly detection, electric-field sensing, tow stability evaluation, and future AUV-based survey development.

## Primary Mission Roles

- Collect magnetic-field data over known targets such as artificial reefs, shipwrecks, or ferrous underwater objects.
- Collect electric-field and supporting environmental data during tow operations.
- Measure TowFish motion using onboard IMU data so magnetic and electromagnetic measurements can be interpreted in relation to pitch, roll, yaw, acceleration, and tow behavior.
- Evaluate autonomous survey execution using lawnmower patterns from the WAM-V.
- Provide a development platform for future integration with smaller autonomous underwater vehicles.

## Typical Deployment Scenario

1. The TowFish is powered and sealed before deployment.
2. The tow vessel or WAM-V is positioned near the survey area.
3. The TowFish is deployed behind the vehicle using the tow cable.
4. The WAM-V runs manual or autonomous survey lines.
5. TowFish telemetry is logged to a topside computer.
6. Post-processing is performed to review magnetic field, IMU, pressure/depth, and vehicle track data.

## Relationship to WAM-V

The WAM-V acts as the towing and navigation platform. Its main responsibilities are to maintain heading, speed, and waypoint tracking while towing the TowFish. The TowFish acts as the underwater sensing payload. For current testing, WAM-V performance and TowFish performance are evaluated together:

- WAM-V: autonomous navigation, lawnmower pattern execution, tow stability, speed control.
- TowFish: data collection, telemetry stability, depth behavior, magnetic anomaly detection.

## Current Development Focus

The current focus is offshore integration testing. Key goals include:

- Verify that the TowFish records data continuously in an ocean environment.
- Confirm that the Twinleaf/QuSpin magnetometer data stream is stable.
- Confirm that the RP2040 USB-host bridge and Teensy 4.1 telemetry system function reliably.
- Validate tow behavior behind the WAM-V.
- Detect repeatable magnetic anomalies over known wreck or artificial reef targets.
