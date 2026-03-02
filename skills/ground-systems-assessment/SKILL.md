---
name: ground-systems-assessment
description: Design and assess ground segment infrastructure for space missions. Use this skill to plan mission control centers, ground station networks, launch facility requirements, GSE design, and range safety. Trigger for "ground systems," "mission control," "ground station," "GSE," "ground support equipment," "launch pad," "range safety," "mission operations center," "telemetry tracking," or "ground segment."
---

# Ground Systems Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill sizes and plans the ground infrastructure that supports a space mission — from the launch pad to the mission control center to the tracking network. Ground systems are typically 30-50% of total mission cost for government programs, yet are often an afterthought in early design phases. Addressing them early avoids expensive surprises.

## Before You Begin

Ask the user (if not already known):
1. **Mission type?** (Commercial LEO, deep space, crewed, launch services — each has fundamentally different ground needs)
2. **Existing ground infrastructure?** (Own ground stations, commercial providers like KSAT/AWS Ground Station/SSC, NASA DSN/NEN, ESA ESTRACK)
3. **Telemetry & command approach?** (Direct-to-ground, relay via TDRS/EDRS, ISL to other constellation members)
4. **Launch site constraints?** (Latitude limits inclination; coastal for range safety; logistics for payload transport)
5. What design phase?

## Applicable Phases
- **Primary**: Phase B (ground segment architecture), Phase C (ground system detailed design)
- **Supporting**: Phase A (ground segment feasibility and cost), Phase D (activation and commissioning)

## Analysis Domains

### 1. Mission Control Center (MCC)
- **Functions**: Command generation, telemetry processing, flight dynamics, anomaly management.
- **Sizing drivers**: Number of simultaneous contacts, data rate, autonomy level, crew size.
- **Architecture options**:
  - **Dedicated facility**: Full-time team, purpose-built. ($10-100M+ for build + $5-30M/year ops)
  - **Shared/commercial**: AWS Ground Station, Leaf Space, Azure Orbital — pay-per-pass model.
  - **Lights-out (autonomous)**: Minimal ops team, spacecraft handles routine operations autonomously. Suitable for mature constellations.
- **Key systems**: Telemetry display, command authorization chain, orbit determination, event scheduling, data archive.

### 2. Ground Station Network
- **Functions**: Telemetry downlink, command uplink, ranging/tracking.
- **Sizing**:
  - **Contact time per pass**: LEO at 400-800 km: ~5-15 minutes per pass.
  - **Passes per day per station**: LEO ~4-8 visible passes, 1-4 useful (above minimum elevation).
  - **Data volume**: Total daily data / contact time = required downlink rate.
  - **Required stations**: Depends on latency requirements and data volume. Single station: long gaps (hours). 3 stations at ~120° spacing: gaps < 90 min (LEO).
- **Antenna sizing**: Driven by link budget (see `communications-assessment`).
  - S-band: 3-12 m dish (LEO TT&C)
  - X-band: 5-12 m dish (high-rate EO data)
  - Ka-band: 7-15 m dish (very high rate)
  - DSN 34/70 m: Deep space missions
- **Commercial ground providers**: KSAT SvalSat/TrollSat, SSC, AWS Ground Station, Atlas Space Operations, Leaf Space.

### 3. Launch Facility & Range
- **Launch site selection factors**:
  - Latitude → achievable inclinations (lower latitude = more ΔV savings for equatorial orbits)
  - Azimuth safety corridors → ocean downrange preferred
  - Logistics → payload transport, propellant supply, workforce access
  - Weather → acceptable launch rate
- **Ground Support Equipment (GSE)**:
  - Transporter/Erector/Launcher (TEL) or fixed pad
  - Propellant storage, transfer, and conditioning systems
  - Umbilical and quick-disconnect interfaces
  - Environmental control (cleanroom tent, payload fairing purge)
  - Launch mount, flame trench, water deluge
- **Range safety**: Flight termination system (FTS), telemetry tracking, destruct criteria, population exclusion zones, debris footprint analysis.

### 4. Payload Processing
- **Flow**: Receiving → inspection → integration → fueling → encapsulation → transport to pad → launch.
- **Cleanroom**: ISO Class 7 (10,000) typical, ISO Class 5 (100) for sensitive optics.
- **Hazardous operations**: Propellant loading, ordnance installation — separate facilities, limited personnel, PPE.
- **Schedule**: Typically 2-6 weeks for payload processing at the launch site.

### 5. Data Management & Distribution
- **Level 0 processing**: Raw data decommutation and formatting.
- **Archive**: Short-term (ops) and long-term (mission data archive). NASA PDS for science missions.
- **Distribution**: How does data reach end users? Direct download, cloud storage, API, or dedicated network.
- **Cybersecurity**: CCSDS security standards, encryption for command link, access control for telemetry.

## Cost Drivers

| Element | Rough Cost Range | Notes |
|:---|:---|:---|
| Dedicated MCC (build) | $10-100M | Depends on capability |
| MCC operations (annual) | $5-30M/year | Staffing is dominant cost |
| Ground station (per site) | $1-10M (build) | Antenna + infrastructure |
| Commercial ground pass | $5-50 per minute | Volume discounts available |
| Launch pad (new build) | $50-500M | Highly variable |
| Launch pad refurb | $5-50M | Between campaigns |

## Output Format
1. **Ground Segment Architecture (`ground_systems_report.md`)**: MCC design, ground station network, data flow, and operations concept.
2. **Contact Analysis**: Passes per day, contact time, data throughput per station.
3. **GSE List**: Major ground support equipment items with functions.
4. **Cost Estimate**: Ground segment lifecycle cost (build + operate).
5. **🟢 / 🟡 / 🔴 status**: Ground segment readiness per element.

## Interface
- **Reads from**: `/requirements/`, `/analysis/communications-assessment/` (link budget, data rates), `/analysis/mission-analysis-specialist/` (orbit for contact analysis), `/analysis/mission-operations-manager/` (pass planning, T&C), `/analysis/constellation-design/` (multi-satellite ground architecture)
- **Writes to**: `/analysis/ground-systems-assessment/`
- **Consumed by**: `cost-modeling` (ground segment cost), `systems-engineering-assessment` (ground segment summary), `mission-operations-manager` (ground infrastructure for ops planning)
