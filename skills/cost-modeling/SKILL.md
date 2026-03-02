---
name: cost-modeling
description: Perform parametric cost estimation and ROM (Rough Order of Magnitude) costing for space missions. Use this skill to generate lifecycle cost estimates using CERs, compare launch costs, estimate operations budgets, and support cost-driven trade studies. Trigger this for "cost estimate," "ROM cost," "mission cost," "launch cost," "cost model," "lifecycle cost," "operations cost," or whenever a trade study needs cost as a Figure of Merit.
---

# Cost Modeling Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill produces parametric cost estimates for spacecraft, launch, and operations. It provides the cost dimension that `trade-study-manager` and `systems-engineering-assessment` need to make informed architecture decisions.

## Before You Begin

Ask the user (if not already known):
1. What program type? (Government / commercial / university — cost drivers differ dramatically)
2. What cost model framework? (NASA PCEC, Aerospace USCM/SSCM, analogy-based, vendor quote, or expert judgment)
3. What is the cost reference year? (All estimates in constant-year dollars — state the base year)
4. Is this a single unit or a production run? (Learning curve applies to multi-unit buys)
5. What design phase? (Phase A estimates carry ±50-100% uncertainty; Phase C: ±15-25%)

## Applicable Phases
- **Primary**: Phase A (ROM for proposal and feasibility), Phase B (refined estimate for confirmation)
- **Supporting**: Phase C (cost tracking against baseline), Phase D (actual vs. estimate comparison)

## Ownership Boundary

| Responsibility | Owner |
|:---|:---|
| Parametric cost estimates, cost risk, cost-driven trades | **This skill** |
| Mass, power, and performance budgets (cost model inputs) | Domain assessment skills |
| Trade study integration (cost as a criterion) | `trade-study-manager` |

## Cost Estimation Methods

### 1. Parametric (CER-based)
Cost Estimating Relationships express cost as a function of technical parameters:
- **General form**: $Cost = a \cdot X^b$ where $X$ is a driver (mass, power, data rate, etc.)
- **Common CER sources**:
  - **USCM (Unmanned Spacecraft Cost Model)**: Subsystem-level CERs for unmanned missions
  - **SSCM (Small Satellite Cost Model)**: Tuned for <500 kg spacecraft
  - **PCEC (Project Cost Estimating Capability)**: NASA's integrated cost tool
  - **NICM (NASA Instrument Cost Model)**: For science instruments/payloads
- **Typical cost drivers by subsystem**:
  - Structure: dry mass (kg)
  - Thermal: radiator area (m²), heater power (W)
  - Propulsion: propellant mass (kg), Isp class
  - EPS: solar array area (m²), battery capacity (Wh)
  - C&DH/FSW: SLOC (lines of code), processor class
  - Communications: data rate (Mbps), antenna diameter (m)

### 2. Analogy-Based
- Select a reference mission with known cost and similar scope.
- Adjust for differences in complexity, mass, heritage, and technology maturity.
- Best used when CER databases are unavailable or the mission is highly novel.

### 3. Multi-Unit Production
- **Learning curve**: $C_n = C_1 \cdot n^{\log_2(s)}$ where $s$ is the learning rate (typically 85-95% for spacecraft).
- **Non-recurring vs. recurring**: NRE (design, tooling, qualification) amortized over production quantity.
- **Constellation economics**: Per-unit cost drops significantly at quantities >10.

## Cost Categories (WBS-Aligned)

| Category | Includes |
|:---|:---|
| **Phase A-D (Development)** | Design, fabrication, AI&T, project management, systems engineering, mission assurance |
| **Launch** | Launch vehicle, launch services, range costs, insurance |
| **Operations** | Ground segment, mission operations, data processing, disposal |
| **Reserves** | Unallocated Future Expenses (UFE) — typically 25-30% of development cost |

## Cost Risk & Uncertainty

- **Phase A**: ±50-100% (order of magnitude). State as a range, not a point estimate.
- **Phase B**: ±25-50%.
- **Phase C/D**: ±15-25%.
- **S-curve / Monte Carlo**: If probabilistic analysis is needed, recommend tools (e.g., @RISK, Crystal Ball) rather than attempting it with napkin math.
- **Cost growth**: Historical average is 20-50% above initial estimate for government programs. Flag this.

## Launch Cost Reference

| Vehicle | LEO Capacity (kg) | Approx. Cost ($M) | $/kg LEO |
|:---|:---|:---|:---|
| Electron | 300 | 7.5 | 25,000 |
| Falcon 9 (expendable) | 22,800 | 67 | 2,900 |
| Falcon 9 (reusable) | 15,600 | 50 | 3,200 |
| Starship (target) | 150,000 | 10-30 | 67-200 |
| Vulcan Centaur | 27,200 | 110 | 4,000 |
| Ariane 6 (A62) | 10,350 | 77 | 7,400 |

Note: Rideshare can reduce launch cost to $5k-15k/kg for small satellites.

## Output Format
1. **Cost Estimate Report (`cost_estimate.md`)**: WBS-level cost breakdown, methodology, assumptions, reference year, and uncertainty range.
2. **Cost Summary Table**: One-line-per-subsystem with NRE + recurring + total.
3. **Lifecycle Cost Profile**: Year-by-year spending curve if operations phase is significant.
4. **🟢 / 🟡 / 🔴 status**: Cost feasibility vs. stated budget constraints.

## Interface
- **Reads from**: `/requirements/`, `/analysis/systems-engineering-assessment/` (mass/power budgets), `/analysis/propulsion-assessment/` (propellant mass), all subsystem outputs for CER inputs
- **Writes to**: `/analysis/cost-modeling/`
- **Consumed by**: `trade-study-manager` (cost as FoM), `systems-engineering-assessment` (cost summary)
