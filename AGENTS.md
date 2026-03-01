# AI Agent Guidelines (AGENTS.md)

Welcome, AI Coding Agent! This file provides the context, rules, and preferences you need to effectively contribute to the LunCo space-engineering-skills repository.

## Project Context
This repository houses a suite of **Space Engineering Skills**. These skills empower AI agents to perform complex space engineering tasks, from early conceptual systems engineering to detailed thermal and structural analysis.

## Core Principles
1. **Focus on the Objective**: The primary goal is to build robust, modular, and easily verifiable skills that space engineers can rely on.
2. **Standardization**: When dealing with industry data, always prefer established standards (e.g., ReqIF for requirements, XTCE for telemetry) *as an export/import layer*, while keeping the internal human-readable layer simple (Markdown/YAML).
3. **Progressive Disclosure**: Keep skill files concise. The main `SKILL.md` should be under 500 lines. Use the `scripts/` and `references/` directories to offload complexity.

## Directory Structure Rules

When creating or modifying a skill, adhere strictly to this structure:

```text
skills/
└── [skill-name]/
    ├── SKILL.md                 # Required: The core instructions and triggers
    ├── evals/                   # Required: The evaluation JSON and outputs
    │   └── evals.json           # Test cases for the skill
    ├── scripts/                 # Executable code for deterministic tasks
    ├── references/              # Context documents loaded as needed
    └── [skill-specific-data]/   # e.g., requirements/, templates/
```

**Crucial Directory Rule:** NEVER write output files to the root directory or global generic folders unless explicitly requested by the user. An agent running a skill must keep all generated artifacts contained *within* that skill's specific directory (e.g., `skills/requirements-manager/requirements/`).

## Tech Stack & Formatting
*   **Documentation**: Markdown (`.md`) is heavily preferred for documentation and human-readable data (like requirements).
*   **Data Serialization**: Use JSON or YAML. JSON is preferred for programmatic evals and configurations. YAML is preferred for human-editable configurations.
*   **Skill Creation**: When building new skills, always use the `skill-creator` methodology. Do not attempt to build a skill "from scratch" without writing evaluation prompts first.

## Workflow Rules
1. **Ask for Clarification**: Space engineering is an exact science. If an instruction regarding margins, thermal limits, orbital mechanics, or other things is ambiguous, stop and ask the user. Do not guess engineering values.
