# WAMV-2 Electronics

## Power System

WAMV-2 uses a DC power system based on onboard battery packs similar to WAMV-1.

- **Batteries**
  - One or more main battery banks supplying propulsion, hotel loads, and additional environmental sensors.
  - Batteries are housed in sealed or splash-resistant compartments.
- **Voltage rails**
  - Common rails include 24 V (for propulsion) and 12 V (for electronics), with DC-DC converters as needed.
  - Additional low-voltage rails (e.g., 5 V) may be used for sensors and weather station components.
- **Protection and distribution**
  - Main breakers and fuses protect battery outputs and distribution branches.
  - Power distribution panels or bus bars route power to thrusters, computers, sensors, weather station, and auxiliary loads.
  - Include the locations of emergency stop switches and any physical disconnects.

## Onboard Computers and Controllers

WAMV-2 contains a primary computing stack, motor controllers, and interfaces for environmental sensors.

- **Primary computer**
  - Ruggedized or protected computer running Ubuntu 22.04 and ROS 2 Humble.
  - Located in an electronics enclosure or rack with adequate cooling.
- **Motor controllers**
  - Dedicated controllers for the main thrusters (update with specific models and interfaces).
  - Interfaced via CAN, serial, or Ethernet depending on hardware.
- **Sensor and weather station interfaces**
  - Interface modules or IO boards for the depth sensor and weather station.
  - May include serial, Ethernet, or other communication links depending on the devices used.

## Wiring and Connectors

WAMV-2 wiring connects batteries, thrusters, computers, environmental sensors, and payloads.

- **High-level wiring layout**
  - Power cables from battery banks to main distribution, then to thrusters, computers, weather station, and auxiliary loads.
  - Signal cables (Ethernet, CAN, serial, USB) connecting computers, controllers, depth sensor, weather station, and other sensors.
- **Connectors and cable management**
  - Marine-grade connectors and cable glands used where cables pass through enclosures.
  - Strain relief and routing practices to avoid chafe and water ingress, particularly for cables to exposed sensors.
- **Known quirks and notes**
  - Document any connectors or cable runs that are particularly sensitive (e.g., cables to the weather station mast or depth sensor).
  - Note any special startup or shutdown considerations related to sensor power.
