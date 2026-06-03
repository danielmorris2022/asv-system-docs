# Base Station

This document describes the hardware and software used on the base station to control and monitor the platforms.

## Hardware

The base station is typically a laptop or desktop computer used by the operator.

- One or more laptops/desktops with sufficient CPU, RAM, and storage for running ROS 2, visualization tools, and logging.
- External monitors for improved situational awareness in the field (if available).
- Peripherals such as joysticks/gamepads, headsets, and pointing devices.
- Network interfaces (Wi-Fi and/or Ethernet) used to connect to the platform network.

## Operating System and Software

- Operating system: Ubuntu 22.04 (recommended) or another supported Linux distribution.
- ROS 2 distribution: Humble, installed via the [`ros2_humble_install`](https://github.com/Marine-Robotics-Club/ros2_humble_install) script for consistency across machines.
- Additional tools:
  - `rviz2` for visualization.
  - `rqt` and related plugins for debugging and introspection.
  - Terminal multiplexer (e.g., `tmux`) for managing multiple sessions.
  - Any custom GUIs, dashboards, or logging tools used by the team.

## Connecting to Platforms

To connect the base station to a platform:

1. Join the appropriate network:
   - Connect to the field Wi-Fi SSID or plug in to the wired Ethernet network used by the platforms.
2. Verify connectivity:
   - Ping the platform's IP address (see [network_topology.md](network_topology.md)).
3. Access the platform:
   - Use SSH to log into the onboard computer for diagnostics and control.
4. Configure ROS 2 environment as needed:
   - Ensure the base station and platform share appropriate ROS 2 discovery settings (domain IDs, environment variables, etc.).

Add platform-specific connection details (e.g., hostnames, addresses, and convenience scripts) here as they are standardized.

## Standard Session Layout

Describe a standard workflow for running a mission from the base station.

- Recommended `tmux` layout (e.g., windows for SSH sessions, visualization, logging).
- Common launch scripts or commands used to start monitoring tools and remote nodes.
- Any standard checks (e.g., verifying topics, checking diagnostics) performed before and during missions.

Link to any scripts or helper tools in your code repositories that support these workflows.
