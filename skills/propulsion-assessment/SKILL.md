---
name: propulsion-assessment
description: Perform propellant sizing and trajectory assessments. Use this skill to calculate Delta-V requirements, propellant mass, and thrust-to-weight ratios. Trigger this for "delta-v calculation," "propellant sizing," or "burn duration."
---

# Propulsion & Trajectory Assessment Skill

This skill calculates the "Gas Gauge" for the mission, ensuring the spacecraft has sufficient propellant to complete its orbital maneuvers.

## Analysis Domains

### 1. Delta-V ($\Delta V$) Requirements
- **Standard Maneuvers**: Maintain a reference table for standard maneuvers (e.g., LEO to GTO, LLO to Lunar Surface).
- **Gravity Losses**: Account for gravity losses during high-thrust burns.
- **Margin**: Always apply a 5-10% $\Delta V$ margin for navigation errors and ACS (Attitude Control System) "unloading."

### 2. Tsiolkovsky Rocket Equation
- **Equation**: $\Delta V = I_{sp} \cdot g_0 \cdot \ln(m_{initial} / m_{final})$
- **Propellant Mass**: $m_p = m_{initial} \cdot (1 - e^{-\Delta V / (I_{sp} \cdot g_0)})$
- **Specific Impulse ($I_{sp}$)**: Use engine-specific values (e.g., 300s for Hydrazine, 450s for Hydrolox).

### 3. Thrust-to-Weight (T/W) Ratio
- **Surface Launches/Landings**: Ensure $T/W > 1.0$ (relative to local gravity) for ascent/descent.
- **Burn Duration**: $t_b = (m_p \cdot I_{sp} \cdot g_0) / F_{thrust}$.

## Output Format
1. **Propulsion Report (`propulsion_report.md`)**: Breakdown of maneuvers, $\Delta V$ budget, required propellant mass, and predicted burn durations.
2. **Feasibility Note**: Flag if the required propellant exceeds the tank capacity defined in the Mass Budget.
