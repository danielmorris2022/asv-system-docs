# WAMV-2 Networking

## Network Devices

WAMV-2 includes onboard network devices that connect the main computer, environmental sensors, controllers, and payloads to the platform network.

- Compact Ethernet switch located near the main electronics bay (update with model and port count).
- Any onboard Wi-Fi access point or bridge used to communicate with the base station.
- Additional interfaces (e.g., direct Ethernet links to the depth sensor or weather station) as required.

## IP Configuration

The onboard computer on WAMV-2 is assigned a static or DHCP IP address on the platform network.

- Hostname and IP address should be recorded in [common/network_topology.md](../common/network_topology.md).
- Interfaces connected to environmental sensors (depth, weather) should be documented, including any dedicated subnets.

## ROS/ROS2 Networking

WAMV-2 participates in the ROS 2 network for telemetry, control, and environmental data.

- ROS 2 Humble running on Ubuntu 22.04.
- Use of ROS 2 DDS discovery over the platform network to communicate with the base station.
- Domain IDs or other ROS 2 environment settings may be used to separate deployments; document any WAMV-2-specific values here.
- Note any special firewall or QoS settings that affect communication, particularly for higher-rate sensor data.
