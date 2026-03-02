---
name: reusability-analysis
description: Analyze reusability of launch vehicles and spacecraft systems. Use this skill to size recovery hardware, estimate refurbishment costs, model reuse degradation, and calculate flight-rate economics. Trigger for "reusability," "landing propellant," "recovery system," "refurbishment," "turnaround time," "flight rate economics," "booster recovery," or "reuse degradation."
---

# Reusability Analysis Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill evaluates whether reusing a vehicle element is economically and technically viable. Reusability is the single largest cost lever in spaceflight — but only if the recovery penalties and refurbishment costs don't erase the savings.

## Before You Begin

Ask the user (if not already known):
1. **What element is being reused?** (First stage booster, upper stage, fairing, spacecraft, capsule)
2. **Recovery method?** (Propulsive landing, parachute + ocean recovery, parachute + land, glide-back, mid-air capture)
3. **Target reuse count?** (1x = expendable baseline, 10x = Falcon 9 class, 100x+ = Starship ambition)
4. **Flight rate?** (Launches per year — amortization only works above a minimum cadence)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (reusability vs. expendable trade), Phase B (recovery system preliminary design)
- **Supporting**: Phase C (detailed refurbishment planning), Phase D (operational reuse tracking)

## Analysis Domains

### 1. Recovery System Sizing

#### Propulsive Landing
- **Landing propellant reserve**: Typically 10-15% of stage propellant for boostback + entry burn + landing burn.
- **Performance penalty**: Payload reduction = $\Delta m_{payload} \approx m_{prop,landing} \cdot (1 + k_{structural})$ where $k_{structural}$ accounts for landing legs, grid fins, and additional avionics.
- **Reference**: Falcon 9 loses ~30-40% payload to LEO for RTLS, ~15-20% for ASDS (downrange landing).
- **Landing hardware mass**: Legs (1-3% of stage dry mass), grid fins (0.5-1%), additional avionics and sensors.

#### Parachute / Splashdown
- **Parachute system mass**: Typically 1-3% of recovered mass.
- **Salt water exposure**: Drives refurbishment scope — engines, avionics, and structures all affected.
- **Recovery fleet**: Ship + crane + logistics per recovery (operational cost).

#### Fairing Recovery
- **Parafoil + GPS guidance**: Fairing halves, ~1000 kg each.
- **Economic value**: Each fairing costs $5-6M; recovery saves ~$3-4M per flight after capture costs.

### 2. Reuse Degradation Modeling
Components degrade with each flight cycle:

| System | Degradation Driver | Typical Limit | Inspection |
|:---|:---|:---|:---|
| Engines | Turbopump wear, chamber erosion, injector coking | 10-100+ flights (depends on engine) | Borescope, flow testing |
| Structures | Fatigue (launch + landing loads), thermal cycling | Fatigue life analysis per MIL-STD | NDT (UT, X-ray) |
| Thermal Protection | Ablation, tile damage, heat shield erosion | Per-flight mass loss tracking | Visual + thickness gauge |
| Avionics | Vibration fatigue, connector wear | Typically not the limiter | Functional test |
| Tanks | Pressure cycling, cryo-cycling fatigue | COPV cycle life (design dependent) | Proof test, acoustic emission |

### 3. Refurbishment Cost Model
- **Level 0 — Inspect & Fly**: Visual inspection, functional test, propellant reload. (Target: <5% of new-build cost)
- **Level 1 — Minor Refurb**: Replace consumables (igniters, seals, pyros), touch-up coatings. (Target: 5-15%)
- **Level 2 — Major Refurb**: Engine teardown/rebuild, structural repair, avionics replacement. (Target: 15-40%)
- **Level 3 — Overhaul**: Comparable to new build — if you're here regularly, reuse isn't saving money.
- **Turnaround time**: Days (inspect & fly) to months (major refurb). This directly limits flight rate.

### 4. Flight-Rate Economics
The break-even calculation:
- **Expendable cost per flight**: $C_{exp}$
- **Reusable cost per flight**: $C_{reuse} = (C_{vehicle} / N_{flights}) + C_{refurb} + C_{recovery} + C_{ops}$
- **Break-even flight count**: $N_{break} = C_{vehicle} / (C_{exp} - C_{refurb} - C_{recovery} - C_{ops})$
- **Payload penalty cost**: If reuse reduces payload, some missions need a larger (more expensive) vehicle — factor this in.

Key insight: Reuse only wins if $N_{flights}$ exceeds $N_{break}$ AND the flight rate is high enough to amortize fixed costs (facilities, recovery fleet, refurb workforce).

### 5. Design-for-Reuse Checklist
- [ ] Landing load cases added to structural design
- [ ] Engine designed for multiple ignition cycles (bearing life, seal life)
- [ ] TPS designed for multi-flight thermal cycling
- [ ] Avionics designed for rapid functional test (automatable)
- [ ] Propellant system designed for rapid drain/purge/reload
- [ ] Access panels for inspection without extensive disassembly
- [ ] Data recording (flight loads, temperatures) to support health monitoring

## Output Format
1. **Reusability Trade Report (`reusability_report.md`)**: Expendable vs. reusable comparison with performance penalty, break-even analysis, and economic model.
2. **Recovery System Summary**: Hardware mass, propellant reserves, and mission impact.
3. **Refurbishment Plan**: Per-flight and periodic maintenance requirements.
4. **🟢 / 🟡 / 🔴 status**: Economic viability at projected flight rate.

## Interface
- **Reads from**: `/requirements/`, `/analysis/propulsion-assessment/` (engine specs, propellant), `/analysis/structural-assessment/` (fatigue life), `/analysis/cost-modeling/` (expendable baseline cost)
- **Writes to**: `/analysis/reusability-analysis/`
- **Consumed by**: `cost-modeling` (reuse economics), `trade-study-manager` (reuse as architecture option), `systems-engineering-assessment` (mass/performance impact)
