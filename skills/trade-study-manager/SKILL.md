---
name: trade-study-manager
description: Conduct architecture-level trade studies and decision analysis. Use this skill to build trade matrices, define Figures of Merit, compare mission architectures, and document design decisions. Trigger for "trade study," "trade matrix," "Pugh matrix," "architecture selection," "Figure of Merit," or "design decision."
---

# Trade Study Manager Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill supports the decision-making that happens *before* detailed analysis — choosing between mission architectures, subsystem approaches, or technology options using structured trade methodology.

## Before You Begin

Ask the user (if not already known):
1. What decision needs to be made? (orbit selection, propulsion type, power source, payload selection, launch vehicle, constellation vs single, etc.)
2. What are the candidate options? (at least 2, ideally 3-5)
3. Who are the stakeholders? (engineering, science, management, cost — each may weight criteria differently)
4. What is the decision deadline / review gate? (SRR, PDR, mission proposal)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (architecture selection — this is when most trades happen)
- **Supporting**: Phase B (detailed subsystem trades), Phase C (change impact assessment)

## Trade Study Methodology

### 1. Define the Trade Space
- List candidate options with a brief description of each.
- Define the "do nothing" or "baseline" option if applicable.
- Eliminate clearly infeasible options early (with documented rationale).

### 2. Define Evaluation Criteria (Figures of Merit)
Common criteria for space missions:
- **Performance**: Resolution, data rate, coverage, revisit time
- **Mass**: Total system mass (lighter = more launch margin or smaller LV)
- **Power**: Total power demand vs. available
- **Cost**: Development cost, operations cost, launch cost
- **Risk**: Technical maturity (TRL), schedule risk, supply chain
- **Heritage**: Flight-proven vs. novel
- **Operability**: Ground station needs, autonomous capability, complexity
- **Lifetime**: Degradation, consumables, reliability

### 3. Weight the Criteria
- Use pairwise comparison (AHP) or direct assignment.
- Weights must sum to 1.0 (or 100%).
- **Document the rationale** for weighting — this is often the most debated step.

### 4. Score the Options
- Score each option against each criterion (typically 1-5 or 1-10 scale).
- **Scoring rules**: Define what each score means before scoring (e.g., "5 = exceeds requirement with margin, 3 = meets requirement, 1 = does not meet").
- Score independently before group discussion to avoid groupthink.

### 5. Calculate Weighted Scores
$S_{option} = \sum_{i} w_i \times s_i$

### 6. Sensitivity Analysis
- Vary the weights ±20% and see if the winner changes.
- Identify which criteria are "swing factors" — small weight changes flip the result.
- If the trade is sensitive, the decision needs more analysis before commitment.

## Trade Matrix Format

| Criterion | Weight | Option A | Score | Option B | Score | Option C | Score |
|:---|:---|:---|:---|:---|:---|:---|:---|
| Performance | 0.30 | Excellent | 5 | Good | 3 | Good | 3 |
| Mass | 0.20 | 85 kg | 4 | 120 kg | 2 | 95 kg | 3 |
| Cost | 0.25 | $15M | 3 | $8M | 5 | $12M | 4 |
| Risk (TRL) | 0.15 | TRL 6 | 3 | TRL 9 | 5 | TRL 4 | 2 |
| Heritage | 0.10 | Partial | 3 | Full | 5 | None | 1 |
| **Weighted Total** | — | — | **3.70** | — | **3.70** | — | **2.85** |

## Common Space Mission Trades

Reference trades that come up repeatedly:
- **Orbit**: LEO (low latency, high drag) vs. MEO vs. GEO (large coverage, high delta-v, radiation)
- **Propulsion**: Chemical (simple, high thrust) vs. Electric (efficient, slow) vs. Cold gas (simple, low performance)
- **Power**: Solar (common, distance-limited) vs. RTG (deep space, regulatory) vs. Battery-only (short missions)
- **Constellation**: Single high-capability vs. distributed lower-capability (coverage vs. cost)
- **Launch vehicle**: Rideshare (cheap, constrained orbit) vs. Dedicated (expensive, flexible)

## Output Format
1. **Trade Study Report (`trade_report.md`)**: Decision question, options, criteria, weights, scoring matrix, sensitivity analysis, and recommendation with rationale.
2. **Decision Record**: One-paragraph summary capturing: "We chose [Option X] because [key reasons], accepting [trade-offs]."

## Interface
- **Reads from**: `/requirements/` (mission objectives and constraints), any existing `/analysis/` outputs that inform the trade
- **Writes to**: `/analysis/trade-study-manager/`
- **Consumed by**: All downstream skills (the trade decision shapes the entire mission architecture)
