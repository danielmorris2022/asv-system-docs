# Network Topology

This document describes how all platforms (WAMV-1, WAMV-2, RoboBoat, TowFish) and the base station are connected on the network.

## High-Level Overview

In typical operation, a shore-based or boat-based **base station** connects to each platform over a dedicated field network.

- The base station connects to WAMV-1, WAMV-2, RoboBoat, and any TowFish logging system via Wi-Fi and/or wired Ethernet.
- Onboard computers on each platform connect to local switches or access points, which in turn connect to the base station network.
- The same network is used for ROS 2 traffic, telemetry, and remote access (SSH, monitoring tools).

Different profiles may be used for **lab testing** (wired or internal Wi-Fi) versus **field operations** (dedicated AP or boat-based router). Document those profiles here.

## IP Addressing

Use this table to record the actual IP addresses and subnets used in your deployment:

| Device / Platform | Interface | IP Address | Subnet        | Notes                          |
|-------------------|-----------|-----------|--------------|---------------------------------|
| Base station      | wlan0     |           |              | Primary operator machine        |
| Base station      | eth0      |           |              | Wired link (if used)           |
| WAMV-1 computer   | eth0      |           |              | Onboard main computer          |
| WAMV-2 computer   | eth0      |           |              | Onboard main computer          |
| RoboBoat computer | eth0      |           |              | Onboard main computer          |
| TowFish logger    |           |           |              | Onboard or tow-vessel logger   |

Add additional rows as needed for switches, access points, and other devices.

## Switches and Access Points

Document the network infrastructure used to connect platforms and the base station.

- Switches on each platform (location, port usage).
- Access points or routers providing Wi-Fi connectivity, including SSIDs and passwords (if appropriate for internal docs).
- Any VLANs or segmented networks (e.g., separating control and payload traffic).

## Ports and Services

Record key ports and services used across platforms.

- ROS 2 uses DDS over UDP on a range of ports; domain IDs may be used to separate deployments.
- Telemetry, web interfaces, or other services (e.g., dashboards, logging servers) may listen on specific TCP/UDP ports.
- Note any firewall rules, port forwarding, or NAT considerations that affect communication between the base station and platforms.
