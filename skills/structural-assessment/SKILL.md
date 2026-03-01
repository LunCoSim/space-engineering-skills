---
name: structural-assessment
description: Perform structural and mass properties analysis for spacecraft. Use this skill to calculate Center of Gravity (CG), Moments of Inertia (MOI), and Margins of Safety under launch loads. Trigger this for "mass properties," "structural margin," or "load analysis."
---

# Structural & Mass Properties Assessment Skill

This skill ensures the physical configuration of the spacecraft is compatible with the launch vehicle and survives the mechanical environment.

## Analysis Domains

### 1. Mass Properties
- **Center of Gravity (CG)**: $\sum(m_i \cdot r_i) / \sum m_i$. Calculate for both stowed (launch) and deployed configurations.
- **Moments of Inertia (MOI)**: $\sum(m_i \cdot d_i^2)$ (Parallel Axis Theorem). Essential for Attitude Control System (ACS) sizing.
- **Product of Inertia**: Identify any principal axis misalignments that could cause "wobble" during spin-stabilized phases.

### 2. Structural Integrity
- **Static Loads**: Apply launch vehicle "G-loads" (typically 6-10G axial/lateral).
- **Stress Calculation**: $\sigma = F/A$ or $M \cdot c / I$ (for bending).
- **Margin of Safety (MoS)**: $MoS = (\text{Allowable Load} / (\text{Limit Load} \times \text{Safety Factor})) - 1$.
    - *Note*: A negative MoS indicates structural failure.
- **Safety Factors**: Typically 1.25 for Yield and 1.5 for Ultimate strength.

### 3. Modal Analysis (Rigidity)
- **Fundamental Frequency**: Estimate based on $f = \frac{1}{2\pi} \sqrt{k/m}$.
- **Requirement**: Ensure the first natural frequency is above the launch vehicle's "Kick" frequency (typically >25 Hz for small sats).

## Output Format
1. **Structural Report (`structural_report.md`)**: Summary of CG position, MOI matrix, and a table of Margins of Safety for critical components.
2. **Configuration Summary**: Note if any "Ballast" mass is required to balance the CG.
