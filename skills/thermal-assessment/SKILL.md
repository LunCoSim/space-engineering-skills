---
name: thermal-assessment
description: Perform detailed thermal analysis for spacecraft and rovers across various celestial environments. Use this skill to calculate heat balance, size radiators, determine MLI requirements, and predict nodal temperatures. Trigger this for "thermal modeling," "radiator sizing," or "temperature prediction."
---

# Thermal Engineering Assessment Skill

This skill performs detailed analytical thermal modeling. It calculates the thermal equilibrium of spacecraft components by accounting for environmental fluxes, internal heat generation, and material properties.

## Analysis Domains

### 1. Environmental Heat Flux Calculation
Avoid hardcoded values where possible. Use the following logic for any celestial body:
- **Solar Flux ($G_s$):** Scale based on distance from the Sun ($1/d^2$). 
    - *Example*: ~1361 $W/m^2$ at 1 AU (Earth/Moon), ~589 $W/m^2$ at 1.52 AU (Mars).
- **Albedo Flux ($q_a$):** $G_s \times \text{Albedo Coefficient} \times \text{View Factor}$.
- **Planetary Infrared ($q_{ir}$):** Calculate based on the local surface temperature and emissivity of the celestial body.

### 2. Nodal Heat Balance (Steady State & Transient)
- **Conservation of Energy**: $Q_{in} + Q_{gen} = Q_{out} + m c_p (dT/dt)$.
- **Conduction**: $k A \Delta T / L$ (Fixed couplings between internal nodes).
- **Radiation**: $\epsilon \sigma A (T_1^4 - T_2^4)$ (Coupling to deep space or other nodes).
- **Internal Dissipation ($Q_{gen}$)**: Account for electronics efficiency and heater inputs.

### 3. Thermal Hardware Sizing
- **Multi-Layer Insulation (MLI)**: Use effective emissivity ($\epsilon^*$) to model insulation performance.
- **Radiators**: Size based on $\epsilon \sigma \eta T^4$ to reject the maximum expected heat load in the worst-case hot environment.
- **Heaters**: Determine the power required to maintain nodes above minimum survival/operational limits in the worst-case cold environment.

## Verification & Margins
- **Operational Margin**: Maintain a 10°C buffer between predicted T and component hardware limits.
- **Survival Margin**: Maintain a 5°C buffer for non-operational states.
- **Uncertainty**: State assumptions for contact conductance and coating degradation over time (EOL vs BOL).

## Output Format
1. **Thermal Model Summary (`thermal_model.md`)**: A report defining the environment, nodes, heat loads, and predicted temperature ranges.
2. **Material Recommendations**: Specification of surface treatments (e.g., Anodizing, Black/White Paint) based on $\alpha/\epsilon$ ratios.
