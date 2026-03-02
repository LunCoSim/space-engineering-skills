---
name: propulsion-assessment
description: Perform propulsion subsystem sizing and propellant mass estimation. Use this skill to select engine types, size propellant tanks, calculate burn durations, and evaluate electric vs chemical propulsion trades. Trigger this for "propellant sizing," "engine selection," "tank sizing," "burn duration," "electric propulsion," or "thrust-to-weight."
---

# Propulsion Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill sizes the propulsion subsystem hardware — engines, tanks, and feed systems. It consumes the Delta-V budget from `mission-analysis-specialist` and produces propellant mass and hardware specifications for the mass/power budgets.

## Before You Begin

Ask the user (if not already known):
1. What is the mission type and target body?
2. **Chemical or electric propulsion?** (or both — many missions use chemical for large burns and EP for station keeping)
3. Are there constraints on propellant type? (e.g., "green" non-toxic, heritage hydrazine, bipropellant, cold gas for CubeSats)
4. What standards framework applies?
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (trade studies, first-order sizing), Phase B (preliminary design)
- **Supporting**: Phase C (performance verification), Phase D (propellant loading planning)

## Ownership Boundary

| Responsibility | Owner |
|:---|:---|
| Delta-V budget (maneuver list, margins) | `mission-analysis-specialist` |
| Propellant mass, engine selection, tank sizing, feed system | **This skill** |
| Mass budget integration | `systems-engineering-assessment` |

## Analysis Workflows

### 1. Propellant Mass Sizing (Chemical)
- **Input**: Total Delta-V (from `mission-analysis-specialist`), spacecraft dry mass, engine Isp.
- **Tsiolkovsky Equation**: $m_p = m_{dry} \cdot (e^{\Delta V / (I_{sp} \cdot g_0)} - 1)$
- **Margin**: Add ullage (typically 3-5%), residuals (1-2%), and loading uncertainty (1%).
- **Reference Isp values** (ask user to confirm or provide actual):
  - Cold gas (N₂): 65-70s
  - Monopropellant (Hydrazine): 220-230s
  - Green monoprop (AF-M315E/LMP-103S): 235-255s
  - Bipropellant (MMH/NTO): 310-320s
  - Bipropellant (LOX/LH2): 440-460s

### 2. Electric Propulsion Sizing
- **Key difference**: EP is power-limited, not propellant-limited. Thrust is low, burn durations are weeks to months.
- **Input**: Delta-V, available power from `power-assessment`, mission timeline constraints.
- **Thrust**: $F = 2 \eta P / (g_0 \cdot I_{sp})$ where $\eta$ is thruster efficiency.
- **Burn duration**: $t_b = m_p \cdot I_{sp} \cdot g_0 / F$
- **Reference Isp values**:
  - Electrospray: 800-1500s
  - Hall-effect: 1200-1800s
  - Gridded ion (e.g., NSTAR): 2500-3500s
- **Power requirement**: Typically 1-30 kW depending on thruster type. Flag if this exceeds the power budget.

### 3. Tank Sizing
- **Volume**: $V = m_p / \rho_{prop}$ with ullage margin (typically 5-10%).
- **Material**: Titanium (heritage), COPV (mass savings), aluminum (budget).
- **Pressure**: MEOP for blowdown vs. regulated systems.

### 4. Thrust-to-Weight (Chemical Only)
- **Powered descent/ascent**: Require $T/W > 1.0$ relative to local gravity.
- **Orbit maneuvers**: T/W is less critical but affects gravity losses (flag if $T/W < 0.1$).

## Output Format
1. **Propulsion Report (`propulsion_report.md`)**:
   - Engine selection rationale (type, Isp, thrust level)
   - Propellant mass breakdown (usable, ullage, residual, total)
   - Tank specifications (volume, pressure, material)
   - Burn durations per maneuver
   - 🟢 / 🟡 / 🔴 feasibility status
2. **Feasibility flags**: Alert if propellant mass exceeds reasonable wet-mass fraction (>40% for most missions, >80% for lunar/interplanetary)

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (Delta-V budget), `/analysis/power-assessment/` (for EP power availability)
- **Writes to**: `/analysis/propulsion-assessment/`
