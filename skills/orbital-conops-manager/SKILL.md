---
name: orbital-conops-manager
description: Define the Concept of Operations (ConOps) for Earth-centered and deep space orbital missions. Use this skill to structure mission phases, plan mission timelines, define operational modes, and coordinate with analysis skills. Trigger for "ConOps," "mission phases," "mission timeline," "operational modes," or "end-of-life plan."
---

# Orbital ConOps Manager Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill defines *what* happens and *when* during the mission — the logical sequence of events from launch to disposal. For the *how* (orbital mechanics, Delta-V), it delegates to `mission-analysis-specialist`.

## Before You Begin

Ask the user (if not already known):
1. What is the orbit type? (LEO, SSO, MEO, GEO, HEO, L-point, interplanetary)
2. What is the mission duration? (months / years)
3. Is there a constellation or formation flying aspect?
4. What are the end-of-life requirements? (de-orbit, graveyard orbit, passivation)
5. What standards framework? (determines disposal compliance rules)

## Applicable Phases
- **Primary**: Phase A (concept definition), Phase B (preliminary design)
- **Supporting**: Phase C/D (operations planning, procedure development)

## Mission Phase Definitions

Every mission should define these phases (tailor as needed):

| Phase | Key Activities | Typical Duration |
|:---|:---|:---|
| **LEOP** | Separation, detumble, initial acquisition, systems checkout | Hours to days |
| **Commissioning** | Payload calibration, performance verification, orbit trim | Days to weeks |
| **Nominal Operations** | Primary science or service operations | Months to years |
| **Extended Operations** | If mission is extended beyond design life | Variable |
| **Decommissioning** | Passivation, de-orbit or graveyard transfer | Days to weeks |

## Operational Mode Definitions

For each mission phase, define spacecraft modes:
- **Science / Service Mode**: Primary payload active, nominal pointing
- **Safe Mode**: Minimum power, sun-pointing, awaiting ground intervention
- **Eclipse Mode**: Battery-powered, heaters active, payload standby
- **Maneuver Mode**: Thrusters enabled, payload off, specific attitude
- **Communication Mode**: Antenna pointed to ground station, high-rate downlink

## End-of-Life Compliance

Ask the user which disposal regulations apply. Common frameworks:
- **NASA**: ODMSP (25-year de-orbit rule for LEO, passivation required)
- **ESA**: ESA Space Debris Mitigation Guidelines (25-year, passivation)
- **FCC**: 5-year de-orbit rule (as of 2024 for US-licensed)
- **ITU**: Frequency coordination and de-registration

Ensure the ConOps includes a disposal maneuver and `mission-analysis-specialist` provides the Delta-V.

## ConOps-Level Resource Awareness
- **Eclipse/Sunlight cycle**: Request eclipse duration from `mission-analysis-specialist`. Use this to inform power modes and thermal states.
- **Beta angle variation**: Flag beta angle ranges that cause extended eclipses or continuous sunlight.
- **Communication windows**: Request ground station access from `mission-analysis-specialist`. Drives data downlink scheduling.

## Output Format
1. **ConOps Document (`conops.md`)**: Phase-by-phase timeline with modes, durations, and key events.
2. **Maneuver Schedule**: Table of burns, their purpose, timing, and Delta-V (from `mission-analysis-specialist`).
3. **Mode Transition Diagram**: Which events trigger mode changes (e.g., eclipse entry → Eclipse Mode).

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (eclipse, access, Delta-V)
- **Writes to**: `/analysis/orbital-conops-manager/`
- **Consumed by**: `power-assessment` (duty cycles), `thermal-assessment` (environment timeline), `mission-operations-manager` (ops planning)
