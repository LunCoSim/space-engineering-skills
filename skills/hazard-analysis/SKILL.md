---
name: hazard-analysis
description: Perform top-down Hazard Analysis (HA) and System Safety assessments. Use this skill to identify hazards, define safety controls (inhibits), and trace them to mission requirements. Trigger this for "hazard report," "safety analysis," or "risk index."
---

# Hazard Analysis Skill (System Safety)

This skill performs top-level safety assessments to prevent damage to the spacecraft, launch vehicle, or crew. It complements the bottom-up FMECA from `reliability-assessment`.

## Safety Assessment Workflow

### 1. Hazard Identification
- **Categories**: Identify hazards such as Fire, Explosion, Collision, Toxicity, Pressure Release, and Battery Thermal Runaway.
- **Top-Down Perspective**: Start with the undesirable event and work backward to causes.

### 2. Risk Ranking
- **Severity**: 
  - Catastrophic (Loss of mission/life)
  - Critical (Major subsystem loss)
  - Marginal (Degraded mission)
  - Negligible (Minor impact)
- **Likelihood**: Frequent, Probable, Occasional, Remote, Improbable.
- **Risk Index**: Determine the 5x5 matrix position (e.g., 1A, 2C).

### 3. Hazard Controls & Inhibits
- **Controls**: Define design features or operational procedures to mitigate hazards.
- **Inhibits**: For high-severity hazards, define independent safety inhibits (e.g., "Two-fault tolerant").
- **Requirement Trace**: Every control MUST be traced to a requirement in `requirements-manager`.

### 4. Integration with Reliability
- **Input**: Proactively check `reliability-assessment` reports (like `risk_assessment.md`).
- **Linking**: Failure modes identified in FMECA should be cited as "Causes" in the Hazard Reports.

## Output Format
1. **Hazard Report (`hazard_report.md`)**: A detailed document containing Hazard IDs, Descriptions, Risk Indices (Before/After Control), and specific Verification Methods.
2. **Safety Control Matrix (`controls.csv`)**: A summary file for quick tracing to requirements and test plans.

## Safety Standards
- Prefer nomenclature consistent with **SSP 30559** (ISS Safety) or **ECSS-Q-ST-40-02C** (Europe) where applicable.
