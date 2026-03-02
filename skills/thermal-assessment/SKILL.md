---
name: thermal-assessment
description: Perform detailed thermal analysis for spacecraft and rovers across various celestial environments. Use this skill to calculate heat balance, size radiators, determine MLI requirements, predict nodal temperatures, and plan thermal hardware. Trigger this for "thermal modeling," "radiator sizing," "temperature prediction," "heater sizing," or "thermal control."
---

# Thermal Engineering Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill performs analytical thermal modeling — calculating thermal equilibrium of spacecraft components by accounting for environmental fluxes, internal heat, and material properties.

## Before You Begin

Ask the user (if not already known):
1. What is the celestial environment? (Earth orbit, lunar surface, Mars, deep space — determines environmental fluxes)
2. What are the critical components and their temperature limits? (Electronics, batteries, propellant, optics, mechanisms)
3. Is this a spinning or 3-axis stabilized spacecraft? (Affects heat distribution)
4. What thermal analysis standard applies?
   - NASA: NASA-HDBK-4002 (Thermal), GSFC-STD-7000 (GEVS Ch.14)
   - ESA: ECSS-E-ST-31 (Thermal Control)
5. What design phase? (Phase A: hand calcs; Phase B+: recommend Thermal Desktop, ESATAN-TMS, or equivalent)

## Applicable Phases
- **Primary**: Phase A (first-order sizing), Phase B (preliminary thermal model)
- **Supporting**: Phase C (detailed model correlation), Phase D (TVAC test planning with `ait-manager`)

## Analysis Domains

### 1. Environmental Heat Flux Calculation
Avoid hardcoded values — scale by mission:
- **Solar flux ($G_s$)**: $1361\ W/m^2$ at 1 AU, scales as $1/d^2$.
- **Albedo ($q_a$)**: $G_s \times a \times F_{view}$ (Earth albedo ~0.3, Moon ~0.12, Mars ~0.25).
- **Planetary IR ($q_{IR}$)**: Based on body surface temperature and view factor.
- **Deep space sink**: 2.7 K cosmic microwave background.

### 2. Nodal Heat Balance
- **Steady state**: $Q_{in} + Q_{gen} = Q_{out}$
- **Transient**: $Q_{in} + Q_{gen} = Q_{out} + mc_p(dT/dt)$
- **Conduction**: $Q = kA\Delta T / L$
- **Radiation**: $Q = \epsilon\sigma A(T_1^4 - T_2^4)$
- **Internal dissipation ($Q_{gen}$)**: Electronics waste heat, heater inputs.

### 3. Thermal Hardware Sizing
- **MLI**: Effective emissivity $\epsilon^*$ (typically 0.01-0.05 for good MLI).
- **Radiators**: Area from $Q_{reject} = \epsilon\sigma\eta A T^4$. Size for worst-case hot.
- **Heaters**: Power to maintain min temperature in worst-case cold.
- **Heat pipes / loop heat pipes**: For spreading heat from high-dissipation components.
- **Thermal straps / isolators**: Conductive coupling or decoupling between components.

### 4. Surface Properties
| Surface | Solar Absorptivity ($\alpha$) | IR Emissivity ($\epsilon$) | $\alpha/\epsilon$ |
|:---|:---|:---|:---|
| White paint (S13G) | 0.20 | 0.85 | 0.24 |
| Black paint (Z306) | 0.95 | 0.85 | 1.12 |
| Bare aluminum | 0.09 | 0.03 | 3.0 |
| Gold tape | 0.23 | 0.04 | 5.75 |
| OSR (Optical Solar Reflector) | 0.08 | 0.80 | 0.10 |

Note: these degrade with UV exposure and atomic oxygen over mission life (BOL vs EOL).

## Verification & Margins
- **Operational margin**: 10°C buffer between predicted T and component hardware limits.
- **Survival margin**: 5°C buffer for non-operational states.
- **Uncertainty**: State assumptions for contact conductance and coating degradation (BOL vs EOL).

## Output Format
1. **Thermal Model Summary (`thermal_model.md`)**: Environment, nodes, heat loads, predicted temperature ranges, and margin status.
2. **Material Recommendations**: Surface treatments with $\alpha/\epsilon$ ratios.
3. **Hardware List**: Radiator area, heater power, MLI coverage, heat pipes.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (eclipse duration, solar distance), `/analysis/power-assessment/` (component dissipation), `/analysis/structural-assessment/` (configuration)
- **Writes to**: `/analysis/thermal-assessment/`
- **Consumed by**: `systems-engineering-assessment` (thermal summary), `power-assessment` (battery temp limits, heater power), `ait-manager` (TVAC test limits), `lunar-conops-manager` (surface thermal environment)
