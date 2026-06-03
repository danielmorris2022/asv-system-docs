# TowFish Software Stack

## Overview

The TowFish software stack is split across three main environments:

1. **TowFish embedded firmware** running on the Teensy 4.1.
2. **USB-host bridge firmware** running on the RP2040.
3. **Topside logging and post-processing tools** running on a laptop or WAM-V computer.

The system is designed to collect raw sensor data from the TowFish, combine it into a single telemetry stream, log that stream during missions, and post-process it after recovery.

## Embedded Controller: Teensy 4.1

The Teensy 4.1 is the main TowFish telemetry controller. It runs the primary firmware that reads onboard sensors and receives magnetometer data from the RP2040 bridge.

Main firmware responsibilities:

- Initialize the BNO055 IMU.
- Initialize the MS5837 pressure sensor.
- Read analog sensor channels.
- Monitor leak detection, battery voltage, temperature, electric-field channels, and Hall sensors.
- Receive Twinleaf magnetometer text frames through `Serial5`.
- Transmit combined telemetry packets at approximately 20 Hz.

Important serial configuration:

```text
Main telemetry output: 115200 baud
Twinleaf input:        Serial5 at 115200 baud
Approximate packet rate: 20 Hz
```

Current telemetry packets are comma-separated and wrapped in angle brackets. The final field appends the latest Twinleaf text line.

## RP2040 USB-Host Bridge

The RP2040 bridge provides a USB-host interface for the Twinleaf Ethernet-to-USB-C adapter. It receives magnetometer output from the USB adapter and forwards it to the Teensy over UART.

RP2040 software requirements:

- Board target must be an RP2040 board, not Teensy 4.1.
- USB stack must be set to TinyUSB.
- CPU speed must be 120 MHz or 240 MHz for PIO USB host mode.
- CPU speed of 200 MHz causes PIO USB failure.
- The watchdog reset behavior should be disabled or set to a long timeout during debugging.

The RP2040 bridge has been observed to print repeated messages such as:

```text
[watchdog] hard reboot
```

This indicates that the watchdog reboot logic is firing. During debugging, disable the actual reboot call and print a warning instead so that the USB enumeration and data path can be inspected.

## Magnetometer Data Format

The Twinleaf adapter has been observed producing text lines similar to:

```text
74 42369.9 29 4
75 42367.5 29 4
76 42358.6 29 4
```

Working interpretation:

```text
sample_counter magnetic_field_nT status_or_counter status_code
```

The second value is the magnetic field magnitude in nanotesla. Local field values around `42360-42375 nT` have been observed during bench testing.

Before valid data appears, the sensor may output:

```text
16 nan 4 1
17 nan 4 1
```

This means the communication link is alive, but the magnetic measurement is invalid or not yet ready.

## Topside Python Logger

A Python logging script may be used on the laptop or WAM-V computer to read the TowFish serial telemetry and save raw CSV files.

Python dependencies:

```bash
pip install pyserial
```

On Windows, serial ports usually appear as:

```text
COM3
COM4
COM5
```

On Linux, serial ports usually appear as:

```text
/dev/ttyUSB0
/dev/ttyACM0
```

The logger should be configured with the correct port for the connected TowFish telemetry interface.

Common Windows issue:

```text
ModuleNotFoundError: No module named 'serial'
```

Fix:

```powershell
python -m pip install pyserial
```

Common Windows port issue:

```text
ERROR opening serial port /dev/ttyUSB0
```

Fix: change the logger port to the correct `COMx` port.

## MATLAB Logging and Post-Processing

MATLAB has been used for both raw serial logging and post-processing. Earlier scripts include:

- `Towfish_CollectData.m`
- `Towfish_PostProcessing.m`
- `IMU_data_analysis.m`
- `Magnet_data_analysis.m`
- `Pressure_data_analysis.m`

Important limitations of the earlier MATLAB scripts:

- Some scripts assume `Test.csv` as the input file.
- Some scripts assume fixed numeric columns.
- Raw packets include angle brackets and mixed text fields.
- The final Twinleaf field may include text and spaces.
- `readmatrix` may convert text fields to `NaN`.
- `readtable` may import numeric fields as strings or cells.

A more robust parser should:

- Remove angle brackets.
- Split comma-separated packet fields carefully.
- Preserve the raw Twinleaf string.
- Convert numeric fields using safe conversion.
- Avoid assuming the magnetometer field is a clean numeric CSV column.
- Save processed CSV output separately from raw logs.

## Data Format

Raw TowFish telemetry packet example structure:

```text
<euler_x,euler_y,euler_z,accel_x,accel_y,accel_z,gyro_x,gyro_y,gyro_z,
mag_x,mag_y,mag_z,quat_w,quat_x,quat_y,quat_z,linacc_x,linacc_y,linacc_z,
gravity_x,gravity_y,gravity_z,battery_v,leak_status,leak_voltage,temp_c,
hall1,hall2,hall3,pressure_mbar,pressure_temp_c,depth_m,twinleaf ...>
```

The exact field order must be kept synchronized between:

- Teensy firmware.
- Python logger.
- MATLAB post-processing.
- Documentation.

A future improvement should add a header row to every mission file.

## Linux / WAM-V Computer Commands

Useful commands when connected to the WAM-V computer over SSH:

```bash
pwd
ls -la
cd foldername
cd ..
find . -name "*towfish*"
lsusb
ls /dev/tty*
dmesg | tail -50
screen /dev/ttyACM0 115200
python3 towfish_log_data.py
ps aux | grep towfish
```

To install serial support:

```bash
pip3 install pyserial
```

To view serial data directly:

```bash
screen /dev/ttyACM0 115200
```

Exit screen with:

```text
Ctrl+A, then K
```

## Coordinate Frames

Current logging includes IMU orientation and acceleration, but coordinate-frame conventions should be formally documented. At minimum, future documentation should define:

- TowFish body-frame axes.
- IMU chip orientation relative to the TowFish body.
- Magnetometer sensor orientation.
- WAM-V GPS/heading reference frame.
- Whether Euler channels are interpreted as roll, pitch, and yaw directly or require axis remapping.

## Lessons Learned

- The Twinleaf adapter can output valid magnetic data directly to a computer.
- `nan` in the magnetometer field does not necessarily mean serial communication is broken; it can mean the measurement is invalid or not ready.
- The RP2040 USB-host bridge is sensitive to CPU-speed and USB-stack settings.
- Watchdog reboot logic can hide the real failure mode during debugging.
- The Teensy mag-only test is useful for isolating whether Serial5 is receiving anything.
- The full TowFish telemetry parser must handle mixed numeric and text data.
