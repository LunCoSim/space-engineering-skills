---
name: v-and-v-manager
description: Manage the Verification and Validation (V&V) process by linking technical assessments to system requirements. Use this skill to build Verification Control Matrices (VCM), track compliance status, and ensure all requirements have evidence. Trigger for "verification matrix," "VCM," "compliance check," "requirements closure," or "verification status."
---

# V&V Manager Skill (Verification & Validation)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill closes the engineering loop. It compares "As-Required" (from `requirements-manager`) against "As-Analyzed" or "As-Tested" (from domain skills and `ait-manager`).

## Before You Begin

Ask the user (if not already known):
1. What V&V standard applies?
   - NASA: NPR 7123.1 (SE Processes), NASA-STD-8739 series
   - ESA: ECSS-E-ST-10-02 (Verification)
   - INCOSE: Systems Engineering Handbook Ch. 12
2. What design phase? (Determines which verification methods are expected to have evidence)
   - Phase A/B: Analysis only — most items will be OPEN
   - Phase C: Analysis and some inspection
   - Phase D: Test evidence becomes available
3. Where are the analysis outputs stored? Default: `/analysis/[skill-name]/`

## Applicable Phases
- **Primary**: Phase B (verification planning), Phase C/D (verification execution and closure)
- **Supporting**: Phase A (define verification approach per requirement)

## Core Workflow

### 1. Requirement Lookup
- Scan `/requirements/` for `### [REQ-ID]` headers.
- For each requirement, identify the specified **Verification Method**: Test, Analysis, Inspection, or Demonstration (TAID).

### 2. Evidence Retrieval
- Search `/analysis/` for corresponding output files.
- **Match logic**:
  - REQ-PWR-* → look in `/analysis/power-assessment/`
  - REQ-THR-* → look in `/analysis/thermal-assessment/`
  - REQ-STR-* → look in `/analysis/structural-assessment/`
  - etc.
- If evidence exists: compare the result against the requirement threshold.
- If evidence is missing: flag as OPEN and recommend which skill to run.

### 3. Phase-Appropriate Expectations
| Verification Method | Phase A | Phase B | Phase C | Phase D |
|:---|:---|:---|:---|:---|
| Analysis | OPEN (expected) | Should have evidence | Must have evidence | Must have evidence |
| Test | OPEN | OPEN | OPEN (test planned) | Must have evidence |
| Inspection | OPEN | OPEN | May have evidence | Must have evidence |
| Demonstration | OPEN | OPEN | OPEN | Should have evidence |

### 4. VCM Generation
Create a master table:

| ID | Requirement | Method | Evidence File | Status | Notes |
|:---|:---|:---|:---|:---|:---|
| REQ-PWR-001 | Battery ≥ 1500 Wh EOL | Analysis | `power_analysis.md` | 🟢 PASS | 1620 Wh predicted |
| REQ-THR-003 | Battery 0-30°C operating | Analysis | `thermal_model.md` | 🟡 OPEN | Pending thermal run |
| REQ-STR-010 | First freq > 25 Hz | Test | — | 🔴 OPEN | No test yet |

## Compliance Rules
- **PASS** 🟢: Evidence exists and values meet requirement (including margins).
- **FAIL** 🔴: Evidence exists but values exceed limits.
- **OPEN** 🟡: No evidence yet or evidence is from a previous design iteration.
- **WAIVER**: Requirement formally relaxed with documented justification.

## Output Format
1. **Verification Control Matrix (`vcm.md`)**: Master tracking table.
2. **Analysis Requests**: List of "To-Do" assessments to close OPEN requirements.
3. **Compliance Summary**: Overall percentage of PASS/FAIL/OPEN, with traffic light status.

## Interface
- **Reads from**: `/requirements/`, `/analysis/*/` (all domain skill outputs), `/analysis/ait-manager/` (test evidence)
- **Writes to**: `/analysis/v-and-v-manager/`
- **Consumed by**: Review boards (SRR, PDR, CDR), project management
