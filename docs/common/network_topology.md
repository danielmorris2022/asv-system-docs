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

| Device / Platform     | Interface | IP Address    | Subnet        | Notes                                   |
|-----------------------|-----------|---------------|---------------|-----------------------------------------|
| Base station router   |           | 192.168.1.5   | 192.168.1.0/24| SSID WAMV16_BASE2                       |
| WAMV1 router          |           | 192.168.1.1   | 192.168.1.0/24| SSID WAMV16_CB1                         |
| WAMV2 router          |           | 192.168.1.2   | 192.168.1.0/24| SSID WAMV16_CB2                         |
| WAMV1 edge router     |           | 192.168.1.3   | 192.168.1.0/24| Ubiquiti EdgeRouter                     |
| WAMV2 edge router     |           | 192.168.1.4   | 192.168.1.0/24| Ubiquiti EdgeRouter                     |
| RoboBoat edge router  |           | 192.168.1.6   | 192.168.1.0/24| Ubiquiti EdgeRouter                     |
| Ubiquiti base AP      |           | 192.168.1.10  | 192.168.1.0/24| Ubiquiti radio at base station          |
| Ubiquiti WAMV2 AP     |           | 192.168.1.11  | 192.168.1.0/24| Ubiquiti radio on WAMV2                 |
| Ubiquiti WAMV1 AP     |           | 192.168.1.12  | 192.168.1.0/24| Ubiquiti radio on WAMV1                 |
| WAMV1 Jetson Xavier   |           | 192.168.1.110 | 192.168.1.0/24| Low-level controller (LL1)              |
| WAMV1 Jetson Orin     |           | 192.168.1.120 | 192.168.1.0/24| High-level controller (HL1)             |
| WAMV2 Jetson Xavier   |           | 192.168.1.100 | 192.168.1.0/24| Low-level controller (LL2)              |
| WAMV2 Jetson Orin     |           | 192.168.1.186 | 192.168.1.0/24| High-level controller (HL2)             |
| RoboBoat Amcrest cam1 |           | 192.168.1.131 | 192.168.1.0/24| Camera 1                                |
| RoboBoat Amcrest cam2 |           | 192.168.1.132 | 192.168.1.0/24| Camera 2                                |
| RoboBoat Amcrest cam3 |           | 192.168.1.133 | 192.168.1.0/24| Camera 3                                |

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
