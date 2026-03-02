---
name: structural-assessment
description: Perform structural and mass properties analysis for spacecraft. Use this skill to calculate Center of Gravity (CG), Moments of Inertia (MOI), Margins of Safety under launch loads, and fundamental frequency estimates. Trigger this for "mass properties," "structural margin," "load analysis," "CG calculation," or "natural frequency."
---

# Structural & Mass Properties Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill ensures the physical configuration of the spacecraft is compatible with the launch vehicle and survives the mechanical environment.

## Before You Begin

Ask the user (if not already known):
1. What launch vehicle? (Determines loads — quasi-static, random vibration, acoustic, shock)
2. Is the Launch Vehicle User Manual available? (Primary source for environmental test levels)
3. What structural standard applies?
   - NASA: NASA-STD-5001 (Structural Design), GSFC-STD-7000 (GEVS)
   - ESA: ECSS-E-ST-32 (Structural)
   - AIAA S-110 / S-111 (Structural verification)
4. What is the spacecraft configuration? (3U CubeSat, microsatellite, large deployable, etc.)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (mass budget, first-order stiffness), Phase B (preliminary structural design)
- **Supporting**: Phase C (detailed FEA, coupled loads analysis), Phase D (structural testing)

## Analysis Domains

### 1. Mass Properties
- **Center of Gravity (CG)**: $CG = \sum(m_i \cdot r_i) / \sum m_i$. Calculate for both stowed (launch) and deployed configurations.
- **Moments of Inertia (MOI)**: $I = \sum(m_i \cdot d_i^2)$ using Parallel Axis Theorem. Essential input for `gnc-assessment`.
- **Products of Inertia**: Identify principal axis misalignments (causes nutation during spin-stabilized phases).
- **Mass growth tracking**: Phase A budgets should include 20-30% margin. Track current best estimate (CBE) vs. allocation.

### 2. Structural Integrity
- **Quasi-static loads**: Apply launch vehicle G-loads (typically 6-10G axial, 2-5G lateral, combined).
- **Dynamic loads**: Random vibration and acoustic environments per LV User Manual.
- **Stress**: $\sigma = F/A$ (axial) or $\sigma = Mc/I$ (bending).
- **Margin of Safety**: $MoS = \frac{F_{allowable}}{F_{applied} \times SF} - 1$
  - SF = 1.25 for Yield, 1.5 for Ultimate (ask user — standards vary)
  - Negative MoS = structural failure — must redesign.

### 3. Fundamental Frequency (Stiffness)
- **Estimate**: $f = \frac{1}{2\pi}\sqrt{k/m}$ for simple models.
- **Requirement**: First natural frequency must be above LV "kick" frequency:
  - Typically > 25 Hz lateral, > 35 Hz axial for small satellites
  - Large spacecraft: check LV manual for specific requirements
- **If below threshold**: Stiffen the structure or add mass constraints.

### 4. Launch Vehicle Interface
- **Adapter**: Separation system type (clamp band, Lightband, motorized), bolt circle diameter.
- **CG envelope**: LV specifies maximum CG height above separation plane.
- **Coupled Loads Analysis (CLA)**: At Phase C, a formal CLA with the LV provider is typically required.

## Output Format
1. **Structural Report (`structural_report.md`)**: CG position (stowed/deployed), MOI matrix, MoS table for critical components.
2. **Mass Properties Summary**: CBE mass, allocation, margin, CG coordinates.
3. **Configuration Notes**: Ballast requirements, deployment sequence constraints.

## Interface
- **Reads from**: `/requirements/`, subsystem mass estimates from all domain skills
- **Writes to**: `/analysis/structural-assessment/`
- **Consumed by**: `systems-engineering-assessment` (mass budget), `gnc-assessment` (MOI, CG-CP offset), `ait-manager` (structural test levels)
