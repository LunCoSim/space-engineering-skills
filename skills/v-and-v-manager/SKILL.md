---
name: v-and-v-manager
description: Manage the Verification and Validation (V&V) process by linking technical assessments to system requirements. Use this skill to build Verification Control Matrices (VCM) and track compliance status.
---

# V&V Manager Skill (Verification & Validation)

This skill closes the engineering loop. It takes the "As-Defined" requirements and compares them against the "As-Modeled" analysis results from other skills.

## Core Integration Architecture

### 1. Requirement Lookup
- Scan for `### [REQ-ID]` headers in the project's `/requirements/` directory.
- For each requirement, identify the defined **Verification Method** (Test, Analysis, Inspection, Demonstration).

### 2. Evidence Retrieval (Automatic)
- Search for corresponding analysis output files (e.g., `assessment_report.md`, `thermal_model.md`).
- **Logic**:
    - If evidence exists: Parse for the relevant parameter (e.g., predicted mass) and compare against the requirement limit.
    - If evidence is missing: Inform the user and suggest running the relevant skill (e.g., "Requirement REQ-PWR-001 needs analysis. Suggest running `thermal-assessment`.").

### 3. VCM Generation
- Create a master table with columns: [ID], [Requirement Text], [Method], [Evidence File], [Compliance Status (PASS/FAIL/OPEN)].

## Verification Rules
- **Compliance Status**:
    - **PASS**: Evidence exists and values are within the required limits (including margins).
    - **FAIL**: Evidence exists but values exceed limits.
    - **OPEN**: No evidence found yet.
- **Verification Level**: Focus on System, Subsystem, and Component-level closure.

## Output Format
1. **Verification Control Matrix (`vcm.md`)**: The master tracking table.
2. **Analysis Requests**: A list of "To-Do" assessments for the user to close "OPEN" requirements.
