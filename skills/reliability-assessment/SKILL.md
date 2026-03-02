---
name: reliability-assessment
description: Perform reliability, radiation risk, and failure mode assessments for space missions. Use this skill to calculate mission life probability, TID exposure, FMECA, and parts derating. This skill provides the bottom-up failure data used as "Causes" in hazard-analysis. Trigger for "FMECA," "reliability," "radiation," "TID," "single event effects," or "parts screening."
---

# Reliability & Radiation Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill assesses what could fail and why, providing bottom-up failure analysis data that feeds into `hazard-analysis` (top-down) and informs design trades.

## Before You Begin

Ask the user (if not already known):
1. What orbit/trajectory? (Drives radiation environment — LEO is mild, GEO/MEO Van Allen belts are harsh, interplanetary is variable)
2. Are parts space-grade (Class S/B), military (Class M), or COTS? (Dramatically affects failure rates and radiation tolerance)
3. What mission lifetime? (Drives Total Ionizing Dose accumulation and wear-out)
4. What reliability standard applies?
   - NASA: NPR 8705.4, EEE-INST-002
   - ESA: ECSS-Q-ST-30 (Dependability), ECSS-Q-ST-60 (EEE Components)
   - MIL: MIL-STD-1629 (FMECA), MIL-HDBK-217 (failure rates)
5. What design phase?

## Applicable Phases
- **Primary**: Phase B (preliminary FMECA, radiation assessment), Phase C (detailed FMECA, parts selection)
- **Supporting**: Phase A (risk identification), Phase D (anomaly investigation)

## Analysis Domains

### 1. Reliability Modeling
- **Reliability Block Diagrams (RBD)**: $R_{sys} = \prod R_i$ (series) or $R_{sys} = 1 - \prod(1-R_i)$ (parallel/redundant).
- **Failure rates ($\lambda$)**: Use MIL-HDBK-217 or FIDES for initial estimates. Actual rates from manufacturer data where available.
- **MTTF/MTBF**: $MTTF = 1/\lambda$ for exponential distribution.
- **Redundancy**: Identify single points of failure (SPOFs). Recommend cold, warm, or hot redundancy where criticality warrants it.

### 2. Radiation Assessment
- **Total Ionizing Dose (TID)**: Estimate based on orbit, duration, and shielding. Use AP-8/AE-8 or AP-9/AE-9 models for trapped radiation.
- **Single Event Effects (SEE)**: SEU (bit flip — recoverable), SEL (latch-up — potentially destructive), SEB (burnout — catastrophic).
- **Shielding**: Aluminum equivalent thickness. Typical spacecraft provide 3-10 mm Al shielding. Spot shielding for sensitive parts.
- **Parts derating**: Ensure components operate below maximum rated values (typically 60-80% derating for voltage, current, power).

### 3. FMECA
- **Identification**: List failure modes for each subsystem/component.
- **Effects**: What happens at component, subsystem, and system level?
- **Criticality**: Severity (1-4) × Probability (A-E) matrix.
- **Mitigation**: Hardware redundancy, software FDIR, operational procedures.
- **Feed to `hazard-analysis`**: Failure modes become "Causes" in the top-down hazard reports.

### 4. Parts & Materials
- **Screening**: Space-grade (expensive, radiation-hard, long lead) vs. COTS (cheap, needs testing/shielding, shorter lead).
- **Obsolescence**: Flag parts with end-of-life risk over mission development timeline.
- **Outgassing**: Materials near sensitive surfaces must meet ASTM E595 (TML < 1.0%, CVCM < 0.1%).

## Output Format
1. **Risk Assessment Report (`risk_assessment.md`)**: RBD results, TID estimate, 5×5 Criticality Matrix, SPOF list.
2. **Probability of Success**: Mission reliability estimate (e.g., "93% probability of completing 2-year primary mission").
3. **FMECA Table**: Failure modes, effects, severity, probability, mitigation.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (orbit for radiation environment)
- **Writes to**: `/analysis/reliability-assessment/`
- **Consumed by**: `hazard-analysis` (failure modes as causes), `flight-software-architect` (rad-hard requirements), `systems-engineering-assessment` (risk summary)
