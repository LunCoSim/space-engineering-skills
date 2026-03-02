# Space Engineering Skills — Shared Conventions

All skills in this collection follow these conventions. Read this file before executing any skill.

## 1. Clarification-First Rule

Before producing any analysis, you MUST ask the user:

1. **Mission type** — Earth observation, lunar surface, deep space, constellation, space station, launch vehicle, etc.
2. **Target body** — Earth orbit (LEO/MEO/GEO/HEO), Moon, Mars, asteroid, comet, outer planets, Sun-Earth L-points.
3. **Standards framework** — NASA (NPR 7120.5, GEVS), ESA (ECSS), JAXA, CSA, or project-specific. If the user doesn't know, default to NASA and note it.
4. **Design phase** — Phase A (concept), Phase B (preliminary), Phase C (detailed), Phase D (fabrication/test). This determines the level of detail and margin policy.

If the user has already answered these questions (e.g., in a previous skill output or requirements file), do not re-ask — pull the answers from the existing project context.

## 2. Data Flow Protocol

### File Locations
- **Requirements** are read from `/requirements/` (managed by `requirements-manager`).
- **Analysis outputs** are written to `/analysis/[skill-name]/` (e.g., `/analysis/thermal-assessment/thermal_model.md`).
- **Each skill reads** from `/requirements/` and from `/analysis/[dependency-skill-name]/` for upstream data.

### Avoid Duplication
Before generating any budget or calculation, check if a domain skill has already produced the authoritative output:
- Power budgets → `power-assessment` owns the detail, `systems-engineering-assessment` integrates the summary
- Link budgets → `communications-assessment` owns the detail
- Thermal sizing → `thermal-assessment` owns the detail
- Delta-V budgets → `mission-analysis-specialist` owns the budget, `propulsion-assessment` sizes the hardware

### Multi-Agent Handoff
When a skill needs input from another skill that hasn't run yet:
1. State what data is needed (e.g., "Eclipse duration from `mission-analysis-specialist`").
2. Use a reasonable placeholder assumption and mark it clearly: **[ASSUMPTION — pending MAS output]**.
3. Note the dependency in the output so the integrating engineer knows to re-run after the upstream skill completes.

## 3. Phase-Gate Mapping

| Phase | Primary Skills | Supporting Skills |
|:---|:---|:---|
| **A — Concept** | `trade-study-manager`, `systems-engineering-assessment`, `requirements-manager`, `mission-analysis-specialist` | All others for first-order estimates |
| **B — Preliminary** | All analysis skills, `orbital-conops-manager` / `lunar-conops-manager`, `hazard-analysis` | `ait-manager`, `v-and-v-manager` |
| **C — Detailed** | All analysis skills (refined), `flight-software-architect`, `reliability-assessment`, `v-and-v-manager` | `ait-manager` |
| **D — Fabrication/Test** | `ait-manager`, `mission-operations-manager`, `v-and-v-manager` | Analysis skills for anomaly resolution |

## 4. Conflict & Escalation

When a skill detects an inconsistency (e.g., mass budget exceeds launch capacity, power budget shows negative margin):
1. **Flag it** with a ⚠️ warning in the output, stating the conflicting values and their sources.
2. **Do not silently resolve it** — the human engineer makes the decision.
3. **Suggest** running `systems-engineering-assessment` to re-integrate the affected budgets.

## 5. Output Format Rules

- **Markdown** — all outputs are human-readable Markdown files.
- **Tables** for budgets, compliance matrices, and trade results.
- **Rationale** — every assumption must have a one-line rationale.
- **Traffic Light** — use 🟢 / 🟡 / 🔴 for budget status where applicable.
- **SI units** exclusively unless the user specifies otherwise (e.g., imperial for NASA legacy systems).
- **Margins** — always state the assumed margin percentage and the design phase it corresponds to.

## 6. Fidelity Level

These skills target **low-fidelity, Phase A "napkin math"** analysis by default. This means:
- Closed-form analytical equations over numerical simulation.
- Conservative margins (20-30% mass, 25% power, 100% delta-v for station keeping).
- Order-of-magnitude estimates are acceptable and should be flagged as such.
- If the user needs higher fidelity, the skill should recommend appropriate tools (e.g., STK, GMAT, Thermal Desktop, NASTRAN) rather than attempting to replicate them.
