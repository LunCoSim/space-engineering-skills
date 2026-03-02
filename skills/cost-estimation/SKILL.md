---
name: cost-estimation
description: Perform parametric cost estimation and cost-risk analysis for space missions. Use this skill to generate ROM (Rough Order of Magnitude) costs, build Cost Estimating Relationships (CERs), produce cost breakdowns by subsystem and WBS element, and generate cost S-curves for risk assessment. Trigger this for "mission cost," "cost estimate," "ROM cost," "cost model," "cost-to-complete," "budget estimate," "proposal cost," "cost breakdown," or "cost risk."
---

# Mission & System Cost Estimation Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill generates parametric cost estimates for space missions — from early ROM for proposals and investor pitches through detailed cost breakdowns for Phase A/B design reviews.

## Before You Begin

Ask the user (if not already known):
1. **What is being costed?** — Full mission (spacecraft + launch + ground + ops), spacecraft bus only, single subsystem, or a component?
2. **What cost class?** — NewSpace/commercial (lean teams, COTS parts, agile development) or Traditional/institutional (full documentation, Class B/C, government oversight)?
3. **What is the mission type?** — Earth observation, comms, science, lunar, interplanetary, constellation, crewed? (Different CERs apply)
4. **What cost year (base year)?** — All costs must be stated in a specific fiscal year (e.g., FY2026$). Apply NASA New Start Inflation Index or GDP deflator for conversion.
5. **What design phase?** — Phase A: ROM ±50%; Phase B: ±30%; Phase C: ±15%.
6. **Is there a target cost or cost cap?** — Design-to-cost constraints change the analysis approach.

## Applicable Phases
- **Primary**: Phase A (ROM for proposals, mission feasibility), Phase B (refined estimates for confirmation review)
- **Supporting**: Phase C (cost-at-completion tracking), Phase D (actuals reconciliation)

## Ownership Boundary

| Responsibility | Owner |
|:---|:---|
| Subsystem mass and complexity inputs | Domain analysis skills (`structural-assessment`, `power-assessment`, etc.) |
| System-level cost estimate, cost model selection, wraps, and risk | **This skill** |
| Trade study cost criterion scoring | `trade-study-manager` (uses cost estimates from this skill) |
| Schedule and program timeline | `schedule-risk-assessment` (when available) |

## Cost Estimation Methodology

### 1. Select the Cost Model

Choose the appropriate model based on mission type and available data:

| Model | Best For | Key Input | Notes |
|:---|:---|:---|:---|
| **USCM8/9** (Unmanned Spacecraft Cost Model) | Traditional government S/C | Dry mass by subsystem | NASA/Air Force heritage, well-calibrated |
| **SSCM** (Small Satellite Cost Model) | SmallSats < 500 kg | Total dry mass, mission type | Aerospace Corp, better for small missions |
| **NICM** (NASA Instrument Cost Model) | Science instruments/payloads | Instrument mass, type, aperture | Separate from bus cost |
| **PCEC** (Project Cost Estimating Capability) | NASA missions (full lifecycle) | WBS-level inputs | NASA's primary tool |
| **Analogy** | When a heritage mission exists | Historical actual cost + adjustments | Best when strong analog exists |
| **NewSpace Parametric** | Commercial/startup missions | Mass, TRL, team size, reuse factors | Calibrated to commercial actuals |

**Default approach**: If the user doesn't specify, use **mass-based parametric CERs** (USCM-class) with NewSpace adjustment factors where applicable.

### 2. Spacecraft Bus Cost (Hardware)

Estimate each subsystem using mass-based CERs. Generic form:

$$C_{subsystem} = a \cdot M^b \cdot F_{complexity}$$

Where:
- $M$ = subsystem mass (kg)
- $a$, $b$ = model coefficients (subsystem-specific)
- $F_{complexity}$ = complexity adjustment (1.0 = average, 0.7 = COTS/high-heritage, 1.5 = novel/custom)

#### Reference CER Coefficients (USCM-class, FY2020$K)

| Subsystem | a | b | Typical Mass Range |
|:---|:---|:---|:---|
| Structure & Mechanisms | 157 | 0.83 | 10-500 kg |
| Thermal Control | 394 | 0.635 | 2-50 kg |
| EPS (Power) | 62.7 | 1.00 | 5-200 kg |
| TT&C (Communications) | 545 | 0.761 | 2-50 kg |
| ADCS (GNC) | 464 | 0.867 | 3-100 kg |
| Propulsion | 18.4 | 0.846 | 5-500 kg |
| C&DH (Flight Computer) | 545 | 0.761 | 2-30 kg |

> ⚠️ These are **reference values only**. Actual CER coefficients vary by source database and should be stated with their provenance. Ask the user if they have project-specific CERs.

#### NewSpace Adjustment Factors

For commercial/startup missions, apply reduction factors to traditional CERs:

| Factor | Typical Range | Rationale |
|:---|:---|:---|
| COTS Hardware | 0.3 - 0.6 | Commercial off-the-shelf vs. custom flight hardware |
| Lean Team | 0.5 - 0.7 | Small agile team vs. large institutional workforce |
| Reduced Documentation | 0.7 - 0.85 | Streamlined reviews vs. full NASA/ECSS doc suite |
| Design Reuse / Heritage | 0.4 - 0.7 | Block-buy, repeat build, product line |
| **Composite NewSpace Factor** | **0.15 - 0.40** | Combined effect (multiply individual factors) |

### 3. Payload / Instrument Cost

Use NICM or analogy:
- **Optical instruments**: $C = f(\text{aperture diameter}, \text{mass}, \text{detector type})$
- **RF instruments (SAR, radiometers)**: $C = f(\text{antenna area}, \text{power}, \text{bandwidth})$
- **In-situ (spectrometers, drills)**: Best estimated by analogy to heritage instruments.

**Rule of thumb**: Payload cost is typically 20-40% of spacecraft bus cost for observatory missions, but can exceed bus cost for flagship science instruments.

### 4. Wrap Factors (Non-Hardware Costs)

Hardware cost alone dramatically underestimates total mission cost. Apply wrap factors:

| WBS Element | Typical % of Hardware Cost | Notes |
|:---|:---|:---|
| **Program Management (PM)** | 8-15% | Lower for small missions, higher for institutional |
| **Systems Engineering (SE)** | 10-18% | Includes budgets, interfaces, trade studies |
| **Mission Assurance (MA)** | 3-8% | Quality, reliability, parts screening |
| **Integration & Test (I&T)** | 10-20% | AIT campaign, environmental testing, GSE |
| **Ground Segment** | 15-30% of total | MOC, ground stations, data processing |
| **Launch Services** | Market price | SpaceX rideshare: ~$5.5K/kg; Dedicated small LV: $10-30M; Medium/Heavy: $60-150M |
| **Operations (per year)** | 5-15% of S/C cost per year | Staff, ground network fees, data processing |

### 5. Mission Lifecycle Cost

Assemble the total:

$$C_{total} = C_{payload} + C_{bus} + C_{PM/SE/MA} + C_{I\&T} + C_{launch} + C_{ground} + C_{ops} + C_{reserve}$$

**Cost Reserve (Unallocated Future Expenses — UFE)**:
- Phase A: 30-50% of estimated cost
- Phase B: 20-30%
- Phase C/D: 10-15%
- NASA policy (NPR 7120.5): minimum 25% UFE at KDP-C for Class B missions.

### 6. Cost-Risk Analysis

Go beyond point estimates:

#### Confidence Levels
- **50th percentile (P50)**: Equal chance of overrun or underrun. Typical for proposals.
- **70th percentile (P70)**: Common for budgeting. NASA often uses P70 for Class B/C.
- **80th percentile (P80)**: Conservative. Used for cost caps and flagship missions.

#### S-Curve Generation
If uncertainty distributions are available:
1. Assign triangular or log-normal distributions to each subsystem cost (min, most likely, max).
2. Run Monte Carlo simulation (1000+ draws).
3. Plot cumulative probability vs. total cost = S-curve.
4. Read off P50, P70, P80 values.

**Simplified approach (Phase A)**: Apply cost growth factors from historical data:
- Average NASA mission cost growth: 1.4× from Phase A to actuals
- Average commercial: 1.2× (smaller scope, fewer changes)

### 7. Design-to-Cost (DTC)

When the user specifies a cost cap:
1. Work backward from the cap to allocate subsystem budgets.
2. Identify which subsystems are cost-driving (typically payload, structure, and ADCS).
3. Flag if the cost cap is unrealistic given the mission requirements.
4. Recommend trades: reduce performance, increase COTS usage, descope payload, or change orbit.

## Common Reference Costs (FY2025$, approximate)

For quick sanity checks and Phase A estimates:

| Mission Class | Typical Total Cost | Examples |
|:---|:---|:---|
| 3U CubeSat (university) | $0.5-2M | Educational, tech demo |
| 6-12U CubeSat (commercial) | $2-10M | Planet Dove, Spire |
| Microsatellite (50-150 kg) | $10-50M | ICEYE SAR, BlackSky |
| Small satellite (150-500 kg) | $30-150M | RCM, WorldView Legion |
| Medium satellite (500-2000 kg) | $150-500M | Sentinel, GOES-R instruments |
| Large science mission | $500M-2B | JWST ($10B is an outlier) |
| Flagship mission | $2-10B | Mars Sample Return, Europa Clipper |
| Lunar lander (commercial) | $50-300M | CLPS landers (Astrobotic, Intuitive Machines) |
| Constellation (per sat, at scale) | $0.5-5M | Starlink, OneWeb (unit cost at volume) |

> ⚠️ **These are order-of-magnitude reference points.** Actual costs vary enormously based on complexity, heritage, and programmatic approach. Always state the basis of estimate.

## Output Format

### 1. Cost Estimate Report (`cost_estimate.md`)
A Markdown document containing:
- **Basis of Estimate (BoE)**: Model used, key assumptions, cost year, heritage reference
- **Cost Breakdown Table**: By WBS element (hardware, PM, SE, I&T, launch, ground, ops)
- **Subsystem Cost Table**: By spacecraft subsystem
- **Confidence Level**: Point estimate and stated confidence (P50/P70/P80)
- **Cost Drivers**: Top 3 cost-driving items
- **Cost-Risk Summary**: Key uncertainties and their cost impact
- **🟢 / 🟡 / 🔴 Status**: Green if within cap/target, yellow if within reserve, red if exceeds cap

### 2. Cost Comparison Table (`cost_comparison.csv`)
If multiple options are being compared (for `trade-study-manager`):
- Side-by-side cost breakdown for each option
- Lifecycle cost comparison (development + operations over mission life)

## Verification & Cross-Checks
- **Mass-cost correlation**: Verify that $/kg is within reasonable bounds for the mission class (typically $50K-500K/kg for traditional, $10K-100K/kg for NewSpace).
- **Historical analog**: If possible, compare to a known mission of similar scope.
- **Subsystem proportions**: Check that no single subsystem exceeds 40% of bus cost unless justified (e.g., a very large solar array).

## Interface
- **Reads from**: `/requirements/`, `/analysis/systems-engineering-assessment/` (mass budget), `/analysis/structural-assessment/` (mass properties), all domain skill outputs (for subsystem mass inputs)
- **Writes to**: `/analysis/cost-estimation/`
- **Consumed by**: `trade-study-manager` (cost as a Figure of Merit), `systems-engineering-assessment` (mission-level summary), project management / proposal teams
