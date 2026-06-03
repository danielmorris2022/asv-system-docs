# TowFish Operations

## Overview

This page describes recommended operating procedures for preparing, deploying, towing, logging, and recovering the TowFish system during WAM-V and vessel-based field tests. These procedures should be updated after each field operation as lessons are learned.

The current operational focus is the integrated WAM-V + TowFish test program. In this configuration, the WAM-V tows the TowFish through survey lines while the TowFish records magnetometer, IMU, pressure/depth, battery, leak, and health data.

## Mission Planning

Before an offshore test, define the following:

- Test date and time window.
- Launch and recovery vessel.
- Survey site and coordinates.
- Target type, if any.
- Expected depth and seafloor conditions.
- Tow speed.
- Tow cable length.
- Survey pattern.
- Data logging location.
- Abort criteria.

For known-target magnetic testing, the target should be large, ferrous, and accurately located. The Duzaway Houseboat artificial reef has been selected as a near-term test target because it is close to Port Everglades and provides a known submerged metallic structure.

Example target:

```text
Duzaway Houseboat Artificial Reef
Coordinates: 26 deg 06.677' N, 80 deg 03.716' W
Approximate decimal coordinates: 26.111283, -80.061933
```

## Pre-Deployment Checklist

### TowFish Physical Inspection

- Inspect body and pressure-vessel sections.
- Inspect fins and external hardware.
- Inspect tow cable for cuts, kinks, corrosion, crushed sections, or connector damage.
- Confirm strain relief is secure.
- Confirm all connectors are fully seated.
- Confirm pressure-vessel seals are closed.
- Confirm no loose wires or unsecured payload components are present.

### Battery and Power

- Confirm 6S LiPo is charged.
- Confirm battery is not swollen, damaged, or warm before use.
- Confirm battery connector is secure.
- Confirm expected battery telemetry is approximately 25.2 V when fully charged.
- Confirm low-voltage threshold for the mission.

### Sensor Checkout

Before deployment, confirm that the logger shows valid data from:

- Twinleaf / QuSpin magnetometer.
- BNO055 IMU.
- MS5837 pressure sensor.
- Battery-voltage channel.
- Leak-detection channel.
- Temperature channel.
- Hall sensors, if applicable.

The magnetometer should show numeric field values, not persistent `nan` values. Example valid magnetometer output observed during testing:

```text
74 42369.9 29 4
75 42367.5 29 4
76 42358.6 29 4
```

### Electronics Checkout

- Confirm Teensy 4.1 main firmware is running.
- Confirm RP2040 USB-host bridge is running.
- Confirm RP2040 is not repeatedly printing `[watchdog] hard reboot`.
- Confirm telemetry reaches the topside computer.
- Confirm a short test log file can be created and opened.
- Confirm no other program is holding the serial port open.

## Dockside / Land Test

Before any water deployment, perform a short land or dockside test.

Minimum recommended test:

1. Power the TowFish.
2. Start the logger.
3. Record at least 5-10 minutes of data.
4. Move the TowFish by hand in roll, pitch, and yaw.
5. Verify IMU response.
6. Bring a ferrous object near the magnetometer and verify magnetic response.
7. Confirm pressure and leak sensor values are reasonable.
8. Stop the logger and confirm the file was saved.

Acceptance criteria:

- Continuous telemetry with no major packet dropouts.
- Magnetometer produces numeric readings.
- IMU values change when the TowFish is rotated.
- Leak sensor reports `NO_LEAK`.
- Battery voltage is realistic.
- No repeated RP2040 watchdog resets.

## Deployment and Recovery

### Deployment

- Deploy only after live telemetry has been verified.
- Maintain control of the tow cable during deployment.
- Keep the cable clear of propellers, thrusters, rudders, and sharp edges.
- Lower the TowFish gently into the water.
- Do not drop or shock-load the tow cable.
- Begin towing slowly before increasing speed.

Recommended initial speed:

```text
1.0 knot, then increase gradually if stable
```

Recommended first-test tow length:

```text
15-30 m depending on sea state, WAM-V handling, and recovery constraints
```

### Recovery

- Slow or stop the tow vehicle before recovery.
- Keep the cable clear of propulsion systems.
- Recover the TowFish by the tow line or approved lifting points, not by sensor wiring.
- Inspect for water ingress, damaged fins, cable damage, or loose connectors.
- Stop logging only after the TowFish has been recovered or after the test objective is complete.

## Mission Operations

### WAM-V Integration

During WAM-V operations, evaluate both vehicle and TowFish performance:

WAM-V checks:

- Autonomous waypoint tracking.
- Lawnmower pattern execution.
- Speed control.
- Turning behavior while towing.
- Communication with operator station.

TowFish checks:

- Telemetry continuity.
- Magnetometer baseline stability.
- Depth/pressure behavior.
- IMU motion stability.
- Tow cable behavior.
- Leak and battery status.

### Survey Pattern

For magnetic anomaly testing, use simple repeated patterns first:

1. Straight pass through known target coordinates.
2. Reverse-direction pass over the same line.
3. Small lawnmower grid centered on the target.
4. Repeat one or two central passes after the grid.

For first ocean tests, use small survey boxes before expanding:

```text
Initial box: 100 m x 100 m
Expanded box: 200 m x 200 m or larger if stable
Line spacing: 10-20 m depending on target size and navigation accuracy
Speed: 1.0-1.5 knots
```

### Duzaway Houseboat Test Concept

The Duzaway Houseboat site can be used to evaluate whether the TowFish detects a repeatable magnetic anomaly over a known artificial reef.

Example mission objective:

- Tow the TowFish over and around the wreck site.
- Record magnetometer, IMU, pressure/depth, and WAM-V GPS/heading data.
- Compare magnetic peaks against the known target location.
- Confirm that repeated passes produce repeatable anomaly signatures.

The most important success metric is not simply a large magnetic spike; it is a repeatable magnetic feature that appears at the expected location across multiple passes.

## Starting the Logger

### Linux / WAM-V Computer

Check connected devices:

```bash
lsusb
ls /dev/tty*
dmesg | tail -50
```

Run the Python logger:

```bash
python3 towfish_log_data.py
```

If `pyserial` is missing:

```bash
pip3 install pyserial
```

To view raw serial data:

```bash
screen /dev/ttyACM0 115200
```

Exit screen:

```text
Ctrl+A, then K
```

### Windows Laptop

Use the correct COM port, such as:

```text
COM3
COM4
COM5
```

Do not use Linux paths such as `/dev/ttyUSB0` on Windows.

## In-Mission Monitoring

During the mission, monitor:

- Magnetometer values.
- Leak status.
- Battery voltage.
- Pressure/depth.
- Packet dropouts.
- RP2040 watchdog messages.
- WAM-V position and speed.
- Tow cable angle and tension.

Abort or pause the survey if:

- Leak status changes to `LEAK`.
- Battery voltage drops below safe mission threshold.
- Telemetry stops.
- RP2040 enters repeated watchdog reboot cycle.
- TowFish motion becomes unstable.
- Tow cable risks contacting propulsion.
- WAM-V cannot maintain safe control.

## Shutdown

Recommended shutdown sequence:

1. Complete final survey line or abort safely.
2. Slow or stop WAM-V.
3. Recover TowFish.
4. Stop data logger.
5. Confirm data file exists and is non-empty.
6. Power down TowFish electronics.
7. Inspect TowFish body and connectors.
8. Back up mission data.
9. Record field notes immediately.

## Post-Mission Notes

After each mission, record:

- Date and time.
- Site and coordinates.
- Crew.
- Weather and sea state.
- Tow speed.
- Tow cable length.
- Battery voltage start/end.
- Sensor issues.
- WAM-V autonomy issues.
- Whether the magnetometer produced numeric data.
- Whether magnetic anomalies were observed.
- Any hardware damage or water ingress.

Recommended post-processing plots:

- Magnetic field vs. time.
- Magnetic field vs. GPS position.
- TowFish pressure/depth vs. time.
- IMU roll/pitch/yaw vs. time.
- WAM-V track over survey box.
- Magnetic anomaly map or contour plot.
