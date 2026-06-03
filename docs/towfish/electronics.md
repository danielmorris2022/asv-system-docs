# TowFish Electronics

## Overview

The TowFish electronics system provides power distribution, sensor acquisition, telemetry aggregation, and health monitoring for the TowFish payload. The system is centered around a Teensy 4.1 microcontroller on the main TowFish motherboard and an RP2040 USB-host bridge used to interface with the external Twinleaf / QuSpin magnetometer adapter.

The electronics support:

- Magnetometer data acquisition.
- IMU attitude and motion sensing.
- Pressure/depth sensing.
- Electric-field channel measurement.
- Leak detection.
- Temperature monitoring.
- Battery-voltage monitoring.
- Hall-effect switch inputs.
- Serial telemetry to the topside logger.

## Main Controller

The primary onboard controller is a **Teensy 4.1**. It runs the main TowFish telemetry firmware and is responsible for reading local sensors, receiving external magnetometer data, and transmitting one combined telemetry packet to the logging computer.

Main Teensy responsibilities:

- Initialize sensors during startup.
- Read BNO055 IMU data.
- Read MS5837 pressure/depth data.
- Read analog health-monitoring channels.
- Read Hall-effect switch states.
- Receive magnetometer text frames from the RP2040 bridge on Serial5.
- Format and transmit combined telemetry packets.

Important serial settings:

- Main telemetry output: 115200 baud.
- Twinleaf / magnetometer input: 115200 baud.
- Packet output rate: approximately 20 Hz in the current firmware.

## RP2040 USB Host Bridge

A separate RP2040 board is used as a USB-host serial bridge for the magnetometer adapter. The bridge allows the TowFish electronics to receive data from a USB-C Twinleaf adapter and forward it as UART serial data to the Teensy.

Data path:

```text
Twinleaf microSAM Magnetometer
    -> CAT5 / RJ45-style differential cable
    -> Twinleaf Ethernet-to-USB-C adapter
    -> RP2040 USB host bridge
    -> UART Serial1
    -> Teensy Serial5
    -> TowFish telemetry packet
```

The RP2040 bridge uses Adafruit TinyUSB and PIO USB host functionality. The USB-host helper configuration requires the RP2040 CPU clock to be set to either **120 MHz** or **240 MHz**. A 200 MHz CPU clock causes PIO USB host failure.

Important RP2040 notes:

- Select TinyUSB as the USB stack.
- Select a valid RP2040 board target, not Teensy 4.1.
- Set CPU speed to 120 MHz or 240 MHz.
- The watchdog reboot logic should be disabled or set to a long timeout during debugging.
- The bridge must provide 5 V USB-host power to the Twinleaf USB adapter.

## Power System

The current TowFish uses a 6S lithium-polymer battery pack.

Current battery configuration:

- Battery type: LiPo.
- Cell count: 6S.
- Nominal voltage: 22.2 V.
- Fully charged voltage: 25.2 V.
- Capacity: 3300 mAh.
- Discharge rating: 60C.

Telemetry values around 25.0 V to 25.2 V indicate a fully charged battery. Values near 22.8 V are approximately storage voltage. Values below roughly 20 V should be treated as low-voltage conditions unless a more precise operating limit is defined.

Earlier project references to 2S / 7.4 V battery packs apply to earlier RASP-style prototype systems and should not be used to interpret the current TowFish voltage telemetry.

## Voltage Rails

The electronics include regulated low-voltage rails for microcontrollers and sensors. The system includes:

- Main battery input.
- Regulated 5 V supply for USB-host and sensor electronics.
- Regulated 3.3 V supply for logic and sensors.
- Analog measurement channels for battery and sensor health monitoring.

Before deployment, verify that all regulators and downstream electronics are rated for the full 6S LiPo voltage range.

## Battery Voltage Measurement

The firmware calculates battery voltage from an analog sense line using a voltage-divider scale factor. The scale factor must match the physical resistor divider and ADC reference configuration.

If any of the following change, battery-voltage calibration must be redone:

- Battery voltage range.
- Voltage divider resistors.
- ADC reference voltage.
- Teensy analog resolution.
- Regulator or PCB changes.

## Wiring and Connectors

The TowFish uses pressure-rated connectors and internal harnesses to connect sensors, pressure vessels, and electronics boards. The wiring diagram identifies a Twinleaf magnetometer path using an RJ45-style connector with differential signal pairs.

The RJ45/CAT5 wiring should not be assumed to be Ethernet networking. It is used as a physical connector and cable assembly for Twinleaf differential signals and power.

Relevant magnetometer-side signals include:

- TX+ / TX-
- RX+ / RX-
- 5 V
- GND
- SYNC-related lines

## Telemetry Packet

The main TowFish packet is a comma-separated line wrapped in angle brackets. It combines:

- Euler angles.
- Acceleration.
- Gyroscope.
- BNO055 magnetometer channels.
- Quaternion orientation.
- Linear acceleration.
- Gravity vector.
- Battery voltage.
- Leak status and leak voltage.
- Temperature.
- Hall sensor states.
- Pressure.
- Pressure-sensor temperature.
- Depth.
- Latest Twinleaf magnetometer text line.

Because the Twinleaf text frame is appended as raw text, commas or unexpected characters in the magnetometer string can affect CSV parsing. Logging and post-processing code should treat the final Twinleaf field carefully.

## Health Monitoring

The TowFish electronics include several health-monitoring channels:

- Battery voltage.
- Internal temperature.
- Leak detector voltage and leak status.
- Hall-effect switch states.
- Pressure/depth sensor.

Leak status is currently determined from a voltage threshold in firmware. The exact leak threshold should be verified against the physical leak-detection circuit before operational use.

## Field Electronics Checklist

Before each deployment:

- Confirm battery is charged and not damaged.
- Confirm pressure vessel is sealed.
- Confirm leak sensor reports NO_LEAK.
- Confirm battery telemetry is realistic for a 6S LiPo.
- Confirm pressure sensor reads near atmospheric pressure on deck.
- Confirm magnetometer outputs numeric field values, not `nan`.
- Confirm RP2040 is not repeatedly watchdog rebooting.
- Confirm telemetry packets are being received by the logger.
