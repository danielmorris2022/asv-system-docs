# MRC Google Drive – High-Level Inventory

This page summarizes the major contents of the Marine Robotics Club (MRC) Google Drive and maps them to how they should be referenced from this GitHub repository.

Only links and descriptions are recorded here. Large files (videos, CAD assemblies, PDFs, datasets) remain in Google Drive.

## Root-Level Items

- **Competition and team videos**
  - `20260529_151911.mp4` – May 2026 video of a competition or test run.
  - `Florida Atlantic University_Team Video.mp4` – team promo or overview video.
  - `Owltonomous_RobotX_2024.mp4` – RobotX 2024 competition footage.
- **Presentations**
  - MRC annual budget proposal presentation (Google Slides).
  - MRC IAB (Industrial Advisory Board) presentation.
  - MRC presentation(s) for students and outreach.

These videos and slides should be referenced from project reports and media pages but not checked into Git.

## Competitions – RoboBoat

### RoboBoat 23–24

- `Hardware/` and `Software/` subfolders for the 2023–2024 RoboBoat season.
- Final Design Report (FDR) PDF: `RB24_LSSU-FAU Amore.pdf`.

### RoboBoat 24–25

- **Battery safety** documentation for RB25 `Owltonomous`.
- `AO/` (Autonomy and Operations) folder with dynamic docs.
- 2023–2024 RoboBoat paper: `RB24_TDR`.

### RoboBoat 25–26

- `Software/` folder with:
  - `YOLO/` – AI perception code, training data references.
  - `WAMV/` – WAM-V software integration for competitions.
- `Electrical-Current/` and `Mechanical/` folders for the current season.
- `Paper/` folder containing:
  - `TDR_Owltonomous_RB2025` (Google Doc).
  - `TDR_Owltonomous_RB2026` (Google Doc).
- `Tutorials/` folder with ROS and simulation tutorials:
  - ROS2 intro.
  - VRX simulator setup.
  - YOLOv9 notes.
  - ROS Jazzy installation.
  - Gazebo Harmonic installation.
- VRX classes spreadsheet.
- **Team handbook**: `Team Handbook - RoboBoat 2025` (Google Doc).
- **IP address docs**: `IP Addresses` and `USV IP Addresses` Google Docs.
- **System documentation**:
  - `WAMV-16 2022-23 System Documentation.pdf`.
  - `Copy of FAU WAMV-16 Software Documentation` (Google Doc).

## Competitions – RobotX

### RobotX 2024

- Competition documentation and system design files for RobotX 2024.

### RobotX 2026

- Folder for RobotX 2026 work, including WAM-V and AUV content.

### WAM-V and AUV Structure

Within RobotX mechanical folders:

- `WAM-V-20241128T155431Z-001/WAM-V/` – WAM-V mechanical content.
- `AUV/` – AUV-related mechanical and design content.
- `Mechanical-Wam-V/` – full WAM-V mechanical tree (see separate CAD page).

## Mechanical – WAM-V and AUV

Key mechanical folders and files:

- `Mechanical-Wam-V/CAD/`
  - `WAM-V.SLDASM` – top-level WAM-V assembly.
  - Subfolders such as `WAM-V_BaseVessel/`, `WAM-V_ControlBox/`, `WAM-V_OwlsNest/`, `WAM-V_PropulsionPod/`, `WAM-V_LARS-VideoRayPro4/`, `WAM-V_Dolly/`, `WAM-V_LoadTraySupport/`, etc.
  - `WAM-V_ISO.JPG` – isometric rendering of the assembled vessel.
  - `2016_Seacat_Full_edit.SLDASM` – underlying hull (Seacat) model.
  - `WAM-V-USVx_Final_Design_Specs_and_Renderings.pdf` – official design specs and renderings.
- `WAM-V_LARS-VideoRayPro4/`
  - `Cage/`, `Pully/`, `StrainRelief/`, `Winch/` – full CAD models of the LARS system.
  - `Cage.SLDASM` and `Cage_BOM.xlsx` – Tow cage assembly and bill of materials.
- `HedyV2.zip` – AUV CAD assembly for the Hedy vehicle.
- `MRC Mechanical.docx` – mechanical notes.
- `JIG/` – fabrication jig CAD.
- `Proposed Changes/` – proposed modifications to WAM-V structures.
- `motorTorque.m` – MATLAB script for motor torque calculations.

## Gantt Charts and Project Planning

- `Gannt Charts/` (overall MRC planning).
  - `June Gannt Chart/Business June Gannt Chart.xlsx` – business-focused plan.
  - `Year Gannt Chart/Overall TimeLine RoboBoat.xlsx` – overall RoboBoat schedule.
  - `Year Gannt Chart/Software TimeLine RoboBoat.xlsx` – software sprint/task schedule.

These should be referenced in project-management docs rather than checked into Git.

## Published Papers and Design Reports

- **RoboBoat papers:**
  - `FAU_RB11_Paper.pdf` – 2011.
  - `FAU_RB12_Paper.pdf` – 2012.
  - `FAU_RB13_Paper.pdf` – 2013.
  - `FAU_RB14_Paper.pdf` – 2014.
  - `TDR_TeamAmore_RB2024.pdf` – 2024 Team Amore TDR.
- **RobotX papers:**
  - `FAU_VU_RX14_Paper.pdf` – RobotX 2014.
  - `FAU_VU_RX16_Paper.docx` – RobotX 2016 draft.
  - `RX22_TDR_FAU-Owltonomous.pdf` – RobotX 2022 TDR.

These are cited in `docs/resources/papers_and_reports.md` with links rather than stored directly in this repo.

## Student Folders

Student-named folders (Adriana, Armando, Hugo, Srada, Travis, Xavi) contain personal or work-in-progress material. When content is promoted to lab-wide documentation, it should be summarized in the relevant platform or competition docs and linked here.
