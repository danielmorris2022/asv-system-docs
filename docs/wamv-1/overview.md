# WAMV-1 Overview

WAMV-1 is an FAU Marine Robotics Club WAM-V 16 surface vessel configured as a research and competition platform for RoboBoat and RobotX tasks.

It uses the standard WAM-V 16 twin-pontoon hull and cross-structure (see the WAM-V CAD assemblies in `docs/resources/wamv_and_auv_cad.md` for the base geometry and structural layout). The vessel carries modular payloads on the bow and mid-deck along with a control box, sensor mast, and support structures derived from the `Mechanical-Wam-V` CAD tree.

For competition work, WAMV-1 typically runs:

- Low-level control on a Jetson Xavier (LL1, `192.168.1.110`).
- High-level autonomy, perception, and mission logic on a Jetson Orin (HL1, `192.168.1.120`).
- Network connectivity via an onboard router (`192.168.1.1`) and Ubiquiti EdgeRouter/bridge (`192.168.1.3`, `192.168.1.12`) connecting to the base station.

Additional context and historical system documentation can be found in:

- WAMV-16 2022–23 System Documentation (Google Drive).
- FAU WAMV-16 Software Documentation (Google Drive).
- RoboBoat and RobotX design reports and papers summarized in `docs/resources/papers_and_reports.md`.
