---
name: flight-software-architect
description: Define Flight Software (FSW) architectures and C&DH hardware requirements. Use this skill to size processors, estimate memory, plan software tasking, and design FDIR strategies. Trigger this for "FSW architecture," "processor sizing," "data handling," "FDIR design," or "onboard computer selection."
---

# Flight Software & C&DH Architect Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill defines the computational brain of the spacecraft — the software architecture and Command & Data Handling (C&DH) hardware that manage all onboard logic and data.

## Before You Begin

Ask the user (if not already known):
1. What is the radiation environment? (LEO is mild, GEO/interplanetary is harsh — drives rad-hard vs COTS decision)
2. What is the mission-critical autonomy level? (ground-controlled vs autonomous — deep space missions need more autonomy due to comms delay)
3. Is there heritage from a previous mission? (drives OBC and framework selection)
4. What standards framework? (NASA NPR 7150.2 for FSW, ECSS-E-ST-40C, or DO-178C for human-rated)
5. What design phase?

## Applicable Phases
- **Primary**: Phase B (architecture selection), Phase C (detailed design and coding)
- **Supporting**: Phase A (feasibility), Phase D (integration and test)

## Design Domains

### 1. C&DH Hardware Sizing
- **Processor selection**: Estimate MIPS/FLOPS based on GNC loop rate, payload processing, and housekeeping tasks.
  - **Rad-hard**: RAD750 (~400 MIPS, $200k+), GR740 (LEON4, ~800 MIPS), Vorago VA41630
  - **COTS with shielding**: ARM Cortex, Zynq FPGA — viable for LEO with latch-up protection
  - **Ask user**: Is radiation tolerance driven by requirements or by a blanket policy?
- **Memory**: RAM (working), Flash (code + data storage), EEPROM (critical boot parameters).
- **Interfaces**: SpaceWire, CAN, I2C, RS-422, LVDS — define bus loading and redundancy.

### 2. Software Architecture
- **Frameworks**:
  - NASA cFS (core Flight System) — open source, flight-proven, modular
  - JPL F Prime — lightweight, component-based, good for small missions
  - Custom RTOS (VxWorks, RTEMS, FreeRTOS) — when heritage or constraints dictate
- **Layered architecture**: Boot → OS/RTOS → Middleware (cFS/F') → Application layer (GNC, Payload, Comm)
- **Autonomy / FDIR**: Define Fault Detection, Isolation, and Recovery levels:
  - Level 0: Hardware autonomous (e.g., watchdog timer reset)
  - Level 1: Software autonomous (e.g., switch to redundant unit)
  - Level 2: Ground-commanded recovery

### 3. Telemetry & Command
- **Packet definitions**: CCSDS standard (APIDs, packet headers, CRC)
- **Housekeeping**: Define critical vs. diagnostic telemetry rates
- **CPU utilization budget**: Nominal vs. peak (e.g., imaging + downlink + GNC simultaneously)
- **Boot sequence**: Cold boot, warm boot, safe-mode boot — define expected behavior and duration

## Output Format
1. **FSW Architecture Document (`fsw_architecture.md`)**: Software layers, tasking model, FDIR strategy, boot sequence.
2. **Computational Budget (`computing_budget.md`)**: Table of CPU utilization, RAM/Flash usage, and bus bandwidth per function.
3. **OBC Trade**: If multiple options were considered, include the trade rationale.

## Interface
- **Reads from**: `/requirements/`, `/analysis/gnc-assessment/` (processing needs), `/analysis/communications-assessment/` (data handling rates), `/analysis/reliability-assessment/` (radiation environment)
- **Writes to**: `/analysis/flight-software-architect/`
- **Consumed by**: `systems-engineering-assessment` (power/mass for OBC), `ait-manager` (FlatSat/EGSE planning)
