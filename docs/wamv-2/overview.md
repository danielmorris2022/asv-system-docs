# WAMV-2 Overview

WAMV-2 is a second WAM-V 16 platform used by the FAU Marine Robotics Club as a complementary research and competition vessel alongside WAMV-1.

The hull and base structure are derived from the same WAM-V 16 CAD models (`Mechanical-Wam-V/CAD/` in Google Drive; see `docs/resources/wamv_and_auv_cad.md`), with mission-specific mounts for environmental sensors and payloads such as:

- A hull-mounted or through-hull depth sensor.
- A weather station on the sensor mast.
- A waterwheel payload and drone platform.

WAMV-2 uses a similar computing architecture to WAMV-1:

- Low-level control on a Jetson Xavier (LL2, `192.168.1.100`).
- High-level autonomy and perception on a Jetson Orin (HL2, `192.168.1.186`).
- An onboard router at `192.168.1.2` and Ubiquiti EdgeRouter/bridge at `192.168.1.4` and `192.168.1.11` for field networking.

WAMV-2 is often used for:

- Environmental and survey missions (depth grids, weather logging).
- Multi-vessel experiments where WAMV-1 and WAMV-2 operate in the same field area.
- Future RobotX configurations that require additional payload capacity.

Mechanical and system-level reference material is shared with WAMV-1 and documented in:

- WAMV-16 System Documentation (Google Drive).
- RobotX and RoboBoat design reports (see `docs/resources/papers_and_reports.md`).
