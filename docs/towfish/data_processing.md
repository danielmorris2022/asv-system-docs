# TowFish Data Processing

## Overview

TowFish data processing converts raw telemetry logs into usable engineering and survey products. The main goals are:

- Verify telemetry quality.
- Extract magnetometer, IMU, pressure/depth, and health-monitoring data.
- Detect packet dropouts and invalid fields.
- Plot magnetic field against time and position.
- Compare magnetic anomalies with known target locations.
- Evaluate TowFish motion and depth behavior during tow operations.

The current workflow uses a combination of raw serial logging, Python logging, and MATLAB post-processing.

## Raw Telemetry Format

The main TowFish firmware outputs comma-separated telemetry packets wrapped in angle brackets.

Typical packet structure:

```text
<euler_x,euler_y,euler_z,accel_x,accel_y,accel_z,gyro_x,gyro_y,gyro_z,
mag_x,mag_y,mag_z,quat_w,quat_x,quat_y,quat_z,linacc_x,linacc_y,linacc_z,
gravity_x,gravity_y,gravity_z,battery_v,leak_status,leak_voltage,temp_c,
hall1,hall2,hall3,pressure_mbar,pressure_temp_c,depth_m,twinleaf ...>
```

Important notes:

- Packets are not pure numeric CSV rows.
- The packet contains text fields such as `NO_LEAK`.
- The final Twinleaf field is raw text.
- Angle brackets should be removed during parsing.
- Commas inside future Twinleaf messages could break simple CSV parsing.

## Twinleaf Magnetometer Field

The Twinleaf / QuSpin magnetometer adapter has been observed outputting text lines such as:

```text
74 42369.9 29 4
75 42367.5 29 4
76 42358.6 29 4
```

Working interpretation:

```text
sample_counter magnetic_field_nT status_or_counter status_code
```

The second value is the magnetic field magnitude in nanotesla. Local baseline values around `42360-42375 nT` have been observed during bench testing.

Invalid or not-ready readings may appear as:

```text
16 nan 4 1
17 nan 4 1
```

During processing, preserve both numeric magnetic values and the raw text field so invalid states can be reviewed later.

## Recommended Processed Columns

A processed TowFish CSV should include at minimum:

```text
sample
time_s
valid_packet
num_fields
euler_x_deg
euler_y_deg
euler_z_deg
accel_x
accel_y
accel_z
gyro_x
gyro_y
gyro_z
imu_mag_x
imu_mag_y
imu_mag_z
imu_mag_norm
quat_w
quat_x
quat_y
quat_z
linacc_x
linacc_y
linacc_z
gravity_x
gravity_y
gravity_z
battery_v
leak_status
leak_voltage
temperature_c
hall_1
hall_2
hall_3
pressure_mbar
pressure_temp_c
depth_sensor_m
depth_calc_m
depth_best_m
twinleaf_raw
twinleaf_sample
twinleaf_field_nT
twinleaf_status_1
twinleaf_status_2
```

## Parsing Rules

Recommended parser behavior:

1. Read raw file line-by-line.
2. Remove leading and trailing whitespace.
3. Remove `<` and `>` packet wrappers.
4. Split the line into fields.
5. Convert known numeric fields with safe conversion.
6. Preserve text fields separately.
7. Extract Twinleaf values from the raw Twinleaf string.
8. Add quality flags for malformed rows, missing IMU data, missing pressure data, missing Twinleaf values, and leak status.

Avoid using simple `readmatrix` on raw logs because text fields may become `NaN` and column alignment may be lost.

## Common MATLAB Issue

An earlier post-processing script produced:

```text
Error using plot
Invalid data argument.
```

This occurs when MATLAB imports values as strings, cells, or table variables instead of numeric arrays. Always verify data classes before plotting.

Example:

```matlab
class(time_imu)
class(x_accel)
size(time_imu)
size(x_accel)
```

## Recommended Plots

For each mission, generate:

1. Magnetic field vs. time.
2. Magnetic field vs. GPS position, if GPS is available.
3. Magnetic anomaly map or contour plot.
4. TowFish depth vs. time.
5. Pressure vs. time.
6. IMU roll/pitch/yaw vs. time.
7. Acceleration and gyroscope vs. time.
8. Battery voltage vs. time.
9. Leak voltage and leak status vs. time.
10. Data-quality flags vs. time.

## Magnetic Anomaly Processing

For wreck or artificial reef testing:

1. Establish a local baseline magnetic field away from the target.
2. Subtract or detrend the baseline.
3. Plot the magnetic residual.
4. Compare anomaly peaks against known target coordinates.
5. Check whether the anomaly repeats on multiple passes.
6. Compare anomaly shape with TowFish motion and depth.

A successful detection is not only a single spike. The stronger evidence is a repeatable anomaly that appears at the expected location across multiple survey lines.

## GPS Synchronization

When WAM-V GPS is available, magnetic field readings should be synchronized to vehicle position. If the TowFish logger and WAM-V logger are separate, record:

- Computer time.
- GPS time, if available.
- WAM-V position.
- TowFish sample time.
- Tow speed and heading.

Future work should standardize timestamps across the WAM-V and TowFish logs.

## Data Quality Checks

After every test, check:

- Number of total packets.
- Number of valid packets.
- Packet rate.
- Duration.
- Missing IMU samples.
- Missing pressure samples.
- Missing Twinleaf samples.
- Leak flags.
- Battery voltage minimum and maximum.
- Magnetometer numeric percentage.
- Magnetometer `nan` count.

## File Naming

Recommended file naming format:

```text
Mission_YYYY-MM-DD_HH-MM-SS_raw.csv
Mission_YYYY-MM-DD_HH-MM-SS_processed.csv
Mission_YYYY-MM-DD_HH-MM-SS_summary.txt
```

Recommended folder structure:

```text
data/
  raw/
  processed/
  figures/
  notes/
```

## Post-Mission Deliverables

For each field mission, save:

- Raw telemetry log.
- Processed CSV.
- Summary text file.
- Magnetometer plot.
- Depth plot.
- IMU plot.
- Vehicle track plot, if GPS is available.
- Field notes.

## Lessons Learned

- Raw TowFish files are mixed numeric/text logs, not clean numeric matrices.
- The Twinleaf field must be parsed separately.
- `nan` in Twinleaf output can indicate not-ready sensor state rather than serial failure.
- Plotting should use processed numeric arrays, not raw table fields.
- Every mission file should eventually include a header row or a matching metadata file.
