# WAMV-2 Software Stack

## Overview

WAMV-2 runs a ROS 2 Humble-based software stack on Ubuntu 22.04. The stack includes low-level control, higher-level navigation, sensor drivers, and additional nodes for environmental sensing (depth and weather), and is designed to integrate with the OSRF VRX simulation environment via the `wamv_nav` repository.

## Main Repositories

- [`xvrobotics/wamv_nav`](https://github.com/xvrobotics/wamv_nav) – provides navigation, control, and mission logic for WAMV platforms.
- [`Marine-Robotics-Club/ros2_humble_install`](https://github.com/Marine-Robotics-Club/ros2_humble_install) – used to install ROS 2 Humble and dependencies on the WAMV-2 onboard computer.
- Additional repositories used by WAMV-2 (e.g., drivers for depth sensor, weather station, perception packages) should be listed here as they are standardized.

## ROS 2 Nodes (High-Level)

List the main categories of nodes that run on WAMV-2, and fill in specifics as the stack is finalized.

- **Control nodes**
  - Low-level thruster control nodes that convert velocity/command inputs into motor controller commands.
- **Navigation and planning nodes**
  - Nodes that estimate vessel state (pose, velocity) from GNSS/INS and IMU data.
  - Path-following or waypoint navigation nodes that compute desired motions.
- **Sensor driver nodes**
  - Nodes that interface with GNSS/INS, IMUs, cameras, and any LiDAR units installed on WAMV-2.
- **Environmental sensor nodes**
  - Nodes that read depth sensor data and publish depth/bathymetry information.
  - Nodes that read weather station data and publish wind, temperature, humidity, and pressure.
- **Utility and support nodes**
  - Diagnostic, logging, and health-monitoring nodes.

For each node, document:

- Package and node name.
- Subscribed and published topics (including message types).
- Services or actions provided, if any.

## Launch Files

Document the main launch files used to bring up WAMV-2 in different scenarios.

- **Dock test launch**
  - Starts a minimal set of nodes for checking thruster response, basic sensor connectivity, and network health.
- **Full mission launch**
  - Starts control, navigation, sensor drivers, and any mission logic required for autonomous runs.
- **Environmental survey launch**
  - Starts additional nodes for depth and weather data collection during survey missions.
- **Simulation launch (VRX)**
  - Launch configuration that connects WAMV-2’s stack to the VRX simulation for testing without hardware.

For each launch file, record:

- Path in the repository (e.g., `wamv_nav/launch/...`).
- Nodes started and any important parameters or arguments.

## Coordinate Frames

Describe the key coordinate frames used by WAMV-2.

- **Base frame** (e.g., `base_link`) located at a defined reference point on the vessel.
- **Sensor frames** attached to GNSS antennas, IMUs, cameras, depth sensor, weather station, and other sensors.
- **World/map frames** used for global navigation.

Link to any frame diagrams or URDF/Xacro models once available, and note where transforms (static or dynamic) are defined in configuration or launch files.
