# TowFish Configuration

## Overview

This page documents important TowFish configuration settings, parameters, and software assumptions. It should be kept synchronized with the Teensy firmware, RP2040 bridge firmware, Python logger, MATLAB post-processing scripts, and WAM-V mission setup.

## Embedded Firmware Configuration

### Main TowFish Controller

Board:

```text
Teensy 4.1
```

Main responsibilities:

- Read IMU, pressure, analog, leak, temperature, Hall, and electric-field channels.
- Receive Twinleaf magnetometer data from RP2040 bridge.
- Output combined telemetry packet.

Typical serial settings:

```text
Main telemetry baud rate: 115200
Twinleaf input baud rate: 115200
Twinleaf input port: Teensy Serial5
Approximate packet rate: 20 Hz
Telemetry interval: 50 ms
```

### Twinleaf / Magnetometer Input

The main firmware polls the Twinleaf stream non-blocking and stores the latest complete line. The magnetometer text frame is appended to the final telemetry packet.

Expected observed Twinleaf format:

```text
74 42369.9 29 4
```

Working interpretation:

```text
sample_counter magnetic_field_nT status_or_counter status_code
```

## RP2040 USB-Host Bridge Configuration

Board:

```text
Adafruit Feather RP2040 or compatible RP2040 board
```

Required Arduino settings:

```text
Board: RP2040 board target, not Teensy 4.1
USB Stack: Adafruit TinyUSB
CPU Speed: 120 MHz or 240 MHz
```

Recommended CPU speed for debugging:

```text
120 MHz
```

PIO USB host will fail at 200 MHz with the error:

```text
PIO USB require CPU clock must be multiple of 120 MHz
```

### Watchdog Timeout

The RP2040 bridge includes watchdog reboot behavior if USB data is not received within a configured timeout. During debugging, the actual reboot call should be disabled or the timeout should be increased.

Recommended debug setting:

```cpp
#define USB_SILENCE_TIMEOUT_MS 30000UL
```

Recommended debug behavior:

```cpp
// watchdog_reboot(0, 0, 0);
Serial.println("[watchdog disabled] no USB activity");
```

Only re-enable automatic watchdog reboot after the bridge has been validated.

## Battery Configuration

Current battery:

```text
Battery type: LiPo
Cell count: 6S
Nominal voltage: 22.2 V
Fully charged voltage: 25.2 V
Capacity: 3300 mAh
Discharge rating: 60C
```

Telemetry interpretation:

```text
25.0-25.2 V: fully charged
~22.8 V: storage voltage region
~22.2 V: nominal voltage
<20 V: treat as low-voltage condition unless mission-specific limits are defined
```

Earlier 2S / 7.4 V references apply to older prototype systems and should not be used for the current TowFish configuration.

## Pressure Sensor Configuration

Sensor:

```text
Blue Robotics Bar30 / MS5837-style pressure sensor
```

Recommended fluid density for offshore seawater operations:

```text
1029 kg/m^3
```

A land-test depth offset near zero is expected. Depth should be zeroed at startup or corrected in post-processing using an atmospheric baseline.

## Leak Detection Configuration

Leak status is determined from a leak-sensor voltage threshold.

Current approximate threshold:

```text
NO_LEAK if leak voltage >= 3.0 V
LEAK if leak voltage < 3.0 V
```

This threshold should be verified against the physical circuit before operational deployment.

## Python Logger Configuration

Python dependency:

```bash
pip install pyserial
```

Windows serial port examples:

```text
COM3
COM4
COM5
```

Linux serial port examples:

```text
/dev/ttyUSB0
/dev/ttyACM0
```

Common WAM-V computer check:

```bash
lsusb
ls /dev/tty*
dmesg | tail -50
```

Common logger parameters:

```text
baud rate: 115200
log mode: continuous until stopped
output folder: data/
```

Recommended file naming:

```text
Mission_YYYY-MM-DD_HH-MM-SS_raw.csv
Mission_YYYY-MM-DD_HH-MM-SS_processed.csv
Mission_YYYY-MM-DD_HH-MM-SS_summary.txt
```

## MATLAB Post-Processing Configuration

Important assumptions to avoid:

- Do not assume the raw log is numeric-only.
- Do not assume `readmatrix` preserves text fields correctly.
- Do not assume the Twinleaf field is a simple numeric CSV column.

Recommended parser behavior:

- Strip angle brackets.
- Split fields safely.
- Preserve `NO_LEAK` as text.
- Preserve `twinleaf` raw field as text.
- Convert known numeric columns using `str2double`.
- Export a processed CSV with headers.

## Survey Configuration Recommendations

### First WAM-V Tow Tests

Recommended initial values:

```text
Tow speed: 1.0 knot initially, increase to 1.5 knots if stable
Tow cable length: 15-30 m for first tests
Initial survey box: 100 m x 100 m
Expanded survey box: 200 m x 200 m or larger if stable
Line spacing: 10-20 m
```

### Duzaway Houseboat Mission

Target:

```text
Duzaway Houseboat Artificial Reef
Coordinates: 26 deg 06.677' N, 80 deg 03.716' W
Approximate decimal: 26.111283, -80.061933
Approximate depth: 95 ft / 29 m
```

Recommended pattern:

- One direct pass over target.
- Reverse-direction repeat pass.
- Small lawnmower grid centered on target.
- Repeat central pass after the grid.

## Configuration Items To Verify Before Deployment

- Teensy firmware version.
- RP2040 bridge firmware version.
- RP2040 CPU speed.
- RP2040 USB stack selection.
- Watchdog timeout setting.
- Logger serial port.
- Logger baud rate.
- Battery voltage scale factor.
- Pressure fluid density.
- Leak threshold.
- Telemetry packet field order.
- Post-processing column map.
