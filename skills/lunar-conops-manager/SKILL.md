---
name: lunar-conops-manager
description: Define surface operations and mission timelines for lunar missions. Use this skill to manage lunar day/night cycles, traverse planning, landing site analysis, and surface resource management. Trigger for "lunar operations," "lunar timeline," "surface traverse," "lunar night survival," or "landing site."
---

# Lunar ConOps Manager Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill focuses on the unique challenges of the lunar surface environment. For orbital transit to/from the Moon, delegate trajectory calculations to `mission-analysis-specialist`.

## Before You Begin

Ask the user (if not already known):
1. What is the landing site? (latitude/longitude — drives sun cycle duration and Earth visibility)
2. Is this a lander, rover, or both?
3. Must the mission survive lunar night? (14 Earth-days of darkness, ~-170°C)
4. Is ISRU (In-Situ Resource Utilization) part of the mission? (e.g., water ice extraction at polar sites)
5. Is there a relay asset? (Lunar Gateway, orbiter, or direct-to-Earth only)
6. What design phase?

## Applicable Phases
- **Primary**: Phase A (site selection, mission concept), Phase B (surface timeline)
- **Supporting**: Phase C/D (operations procedure development)

## Mission Phases
- **Descent & Landing**: Final approach, powered descent, touchdown sequence.
- **Surface Deployment**: Unstow solar arrays, antennas, rover ramps.
- **Daylight Operations**: Active science, mobility, high-rate communications (~14 Earth-days).
- **Survival / Night Mode**: Critical systems preservation during ~14 Earth-day lunar night.
- **Dawn Recovery**: Power-up, thermal recovery, system health check.

## Surface Analysis

### Sun/Night Cycle
- **Non-polar sites**: ~14 Earth-day illumination, ~14 Earth-day darkness (708-hour synodic period).
- **Polar sites**: Peaks of Eternal Light may have >80% illumination. Permanently Shadowed Regions (PSRs) have no direct sunlight.
- Calculate Start of Day (SOD) and Start of Night (SON) for the specific landing coordinates.

### Thermal Context
- Communicate directly with `thermal-assessment`: lunar day surface ~+120°C, lunar night surface ~-170°C.
- Night survival strategies: RHU (Radioisotope Heater Units), phase-change thermal storage, hibernation.

### Traverse Planning (Rovers)
- Define waypoints with science justification.
- Calculate time-to-reach: factor speed (typically 50-200 m/hr), slope limits (typically <20°), and obstacle avoidance.
- Energy cost per traverse segment.

### Communication & Relay
- **Direct-to-Earth (DTE)**: Calculate Earth visibility windows from landing site.
- **Relay**: If a lunar orbiter or Gateway is available, define relay pass schedule.
- **Far-side operations**: Require relay asset (e.g., Queqiao-type at Earth-Moon L2).

## Resource Management
- **Power**: Map battery state-of-charge across the full lunar synodic period. Night survival requires either RTG, batteries, or fuel cells.
- **Data**: Prioritize science data downlink during Earth-visible periods.

## ISRU Awareness
If the mission includes ISRU:
- Define resource extraction operations (timing, power, thermal requirements).
- Coordinate with `power-assessment` for ISRU power demands.
- Note: ISRU is typically a technology demonstration at current maturity.

## Output Format
1. **Lunar Mission Profile (`lunar_profile.md`)**: Phase-by-phase timeline of the lunar stay.
2. **Traverse Plan (`traverse.csv`)**: Waypoints with distance, slope, and estimated power cost.
3. **Communication Schedule**: Earth visibility and relay pass windows.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (transit trajectory, descent delta-v), `/analysis/thermal-assessment/` (surface thermal environment)
- **Writes to**: `/analysis/lunar-conops-manager/`
- **Consumed by**: `power-assessment` (duty cycles), `thermal-assessment` (surface environment), `communications-assessment` (link geometry)
