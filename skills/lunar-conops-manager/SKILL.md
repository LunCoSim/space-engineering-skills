---
name: lunar-conops-manager
description: Define surface operations and mission timelines for lunar missions. Use this skill to manage 14-day lunar day/night cycles, traverse planning for rovers, and landing site selection assets.
---

# Lunar ConOps Manager Skill

This skill focuses on the unique challenges of the lunar surface environment, specifically the extreme duty cycles and environmental constraints. For the orbital transit to the moon or Earth-approach trajectories, this skill should delegate and coordinate with the `mission-analysis-specialist`.

## Mission Phases
- **Descent & Landing**: Final approach and touchdown sequence.
- **Surface Deployment**: Unstowing solar arrays, antennas, or rover ramps.
- **Daylight Operations**: Active science, mobility, and high-rate communications.
- **Survival/Night Mode**: Critical systems preservation during the 14-day lunar night.

## Surface Analysis
- **Sun/Night Cycle**: Calculate the Start of Day (SOD) and Start of Night (SON) for specific lunar coordinates.
- **Thermal Context**: Direct communication with `thermal-assessment` regarding the -170°C night and +120°C day limits.
- **Traverse Planning**: For rovers, define "Waypoints" and calculate time-to-reach based on speed and slope constraints.

## Resource Management
- **Power Duty Cycle**: Map battery charge levels across the 708-hour lunar synodic period.
- **Earth Visibility**: Calculate when direct-to-earth (DTE) communication is possible from the landing site.

## Output Format
1. **Lunar Mission Profile (`lunar_profile.md`)**: A phase-by-phase timeline of the lunar stay.
2. **Traverse Plan (`traverse.csv`)**: For rovers, a list of waypoints with distance and estimated power cost.
