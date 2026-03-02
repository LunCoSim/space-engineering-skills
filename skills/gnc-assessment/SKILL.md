---
name: gnc-assessment
description: Perform Guidance, Navigation, and Control (GNC) and ADCS assessments. Use this skill to calculate pointing budgets, size actuators (reaction wheels, torque rods, thrusters), define sensor suites, and specify attitude modes. Trigger this for "pointing budget," "ADCS sizing," "stabilization analysis," "attitude control," or "reaction wheel sizing."
---

# GNC & ADCS Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill performs sizing and performance estimation for the Guidance, Navigation, and Control (GNC) subsystem — also called Attitude Determination and Control System (ADCS).

## Before You Begin

Ask the user (if not already known):
1. What are the pointing requirements? (accuracy, stability, and knowledge — in degrees or arcseconds)
2. What is the orbit? (affects disturbance torques — LEO has drag/magnetic, GEO has solar radiation pressure)
3. Is 3-axis stabilization required, or is spin stabilization acceptable?
4. Are there slew requirements? (e.g., agile imaging needs fast repointing)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (requirements flowdown, actuator trade), Phase B (detailed sizing)
- **Supporting**: Phase C (pointing budget refinement), Phase D (calibration planning)

## Attitude Mode Definitions

Define these modes for the mission (tailor as needed):
- **Detumble**: Post-separation rate damping (typically B-dot control using magnetorquers)
- **Sun-Safe / Safe Mode**: Sun-pointing for power, minimal actuator use
- **Nadir-Pointing**: Earth/body-facing for payload or comms
- **Inertial-Pointing**: Fixed celestial target (e.g., star observation)
- **Slew**: Transition between targets
- **Thrust**: Attitude hold during propulsive maneuvers

## Core Analysis Workflows

### 1. Pointing Error Budget
- **Components**: Accuracy (knowledge error), Stability (jitter), Control Error.
- **Statistical method**: Root Sum Square (RSS) for independent sources.
- **Output**: Table of error sources, values (arcsec or deg), and contribution.
- **Key drivers**: Star tracker accuracy, reaction wheel vibration, structural flexibility.

### 2. Disturbance Torque Estimation
- **Solar Radiation Pressure**: $T_{SRP} = F_{SRP} \cdot A \cdot c_{ps}$ (force × area × CG-to-CP offset)
- **Gravity Gradient**: $T_{GG} = \frac{3\mu}{2R^3}|I_z - I_y|\sin(2\theta)$ (uses MOI from `structural-assessment`)
- **Magnetic**: Relevant for LEO — residual dipole interacting with Earth's field
- **Aerodynamic drag**: LEO only — $T_{drag} = F_{drag} \cdot c_{pa}$

### 3. Actuator Sizing
- **Reaction Wheels**: Momentum storage $H = T_{dist} \cdot t_{accumulation}$ over one orbit or desaturation interval.
- **Magnetorquers**: Dipole moment ($Am^2$) to desaturate wheels — only usable near magnetic-field body.
- **Thrusters**: For large slews, orbit maneuver attitude hold, or when magnetic desaturation isn't available.
- **Slew rate**: $T = I \cdot \alpha$ — check that wheels can provide the required angular acceleration.

### 4. Sensor Suite
- **Sun sensors**: Coarse (~1°) for safe mode, fine (~0.1°) for power-positive pointing
- **Star trackers**: High accuracy (~arcsec), but blinded by Sun/Moon exclusion zones
- **IMUs**: Rate sensing, needed for slew control and during eclipses
- **Magnetometers**: LEO only — for B-dot detumble and magnetic field model
- **GPS receivers**: LEO orbit determination (not attitude, typically)

## Output Format
1. **Pointing Budget (`pointing_budget.md`)**: Error breakdown against requirements.
2. **ADCS Sizing Report (`adcs_report.md`)**: Actuator and sensor selection with rationale, disturbance torque estimates.
3. **Mode Transition Table**: Which modes use which actuators/sensors.

## Interface
- **Reads from**: `/requirements/`, `/analysis/structural-assessment/` (MOI, CG-CP offset), `/analysis/mission-analysis-specialist/` (orbit parameters, eclipse)
- **Writes to**: `/analysis/gnc-assessment/`
- **Consumed by**: `systems-engineering-assessment` (mass/power), `flight-software-architect` (processing needs), `power-assessment` (actuator power)
