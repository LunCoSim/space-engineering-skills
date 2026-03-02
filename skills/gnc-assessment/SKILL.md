---
name: gnc-assessment
description: Perform Guidance, Navigation, and Control (GNC) and ADCS assessments. Use this skill to calculate pointing budgets, size actuators (reaction wheels, torque rods), and define sensor requirements. Trigger this for "pointing budget," "ADCS sizing," or "stabilization analysis."
---

# GNC & ADCS Assessment Skill

This skill performs the sizing and performance estimation for the Guidance, Navigation, and Control (GNC) subsystem, also known as Attitude Determination and Control System (ADCS).

## Core Analysis Workflows

### 1. Pointing Error Budgets
- **Calculations**: Sum of Accuracy (Knowledge), Stability (Jitter), and Control Error.
- **Statistical Method**: Use Root Sum Square (RSS) for independent error sources.
- **Output**: A table showing Error Source, Value (arcsec or deg), and Contribution to Total Error.

### 2. Actuator Sizing (Momentum & Torque)
- **Reaction Wheels**: Calculate required momentum storage $H = T \cdot \Delta t$ based on external disturbance torques (Solar Radiation Pressure, Gravity Gradient, Magnetic).
- **Torque Rods**: Size magnetic dipoles ($Am^2$) required to desaturate reaction wheels within a specific portion of the orbit.
- **Slew Rate**: Estimate torque required for maneuvers: $T = I \cdot \alpha$ (Inertia $	imes$ Angular Acceleration).

### 3. Sensor Selection & Knowledge
- **Requirements**: Define required update rates (Hz) and 3-sigma accuracy for Sun Sensors, Star Trackers, IMUs, and Magnetometers.
- **Environment**: Factor in Earth albedo for sun sensors and "Moon/Sun exclusion zones" for star trackers.

## Output Format
1. **Pointing Budget (`pointing_budget.md`)**: A detailed breakdown of knowledge and control errors against mission requirements.
2. **ADCS Sizing Report (`adcs_report.md`)**: Technical rationale for selected actuator and sensor performance specs.
