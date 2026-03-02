---
name: manufacturing-assessment
description: Assess spacecraft manufacturability, production planning, and fabrication trades. Use this skill for Design for Manufacturing (DFM), Design for Assembly (DFA), make-vs-buy decisions, production rate analysis, and vertical integration trades. Trigger for "manufacturability," "DFM," "DFA," "production rate," "make or buy," "fabrication," "assembly sequence," or "vertical integration."
---

# Manufacturing & Producibility Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill evaluates whether a design can be built efficiently, at the required rate, and at acceptable cost. It bridges the gap between paper designs and factory floor reality — the discipline that separates companies that ship from companies that present.

## Before You Begin

Ask the user (if not already known):
1. **Production quantity?** (One-off prototype, small batch 2-10, constellation 50+, mega-constellation 1000+)
2. **Facility constraints?** (Cleanroom class, crane capacity, chamber sizes, geographic location)
3. **Target production rate?** (Units per month/year — this drives tooling and staffing decisions)
4. **Heritage processes?** (Are there existing manufacturing lines or is this greenfield?)
5. What design phase? (Phase A: feasibility; Phase C: detailed manufacturing planning)

## Applicable Phases
- **Primary**: Phase B (preliminary manufacturing assessment), Phase C (detailed process planning)
- **Supporting**: Phase A (feasibility screening), Phase D (production execution and yield tracking)

## Analysis Domains

### 1. Design for Manufacturing (DFM)
Evaluate whether the design can be practically fabricated:
- **Material selection**: Availability, lead time, machinability, weldability, outgassing compliance.
- **Tolerances**: Flag tolerances tighter than standard CNC capability (±0.05 mm) — each tighter step increases cost exponentially.
- **Geometry**: Identify features requiring 5-axis machining, EDM, or custom tooling.
- **Joining methods**: Welding (FSW, TIG, EB), bonding (structural adhesives), fasteners — each has mass, cost, and inspection implications.
- **Additive manufacturing**: Identify candidates for metal 3D printing (topology-optimized brackets, complex internal channels). Note: AM parts typically require HIP and CT inspection.
- **Surface treatments**: Anodizing, conversion coating, plating, painting — compatibility with thermal and outgassing requirements.

### 2. Design for Assembly (DFA)
Evaluate whether the design can be efficiently integrated:
- **Part count**: Fewer parts = fewer interfaces = fewer failure modes. Target part count reduction where possible.
- **Assembly sequence**: Define the order-of-build. Identify steps requiring special tooling, fixtures, or cleanroom access.
- **Access**: Verify that fasteners, connectors, and test points are accessible in the assembled configuration. Late discoveries here cause costly redesigns.
- **Alignment**: Optical, antenna, and sensor alignment requirements — drives fixture precision and verification methods.
- **Cable routing**: Harness routing, connector mate/de-mate cycles, EMI shielding requirements.

### 3. Make-vs-Buy Trade
For each major component or subsystem:

| Factor | Make (In-House) | Buy (Vendor) |
|:---|:---|:---|
| **Control** | Full design authority | Specification-driven |
| **Lead time** | Depends on capacity | Vendor-dependent |
| **Cost** | High NRE, low recurring | Low NRE, higher unit cost |
| **Quality** | Direct oversight | Incoming inspection needed |
| **IP** | Retained | Shared or lost |
| **Risk** | Schedule risk if capacity-limited | Supply chain risk |

Decision guideline: Make mission-critical / novel items in-house. Buy commodity items (fasteners, connectors, standard electronics) from qualified vendors.

### 4. Production Rate Analysis
For multi-unit production:
- **Takt time**: Required time per unit = available production hours / required quantity.
- **Bottleneck identification**: Which process step has the longest cycle time? (Typically AI&T, not fabrication)
- **Parallel paths**: Can subassemblies be built simultaneously? Map the critical path.
- **Tooling**: Soft tooling (prototype, <10 units) vs. hard tooling (production, 50+ units).
- **Staffing**: Skilled labor requirements — technicians, inspectors, test engineers per shift.

### 5. Quality & Inspection
- **Inspection points**: Define in-process inspection (dimensional, visual, NDT) and acceptance test flow.
- **Non-destructive testing**: X-ray, ultrasonic, dye penetrant, CT scan — specify per joint/weld type.
- **Statistical process control**: For production runs, define control limits and sampling plans.
- **Non-conformance**: Process for material review board (MRB) disposition — use-as-is, rework, scrap.

## Output Format
1. **Manufacturing Assessment Report (`manufacturing_report.md`)**: DFM/DFA findings, risk items, make-vs-buy recommendations.
2. **Assembly Sequence**: Step-by-step build plan with tooling and cleanroom requirements.
3. **Production Plan** (if multi-unit): Takt time, bottleneck analysis, tooling requirements.
4. **🟢 / 🟡 / 🔴 status**: Manufacturability feasibility per subsystem.

## Interface
- **Reads from**: `/requirements/`, `/analysis/structural-assessment/` (materials, geometry), `/analysis/systems-engineering-assessment/` (configuration), `/analysis/cost-modeling/` (production cost targets)
- **Writes to**: `/analysis/manufacturing-assessment/`
- **Consumed by**: `cost-modeling` (production cost inputs), `ait-manager` (AI&T sequence feeds from assembly sequence), `systems-engineering-assessment` (schedule/risk)
