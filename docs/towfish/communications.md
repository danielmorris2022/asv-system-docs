# TowFish Communications

## Overview

The TowFish communication architecture is primarily serial and USB-based. Although the Twinleaf magnetometer uses RJ45/CAT5-style cabling in part of the signal path, this connection should not be treated as normal Ethernet/IP networking.

The communication system has three major paths:

1. Magnetometer communication from the Twinleaf sensor to the Teensy through the RP2040 bridge.
2. Main TowFish telemetry from the Teensy to a topside logger.
3. Operator access to the WAM-V computer over SSH for logging and field checks.

## Magnetometer Communication Path

The Twinleaf / QuSpin magnetometer system communicates through a Twinleaf adapter chain before reaching the TowFish main controller.

```text
Twinleaf microSAM Magnetometer
    -> CAT5 / RJ45-style differential cable
    -> Twinleaf Ethernet-to-USB-C adapter
    -> RP2040 USB host bridge
    -> UART Serial1
    -> Teensy Serial5
    -> Combined TowFish telemetry packet
```

Important notes:

- The RJ45/CAT5 section is a physical wiring interface, not standard Ethernet networking.
- The RP2040 must run USB-host bridge firmware.
- The RP2040 forwards USB CDC data to UART.
- The Teensy receives the magnetometer stream on Serial5.
- The main TowFish firmware appends the latest Twinleaf line to the telemetry packet.

## Main TowFish Telemetry

The Teensy 4.1 outputs a combined telemetry packet at approximately 20 Hz. The telemetry stream includes:

- IMU data.
- Pressure/depth data.
- Battery voltage.
- Leak status.
- Temperature.
- Hall sensor states.
- Electric-field channel values.
- Latest Twinleaf magnetometer line.

Typical serial setting:

```text
Baud rate: 115200
```

The logger should be configured for the correct serial device.

Windows examples:

```text
COM3
COM4
COM5
```

Linux examples:

```text
/dev/ttyUSB0
/dev/ttyACM0
```

## WAM-V Computer Access

During WAM-V testing, operators may SSH into the WAM-V computer from a laptop.

Example:

```bash
ssh username@IP_ADDRESS
```

Useful commands after logging in:

```bash
pwd
ls -la
cd foldername
find . -name "*towfish*"
lsusb
ls /dev/tty*
dmesg | tail -50
```

To monitor serial data directly:

```bash
screen /dev/ttyACM0 115200
```

Exit screen with:

```text
Ctrl+A, then K
```

## Checking Whether the TowFish Is Connected

On Linux:

```bash
lsusb
ls /dev/tty*
dmesg | tail -50
```

Recommended procedure:

1. Run `ls /dev/tty*` before connecting the TowFish.
2. Connect the TowFish telemetry cable.
3. Run `dmesg | tail -50`.
4. Identify the new serial device.
5. Use that device path in the Python logger.

## Common Communication Problems

### Wrong Port Name

A common issue is using a Linux port name on Windows or a Windows COM port on Linux.

Incorrect on Windows:

```text
/dev/ttyUSB0
```

Typical Windows format:

```text
COM4
```

Typical Linux format:

```text
/dev/ttyACM0
```

### Serial Port Already Open

Only one program can usually hold a serial port at a time. Close Arduino Serial Monitor, MATLAB, PuTTY, screen, or Python logger before opening the port from another program.

### RP2040 Watchdog Reboot

If the telemetry contains:

```text
[watchdog] hard reboot
```

then the RP2040 bridge is resetting. During debugging, disable the watchdog reboot call or increase the timeout so the USB-host behavior can be inspected.

### TinyUSB / RP2040 Configuration

For the RP2040 bridge:

- USB stack must be TinyUSB.
- CPU speed must be 120 MHz or 240 MHz.
- Board target must be RP2040, not Teensy.

## Bandwidth Considerations

The TowFish telemetry packet is relatively large because it includes IMU vectors, quaternion data, pressure, health channels, and a raw magnetometer text line. At approximately 20 Hz and 115200 baud, packet size and logging reliability should be monitored.

Possible future improvements:

- Add a compact binary format.
- Add a CSV header row.
- Separate raw magnetometer logging from health telemetry.
- Increase telemetry baud rate if the physical link supports it.
- Use differential serial such as RS-485 for long cable runs.
