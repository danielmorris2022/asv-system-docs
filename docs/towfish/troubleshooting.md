# TowFish Troubleshooting

## Overview

This page documents recurring TowFish issues discovered during bench testing, WAM-V integration, RP2040 debugging, Twinleaf magnetometer testing, Python logging, and MATLAB post-processing. It is intended as a field troubleshooting guide.

## Quick Diagnostic Flow

If the TowFish is not logging correctly, isolate the problem in this order:

```text
1. Is the TowFish powered?
2. Is the Teensy outputting telemetry?
3. Is the logger connected to the correct serial port?
4. Is the RP2040 bridge running?
5. Is the Twinleaf adapter outputting valid magnetometer data?
6. Is the Teensy receiving magnetometer data on Serial5?
7. Is the post-processing script parsing the file correctly?
```

Do not debug all layers at once. Test one link at a time.

## Issue: Python says `No module named serial`

### Symptom

```text
ModuleNotFoundError: No module named 'serial'
```

### Cause

The Python `pyserial` package is not installed for the Python interpreter being used.

### Fix

Windows:

```powershell
python -m pip install pyserial
```

If using a specific Python version:

```powershell
C:\Users\<user>\AppData\Local\Programs\Python\Python313\python.exe -m pip install pyserial
```

Linux:

```bash
pip3 install pyserial
```

## Issue: Logger tries to open `/dev/ttyUSB0` on Windows

### Symptom

```text
ERROR opening serial port /dev/ttyUSB0
```

### Cause

The script is configured for a Linux serial device path while running on Windows.

### Fix

Change the port in the logger to a Windows COM port:

```text
COM3
COM4
COM5
```

Use Device Manager to identify the correct port.

## Issue: Serial port is busy

### Symptom

The logger cannot open the port, or data appears in one program but not another.

### Cause

Arduino Serial Monitor, MATLAB, PuTTY, screen, or another Python logger is already using the port.

### Fix

Close all other serial programs before starting the logger.

## Issue: RP2040 bridge compiled for Teensy 4.1

### Symptom

Compilation errors such as:

```text
hardware/watchdog.h: No such file or directory
TinyUSB Arduino Library does not support your core yet
Error compiling for board Teensy 4.1
```

### Cause

The RP2040 USB-host bridge code was compiled for the wrong board.

### Fix

Select an RP2040 board target, such as Adafruit Feather RP2040 or an equivalent RP2040 board using the Earle Philhower RP2040 core.

## Issue: TinyUSB not selected

### Symptom

```text
#error TinyUSB is not selected, please select it in Tools->Menu->USB Stack
```

### Cause

The RP2040 bridge requires TinyUSB, but the Arduino USB stack is selected.

### Fix

In Arduino IDE:

```text
Tools -> USB Stack -> Adafruit TinyUSB
```

Then compile and upload again.

## Issue: RP2040 CPU clock incompatible with PIO USB

### Symptom

```text
Error: CPU Clock = 200000000, PIO USB require CPU clock must be multiple of 120 Mhz
Change your CPU Clock to either 120 or 240 Mhz
```

### Cause

PIO USB host mode requires the RP2040 CPU clock to be 120 MHz or 240 MHz.

### Fix

In Arduino IDE:

```text
Tools -> CPU Speed -> 120 MHz
```

or:

```text
Tools -> CPU Speed -> 240 MHz
```

Use 120 MHz first for stability.

## Issue: RP2040 upload fails with `No drive to deploy`

### Symptom

```text
Scanning for RP2040 devices
No drive to deploy.
Failed uploading
```

### Cause

The RP2040 is not in bootloader / UF2 mode.

### Fix

1. Unplug RP2040 USB cable.
2. Hold BOOTSEL.
3. Plug USB cable into the laptop while holding BOOTSEL.
4. Release BOOTSEL.
5. Confirm a drive named `RPI-RP2` appears.
6. Upload again.

If no drive appears:

- Try another USB cable.
- Try another USB port.
- Avoid hubs.
- Confirm the cable is connected to the RP2040, not the Teensy or TowFish PCB USB port.

## Issue: Repeated `[watchdog] hard reboot`

### Symptom

TowFish telemetry or RP2040 serial monitor repeatedly shows:

```text
[watchdog] hard reboot
```

### Likely Causes

- RP2040 watchdog timeout is too aggressive.
- Twinleaf adapter is not sending data frequently enough.
- USB host enumeration has failed.
- CPU speed or TinyUSB configuration is wrong.
- Bridge code is rebooting before useful debug output can be inspected.

### Fix

During debugging, disable the actual watchdog reboot call:

```cpp
// watchdog_reboot(0, 0, 0);
```

Replace with a print statement:

```cpp
Serial.println("[watchdog disabled] no USB activity");
```

Also increase the timeout:

```cpp
#define USB_SILENCE_TIMEOUT_MS 30000UL
```

Only re-enable watchdog behavior after the bridge has been validated.

## Issue: Magnetometer outputs `nan`

### Symptom

Twinleaf output looks like:

```text
16 nan 4 1
17 nan 4 1
18 nan 4 1
```

### Meaning

The communication path is alive, but the magnetic measurement field is invalid or not ready.

### Possible Causes

- Sensor has not warmed up.
- Magnetometer head is not powered correctly.
- Adapter is connected but sensor is not measuring.
- Sensor requires initialization.
- Cable or RJ45/CAT5 connection issue.

### Fix

- Leave sensor powered for several minutes.
- Check sensor and adapter power.
- Check RJ45/CAT5 wiring and connector seating.
- Plug the Twinleaf USB adapter directly into a laptop and verify output.
- Look for valid output such as:

```text
74 42369.9 29 4
75 42367.5 29 4
```

## Issue: Teensy magnetometer-only test shows no lines

### Symptom

The Teensy mag-only sketch repeatedly reports no magnetometer lines.

### Likely Causes

- RP2040 is not outputting UART data.
- RP2040 is rebooting.
- RP2040 TX is not connected to Teensy Serial5 RX.
- PCB routing does not match assumed Serial5 pins.
- No common ground between RP2040 and Teensy.
- Wrong baud rate.

### Fix

1. Confirm the Twinleaf adapter outputs data directly to a laptop.
2. Confirm the RP2040 bridge receives or forwards data.
3. Temporarily make RP2040 output a test line:

```cpp
Serial1.println("RP2040 UART TEST");
```

4. Run the Teensy magnetometer-only test.
5. Confirm the Teensy sees:

```text
[MAG] RP2040 UART TEST
```

If not, debug wiring and PCB routing.

## Issue: MATLAB `Invalid data argument` when plotting

### Symptom

```text
Error using plot
Invalid data argument.
```

### Cause

The script is trying to plot non-numeric data. Raw TowFish files include angle brackets and text fields such as `NO_LEAK` and `twinleaf`, which can cause MATLAB to import numeric columns as strings, cells, or table data.

### Fix

Use robust parsing:

- Strip `<` and `>`.
- Convert fields using `str2double`.
- Preserve text fields separately.
- Verify classes before plotting.

Example checks:

```matlab
class(x_accel)
size(x_accel)
```

The plotted vector should be numeric `double`.

## Issue: Battery voltage looks too high

### Symptom

Battery telemetry around 25 V appears high if expecting a 2S LiPo.

### Cause

The current TowFish uses a 6S LiPo, not the older 2S RASP battery configuration.

### Correct Interpretation

- Fully charged 6S LiPo: approximately 25.2 V.
- Nominal 6S LiPo: approximately 22.2 V.
- Values around 25 V are expected when fully charged.

## Issue: Pressure/depth offset on land

### Symptom

Depth reads slightly positive or negative on land.

### Cause

Atmospheric pressure offset or depth zeroing issue.

### Fix

- Zero depth at startup or in post-processing.
- Use the first stable pressure reading as atmospheric reference.
- Use seawater density for offshore tests.

## Issue: No TowFish device visible on Linux

### Diagnostic Commands

```bash
lsusb
ls /dev/tty*
dmesg | tail -50
```

### Fixes

- Try another USB port.
- Try another cable.
- Confirm power to TowFish electronics.
- Confirm the correct USB path is connected.
- Check whether the device appears as `/dev/ttyACM0` or `/dev/ttyUSB0`.

## Field Rule

If debugging offshore, simplify the system:

1. Confirm raw serial data.
2. Confirm logger saves data.
3. Confirm magnetometer values are numeric.
4. Confirm IMU and pressure data are present.
5. Only then run the autonomous survey pattern.
