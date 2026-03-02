---
name: reliability-assessment
description: Perform reliability and radiation risk assessments for space missions. Use this skill to calculate mission life probability, TID, and FMECA. This skill provides the bottom-up failure data used as 'Causes' in the hazard-analysis skill.
---

# Reliability & radiation Assessment Skill

This skill assesses the programmatic and technical risks that could end the mission prematurely, providing data for engineering trades and insurance underwriters.

## Analysis Domains

### 1. Reliability Modeling
- **Reliability Block Diagrams (RBD)**: Calculate system reliability $R_{sys} = \prod R_i$ for series components or $1 - \prod(1-R_i)$ for parallel/redundant components.
- **Bathtub Curve**: Consider Infant Mortality, Useful Life (set by $\lambda$, failure rate), and Wear-out phases.
- **MTTF/MTBF**: Calculate Mean Time To Failure (or Between Failures).

### 2. Radiation Assessment
- **Total Ionizing Dose (TID)**: Estimate based on orbital altitude and duration (e.g., LEO vs GEO vs Van Allen Belts).
- **Single Event Effects (SEE)**: Identify risks from SEU (Upset), SEL (Latch-up), or SEB (Burnout).
- **Shielding Estimation**: Calculate the effectiveness of Aluminum shielding using simplified "Depth-Dose" curves.

### 3. FMECA (Failure Mode, Effects, and Criticality Analysis)
- **Identification**: List potential failure modes for each subsystem.
- **Criticality**: Rank by Severity (1-4) and Probability (A-E).
- **Mitigation**: Recommend hardware redundancy or software "safe modes."

## Output Format
1. **Risk Matrix (`risk_assessment.md`)**: A report containing the RBD results, estimated TID, and a 5x5 Criticality Matrix.
2. **Probability of Success**: A final percentage value for the mission surviving its primary duration (e.g., "95% probability of 1-year mission success").
