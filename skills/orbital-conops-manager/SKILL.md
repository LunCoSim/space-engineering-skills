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
- **Decommissioning/Disposal**: De-orbiting or moving to a graveyard orbit.

## Strategic Planning
Focus on the *logic* and *timing* of mission events. For technical orbital calculations (Delta-V, ground tracks, eclipse durations), always consult the `mission-analysis-specialist` skill.

## Maneuver Strategy
- **Phase Definition**: Define *when* and *why* a maneuver is needed (e.g., "Orbit raising to operational altitude during Day 2").
- **Collaboration**: Provide the mission requirements to `mission-analysis-specialist` to get the necessary $\Delta V$ values, then pass those to `propulsion-assessment` for fuel sizing.

## Output Format
1. **ConOps Document (`conops.md`)**: A timeline of mission events and a description of orbital states.
2. **Maneuver Schedule**: A table of impulsive burns, their purpose, and their estimated $\Delta V$ costs.
