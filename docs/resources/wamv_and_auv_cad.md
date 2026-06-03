# WAM-V and AUV CAD Resources

This page summarizes the key mechanical CAD resources for the WAM-V platforms and associated AUVs. The actual SolidWorks assemblies and other large files remain in Google Drive.

## WAM-V Mechanical Assemblies

Primary source: `Mechanical-Wam-V/CAD/` folder in MRC Google Drive.

Key contents:

- **WAM-V.SLDASM** – top-level SolidWorks assembly of the WAM-V platform, including pontoons, cross structure, payload decks, and major subsystems.
- **Subsystem folders** (examples):
  - `WAM-V_BaseVessel/` – base hull and structural elements.
  - `WAM-V_ControlBox/` – control box enclosure and mounting hardware.
  - `WAM-V_OwlsNest/` – superstructure / sensor mast assembly.
  - `WAM-V_PropulsionPod/` – propulsion pod and shroud assemblies.
  - `WAM-V_Dolly/` – transport dolly for moving the vessel on land.
  - `WAM-V_LoadTraySupport/` – payload support structures.
- **WAM-V_ISO.JPG** – isometric rendering of the assembled vessel.
- **2016_Seacat_Full_edit.SLDASM** – underlying Seacat hull model used as the basis for the WAM-V geometry.
- **WAM-V-USVx_Final_Design_Specs_and_Renderings.pdf** – official WAM-V design specifications and rendering package.

These assemblies should be used as the mechanical reference for:

- WAMV-1 and WAMV-2 hardware documentation.
- RobotX competition configuration notes.

## LARS (Launch and Recovery System) for VideoRay Pro4

Source: `WAM-V_LARS-VideoRayPro4/` folder in Google Drive.

Key elements:

- **Cage/** – tow cage and frame for the VideoRay Pro4 ROV.
  - `Cage.SLDASM` – cage assembly.
  - `Cage_BOM.xlsx` – bill of materials for the cage.
- **Pully/** – pulley system CAD.
- **StrainRelief/** – strain relief hardware.
- **Winch/** – winch assembly and mounting.

These models define the mechanical interface between the WAM-V and the tethered ROV.

## AUV – Hedy Vehicle

Source: `HedyV2.zip` and related AUV folders.

- **HedyV2.zip** – zipped SolidWorks assembly of the Hedy AUV.
- Additional AUV mechanical content appears under `AUV/Mechanical/` in the RobotX mechanical directory.

## Supporting Mechanical Files

- **MRC Mechanical.docx** – notes on mechanical design decisions.
- **JIG/** – fabrication jig CAD for WAM-V components.
- **Proposed Changes/** – proposed modifications and improvements to WAM-V structures.
- **motorTorque.m** – MATLAB script for basic motor torque calculations, useful when sizing propulsion or validating design assumptions.

When creating or updating WAM-V hardware documentation in this repo, reference these CAD resources instead of duplicating large binary files.
