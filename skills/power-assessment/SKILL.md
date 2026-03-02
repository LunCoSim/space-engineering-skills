---
name: power-assessment
description: Perform detailed Electrical Power System (EPS) sizing and analysis. Use this skill to model solar array degradation, battery depth-of-discharge (DoD), and power bus regulation. Trigger this for "solar array sizing," "battery life," or "EPS architecture."
---

# Power Assessment Skill (EPS)

This skill performs detailed Electrical Power System (EPS) analysis for spacecraft missions. It provides high-fidelity sizing for solar arrays and batteries, moving beyond simple orbital averages to detailed energy balance.

## Core Analysis Workflows

### 1. Solar Array Sizing (BOL/EOL)
- **Inputs**: Required Power ($P_{req}$), Solar constant ($1361 W/m^2$), Cell Efficiency ($\eta$), Sun Incidence Angle ($\theta$), Degradation Rate ($F_d$), Mission Life ($L$), Packing Factor.
- **Calculations**:
    - **P_out per unit area**: $P_{area} = S \cdot \eta \cdot \cos(\theta) \cdot I_d$ (where $I_d$ is inherent degradation, typically 0.77-0.9).
    - **EOL Factor**: $L_d = (1 - F_d)^L$ (Radiation, thermal cycling, and UV browning).
    - **Required Area**: $A = P_{req} / (P_{area} \cdot L_d)$.
- **Outputs**: Required Solar Array Area ($m^2$), BOL Power, EOL Power.

### 2. Battery & Energy Storage
- **Inputs**: Eclipse Power ($P_{ecl}$), Eclipse Duration ($t_{ecl}$), Depth of Discharge ($DOD$), Battery Efficiency ($\eta_{bat}$), Transmission Efficiency ($\eta_{trans}$), Nominal Voltage ($V_{nom}$).
- **Sizing**: $E_{req} (Wh) = (P_{ecl} \cdot t_{ecl}) / (\eta_{bat} \cdot \eta_{trans} \cdot DOD)$.
- **Life Cycle**: Estimate battery health based on Depth of Discharge (DoD) per orbit over the mission duration. Li-ion DOD is typically capped at 40% for LEO (high cycle count) and up to 80% for GEO (low cycle count).
- **Thermal Interdependency**: Check battery temperature limits (typically 0°C to 30°C for charge).

### 3. Power Distribution & Architecture
- **Bus Regulation**: Trade-off between Regulated (e.g., 28V) vs. Unregulated (Battery Voltage) buses.
- **Peak Power**: Model Peak Power Tracking (MPPT) efficiency vs. Direct Energy Transfer (DET).
- **Voltage Drop**: Verify margins (typically < 2%).

## Output Templates

### 1. Power Analysis Report (`power_analysis.md`)
- **Solar Array Analysis**: Target EOL Power, Required Area, BOL/EOL Ratio.
- **Battery Storage Analysis**: Eclipse Energy Required, Total Nameplate Capacity (Wh/Ah), Assumed DOD, Cycle Life Impact.
- **Findings & Recommendations**: Traffic light status, key constraints, and risk assessment.

### 2. EPS Configuration (`eps_config.csv`)
Summary of solar area, battery Wh, and bus voltage.

## Reference Data
- **Solar Flux**: Earth (1367 $W/m^2$), Moon (1367 $W/m^2$), Mars (589 $W/m^2$).
- **Li-ion Density**: ~150-250 Wh/kg.
- **Solar Cell Efficiency**: Triple Junction (28-32%), Silicon (15-20%).

## Writing Style
- Use SI units (Watts, Watt-hours, Kilograms, Meters).
- Always include a **Rationale** for DOD and Efficiency assumptions.
- Flag "Power Deficit" states (Generated Power < Charge + Load).

