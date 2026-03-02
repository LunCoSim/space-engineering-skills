---
name: hazard-analysis
description: Perform top-down Hazard Analysis (HA) and System Safety assessments. Use this skill to identify hazards, define safety controls (inhibits), trace them to requirements, and integrate with reliability FMECA. Trigger this for "hazard report," "safety analysis," "risk index," "inhibit design," or "system safety."
---

# Hazard Analysis Skill (System Safety)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill performs top-level safety assessments to prevent damage to the spacecraft, launch vehicle, personnel, or environment. It uses a top-down "what could go wrong" approach, complementing the bottom-up FMECA from `reliability-assessment`.

## Before You Begin

Ask the user (if not already known):
1. Is this a crewed or uncrewed mission? (Crewed missions have dramatically stricter safety requirements)
2. What safety standards apply? Common frameworks:
   - NASA: NPR 8715.3 (General Safety), SSP 30559 (ISS Safety), NASA-STD-8719.24 (ODMSP)
   - ESA: ECSS-Q-ST-40-02C (Safety)
   - Commercial: FAR Part 450 (US commercial launch safety)
3. Are there hazardous materials? (Hydrazine, pressurized vessels, pyrotechnics, batteries, RF radiation)
4. What design phase?

## Applicable Phases
- **Primary**: Phase A/B (hazard identification), Phase C (control definition)
- **Supporting**: Phase D (safety review for launch readiness)

## Safety Assessment Workflow

### 1. Hazard Identification
- **Categories**: Fire, Explosion, Collision, Toxicity, Pressure Release, Battery Thermal Runaway, Debris Generation, RF Radiation, Contamination.
- **Top-down**: Start with the undesirable event (e.g., "Loss of spacecraft") and work backward to causes using fault tree logic.

### 2. Risk Ranking
- **Severity**: Catastrophic (loss of life/mission), Critical (major subsystem loss), Marginal (degraded mission), Negligible (minor impact).
- **Likelihood**: Frequent, Probable, Occasional, Remote, Improbable.
- **Risk Index**: 5×5 matrix position (e.g., 1A = Catastrophic-Frequent = unacceptable).

### 3. Hazard Controls & Inhibits
- **Controls**: Design features or operational procedures to mitigate hazards.
- **Inhibits**: For catastrophic/critical hazards, define independent safety inhibits (e.g., "Two-fault tolerant" for crewed missions, "Single-fault tolerant" for uncrewed).
- **Requirement traceability**: Every control MUST be traced to a requirement in `requirements-manager`.

### 4. Integration with Reliability
- **Input**: Check `reliability-assessment` FMECA outputs — failure modes become "Causes" in hazard reports.
- **Feedback**: Hazard controls may drive new reliability requirements (e.g., redundancy).

## Output Format
1. **Hazard Report (`hazard_report.md`)**: Hazard ID, Description, Causes, Risk Index (Before/After Control), Controls, Verification Method.
2. **Safety Control Matrix (`controls.csv`)**: Summary for tracing to requirements and test plans.

## Interface
- **Reads from**: `/requirements/`, `/analysis/reliability-assessment/` (FMECA failure modes)
- **Writes to**: `/analysis/hazard-analysis/`
- **Consumed by**: `requirements-manager` (new safety requirements), `v-and-v-manager` (verification of controls)
