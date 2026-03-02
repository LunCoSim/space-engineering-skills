---
name: isru-assessment
description: Perform In-Situ Resource Utilization (ISRU) sizing and feasibility analysis. Use this skill to evaluate resource extraction from planetary regolith, propellant production from local materials, water ice mining, and closed-loop resource economics. Trigger for "ISRU," "in-situ resources," "regolith processing," "propellant production," "water ice extraction," "lunar mining," "Mars ISRU," "Sabatier reactor," or "oxygen from regolith."
---

# ISRU Assessment Skill (In-Situ Resource Utilization)

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill evaluates whether local resources at the destination (Moon, Mars, asteroids) can be extracted and processed to reduce launched mass from Earth. ISRU is the key enabler for sustainable beyond-Earth operations — every kilogram produced in-situ is a kilogram you don't launch.

## Before You Begin

Ask the user (if not already known):
1. **Destination body?** (Moon, Mars, asteroid, other — determines available resources and environment)
2. **Target product?** (Oxygen, water, propellant LOX/LH2, metals, building materials, breathing air)
3. **Required production rate?** (kg/day or tonnes/year — drives plant sizing)
4. **Resource characterization confidence?** (Proven reserves vs. orbital remote sensing — huge uncertainty factor)
5. What design phase?

## Applicable Phases
- **Primary**: Phase A (ISRU vs. Earth-supply trade), Phase B (process selection and plant sizing)
- **Supporting**: Phase C (detailed process engineering), Phase D (in-situ demonstration planning)

## Resource Availability by Body

### Moon
| Resource | Source | Extraction Method | Confidence |
|:---|:---|:---|:---|
| Oxygen | Regolith (40-45% O by mass in oxides) | Hydrogen reduction, carbothermal, molten salt electrolysis | High (Apollo samples) |
| Water ice | Permanently shadowed craters (PSRs) | Excavation + sublimation/heating | Medium (LCROSS, M³, Chandrayaan) |
| Iron, Titanium | Ilmenite (FeTiO₃) in mare regolith | Beneficiation + reduction | High |
| Helium-3 | Solar wind implanted in regolith | Heating to 700°C, very low concentration (~20 ppb) | Low economic viability |
| Silicon | Silicate minerals | Reduction (high energy) | High availability, low TRL for processing |

### Mars
| Resource | Source | Extraction Method | Confidence |
|:---|:---|:---|:---|
| O₂ + CO | CO₂ atmosphere (95.3%) | MOXIE (solid oxide electrolysis): $2CO_2 \rightarrow 2CO + O_2$ | High (demonstrated by Perseverance) |
| CH₄ + O₂ | CO₂ + H₂O (subsurface ice) | Sabatier: $CO_2 + 4H_2 \rightarrow CH_4 + 2H_2O$ then electrolysis | Medium |
| Water | Subsurface ice, hydrated minerals | Excavation + heating, or microwave extraction | Medium-High (SHARAD radar) |
| Iron, aluminum | Regolith minerals | Thermite or electrolytic reduction | Low TRL |

### Asteroids
- C-type: 10-20% water, organics, clays
- S-type: Metals (Fe, Ni), silicates
- M-type: High metal content (Fe, Ni, possibly PGMs)
- Extraction in microgravity is a largely unsolved engineering problem.

## Analysis Workflows

### 1. ISRU vs. Earth Supply Trade
The fundamental trade:
- **Launched cost**: $C_{launch} = m_{product} \times \$/kg_{to\_destination}$
  - Earth to Moon surface: ~$100k-500k/kg (current), ~$10k-50k/kg (target, Starship-class)
  - Earth to Mars surface: ~$500k-2M/kg (current estimates)
- **ISRU cost**: $C_{ISRU} = C_{plant} + C_{power} + C_{ops}$ amortized over production lifetime.
- **Break-even**: ISRU wins when $C_{ISRU}/m_{produced} < C_{launch}$ over mission lifetime.

### 2. Process Sizing
For each ISRU process:
- **Feedstock rate**: Mass of raw material per mass of product (stoichiometric + losses).
- **Energy requirement**: $E_{spec}$ (kWh/kg product) — this is usually the dominant constraint.
  - Lunar O₂ from ilmenite reduction: ~4-8 kWh/kg O₂
  - Mars MOXIE-class electrolysis: ~5-10 kWh/kg O₂
  - Water electrolysis: ~5 kWh/kg O₂ (plus ~0.6 kWh/kg H₂ co-product)
  - Sabatier: ~2 kWh/kg CH₄ (exothermic, but needs thermal management)
- **Plant mass**: Typically 50-200 kg per kg/day production rate (highly dependent on TRL).
- **Ancillary systems**: Excavation/collection, beneficiation, storage, distribution.

### 3. Power Requirements
ISRU is energy-intensive. Size the dedicated power supply:
- **Solar**: Available on lunar dayside (~1361 W/m²) and Mars (~589 W/m²). Not available during lunar night (14 days).
- **Nuclear (Kilopower/fission)**: 1-10 kW per unit. Enables continuous operation, night-time processing, and PSR operations. Strongly recommended for any large-scale ISRU.
- **Interface with `power-assessment`**: ISRU power is additive to spacecraft/habitat loads — flag total demand.

### 4. Storage & Distribution
- **Cryogenic storage**: LOX, LH₂, LCH₄ — boil-off management is critical.
  - Zero Boil-Off (ZBO): Active cryo-coolers, ~15-20 W/kg stored per day.
  - Passive: MLI + low-conductivity supports. Boil-off rate 0.1-1%/day depending on scale and insulation.
- **Gas storage**: High-pressure composite tanks for O₂, N₂ breathing atmosphere.
- **Propellant transfer**: Pumped or pressure-fed transfer to vehicles. Interface definition needed.

## Output Format
1. **ISRU Feasibility Report (`isru_report.md`)**: Available resources, process selection, plant sizing, energy requirements, and break-even analysis.
2. **Production Budget**: Mass/energy/time per unit product.
3. **Infrastructure Requirements**: Power plant, excavation equipment, storage, and distribution.
4. **🟢 / 🟡 / 🔴 status**: Feasibility and TRL assessment per process.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (destination, surface conditions), `/analysis/power-assessment/` (available power for ISRU), `/analysis/propulsion-assessment/` (propellant demand)
- **Writes to**: `/analysis/isru-assessment/`
- **Consumed by**: `propulsion-assessment` (in-situ propellant availability), `eclss-assessment` (in-situ O₂/H₂O), `cost-modeling` (ISRU economics vs. launch), `systems-engineering-assessment` (infrastructure mass/power)
