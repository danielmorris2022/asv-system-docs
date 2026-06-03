# Network Topology

This document describes how all platforms (WAMV-1, WAMV-2, RoboBoat, TowFish) and the base station are connected on the network.

## High-Level Overview

- Describe how the base station connects to each platform (Wi-Fi, wired, etc.).
- Summarize which networks are used in the field versus in the lab.

## IP Addressing

Create a table like:

| Device / Platform | Interface | IP Address | Subnet | Notes |
|-------------------|-----------|-----------|--------|-------|
| Base station      | wlan0     |           |        |       |
| WAMV-1 computer   | eth0      |           |        |       |
| WAMV-2 computer   | eth0      |           |        |       |
| RoboBoat computer | eth0      |           |        |       |
| TowFish logger    |           |           |        |       |

## Switches and Access Points

- List switches, routers, and access points used.
- Document SSIDs, VLANs, and any special configuration (if appropriate).

## Ports and Services

- List key ROS/ROS2 ports, telemetry ports, and any custom services in use.
- Note any firewall rules or port forwarding that matter for field ops.
