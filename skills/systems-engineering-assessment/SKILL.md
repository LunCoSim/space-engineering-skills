---
name: systems-engineering-assessment
description: Perform integrated systems engineering assessments. Use this skill to generate and integrate Mass, Power, and Link budgets from domain skill outputs. Use this as the top-level integrator, budget arbitrator, and conflict resolver. Trigger this when the user asks for a "mission sizing," "budget update," "technical assessment," or when budgets from different subsystems need reconciliation.
---

# Systems Engineering Assessment Skill (The Integrator)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill is the **top-level integrator**. It does not perform detailed subsystem analysis — it synthesizes outputs from domain skills into a cohesive mission-level view and flags conflicts.

## Before You Begin

Ask the user (if not already known from project context):
1. What is the mission type and target body?
2. What standards framework applies? (NASA / ECSS / other)
3. What design phase are we in? (A/B/C/D — this sets the margin policy)
4. Is there an existing mass/power envelope from a launch vehicle or mission constraint?

## Applicable Phases
- **Primary**: Phase A (concept sizing), Phase B (preliminary design)
- **Supporting**: Phase C/D (budget maintenance and change impact assessment)

## Core Responsibility: Budget Integration (Not Derivation)

You own the **summary-level budgets**. Detailed calculations belong to domain skills. Your job is to:
1. **Collect** outputs from domain skills (from `/analysis/[skill-name]/`)
2. **Integrate** them into system-level budget tables
3. **Flag conflicts** — mass overruns, power deficits, link margin violations
4. **Arbitrate** — recommend trades or re-allocations when budgets don't close

### 1. Mass Budget
- **Sources**: Component dry masses from all subsystem assessments + propellant mass from `propulsion-assessment`.
- **Margins**: Apply growth allowance per design phase (Phase A: 20-30%, Phase B: 15-20%, Phase C: 5-10%).
- **Constraint**: Compare total wet mass against launch vehicle capacity.

### 2. Power Budget
- **Source**: Read detailed power analysis from `power-assessment` (solar array area, battery sizing, bus architecture).
- **Your role**: Integrate orbit-average and peak power across all subsystems into a summary table. Include heater power from `thermal-assessment`.
- **Do NOT re-derive** solar array sizing or battery capacity — reference the domain skill output.

### 3. Link Budget
- **Source**: Read detailed link budget from `communications-assessment` (EIRP, path loss, margins).
- **Your role**: Verify the link margin meets mission requirements. Summarize uplink/downlink status.
- **Do NOT re-derive** EIRP or path loss — reference the domain skill output.

### 4. Thermal Summary
- **Source**: Read thermal predictions from `thermal-assessment`.
- **Your role**: Verify all components remain within temperature limits. Summarize radiator area and heater power.

## Conflict Resolution Protocol

When you detect a conflict (e.g., mass > launch capacity, power deficit in eclipse):
1. ⚠️ Flag it clearly with the conflicting values and sources.
2. Present the human engineer with options (e.g., "Reduce payload mass by 5 kg" OR "Switch to a larger launch vehicle").
3. Do NOT silently adjust numbers — the engineer decides.
4. Record the decision rationale in the output.

## Output Format
1. **Assessment Report (`assessment_report.md`)**: A Markdown technical memo with:
   - Summary table for each budget (Mass, Power, Link, Thermal)
   - 🟢 / 🟡 / 🔴 traffic light status per budget
   - Margin analysis with design-phase-appropriate growth factors
   - Conflicts and recommended trades
2. **Budgets Snapshot (`budgets.csv`)**: Flat file for import into external tools.

## Interface
- **Reads from**: `/requirements/`, `/analysis/power-assessment/`, `/analysis/communications-assessment/`, `/analysis/thermal-assessment/`, `/analysis/propulsion-assessment/`, `/analysis/structural-assessment/`, `/analysis/gnc-assessment/`
- **Writes to**: `/analysis/systems-engineering-assessment/`
