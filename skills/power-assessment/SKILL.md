---
name: power-assessment
description: Perform detailed Electrical Power System (EPS) sizing and analysis. Use this skill to model solar array degradation, battery depth-of-discharge, power bus architecture, and energy balance. Trigger this for "solar array sizing," "battery life," "EPS architecture," "power budget," or "energy balance."
---

# Power Assessment Skill (EPS)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill performs detailed Electrical Power System analysis. It provides sized solar arrays, batteries, and bus architecture — going beyond the summary-level power budget in `systems-engineering-assessment`.

## Before You Begin

Ask the user (if not already known):
1. What is the primary power source? (Solar, RTG, fuel cells, primary batteries — driven by mission type and distance from Sun)
2. What is the orbit? (Eclipse duration from `mission-analysis-specialist` is a critical input)
3. What is the mission lifetime? (Drives degradation and cycle-life calculations)
4. What is the bus voltage? (28V regulated is common; ask if there's a heritage constraint)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (first-order sizing), Phase B (detailed energy balance)
- **Supporting**: Phase C (power profile verification), Phase D (solar array deployment test planning)

## Core Analysis Workflows

### 1. Solar Array Sizing (BOL/EOL)
- **Inputs**: Required power ($P_{req}$), solar constant ($1361\ W/m^2$ at 1 AU), cell efficiency ($\eta$), sun incidence angle ($\theta$), degradation rate ($F_d$), mission life ($L$), packing factor.
- **For non-Earth missions**: Scale solar flux by $1/d^2$ from Sun. Mars: ~589 W/m², Jupiter: ~50 W/m².
- **EOL factor**: $L_d = (1 - F_d)^L$
- **Required area**: $A = P_{req} / (S \cdot \eta \cdot \cos\theta \cdot I_d \cdot L_d)$
- **If solar is not viable** (e.g., outer planets, permanent shadow): Recommend RTG or nuclear and flag for user decision.

### 2. Battery & Energy Storage
- **Inputs**: Eclipse power ($P_{ecl}$), eclipse duration ($t_{ecl}$ from `mission-analysis-specialist`), DOD, battery efficiency, transmission efficiency.
- **Sizing**: $E_{req} = (P_{ecl} \cdot t_{ecl}) / (\eta_{bat} \cdot \eta_{trans} \cdot DOD)$
- **DOD policy**:
  - LEO (high cycle count): 20-40% DOD for Li-ion
  - GEO (low cycle count): up to 80% DOD
  - Lunar night (~14 days): batteries alone are typically insufficient — flag this
- **Thermal**: Battery charge typically 0°C to 30°C — coordinate with `thermal-assessment`.

### 3. Power Distribution & Architecture
- **Bus regulation**: Regulated (28V typical) vs. unregulated (battery voltage varies).
- **Peak power tracking**: MPPT vs. Direct Energy Transfer (DET).
- **Harness losses**: Typically < 2% voltage drop budget.

### 4. Alternative Power Sources
For missions where solar power is insufficient:
- **RTG**: ~120W per unit, multi-decade lifetime, ~4.8% efficiency. Subject to nuclear safety review.
- **Fuel Cells**: Short-duration high-power missions (e.g., crewed lunar sortie).
- **Primary Batteries**: Very short missions only (< days).

## Output Format
1. **Power Analysis Report (`power_analysis.md`)**:
   - Solar array: BOL/EOL power, required area, degradation assumptions
   - Battery: capacity (Wh/Ah), DOD, cycle life
   - 🟢 / 🟡 / 🔴 status
2. **EPS Configuration (`eps_config.csv`)**: Solar area, battery Wh, bus voltage.

## Reference Data
- **Solar Flux**: Earth/Moon ~1361 W/m², Mars ~589 W/m², Jupiter ~50 W/m²
- **Li-ion energy density**: 150-250 Wh/kg
- **Solar cell efficiency**: Triple-junction GaAs 28-32%, Silicon 15-20%, advanced multi-junction >35%

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (eclipse duration, solar distance), `/analysis/thermal-assessment/` (battery temp limits)
- **Writes to**: `/analysis/power-assessment/`
- **Consumed by**: `systems-engineering-assessment` (power budget summary), `propulsion-assessment` (EP power availability), `gnc-assessment` (actuator power)
