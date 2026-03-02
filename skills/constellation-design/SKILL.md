---
name: constellation-design
description: Design and analyze satellite constellations and distributed space architectures. Use this skill for Walker constellation patterns, coverage analysis, inter-satellite links, orbital shell design, collision avoidance, spectrum coordination, and deorbit compliance. Trigger for "constellation," "Walker pattern," "coverage analysis," "orbital shell," "inter-satellite link," "ISL," "revisit time," "ground coverage," "mega-constellation," or "distributed architecture."
---

# Constellation Design Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill designs and evaluates satellite constellations — multiple spacecraft working together to provide coverage, capacity, or capability that a single satellite cannot. Constellation design is fundamentally different from single-satellite design: the system-level architecture (number of planes, phasing, altitude) drives individual satellite requirements, not the other way around.

## Before You Begin

Ask the user (if not already known):
1. **Mission objective?** (Communications, Earth observation, weather, navigation, IoT, SSA, science — each has different coverage metrics)
2. **Coverage requirement?** (Continuous global, regional, specific latitude bands, revisit time, max gap duration)
3. **Number of satellites (range)?** (Budget-driven constraint — can be 3 or 30,000)
4. **Inter-satellite links?** (Yes = mesh network, reduced ground stations. No = simpler satellites, more ground infrastructure)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (constellation architecture trade), Phase B (orbit and phasing design)
- **Supporting**: Phase C (deployment sequencing), Phase D (on-orbit constellation management)

## Core Concepts

### 1. Walker Constellation Notation
The standard notation for symmetric constellations: **T/P/F**
- **T**: Total number of satellites
- **P**: Number of orbital planes (equally spaced in RAAN)
- **F**: Phasing parameter (relative spacing between planes, 0 ≤ F < P)
- **Example**: GPS = 24/6/1 at 55° inclination, 20,200 km altitude
- **Example**: Iridium = 66/6/1 at 86.4° inclination, 780 km altitude

### 2. Coverage Geometry
- **Half-cone angle (footprint)**: $\rho = \arccos(R_E / (R_E + h))$ — the angular radius of visibility from one satellite.
- **Minimum elevation angle ($\epsilon_{min}$)**: Typical 5-15°. Higher elevation = smaller footprint but better link quality.
- **Effective footprint**: $\theta = 90° - \epsilon_{min} - \arcsin((R_E \cdot \cos\epsilon_{min}) / (R_E + h))$
- **Street-of-coverage width**: For a single plane, the ground swath covered by N satellites evenly spaced.

### 3. Coverage Metrics
| Metric | Definition | Typical Target |
|:---|:---|:---|
| **Continuous coverage** | 100% of target area has ≥1 satellite visible at all times | GPS, comms constellations |
| **Revisit time** | Max time between consecutive passes over a point | EO: 1-24 hours |
| **Max gap** | Longest period with no coverage at any point | Comms: 0 (continuous) |
| **Number of folds** | Minimum simultaneous visible satellites at any point | Navigation: ≥4 for 3D fix |
| **Contact duration** | Time a satellite is visible per pass | LEO: 5-15 min per pass |

### 4. Key Trade Parameters
| Parameter | Lower Value | Higher Value |
|:---|:---|:---|
| **Altitude** | Lower drag, shorter life, smaller footprint, more sats needed | Higher footprint, fewer sats, radiation, higher launch ΔV |
| **Inclination** | Covers equatorial region well, misses poles | Near-polar covers all latitudes, ground track complexity |
| **N satellites** | Cheaper, less coverage, longer revisit | Better coverage, higher cost, more complex management |
| **N planes** | Simpler deployment (all in one plane) | Better longitudinal distribution, needs multiple launches or RAAN drift |

## Analysis Workflows

### 1. Coverage Analysis
- **Analytical (Phase A)**: Use the Walker formula to estimate single-fold or multi-fold coverage for a given T/P/F/i combination.
- **Grid-based**: Discretize the Earth's surface and compute visibility from each satellite at time steps.
- **Higher fidelity**: Recommend STK, GMAT, or Orekit propagation for detailed gap analysis.
- **Latitude dependency**: Coverage is always better near the inclination-matching latitude. Polar coverage requires i > 80°.

### 2. Inter-Satellite Links (ISL)
- **Intra-plane links**: Between adjacent satellites in the same plane. Geometry is relatively stable.
- **Cross-plane links**: Between satellites in adjacent planes. Range and angle vary — most complex for seam planes (where ascending/descending nodes meet).
- **RF ISL**: Proven (TDRS heritage), lower data rate (1-10 Gbps typical).
- **Optical ISL**: Higher data rate (10-100+ Gbps), narrower beam, needs precision pointing (Starlink V2 uses optical ISL).
- **Latency benefit**: ISL routing can be faster than fiber for long distances — signal travels at c in vacuum vs. ~0.67c in fiber.

### 3. Deployment Strategy
- **Shared launch**: Multiple satellites per launch to the same plane. Then phase within the plane using differential drag or low-thrust maneuvering.
- **RAAN spreading**: Either launch to different RAANs directly (expensive) or use J2 precession at different altitudes to naturally drift planes apart.
- **Build-up**: Deploy in phases — initial operating capability (IOC) with partial constellation, full operating capability (FOC) with all satellites.

### 4. Constellation Management
- **Station-keeping**: Drag makeup (LEO), orbit maintenance, RAAN/argument-of-perigee corrections.
- **Spare strategy**: On-orbit spares (parked at different altitude), ground spares, or rapid launch capability.
- **Collision avoidance**: Maneuver authority, conjunction assessment cadence, coordination with other operators.
- **End-of-life**: Deorbit within 5 years of mission end (current guidelines, moving toward 0 years). $\Delta V_{deorbit}$ budget.

### 5. Spectrum & Regulatory
- **ITU coordination**: Frequency filing, interference analysis with existing systems.
- **Orbital debris compliance**: FCC/ITU 25-year rule (legacy), accelerated timelines for large constellations.
- **Licensing**: National licensing (FCC for US, Ofcom for UK, etc.) before deployment.

## Output Format
1. **Constellation Design Report (`constellation_report.md`)**: Walker notation, orbit parameters, coverage analysis, ISL architecture.
2. **Coverage Map / Statistics**: Revisit time, max gap, number of folds by latitude.
3. **Deployment Plan**: Launch manifest, phasing strategy, IOC/FOC timeline.
4. **Per-Satellite Requirements**: Derived requirements for each satellite (mass, power, propulsion for station-keeping and deorbit).
5. **🟢 / 🟡 / 🔴 status**: Coverage compliance, regulatory readiness, collision risk.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (orbital mechanics, ΔV), `/analysis/communications-assessment/` (link budgets for ISL and ground), `/analysis/cost-modeling/` (per-satellite and total constellation cost)
- **Writes to**: `/analysis/constellation-design/`
- **Consumed by**: `systems-engineering-assessment` (per-satellite requirements flowdown), `cost-modeling` (constellation economics — per-sat × quantity), `trade-study-manager` (constellation architecture as a trade option)
