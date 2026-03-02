---
name: ait-manager
description: Plan Assembly, Integration, and Test (AIT/AIV) phases. Use this skill to define test flows, GSE requirements, environmental test campaigns, model philosophy, and cleanliness requirements. Trigger this for "test plan," "TVAC setup," "integration flow," "model philosophy," or "GSE requirements."
---

# AIT / AIV Management Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill focuses on the physical realization of the spacecraft — from component delivery through environmental testing to launch site operations.

## Before You Begin

Ask the user (if not already known):
1. What is the model philosophy? (Proto-flight, full qualification + flight, engineering model only)
2. What is the launch vehicle? (Determines environmental test levels)
3. What cleanliness class is required? (Driven by payload type — e.g., optical payloads need Class 10,000 or better)
4. What standards framework? (NASA GEVS, ECSS-E-ST-10-03, MIL-STD-1540)
5. What design phase?

## Applicable Phases
- **Primary**: Phase C (test planning), Phase D (execution)
- **Supporting**: Phase B (early test philosophy definition)

## Model Philosophy

Define the build philosophy early — it drives cost, schedule, and risk:

| Model | Purpose | Typical Usage |
|:---|:---|:---|
| **STM** (Structural/Thermal Model) | Structural qualification, thermal balance test | When mass/thermal are high-risk |
| **EM** (Engineering Model) | Electrical integration, FSW development | Most programs |
| **QM** (Qualification Model) | Full environmental qualification campaign | When heritage is low |
| **FM** (Flight Model) | The actual flight unit | Always |
| **PFM** (Proto-Flight Model) | Combined QM+FM at reduced test levels | CubeSats, constrained budgets |

## AIT Planning Workflows

### 1. Test Flow Definition
- **Sequence**: Components → Subsystem → System-Level Integration
- **Milestones**: Pre-Environmental Review (PER), Test Readiness Review (TRR), Post-Test Review
- **Philosophy**: test-as-you-fly, fly-as-you-test

### 2. Environmental Campaign
- **Thermal Vacuum (TVAC)**: Define cycle counts, dwell times, survival vs. operational limits. Reference thermal limits from `thermal-assessment`.
- **Vibration**: Sine sweep, random vibration, and acoustic profiles based on launch vehicle User Manual.
- **Shock**: Separation shock levels (typically pyro-induced).
- **EMC/EMI**: Radiated and conducted emissions/susceptibility per applicable standard.

### 3. Ground Support Equipment (GSE)
- **MGSE**: Handling fixtures, transport containers, lifting devices, mass property measurement tools.
- **EGSE**: FlatSat setups, power simulators, umbilical interfaces, checkout software.
- **Verify GSE**: GSE itself requires verification before use on flight hardware.

### 4. Cleanliness & Contamination
- **Cleanroom class**: Specify ISO class per integration phase (e.g., ISO 7 for general, ISO 5 for optics).
- **Molecular contamination**: Define outgassing requirements (CVCM, TML) for materials near sensitive surfaces.
- **Particulate**: Define particle fallout budgets if applicable.

## Output Format
1. **Integration Flowchart (`ait_flow.md`)**: Step-by-step sequence of assembly, integration, and test activities with milestone reviews.
2. **GSE Requirements (`gse_list.md`)**: List of MGSE and EGSE with specifications.
3. **Environmental Test Matrix**: Table mapping each test type to the applicable model and test level (qualification vs. acceptance).

## Interface
- **Reads from**: `/requirements/`, `/analysis/structural-assessment/` (loads for vib levels), `/analysis/thermal-assessment/` (temperature limits for TVAC)
- **Writes to**: `/analysis/ait-manager/`
- **Consumed by**: `v-and-v-manager` (test evidence)
