---
name: eclss-assessment
description: Perform Environmental Control and Life Support System (ECLSS) sizing and analysis for crewed spacecraft, stations, and habitats. Use this skill to size O₂ generation, CO₂ removal, water recovery, thermal/humidity control, and habitable volume. Trigger for "life support," "ECLSS," "crew systems," "O2 generation," "CO2 scrubbing," "water recycling," "habitable volume," "cabin pressure," or "crew consumables."
---

# ECLSS Assessment Skill (Life Support)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill sizes the Environmental Control and Life Support System for crewed vehicles and habitats. ECLSS is the system that keeps humans alive — it manages atmosphere, water, waste, and thermal comfort inside the pressurized volume.

## Before You Begin

Ask the user (if not already known):
1. **Crew size and mission duration?** (These two numbers drive everything)
2. **Open-loop or closed-loop?** (Open = consumables carried from Earth; Closed = recycling. Short missions (<2 weeks) typically open-loop; long missions require closure.)
3. **Cabin atmosphere?** (Sea-level: 101.3 kPa, 21% O₂ / 79% N₂. Low-pressure: 55.2 kPa, 34% O₂ — reduces structural mass but changes fire risk. EVA-compatible: 56.5 kPa, 34% O₂ — eliminates pre-breathe.)
4. **Gravity environment?** (Microgravity, partial-g lunar/Mars, or sustained-g rotation — affects fluid management, fire behavior, and ECLSS hardware selection.)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (consumables vs. regenerative trade), Phase B (subsystem sizing)
- **Supporting**: Phase C (detailed design), Phase D (crew qualification testing)

## Metabolic Reference Rates (Per Crew-Member Per Day)

| Parameter | Value | Notes |
|:---|:---|:---|
| O₂ consumption | 0.84 kg/day | ~840 L at STP |
| CO₂ production | 1.00 kg/day | Respiratory quotient ~0.87 |
| Metabolic water production | 0.35 kg/day | From respiration |
| Drinking water | 2.0 kg/day | Minimum; 2.5 nominal |
| Food (dry mass) | 0.6 kg/day | ~2500 kcal/day |
| Hygiene water | 1.5-4.0 kg/day | Sponge bath vs. shower |
| Solid waste | 0.11 kg/day | |
| Metabolic heat | 120 W average | 70W resting, 300W+ exercise |

Reference: NASA-STD-3001 (Crew Health), BVAD (Baseline Values and Assumptions Document).

## Analysis Domains

### 1. Atmosphere Management
- **O₂ supply**: Stored gas (high-pressure tanks), stored liquid (cryogenic), electrolysis (water → O₂ + H₂), SFOG (Solid Fuel Oxygen Generator — emergency backup).
  - Electrolysis sizing: $P_{elec} = \dot{m}_{O_2} \cdot E_{spec}$ where $E_{spec}$ ≈ 12-15 MJ/kg O₂.
- **CO₂ removal**:
  - Open-loop: LiOH canisters (~0.9 kg LiOH per kg CO₂ removed). Simple, proven, but consumable.
  - Regenerative: CDRA (Carbon Dioxide Removal Assembly) using zeolite beds — ISS heritage. Recovers CO₂ for Sabatier or venting.
  - Partial CO₂ level target: <5.3 mmHg (0.7 kPa) for long-duration; <7.6 mmHg for short missions.
- **N₂ makeup**: Leak rate replacement (~0.05-0.1 kg/day for ISS-class modules) + airlock losses.
- **Trace contaminant control**: Activated charcoal + catalytic oxidizer for VOCs, CO, NH₃.

### 2. Water Recovery
- **Open-loop**: Carry all water. Mass = crew × rate × duration. Gets prohibitive beyond ~2 weeks.
- **Closed-loop water recovery**:
  - **Urine processing**: Vapor compression distillation (VCD) or ISS WPA — ~85-93% recovery.
  - **Humidity condensate**: Collected from cabin air. ~1.0-1.5 kg/person/day.
  - **Total water recovery**: ISS achieves ~90% overall closure.
- **Water quality**: Potable (iodine or silver biocide), hygiene, technical (coolant loops — separate circuit).
- **Mass savings from closure**: $\Delta m = m_{water,daily} \times duration \times crew \times (1 - recovery\_rate)$

### 3. Thermal & Humidity Control (Cabin)
- **Cabin temperature**: 18-27°C (65-80°F) nominal range.
- **Humidity**: 25-75% relative humidity. Target 40-60% for comfort.
- **Metabolic heat removal**: $Q_{crew} = N_{crew} \times 120W$ average, plus equipment waste heat in cabin.
- **Cabin air circulation**: Minimum 0.08 m/s airflow to prevent CO₂ pockets in microgravity.
- **Interface with `thermal-assessment`**: Cabin heat rejection adds to spacecraft radiator load.

### 4. Habitable Volume
- **Minimum**: NASA-STD-3001 recommends minimum volume per crew-member based on mission duration:
  - <2 weeks: 2.5 m³/person (capsule-class)
  - 2 weeks - 6 months: 10 m³/person
  - 6 months - 3 years: 25 m³/person (quality of life, exercise, private space)
- **Functional zones**: Sleep, hygiene, galley, exercise, work, stowage, EVA airlock.
- **Noise**: <60 dBA continuous (sleep areas <50 dBA).
- **Radiation shelter**: Area where crew retreats during solar particle events — additional shielding.

### 5. Consumables vs. Regenerative Trade
The break-even for regenerative systems:
- **Regenerative equipment mass** ($m_{equip}$) + power penalty vs. **consumable mass** ($m_{cons/day} \times duration$).
- **Break-even duration**: $t_{break} = m_{equip} / m_{cons/day}$
- **Rule of thumb**: Electrolysis + CDRA + WPA pays for itself in mass beyond ~15-30 days for 4 crew.

## Output Format
1. **ECLSS Sizing Report (`eclss_report.md`)**: Subsystem sizing, consumables manifest, regeneration rates, closure percentages.
2. **Consumables Budget**: Mass per day and total mission consumables (O₂, N₂, LiOH, water, food).
3. **Power & Thermal Load**: ECLSS power demand and heat rejection to `power-assessment` and `thermal-assessment`.
4. **🟢 / 🟡 / 🔴 status**: Habitability and consumable margin status.

## Interface
- **Reads from**: `/requirements/`, `/analysis/thermal-assessment/` (cabin heat loads), `/analysis/power-assessment/` (available power for electrolysis/CDRA), `/analysis/structural-assessment/` (pressurized volume)
- **Writes to**: `/analysis/eclss-assessment/`
- **Consumed by**: `systems-engineering-assessment` (mass/power/thermal roll-up), `cost-modeling` (consumables cost), `isru-assessment` (in-situ O₂/H₂O production can supplement ECLSS)
