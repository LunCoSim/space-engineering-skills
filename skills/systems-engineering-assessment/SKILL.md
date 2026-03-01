---
name: systems-engineering-assessment
description: Perform integrated systems engineering assessments. Use this skill to generate Mass, Power, and Link budgets, and to perform first-order thermal sizing based on mission requirements. Trigger this when the user asks for a "mission sizing," "budget update," or "technical assessment."
---

# Systems Engineering Assessment Skill (The Integrator)

This skill performs top-level mission architecture sizing. It integrates inputs from requirements and engineering constants to produce a cohesive view of the spacecraft's feasibility.

## Core Multi-Budget Workflow

### 1. Mass Budgeting
- **Calculations**: Sum of Component Dry Masses + Growth Allowance (Margin) + Propellant Mass (Wet Mass).
- **Rule of Thumb**: Apply 20% margin for "Design" phase, 10% for "Approved."
- **Output**: A table showing Subsystem, Current Mass (kg), Margin (%), and Total Mass.

### 2. Power Budgeting
- **Calculations**: Sum of Orbit Average Power (OAP) and Peak Power.
- **Battery Sizing**: Calculate required Wh based on eclipse duration and depth of discharge (DoD).
- **Thermal Interdependency**: Include heater power (typically 10-20% of basic load) in the eclipse budget.

### 3. Link Budgeting (Communication)
- **Calculations**: EIRP = Transmitter Power + Antenna Gain - Cable Loss.
- **Path Loss**: Calculate based on distance (e.g., LEO, Lunar) and frequency (S-band, X-band).
- **Target**: Maintain a minimum 3 dB margin for telemetry/command.

### 4. Thermal Integration (First-Order Sizing)
- **Radiator Area**: Estimate $A = Q / (\epsilon \sigma T^4)$ where $Q$ is heat to reject.
- **Heater Power**: Estimate survival heater power based on worst-case cold environment (e.g., Lunar Night).

## Output Format
Always produce two outputs:
1. **Assessment Report (`assessment_report.md`)**: A Markdown technical memo explaining the assumptions, margins, and "Traffic Light" status (Green/Yellow/Red) for each budget.
2. **Budgets Snapshot (`budgets.csv`)**: A flat file containing the raw numbers for easier import into external tools.

## Writing Style & Precision
- Use SI units exclusively.
- Always state the "Assumed Margin" for every calculation.
- Flag any "Budget Overruns" immediately (e.g., Current Mass > Launch Capacity).
