---
name: ait-manager
description: Plan Assembly, Integration, and Test (AIT/AIV) phases. Use this skill to define test flows, GSE requirements, and environmental test campaigns. Trigger this for "test plan," "TVAC setup," or "integration flow."
---

# AIT / AIV Management Skill

This skill focuses on the physical realization of the spacecraft, from component delivery to the launch site.

## AIT Planning Workflows

### 1. Test Flow Definition
- **Sequence**: Components -> Subsystem -> System Level Integration.
- **Milestones**: Pre-Environmental Review (PER), Test Readiness Review (TRR), and Post-Test Review.

### 2. Environmental Campaign
- **Thermal Vacuum (TVAC)**: Define cycle counts, dwell times, and survival vs. operational limits.
- **Dynamics**: Sine and Random vibration profiles based on the launch vehicle's User Manual.
- **EMC/EMI**: Identify critical electromagnetic compatibility tests.

### 3. Ground Support Equipment (GSE)
- **MGSE**: Sizing of handling fixtures, transport containers, and mass property measurement tools.
- **EGSE**: Defining "FlatSat" setups, power simulators, and umbilical interfaces.

## Output Format
1. **Integration Flowchart (`ait_flow.md`)**: A step-by-step sequence of the assembly and test campaign.
2. **GSE Requirements (`gse_list.md`)**: A list of required mechanical and electrical ground hardware.
