# WAMV-1 Networking

## Network Devices

WAMV-1 includes onboard network devices that connect the main computer, controllers, and sensors to the platform network.

- Compact Ethernet switch located near the main electronics bay (update with model and port count).
- Any onboard Wi-Fi access point or bridge used to communicate with the base station.
- Additional interfaces (e.g., direct Ethernet links to specific sensors) as required.

## IP Configuration

The onboard computer on WAMV-1 is assigned a static or DHCP IP address on the platform network.

- Hostname and IP address should be recorded in [common/network_topology.md](../common/network_topology.md).
- Additional network interfaces (e.g., separate management or sensor networks) should be documented if used.

## ROS/ROS2 Networking

WAMV-1 participates in the ROS 2 network for telemetry and control.

- ROS 2 Humble running on Ubuntu 22.04.
- Use of ROS 2 DDS discovery over the platform network to communicate with the base station.
- Domain IDs or other ROS 2 environment settings may be used to separate deployments; document any WAMV-1-specific values here.
- Note any special firewall or QoS settings that affect communication.
