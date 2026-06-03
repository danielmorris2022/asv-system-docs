# TowFish Sensors and Payloads

## Overview

The TowFish carries the sensing payload used for magnetic-field survey work, platform-motion characterization, depth monitoring, and electronics health monitoring. The main scientific payload is the Twinleaf / QuSpin magnetometer system. Supporting sensors provide attitude, pressure/depth, leak status, temperature, battery voltage, electric-field channels, and Hall-effect switch states.

The current sensor suite is intended to support two mission goals:

1. Detect magnetic anomalies from submerged ferrous structures such as artificial reefs, wrecks, or other metallic targets.
2. Measure TowFish motion and health so that magnetic and electromagnetic data can be interpreted correctly.

## Magnetometer

The TowFish uses a Twinleaf / QuSpin-style magnetometer system connected through a Twinleaf microSAM interface. The wiring diagram shows the magnetometer connected through a CAT5 / RJ45-style cable into a Twinleaf Ethernet-to-USB-C adapter. This adapter then connects to the RP2040 USB-host bridge.

Important note: the RJ45 / CAT5 connection is not normal Ethernet networking. It is used as a physical connector and cable assembly for differential sensor signals, power, and sync lines.

### Magnetometer Signal Chain

```text
Twinleaf microSAM Magnetometer
    -> CAT5 / RJ45-style differential cable
    -> Twinleaf Ethernet-to-USB-C adapter
    -> RP2040 USB host bridge
    -> UART Serial1
    -> Teensy Serial5
    -> TowFish telemetry packet
    -> Logger / post-processing
```

### Observed Output Format

During bench testing, the magnetometer adapter produced text output in the following format:

```text
74 42369.9 29 4
75 42367.5 29 4
76 42358.6 29 4
```

The apparent field structure is:

```text
sample_counter magnetic_field_nT status_or_counter status_code
```

The second field is the magnetic field magnitude in nanotesla. Observed values around `42360-42375 nT` are realistic local Earth-field values for the Fort Lauderdale / Port Everglades area and indicate that the magnetometer is producing valid data.

Earlier testing showed output such as:

```text
16 nan 4 1
17 nan 4 1
```

This indicates that communication was active but the measurement field was invalid or not yet ready. If `nan` persists after warmup, check sensor power, adapter connection, and whether the sensor requires initialization.

## IMU / Attitude Sensor

The TowFish uses an Adafruit BNO055 IMU for motion and attitude sensing. The BNO055 provides onboard sensor fusion and outputs:

- Euler orientation.
- Quaternion orientation.
- Acceleration.
- Gyroscope data.
- Magnetometer data.
- Linear acceleration.
- Gravity vector.

The IMU is important because TowFish roll, pitch, yaw, and vibration affect how magnetic and electric-field data should be interpreted. The IMU also helps diagnose towing stability problems such as spinning, excessive roll, or pitch oscillation.

Current firmware reads the BNO055 using I2C and includes all major BNO055 outputs in the telemetry packet.

## Pressure / Depth Sensor

The TowFish uses an MS5837 pressure sensor for pressure, temperature, and estimated depth. The current firmware initializes the MS5837 as a 30-bar model and reads:

- Pressure.
- Pressure-sensor temperature.
- Estimated depth.

The pressure sensor is useful for verifying whether the TowFish is submerged, estimating tow depth, and identifying possible bottom contact or depth oscillations.

The firmware should use a seawater density value for offshore operations. Freshwater density is approximately 997 kg/m^3, while seawater is commonly set near 1029 kg/m^3 for rough depth estimation.

## Electric-Field Channels

The TowFish firmware includes three analog electric-field channels. These are currently logged as raw analog voltage-derived values rather than calibrated physical electric-field units.

Future documentation should add:

- Electrode geometry.
- Amplifier gain.
- Channel units.
- Calibration procedure.
- Expected signal range.

## Leak Detection

A leak-detection voltage is read by the Teensy firmware. Current logic reports:

- `NO_LEAK` if leak voltage is above the configured threshold.
- `LEAK` if leak voltage is below the configured threshold.

The current hard-coded threshold is approximately 3.0 V. This threshold should be verified against the physical leak-detection circuit before operational deployment.

## Temperature Monitoring

The firmware logs a temperature sensor value in addition to pressure-sensor temperature. Temperature monitoring is useful for:

- Electronics health monitoring.
- Pressure-vessel environmental tracking.
- Detecting unusual heating from regulators or electronics.

## Battery Voltage Monitoring

Battery voltage is measured through an analog voltage-divider circuit. The current TowFish battery is a 6S LiPo:

- 22.2 V nominal.
- 25.2 V fully charged.
- 3300 mAh.
- 60C discharge rating.

Telemetry around 25 V is expected when fully charged.

## Hall-Effect Sensors

The TowFish firmware reads three Hall-effect sensor inputs. These are used for magnetic switch logic and may support external control without opening the pressure vessel.

The firmware uses debounce logic for Hall sensor state detection. Current notes indicate one Hall sensor pin assignment should be checked against the PCB because pin conflicts are possible on some boards.

## Calibration Notes

### Magnetometer

For field testing:

- Keep steel tools, magnets, and high-current electronics away from the sensor.
- Record baseline magnetic field away from known targets before surveying.
- Move a ferrous object near the sensor during bench tests to confirm response.
- Verify the sensor produces numeric field values rather than `nan`.
- Compare repeated passes over the same target to evaluate repeatability.

### IMU

For IMU checks:

- Rotate the TowFish by hand in roll, pitch, and yaw.
- Confirm Euler and quaternion values update smoothly.
- Watch for heading discontinuities due to angle wraparound.
- Compare gravity vector and acceleration outputs while stationary.

### Pressure Sensor

For pressure/depth checks:

- Confirm pressure reads near atmospheric pressure on deck.
- Confirm depth is close to zero before deployment.
- For offshore tests, use seawater density in the firmware or post-processing.
- Check for sudden pressure jumps that could indicate sensor problems or bottom contact.

## Field Acceptance Criteria

Before ocean deployment, the sensor suite should meet the following minimum criteria:

- Magnetometer outputs numeric magnetic-field values.
- No repeated RP2040 watchdog resets.
- IMU orientation values are stable when stationary.
- Pressure sensor outputs valid pressure and temperature.
- Battery voltage is realistic for a 6S pack.
- Leak status reports `NO_LEAK`.
- Telemetry packets are received continuously for at least 10-30 minutes on land.
