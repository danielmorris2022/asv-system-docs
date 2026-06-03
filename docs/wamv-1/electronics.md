# WAMV-1 Electronics

## Power System

WAMV-1 uses a DC power system based on onboard battery packs.

- **Batteries**
  - One or more main battery banks supplying propulsion and hotel loads (update with chemistry, capacity, and nominal voltage).
  - Batteries are typically housed in sealed or splash-resistant compartments.
- **Voltage rails**
  - Common rails include 24 V (for propulsion) and 12 V (for electronics), with DC-DC converters as needed.
  - Document any additional rails (e.g., 5 V) used for sensors and low-power devices.
- **Protection and distribution**
  - Main breakers and fuses protect battery outputs and distribution branches.
  - Power distribution panels or bus bars route power to thrusters, computers, sensors, and auxiliary loads.
  - Include the locations of emergency stop switches and any physical disconnects.

## Onboard Computers and Controllers

WAMV-1 contains a primary computing stack and associated motor controllers.

- **Primary computer**
  - Ruggedized or protected computer running Ubuntu 22.04 and ROS 2 Humble.
  - Located in an electronics enclosure or rack with adequate cooling.
- **Motor controllers**
  - Dedicated controllers for the main thrusters and bow thrusters (update with specific models and interfaces).
  - Interfaced via CAN, serial, or Ethernet depending on hardware.
- **Other control electronics**
  - Any autopilot boards, microcontrollers, or IO modules used for low-level control, sensor integration, or safety functions.

## Wiring and Connectors

WAMV-1 wiring connects batteries, thrusters, computers, and sensors.

- **High-level wiring layout**
  - Power cables from battery banks to main distribution, then to thrusters, computers, and auxiliary loads.
  - Signal cables (Ethernet, CAN, serial, USB) connecting computers, controllers, and sensors.
- **Connectors and cable management**
  - Use of marine-grade connectors and cable glands where cables pass through enclosures.
  - Strain relief and routing practices to avoid chafe and water ingress.
- **Known quirks and notes**
  - Document any connectors that are prone to loosening, areas susceptible to corrosion, or other recurring issues.
  - Note any special startup or shutdown considerations related to the power system.
