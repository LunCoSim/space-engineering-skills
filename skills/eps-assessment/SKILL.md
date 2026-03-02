---
name: eps-assessment
description: Perform detailed Electrical Power System (EPS) sizing and analysis. Use this skill to calculate solar array area (BOL/EOL), battery capacity requirements based on depth-of-discharge (DOD), and power bus performance. Trigger this when the user asks for "solar array sizing," "battery trade-offs," "EPS analysis," or "power system margins."
---

# EPS Assessment Skill

This skill performs detailed Electrical Power System (EPS) analysis for spacecraft missions. It builds on the top-level power budget to provide actionable engineering parameters for solar arrays and batteries.

## Core Analysis Workflows

### 1. Solar Array Sizing (BOL/EOL)
- **Inputs**: Required Power ($P_{req}$), Solar Flux ($S$), Cell Efficiency ($\eta$), Sun Incidence Angle ($\theta$), Degradation Rate ($F_d$), Mission Life ($L$).
- **Calculations**:
    - **P_out per unit area**: $P_{area} = S \cdot \eta \cdot \cos(\theta) \cdot I_d$ (where $I_d$ is inherent degradation, typically 0.77-0.9).
    - **EOL Factor**: $L_d = (1 - F_d)^L$.
    - **Required Area**: $A = P_{req} / (P_{area} \cdot L_d)$.
- **Outputs**: Required Solar Array Area ($m^2$), BOL Power, EOL Power.

### 2. Battery Sizing
- **Inputs**: Eclipse Power ($P_{ecl}$), Eclipse Duration ($t_{ecl}$), Depth of Discharge ($DOD$), Battery Efficiency ($\eta_{bat}$), Transmission Efficiency ($\eta_{trans}$), Nominal Voltage ($V_{nom}$).
- **Calculations**:
    - **Required Energy (Wh)**: $E_{req} = (P_{ecl} \cdot t_{ecl}) / (\eta_{bat} \cdot \eta_{trans} \cdot DOD)$.
    - **Capacity (Ah)**: $C = E_{req} / V_{nom}$.
- **Constraints**: Li-ion DOD is typically capped at 40% for LEO (high cycle count) and up to 80% for GEO (low cycle count).

### 3. Power Bus Assessment
- **Inputs**: Peak Load, Average Load, Bus Voltage.
- **Rules**: Check if Peak Load exceeds Battery Discharge Current capacity. Verify Voltage Drop margins (typically < 2%).

## Output Template

### Assessment Report (`eps_report.md`)
```markdown
# EPS Assessment: [Mission Name]

## 1. Solar Array Analysis
- **Target EOL Power**: [X] W
- **Required Area**: [X] m² (Efficiency: [X]%, Margin: [X]%)
- **BOL/EOL Ratio**: [X]

## 2. Battery Storage Analysis
- **Eclipse Energy Required**: [X] Wh
- **Total Nameplate Capacity**: [X] Wh / [X] Ah
- **Assumed DOD**: [X]%
- **Cycle Life Impact**: [Low/Medium/High]

## 3. Findings & Recommendations
- [Traffic Light: Green/Yellow/Red]
- [Key Constraint/Risk]
```

## Reference Data
- **Solar Flux**: Earth (1367 $W/m^2$), Moon (1367 $W/m^2$), Mars (589 $W/m^2$).
- **Li-ion Density**: ~150-250 Wh/kg.
- **Solar Cell Efficiency**: Triple Junction (28-32%), Silicon (15-20%).

## Writing Style
- Use SI units (Watts, Watt-hours, Kilograms, Meters).
- Always include a **Rationale** for DOD and Efficiency assumptions.
- Flag "Power Deficit" states (Generated Power < Charge + Load).
