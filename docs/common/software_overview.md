# Software Overview

This document summarizes the major software components used across platforms.

## Main Repositories

The following GitHub repositories form the core of the software stack used on the WAMV platforms, RoboBoat, and TowFish:

- [`xvrobotics/wamv_nav`](https://github.com/xvrobotics/wamv_nav) – navigation and autonomy stack for the WAMV platforms. Provides ROS 2 packages for low-level control, higher-level navigation, and mission logic, designed to integrate with the OSRF VRX simulation environment and ROS 2 Humble.
- [`Marine-Robotics-Club/ros2_humble_install`](https://github.com/Marine-Robotics-Club/ros2_humble_install) – shell script to install ROS 2 Humble Desktop on Ubuntu 22.04 host and onboard machines, including necessary dependencies for VRX and related packages.
- [`Marine-Robotics-Club/ros2_humble_velodyne_install`](https://github.com/Marine-Robotics-Club/ros2_humble_velodyne_install) – shell script to install Velodyne LiDAR drivers and create a Vision2 workspace on Ubuntu 22.04, including a standard launch file for Velodyne nodes.

Add any other repositories here as they are adopted into the stack.

## ROS 2 Distribution and Environment

- Primary ROS 2 distribution: **ROS 2 Humble**, running on **Ubuntu 22.04** for development machines and onboard computers.
- Use the `ros2_humble_install` script to standardize installations on new machines.
- VRX simulation (from `osrf/vrx`) is used alongside the WAMV navigation stack for simulation-based development and testing.

## Common Patterns

Across platforms, the software stack generally follows these patterns:

- Core navigation and control logic is implemented in ROS 2 nodes (from `wamv_nav` and related packages).
- Sensor drivers (e.g., GNSS/INS, cameras, LiDAR) publish ROS 2 topics consumed by navigation, perception, and logging nodes.
- Launch files are used to start groups of nodes for specific scenarios (e.g., dock tests, full missions, simulations).
- Configuration files (YAML, etc.) define platform-specific parameters such as thruster mappings, sensor transforms, and controller gains.

See the platform-specific `software_stack.md` and `configuration_and_parameters.md` documents for details on each vehicle.
