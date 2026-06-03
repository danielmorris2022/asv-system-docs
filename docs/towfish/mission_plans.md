# TowFish Mission Plans

## Overview

This page stores planned TowFish missions and reusable mission templates. It should be updated before and after each field operation.

## Mission 1: WAM-V TowFish Survey at Duzaway Houseboat

**Date:** Tuesday, June 9, 2026

**Time:** 10:00 AM - 4:00 PM

**Location:** Duzaway Houseboat Artificial Reef

**Coordinates:** 26 deg 06.677' N, 80 deg 03.716' W

**Approximate Decimal Coordinates:** 26.111283, -80.061933

**Vessel:** McAllister

**Vehicle:** FAU WAM-V

**Crew Members:**

- Danny Morris
- Xavi Vicent-Navarro
- Raul Velasco
- John Frankenfield
- Dr. Dhanak
- John Shaw (Captain)

### Description

The FAU team will conduct an offshore test of the WAM-V and TowFish system at the Duzaway Houseboat artificial reef. The WAM-V will be transported to the survey area aboard the McAllister and deployed near the wreck site.

The primary objectives are to evaluate the WAM-V's ability to autonomously tow the TowFish while executing a lawnmower survey pattern and to verify reliable TowFish data acquisition in an offshore environment. Data collected will include magnetometer, IMU, pressure/depth, GPS, and vehicle telemetry.

Survey lines will be conducted over and around the Duzaway Houseboat wreck to assess whether the TowFish can record repeatable magnetic anomalies associated with a known submerged ferrous structure.

### Objectives

1. Verify safe deployment and recovery of the WAM-V and TowFish.
2. Verify WAM-V towing behavior in open-ocean conditions.
3. Verify autonomous lawnmower pattern execution while towing.
4. Verify continuous TowFish telemetry and data logging.
5. Detect repeatable magnetic anomalies associated with the Duzaway Houseboat.

### Suggested Survey Plan

1. Conduct dockside telemetry check.
2. Transit to the Duzaway site aboard the McAllister.
3. Deploy the WAM-V and TowFish.
4. Run one direct pass over the target coordinates.
5. Run a reverse-direction pass over the same line.
6. Execute a small lawnmower survey centered on the wreck.
7. Repeat the central pass after the lawnmower survey.
8. Recover TowFish and WAM-V.
9. Verify data file integrity before leaving the area.

### Success Criteria

- WAM-V completes at least one controlled tow pass.
- TowFish telemetry remains continuous.
- Magnetometer produces numeric values.
- No leak state is detected.
- RP2040 does not enter repeated watchdog reboot cycle.
- Magnetic field shows a repeatable feature near the expected wreck location.

## Mission 2: Magnetometer Validation Test

**Date:** Thursday, June 11, 2026

**Location:** Offshore validation site

**Coordinates:** 26.1610, -80.0800

**Vehicle:** FAU WAM-V and TowFish

### Description

This mission is focused on magnetometer accuracy, repeatability, and data quality. The WAM-V will tow the TowFish through controlled passes around the validation coordinates. The goal is to evaluate baseline stability, repeatability across repeated lines, and sensitivity to tow motion.

### Objectives

1. Verify stable magnetometer operation during offshore towing.
2. Evaluate repeatability of magnetic readings over repeated passes.
3. Compare magnetometer readings with TowFish IMU and pressure/depth behavior.
4. Confirm continuous data logging and timestamp consistency.
5. Build a baseline dataset for future magnetic anomaly surveys.

### Suggested Survey Plan

1. Perform dockside telemetry and magnetometer check.
2. Tow to the validation area.
3. Run repeated straight-line transects through the coordinate.
4. Run a small lawnmower pattern centered on the coordinate.
5. Repeat one selected transect three or more times.
6. Review raw magnetometer values before recovery if possible.

### Success Criteria

- Magnetometer remains numeric and stable.
- Repeated lines produce consistent baseline trends.
- No major data dropouts occur.
- TowFish pressure/depth and IMU data are valid.
- Data can be post-processed into magnetic field vs. time and magnetic field vs. position plots.

## Reusable TowFish Mission Template

**Date:**

**Time:**

**Location:**

**Coordinates:**

**Vessel:**

**Vehicle:**

**Crew Members:**

**Mission Type:**

- [ ] Bench / dockside test
- [ ] Manual tow test
- [ ] Autonomous WAM-V tow test
- [ ] Magnetometer validation
- [ ] Known-target magnetic anomaly survey
- [ ] Pressure/depth test
- [ ] Software/logging test

### Description

Write a short description of the planned mission.

### Objectives

1.
2.
3.

### Planned Survey Pattern

- Pattern type:
- Box size:
- Line spacing:
- Tow speed:
- Tow cable length:
- Target depth:

### Required Data

- [ ] Raw TowFish telemetry
- [ ] Processed TowFish CSV
- [ ] WAM-V GPS track
- [ ] WAM-V heading and speed
- [ ] Field notes
- [ ] Photos or screenshots

### Abort Criteria

- Leak detected.
- Loss of TowFish telemetry.
- Loss of WAM-V control.
- Tow cable near propulsion.
- Unsafe traffic, weather, or sea state.
- Battery below safe threshold.

### Post-Mission Deliverables

- Raw data log.
- Processed CSV.
- Summary plots.
- Field notes.
- Lessons learned.
