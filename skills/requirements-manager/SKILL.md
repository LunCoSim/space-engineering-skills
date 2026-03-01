---
name: requirements-manager
description: Manage space engineering requirements in a human-readable, trackable format. Use this skill whenever the user wants to define, update, trace, or review system requirements, component specifications, mass/power budgets, or verification criteria.
---

# Requirements Management Skill

This skill defines how to manage engineering requirements for the LunCo space engineering platform. We prioritize human readability and simple version control over complex XML standards. 

## Core Philosophy
Requirements change frequently. They should be easy to read, easy to diff in git, and easy to parse by scripts if needed. We use a combination of **Markdown** for tracking high-level intent and **YAML frontmatter** (or pure YAML) for structured data mapping.

## The Requirements Format

By default, every requirement or logical group of requirements should be stored in the `/requirements/` directory at the root of the project. **If the user specifies a different path in their prompt (e.g., for automated testing or sandboxing), you must use that path instead.**

### Structure of a Requirement File
Create individual Markdown files for subsystems or components (e.g., `requirements/power_system.md`). 

Each specific requirement follows this block structure:

```markdown
### [REQ-PWR-001] Minimum Battery Capacity
**Status**: DRAFT | APPROVED | VERIFIED
**Parent**: [REQ-SYS-050]
**Verification Method**: Test | Analysis | Inspection | Demonstration

The spacecraft shall have a minimum usable battery capacity of 1500 Wh at end-of-life (EOL) to support eclipse operations.

**Rationale**: Based on the worst-case eclipse duration of 35 minutes and an average power draw of 2kW, with a 20% margin.
```

### When Tracing Requirements
When tracing a child requirement to a parent, always include the parent's ID in the **Parent** field. If tracing down to components, refer to specific component IDs if they exist.

## Typical Workflows

### 1. Creating New Requirements
- Ask the user for the subsystem and the goal.
- Generate new requirement blocks with unique, logical IDs (e.g., `REQ-[SUBSYSTEM]-[NUMBER]`).
- Ensure every requirement has a clear, testable "shall" statement and a rationale.

### 2. Updating Requirements
- When asked to update a parameter (e.g., "Change the margin to 30%"), use codebase search to find the relevant requirement ID.
- Update the text and the rationale to reflect the change.
- Optionally add a `**History**:` note to the block summarizing the change.

### 3. Traceability Audit
- If the user asks for a traceability check, scan the `/requirements/` directory (or the custom directory specified by the user).
- Build a tree of Parent -> Child relationships and flag any "Orphan" requirements (requirements without parents, unless they are top-level system requirements).

## Writing Style
- Be concise. 
- Ensure requirements are unambiguous and verifiable. 
- Avoid "soft" words like *approximately, sufficient, robust, or flexible*.
- Use "shall" for requirements, "will" for facts/declarations, and "should" for best-effort goals.
