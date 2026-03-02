---
name: flight-software-architect
description: Define Flight Software (FSW) architectures and C&DH hardware requirements. Use this skill to size processors, estimate memory, and plan software tasking. Trigger this for "FSW architecture," "processor sizing," or "data handling."
---

# Flight Software & C&DH Architect Skill

This skill defines the "computational brain" of the spacecraft, ensuring the software and Command & Data Handling (C&DH) hardware can handle the mission's logic and data.

## Design Domains

### 1. C&DH Hardware Sizing
- **Processor**: Estimate MIPS/FLOPS requirements based on GNC loop rates and payload processing.
- **Memory**: Size RAM (volatile), Flash (non-volatile), and EEPROM (critical parameters).
- **Interfaces**: Define SpaceWire, CAN, I2C, or RS-422 bus loading.

### 2. Software Architecture
- **Frameworks**: Evaluate NASA cFS (core Flight System), JPL F Prime, or custom bare-metal/RTOS approaches.
- **Autonomy**: Define Fault Detection, Isolation, and Recovery (FDIR) levels and "Safe Mode" logic.

### 3. Telemetry & Command (T&C)
- **Dictionaries**: Define critical housekeeping telemetry and telecommand formats (e.g., CCSDS).
- **Execution**: Estimate CPU utilization for nominal vs. peak (e.g., imaging + downlink) states.

## Output Format
1. **FSW Architecture Doc (`fsw_architecture.md`)**: Description of software layers, tasking, and FDIR strategy.
2. **Computational Budget (`computing_budget.md`)**: Table of CPU utilization, RAM usage, and bus bandwidth.
