---
name: flight-software-architect
description: Define Flight Software (FSW) architectures, C&DH hardware requirements, and modern DevOps practices for space. Use this skill to size processors, estimate memory, plan software tasking, design FDIR strategies, plan CI/CD pipelines, set up simulation-in-the-loop testing, and define automated test infrastructure. Trigger this for "FSW architecture," "processor sizing," "data handling," "FDIR design," "onboard computer selection," "CI/CD for flight software," "simulation in the loop," "DevOps for space," or "automated FSW testing."
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

### 4. DevOps & CI/CD for Flight Software
Modern FSW development uses continuous integration to catch defects early and enable rapid iteration:
- **Version control**: Git-based workflow with branch protection, code review, and signed commits.
- **CI pipeline stages**:
  1. Static analysis (MISRA-C, cppcheck, Coverity) — run on every commit.
  2. Unit tests — component-level tests with code coverage tracking (target >80%).
  3. Integration tests — inter-module tests with mocked hardware interfaces.
  4. Software-in-the-loop (SIL) — full FSW on host PC with simulated plant models.
  5. Processor-in-the-loop (PIL) — FSW compiled for target CPU, run on eval board or emulator.
  6. Hardware-in-the-loop (HIL) — FSW on flight-like hardware with simulated sensors/actuators.
- **Artifact management**: Versioned build artifacts, traceable to requirements and test results.
- **Deployment**: Over-the-air (OTA) update capability for on-orbit patches — requires safe rollback mechanism.
- **Standards**: NASA-STD-8739.8 (SW Assurance), ECSS-Q-ST-80C, DO-178C (if human-rated).

### 5. Simulation-in-the-Loop Testing
Simulation environments validate FSW behavior before integration with real hardware:
- **SIL (Software-in-the-Loop)**: FSW runs on development host. Plant dynamics, sensors, and actuators are mathematical models. Fastest iteration cycle — run thousands of scenarios per night.
- **PIL (Processor-in-the-Loop)**: Cross-compiled FSW runs on target processor (or cycle-accurate emulator). Validates timing, memory layout, and compiler behavior.
- **HIL (Hardware-in-the-Loop)**: FSW runs on flight-representative OBC. Real sensor interfaces with signal injection. Most representative but slowest and most expensive.
- **Digital twin**: Persistent simulation that mirrors on-orbit state. Used for anomaly investigation, rehearsal, and predictive maintenance.
- **Scenario libraries**: Build libraries of nominal, off-nominal, and edge-case scenarios that run in CI — regression testing for every commit.

### 6. Automated Test Infrastructure
- **FlatSat / Engineering Development Unit (EDU)**: Bench-level integration of flight-representative hardware — used for continuous HIL testing.
- **Test automation framework**: Scripted test execution with automatic pass/fail evaluation. Reduce manual test from weeks to hours.
- **Test coverage matrix**: Map requirements → test cases → test results. Automated gap reporting.
- **Regression suite**: Library of tests that runs on every build to catch unintended side effects.
- **Formal verification**: For safety-critical modules (FDIR state machines, command validators), consider model checking or formal proof tools (SPARK/Ada, TLA+, CBMC).

## Output Format
1. **FSW Architecture Document (`fsw_architecture.md`)**: Software layers, tasking model, FDIR strategy, boot sequence.
2. **Computational Budget (`computing_budget.md`)**: Table of CPU utilization, RAM/Flash usage, and bus bandwidth per function.
3. **OBC Trade**: If multiple options were considered, include the trade rationale.

## Interface
- **Reads from**: `/requirements/`, `/analysis/gnc-assessment/` (processing needs), `/analysis/communications-assessment/` (data handling rates), `/analysis/reliability-assessment/` (radiation environment)
- **Writes to**: `/analysis/flight-software-architect/`
- **Consumed by**: `systems-engineering-assessment` (power/mass for OBC), `ait-manager` (FlatSat/EGSE planning)
