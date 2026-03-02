---
name: payload-assessment
description: Perform payload and instrument sizing for space missions. Use this skill to estimate optical system parameters (aperture, GSD, FOV), RF instrument sizing (SAR, radiometer), in-situ instrument planning, data rate derivation, and payload-to-bus resource requirements. Trigger for "payload sizing," "instrument design," "GSD calculation," "sensor SNR," "payload data rate," or "science instrument."
---

# Payload Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill sizes the mission payload — the instruments or systems that fulfill the mission's primary objective. Every other subsystem exists to support the payload, so this skill's outputs drive the bus design.

## Before You Begin

Ask the user (if not already known):
1. What is the mission objective? (Earth observation, science, comms relay, technology demo, ISRU, etc.)
2. What type of payload?
   - **Optical**: Camera, spectrometer, lidar, telescope
   - **RF**: SAR, radiometer, transponder, antenna
   - **In-situ**: Mass spectrometer, drill, sample handler, seismometer
   - **Communication**: Relay transponder, inter-satellite link
   - **Other**: Robotic arm, 3D printer, biological experiment
3. What are the key performance requirements? (resolution, coverage, sensitivity, data rate)
4. What orbit/altitude? (Drives ground resolution for remote sensing)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (payload trade, first-order sizing), Phase B (preliminary design)
- **Supporting**: Phase C (performance verification), Phase D (calibration planning)

## Analysis Workflows

### 1. Optical Payload Sizing
- **Ground Sampling Distance (GSD)**: $GSD = h \cdot p / f$ where $h$ = altitude, $p$ = pixel pitch, $f$ = focal length.
- **Diffraction limit**: $\theta = 1.22 \lambda / D$ where $D$ = aperture diameter.
- **Swath width**: $W = 2h \tan(FOV/2)$ or $W = N_{pixels} \times GSD$.
- **SNR estimation**: Signal-to-noise ratio depends on solar irradiance, surface reflectance, atmospheric transmission, detector quantum efficiency, and integration time.
- **MTF (Modulation Transfer Function)**: Budget for optics, detector, motion blur, and jitter (from `gnc-assessment`).

### 2. RF Payload Sizing
- **SAR**: Resolution = $\delta_r = c/(2B)$ (range) and $\delta_{az} = L_{ant}/2$ (azimuth). Power-aperture product drives overall system size.
- **Radiometer**: Sensitivity $\Delta T = T_{sys} / \sqrt{B \cdot \tau}$ where $B$ = bandwidth, $\tau$ = integration time.
- **Communication relay**: Size transponders, antennas, and link budget per `communications-assessment`.

### 3. In-Situ Instruments
- **Mass spectrometer**: Mass, power, and data rate estimates. Heritage instruments provide reference.
- **Drill / Sample handling**: Depth, torque, sample volume, contamination control requirements.
- **Seismometer / magnetometer**: Sensitivity, noise floor, deployment requirements.

### 4. Payload Resource Requirements
Derive the payload's demands on the bus:
- **Mass**: Instrument mass + optics + baffle + electronics + harness.
- **Power**: Continuous and peak power during observation modes.
- **Data rate**: Raw data rate = pixels × bits/pixel × frame rate (for imagers) or bandwidth × bits/sample (for RF).
- **Data volume**: Data rate × observation time per orbit. Feed to `communications-assessment` for downlink sizing.
- **Thermal**: Instrument dissipation and temperature limits. Feed to `thermal-assessment`.
- **Pointing**: Required accuracy, stability, and knowledge. Feed to `gnc-assessment`.

## Output Format
1. **Payload Sizing Report (`payload_report.md`)**: Instrument parameters, mass/power/data estimates, key performance metrics.
2. **Bus Requirements Flowdown**: Table of payload-driven requirements for each bus subsystem (mass, power, thermal, pointing, data).
3. **Trade Notes**: If multiple instrument options were considered, document the trade rationale.

## Interface
- **Reads from**: `/requirements/` (mission objectives, performance requirements)
- **Writes to**: `/analysis/payload-assessment/`
- **Consumed by**: `systems-engineering-assessment` (mass/power budget), `thermal-assessment` (dissipation), `gnc-assessment` (pointing requirements), `communications-assessment` (data volume), `structural-assessment` (CG impact), `power-assessment` (payload power)
