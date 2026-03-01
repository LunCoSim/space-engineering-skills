---
name: orbital-conops-manager
description: Define the Concept of Operations (ConOps) and orbital mechanics for Earth-centered missions. Use this skill to structure mission phases, determine eclipse/daylight durations, and plan orbital maneuvers using provided scripts.
---

# Orbital ConOps Manager Skill

This skill defines how the spacecraft lives and moves in orbit. It provides the temporal and spatial context for all other engineering assessments.

## Mission Phases
- **LEOP (Launch & Early Orbit Phase)**: Deployment, detumble, and initial systems checkout.
- **Commissioning**: Payload calibration and performance verification.
- **Nominal Operations**: The primary science or service phase of the mission.
- **Decomissioning/Disposal**: De-orbiting or moving to a graveyard orbit.

## Orbital Analysis
- **Parameters**: Altitude (km), Inclination (deg), Eccentricity, and RAAN.
- **Environment**: Calculate eclipse duration and Beta Angle to inform the Power and Thermal budgets.
- **Ground Access**: Estimate contact windows with ground stations based on orbital height and station coordinates.

## Maneuver Planning
- **Deterministic Logic**: Use embedded scripts (when available) or verified equations for:
    - Hohmann Transfers ($\Delta V$ for altitude changes).
    - Plane Changes ($\Delta V$ for inclination adjustments).
    - Drag Compensation (Station-keeping).
- **Output**: Feeding $\Delta V$ values directly into the `propulsion-assessment` skill.

## Output Format
1. **ConOps Document (`conops.md`)**: A timeline of mission events and a description of orbital states.
2. **Maneuver Schedule**: A table of impulsive burns, their purpose, and their estimated $\Delta V$ costs.
