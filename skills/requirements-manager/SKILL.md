---
name: requirements-manager
description: Manage space engineering requirements in a human-readable, trackable format. Use this skill whenever the user wants to define, update, trace, or review system requirements, component specifications, verification criteria, mass/power budgets as requirements, or perform traceability audits. Trigger for "requirement," "specification," "shall statement," "traceability," or "requirements review."
---

# Requirements Management Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill defines how to manage engineering requirements. It is the foundation skill — every other skill checks requirements before doing analysis.

## Before You Begin

Ask the user (if not already known):
1. What requirements standard/format do they follow?
   - INCOSE Guide for Writing Requirements
   - ISO/IEC/IEEE 29148
   - NASA NPR 7123.1
   - ECSS-E-ST-10-06
   - Or project-specific — this skill's Markdown format is always acceptable
2. Where should requirements be stored? Default: `/requirements/` at project root.
3. Is there an existing requirements database to import from? (DOORS, Jama, Polarion)

## Applicable Phases
- **Primary**: All phases — requirements are a living document from Phase A through operations.
- **Critical gates**: SRR (System Requirements Review), PDR, CDR.

## Core Philosophy

Requirements change frequently. They should be easy to read, easy to diff in git, and easy to parse by scripts if needed. We use **Markdown** with structured blocks.

## The Requirements Format

### Structure of a Requirement Block
```markdown
### [REQ-PWR-001] Minimum Battery Capacity
**Status**: DRAFT | APPROVED | VERIFIED
**Parent**: [REQ-SYS-050]
**Verification Method**: Test | Analysis | Inspection | Demonstration

The spacecraft shall have a minimum usable battery capacity of 1500 Wh at end-of-life (EOL) to support eclipse operations.

**Rationale**: Based on worst-case eclipse duration of 35 minutes and average power draw of 2 kW, with 20% margin.
```

### File Organization
Store requirements by subsystem in `/requirements/`:
- `requirements/system_level.md` — Top-level mission and system requirements
- `requirements/power_system.md` — EPS requirements
- `requirements/thermal.md` — Thermal requirements
- etc.

### ID Convention
`REQ-[SUBSYSTEM]-[NUMBER]` — e.g., `REQ-PWR-001`, `REQ-STR-003`, `REQ-SYS-100`.

## Workflows

### 1. Creating Requirements
- Ask for the subsystem and goal.
- Generate new requirement blocks with unique IDs.
- Ensure every requirement has a clear, testable "shall" statement and a rationale.

### 2. Updating Requirements
- Use search to find the relevant requirement ID.
- Update text and rationale.
- Add a `**History**:` note summarizing the change.

### 3. Traceability Audit
- Scan the requirements directory.
- Build a Parent → Child tree.
- Flag orphan requirements (no parent, unless top-level system).
- Flag requirements with no verification method.

### 4. Requirements Flowdown
When a domain skill (e.g., `thermal-assessment`) derives a requirement from analysis:
- Create the requirement in the appropriate file.
- Trace it to the parent system requirement.
- Set verification method based on how it will be confirmed.

## Writing Style
- Use "shall" for requirements, "will" for facts/declarations, "should" for goals.
- Avoid "soft" words: ~approximately, sufficient, robust, flexible~.
- Every requirement must be testable — if you can't verify it, rewrite it.
- Be concise.

## Interface
- **Reads from**: User input, existing requirements files
- **Writes to**: `/requirements/`
- **Consumed by**: All other skills (as their starting point), `v-and-v-manager` (for verification closure)
